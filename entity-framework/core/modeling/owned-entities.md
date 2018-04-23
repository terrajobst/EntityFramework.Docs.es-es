---
title: Propiedad de tipos de entidad - Core EF
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: f2f05499a3e3494f420d916df2db19667a6f1e29
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/19/2018
---
# <a name="owned-entity-types"></a><span data-ttu-id="77d2a-102">Tipos de entidad en propiedad</span><span class="sxs-lookup"><span data-stu-id="77d2a-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="77d2a-103">Esta función es nueva en EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="77d2a-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="77d2a-104">Núcleo EF permite a tipos de entidad del modelo que sólo pueden aparecer en las propiedades de navegación de otros tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="77d2a-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="77d2a-105">Estos se denominan _propiedad de tipos de entidad_.</span><span class="sxs-lookup"><span data-stu-id="77d2a-105">These are called _owned entity types_.</span></span> <span data-ttu-id="77d2a-106">La entidad que contiene un tipo de entidad en propiedad es su _propietario_.</span><span class="sxs-lookup"><span data-stu-id="77d2a-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="77d2a-107">Configuración explícita</span><span class="sxs-lookup"><span data-stu-id="77d2a-107">Explicit configuration</span></span>

<span data-ttu-id="77d2a-108">Propiedad de entidad tipos nunca se incluyen por núcleo de EF en el modelo por convención.</span><span class="sxs-lookup"><span data-stu-id="77d2a-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="77d2a-109">Puede utilizar el `OwnsOne` método `OnModelCreating` o anotar el tipo con `OwnedAttribute` (nuevo en EF Core 2.1) para configurar el tipo como un tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="77d2a-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="77d2a-110">En este ejemplo, StreetAddress es un tipo con ninguna propiedad de identidad.</span><span class="sxs-lookup"><span data-stu-id="77d2a-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="77d2a-111">Se usa como propiedad del tipo Order para especificar la dirección de envío de un pedido en concreto.</span><span class="sxs-lookup"><span data-stu-id="77d2a-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="77d2a-112">En `OnModelCreating`, usamos el `OwnsOne` para especificar que la propiedad ShippingAddress es una entidad de propiedad del tipo de orden.</span><span class="sxs-lookup"><span data-stu-id="77d2a-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="77d2a-113">Si la propiedad ShippingAddress es privada en el tipo de pedido, puede utilizar la versión de cadena de la `OwnsOne` método:</span><span class="sxs-lookup"><span data-stu-id="77d2a-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="77d2a-114">En este ejemplo, usamos el `OwnedAttribute` para lograr el mismo objetivo:</span><span class="sxs-lookup"><span data-stu-id="77d2a-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="77d2a-115">Claves implícitas</span><span class="sxs-lookup"><span data-stu-id="77d2a-115">Implicit keys</span></span>

