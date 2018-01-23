---
title: Carga los datos - Core EF relacionados
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: ec69bb128890a1e0b72fe77014f37747585bb5a5
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="7a8e2-102">Cargar datos relacionados</span><span class="sxs-lookup"><span data-stu-id="7a8e2-102">Loading Related Data</span></span>

<span data-ttu-id="7a8e2-103">Entity Framework Core le permite usar las propiedades de navegación en el modelo para cargar las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="7a8e2-104">Hay tres patrones O/RM comunes usados para cargar los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="7a8e2-105">**Carga diligente** significa que los datos relacionados se cargan desde la base de datos como parte de la consulta inicial.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="7a8e2-106">**Carga explícita** significa que los datos relacionados se cargan explícitamente desde la base de datos en un momento posterior.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="7a8e2-107">**Carga diferida** significa que los datos relacionados se cargan transparente desde la base de datos cuando se tiene acceso a la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="7a8e2-108">Carga diferida aún no es posible con EF principales.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="7a8e2-109">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="7a8e2-110">Carga diligente</span><span class="sxs-lookup"><span data-stu-id="7a8e2-110">Eager loading</span></span>

<span data-ttu-id="7a8e2-111">Puede usar el `Include` método para especificar los datos relacionados que se incluirá en los resultados de la consulta.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="7a8e2-112">En el ejemplo siguiente, tendrá los blogs que se devuelven en los resultados de sus `Posts` propiedad que se rellene con las entradas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="7a8e2-113">Propiedades de navegación automáticamente revisión de seguridad de entidad Framework Core will a cualquier otra entidad que se cargaron previamente en la instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="7a8e2-114">Por lo que incluso si no se incluye explícitamente los datos de una propiedad de navegación, la propiedad también puede rellenar si algunas o todas las entidades relacionadas se cargaron previamente.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="7a8e2-115">Puede incluir datos relacionados de varias relaciones en una sola consulta.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="7a8e2-116">Incluidos varios niveles</span><span class="sxs-lookup"><span data-stu-id="7a8e2-116">Including multiple levels</span></span>

