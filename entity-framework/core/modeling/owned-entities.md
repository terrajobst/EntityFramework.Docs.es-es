---
title: Propiedad de tipos de entidad - EF Core
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: 3eb7480625db4ebc3ce0b7a18d042139f888dab8
ms.sourcegitcommit: 0935ff275ae739243297f5b97eb21414398125c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39201898"
---
# <a name="owned-entity-types"></a>Tipos de entidad en propiedad

>[!NOTE]
> Esta característica es nueva en EF Core 2.0.

EF Core permite a tipos de entidad del modelo que sólo pueden aparecer en las propiedades de navegación de otros tipos de entidad. Se denominan _tipos de entidad en propiedad_. La entidad que contiene un tipo de entidad de propiedad es su _propietario_.

## <a name="explicit-configuration"></a>Configuración explícita

Propiedad de entidad tipos nunca se incluyen por EF Core en el modelo por convención. Puede usar el `OwnsOne` método `OnModelCreating` o anotar el tipo con `OwnedAttribute` (Novedades de EF Core 2.1) para configurar el tipo como un tipo de propiedad.

En este ejemplo, StreetAddress es un tipo con ninguna propiedad de identidad. Se usa como propiedad del tipo Order para especificar la dirección de envío de un pedido en concreto. En `OnModelCreating`, usamos el `OwnsOne` método para especificar que la propiedad ShippingAddress es una entidad de propiedad del tipo de orden.

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

Si la propiedad ShippingAddress es privada en el tipo de pedido, puede usar la versión de cadena del `OwnsOne` método:

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

En EF Core 2.0 y 2.1, solo las propiedades de navegación de referencia pueden apuntar a tipos de propiedad. No se admiten colecciones de tipos de propiedad. Propiedad de la referencia de estos tipos siempre tienen una relación uno a uno con el propietario, por lo tanto, no necesitan sus propios valores de clave. En el ejemplo anterior, el tipo StreetAddress no necesita definir una propiedad de clave.  

Para comprender cómo EF Core realiza el seguimiento de estos objetos, es útil pensar que una clave principal se crea como un [propiedad reemplazada](xref:core/modeling/shadow-properties) para el tipo de propiedad. El valor de la clave de una instancia del tipo de propiedad será el mismo que el valor de la clave de la instancia de propietario.      

## <a name="mapping-owned-types-with-table-splitting"></a>Asignación de tipos con la división de tablas de propiedad

Al usar bases de datos relacionales, por convención, tipos de propiedad se asignan a la misma tabla como el propietario. Esto requiere la división de la tabla en dos: algunas columnas que se usará para almacenar los datos del propietario, y algunas columnas que se usará para almacenar los datos de la entidad en propiedad. Se trata de una característica común que se conoce como la división de tablas.

> [!TIP]
> Propiedad de tipos almacenados con la división de tablas se pueden usar de forma muy similar a los tipos complejos de cómo se usan en EF6.

Por convención, EF Core asignará nombre a las columnas de base de datos para las propiedades del tipo de entidad propiedad seguir el patrón de _EntityProperty_OwnedEntityProperty_. Por lo tanto, las propiedades StreetAddress aparecerá en la tabla Orders con los nombres ShippingAddress_Street y ShippingAddress_City.

Puede anexar el `HasColumnName` método para cambiar el nombre de esas columnas. En el caso donde StreetAddress es una propiedad pública, las asignaciones serían

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Uso compartido entre varios tipos que se poseen el mismo tipo de .NET

Un tipo de entidad de propiedad puede ser del mismo tipo de .NET como otro tipo de entidad en propiedad, por lo tanto, que el tipo de .NET puede no ser suficiente para identificar un tipo de propiedad.

En esos casos, se convierte en la propiedad señalando el propietario de la entidad en propiedad el _definir la navegación_ del tipo de entidad en propiedad. Desde la perspectiva del EF Core, la definición de la navegación es parte de la identidad del tipo junto con el tipo de .NET.   

Por ejemplo, en la siguiente clase, ShippingAddress y BillingAddress son del mismo tipo. NET, StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Para comprender cómo EF Core distinguirá sometidas a seguimiento de las instancias de estos objetos, puede ser útil pensar que la navegación definitoria se ha convertido en parte de la clave de la instancia junto con el valor de la clave del propietario y el tipo de .NET del tipo de propiedad.

## <a name="nested-owned-types"></a>Tipos de propiedad anidados

En este ejemplo OrderDetails posee BillingAddress y ShippingAddress, que son tipos StreetAddress. A continuación, OrderDetails es propiedad del tipo de orden.

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

public enum OrderStatus
{
    Pending,
    Shipped
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

Es posible encadenar el `OwnsOne` método en una asignación fluida para configurar este modelo:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Es posible lograr lo mismo mediante `OwnedAttribute` en OrderDetails y StreetAdress.

Además de los tipos de propiedad anidados, un tipo de propiedad puede hacer referencia a una entidad regular. En el ejemplo siguiente, país es una entidad no propiedad normal:

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Esta capacidad establece los tipos de entidad en propiedad aparte de los tipos complejos en EF6.

## <a name="storing-owned-types-in-separate-tables"></a>Almacenar tipos de propiedad en tablas independientes

También a diferencia de los tipos complejos de EF6, tipos de propiedad pueden almacenarse en una tabla independiente del propietario. Con el fin de invalidar la convención que asigna un tipo de propiedad a la misma tabla como el propietario, puede llamar simplemente `ToTable` y proporcione un nombre de tabla diferente. En el siguiente ejemplo se asignará OrderDetails y sus dos direcciones a una tabla independiente de orden:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Consultar los tipos de propiedad

Al consultar al propietario, los tipos de propiedad se incluyen de forma predeterminada. No es necesario utilizar el `Include` método, incluso si los tipos de propiedad se almacenan en una tabla independiente. Según el modelo descrito anteriormente, la siguiente consulta extraerá Order, OrderDetails y los dos StreetAddresses propietario para todos los pedidos pendientes de la base de datos:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Limitaciones

Algunas de estas limitaciones son fundamentales para que se poseen cómo funcionan los tipos entity, pero otros son restricciones que podamos quite en futuras versiones:

### <a name="shortcomings-in-previous-versions"></a>Limitaciones en las versiones anteriores
- En EF Core 2.0, navegaciones a propiedad de tipos de entidad no se puede declarar en tipos de entidad derivados a menos que las entidades en propiedad explícitamente se asignan a una tabla independiente de la jerarquía del propietario. Esta limitación se ha quitado en EF Core 2.1

### <a name="current-shortcomings"></a>Limitaciones actuales
- Jerarquías de herencia que incluyen propiedad no se admiten los tipos de entidad
- Tipos de entidad de propiedad no señala a una propiedad de navegación de colección (referencia solo se admiten actualmente las navegaciones)
- Navegaciones a la propiedad de tipos de entidad no pueden ser nulos a menos que explícitamente se asignan a una tabla independiente del propietario
- Las instancias de tipos de entidad de propiedad no pueden compartirse entre varios propietarios (este es un escenario muy conocido para los objetos de valor que no se puede implementar mediante tipos de entidad de propiedad)

### <a name="by-design-restrictions"></a>Restricciones de diseño
- No puede crear un `DbSet<T>`
- No se puede llamar a `Entity<T>()` con un tipo de propiedad en `ModelBuilder`
