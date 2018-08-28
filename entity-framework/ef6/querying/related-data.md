---
title: Carga relacionados entidades - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 091493f60c732573ca63adb8c65bc28606453d34
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995930"
---
# <a name="loading-related-entities"></a><span data-ttu-id="40d37-102">Cargar entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="40d37-102">Loading Related Entities</span></span>
<span data-ttu-id="40d37-103">Entity Framework admite tres formas de cargar datos relacionados: carga diligente, la carga diferida y la carga explícita.</span><span class="sxs-lookup"><span data-stu-id="40d37-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="40d37-104">Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.</span><span class="sxs-lookup"><span data-stu-id="40d37-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="40d37-105">Cargar concienzudamente</span><span class="sxs-lookup"><span data-stu-id="40d37-105">Eagerly Loading</span></span>  

<span data-ttu-id="40d37-106">La carga diligente es el proceso mediante el cual una consulta para un tipo de entidad también carga las entidades relacionadas como parte de la consulta.</span><span class="sxs-lookup"><span data-stu-id="40d37-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="40d37-107">La carga diligente se logra mediante el uso del método Include.</span><span class="sxs-lookup"><span data-stu-id="40d37-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="40d37-108">Por ejemplo, las consultas siguientes cargará blogs y todas las entradas relacionadas con cada blog.</span><span class="sxs-lookup"><span data-stu-id="40d37-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                          .Include(b => b.Posts)
                          .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include(b => b.Posts)
                        .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                          .Include("Posts")
                          .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include("Posts")
                        .FirstOrDefault();
}
```  

<span data-ttu-id="40d37-109">Tenga en cuenta que incluir un método de extensión en el espacio de nombres System.Data.Entity así que asegúrese de que está utilizando ese espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="40d37-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="40d37-110">Cargar concienzudamente varios niveles</span><span class="sxs-lookup"><span data-stu-id="40d37-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="40d37-111">También es posible cargar concienzudamente varios niveles de entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="40d37-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="40d37-112">Las consultas siguientes muestran ejemplos de cómo hacer esto para la recopilación y las propiedades de navegación de referencia.</span><span class="sxs-lookup"><span data-stu-id="40d37-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

<span data-ttu-id="40d37-113">Tenga en cuenta que no es actualmente posible filtrar qué entidades relacionadas se cargan.</span><span class="sxs-lookup"><span data-stu-id="40d37-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="40d37-114">Incluir will siempre aportar en todos ellos relacionados con entidades.</span><span class="sxs-lookup"><span data-stu-id="40d37-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="40d37-115">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="40d37-115">Lazy Loading</span></span>  

<span data-ttu-id="40d37-116">La carga diferida es el proceso mediante el cual una entidad o colección de entidades se carga automáticamente desde la base de datos la primera vez que se accede a una propiedad que hace referencia a la entidad o entidades.</span><span class="sxs-lookup"><span data-stu-id="40d37-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="40d37-117">Al utilizar tipos de entidad POCO, la carga diferida se logra mediante la creación de instancias de tipos proxy derivado y, a continuación, reemplazar propiedades virtuales para agregar el enlace de la carga.</span><span class="sxs-lookup"><span data-stu-id="40d37-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="40d37-118">Por ejemplo, cuando se usa la clase de entidad Blog definida a continuación, las entradas relacionadas se cargará la primera vez que se tiene acceso a la propiedad de navegación de publicaciones:</span><span class="sxs-lookup"><span data-stu-id="40d37-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public virtual ICollection<Post> Posts { get; set; }  
}
```  

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="40d37-119">Activar la carga desactivado para la serialización diferida</span><span class="sxs-lookup"><span data-stu-id="40d37-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="40d37-120">Serialización y la carga diferida no mezclan bien y, si no tiene cuidado puede acabar consultar toda la base de datos simplemente porque está habilitada la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="40d37-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="40d37-121">La mayoría de los serializadores funcionan mediante el acceso a cada propiedad en una instancia de un tipo.</span><span class="sxs-lookup"><span data-stu-id="40d37-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="40d37-122">Acceso a la propiedad desencadena la carga diferida, por lo que se serializan más entidades.</span><span class="sxs-lookup"><span data-stu-id="40d37-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="40d37-123">En esas entidades se tiene acceso a las propiedades y se cargan entidades aún más.</span><span class="sxs-lookup"><span data-stu-id="40d37-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="40d37-124">Es una buena práctica para activar carga desactivado antes de serializar una entidad diferida.</span><span class="sxs-lookup"><span data-stu-id="40d37-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="40d37-125">Las secciones siguientes muestran cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="40d37-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="40d37-126">Al desactivar la carga diferida para las propiedades de navegación específicos</span><span class="sxs-lookup"><span data-stu-id="40d37-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="40d37-127">Se puede desactivar la carga diferida de la colección de entradas mediante la realización de la propiedad de publicaciones no virtual:</span><span class="sxs-lookup"><span data-stu-id="40d37-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public ICollection<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="40d37-128">La carga de las entradas de colección todavía se puede lograr mediante la carga diligente (consulte *cargar concienzudamente* anteriormente) o el método Load (consulte *cargar explícitamente* a continuación).</span><span class="sxs-lookup"><span data-stu-id="40d37-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="40d37-129">Desactivar la carga diferida para todas las entidades</span><span class="sxs-lookup"><span data-stu-id="40d37-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="40d37-130">La carga diferida se puede desactivar para todas las entidades en el contexto estableciendo un indicador en la propiedad de configuración.</span><span class="sxs-lookup"><span data-stu-id="40d37-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="40d37-131">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="40d37-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="40d37-132">Todavía se puede lograr la carga de las entidades relacionadas mediante la carga diligente (consulte *cargar concienzudamente* anteriormente) o el método Load (consulte *cargar explícitamente* a continuación).</span><span class="sxs-lookup"><span data-stu-id="40d37-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="40d37-133">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="40d37-133">Explicitly Loading</span></span>  

