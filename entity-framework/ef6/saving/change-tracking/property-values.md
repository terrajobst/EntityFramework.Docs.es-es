---
title: Trabajar con valores de propiedad - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
caps.latest.revision: 3
ms.openlocfilehash: 07b71c9efe4e1fc3fd25a52c9cfb25f61e92f859
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121919"
---
# <a name="working-with-property-values"></a><span data-ttu-id="84ab0-102">Trabajar con valores de propiedad</span><span class="sxs-lookup"><span data-stu-id="84ab0-102">Working with property values</span></span>
<span data-ttu-id="84ab0-103">La mayor parte, Entity Framework se encargará de seguimiento del estado, los valores originales y los valores actuales de las propiedades de las instancias de entidad.</span><span class="sxs-lookup"><span data-stu-id="84ab0-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="84ab0-104">Sin embargo, puede haber algunos casos, como los escenarios desconectados - donde desea ver o manipular la información de que EF tiene acerca de las propiedades.</span><span class="sxs-lookup"><span data-stu-id="84ab0-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="84ab0-105">Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.</span><span class="sxs-lookup"><span data-stu-id="84ab0-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="84ab0-106">Entity Framework realiza un seguimiento de los dos valores para cada propiedad de una entidad con seguimiento.</span><span class="sxs-lookup"><span data-stu-id="84ab0-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="84ab0-107">El valor actual es, como su nombre indica, el valor actual de la propiedad de la entidad.</span><span class="sxs-lookup"><span data-stu-id="84ab0-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="84ab0-108">El valor original es el valor de la propiedad que tenía cuando se consultan desde la base de datos o adjunta al contexto de la entidad.</span><span class="sxs-lookup"><span data-stu-id="84ab0-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="84ab0-109">Existen dos mecanismos generales para trabajar con valores de propiedad:</span><span class="sxs-lookup"><span data-stu-id="84ab0-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="84ab0-110">El valor de una propiedad única puede obtenerse de una manera fuertemente tipada mediante el método de propiedad.</span><span class="sxs-lookup"><span data-stu-id="84ab0-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="84ab0-111">Los valores para todas las propiedades de una entidad pueden leerse en un objeto DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="84ab0-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="84ab0-112">DbPropertyValues, a continuación, actúa como un objeto de diccionario para permitir que se puede leer y establecer los valores de propiedad.</span><span class="sxs-lookup"><span data-stu-id="84ab0-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="84ab0-113">Los valores de un objeto DbPropertyValues pueden establecerse de valores en otro objeto DbPropertyValues o de los valores de otro objeto, como otra copia de la entidad o un objeto de transferencia de datos simples (DTO).</span><span class="sxs-lookup"><span data-stu-id="84ab0-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="84ab0-114">Las secciones siguientes muestran ejemplos del uso de los mecanismos anteriores ambos.</span><span class="sxs-lookup"><span data-stu-id="84ab0-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="84ab0-115">Obtener y establecer el valor original o actual de una propiedad individual</span><span class="sxs-lookup"><span data-stu-id="84ab0-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="84ab0-116">El ejemplo siguiente muestra cómo se puede leer el valor actual de una propiedad y, a continuación, se establece en un nuevo valor:</span><span class="sxs-lookup"><span data-stu-id="84ab0-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(name).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="84ab0-117">Utilice la propiedad OriginalValue en lugar de la propiedad CurrentValue para leer o establecer el valor original.</span><span class="sxs-lookup"><span data-stu-id="84ab0-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="84ab0-118">Tenga en cuenta que el valor devuelto se escribe como "objeto" cuando se usa una cadena para especificar el nombre de propiedad.</span><span class="sxs-lookup"><span data-stu-id="84ab0-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="84ab0-119">Por otro lado, el valor devuelto está fuertemente tipado si se usa una expresión lambda.</span><span class="sxs-lookup"><span data-stu-id="84ab0-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="84ab0-120">Establecer el valor de propiedad similar al siguiente solo marcar la propiedad como modificada si el nuevo valor es diferente del valor antiguo.</span><span class="sxs-lookup"><span data-stu-id="84ab0-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="84ab0-121">Cuando se establece un valor de propiedad de este modo el cambio se detecta automáticamente, incluso si se desactiva AutoDetectChanges.</span><span class="sxs-lookup"><span data-stu-id="84ab0-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="84ab0-122">Obtener y establecer el valor actual de una propiedad sin asignar</span><span class="sxs-lookup"><span data-stu-id="84ab0-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="84ab0-123">También se puede leer el valor actual de una propiedad que no está asignado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="84ab0-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="84ab0-124">Un ejemplo de una propiedad sin asignar podría ser una propiedad RssLink en el Blog.</span><span class="sxs-lookup"><span data-stu-id="84ab0-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="84ab0-125">Este valor se puede calcular basándose en la BlogId y, por tanto, no necesita almacenarse en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="84ab0-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="84ab0-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84ab0-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    // Read the current value of an unmapped property
    var rssLink = context.Entry(blog).Property(p => p.RssLink).CurrentValue;

    // Use a string to specify the property name
    var rssLinkAgain = context.Entry(blog).Property("RssLink").CurrentValue;
}
```  

<span data-ttu-id="84ab0-127">También se puede establecer el valor actual si la propiedad expone un establecedor.</span><span class="sxs-lookup"><span data-stu-id="84ab0-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="84ab0-128">Lectura de los valores de las propiedades sin asignar es útil al realizar la validación de Entity Framework de las propiedades sin asignar.</span><span class="sxs-lookup"><span data-stu-id="84ab0-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="84ab0-129">Por la misma razón se pueden leer y establecer para propiedades de entidades que no contarán actualmente por el contexto de los valores actuales.</span><span class="sxs-lookup"><span data-stu-id="84ab0-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="84ab0-130">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84ab0-130">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Create an entity that is not being tracked
    var blog = new Blog { Name = "ADO.NET Blog" };

    // Read and set the current value of Name as before
    var currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";
    var currentName2 = context.Entry(blog).Property("Name").CurrentValue;
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="84ab0-131">Tenga en cuenta que los valores originales no están disponibles para las propiedades sin asignar o de las propiedades de entidades que no se realiza el seguimiento por el contexto.</span><span class="sxs-lookup"><span data-stu-id="84ab0-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="84ab0-132">Comprobar si una propiedad está marcada como modificada</span><span class="sxs-lookup"><span data-stu-id="84ab0-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="84ab0-133">El ejemplo siguiente muestra cómo comprobar si una propiedad individual se marca como modificado:</span><span class="sxs-lookup"><span data-stu-id="84ab0-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="84ab0-134">Los valores de las propiedades modificadas se envían como las actualizaciones a la base de datos cuando se llama a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="84ab0-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="84ab0-135">Marcar una propiedad como modificada</span><span class="sxs-lookup"><span data-stu-id="84ab0-135">Marking a property as modified</span></span>  

<span data-ttu-id="84ab0-136">El ejemplo siguiente muestra cómo forzar una propiedad individual esté marcado como modificado:</span><span class="sxs-lookup"><span data-stu-id="84ab0-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="84ab0-137">Al marcar una propiedad como modificada fuerza una actualización se puede enviar a la base de datos para la propiedad cuando se llama a SaveChanges incluso si el valor actual de la propiedad es igual que su valor original.</span><span class="sxs-lookup"><span data-stu-id="84ab0-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="84ab0-138">No es actualmente posible restablecer una propiedad individual que no se puede modificar después de que se ha marcado como modificado.</span><span class="sxs-lookup"><span data-stu-id="84ab0-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="84ab0-139">Esto es algo que tenemos previsto admitir en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="84ab0-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="84ab0-140">Lectura actuales, originales y los valores de la base de datos para todas las propiedades de una entidad</span><span class="sxs-lookup"><span data-stu-id="84ab0-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="84ab0-141">El ejemplo siguiente muestra cómo leer los valores actuales, los valores originales y los valores realmente en la base de datos para todas las propiedades asignadas de una entidad.</span><span class="sxs-lookup"><span data-stu-id="84ab0-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Make a modification to Name in the tracked entity
    blog.Name = "My Cool Blog";

    // Make a modification to Name in the database
    context.Database.SqlCommand("update dbo.Blogs set Name = 'My Boring Blog' where Id = 1");

    // Print out current, original, and database values
    Console.WriteLine("Current values:");
    PrintValues(context.Entry(blog).CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(context.Entry(blog).OriginalValues);

    Console.WriteLine("\nDatabase values:");
    PrintValues(context.Entry(blog).GetDatabaseValues());
}

public static void PrintValues(DbPropertyValues values)
{
    foreach (var propertyName in values.PropertyNames)
    {
        Console.WriteLine("Property {0} has value {1}",
                          propertyName, values[propertyName]);
    }
}
```  

