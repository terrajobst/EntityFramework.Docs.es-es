---
title: Configurar un DbContext - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: de26e3b28851d4dc4e50f0490093dd05ad489b31
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="8b5b7-102">Configurar un DbContext</span><span class="sxs-lookup"><span data-stu-id="8b5b7-102">Configuring a DbContext</span></span>

<span data-ttu-id="8b5b7-103">Este artículo muestra los patrones básicos para configurar un `DbContext` a través de un `DbContextOptions` para conectarse a una base de datos mediante un proveedor de EF principales específico y comportamientos opcionales.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="8b5b7-104">Configuración de DbContext de tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="8b5b7-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="8b5b7-105">Tiempo de diseño EF principales herramientas como [migraciones](xref:core/managing-schemas/migrations/index) necesita ser capaz de detectar y crear una instancia de trabajo de un `DbContext` tipo con el fin de obtener detalles sobre los tipos de entidad de la aplicación y cómo se asignan a un esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="8b5b7-106">Este proceso puede ser automática siempre que la herramienta puede crear fácilmente el `DbContext` de tal manera que se configurará con forma similar a cómo se puede configurar en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at runt-time.</span></span>

<span data-ttu-id="8b5b7-107">Mientras cualquier patrón que proporciona la información de configuración necesaria para la `DbContext` puede trabajar en tiempo de ejecución, herramientas que requieren el uso de un `DbContext` en tiempo de diseño sólo puede trabajar con un número limitado de patrones.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="8b5b7-108">Se tratan con más detalle en la [creación del contexto de tiempo de diseño](xref:core/miscellaneous/cli/dbcontext-creation) sección.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="8b5b7-109">Configurar DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="8b5b7-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="8b5b7-110">`DbContext`debe tener una instancia de `DbContextOptions` para realizar cualquier trabajo.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="8b5b7-111">El `DbContextOptions` instancia lleva información de configuración como:</span><span class="sxs-lookup"><span data-stu-id="8b5b7-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="8b5b7-112">El proveedor de base de datos que se utiliza normalmente selecciona invocando un método como `UseSqlServer` o`UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="8b5b7-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="8b5b7-113">Cualquier cadena de conexión necesaria o el identificador de la instancia de base de datos, pasa normalmente como un argumento para el método de selección de proveedor mencionado anteriormente</span><span class="sxs-lookup"><span data-stu-id="8b5b7-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="8b5b7-114">Los selectores de comportamiento opcional de nivel del proveedor, normalmente también encadenados dentro de la llamada al método de selección de proveedor</span><span class="sxs-lookup"><span data-stu-id="8b5b7-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="8b5b7-115">Los selectores de comportamiento EF Core generales normalmente encadenados después o antes del método de selector de proveedor</span><span class="sxs-lookup"><span data-stu-id="8b5b7-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="8b5b7-116">En el ejemplo siguiente se configura el `DbContextOptions` para usar el proveedor de SQL Server, una conexión incluidos en el `connectionString` variable, un tiempo de espera de comando de nivel del proveedor y un selector de comportamiento de EF Core que hace que todas las consultas ejecutadas en la `DbContext` [seguimiento no](xref:core/querying/tracking#no-tracking-queries) de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="8b5b7-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="8b5b7-117">Métodos de selector de proveedor y otros métodos de selector de comportamiento mencionados anteriormente son métodos de extensión en `DbContextOptions` o clases de opciones específicas del proveedor.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="8b5b7-118">Para tener acceso a estos métodos de extensión debe tener un espacio de nombres (normalmente `Microsoft.EntityFrameworkCore`) en el ámbito e incluir las dependencias del paquete adicional en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="8b5b7-119">El `DbContextOptions` pueden proporcionar a la `DbContext` invalidando el `OnConfiguring` método o externamente a través de un argumento de constructor.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="8b5b7-120">Si se utilizan ambos, `OnConfiguring` se aplica en último lugar y pueden sobrescribir opciones proporcionadas para el argumento del constructor.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="8b5b7-121">Argumentos de constructor</span><span class="sxs-lookup"><span data-stu-id="8b5b7-121">Constructor argument</span></span>

<span data-ttu-id="8b5b7-122">Código del contexto con el constructor:</span><span class="sxs-lookup"><span data-stu-id="8b5b7-122">Context code with constructor:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="8b5b7-123">El constructor base de DbContext acepta también la versión no genérica de `DbContextOptions`, pero no se recomienda utilizar la versión no genérica para aplicaciones con varios tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="8b5b7-124">Código de la aplicación para inicializar de argumentos de constructor:</span><span class="sxs-lookup"><span data-stu-id="8b5b7-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="8b5b7-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="8b5b7-125">OnConfiguring</span></span>

<span data-ttu-id="8b5b7-126">Código del contexto con `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="8b5b7-126">Context code with `OnConfiguring`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="8b5b7-127">Código de la aplicación para inicializar un `DbContext` que utiliza `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="8b5b7-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="8b5b7-128">Este enfoque no se presta a las pruebas, a menos que la base de datos completa de las pruebas de destino.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="8b5b7-129">Uso de DbContext con inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="8b5b7-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="8b5b7-130">Núcleo EF admite el uso de `DbContext` con un contenedor de inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="8b5b7-131">El tipo de DbContext se puede agregar al contenedor de servicios mediante la `AddDbContext<TContext>` método.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="8b5b7-132">`AddDbContext<TContext>`hará que tanto el tipo de DbContext, `TContext`y la correspondiente `DbContextOptions<TContext>` disponibles para la inyección del contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="8b5b7-133">Vea [leer más](#more-reading) a continuación para obtener más información sobre la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="8b5b7-134">Agregar el `Dbcontext` a la inyección de dependencia:</span><span class="sxs-lookup"><span data-stu-id="8b5b7-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="8b5b7-135">Esto es necesario agregar una [argumento del constructor](#constructor-argument) a su tipo de DbContext que acepta `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="8b5b7-136">Código del contexto:</span><span class="sxs-lookup"><span data-stu-id="8b5b7-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="8b5b7-137">Código de la aplicación (en ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="8b5b7-137">Application code (in ASP.NET Core):</span></span>

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

<span data-ttu-id="8b5b7-138">Código de la aplicación (mediante ServiceProvider directamente, menos común):</span><span class="sxs-lookup"><span data-stu-id="8b5b7-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="8b5b7-139">Leer más</span><span class="sxs-lookup"><span data-stu-id="8b5b7-139">More reading</span></span>

* <span data-ttu-id="8b5b7-140">Lectura [Introducción a ASP.NET Core](../get-started/aspnetcore/index.md) para obtener más información sobre el uso de EF con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="8b5b7-141">Lectura [inyección de dependencia](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para obtener información adicional acerca del uso DI.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="8b5b7-142">Lectura [pruebas](testing/index.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="8b5b7-142">Read [Testing](testing/index.md) for more information.</span></span>