<span data-ttu-id="40d37-134">Incluso con la carga diferida deshabilitada es posible cargar entidades relacionadas de forma diferida, pero debe hacerse con una llamada explícita.</span><span class="sxs-lookup"><span data-stu-id="40d37-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="40d37-135">Para ello, utilice el método Load en la entrada de la entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="40d37-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="40d37-136">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="40d37-136">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

<span data-ttu-id="40d37-137">Tenga en cuenta que el método de referencia debe usarse cuando una entidad tiene una propiedad de navegación a otra entidad única.</span><span class="sxs-lookup"><span data-stu-id="40d37-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="40d37-138">Por otro lado, el método de colección debe usarse cuando una entidad tiene una propiedad de navegación a una colección de otras entidades.</span><span class="sxs-lookup"><span data-stu-id="40d37-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="40d37-139">Aplicar filtros al cargar explícitamente entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="40d37-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="40d37-140">El método de consulta proporciona acceso a la consulta subyacente que se va a usar Entity Framework al cargar entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="40d37-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="40d37-141">A continuación, puede usar LINQ para aplicar filtros a la consulta antes de ejecutarlo con una llamada a un método de extensión LINQ como ToList, carga, etcetera. El método de consulta se puede usar con las propiedades de navegación de referencia y de colección, pero es muy útil para las colecciones donde se puede usar para cargar solo una parte de la colección.</span><span class="sxs-lookup"><span data-stu-id="40d37-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="40d37-142">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="40d37-142">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

<span data-ttu-id="40d37-143">Cuando se usa el método de consulta normalmente es mejor desactivar la carga diferida para la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="40d37-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="40d37-144">Esto es porque en caso contrario, toda la colección es posible que se cargan automáticamente mediante el mecanismo de carga diferida antes o después de haber ejecutado la consulta filtrada.</span><span class="sxs-lookup"><span data-stu-id="40d37-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="40d37-145">Tenga en cuenta que mientras que la relación se puede especificar como una cadena en lugar de una expresión lambda, IQueryable devuelta no es genérico cuando se usa una cadena, por lo que el método de conversión de tipos suele ser necesario antes de nada útil se puede hacer con él.</span><span class="sxs-lookup"><span data-stu-id="40d37-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="40d37-146">Uso de consultas para contar las entidades relacionadas sin cargarlos</span><span class="sxs-lookup"><span data-stu-id="40d37-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="40d37-147">A veces resulta útil saber el número de entidades relacionadas con otra entidad en la base de datos sin incurrir en realidad en el costo de la carga de todas las entidades.</span><span class="sxs-lookup"><span data-stu-id="40d37-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="40d37-148">El método de consulta con el método de recuento de LINQ puede usarse para hacer esto.</span><span class="sxs-lookup"><span data-stu-id="40d37-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="40d37-149">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="40d37-149">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                          .Collection(b => b.Posts)
                          .Query()
                          .Count();
}
```  
