---
title: Propiedad de tipos de entidad - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: 58da3b6b951b3fa4aa04ec75f5759555c1f0cde5
ms.sourcegitcommit: 39080d38e1adea90db741257e60dc0e7ed08aa82
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980033"
---
# <a name="owned-entity-types"></a>Tipos de entidad en propiedad

>[!NOTE]
> Esta característica es nueva en EF Core 2.0.

EF Core permite a tipos de entidad del modelo que sólo pueden aparecer en las propiedades de navegación de otros tipos de entidad. Se denominan _tipos de entidad en propiedad_. La entidad que contiene un tipo de entidad de propiedad es su _propietario_.

## <a name="explicit-configuration"></a>Configuración explícita

Propiedad de entidad tipos nunca se incluyen por EF Core en el modelo por convención. Puede usar el `OwnsOne` método `OnModelCreating` o anotar el tipo con `OwnedAttribute` (Novedades de EF Core 2.1) para configurar el tipo como un tipo de propiedad.

En este ejemplo, `StreetAddress` es un tipo con ninguna propiedad de identidad. Se usa como propiedad del tipo Order para especificar la dirección de envío de un pedido en concreto.

Podemos usar el `OwnedAttribute` para tratarlo como una entidad de propiedad cuando se hace referencia desde otro tipo de entidad:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

También es posible usar el `OwnsOne` método `OnModelCreating` para especificar que el `ShippingAddress` propiedad es una entidad de propiedad de la `Order` tipo de entidad y configurar aspectos adicionales si es necesario.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Si el `ShippingAddress` propiedad es privada en el `Order` tipo, puede usar la versión de cadena de la `OwnsOne` método:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Consulte la [proyecto de ejemplo completa](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) para obtener más contexto. 

## <a name="implicit-keys"></a>Claves implícitas

Propiedad tipos configurados con `OwnsOne` o detectados a través de una navegación de referencia siempre tienen una relación uno a uno con el propietario, por lo tanto, no necesitan sus propios valores de clave ya que son únicos los valores de clave externos. En el ejemplo anterior, el `StreetAddress` tipo no necesita definir una propiedad de clave.  

Para comprender cómo EF Core realiza el seguimiento de estos objetos, es útil pensar que una clave principal se crea como un [propiedad reemplazada](xref:core/modeling/shadow-properties) para el tipo de propiedad. El valor de la clave de una instancia del tipo de propiedad será el mismo que el valor de la clave de la instancia de propietario.

## <a name="collections-of-owned-types"></a>Colecciones de tipos de propiedad

>[!NOTE]
> Esta característica es nueva en EF Core 2.2.

Para configurar una colección de tipos de propiedad `OwnsMany` debe usarse en `OnModelCreating`. Sin embargo la clave principal no se configura por convención, por lo que necesita especificarse explícitamente. Es habitual usar una clave compleja para estos tipos de entidades que incorpora la clave externa para el propietario y una propiedad adicional única que también puede estar en estado de la sombra:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a>Asignación de tipos con la división de tablas de propiedad

Al usar relacional bases de datos, por referencia convención propiedad tipos se asignan a la misma tabla como el propietario. Esto requiere la división de la tabla en dos: algunas columnas que se usará para almacenar los datos del propietario, y algunas columnas que se usará para almacenar los datos de la entidad en propiedad. Se trata de una característica común que se conoce como la división de tablas.

> [!TIP]
> Propiedad de tipos almacenados con la división de tablas se pueden usar de forma similar a los tipos complejos de cómo se usan en EF6.

Por convención, EF Core denominará las columnas de la base de datos para las propiedades del tipo de entidad en propiedad siguiendo el patrón de _Navigation_OwnedEntityProperty_. Por lo tanto, la `StreetAddress` propiedades aparecerán en la tabla "Orders" con los nombres 'ShippingAddress_Street' y 'ShippingAddress_City'.

Puede anexar el `HasColumnName` método para cambiar el nombre de las columnas:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Uso compartido entre varios tipos que se poseen el mismo tipo de .NET

