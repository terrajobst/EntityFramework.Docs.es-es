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
# <a name="loading-related-data"></a>Cargar datos relacionados

Entity Framework Core le permite usar las propiedades de navegación en el modelo para cargar las entidades relacionadas. Hay tres patrones O/RM comunes usados para cargar los datos relacionados.
* **Carga diligente** significa que los datos relacionados se cargan desde la base de datos como parte de la consulta inicial.
* **Carga explícita** significa que los datos relacionados se carguen explícitamente desde la base de datos en un momento posterior.
* **La carga diferida** significa que los datos relacionados se cargan transparente desde la base de datos cuando se tiene acceso a la propiedad de navegación.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.

## <a name="eager-loading"></a>Carga diligente

Puede usar el `Include` método para especificar los datos relacionados que se incluirá en los resultados de la consulta. En el ejemplo siguiente, tendrá los blogs que se devuelven en los resultados de sus `Posts` propiedad que se rellene con las entradas relacionadas.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Propiedades de navegación automáticamente revisión de seguridad de entidad Framework Core will a cualquier otra entidad que se cargaron previamente en la instancia de contexto. Por lo que incluso si no se incluye explícitamente los datos de una propiedad de navegación, la propiedad también puede rellenar si algunas o todas las entidades relacionadas se cargaron previamente.


Puede incluir datos relacionados de varias relaciones en una sola consulta.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Incluidos varios niveles

Puede explorar en profundidad a través de relaciones para incluir varios niveles de datos relacionados con el `ThenInclude` método. En el ejemplo siguiente se cargan todos los blogs, sus entradas relacionadas y el autor de cada publicación.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Las versiones actuales de Visual Studio ofrecen opciones de finalización de código incorrecto y puede causar expresiones correctas se marque con errores de sintaxis cuando se usa el `ThenInclude` método después de una propiedad de navegación de la colección. Esto es un síntoma de un error de IntelliSense que realiza el seguimiento en https://github.com/dotnet/roslyn/issues/8237. Es seguro omitir estos errores de sintaxis falsos siempre que el código es correcto y puede compilarse correctamente. 

Se pueden encadenar varias llamadas a `ThenInclude` para continuar incluso más niveles de los datos relacionados.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Puede combinar todo esto debe incluir datos relacionados de varios niveles y varias raíces en la misma consulta.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Puede incluir varias entidades relacionadas para una de las entidades que se incluye. Por ejemplo, al consultar `Blog`s, incluye `Posts` y, a continuación, se van a incluir tanto el `Author` y `Tags` de la `Posts`. Para ello, debe especificar cada uno de ellos incluye empezando por la raíz de la ruta de acceso. Por ejemplo, `Blog -> Posts -> Author` y `Blog -> Posts -> Tags`. Esto no significa que obtendrá combinaciones redundantes, en la mayoría de los casos que se consolidará EF las combinaciones cuando la generación de SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Incluir en tipos derivados

Puede incluir datos relacionados de navegaciones solo definidas en un tipo derivado mediante `Include` y `ThenInclude`. 

Dado el modelo siguiente:

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

Contenido de `School` panel de navegación de todas las personas que son los estudiantes puede se carguen usando una serie de patrones:

- utilizar cast
```Csharp
context.People.Include(person => ((Student)person).School).ToList()
```

- con `as` (operador)
```Csharp
context.People.Include(person => (person as Student).School).ToList()
```

- mediante la sobrecarga de `Include` que toma el parámetro de tipo `string`
```Csharp
context.People.Include("Student").ToList()
```

### <a name="ignored-includes"></a>Omite incluye

Si cambia la consulta para que ya no devuelve instancias del tipo de entidad que la consulta comienza con, se omiten los operadores de inclusión.

En el ejemplo siguiente, los operadores de inclusión se basan en el `Blog`, pero, a continuación, el `Select` operador se utiliza para cambiar la consulta para devolver un tipo anónimo. En este caso, los operadores de inclusión no tienen ningún efecto.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