<span data-ttu-id="84ab0-142">Los valores actuales son los valores que contienen las propiedades de la entidad.</span><span class="sxs-lookup"><span data-stu-id="84ab0-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="84ab0-143">Los valores originales son los valores que se leyeron desde la base de datos cuando la entidad se consultó.</span><span class="sxs-lookup"><span data-stu-id="84ab0-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="84ab0-144">Los valores de la base de datos son los valores que estén almacenadas actualmente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="84ab0-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="84ab0-145">Obtener los valores de la base de datos es útil cuando los valores de la base de datos pueden haber cambiado desde que se consultó la entidad, como cuando edita un simultáneas a la base de datos se ha realizado por otro usuario.</span><span class="sxs-lookup"><span data-stu-id="84ab0-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="84ab0-146">Establecer valores originales actuales de otro objeto</span><span class="sxs-lookup"><span data-stu-id="84ab0-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="84ab0-147">Los valores originales de una entidad con seguimiento actuales pueden actualizarse mediante la copia de los valores de otro objeto.</span><span class="sxs-lookup"><span data-stu-id="84ab0-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="84ab0-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84ab0-148">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var coolBlog = new Blog { Id = 1, Name = "My Cool Blog" };
    var boringBlog = new BlogDto { Id = 1, Name = "My Boring Blog" };

    // Change the current and original values by copying the values from other objects
    var entry = context.Entry(blog);
    entry.CurrentValues.SetValues(coolBlog);
    entry.OriginalValues.SetValues(boringBlog);

    // Print out current and original values
    Console.WriteLine("Current values:");
    PrintValues(entry.CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(entry.OriginalValues);
}