<span data-ttu-id="77d2a-116">En EF Core 2.0 y 2.1, propiedades de navegación de referencia sólo pueden señalar a los tipos de propiedad.</span><span class="sxs-lookup"><span data-stu-id="77d2a-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="77d2a-117">No se admiten colecciones de tipos de propiedad.</span><span class="sxs-lookup"><span data-stu-id="77d2a-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="77d2a-118">Estas referencia propiedad tipos siempre tienen una relación uno a uno con el propietario, por lo tanto, no necesitan sus propios valores de clave.</span><span class="sxs-lookup"><span data-stu-id="77d2a-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="77d2a-119">En el ejemplo anterior, el tipo de StreetAddress no tiene que definir una propiedad de clave.</span><span class="sxs-lookup"><span data-stu-id="77d2a-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="77d2a-120">Para comprender cómo EF núcleo realiza el seguimiento de estos objetos, es útil considerar que una clave principal se crea como un [sombrear a la propiedad](xref:core/modeling/shadow-properties) para el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="77d2a-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="77d2a-121">El valor de la clave de una instancia del tipo de propiedad será el mismo que el valor de la clave de la instancia de propietario.</span><span class="sxs-lookup"><span data-stu-id="77d2a-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="77d2a-122">Tipos con la división de la tabla de asignación de propiedad</span><span class="sxs-lookup"><span data-stu-id="77d2a-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="77d2a-123">Al utilizar bases de datos relacionales, por convención, los tipos de propiedad se asignan a la misma tabla que el propietario.</span><span class="sxs-lookup"><span data-stu-id="77d2a-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="77d2a-124">Esto requiere dividir la tabla en dos: algunas columnas se utilizará para almacenar los datos del propietario, y algunas columnas se utilizará para almacenar los datos de la entidad en propiedad.</span><span class="sxs-lookup"><span data-stu-id="77d2a-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="77d2a-125">Se trata de una característica común conocida como división de la tabla.</span><span class="sxs-lookup"><span data-stu-id="77d2a-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="77d2a-126">Propiedad almacenados con la división de la tabla de tipos pueden ser utilizados de forma muy similar a tipos complejos de cómo se utilizan en EF6.</span><span class="sxs-lookup"><span data-stu-id="77d2a-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="77d2a-127">Por convención, Core EF asignará nombre a las columnas de base de datos para las propiedades del tipo de entidad propiedad seguir el patrón de _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="77d2a-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="77d2a-128">Por consiguiente, las propiedades StreetAddress aparecerá en la tabla de pedidos con los nombres ShippingAddress_Street y ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="77d2a-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="77d2a-129">Puede anexar el `HasColumnName` método para cambiar el nombre de las columnas.</span><span class="sxs-lookup"><span data-stu-id="77d2a-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="77d2a-130">En el caso donde StreetAddress es una propiedad pública, sería las asignaciones</span><span class="sxs-lookup"><span data-stu-id="77d2a-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="77d2a-131">Compartir el mismo tipo de .NET entre varios tipos de propiedad</span><span class="sxs-lookup"><span data-stu-id="77d2a-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="77d2a-132">Un tipo de entidad en propiedad puede ser del mismo tipo .NET como otro tipo de entidad en propiedad, por lo tanto, que el tipo de .NET puede no ser suficiente para identificar un tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="77d2a-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="77d2a-133">En esos casos, la propiedad señalando el propietario de la entidad de propiedad se convierte en el _definición de exploración_ del tipo de entidad en propiedad.</span><span class="sxs-lookup"><span data-stu-id="77d2a-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="77d2a-134">Desde la perspectiva del núcleo EF, la definición de la navegación es parte de la identidad del tipo junto con el tipo de .NET.</span><span class="sxs-lookup"><span data-stu-id="77d2a-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="77d2a-135">Por ejemplo, en la clase siguiente, ShippingAddress y BillingAddress son ambas del mismo tipo. NET, StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="77d2a-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="77d2a-136">Para comprender cómo EF Core distinguirá marcas instancias de estos objetos, puede ser útil pensar que la definición de la navegación se ha convertido en parte de la clave de la instancia junto con el valor de la clave del propietario y el tipo de .NET del tipo propietario.</span><span class="sxs-lookup"><span data-stu-id="77d2a-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="77d2a-137">Propietario de tipos anidados</span><span class="sxs-lookup"><span data-stu-id="77d2a-137">Nested owned types</span></span>

<span data-ttu-id="77d2a-138">En este ejemplo OrderDetails posee BillingAddress y ShippingAddress, que son tipos StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="77d2a-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="77d2a-139">A continuación, el tipo de pedido pertenece OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="77d2a-139">Then OrderDetails is owned by the Order type.</span></span>

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

