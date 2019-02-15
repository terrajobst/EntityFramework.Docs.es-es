---
title: 'Carga de datos relacionados: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 4e042acb805c743ee794f4e61105b8d2136973b1
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668731"
---
# <a name="loading-related-data"></a><span data-ttu-id="888d8-102">Carga de datos relacionados</span><span class="sxs-lookup"><span data-stu-id="888d8-102">Loading Related Data</span></span>

<span data-ttu-id="888d8-103">Entity Framework Core permite usar las propiedades de navegación del modelo para cargar las entidades relacionados.</span><span class="sxs-lookup"><span data-stu-id="888d8-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="888d8-104">Existen tres patrones de O/RM comunes que se usan para cargar los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="888d8-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="888d8-105">**Carga diligente** significa que los datos relacionados se cargan desde la base de datos como parte de la consulta inicial.</span><span class="sxs-lookup"><span data-stu-id="888d8-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="888d8-106">**Carga explícita** significa que los datos relacionados se cargan de manera explícita desde la base de datos más adelanto.</span><span class="sxs-lookup"><span data-stu-id="888d8-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="888d8-107">**Carga diferida** significa que los datos relacionados se cargan de manera transparente desde la base de datos cuando se accede a la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="888d8-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="888d8-108">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="888d8-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="888d8-109">Carga diligente</span><span class="sxs-lookup"><span data-stu-id="888d8-109">Eager loading</span></span>

<span data-ttu-id="888d8-110">Puede usar el método `Include` para especificar los datos relacionados que se incluirán en los resultados de la consulta.</span><span class="sxs-lookup"><span data-stu-id="888d8-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="888d8-111">En el ejemplo siguiente, las entradas relacionadas rellenarán la propiedad `Posts` de los blogs que se devuelvan en los resultados.</span><span class="sxs-lookup"><span data-stu-id="888d8-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="888d8-112">Entity Framework Core corregirá automáticamente las propiedades de navegación para todas las entidades que se cargaron previamente en la instancia del contexto.</span><span class="sxs-lookup"><span data-stu-id="888d8-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="888d8-113">Por tanto, incluso si los datos de una propiedad de navegación no se incluyen explícitamente, es posible que la propiedad se siga rellenando si algunas o todas las entidades relacionadas se cargaron previamente.</span><span class="sxs-lookup"><span data-stu-id="888d8-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="888d8-114">Puede incluir los datos relacionados de varias relaciones en una sola consulta.</span><span class="sxs-lookup"><span data-stu-id="888d8-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="888d8-115">Inclusión de varios niveles</span><span class="sxs-lookup"><span data-stu-id="888d8-115">Including multiple levels</span></span>

<span data-ttu-id="888d8-116">Puede explorar en profundidad las relaciones para incluir varios niveles de datos relacionados con el método `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="888d8-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="888d8-117">En el ejemplo siguiente se cargan todos los blogs, las entradas relacionadas y el creador de cada entrada.</span><span class="sxs-lookup"><span data-stu-id="888d8-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="888d8-118">Las versiones actuales de Visual Studio ofrecen opciones para completar código incorrecto y pueden provocar que las expresiones correctas se marquen como errores de sintaxis cuando se use el método `ThenInclude` después de una propiedad de navegación de colección.</span><span class="sxs-lookup"><span data-stu-id="888d8-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="888d8-119">Este es un síntoma de un error de IntelliSense al que se hace seguimiento en https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="888d8-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="888d8-120">Resulta seguro ignorar estos errores de sintaxis falsos siempre que el código sea correcto y se pueda compilar de manera adecuada.</span><span class="sxs-lookup"><span data-stu-id="888d8-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="888d8-121">Puede encadenar varias llamadas en `ThenInclude` para continuar incluyendo más niveles de datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="888d8-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="888d8-122">Puede combinar todo esto para incluir datos relacionados provenientes de varios niveles y varias raíces en la misma consulta.</span><span class="sxs-lookup"><span data-stu-id="888d8-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="888d8-123">Es posible que quiera incluir varias entidades relacionadas para una de las entidades que se está incluyendo.</span><span class="sxs-lookup"><span data-stu-id="888d8-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="888d8-124">Por ejemplo, cuando consulte `Blog`, incluye `Posts` y luego quiere incluir tanto `Author` como `Tags` de las `Posts`.</span><span class="sxs-lookup"><span data-stu-id="888d8-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="888d8-125">Para hacerlo, debe especificar cada inicio de ruta de acceso de inclusión en la raíz.</span><span class="sxs-lookup"><span data-stu-id="888d8-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="888d8-126">Por ejemplo, `Blog -> Posts -> Author` y `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="888d8-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="888d8-127">Esto no significa que vaya a obtener combinaciones redundantes; en la mayoría de los casos, EF consolidará las combinaciones al generar código SQL.</span><span class="sxs-lookup"><span data-stu-id="888d8-127">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="888d8-128">Inclusión en tipos derivados</span><span class="sxs-lookup"><span data-stu-id="888d8-128">Include on derived types</span></span>

