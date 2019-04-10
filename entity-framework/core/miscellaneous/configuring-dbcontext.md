---
title: Configuración de un elemento DbContext - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 0350b25d0d0efe05df7cb9e93a3f4ae2d864fd63
ms.sourcegitcommit: 47e0a66a136e743a815d099d2bee5f0da1a068c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59363942"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="7ccdd-102">Configuración de un DbContext</span><span class="sxs-lookup"><span data-stu-id="7ccdd-102">Configuring a DbContext</span></span>

<span data-ttu-id="7ccdd-103">En este artículo se muestra los patrones básicos para configurar un `DbContext` a través de un `DbContextOptions` para conectarse a una base de datos mediante un proveedor de EF Core específico y comportamientos opcionales.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="7ccdd-104">Configuración de DbContext de tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="7ccdd-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="7ccdd-105">Tiempo de diseño EF Core herramientas como [migraciones](xref:core/managing-schemas/migrations/index) debe ser capaz de detectar y crear una instancia de trabajo de un `DbContext` tipo con el fin de recopilar detalles acerca de los tipos de entidad de la aplicación y cómo se asignan a un esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="7ccdd-106">Este proceso puede ser automática siempre que la herramienta puede crear fácilmente el `DbContext` de tal manera que se configurará de forma similar a cómo se podría configurar en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="7ccdd-107">Aunque cualquier patrón que proporciona la información de configuración necesaria para la `DbContext` puede funcionar en tiempo de ejecución, las herramientas que requieren el uso de un `DbContext` solo puede funcionar en tiempo de diseño con un número limitado de patrones.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="7ccdd-108">Se tratan con más detalle en la [creación del contexto de tiempo de diseño](xref:core/miscellaneous/cli/dbcontext-creation) sección.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="7ccdd-109">Configuración de DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="7ccdd-109">Configuring DbContextOptions</span></span>

`DbContext` <span data-ttu-id="7ccdd-110">debe tener una instancia de `DbContextOptions` con el fin de realizar cualquier trabajo.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-110">must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="7ccdd-111">El `DbContextOptions` instancia lleva información de configuración, como:</span><span class="sxs-lookup"><span data-stu-id="7ccdd-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="7ccdd-112">Normalmente se selecciona el proveedor de base de datos debe usar, invocando un método como `UseSqlServer` o `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="7ccdd-113">Estos métodos de extensión requieren el correspondiente paquete de proveedor, como `Microsoft.EntityFrameworkCore.SqlServer` o `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="7ccdd-114">Los métodos se definen en el `Microsoft.EntityFrameworkCore` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="7ccdd-115">Cualquier cadena de conexión necesaria o el identificador de la instancia de base de datos, pasa normalmente como un argumento al método de selección del proveedor se ha mencionado anteriormente</span><span class="sxs-lookup"><span data-stu-id="7ccdd-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="7ccdd-116">Selectores de cualquier comportamiento opcional de nivel de proveedor, normalmente también encadenados dentro de la llamada al método de selección del proveedor</span><span class="sxs-lookup"><span data-stu-id="7ccdd-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="7ccdd-117">Cualquier selectores de comportamiento generales de EF Core, normalmente encadenados después o antes del método de selector de proveedor</span><span class="sxs-lookup"><span data-stu-id="7ccdd-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="7ccdd-118">El ejemplo siguiente configura el `DbContextOptions` para utilizar el proveedor de SQL Server, una conexión contenida en el `connectionString` variable, un tiempo de espera de comando de nivel de proveedor y un selector de comportamiento de EF Core que realiza todas las consultas ejecutadas en la `DbContext` [no realizar un seguimiento](xref:core/querying/tracking#no-tracking-queries) de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="7ccdd-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="7ccdd-119">Métodos de selector de proveedor y otros métodos de selector de comportamiento mencionados anteriormente son métodos de extensión en `DbContextOptions` o clases de opciones específicas del proveedor.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="7ccdd-120">Para tener acceso a estos métodos de extensión, es posible que deba tener un espacio de nombres (normalmente `Microsoft.EntityFrameworkCore`) en el ámbito e incluyen las dependencias de paquetes adicionales en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="7ccdd-121">El `DbContextOptions` puede suministrarse a la `DbContext` invalidando el `OnConfiguring` método o externamente mediante un argumento de constructor.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="7ccdd-122">Si se usan ambos, `OnConfiguring` se aplica en último lugar y se puede sobrescribir las opciones proporcionadas para el argumento del constructor.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="7ccdd-123">Argumento de constructor</span><span class="sxs-lookup"><span data-stu-id="7ccdd-123">Constructor argument</span></span>

<span data-ttu-id="7ccdd-124">Código del contexto con el constructor:</span><span class="sxs-lookup"><span data-stu-id="7ccdd-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="7ccdd-125">El constructor de DbContext base también acepta la versión no genérica de `DbContextOptions`, pero no se recomienda usar la versión no genérica para aplicaciones con varios tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="7ccdd-126">Código de aplicación para inicializar a partir del argumento de constructor:</span><span class="sxs-lookup"><span data-stu-id="7ccdd-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="7ccdd-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="7ccdd-127">OnConfiguring</span></span>

