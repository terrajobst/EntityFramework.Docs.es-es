---
title: Propiedad de tipos de entidad - EF Core
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: 476a1dcaadcd99eba0cd4f5f0ac40c32a97af5c9
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949432"
---
# <a name="owned-entity-types"></a><span data-ttu-id="9f90f-102">Tipos de entidad en propiedad</span><span class="sxs-lookup"><span data-stu-id="9f90f-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="9f90f-103">Esta característica es nueva en EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="9f90f-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="9f90f-104">EF Core permite a tipos de entidad del modelo que sólo pueden aparecer en las propiedades de navegación de otros tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="9f90f-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="9f90f-105">Se denominan _tipos de entidad en propiedad_.</span><span class="sxs-lookup"><span data-stu-id="9f90f-105">These are called _owned entity types_.</span></span> <span data-ttu-id="9f90f-106">La entidad que contiene un tipo de entidad de propiedad es su _propietario_.</span><span class="sxs-lookup"><span data-stu-id="9f90f-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="9f90f-107">Configuración explícita</span><span class="sxs-lookup"><span data-stu-id="9f90f-107">Explicit configuration</span></span>

<span data-ttu-id="9f90f-108">Propiedad de entidad tipos nunca se incluyen por EF Core en el modelo por convención.</span><span class="sxs-lookup"><span data-stu-id="9f90f-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="9f90f-109">Puede usar el `OwnsOne` método `OnModelCreating` o anotar el tipo con `OwnedAttribute` (Novedades de EF Core 2.1) para configurar el tipo como un tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f90f-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="9f90f-110">En este ejemplo, StreetAddress es un tipo con ninguna propiedad de identidad.</span><span class="sxs-lookup"><span data-stu-id="9f90f-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="9f90f-111">Se usa como propiedad del tipo Order para especificar la dirección de envío de un pedido en concreto.</span><span class="sxs-lookup"><span data-stu-id="9f90f-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="9f90f-112">En `OnModelCreating`, usamos el `OwnsOne` método para especificar que la propiedad ShippingAddress es una entidad de propiedad del tipo de orden.</span><span class="sxs-lookup"><span data-stu-id="9f90f-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="9f90f-113">Si la propiedad ShippingAddress es privada en el tipo de pedido, puede usar la versión de cadena del `OwnsOne` método:</span><span class="sxs-lookup"><span data-stu-id="9f90f-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="9f90f-114">En este ejemplo, usamos el `OwnedAttribute` para lograr el mismo objetivo:</span><span class="sxs-lookup"><span data-stu-id="9f90f-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="9f90f-115">Claves implícitas</span><span class="sxs-lookup"><span data-stu-id="9f90f-115">Implicit keys</span></span>

