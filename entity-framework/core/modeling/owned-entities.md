---
title: Propiedad de tipos de entidad - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: f69bae2de28156876e0aa57376b5dfac053adb9c
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149141"
---
# <a name="owned-entity-types"></a><span data-ttu-id="68625-102">Tipos de entidad propiedad</span><span class="sxs-lookup"><span data-stu-id="68625-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="68625-103">Esta característica es nueva en EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="68625-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="68625-104">EF Core permite a tipos de entidad del modelo que sólo pueden aparecer en las propiedades de navegación de otros tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="68625-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="68625-105">Se denominan _tipos de entidad de propiedad_.</span><span class="sxs-lookup"><span data-stu-id="68625-105">These are called _owned entity types_.</span></span> <span data-ttu-id="68625-106">La entidad que contiene un tipo de entidad propiedad es su _propietario_.</span><span class="sxs-lookup"><span data-stu-id="68625-106">The entity containing an owned entity type is its _owner_.</span></span>

<span data-ttu-id="68625-107">Las entidades propiedad son esencialmente parte del propietario y no pueden existir sin ella, son conceptualmente similares a los [agregados](https://martinfowler.com/bliki/DDD_Aggregate.html).</span><span class="sxs-lookup"><span data-stu-id="68625-107">Owned entities are essentially a part of the owner and cannot exist without it, they are conceptually similar to [aggregates](https://martinfowler.com/bliki/DDD_Aggregate.html).</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="68625-108">Configuración explícita</span><span class="sxs-lookup"><span data-stu-id="68625-108">Explicit configuration</span></span>

<span data-ttu-id="68625-109">Propiedad de entidad tipos nunca se incluyen por EF Core en el modelo por convención.</span><span class="sxs-lookup"><span data-stu-id="68625-109">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="68625-110">Puede usar el `OwnsOne` método en `OnModelCreating` o anotar el tipo con `OwnedAttribute` (novedad en EF Core 2,1) para configurar el tipo como un tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="68625-110">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="68625-111">En este ejemplo, `StreetAddress` es un tipo sin propiedad de identidad.</span><span class="sxs-lookup"><span data-stu-id="68625-111">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="68625-112">Se usa como propiedad del tipo Order para especificar la dirección de envío de un pedido en concreto.</span><span class="sxs-lookup"><span data-stu-id="68625-112">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="68625-113">Podemos usar el `OwnedAttribute` para tratarlo como una entidad propiedad cuando se hace referencia a él desde otro tipo de entidad:</span><span class="sxs-lookup"><span data-stu-id="68625-113">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="68625-114">También es `OwnsOne` posible utilizar el método en `OnModelCreating` para especificar que la `ShippingAddress` propiedad es una entidad propiedad del tipo de `Order` entidad y para configurar aspectos adicionales si es necesario.</span><span class="sxs-lookup"><span data-stu-id="68625-114">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="68625-115">Si la `ShippingAddress` propiedad es privada en el `Order` tipo, puede usar `OwnsOne` la versión de cadena del método:</span><span class="sxs-lookup"><span data-stu-id="68625-115">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="68625-116">Vea el [proyecto de ejemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) para obtener más contexto.</span><span class="sxs-lookup"><span data-stu-id="68625-116">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="68625-117">Claves IMPLÍCITAS</span><span class="sxs-lookup"><span data-stu-id="68625-117">Implicit keys</span></span>

<span data-ttu-id="68625-118">Los tipos de propiedad `OwnsOne` configurados con o detectados a través de una navegación de referencia siempre tienen una relación uno a uno con el propietario, por lo que no necesitan sus propios valores de clave, ya que los valores de clave externa son únicos.</span><span class="sxs-lookup"><span data-stu-id="68625-118">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="68625-119">En el ejemplo anterior, el `StreetAddress` tipo no necesita definir una propiedad de clave.</span><span class="sxs-lookup"><span data-stu-id="68625-119">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="68625-120">Para entender cómo EF Core realiza el seguimiento de estos objetos, es útil saber que se crea una clave principal como una [propiedad de sombra](xref:core/modeling/shadow-properties) para el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="68625-120">In order to understand how EF Core tracks these objects, it is useful to know that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="68625-121">El valor de la clave de una instancia del tipo de propiedad será el mismo que el valor de la clave de la instancia de propietario.</span><span class="sxs-lookup"><span data-stu-id="68625-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="68625-122">Colecciones de tipos de propiedad</span><span class="sxs-lookup"><span data-stu-id="68625-122">Collections of owned types</span></span>

> [!NOTE]
> <span data-ttu-id="68625-123">Esta característica es nueva en EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="68625-123">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="68625-124">Para configurar una colección de tipos de propiedad `OwnsMany` , `OnModelCreating`utilice en.</span><span class="sxs-lookup"><span data-stu-id="68625-124">To configure a collection of owned types use `OwnsMany` in `OnModelCreating`.</span></span>

<span data-ttu-id="68625-125">Los tipos de propiedad necesitan una clave principal.</span><span class="sxs-lookup"><span data-stu-id="68625-125">Owned types need a primary key.</span></span> <span data-ttu-id="68625-126">Si no hay buenas propiedades candidatas en el tipo .NET, EF Core puede intentar crear una.</span><span class="sxs-lookup"><span data-stu-id="68625-126">If there are no good candidates properties on the .NET type, EF Core can try to create one.</span></span> <span data-ttu-id="68625-127">Sin embargo, cuando los tipos de propiedad se definen a través de una colección, no basta con crear simplemente una propiedad Shadow que actúe como clave externa en el propietario y la clave principal de la instancia de propiedad, como `OwnsOne`hacemos para: puede haber varias instancias de tipo de propiedad para cada propietario y, por lo tanto, la clave del propietario no es suficiente para proporcionar una identidad única para cada instancia de propiedad.</span><span class="sxs-lookup"><span data-stu-id="68625-127">However, when owned types are defined through a collection, it isn't enough to just create a shadow property to act as both the foreign key into the owner and the primary key of the owned instance, as we do for `OwnsOne`: there can be multiple owned type instances for each owner, and hence the key of the owner isn't enough to provide a unique identity for each owned instance.</span></span>

<span data-ttu-id="68625-128">Las dos soluciones más directas son:</span><span class="sxs-lookup"><span data-stu-id="68625-128">The two most straightforward solutions to this are:</span></span>
- <span data-ttu-id="68625-129">Definir una clave principal suplente en una nueva propiedad independiente de la clave externa que señala al propietario.</span><span class="sxs-lookup"><span data-stu-id="68625-129">Defining a surrogate primary key on a new property independent of the foreign key that points to the owner.</span></span> <span data-ttu-id="68625-130">Los valores contenidos deben ser únicos en todos los propietarios (por ejemplo, si el {1} elemento primario {1}tiene un elemento {2} secundario, el {1}elemento primario no puede tener un elemento secundario), por lo que el valor no tiene ningún significado inherente.</span><span class="sxs-lookup"><span data-stu-id="68625-130">The contained values would need to be unique across all owners (e.g. if Parent {1} has Child {1}, then Parent {2} cannot have Child {1}), so the value doesn't have any inherent meaning.</span></span> <span data-ttu-id="68625-131">Dado que la clave externa no forma parte de la clave principal, sus valores se pueden cambiar, por lo que puede trasladar un elemento secundario de un elemento primario a otro; sin embargo, esto suele pasar por la semántica de agregado.</span><span class="sxs-lookup"><span data-stu-id="68625-131">Since the foreign key is not part of the primary key its values can be changed, so you could move a child from one parent to another one, however this usually goes against aggregate semantics.</span></span>
- <span data-ttu-id="68625-132">Usar la clave externa y una propiedad adicional como clave compuesta.</span><span class="sxs-lookup"><span data-stu-id="68625-132">Using the foreign key and an additional property as a composite key.</span></span> <span data-ttu-id="68625-133">El valor de propiedad adicional ahora solo debe ser único para un elemento primario determinado (por lo {1} que si {1,1} el elemento primario tiene un elemento {2,1}secundario, el elemento primario {2} todavía puede tener un elemento secundario).</span><span class="sxs-lookup"><span data-stu-id="68625-133">The additional property value now only needs to be unique for a given parent (so if Parent {1} has Child {1,1} then Parent {2} can still have Child {2,1}).</span></span> <span data-ttu-id="68625-134">Al convertir la parte de clave externa de la clave principal en la relación entre el propietario y la entidad propiedad, se convierte en inmutable y se refleja mejor la semántica de agregado.</span><span class="sxs-lookup"><span data-stu-id="68625-134">By making the foreign key part of the primary key the relationship between the owner and the owned entity becomes immutable and reflects aggregate semantics better.</span></span> <span data-ttu-id="68625-135">Esto es lo que EF Core hace de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="68625-135">This is what EF Core does by default.</span></span>

<span data-ttu-id="68625-136">En este ejemplo, usaremos la `Distributor` clase:</span><span class="sxs-lookup"><span data-stu-id="68625-136">In this example we'll use the `Distributor` class:</span></span>

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

<span data-ttu-id="68625-137">De forma predeterminada, la clave principal utilizada para el tipo de propiedad al `ShippingCenters` que se hace referencia `("DistributorId", "Id")` a `"DistributorId"` través de la propiedad de navegación será `int` donde sea el FK y `"Id"` sea un valor único.</span><span class="sxs-lookup"><span data-stu-id="68625-137">By default the primary key used for the owned type referenced through the `ShippingCenters` navigation property will be `("DistributorId", "Id")` where `"DistributorId"` is the FK and `"Id"` is a unique `int` value.</span></span>

<span data-ttu-id="68625-138">Para configurar una llamada `HasKey`PK diferente:</span><span class="sxs-lookup"><span data-stu-id="68625-138">To configure a different PK call `HasKey`:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> <span data-ttu-id="68625-139">Antes de que `WithOwner()` EF Core método 3,0 no existiera, se debe quitar esta llamada.</span><span class="sxs-lookup"><span data-stu-id="68625-139">Before EF Core 3.0 `WithOwner()` method didn't exist so this call should be removed.</span></span>

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="68625-140">Asignar tipos de propiedad con división de tabla</span><span class="sxs-lookup"><span data-stu-id="68625-140">Mapping owned types with table splitting</span></span>

<span data-ttu-id="68625-141">Cuando se usan bases de datos relacionales, de forma predeterminada, los tipos de propiedad de propiedad se asignan a la misma tabla que el propietario.</span><span class="sxs-lookup"><span data-stu-id="68625-141">When using relational databases, by default reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="68625-142">Esto requiere dividir la tabla en dos: se usarán algunas columnas para almacenar los datos del propietario, y algunas columnas se utilizarán para almacenar los datos de la entidad propiedad.</span><span class="sxs-lookup"><span data-stu-id="68625-142">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="68625-143">Se trata de una característica común conocida como [División de tablas](table-splitting.md).</span><span class="sxs-lookup"><span data-stu-id="68625-143">This is a common feature known as [table splitting](table-splitting.md).</span></span>

<span data-ttu-id="68625-144">De forma predeterminada, EF Core asignará el nombre a las columnas de la base de datos para las propiedades del tipo de entidad propiedad que sigue el patrón _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="68625-144">By default, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="68625-145">Por lo `StreetAddress` tanto, las propiedades aparecerán en la tabla "Orders" con los nombres "ShippingAddress_Street" y "ShippingAddress_City".</span><span class="sxs-lookup"><span data-stu-id="68625-145">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="68625-146">Puede usar el `HasColumnName` método para cambiar el nombre de esas columnas:</span><span class="sxs-lookup"><span data-stu-id="68625-146">You can use the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="68625-147">Compartir el mismo tipo .NET entre varios tipos de propiedad</span><span class="sxs-lookup"><span data-stu-id="68625-147">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="68625-148">Un tipo de entidad de propiedad puede ser del mismo tipo de .NET que otro tipo de entidad de propiedad, por lo que es posible que el tipo .NET no sea suficiente para identificar un tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="68625-148">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="68625-149">En esos casos, la propiedad que apunta del propietario a la entidad propiedad se convierte en la _navegación de definición_ del tipo de entidad de propiedad.</span><span class="sxs-lookup"><span data-stu-id="68625-149">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="68625-150">Desde la perspectiva del EF Core, la definición de la navegación es parte de la identidad del tipo junto con el tipo de .NET.</span><span class="sxs-lookup"><span data-stu-id="68625-150">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="68625-151">Por ejemplo, en la siguiente clase `ShippingAddress` y `BillingAddress` son ambos del mismo tipo .net, `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="68625-151">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="68625-152">Para entender cómo EF Core distinguirá las instancias de de estos objetos de las que se ha realizado un seguimiento, puede ser útil pensar que la navegación que define se ha convertido en parte de la clave de la instancia junto con el valor de la clave del propietario y el tipo .NET del tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="68625-152">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="68625-153">Tipos de propiedad anidados</span><span class="sxs-lookup"><span data-stu-id="68625-153">Nested owned types</span></span>

<span data-ttu-id="68625-154">En este ejemplo `OrderDetails` , `BillingAddress` posee `ShippingAddress`y, que son `StreetAddress` ambos tipos.</span><span class="sxs-lookup"><span data-stu-id="68625-154">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="68625-155">Luego, `OrderDetails` es propiedad del tipo `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="68625-155">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="68625-156">Además de los tipos de propiedad anidados, un tipo de propiedad puede hacer referencia a una entidad normal, puede ser el propietario o una entidad distinta, siempre y cuando la entidad propiedad esté en el lado dependiente.</span><span class="sxs-lookup"><span data-stu-id="68625-156">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="68625-157">Esta funcionalidad establece tipos de entidad de propiedad además de tipos complejos en EF6.</span><span class="sxs-lookup"><span data-stu-id="68625-157">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="68625-158">Es posible encadenar el `OwnsOne` método en una llamada fluida para configurar este modelo:</span><span class="sxs-lookup"><span data-stu-id="68625-158">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="68625-159">Observe la `WithOwner` llamada usada para configurar la propiedad de navegación que señala hacia atrás en el propietario.</span><span class="sxs-lookup"><span data-stu-id="68625-159">Notice the `WithOwner` call used to configure the navigation property pointing back at the owner.</span></span>

<span data-ttu-id="68625-160">Es posible lograr el resultado con `OwnedAttribute` `OrderDetails` en y `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="68625-160">It is possible to achieve the result using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="68625-161">Almacenar tipos de propiedad en tablas independientes</span><span class="sxs-lookup"><span data-stu-id="68625-161">Storing owned types in separate tables</span></span>

<span data-ttu-id="68625-162">Además, a diferencia de los tipos complejos de EF6, los tipos de propiedad se pueden almacenar en una tabla independiente del propietario.</span><span class="sxs-lookup"><span data-stu-id="68625-162">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="68625-163">Para invalidar la Convención que asigna un tipo de propiedad a la misma tabla que el propietario, puede llamar `ToTable` a y proporcionar un nombre de tabla diferente.</span><span class="sxs-lookup"><span data-stu-id="68625-163">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="68625-164">En el ejemplo siguiente se `OrderDetails` asignarán y sus dos direcciones a una tabla `DetailedOrder`independiente de:</span><span class="sxs-lookup"><span data-stu-id="68625-164">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="68625-165">Consultar tipos de propiedad</span><span class="sxs-lookup"><span data-stu-id="68625-165">Querying owned types</span></span>

<span data-ttu-id="68625-166">Al consultar al propietario, los tipos de propiedad se incluyen de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="68625-166">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="68625-167">No es necesario utilizar el `Include` método, incluso si los tipos de propiedad se almacenan en una tabla independiente.</span><span class="sxs-lookup"><span data-stu-id="68625-167">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="68625-168">En función del modelo descrito anteriormente, se obtendrá `Order` `OrderDetails` la siguiente consulta y las dos propiedad `StreetAddresses` de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="68625-168">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="68625-169">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="68625-169">Limitations</span></span>

<span data-ttu-id="68625-170">Algunas de estas limitaciones son fundamentales para el funcionamiento de los tipos de entidad de propiedad, pero otras son restricciones que podríamos ser capaces de quitar en versiones futuras:</span><span class="sxs-lookup"><span data-stu-id="68625-170">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="68625-171">Restricciones por diseño</span><span class="sxs-lookup"><span data-stu-id="68625-171">By-design restrictions</span></span>
- <span data-ttu-id="68625-172">No se puede crear `DbSet<T>` un para un tipo de propiedad</span><span class="sxs-lookup"><span data-stu-id="68625-172">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="68625-173">No se puede `Entity<T>()` llamar a con un tipo de propiedad en`ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="68625-173">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="68625-174">Deficiencias actuales</span><span class="sxs-lookup"><span data-stu-id="68625-174">Current shortcomings</span></span>
- <span data-ttu-id="68625-175">No se admiten las jerarquías de herencia que incluyen tipos de entidad de propiedad.</span><span class="sxs-lookup"><span data-stu-id="68625-175">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="68625-176">Las navegaciones de referencia a tipos de entidad de propiedad no pueden ser null a menos que se asignen explícitamente a una tabla independiente del propietario.</span><span class="sxs-lookup"><span data-stu-id="68625-176">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="68625-177">Los distintos propietarios no pueden compartir instancias de tipos de entidad con propiedad (este es un escenario conocido para objetos de valor que no se pueden implementar mediante tipos de entidad de propiedad)</span><span class="sxs-lookup"><span data-stu-id="68625-177">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="68625-178">Deficiencias en versiones anteriores</span><span class="sxs-lookup"><span data-stu-id="68625-178">Shortcomings in previous versions</span></span>
- <span data-ttu-id="68625-179">En EF Core 2,0, las navegaciones a tipos de entidad de propiedad no se pueden declarar en tipos de entidad derivadas a menos que las entidades de propiedad se asignen explícitamente a una tabla independiente de la jerarquía de propietarios.</span><span class="sxs-lookup"><span data-stu-id="68625-179">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="68625-180">Esta limitación se ha eliminado en EF Core 2,1</span><span class="sxs-lookup"><span data-stu-id="68625-180">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="68625-181">En EF Core 2,0 y 2,1 solo se admiten las navegaciones de referencia a los tipos de propiedad.</span><span class="sxs-lookup"><span data-stu-id="68625-181">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="68625-182">Esta limitación se ha eliminado en EF Core 2,2</span><span class="sxs-lookup"><span data-stu-id="68625-182">This limitation has been removed in EF Core 2.2</span></span>
