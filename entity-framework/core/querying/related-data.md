---
title: Carga los datos - Core EF relacionados
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 0d7705e0e5368435536e98d319c853ea8c732643
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="e30aa-102">Cargar datos relacionados</span><span class="sxs-lookup"><span data-stu-id="e30aa-102">Loading Related Data</span></span>

<span data-ttu-id="e30aa-103">Entity Framework Core le permite usar las propiedades de navegación en el modelo para cargar las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="e30aa-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="e30aa-104">Hay tres patrones O/RM comunes usados para cargar los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="e30aa-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="e30aa-105">**Carga diligente** significa que los datos relacionados se cargan desde la base de datos como parte de la consulta inicial.</span><span class="sxs-lookup"><span data-stu-id="e30aa-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="e30aa-106">**Carga explícita** significa que los datos relacionados se carguen explícitamente desde la base de datos en un momento posterior.</span><span class="sxs-lookup"><span data-stu-id="e30aa-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="e30aa-107">**La carga diferida** significa que los datos relacionados se cargan transparente desde la base de datos cuando se tiene acceso a la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="e30aa-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="e30aa-108">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="e30aa-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="e30aa-109">Carga diligente</span><span class="sxs-lookup"><span data-stu-id="e30aa-109">Eager loading</span></span>

<span data-ttu-id="e30aa-110">Puede usar el `Include` método para especificar los datos relacionados que se incluirá en los resultados de la consulta.</span><span class="sxs-lookup"><span data-stu-id="e30aa-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="e30aa-111">En el ejemplo siguiente, tendrá los blogs que se devuelven en los resultados de sus `Posts` propiedad que se rellene con las entradas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="e30aa-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="e30aa-112">Propiedades de navegación automáticamente revisión de seguridad de entidad Framework Core will a cualquier otra entidad que se cargaron previamente en la instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="e30aa-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="e30aa-113">Por lo que incluso si no se incluye explícitamente los datos de una propiedad de navegación, la propiedad también puede rellenar si algunas o todas las entidades relacionadas se cargaron previamente.</span><span class="sxs-lookup"><span data-stu-id="e30aa-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="e30aa-114">Puede incluir datos relacionados de varias relaciones en una sola consulta.</span><span class="sxs-lookup"><span data-stu-id="e30aa-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="e30aa-115">Incluidos varios niveles</span><span class="sxs-lookup"><span data-stu-id="e30aa-115">Including multiple levels</span></span>

<span data-ttu-id="e30aa-116">Puede explorar en profundidad a través de relaciones para incluir varios niveles de datos relacionados con el `ThenInclude` método.</span><span class="sxs-lookup"><span data-stu-id="e30aa-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="e30aa-117">En el ejemplo siguiente se cargan todos los blogs, sus entradas relacionadas y el autor de cada publicación.</span><span class="sxs-lookup"><span data-stu-id="e30aa-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="e30aa-118">Las versiones actuales de Visual Studio ofrecen opciones de finalización de código incorrecto y puede causar expresiones correctas se marque con errores de sintaxis cuando se usa el `ThenInclude` método después de una propiedad de navegación de la colección.</span><span class="sxs-lookup"><span data-stu-id="e30aa-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="e30aa-119">Esto es un síntoma de un error de IntelliSense que realiza el seguimiento en https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="e30aa-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="e30aa-120">Es seguro omitir estos errores de sintaxis falsos siempre que el código es correcto y puede compilarse correctamente.</span><span class="sxs-lookup"><span data-stu-id="e30aa-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="e30aa-121">Se pueden encadenar varias llamadas a `ThenInclude` para continuar incluso más niveles de los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="e30aa-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="e30aa-122">Puede combinar todo esto debe incluir datos relacionados de varios niveles y varias raíces en la misma consulta.</span><span class="sxs-lookup"><span data-stu-id="e30aa-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="e30aa-123">Puede incluir varias entidades relacionadas para una de las entidades que se incluye.</span><span class="sxs-lookup"><span data-stu-id="e30aa-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="e30aa-124">Por ejemplo, al consultar `Blog`s, incluye `Posts` y, a continuación, se van a incluir tanto el `Author` y `Tags` de la `Posts`.</span><span class="sxs-lookup"><span data-stu-id="e30aa-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="e30aa-125">Para ello, debe especificar cada uno de ellos incluye empezando por la raíz de la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="e30aa-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="e30aa-126">Por ejemplo, `Blog -> Posts -> Author` y `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="e30aa-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="e30aa-127">Esto no significa que obtendrá combinaciones redundantes, en la mayoría de los casos que se consolidará EF las combinaciones cuando la generación de SQL.</span><span class="sxs-lookup"><span data-stu-id="e30aa-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="e30aa-128">Incluir en tipos derivados</span><span class="sxs-lookup"><span data-stu-id="e30aa-128">Include on derived types</span></span>

<span data-ttu-id="e30aa-129">Puede incluir datos relacionados de navegaciones solo definidas en un tipo derivado mediante `Include` y `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="e30aa-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="e30aa-130">Dado el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e30aa-130">Given the following model:</span></span>

