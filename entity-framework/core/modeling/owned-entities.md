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
# <a name="owned-entity-types"></a><span data-ttu-id="4dde3-102">Tipos de entidad en propiedad</span><span class="sxs-lookup"><span data-stu-id="4dde3-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="4dde3-103">Esta característica es nueva en EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="4dde3-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="4dde3-104">EF Core permite a tipos de entidad del modelo que sólo pueden aparecer en las propiedades de navegación de otros tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="4dde3-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="4dde3-105">Se denominan _tipos de entidad en propiedad_.</span><span class="sxs-lookup"><span data-stu-id="4dde3-105">These are called _owned entity types_.</span></span> <span data-ttu-id="4dde3-106">La entidad que contiene un tipo de entidad de propiedad es su _propietario_.</span><span class="sxs-lookup"><span data-stu-id="4dde3-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="4dde3-107">Configuración explícita</span><span class="sxs-lookup"><span data-stu-id="4dde3-107">Explicit configuration</span></span>

<span data-ttu-id="4dde3-108">Propiedad de entidad tipos nunca se incluyen por EF Core en el modelo por convención.</span><span class="sxs-lookup"><span data-stu-id="4dde3-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="4dde3-109">Puede usar el `OwnsOne` método `OnModelCreating` o anotar el tipo con `OwnedAttribute` (Novedades de EF Core 2.1) para configurar el tipo como un tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="4dde3-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="4dde3-110">En este ejemplo, `StreetAddress` es un tipo con ninguna propiedad de identidad.</span><span class="sxs-lookup"><span data-stu-id="4dde3-110">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="4dde3-111">Se usa como propiedad del tipo Order para especificar la dirección de envío de un pedido en concreto.</span><span class="sxs-lookup"><span data-stu-id="4dde3-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="4dde3-112">Podemos usar el `OwnedAttribute` para tratarlo como una entidad de propiedad cuando se hace referencia desde otro tipo de entidad:</span><span class="sxs-lookup"><span data-stu-id="4dde3-112">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="4dde3-113">También es posible usar el `OwnsOne` método `OnModelCreating` para especificar que el `ShippingAddress` propiedad es una entidad de propiedad de la `Order` tipo de entidad y configurar aspectos adicionales si es necesario.</span><span class="sxs-lookup"><span data-stu-id="4dde3-113">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="4dde3-114">Si el `ShippingAddress` propiedad es privada en el `Order` tipo, puede usar la versión de cadena de la `OwnsOne` método:</span><span class="sxs-lookup"><span data-stu-id="4dde3-114">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="4dde3-115">Consulte la [proyecto de ejemplo completa](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) para obtener más contexto.</span><span class="sxs-lookup"><span data-stu-id="4dde3-115">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="4dde3-116">Claves implícitas</span><span class="sxs-lookup"><span data-stu-id="4dde3-116">Implicit keys</span></span>

<span data-ttu-id="4dde3-117">Propiedad tipos configurados con `OwnsOne` o detectados a través de una navegación de referencia siempre tienen una relación uno a uno con el propietario, por lo tanto, no necesitan sus propios valores de clave ya que son únicos los valores de clave externos.</span><span class="sxs-lookup"><span data-stu-id="4dde3-117">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="4dde3-118">En el ejemplo anterior, el `StreetAddress` tipo no necesita definir una propiedad de clave.</span><span class="sxs-lookup"><span data-stu-id="4dde3-118">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="4dde3-119">Para comprender cómo EF Core realiza el seguimiento de estos objetos, es útil pensar que una clave principal se crea como un [propiedad reemplazada](xref:core/modeling/shadow-properties) para el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="4dde3-119">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="4dde3-120">El valor de la clave de una instancia del tipo de propiedad será el mismo que el valor de la clave de la instancia de propietario.</span><span class="sxs-lookup"><span data-stu-id="4dde3-120">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="4dde3-121">Colecciones de tipos de propiedad</span><span class="sxs-lookup"><span data-stu-id="4dde3-121">Collections of owned types</span></span>