Un tipo de entidad de propiedad puede ser del mismo tipo de .NET como otro tipo de entidad en propiedad, por lo tanto, que el tipo de .NET puede no ser suficiente para identificar un tipo de propiedad.

En esos casos, se convierte en la propiedad señalando el propietario de la entidad en propiedad el _definir la navegación_ del tipo de entidad en propiedad. Desde la perspectiva del EF Core, la definición de la navegación es parte de la identidad del tipo junto con el tipo de .NET.   

Por ejemplo, en la siguiente clase `ShippingAddress` y `BillingAddress` son ambas del mismo tipo. NET, `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Para comprender cómo EF Core distinguirá sometidas a seguimiento de las instancias de estos objetos, puede ser útil pensar que la navegación definitoria se ha convertido en parte de la clave de la instancia junto con el valor de la clave del propietario y el tipo de .NET del tipo de propiedad.

## <a name="nested-owned-types"></a>Tipos de propiedad anidados

En este ejemplo `OrderDetails` posee `BillingAddress` y `ShippingAddress`, que son ambos `StreetAddress` tipos. Luego, `OrderDetails` es propiedad del tipo `DetailedOrder`.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Además de los tipos de propiedad anidados, un tipo de propiedad puede hacer referencia a una entidad normal, puede ser el propietario o una entidad diferente siempre que sea la entidad de propiedad en el lado dependiente. Esta capacidad establece los tipos de entidad en propiedad aparte de los tipos complejos en EF6.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Es posible encadenar el `OwnsOne` método en una llamada fluida para configurar este modelo:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

También es posible lograr lo mismo mediante `OwnedAttribute` en ambos `OrderDetails` y `StreetAdress`.

## <a name="storing-owned-types-in-separate-tables"></a>Almacenar tipos de propiedad en tablas independientes

También a diferencia de los tipos complejos de EF6, tipos de propiedad pueden almacenarse en una tabla independiente del propietario. Con el fin de invalidar la convención que asigna un tipo de propiedad a la misma tabla como el propietario, puede llamar simplemente `ToTable` y proporcione un nombre de tabla diferente. En el ejemplo siguiente, se asignará `OrderDetails` y sus dos direcciones a una tabla independiente de `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Consultar los tipos de propiedad

Al consultar al propietario, los tipos de propiedad se incluyen de forma predeterminada. No es necesario utilizar el `Include` método, incluso si los tipos de propiedad se almacenan en una tabla independiente. Según el modelo descrito anteriormente, obtendrá la siguiente consulta `Order`, `OrderDetails` y los dos pertenece `StreetAddresses` desde la base de datos:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Limitaciones

Algunas de estas limitaciones son fundamentales para que se poseen cómo funcionan los tipos entity, pero otros son restricciones que podamos quite en futuras versiones:

### <a name="by-design-restrictions"></a>Restricciones de diseño
- No puede crear un `DbSet<T>` para un tipo de propiedad
- No se puede llamar a `Entity<T>()` con un tipo de propiedad en `ModelBuilder`

### <a name="current-shortcomings"></a>Limitaciones actuales
- Jerarquías de herencia que incluyen propiedad no se admiten los tipos de entidad
- Propiedad de las navegaciones de referencia para tipos de entidad no pueden ser nulos a menos que explícitamente se asignan a una tabla independiente del propietario del
- Las instancias de tipos de entidad de propiedad no pueden compartirse entre varios propietarios (este es un escenario muy conocido para los objetos de valor que no se puede implementar mediante tipos de entidad de propiedad)

### <a name="shortcomings-in-previous-versions"></a>Limitaciones en las versiones anteriores
- En EF Core 2.0, navegaciones a propiedad de tipos de entidad no se puede declarar en tipos de entidad derivados a menos que las entidades en propiedad explícitamente se asignan a una tabla independiente de la jerarquía del propietario. Esta limitación se ha quitado en EF Core 2.1
- Se admitían navegaciones en EF Core 2.0 y 2.1 de referencia sola a tipos de propiedad. Esta limitación se ha quitado en EF Core 2.2