De forma predeterminada, el núcleo de EF registrará una advertencia cuando se incluyen operadores se omiten. Vea [registro](../miscellaneous/logging.md) para obtener más información acerca de cómo ver los resultados del registro. Puede cambiar el comportamiento cuando un operador de inclusión se omite para producir o no hacer nada. Esto se realiza al configurar las opciones para el contexto - normalmente en `DbContext.OnConfiguring`, o bien en `Startup.cs` si utiliza ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Carga explícita

> [!NOTE]  
> Esta característica se introdujo en EF Core 1.1.

Puede cargar explícitamente una propiedad de navegación a través de la `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Puede cargar explícitamente una propiedad de navegación mediante la ejecución de una consulta independiente que devuelve las entidades relacionadas. Si está habilitado el seguimiento de cambios, al cargar una entidad, Core EF automáticamente establecer las propiedades de navegación de la entitiy recientemente cargados para hacer referencia a una entidad que ya están cargada y establecer las propiedades de navegación de las entidades ya cargado para hacer referencia a la entidad recién cargado.

### <a name="querying-related-entities"></a>Consultar las entidades relacionadas

También puede obtener una consulta LINQ que representa el contenido de una propiedad de navegación.

Esto le permite hacer cosas como la ejecución de un operador agregado sobre las entidades relacionadas sin cargarlos en la memoria.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

También puede filtrar las entidades relacionadas se cargan en memoria.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Carga diferida

> [!NOTE]  
> Esta característica se introdujo en el EF Core 2.1.

La manera más sencilla de usar la carga diferida es instalar el [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) paquete y habilitar con una llamada a `UseLazyLoadingProxies`. Por ejemplo:
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
O bien, cuando se utiliza AddDbContext:
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
EF núcleo permitirán carga-diferida para cualquier propiedad de navegación que se puede reemplazar--que es, debe ser `virtual` y en una clase que se puede heredar de. Por ejemplo, en las siguientes entidades, la `Post.Blog` y `Blog.Posts` las propiedades de navegación será la carga diferida.
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
### <a name="lazy-loading-without-proxies"></a>Carga Lazy sin servidores proxy

Proxies de carga Lazy funcionan mediante la inyección de la `ILazyLoader` de servicio en una entidad, como se describe en [constructores del tipo de entidad](../modeling/constructors.md). Por ejemplo:
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
Esto no requiere tipos de entidad se herede de o propiedades de navegación sea virtual y permite instancias de entidad que se creó con `new` a la carga diferida una vez conectado a un contexto. Sin embargo, requiere una referencia a la `ILazyLoader` servicio, que se acopla con los tipos de entidad del ensamblado principal de EF. Para evitar este núcleo EF permite el `ILazyLoader.Load` método se insertaran como delegado. Por ejemplo:
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
El código anterior utiliza un `Load` método de extensión para hacer utilizando un poco limpiador el delegado:
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
> El parámetro del constructor del delegado de la carga diferida debe llamarse "lazyLoader". Configuración para utilizar un nombre diferente que se ha planificado para una futura versión.

## <a name="related-data-and-serialization"></a>Serialización y los datos relacionados

Porque las propiedades de navegación de corrección telefónico automáticamente de will de EF Core, puede acabar con ciclos en el gráfico de objetos. Por ejemplo, cargando un blog y está relacionado con entradas dará como resultado un objeto de blog que hace referencia a una colección de entradas. Cada una de esas entradas tendrá una referencia al blog.

Algunos marcos de serialización no permiten estos ciclos. Por ejemplo, Json.NET se producirá la siguiente excepción si se produce un ciclo.

> Newtonsoft.Json.JsonSerializationException: Self que hacen referencia a bucle detectado para la propiedad 'Blog' con el tipo 'MyApplication.Models.Blog'.

Si utiliza ASP.NET Core, puede configurar Json.NET para omitir los ciclos que encuentran en el gráfico de objetos. Esto se hace en el `ConfigureServices(...)` método `Startup.cs`.

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