<span data-ttu-id="9f90f-116">En EF Core 2.0 y 2.1, solo las propiedades de navegación de referencia pueden apuntar a tipos de propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f90f-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="9f90f-117">No se admiten colecciones de tipos de propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f90f-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="9f90f-118">Propiedad de la referencia de estos tipos siempre tienen una relación uno a uno con el propietario, por lo tanto, no necesitan sus propios valores de clave.</span><span class="sxs-lookup"><span data-stu-id="9f90f-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="9f90f-119">En el ejemplo anterior, el tipo StreetAddress no necesita definir una propiedad de clave.</span><span class="sxs-lookup"><span data-stu-id="9f90f-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="9f90f-120">Para comprender cómo EF Core realiza el seguimiento de estos objetos, es útil considerar que una clave principal se crea como un [sombrear a la propiedad](xref:core/modeling/shadow-properties) para el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f90f-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="9f90f-121">El valor de la clave de una instancia del tipo de propiedad será el mismo que el valor de la clave de la instancia de propietario.</span><span class="sxs-lookup"><span data-stu-id="9f90f-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="9f90f-122">Asignación de tipos con la división de tablas de propiedad</span><span class="sxs-lookup"><span data-stu-id="9f90f-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="9f90f-123">Al usar bases de datos relacionales, por convención, tipos de propiedad se asignan a la misma tabla como el propietario.</span><span class="sxs-lookup"><span data-stu-id="9f90f-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="9f90f-124">Esto requiere la división de la tabla en dos: algunas columnas que se usará para almacenar los datos del propietario, y algunas columnas que se usará para almacenar los datos de la entidad en propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f90f-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="9f90f-125">Se trata de una característica común que se conoce como la división de tablas.</span><span class="sxs-lookup"><span data-stu-id="9f90f-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="9f90f-126">Propiedad de tipos almacenados con la división de tablas se pueden usar de forma muy similar a los tipos complejos de cómo se usan en EF6.</span><span class="sxs-lookup"><span data-stu-id="9f90f-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="9f90f-127">Por convención, EF Core asignará nombre a las columnas de base de datos para las propiedades del tipo de entidad propiedad seguir el patrón de _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="9f90f-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="9f90f-128">Por lo tanto, las propiedades StreetAddress aparecerá en la tabla Orders con los nombres ShippingAddress_Street y ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="9f90f-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="9f90f-129">Puede anexar el `HasColumnName` método para cambiar el nombre de esas columnas.</span><span class="sxs-lookup"><span data-stu-id="9f90f-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="9f90f-130">En el caso donde StreetAddress es una propiedad pública, las asignaciones serían</span><span class="sxs-lookup"><span data-stu-id="9f90f-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="9f90f-131">Uso compartido entre varios tipos que se poseen el mismo tipo de .NET</span><span class="sxs-lookup"><span data-stu-id="9f90f-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="9f90f-132">Un tipo de entidad de propiedad puede ser del mismo tipo de .NET como otro tipo de entidad en propiedad, por lo tanto, que el tipo de .NET puede no ser suficiente para identificar un tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f90f-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="9f90f-133">En esos casos, se convierte en la propiedad señalando el propietario de la entidad en propiedad el _definir la navegación_ del tipo de entidad en propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f90f-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="9f90f-134">Desde la perspectiva del EF Core, la definición de la navegación es parte de la identidad del tipo junto con el tipo de .NET.</span><span class="sxs-lookup"><span data-stu-id="9f90f-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="9f90f-135">Por ejemplo, en la siguiente clase, ShippingAddress y BillingAddress son del mismo tipo. NET, StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="9f90f-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="9f90f-136">Para comprender cómo EF Core distinguirá sometidas a seguimiento de las instancias de estos objetos, puede ser útil pensar que la navegación definitoria se ha convertido en parte de la clave de la instancia junto con el valor de la clave del propietario y el tipo de .NET del tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f90f-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="9f90f-137">Tipos de propiedad anidados</span><span class="sxs-lookup"><span data-stu-id="9f90f-137">Nested owned types</span></span>

<span data-ttu-id="9f90f-138">En este ejemplo OrderDetails posee BillingAddress y ShippingAddress, que son tipos StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="9f90f-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="9f90f-139">A continuación, OrderDetails es propiedad del tipo de orden.</span><span class="sxs-lookup"><span data-stu-id="9f90f-139">Then OrderDetails is owned by the Order type.</span></span>

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