<span data-ttu-id="888d8-129">Puede incluir datos relacionados provenientes de las navegaciones que se definen solo en un tipo derivado con `Include` y `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="888d8-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="888d8-130">Dado el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="888d8-130">Given the following model:</span></span>

```csharp
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

<span data-ttu-id="888d8-131">El contenido de la navegación `School` de todas las personas que son estudiantes se puede cargar de manera diligente mediante el uso de diversos patrones:</span><span class="sxs-lookup"><span data-stu-id="888d8-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="888d8-132">con conversión</span><span class="sxs-lookup"><span data-stu-id="888d8-132">using cast</span></span>
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="888d8-133">con el operador `as`</span><span class="sxs-lookup"><span data-stu-id="888d8-133">using `as` operator</span></span>
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="888d8-134">con la sobrecarga de `Include` que toma el parámetro del tipo `string`</span><span class="sxs-lookup"><span data-stu-id="888d8-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="888d8-135">Inclusiones omitidas</span><span class="sxs-lookup"><span data-stu-id="888d8-135">Ignored includes</span></span>

<span data-ttu-id="888d8-136">Si modifica la consulta de manera que ya no devuelva instancias del tipo de entidad con que empezó la consulta, se omiten los operadores de inclusión.</span><span class="sxs-lookup"><span data-stu-id="888d8-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="888d8-137">En el ejemplo siguiente, los operadores de inclusión se basan en `Blog`, pero luego el operador `Select` se usa para cambiar la consulta para que devuelva un tipo anónimo.</span><span class="sxs-lookup"><span data-stu-id="888d8-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="888d8-138">En este caso, los operadores de inclusión no tienen ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="888d8-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="888d8-139">De manera predeterminada, EF Core registrará una advertencia cuando se omitan los operadores de inclusión.</span><span class="sxs-lookup"><span data-stu-id="888d8-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="888d8-140">Consulte [Logging](../miscellaneous/logging.md) (Registro) para más información sobre cómo ver la salida de registro.</span><span class="sxs-lookup"><span data-stu-id="888d8-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="888d8-141">Puede cambiar el comportamiento cuando se omite un operador de inclusión, ya sea para que se genere alguna excepción o para que no haga nada.</span><span class="sxs-lookup"><span data-stu-id="888d8-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="888d8-142">Esto se hace al configurar las opciones del contexto, por lo general en `DbContext.OnConfiguring` o en `Startup.cs` si usa ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="888d8-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="888d8-143">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="888d8-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="888d8-144">Esta característica se introdujo en EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="888d8-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="888d8-145">Puede cargar de manera explícita una propiedad de navegación a través de la API `DbContext.Entry(...)`.</span><span class="sxs-lookup"><span data-stu-id="888d8-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="888d8-146">También puede cargar de manera explícita una propiedad de navegación si ejecuta una consulta independiente que devuelve las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="888d8-146">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="888d8-147">Si está habilitado el seguimiento de cambios, cuando se cargue una entidad, EF Core establecerá automáticamente las propiedades de navegación de la entidad recién cargada para hacer referencia a cualquier entidad ya cargada y establecerá las propiedades de navegación de las entidades ya cargadas para hacer referencia a la entidad recién cargada.</span><span class="sxs-lookup"><span data-stu-id="888d8-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="888d8-148">Consulta de las entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="888d8-148">Querying related entities</span></span>