>[!NOTE]
> <span data-ttu-id="4dde3-122">Esta característica es nueva en EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="4dde3-122">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="4dde3-123">Para configurar una colección de tipos de propiedad `OwnsMany` debe usarse en `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="4dde3-123">To configure a collection of owned types `OwnsMany` should be used in `OnModelCreating`.</span></span> <span data-ttu-id="4dde3-124">Sin embargo la clave principal no se configura por convención, por lo que necesita especificarse explícitamente.</span><span class="sxs-lookup"><span data-stu-id="4dde3-124">However the primary key will not be configured by convention, so it need to be specified explicitly.</span></span> <span data-ttu-id="4dde3-125">Es habitual usar una clave compleja para estos tipos de entidades que incorpora la clave externa para el propietario y una propiedad adicional única que también puede estar en estado de la sombra:</span><span class="sxs-lookup"><span data-stu-id="4dde3-125">It is common to use a complex key for these type of entities incorporating the foreign key to the owner and an additional unique property that can also be in shadow state:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="4dde3-126">Asignación de tipos con la división de tablas de propiedad</span><span class="sxs-lookup"><span data-stu-id="4dde3-126">Mapping owned types with table splitting</span></span>

<span data-ttu-id="4dde3-127">Al usar relacional bases de datos, por referencia convención propiedad tipos se asignan a la misma tabla como el propietario.</span><span class="sxs-lookup"><span data-stu-id="4dde3-127">When using relational databases, by convention reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="4dde3-128">Esto requiere la división de la tabla en dos: algunas columnas que se usará para almacenar los datos del propietario, y algunas columnas que se usará para almacenar los datos de la entidad en propiedad.</span><span class="sxs-lookup"><span data-stu-id="4dde3-128">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="4dde3-129">Se trata de una característica común que se conoce como la división de tablas.</span><span class="sxs-lookup"><span data-stu-id="4dde3-129">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="4dde3-130">Propiedad de tipos almacenados con la división de tablas se pueden usar de forma similar a los tipos complejos de cómo se usan en EF6.</span><span class="sxs-lookup"><span data-stu-id="4dde3-130">Owned types stored with table splitting can be used similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="4dde3-131">Por convención, EF Core denominará las columnas de la base de datos para las propiedades del tipo de entidad en propiedad siguiendo el patrón de _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="4dde3-131">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="4dde3-132">Por lo tanto, la `StreetAddress` propiedades aparecerán en la tabla "Orders" con los nombres 'ShippingAddress_Street' y 'ShippingAddress_City'.</span><span class="sxs-lookup"><span data-stu-id="4dde3-132">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="4dde3-133">Puede anexar el `HasColumnName` método para cambiar el nombre de las columnas:</span><span class="sxs-lookup"><span data-stu-id="4dde3-133">You can append the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="4dde3-134">Uso compartido entre varios tipos que se poseen el mismo tipo de .NET</span><span class="sxs-lookup"><span data-stu-id="4dde3-134">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="4dde3-135">Un tipo de entidad de propiedad puede ser del mismo tipo de .NET como otro tipo de entidad en propiedad, por lo tanto, que el tipo de .NET puede no ser suficiente para identificar un tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="4dde3-135">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="4dde3-136">En esos casos, se convierte en la propiedad señalando el propietario de la entidad en propiedad el _definir la navegación_ del tipo de entidad en propiedad.</span><span class="sxs-lookup"><span data-stu-id="4dde3-136">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="4dde3-137">Desde la perspectiva del EF Core, la definición de la navegación es parte de la identidad del tipo junto con el tipo de .NET.</span><span class="sxs-lookup"><span data-stu-id="4dde3-137">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="4dde3-138">Por ejemplo, en la siguiente clase `ShippingAddress` y `BillingAddress` son ambas del mismo tipo. NET, `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="4dde3-138">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="4dde3-139">Para comprender cómo EF Core distinguirá sometidas a seguimiento de las instancias de estos objetos, puede ser útil pensar que la navegación definitoria se ha convertido en parte de la clave de la instancia junto con el valor de la clave del propietario y el tipo de .NET del tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="4dde3-139">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="4dde3-140">Tipos de propiedad anidados</span><span class="sxs-lookup"><span data-stu-id="4dde3-140">Nested owned types</span></span>

<span data-ttu-id="4dde3-141">En este ejemplo `OrderDetails` posee `BillingAddress` y `ShippingAddress`, que son ambos `StreetAddress` tipos.</span><span class="sxs-lookup"><span data-stu-id="4dde3-141">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="4dde3-142">Luego, `OrderDetails` es propiedad del tipo `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="4dde3-142">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="4dde3-143">Además de los tipos de propiedad anidados, un tipo de propiedad puede hacer referencia a una entidad normal, puede ser el propietario o una entidad diferente siempre que sea la entidad de propiedad en el lado dependiente.</span><span class="sxs-lookup"><span data-stu-id="4dde3-143">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="4dde3-144">Esta capacidad establece los tipos de entidad en propiedad aparte de los tipos complejos en EF6.</span><span class="sxs-lookup"><span data-stu-id="4dde3-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="4dde3-145">Es posible encadenar el `OwnsOne` método en una llamada fluida para configurar este modelo:</span><span class="sxs-lookup"><span data-stu-id="4dde3-145">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="4dde3-146">También es posible lograr lo mismo mediante `OwnedAttribute` en ambos `OrderDetails` y `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="4dde3-146">It is also possible to achieve the same thing using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="4dde3-147">Almacenar tipos de propiedad en tablas independientes</span><span class="sxs-lookup"><span data-stu-id="4dde3-147">Storing owned types in separate tables</span></span>