<span data-ttu-id="77d2a-140">Es posible que la cadena de la `OwnsOne` método en una asignación fluent para configurar este modelo:</span><span class="sxs-lookup"><span data-stu-id="77d2a-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="77d2a-141">Es posible lograr lo mismo con `OwnedAttribute` en OrderDetails y StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="77d2a-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="77d2a-142">Además de los tipos de propiedad anidados, un tipo de propiedad puede hacer referencia a una entidad normal.</span><span class="sxs-lookup"><span data-stu-id="77d2a-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="77d2a-143">En el ejemplo siguiente, el país es una entidad normal (es decir, no son propiedad de):</span><span class="sxs-lookup"><span data-stu-id="77d2a-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="77d2a-144">Esta función establece los tipos de entidad en propiedad aparte de tipos complejos EF6.</span><span class="sxs-lookup"><span data-stu-id="77d2a-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="77d2a-145">Almacenar tipos en tablas independientes de propiedad</span><span class="sxs-lookup"><span data-stu-id="77d2a-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="77d2a-146">También a diferencia de los tipos complejos de EF6, tipos de propiedad pueden almacenarse en una tabla independiente del propietario.</span><span class="sxs-lookup"><span data-stu-id="77d2a-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="77d2a-147">Para reemplazar la convención que asigna un tipo perteneciente a la misma tabla que el propietario, puede llamar simplemente `ToTable` y proporcione un nombre de tabla diferente.</span><span class="sxs-lookup"><span data-stu-id="77d2a-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="77d2a-148">En el siguiente ejemplo se asignará OrderDetails y sus dos direcciones en una tabla independiente del orden:</span><span class="sxs-lookup"><span data-stu-id="77d2a-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="77d2a-149">Consultar tipos de propiedad</span><span class="sxs-lookup"><span data-stu-id="77d2a-149">Querying owned types</span></span>

<span data-ttu-id="77d2a-150">Al consultar al propietario, los tipos de propiedad se incluyen de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="77d2a-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="77d2a-151">No es necesario utilizar el `Include` método, incluso si los tipos de propiedad se almacenan en una tabla independiente.</span><span class="sxs-lookup"><span data-stu-id="77d2a-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="77d2a-152">Se basa en el modelo descrito anteriormente, la siguiente consulta extraerá Order, OrderDetails y los dos StreeAddresses propietario para todos los pedidos pendientes de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="77d2a-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="77d2a-153">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="77d2a-153">Limitations</span></span>

<span data-ttu-id="77d2a-154">Aquí hay algunas limitaciones de los tipos de entidad en propiedad.</span><span class="sxs-lookup"><span data-stu-id="77d2a-154">Here are some limitations of owned entity types.</span></span> <span data-ttu-id="77d2a-155">Algunas de estas limitaciones son fundamentales para la propiedad de tipos de trabajo, pero otros son restricciones de punto en el tiempo que nos gustaría quitar en futuras versiones:</span><span class="sxs-lookup"><span data-stu-id="77d2a-155">Some of these limitations are fundamental to how owned types work, but some others are point-in-time restrictions that we would like to remove in future releases:</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="77d2a-156">Actuales deficiencias</span><span class="sxs-lookup"><span data-stu-id="77d2a-156">Current shortcomings</span></span>
- <span data-ttu-id="77d2a-157">No se admite la herencia de tipos de propiedad</span><span class="sxs-lookup"><span data-stu-id="77d2a-157">Inheritance of owned types is not supported</span></span>
- <span data-ttu-id="77d2a-158">Tipos de propiedad no pueden ser señalados por una propiedad de navegación de la colección</span><span class="sxs-lookup"><span data-stu-id="77d2a-158">Owned types cannot be pointed at by a collection navigation property</span></span>
- <span data-ttu-id="77d2a-159">Puesto que utiliza la división de la tabla por predeterminado propiedad de tipos también tienen las siguientes restricciones a menos que explícitamente asignado a una tabla diferente:</span><span class="sxs-lookup"><span data-stu-id="77d2a-159">Since they use table splitting by default owned types also have the following restrictions unless explicitly mapped to a different table:</span></span>
   - <span data-ttu-id="77d2a-160">No puede ser propiedad de un tipo derivado</span><span class="sxs-lookup"><span data-stu-id="77d2a-160">They cannot be owned by a derived type</span></span>
   - <span data-ttu-id="77d2a-161">No se puede establecer la propiedad definidora de exploración a null (es decir, propiedad de tipos en la misma tabla siempre son necesarios)</span><span class="sxs-lookup"><span data-stu-id="77d2a-161">The defining navigation property cannot be set to null (i.e. owned types on the same table are always required)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="77d2a-162">Restricciones de diseño</span><span class="sxs-lookup"><span data-stu-id="77d2a-162">By-design restrictions</span></span>
- <span data-ttu-id="77d2a-163">No puede crear un `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="77d2a-163">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="77d2a-164">No se puede llamar `Entity<T>()` con un tipo de propiedad en `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="77d2a-164">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