public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```  

<span data-ttu-id="84ab0-149">Ejecuta el código anterior imprimirá:</span><span class="sxs-lookup"><span data-stu-id="84ab0-149">Running the code above will print out:</span></span>  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="84ab0-150">Esta técnica a veces se utiliza al actualizar una entidad con los valores obtenidos de una llamada de servicio o un cliente en una aplicación de n niveles.</span><span class="sxs-lookup"><span data-stu-id="84ab0-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="84ab0-151">Tenga en cuenta que usa el objeto no tiene que ser del mismo tipo que la entidad siempre y cuando dicha clase tiene propiedades cuyos nombres coinciden con los de la entidad.</span><span class="sxs-lookup"><span data-stu-id="84ab0-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="84ab0-152">En el ejemplo anterior, se utiliza una instancia de BlogDTO para actualizar los valores originales.</span><span class="sxs-lookup"><span data-stu-id="84ab0-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="84ab0-153">Tenga en cuenta que solo las propiedades que se establecen en valores diferentes cuando se copian desde el otro objeto se marcará como modificada.</span><span class="sxs-lookup"><span data-stu-id="84ab0-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="84ab0-154">Valores de configuración originales o actuales de un diccionario</span><span class="sxs-lookup"><span data-stu-id="84ab0-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="84ab0-155">Los valores originales de una entidad con seguimiento actuales pueden actualizarse mediante la copia de los valores de un diccionario o alguna otra estructura de datos.</span><span class="sxs-lookup"><span data-stu-id="84ab0-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="84ab0-156">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84ab0-156">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "The New ADO.NET Blog" },
        { "Url", "blogs.msdn.com/adonet" },
    };

    var currentValues = context.Entry(blog).CurrentValues;

    foreach (var propertyName in newValues.Keys)
    {
        currentValues[propertyName] = newValues[propertyName];
    }

    PrintValues(currentValues);
}
```  