<span data-ttu-id="4dde3-148">También a diferencia de los tipos complejos de EF6, tipos de propiedad pueden almacenarse en una tabla independiente del propietario.</span><span class="sxs-lookup"><span data-stu-id="4dde3-148">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="4dde3-149">Con el fin de invalidar la convención que asigna un tipo de propiedad a la misma tabla como el propietario, puede llamar simplemente `ToTable` y proporcione un nombre de tabla diferente.</span><span class="sxs-lookup"><span data-stu-id="4dde3-149">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="4dde3-150">En el ejemplo siguiente, se asignará `OrderDetails` y sus dos direcciones a una tabla independiente de `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="4dde3-150">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="4dde3-151">Consultar los tipos de propiedad</span><span class="sxs-lookup"><span data-stu-id="4dde3-151">Querying owned types</span></span>

<span data-ttu-id="4dde3-152">Al consultar al propietario, los tipos de propiedad se incluyen de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="4dde3-152">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="4dde3-153">No es necesario utilizar el `Include` método, incluso si los tipos de propiedad se almacenan en una tabla independiente.</span><span class="sxs-lookup"><span data-stu-id="4dde3-153">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="4dde3-154">Según el modelo descrito anteriormente, obtendrá la siguiente consulta `Order`, `OrderDetails` y los dos pertenece `StreetAddresses` desde la base de datos:</span><span class="sxs-lookup"><span data-stu-id="4dde3-154">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="4dde3-155">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="4dde3-155">Limitations</span></span>

<span data-ttu-id="4dde3-156">Algunas de estas limitaciones son fundamentales para que se poseen cómo funcionan los tipos entity, pero otros son restricciones que podamos quite en futuras versiones:</span><span class="sxs-lookup"><span data-stu-id="4dde3-156">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="4dde3-157">Restricciones de diseño</span><span class="sxs-lookup"><span data-stu-id="4dde3-157">By-design restrictions</span></span>
- <span data-ttu-id="4dde3-158">No puede crear un `DbSet<T>` para un tipo de propiedad</span><span class="sxs-lookup"><span data-stu-id="4dde3-158">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="4dde3-159">No se puede llamar a `Entity<T>()` con un tipo de propiedad en `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="4dde3-159">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="4dde3-160">Limitaciones actuales</span><span class="sxs-lookup"><span data-stu-id="4dde3-160">Current shortcomings</span></span>
- <span data-ttu-id="4dde3-161">Jerarquías de herencia que incluyen propiedad no se admiten los tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="4dde3-161">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="4dde3-162">Propiedad de las navegaciones de referencia para tipos de entidad no pueden ser nulos a menos que explícitamente se asignan a una tabla independiente del propietario del</span><span class="sxs-lookup"><span data-stu-id="4dde3-162">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="4dde3-163">Las instancias de tipos de entidad de propiedad no pueden compartirse entre varios propietarios (este es un escenario muy conocido para los objetos de valor que no se puede implementar mediante tipos de entidad de propiedad)</span><span class="sxs-lookup"><span data-stu-id="4dde3-163">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="4dde3-164">Limitaciones en las versiones anteriores</span><span class="sxs-lookup"><span data-stu-id="4dde3-164">Shortcomings in previous versions</span></span>
- <span data-ttu-id="4dde3-165">En EF Core 2.0, navegaciones a propiedad de tipos de entidad no se puede declarar en tipos de entidad derivados a menos que las entidades en propiedad explícitamente se asignan a una tabla independiente de la jerarquía del propietario.</span><span class="sxs-lookup"><span data-stu-id="4dde3-165">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="4dde3-166">Esta limitación se ha quitado en EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="4dde3-166">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="4dde3-167">Se admitían navegaciones en EF Core 2.0 y 2.1 de referencia sola a tipos de propiedad.</span><span class="sxs-lookup"><span data-stu-id="4dde3-167">En EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="4dde3-168">Esta limitación se ha quitado en EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="4dde3-168">This limitation has been removed in EF Core 2.2</span></span>