<span data-ttu-id="7a8e2-117">Puede explorar en profundidad a través de relaciones para incluir varios niveles de datos relacionados con el `ThenInclude` método.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="7a8e2-118">En el ejemplo siguiente se cargan todos los blogs, sus entradas relacionadas y el autor de cada publicación.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="7a8e2-119">Las versiones actuales de Visual Studio ofrecen opciones de finalización de código incorrecto y puede causar expresiones correctas se marque con errores de sintaxis cuando se usa el `ThenInclude` método después de una propiedad de navegación de la colección.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-119">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="7a8e2-120">Esto es un síntoma de un error de IntelliSense que realiza el seguimiento en https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-120">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="7a8e2-121">Es seguro omitir estos errores de sintaxis falsos siempre que el código es correcto y puede compilarse correctamente.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-121">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="7a8e2-122">Se pueden encadenar varias llamadas a `ThenInclude` para continuar incluso más niveles de los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-122">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="7a8e2-123">Puede combinar todo esto debe incluir datos relacionados de varios niveles y varias raíces en la misma consulta.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-123">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="7a8e2-124">Puede incluir varias entidades relacionadas para una de las entidades que se incluye.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-124">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="7a8e2-125">Por ejemplo, al consultar `Blog`s, incluye `Posts` y, a continuación, se van a incluir tanto el `Author` y `Tags` de la `Posts`.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-125">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="7a8e2-126">Para ello, debe especificar cada uno de ellos incluye empezando por la raíz de la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-126">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="7a8e2-127">Por ejemplo, `Blog -> Posts -> Author` y `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-127">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="7a8e2-128">Esto no significa que obtendrá combinaciones redundantes, en la mayoría de los casos que se consolidará EF las combinaciones cuando la generación de SQL.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-128">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="7a8e2-129">Omite incluye</span><span class="sxs-lookup"><span data-stu-id="7a8e2-129">Ignored includes</span></span>

<span data-ttu-id="7a8e2-130">Si cambia la consulta para que ya no devuelve instancias del tipo de entidad que la consulta comienza con, se omiten los operadores de inclusión.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-130">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="7a8e2-131">En el ejemplo siguiente, los operadores de inclusión se basan en el `Blog`, pero, a continuación, el `Select` operador se utiliza para cambiar la consulta para devolver un tipo anónimo.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-131">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="7a8e2-132">En este caso, los operadores de inclusión no tienen ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-132">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="7a8e2-133">De forma predeterminada, el núcleo de EF registrará una advertencia cuando se incluyen operadores se omiten.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-133">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="7a8e2-134">Vea [registro](../miscellaneous/logging.md) para obtener más información acerca de cómo ver los resultados del registro.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-134">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="7a8e2-135">Puede cambiar el comportamiento cuando un operador de inclusión se omite para producir o no hacer nada.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-135">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="7a8e2-136">Esto se realiza al configurar las opciones para el contexto - normalmente en `DbContext.OnConfiguring`, o bien en `Startup.cs` si utiliza ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-136">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="7a8e2-137">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="7a8e2-137">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="7a8e2-138">Esta característica se introdujo en EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-138">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="7a8e2-139">Puede cargar explícitamente una propiedad de navegación a través de la `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-139">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="7a8e2-140">Puede cargar explícitamente una propiedad de navegación mediante la ejecución de una consulta independiente que devuelve las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-140">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="7a8e2-141">Si está habilitado el seguimiento de cambios, al cargar una entidad, Core EF automáticamente establecer las propiedades de navegación de la entitiy recientemente cargados para hacer referencia a una entidad que ya están cargada y establecer las propiedades de navegación de las entidades ya cargado para hacer referencia a la entidad recién cargado.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-141">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="7a8e2-142">Consultar las entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="7a8e2-142">Querying related entities</span></span>

<span data-ttu-id="7a8e2-143">También puede obtener una consulta LINQ que representa el contenido de una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-143">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="7a8e2-144">Esto le permite hacer cosas como la ejecución de un operador agregado sobre las entidades relacionadas sin cargarlos en la memoria.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-144">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="7a8e2-145">También puede filtrar las entidades relacionadas se cargan en memoria.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-145">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="7a8e2-146">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="7a8e2-146">Lazy loading</span></span>

<span data-ttu-id="7a8e2-147">Carga diferida no es compatible todavía con EF Core.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-147">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="7a8e2-148">Puede ver el [elemento de la carga diferida en nuestro trabajo pendiente](https://github.com/aspnet/EntityFramework/issues/3797) para realizar el seguimiento de esta característica.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-148">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="7a8e2-149">Serialización y los datos relacionados</span><span class="sxs-lookup"><span data-stu-id="7a8e2-149">Related data and serialization</span></span>

<span data-ttu-id="7a8e2-150">Porque las propiedades de navegación de corrección telefónico automáticamente de will de EF Core, puede acabar con ciclos en el gráfico de objetos.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-150">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="7a8e2-151">Por ejemplo, cargando un blog y está relacionado con entradas dará como resultado un objeto de blog que hace referencia a una colección de entradas.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-151">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="7a8e2-152">Cada una de esas entradas tendrá una referencia al blog.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-152">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="7a8e2-153">Algunos marcos de serialización no permiten estos ciclos.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-153">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="7a8e2-154">Por ejemplo, Json.NET producirá la excepción siguiente si un ciclo es generará.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-154">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="7a8e2-155">Newtonsoft.Json.JsonSerializationException: Self que hacen referencia a bucle detectado para la propiedad 'Blog' con el tipo 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-155">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="7a8e2-156">Si utiliza ASP.NET Core, puede configurar Json.NET para omitir los ciclos que encuentran en el gráfico de objetos.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-156">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="7a8e2-157">Esto se hace en el `ConfigureServices(...)` método `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="7a8e2-157">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