```Csharp
    public class SchoolContext : DbContext
    {
        public DbSet<Person> People { get; set; }
        public DbSet<School> Schools { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
        }
    }

    public class Person
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Student : Person
    {
        public School School { get; set; }
    }

    public class School
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public List<Student> Students { get; set; }
    }
```

<span data-ttu-id="e30aa-131">Contenido de `School` panel de navegación de todas las personas que son los estudiantes puede se carguen usando una serie de patrones:</span><span class="sxs-lookup"><span data-stu-id="e30aa-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="e30aa-132">utilizar cast</span><span class="sxs-lookup"><span data-stu-id="e30aa-132">using cast</span></span>
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- <span data-ttu-id="e30aa-133">con `as` (operador)</span><span class="sxs-lookup"><span data-stu-id="e30aa-133">using `as` operator</span></span>
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- <span data-ttu-id="e30aa-134">mediante la sobrecarga de `Include` que toma el parámetro de tipo `string`</span><span class="sxs-lookup"><span data-stu-id="e30aa-134">using overload of `Include` that takes parameter of type `string`</span></span>
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a><span data-ttu-id="e30aa-135">Omite incluye</span><span class="sxs-lookup"><span data-stu-id="e30aa-135">Ignored includes</span></span>

<span data-ttu-id="e30aa-136">Si cambia la consulta para que ya no devuelve instancias del tipo de entidad que la consulta comienza con, se omiten los operadores de inclusión.</span><span class="sxs-lookup"><span data-stu-id="e30aa-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="e30aa-137">En el ejemplo siguiente, los operadores de inclusión se basan en el `Blog`, pero, a continuación, el `Select` operador se utiliza para cambiar la consulta para devolver un tipo anónimo.</span><span class="sxs-lookup"><span data-stu-id="e30aa-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="e30aa-138">En este caso, los operadores de inclusión no tienen ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="e30aa-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="e30aa-139">De forma predeterminada, el núcleo de EF registrará una advertencia cuando se incluyen operadores se omiten.</span><span class="sxs-lookup"><span data-stu-id="e30aa-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="e30aa-140">Vea [registro](../miscellaneous/logging.md) para obtener más información acerca de cómo ver los resultados del registro.</span><span class="sxs-lookup"><span data-stu-id="e30aa-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="e30aa-141">Puede cambiar el comportamiento cuando un operador de inclusión se omite para producir o no hacer nada.</span><span class="sxs-lookup"><span data-stu-id="e30aa-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="e30aa-142">Esto se realiza al configurar las opciones para el contexto - normalmente en `DbContext.OnConfiguring`, o bien en `Startup.cs` si utiliza ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e30aa-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="e30aa-143">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="e30aa-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="e30aa-144">Esta característica se introdujo en EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="e30aa-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="e30aa-145">Puede cargar explícitamente una propiedad de navegación a través de la `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="e30aa-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="e30aa-146">Puede cargar explícitamente una propiedad de navegación mediante la ejecución de una consulta independiente que devuelve las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="e30aa-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="e30aa-147">Si está habilitado el seguimiento de cambios, al cargar una entidad, Core EF automáticamente establecer las propiedades de navegación de la entitiy recientemente cargados para hacer referencia a una entidad que ya están cargada y establecer las propiedades de navegación de las entidades ya cargado para hacer referencia a la entidad recién cargado.</span><span class="sxs-lookup"><span data-stu-id="e30aa-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="e30aa-148">Consultar las entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="e30aa-148">Querying related entities</span></span>

<span data-ttu-id="e30aa-149">También puede obtener una consulta LINQ que representa el contenido de una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="e30aa-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="e30aa-150">Esto le permite hacer cosas como la ejecución de un operador agregado sobre las entidades relacionadas sin cargarlos en la memoria.</span><span class="sxs-lookup"><span data-stu-id="e30aa-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="e30aa-151">También puede filtrar las entidades relacionadas se cargan en memoria.</span><span class="sxs-lookup"><span data-stu-id="e30aa-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="e30aa-152">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="e30aa-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="e30aa-153">Esta característica se introdujo en el EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="e30aa-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="e30aa-154">La manera más sencilla de usar la carga diferida es instalar el [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paquete y habilitar con una llamada a `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="e30aa-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="e30aa-155">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e30aa-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="e30aa-156">O bien, cuando se utiliza AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="e30aa-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="e30aa-157">EF núcleo permitirán carga-diferida para cualquier propiedad de navegación que se puede reemplazar--que es, debe ser `virtual` y en una clase que se puede heredar de.</span><span class="sxs-lookup"><span data-stu-id="e30aa-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="e30aa-158">Por ejemplo, en las siguientes entidades, la `Post.Blog` y `Blog.Posts` las propiedades de navegación será la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="e30aa-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```Csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="e30aa-159">Carga Lazy sin servidores proxy</span><span class="sxs-lookup"><span data-stu-id="e30aa-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="e30aa-160">Proxies de carga Lazy funcionan mediante la inyección de la `ILazyLoader` de servicio en una entidad, como se describe en [constructores del tipo de entidad](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="e30aa-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="e30aa-161">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e30aa-161">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="e30aa-162">Esto no requiere tipos de entidad se herede de o propiedades de navegación sea virtual y permite instancias de entidad que se creó con `new` a la carga diferida una vez conectado a un contexto.</span><span class="sxs-lookup"><span data-stu-id="e30aa-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="e30aa-163">Sin embargo, requiere una referencia a la `ILazyLoader` servicio, que se acopla con los tipos de entidad del ensamblado principal de EF.</span><span class="sxs-lookup"><span data-stu-id="e30aa-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="e30aa-164">Para evitar este núcleo EF permite el `ILazyLoader.Load` método se insertaran como delegado.</span><span class="sxs-lookup"><span data-stu-id="e30aa-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="e30aa-165">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e30aa-165">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="e30aa-166">El código anterior utiliza un `Load` método de extensión para hacer utilizando un poco limpiador el delegado:</span><span class="sxs-lookup"><span data-stu-id="e30aa-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```Csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> <span data-ttu-id="e30aa-167">El parámetro del constructor del delegado de la carga diferida debe llamarse "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="e30aa-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="e30aa-168">Configuración para utilizar un nombre diferente que se ha planificado para una futura versión.</span><span class="sxs-lookup"><span data-stu-id="e30aa-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="e30aa-169">Serialización y los datos relacionados</span><span class="sxs-lookup"><span data-stu-id="e30aa-169">Related data and serialization</span></span>