<span data-ttu-id="888d8-149">También puede obtener una consulta LINQ que represente el contenido de una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="888d8-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="888d8-150">Esto permite, entre otras acciones, ejecutar un operador de agregado en las entidades relacionadas sin cargarlas en la memoria.</span><span class="sxs-lookup"><span data-stu-id="888d8-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="888d8-151">También puede filtrar las entidades relacionadas que se cargan en la memoria.</span><span class="sxs-lookup"><span data-stu-id="888d8-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="888d8-152">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="888d8-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="888d8-153">Esta característica se introdujo en EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="888d8-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="888d8-154">La manera más simple de usar la carga diferida es instalar el paquete [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) y habilitarlo con una llamada a `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="888d8-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="888d8-155">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="888d8-155">For example:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="888d8-156">O al usar AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="888d8-156">Or when using AddDbContext:</span></span>
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
<span data-ttu-id="888d8-157">EF Core habilitará la carga diferida de cualquier propiedad de navegación que se pueda invalidar, es decir, debe ser `virtual` y debe estar en una clase desde la que se pueda heredar.</span><span class="sxs-lookup"><span data-stu-id="888d8-157">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="888d8-158">Por ejemplo, en las entidades siguientes, las propiedades de navegación `Post.Blog` y `Blog.Posts` serán de carga diferida.</span><span class="sxs-lookup"><span data-stu-id="888d8-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```csharp
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="888d8-159">Carga diferida sin servidores proxy</span><span class="sxs-lookup"><span data-stu-id="888d8-159">Lazy loading without proxies</span></span>

<span data-ttu-id="888d8-160">Los servidores proxy de carga diferida funcionan inyectando el servicio `ILazyLoader` en una entidad, tal como se describe en [Entity Type Constructors](../modeling/constructors.md) (Constructores de tipo de entidad).</span><span class="sxs-lookup"><span data-stu-id="888d8-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="888d8-161">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="888d8-161">For example:</span></span>
```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="888d8-162">Esto no requiere tipos de entidad de los cuales heredar ni propiedades de navegación para ser virtual y permite que las instancias de entidad creadas con `new` se carguen de manera diferida una vez que se asocian a un contexto.</span><span class="sxs-lookup"><span data-stu-id="888d8-162">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="888d8-163">Sin embargo, requiere una referencia al servicio `ILazyLoader`, que está definido en el paquete [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/).</span><span class="sxs-lookup"><span data-stu-id="888d8-163">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="888d8-164">Este paquete contiene un conjunto mínimo de tipos, por lo que es muy poco el impacto al depender de él.</span><span class="sxs-lookup"><span data-stu-id="888d8-164">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="888d8-165">Sin embargo, para evitar por completo depender de cualquier paquete de EF Core en los tipos de entidad, es posible insertar el método `ILazyLoader.Load` como delegado.</span><span class="sxs-lookup"><span data-stu-id="888d8-165">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="888d8-166">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="888d8-166">For example:</span></span>
```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="888d8-167">El código anterior usa un método de extensión `Load` para que el uso del delegado sea un poco más limpio:</span><span class="sxs-lookup"><span data-stu-id="888d8-167">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```csharp
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
> <span data-ttu-id="888d8-168">El parámetro de constructor del delegado de carga diferida se debe denominar "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="888d8-168">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="888d8-169">La configuración para usar otro nombre está planificada para una versión futura.</span><span class="sxs-lookup"><span data-stu-id="888d8-169">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="888d8-170">Datos relacionados y serialización</span><span class="sxs-lookup"><span data-stu-id="888d8-170">Related data and serialization</span></span>

<span data-ttu-id="888d8-171">Como EF Core corregirá automáticamente las propiedades de navegación, puede terminar con ciclos en el grafo de objetos.</span><span class="sxs-lookup"><span data-stu-id="888d8-171">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="888d8-172">Por ejemplo, cargar un blog y sus entradas relacionadas dará lugar a un objeto de blog que hará referencia a una colección de entradas.</span><span class="sxs-lookup"><span data-stu-id="888d8-172">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="888d8-173">Cada una de esas entradas tendrá una referencia de vuelta al blog.</span><span class="sxs-lookup"><span data-stu-id="888d8-173">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="888d8-174">Algunos marcos de serialización no permiten ese tipo de ciclos.</span><span class="sxs-lookup"><span data-stu-id="888d8-174">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="888d8-175">Por ejemplo, JSON.NET generará la excepción siguiente si se encuentra un ciclo.</span><span class="sxs-lookup"><span data-stu-id="888d8-175">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="888d8-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'. (Newtonsoft.Json.JsonSerializationException: se detectó un bucle con autorreferencia para la propiedad "Blog" con el tipo "MyApplication.Models.Blog").</span><span class="sxs-lookup"><span data-stu-id="888d8-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="888d8-177">Si usa ASP.NET Core, puede configurar JSON.NET para que omita los ciclos que encuentre en el grafo del objeto.</span><span class="sxs-lookup"><span data-stu-id="888d8-177">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="888d8-178">Esto se hace en el método `ConfigureServices(...)` en `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="888d8-178">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

```csharp
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