<span data-ttu-id="84ab0-157">Utilice la propiedad OriginalValues en lugar de la propiedad CurrentValues para establecer los valores originales.</span><span class="sxs-lookup"><span data-stu-id="84ab0-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="84ab0-158">Valores de configuración originales o actuales de un diccionario utilizando la propiedad</span><span class="sxs-lookup"><span data-stu-id="84ab0-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="84ab0-159">Una alternativa al uso CurrentValues o OriginalValues como se mostró anteriormente es usar el método de propiedad para establecer el valor de cada propiedad.</span><span class="sxs-lookup"><span data-stu-id="84ab0-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="84ab0-160">Esto puede ser preferible cuando se necesita establecer los valores de propiedades complejas.</span><span class="sxs-lookup"><span data-stu-id="84ab0-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="84ab0-161">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84ab0-161">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "John Doe" },
        { "Location.City", "Redmond" },
        { "Location.State.Name", "Washington" },
        { "Location.State.Code", "WA" },
    };

    var entry = context.Entry(user);

    foreach (var propertyName in newValues.Keys)
    {
        entry.Property(propertyName).CurrentValue = newValues[propertyName];
    }
}
```  

<span data-ttu-id="84ab0-162">En el ejemplo anterior propiedades complejas son accesibles mediante nombres con puntos.</span><span class="sxs-lookup"><span data-stu-id="84ab0-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="84ab0-163">Para que conocer otras formas de obtener acceso a propiedades complejas, vea las dos secciones más adelante en este tema en concreto sobre las propiedades complejas.</span><span class="sxs-lookup"><span data-stu-id="84ab0-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="84ab0-164">Creación de un objeto clonado que contiene los valores de base de datos, original o actual</span><span class="sxs-lookup"><span data-stu-id="84ab0-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="84ab0-165">Devuelve el objeto de DbPropertyValues de CurrentValues, OriginalValues, o GetDatabaseValues puede usarse para crear un clon de la entidad.</span><span class="sxs-lookup"><span data-stu-id="84ab0-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="84ab0-166">Este clon contendrá los valores de propiedad del objeto DbPropertyValues usado para crearlo.</span><span class="sxs-lookup"><span data-stu-id="84ab0-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="84ab0-167">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84ab0-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="84ab0-168">Tenga en cuenta que el objeto devuelto no es la entidad y no se realiza el seguimiento del contexto.</span><span class="sxs-lookup"><span data-stu-id="84ab0-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="84ab0-169">El objeto devuelto también no tiene ninguna relación establecido en otros objetos.</span><span class="sxs-lookup"><span data-stu-id="84ab0-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="84ab0-170">El objeto clonado puede ser útil para resolver problemas relacionados con actualizaciones simultáneas de la base de datos, especialmente cuando se está usando una interfaz de usuario que implica el enlace de datos a los objetos de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="84ab0-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="84ab0-171">Obtener y establecer los valores originales y actuales de propiedades complejas</span><span class="sxs-lookup"><span data-stu-id="84ab0-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="84ab0-172">El valor de todo el objeto complejo se pueden leer y definir mediante el método de la propiedad tal y como puede ser para una propiedad primitiva.</span><span class="sxs-lookup"><span data-stu-id="84ab0-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="84ab0-173">Además puede profundizar en el objeto complejo y leer o establecer propiedades de ese objeto, o incluso un objeto anidado.</span><span class="sxs-lookup"><span data-stu-id="84ab0-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="84ab0-174">A continuación se muestran algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="84ab0-174">Here are some examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    // Get the Location complex object
    var location = context.Entry(user)
                       .Property(u => u.Location)
                       .CurrentValue;

    // Get the nested State complex object using chained calls
    var state1 = context.Entry(user)
                     .ComplexProperty(u => u.Location)
                     .Property(l => l.State)
                     .CurrentValue;

    // Get the nested State complex object using a single lambda expression
    var state2 = context.Entry(user)
                     .Property(u => u.Location.State)
                     .CurrentValue;

    // Get the nested State complex object using a dotted string
    var state3 = context.Entry(user)
                     .Property("Location.State")
                     .CurrentValue;

    // Get the value of the Name property on the nested State complex object using chained calls
    var name1 = context.Entry(user)
                       .ComplexProperty(u => u.Location)
                       .ComplexProperty(l => l.State)
                       .Property(s => s.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a single lambda expression
    var name2 = context.Entry(user)
                       .Property(u => u.Location.State.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a dotted string
    var name3 = context.Entry(user)
                       .Property("Location.State.Name")
                       .CurrentValue;
}
```  

<span data-ttu-id="84ab0-175">Utilice la propiedad OriginalValue en lugar de la propiedad CurrentValue a obtener o establecer un valor original.</span><span class="sxs-lookup"><span data-stu-id="84ab0-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="84ab0-176">Tenga en cuenta que la propiedad o el método ComplexProperty puede utilizarse para tener acceso a una propiedad compleja.</span><span class="sxs-lookup"><span data-stu-id="84ab0-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="84ab0-177">Sin embargo, el método ComplexProperty debe usarse si desea profundizar en el objeto complejo con propiedades adicionales o llama ComplexProperty.</span><span class="sxs-lookup"><span data-stu-id="84ab0-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="84ab0-178">Uso de DbPropertyValues para tener acceso a propiedades complejas</span><span class="sxs-lookup"><span data-stu-id="84ab0-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="84ab0-179">Cuando use CurrentValues, OriginalValues o GetDatabaseValues obtener original actual, o valores de la base de datos de una entidad, los valores de propiedades complejas se devuelven como objetos DbPropertyValues anidados.</span><span class="sxs-lookup"><span data-stu-id="84ab0-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="84ab0-180">Estos anidar objetos puede, a continuación, usarse para obtener los valores del objeto complejo.</span><span class="sxs-lookup"><span data-stu-id="84ab0-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="84ab0-181">Por ejemplo, el siguiente método imprimirá los valores de todas las propiedades, incluidos los valores de las propiedades complejas y las propiedades complejas anidadas.</span><span class="sxs-lookup"><span data-stu-id="84ab0-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

``` csharp
public static void WritePropertyValues(string parentPropertyName, DbPropertyValues propertyValues)
{
    foreach (var propertyName in propertyValues.PropertyNames)
    {
        var nestedValues = propertyValues[propertyName] as DbPropertyValues;
        if (nestedValues != null)
        {
            WritePropertyValues(parentPropertyName + propertyName + ".", nestedValues);
        }
        else
        {
            Console.WriteLine("Property {0}{1} has value {2}",
                              parentPropertyName, propertyName,
                              propertyValues[propertyName]);
        }
    }
}
```  

<span data-ttu-id="84ab0-182">Para imprimir todos los valores de propiedad actuales del método se llamaría similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="84ab0-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
