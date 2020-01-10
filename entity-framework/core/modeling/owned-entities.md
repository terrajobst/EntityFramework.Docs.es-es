---
title: Propiedad de tipos de entidad - EF Core
description: Cómo configurar tipos de entidad o agregados de propiedad al usar Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/owned-entities
ms.openlocfilehash: 30b91b6e66b6c0f516d1ba12485304b52770cbef
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781240"
---
# <a name="owned-entity-types"></a>Tipos de entidad en propiedad

EF Core permite a tipos de entidad del modelo que sólo pueden aparecer en las propiedades de navegación de otros tipos de entidad. Se denominan _tipos de entidad de propiedad_. La entidad que contiene un tipo de entidad propiedad es su _propietario_.

Las entidades propiedad son esencialmente parte del propietario y no pueden existir sin ella, son conceptualmente similares a los [agregados](https://martinfowler.com/bliki/DDD_Aggregate.html). Esto significa que el tipo de propiedad es por definición en el lado dependiente de la relación con el propietario.

## <a name="explicit-configuration"></a>Configuración explícita

Propiedad de entidad tipos nunca se incluyen por EF Core en el modelo por convención. Puede usar el método `OwnsOne` en `OnModelCreating` o anotar el tipo con `OwnedAttribute` (novedad en EF Core 2,1) para configurar el tipo como un tipo de propiedad.

En este ejemplo, `StreetAddress` es un tipo sin propiedad de identidad. Se usa como propiedad del tipo Order para especificar la dirección de envío de un pedido en concreto.

Podemos usar el `OwnedAttribute` para tratarlo como una entidad propiedad cuando se hace referencia a él desde otro tipo de entidad:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

También es posible usar el método `OwnsOne` de `OnModelCreating` para especificar que la propiedad `ShippingAddress` es una entidad propiedad del tipo de entidad `Order` y para configurar aspectos adicionales si es necesario.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Si la propiedad `ShippingAddress` es privada en el tipo de `Order`, puede usar la versión de cadena del método `OwnsOne`:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Vea el [proyecto de ejemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) para obtener más contexto.

## <a name="implicit-keys"></a>Claves IMPLÍCITAS

Los tipos de propiedad configurados con `OwnsOne` o detectados a través de una navegación de referencia siempre tienen una relación uno a uno con el propietario, por lo que no necesitan sus propios valores de clave, ya que los valores de clave externa son únicos. En el ejemplo anterior, el tipo de `StreetAddress` no necesita definir una propiedad de clave.  

Para entender cómo EF Core realiza el seguimiento de estos objetos, es útil saber que se crea una clave principal como una [propiedad de sombra](xref:core/modeling/shadow-properties) para el tipo de propiedad. El valor de la clave de una instancia del tipo de propiedad será el mismo que el valor de la clave de la instancia de propietario.

## <a name="collections-of-owned-types"></a>Colecciones de tipos de propiedad

> [!NOTE]
> Esta característica es nueva en EF Core 2.2.

Para configurar una colección de tipos de propiedad, use `OwnsMany` en `OnModelCreating`.

Los tipos de propiedad necesitan una clave principal. Si no hay buenas propiedades candidatas en el tipo .NET, EF Core puede intentar crear una. Sin embargo, cuando los tipos de propiedad se definen a través de una colección, no basta con crear simplemente una propiedad Shadow que actúe como clave externa en el propietario y la clave principal de la instancia de propiedad, como hacemos para `OwnsOne`: puede haber varias instancias de tipo de propiedad para cada propietario y, por lo tanto, la clave del propietario no es suficiente para proporcionar una identidad única para cada instancia de

Las dos soluciones más directas son:

- Definir una clave principal suplente en una nueva propiedad independiente de la clave externa que señala al propietario. Los valores contenidos deben ser únicos en todos los propietarios (por ejemplo, si el {1} primario tiene {1}secundarios, el {2} primario no puede tener {1}secundarios), por lo que el valor no tiene ningún significado inherente. Dado que la clave externa no forma parte de la clave principal, sus valores se pueden cambiar, por lo que puede trasladar un elemento secundario de un elemento primario a otro; sin embargo, esto suele pasar por la semántica de agregado.
- Usar la clave externa y una propiedad adicional como clave compuesta. El valor de propiedad adicional ahora solo debe ser único para un elemento primario determinado (por lo que si el {1} primario tiene {1,1} secundarios, el {2} principal todavía puede tener {2,1}secundarios). Al convertir la parte de clave externa de la clave principal en la relación entre el propietario y la entidad propiedad, se convierte en inmutable y se refleja mejor la semántica de agregado. Esto es lo que EF Core hace de forma predeterminada.

En este ejemplo, usaremos la clase `Distributor`:

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

De forma predeterminada, la clave principal que se usa para el tipo de propiedad al que se hace referencia a través de la propiedad de navegación de `ShippingCenters` se `("DistributorId", "Id")` donde `"DistributorId"` es el FK y `"Id"` es un valor de `int` único.

Para configurar una llamada PK diferente `HasKey`:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> Antes de que el método EF Core 3,0 `WithOwner()` no existiera, se debe quitar esta llamada. Además, la clave principal no se detectó automáticamente, por lo que siempre se especificó.

## <a name="mapping-owned-types-with-table-splitting"></a>Asignar tipos de propiedad con división de tabla

Cuando se usan bases de datos relacionales, de forma predeterminada, los tipos de propiedad de propiedad se asignan a la misma tabla que el propietario. Esto requiere dividir la tabla en dos: se usarán algunas columnas para almacenar los datos del propietario, y algunas columnas se utilizarán para almacenar los datos de la entidad propiedad. Se trata de una característica común conocida como [División de tablas](table-splitting.md).

De forma predeterminada, EF Core asignará el nombre a las columnas de la base de datos para las propiedades del tipo de entidad propiedad que sigue al patrón _Navigation_OwnedEntityProperty_. Por lo tanto, las propiedades de `StreetAddress` aparecerán en la tabla ' Orders ' con los nombres ' ShippingAddress_Street ' y ' ShippingAddress_City '.

Puede usar el método `HasColumnName` para cambiar el nombre de esas columnas:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

> [!NOTE]
> La mayoría de los métodos de configuración de tipo de entidad normales, como [Ignore](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) , se pueden llamar de la misma manera.

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Compartir el mismo tipo .NET entre varios tipos de propiedad

Un tipo de entidad de propiedad puede ser del mismo tipo de .NET que otro tipo de entidad de propiedad, por lo que es posible que el tipo .NET no sea suficiente para identificar un tipo de propiedad.

En esos casos, la propiedad que apunta del propietario a la entidad propiedad se convierte en la _navegación de definición_ del tipo de entidad de propiedad. Desde la perspectiva del EF Core, la definición de la navegación es parte de la identidad del tipo junto con el tipo de .NET.

Por ejemplo, en la siguiente clase `ShippingAddress` y `BillingAddress` son ambos del mismo tipo .NET, `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Para entender cómo EF Core distinguirá las instancias de de estos objetos de las que se ha realizado un seguimiento, puede ser útil pensar que la navegación que define se ha convertido en parte de la clave de la instancia junto con el valor de la clave del propietario y el tipo .NET del tipo de propiedad.

## <a name="nested-owned-types"></a>Tipos de propiedad anidados

En este ejemplo `OrderDetails` posee `BillingAddress` y `ShippingAddress`, que son tipos de `StreetAddress`. Luego, `OrderDetails` es propiedad del tipo `DetailedOrder`.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Cada navegación a un tipo de propiedad define un tipo de entidad independiente con una configuración completamente independiente.

Además de los tipos de propiedad anidados, un tipo de propiedad puede hacer referencia a una entidad normal, puede ser el propietario o una entidad distinta, siempre y cuando la entidad propiedad esté en el lado dependiente. Esta funcionalidad establece tipos de entidad de propiedad además de tipos complejos en EF6.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Es posible encadenar el método `OwnsOne` en una llamada fluida para configurar este modelo:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Observe la llamada `WithOwner` utilizada para configurar la propiedad de navegación que señala hacia atrás en el propietario. Para configurar una navegación al tipo de entidad Owner que no forma parte de la relación de propiedad `WithOwner()` se debe llamar a sin ningún argumento.

Es posible lograr el resultado mediante `OwnedAttribute` en `OrderDetails` y `StreetAddress`.

## <a name="storing-owned-types-in-separate-tables"></a>Almacenar tipos de propiedad en tablas independientes

Además, a diferencia de los tipos complejos de EF6, los tipos de propiedad se pueden almacenar en una tabla independiente del propietario. Para invalidar la Convención que asigna un tipo de propiedad a la misma tabla que el propietario, puede llamar simplemente a `ToTable` y proporcionar un nombre de tabla diferente. En el ejemplo siguiente se asignará `OrderDetails` y sus dos direcciones a una tabla independiente de `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

También es posible usar el `TableAttribute` para lograr esto, pero tenga en cuenta que esto produciría un error si hay varias navegaciones al tipo de propiedad, ya que en ese caso se asignarían varios tipos de entidad a la misma tabla.

## <a name="querying-owned-types"></a>Consultar tipos de propiedad

Al consultar al propietario, los tipos de propiedad se incluyen de forma predeterminada. No es necesario utilizar el método `Include`, incluso si los tipos de propiedad se almacenan en una tabla independiente. En función del modelo descrito anteriormente, la siguiente consulta obtendrá `Order`, `OrderDetails` y los dos `StreetAddresses` de propiedad de la base de datos:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Limitaciones

Algunas de estas limitaciones son fundamentales para el funcionamiento de los tipos de entidad de propiedad, pero otras son restricciones que podríamos ser capaces de quitar en versiones futuras:

### <a name="by-design-restrictions"></a>Restricciones por diseño

- No se puede crear un `DbSet<T>` para un tipo de propiedad
- No se puede llamar a `Entity<T>()` con un tipo de propiedad en `ModelBuilder`

### <a name="current-shortcomings"></a>Deficiencias actuales

- Los tipos de entidad de propiedad no pueden tener jerarquías de herencia
- Las navegaciones de referencia a tipos de entidad de propiedad no pueden ser null a menos que se asignen explícitamente a una tabla independiente del propietario.
- Los distintos propietarios no pueden compartir instancias de tipos de entidad con propiedad (este es un escenario conocido para objetos de valor que no se pueden implementar mediante tipos de entidad de propiedad)

### <a name="shortcomings-in-previous-versions"></a>Deficiencias en versiones anteriores

- En EF Core 2,0, las navegaciones a tipos de entidad de propiedad no se pueden declarar en tipos de entidad derivadas a menos que las entidades de propiedad se asignen explícitamente a una tabla independiente de la jerarquía de propietarios. Esta limitación se ha eliminado en EF Core 2,1
- En EF Core 2,0 y 2,1 solo se admiten las navegaciones de referencia a los tipos de propiedad. Esta limitación se ha eliminado en EF Core 2,2