<span data-ttu-id="e30aa-170">Porque las propiedades de navegación de corrección telefónico automáticamente de will de EF Core, puede acabar con ciclos en el gráfico de objetos.</span><span class="sxs-lookup"><span data-stu-id="e30aa-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="e30aa-171">Por ejemplo, cargando un blog y está relacionado con entradas dará como resultado un objeto de blog que hace referencia a una colección de entradas.</span><span class="sxs-lookup"><span data-stu-id="e30aa-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="e30aa-172">Cada una de esas entradas tendrá una referencia al blog.</span><span class="sxs-lookup"><span data-stu-id="e30aa-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="e30aa-173">Algunos marcos de serialización no permiten estos ciclos.</span><span class="sxs-lookup"><span data-stu-id="e30aa-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="e30aa-174">Por ejemplo, Json.NET se producirá la siguiente excepción si se produce un ciclo.</span><span class="sxs-lookup"><span data-stu-id="e30aa-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="e30aa-175">Newtonsoft.Json.JsonSerializationException: Self que hacen referencia a bucle detectado para la propiedad 'Blog' con el tipo 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="e30aa-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="e30aa-176">Si utiliza ASP.NET Core, puede configurar Json.NET para omitir los ciclos que encuentran en el gráfico de objetos.</span><span class="sxs-lookup"><span data-stu-id="e30aa-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="e30aa-177">Esto se hace en el `ConfigureServices(...)` método `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="e30aa-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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