<span data-ttu-id="9f90f-140">Es posible encadenar el `OwnsOne` método en una asignación fluida para configurar este modelo:</span><span class="sxs-lookup"><span data-stu-id="9f90f-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="9f90f-141">Es posible lograr lo mismo mediante `OwnedAttribute` en OrderDetails y StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="9f90f-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="9f90f-142">Además de los tipos de propiedad anidados, un tipo de propiedad puede hacer referencia a una entidad regular.</span><span class="sxs-lookup"><span data-stu-id="9f90f-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="9f90f-143">En el ejemplo siguiente, país es una entidad no propiedad normal:</span><span class="sxs-lookup"><span data-stu-id="9f90f-143">In the following example, Country is a regular non-owned entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="9f90f-144">Esta capacidad establece los tipos de entidad en propiedad aparte de los tipos complejos en EF6.</span><span class="sxs-lookup"><span data-stu-id="9f90f-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="9f90f-145">Almacenar tipos de propiedad en tablas independientes</span><span class="sxs-lookup"><span data-stu-id="9f90f-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="9f90f-146">También a diferencia de los tipos complejos de EF6, tipos de propiedad pueden almacenarse en una tabla independiente del propietario.</span><span class="sxs-lookup"><span data-stu-id="9f90f-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="9f90f-147">Con el fin de invalidar la convención que asigna un tipo de propiedad a la misma tabla como el propietario, puede llamar simplemente `ToTable` y proporcione un nombre de tabla diferente.</span><span class="sxs-lookup"><span data-stu-id="9f90f-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="9f90f-148">En el siguiente ejemplo se asignará OrderDetails y sus dos direcciones a una tabla independiente de orden:</span><span class="sxs-lookup"><span data-stu-id="9f90f-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="9f90f-149">Consultar los tipos de propiedad</span><span class="sxs-lookup"><span data-stu-id="9f90f-149">Querying owned types</span></span>

<span data-ttu-id="9f90f-150">Al consultar al propietario, los tipos de propiedad se incluyen de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="9f90f-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="9f90f-151">No es necesario utilizar el `Include` método, incluso si los tipos de propiedad se almacenan en una tabla independiente.</span><span class="sxs-lookup"><span data-stu-id="9f90f-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="9f90f-152">Según el modelo descrito anteriormente, la siguiente consulta extraerá Order, OrderDetails y los dos StreetAddresses propietario para todos los pedidos pendientes de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="9f90f-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreetAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="9f90f-153">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="9f90f-153">Limitations</span></span>

<span data-ttu-id="9f90f-154">Algunas de estas limitaciones son fundamentales para que se poseen cómo funcionan los tipos entity, pero otros son restricciones que podamos quite en futuras versiones:</span><span class="sxs-lookup"><span data-stu-id="9f90f-154">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="9f90f-155">Limitaciones en las versiones anteriores</span><span class="sxs-lookup"><span data-stu-id="9f90f-155">Shortcomings in previous versions</span></span>
- <span data-ttu-id="9f90f-156">En EF Core 2.0, navegaciones a propiedad de tipos de entidad no se puede declarar en tipos de entidad derivados a menos que las entidades en propiedad explícitamente se asignan a una tabla independiente de la jerarquía del propietario.</span><span class="sxs-lookup"><span data-stu-id="9f90f-156">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="9f90f-157">Esta limitación se ha quitado en EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="9f90f-157">This limitation has been removed in EF Core 2.1</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="9f90f-158">Limitaciones actuales</span><span class="sxs-lookup"><span data-stu-id="9f90f-158">Current shortcomings</span></span>
- <span data-ttu-id="9f90f-159">Jerarquías de herencia que incluyen propiedad no se admiten los tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="9f90f-159">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="9f90f-160">Tipos de entidad de propiedad no señala a una propiedad de navegación de colección (referencia solo se admiten actualmente las navegaciones)</span><span class="sxs-lookup"><span data-stu-id="9f90f-160">Owned entity types cannot be pointed at by a collection navigation property (only reference navigations are currently supported)</span></span>
- <span data-ttu-id="9f90f-161">Navegaciones a la propiedad de tipos de entidad no pueden ser nulos a menos que explícitamente se asignan a una tabla independiente del propietario</span><span class="sxs-lookup"><span data-stu-id="9f90f-161">Navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="9f90f-162">Las instancias de tipos de entidad de propiedad no pueden compartirse entre varios propietarios (este es un escenario muy conocido para los objetos de valor que no se puede implementar mediante tipos de entidad de propiedad)</span><span class="sxs-lookup"><span data-stu-id="9f90f-162">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="9f90f-163">Restricciones de diseño</span><span class="sxs-lookup"><span data-stu-id="9f90f-163">By-design restrictions</span></span>
- <span data-ttu-id="9f90f-164">No puede crear un `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="9f90f-164">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="9f90f-165">No se puede llamar a `Entity<T>()` con un tipo de propiedad en `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="9f90f-165">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