<span data-ttu-id="7ccdd-128">Código del contexto con `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="7ccdd-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="7ccdd-129">Código de aplicación para inicializar un `DbContext` que usa `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="7ccdd-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="7ccdd-130">Este enfoque no se presta para las pruebas, a menos que la base de datos completa de las pruebas de destino.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="7ccdd-131">Uso de DbContext con inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="7ccdd-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="7ccdd-132">EF Core admite el uso de `DbContext` con un contenedor de inserción de dependencia.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="7ccdd-133">El tipo de DbContext se puede agregar al contenedor de servicios mediante el `AddDbContext<TContext>` método.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

`AddDbContext<TContext>` <span data-ttu-id="7ccdd-134">hará que tanto el tipo de DbContext, `TContext`y la correspondiente `DbContextOptions<TContext>` disponibles para la inserción del contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-134">will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="7ccdd-135">Consulte [leer más](#more-reading) a continuación para obtener más información sobre la inserción de dependencia.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="7ccdd-136">Agregar el `Dbcontext` para inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="7ccdd-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="7ccdd-137">Esto requiere la adición de un [argumento del constructor](#constructor-argument) para el tipo de DbContext que acepta `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="7ccdd-138">Código del contexto:</span><span class="sxs-lookup"><span data-stu-id="7ccdd-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="7ccdd-139">Código de la aplicación (en ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="7ccdd-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="7ccdd-140">Código de la aplicación (mediante ServiceProvider directamente, menos común):</span><span class="sxs-lookup"><span data-stu-id="7ccdd-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="7ccdd-141">Evitar problemas de subprocesamiento de DbContext</span><span class="sxs-lookup"><span data-stu-id="7ccdd-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="7ccdd-142">Entity Framework Core no admite varias operaciones en paralelo que se ejecutan en el mismo `DbContext` instancia.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="7ccdd-143">Acceso simultáneo puede producir un comportamiento indefinido, bloqueos de la aplicación y daños en los datos.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-143">Concurrent access can result in undefined behavior, application crashes and data corruption.</span></span> <span data-ttu-id="7ccdd-144">Debido a esto es importante usar siempre separar `DbContext` instancias para las operaciones que se ejecutan en paralelo.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-144">Because of this it's important to always use separate `DbContext` instances for operations that execute in parallel.</span></span> 

<span data-ttu-id="7ccdd-145">Hay errores comunes que pueden inadvernetly causa simultáneo en la misma `DbContext` instancia:</span><span class="sxs-lookup"><span data-stu-id="7ccdd-145">There are common mistakes that can inadvernetly cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="7ccdd-146">Si se olvida de esperar la finalización de una operación asincrónica antes de iniciar cualquier otra operación en el mismo DbContext</span><span class="sxs-lookup"><span data-stu-id="7ccdd-146">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="7ccdd-147">Los métodos asincrónicos habilitar EF Core iniciar operaciones que tienen acceso a la base de datos en un modo sin bloqueo.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-147">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="7ccdd-148">Sin embargo, si un llamador no espera la finalización de uno de estos métodos y se inicia para realizar otras operaciones en el `DbContext`, el estado de la `DbContext` puede ser (y probablemente habrá) está dañado.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-148">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="7ccdd-149">Siempre se espere inmediatamente los métodos asincrónicos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-149">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="7ccdd-150">Implícitamente compartir instancias de DbContext en varios subprocesos mediante la inserción de dependencia</span><span class="sxs-lookup"><span data-stu-id="7ccdd-150">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="7ccdd-151">El [ `AddDbContext` ](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registra el método de extensión `DbContext` tipos con un [duración con ámbito](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-151">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="7ccdd-152">Esto es seguro de problemas de acceso simultáneo en aplicaciones ASP.NET Core porque hay solo un subproceso que se ejecuta cada solicitud de cliente en un momento dado, y dado que cada solicitud obtiene un ámbito de inyección de dependencia independiente (y, por tanto, otra `DbContext` (instancia).</span><span class="sxs-lookup"><span data-stu-id="7ccdd-152">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="7ccdd-153">Sin embargo, cualquier código que se ejecuta explícitamente varios subprocesos en paralelo debe asegurarse que `DbContext` instancias no son nunca accesed al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-153">However any code that explicitly executes multiple threads in paralell should ensure that `DbContext` instances aren't ever accesed concurrently.</span></span>

<span data-ttu-id="7ccdd-154">Mediante la inserción de dependencias, esto puede lograrse registrando el contexto como ámbito y crear ámbitos (mediante `IServiceScopeFactory`) para cada subproceso, o bien registrar la `DbContext` como transitorio (mediante la sobrecarga de `AddDbContext` que toma un `ServiceLifetime` parámetro).</span><span class="sxs-lookup"><span data-stu-id="7ccdd-154">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="7ccdd-155">Leer más</span><span class="sxs-lookup"><span data-stu-id="7ccdd-155">More reading</span></span>

* <span data-ttu-id="7ccdd-156">Lectura [Introducción a ASP.NET Core](../get-started/aspnetcore/index.md) para obtener más información sobre el uso de EF con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-156">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="7ccdd-157">Lectura [inserción de dependencias](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para obtener más información sobre el uso de DI.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-157">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="7ccdd-158">Lectura [pruebas](testing/index.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="7ccdd-158">Read [Testing](testing/index.md) for more information.</span></span>
