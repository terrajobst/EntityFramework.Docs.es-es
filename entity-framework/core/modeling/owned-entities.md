---
title: Propiedad de tipos de entidad - Core EF
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: a6823377eb626ca92263c31351e1aef61db5a787
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="owned-entity-types"></a>Tipos de entidad en propiedad

>[!NOTE]
> Esta función es nueva en EF Core 2.0.

Núcleo EF permite a tipos de entidad del modelo que sólo pueden aparecer en las propiedades de navegación de otros tipos de entidad. Estos se denominan _propiedad de tipos de entidad_. La entidad que contiene un tipo de entidad en propiedad es su _propietario_.

## <a name="explicit-configuration"></a>Configuración explícita

Propiedad de entidad tipos nunca se incluyen por núcleo de EF en el modelo por convención. Puede utilizar el `OwnsOne` método `OnModelCreating` o anotar el tipo con `OwnedAttrbibute` (nuevo en EF Core 2.1) para configurar el tipo como un tipo de propiedad.

En este ejemplo, StreetAddress es un tipo con ninguna propiedad de identidad. Se usa como propiedad del tipo Order para especificar la dirección de envío de un pedido en concreto. En `OnModelCreating`, usamos el `OwnsOne` para especificar que la propiedad ShippingAddress es una entidad de propiedad del tipo de orden.

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

// OnModelCreating
modelBuilder.Entity<Order>().OwnsOne(p => p.ShippingAddress);
```

Si la propiedad ShippingAddress es privada en el tipo de pedido, puede utilizar la versión de cadena de la `OwnsOne` método:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

En este ejemplo, usamos el `OwnedAttribute` para lograr el mismo objetivo:

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="implicit-keys"></a>Claves implícitas

En EF Core 2.0 y 2.1, propiedades de navegación de referencia sólo pueden señalar a los tipos de propiedad. No se admiten colecciones de tipos de propiedad. Estas referencia propiedad tipos siempre tienen una relación uno a uno con el propietario, por lo tanto, no necesitan sus propios valores de clave. En el ejemplo anterior, el tipo de StreetAddress no tiene que definir una propiedad de clave.  

Para comprender cómo EF núcleo realiza el seguimiento de estos objetos, es útil considerar que una clave principal se crea como un [sombrear a la propiedad](xref:core/modeling/shadow-properties) para el tipo de propiedad. El valor de la clave de una instancia del tipo de propiedad será el mismo que el valor de la clave de la instancia de propietario.      

## <a name="mapping-owned-types-with-table-splitting"></a>Tipos con la división de la tabla de asignación de propiedad

Al utilizar bases de datos relacionales, por convención, los tipos de propiedad se asignan a la misma tabla que el propietario. Esto requiere dividir la tabla en dos: algunas columnas se utilizará para almacenar los datos del propietario, y algunas columnas se utilizará para almacenar los datos de la entidad en propiedad. Se trata de una característica común conocida como división de la tabla.

> [!TIP]
> Propiedad almacenados con la división de la tabla de tipos pueden ser utilizados de forma muy similar a tipos complejos de cómo se utilizan en EF6.

Por convención, Core EF asignará nombre a las columnas de base de datos para las propiedades del tipo de entidad propiedad seguir el patrón de _EntityProperty_OwnedEntityProperty_. Por consiguiente, las propiedades StreetAddress aparecerá en la tabla de pedidos con los nombres ShippingAddress_Street y ShippingAddress_City.

Puede anexar el `HasColumnName` método para cambiar el nombre de las columnas. En el caso donde StreetAddress es una propiedad pública, sería las asignaciones

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Compartir el mismo tipo de .NET entre varios tipos de propiedad

Un tipo de entidad en propiedad puede ser del mismo tipo .NET como otro tipo de entidad en propiedad, por lo tanto, que el tipo de .NET puede no ser suficiente para identificar un tipo de propiedad.

En esos casos, la propiedad señalando el propietario de la entidad de propiedad se convierte en el _definición de exploración_ del tipo de entidad en propiedad. Desde la perspectiva del núcleo EF, la definición de la navegación es parte de la identidad del tipo junto con el tipo de .NET.   

Por ejemplo, en la clase siguiente, ShippingAddress y BillingAddress son ambas del mismo tipo. NET, StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Para comprender cómo EF Core distinguirá marcas instancias de estos objetos, puede ser útil pensar que la definición de la navegación se ha convertido en parte de la clave de la instancia junto con el valor de la clave del propietario y el tipo de .NET del tipo propietario.

## <a name="nested-owned-types"></a>Propietario de tipos anidados

En este ejemplo OrderDetails posee BillingAddress y ShippingAddress, que son tipos StreetAddress. A continuación, el tipo de pedido pertenece OrderDetails.

``` csharp
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
    public OrderStatus Status { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

Es posible que la cadena de la `OwnsOne` método en una asignación fluent para configurar este modelo:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Es posible lograr lo mismo con `OwnedAttribute` en OrderDetails y StreetAdress.

Además de los tipos de propiedad anidados, un tipo de propiedad puede hacer referencia a una entidad normal. En el ejemplo siguiente, el país es una entidad normal (es decir, no son propiedad de):

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Esta función establece los tipos de entidad en propiedad aparte de tipos complejos EF6.

## <a name="storing-owned-types-in-separate-tables"></a>Almacenar tipos en tablas independientes de propiedad

También a diferencia de los tipos complejos de EF6, tipos de propiedad pueden almacenarse en una tabla independiente del propietario. Para reemplazar la convención que asigna un tipo perteneciente a la misma tabla que el propietario, puede llamar simplemente `ToTable` y proporcione un nombre de tabla diferente. En el siguiente ejemplo se asignará OrderDetails y sus dos direcciones en una tabla independiente del orden:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Consultar tipos de propiedad

Al consultar al propietario, los tipos de propiedad se incluyen de forma predeterminada. No es necesario utilizar el `Include` método, incluso si los tipos de propiedad se almacenan en una tabla independiente. Se basa en el modelo descrito anteriormente, la siguiente consulta extraerá Order, OrderDetails y los dos StreeAddresses propietario para todos los pedidos pendientes de la base de datos:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Limitaciones

Aquí hay algunas limitaciones de los tipos de entidad en propiedad. Algunas de estas limitaciones son fundamentales para la propiedad de tipos de trabajo, pero otros son restricciones de punto en el tiempo que nos gustaría quitar en futuras versiones:

### <a name="current-shortcomings"></a>Actuales deficiencias
- No se admite la herencia de tipos de propiedad
- Tipos de propiedad no pueden ser señalados por una propiedad de navegación de la colección
- Puesto que utiliza la división de la tabla por predeterminado propiedad de tipos también tienen las siguientes restricciones a menos que explícitamente asignado a una tabla diferente:
   - No puede ser propiedad de un tipo derivado
   - No se puede establecer la propiedad definidora de exploración a null (es decir, propiedad de tipos en la misma tabla siempre son necesarios)

### <a name="by-design-restrictions"></a>Restricciones de diseño
- No puede crear un `DbSet<T>`
- No se puede llamar `Entity<T>()` con un tipo de propiedad en `ModelBuilder`
