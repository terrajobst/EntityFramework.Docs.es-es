---
title: Configuración de DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: ddabf825ef23c2ec07efcde390df7d0cf48db33c
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306508"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="5a3ca-102">Configuración de DbContext</span><span class="sxs-lookup"><span data-stu-id="5a3ca-102">Configuring a DbContext</span></span>

<span data-ttu-id="5a3ca-103">En este artículo se muestran patrones básicos para `DbContext` configurar un a través de para conectarse a una base de datos de mediante un proveedor de EF Core específico y comportamientos opcionales. `DbContextOptions`</span><span class="sxs-lookup"><span data-stu-id="5a3ca-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="5a3ca-104">Configuración de DbContext en tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="5a3ca-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="5a3ca-105">EF Core herramientas en tiempo de diseño como las [migraciones](xref:core/managing-schemas/migrations/index) deben ser capaces de detectar y crear una instancia de trabajo de un `DbContext` tipo para recopilar detalles sobre los tipos de entidad de la aplicación y cómo se asignan a un esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="5a3ca-106">Este proceso puede ser automático siempre que la herramienta pueda crear fácilmente el `DbContext` de forma que se configure de forma similar a como se configuraría en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="5a3ca-107">Aunque cualquier patrón que proporcione la información de configuración necesaria para `DbContext` puede funcionar en tiempo de ejecución, las herramientas que requieren el `DbContext` uso de en tiempo de diseño solo pueden trabajar con un número limitado de patrones.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="5a3ca-108">Estos se describen con más detalle en la sección [creación de contexto en tiempo de diseño](xref:core/miscellaneous/cli/dbcontext-creation) .</span><span class="sxs-lookup"><span data-stu-id="5a3ca-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="5a3ca-109">Configuración de DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="5a3ca-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="5a3ca-110">`DbContext`debe tener una instancia de `DbContextOptions` para poder realizar cualquier trabajo.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="5a3ca-111">La `DbContextOptions` instancia contiene información de configuración como:</span><span class="sxs-lookup"><span data-stu-id="5a3ca-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="5a3ca-112">Proveedor de base de datos que se va a usar, normalmente seleccionado mediante la `UseSqlServer` invocación de un método como o. `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="5a3ca-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="5a3ca-113">Estos métodos de extensión requieren el paquete de proveedor correspondiente, `Microsoft.EntityFrameworkCore.SqlServer` como `Microsoft.EntityFrameworkCore.Sqlite`o.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="5a3ca-114">Los métodos se definen en el `Microsoft.EntityFrameworkCore` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="5a3ca-115">Cualquier cadena de conexión o identificador de la instancia de base de datos que sea necesario, normalmente se pasa como argumento al método de selección de proveedor mencionado anteriormente</span><span class="sxs-lookup"><span data-stu-id="5a3ca-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="5a3ca-116">Cualquier selector de comportamiento opcional de nivel de proveedor, normalmente también encadenado dentro de la llamada al método de selección de proveedor.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="5a3ca-117">Los selectores de comportamiento de EF Core general, normalmente encadenados después o antes del método de selector de proveedor</span><span class="sxs-lookup"><span data-stu-id="5a3ca-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="5a3ca-118">`DbContextOptions` En el ejemplo siguiente se configura para utilizar el proveedor de SQL Server, una conexión contenida en `connectionString` la variable, un tiempo de espera de comando de nivel de proveedor y un selector de comportamiento de EF Core que `DbContext` hace que todas las consultas se ejecuten en el [sin seguimiento](xref:core/querying/tracking#no-tracking-queries) de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="5a3ca-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="5a3ca-119">Los métodos de selector de proveedor y otros métodos de selector de comportamiento mencionados `DbContextOptions` anteriormente son métodos de extensión de o clases de opciones específicas del proveedor.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="5a3ca-120">Para tener acceso a estos métodos de extensión, puede que necesite tener un espacio de nombres ( `Microsoft.EntityFrameworkCore`normalmente) en el ámbito e incluir dependencias de paquetes adicionales en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="5a3ca-121">Se puede proporcionar `DbContext` al invalidando el `OnConfiguring` método o externamente a través de un argumento de constructor. `DbContextOptions`</span><span class="sxs-lookup"><span data-stu-id="5a3ca-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="5a3ca-122">Si se usan ambos, `OnConfiguring` se aplica en último lugar y puede sobrescribir las opciones proporcionadas al argumento del constructor.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="5a3ca-123">Argumento de constructor</span><span class="sxs-lookup"><span data-stu-id="5a3ca-123">Constructor argument</span></span>

<span data-ttu-id="5a3ca-124">Código de contexto con el constructor:</span><span class="sxs-lookup"><span data-stu-id="5a3ca-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="5a3ca-125">El constructor base de DbContext también acepta la versión no genérica de `DbContextOptions`, pero no se recomienda usar la versión no genérica para aplicaciones con varios tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="5a3ca-126">Código de aplicación que se va a inicializar desde el argumento de constructor:</span><span class="sxs-lookup"><span data-stu-id="5a3ca-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="5a3ca-127">Configuración</span><span class="sxs-lookup"><span data-stu-id="5a3ca-127">OnConfiguring</span></span>

<span data-ttu-id="5a3ca-128">Código de contexto `OnConfiguring`con:</span><span class="sxs-lookup"><span data-stu-id="5a3ca-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="5a3ca-129">Código de aplicación para inicializar `DbContext` un `OnConfiguring`que usa:</span><span class="sxs-lookup"><span data-stu-id="5a3ca-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="5a3ca-130">Este enfoque no se presta a las pruebas, a menos que las pruebas tengan como destino la base de datos completa.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="5a3ca-131">Usar DbContext con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="5a3ca-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="5a3ca-132">EF Core admite el `DbContext` uso de con un contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="5a3ca-133">El tipo DbContext se puede Agregar al contenedor de servicio mediante el `AddDbContext<TContext>` método.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="5a3ca-134">`AddDbContext<TContext>`hará que el tipo DbContext, `TContext`y el correspondiente `DbContextOptions<TContext>` estén disponibles para la inserción desde el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="5a3ca-135">Vea [más](#more-reading) información a continuación para obtener más información sobre la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="5a3ca-136">Agregar a `DbContext` la inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="5a3ca-136">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="5a3ca-137">Esto requiere agregar un [argumento de constructor](#constructor-argument) al tipo DbContext que acepta `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="5a3ca-138">Código de contexto:</span><span class="sxs-lookup"><span data-stu-id="5a3ca-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="5a3ca-139">Código de la aplicación (en ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="5a3ca-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="5a3ca-140">Código de aplicación (mediante ServiceProvider directamente, menos común):</span><span class="sxs-lookup"><span data-stu-id="5a3ca-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="5a3ca-141">Evitar problemas de subprocesos DbContext</span><span class="sxs-lookup"><span data-stu-id="5a3ca-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="5a3ca-142">Entity Framework Core no admite varias operaciones paralelas que se ejecutan en `DbContext` la misma instancia.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="5a3ca-143">Esto incluye la ejecución paralela de consultas asincrónicas y cualquier uso simultáneo explícito de varios subprocesos.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-143">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="5a3ca-144">Por lo tanto `await` , siempre se llama a las llamadas `DbContext` asincrónicas de inmediato o se usan instancias independientes para las operaciones que se ejecutan en paralelo.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-144">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="5a3ca-145">Cuando EF Core detecta un intento de usar una `DbContext` instancia de simultáneamente, verá un `InvalidOperationException` con un mensaje similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="5a3ca-145">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span> 

> <span data-ttu-id="5a3ca-146">Se inició una segunda operación en este contexto antes de que se completara una operación anterior.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-146">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="5a3ca-147">Esto se debe normalmente a distintos subprocesos que usan la misma instancia de DbContext; sin embargo, no se garantiza que los miembros de instancia sean seguros para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-147">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="5a3ca-148">Cuando el acceso simultáneo no se detecta, puede dar lugar a un comportamiento indefinido, a bloqueos de la aplicación y a daños en los datos.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-148">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="5a3ca-149">Hay errores comunes que pueden provocar involuntariamente el acceso simultáneo a la misma `DbContext` instancia:</span><span class="sxs-lookup"><span data-stu-id="5a3ca-149">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="5a3ca-150">Se olvida esperar la finalización de una operación asincrónica antes de iniciar cualquier otra operación en el mismo DbContext</span><span class="sxs-lookup"><span data-stu-id="5a3ca-150">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="5a3ca-151">Los métodos asincrónicos permiten a EF Core iniciar las operaciones que tienen acceso a la base de datos sin bloqueos.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-151">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="5a3ca-152">Pero si un llamador no espera la finalización de uno de estos métodos y continúa realizando otras operaciones en `DbContext`, el estado `DbContext` de puede ser, (y probablemente, estará dañado).</span><span class="sxs-lookup"><span data-stu-id="5a3ca-152">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="5a3ca-153">Esperar siempre EF Core métodos asincrónicos inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-153">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="5a3ca-154">Compartir instancias de DbContext implícitamente entre varios subprocesos mediante la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="5a3ca-154">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="5a3ca-155">El [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) método de extensión `DbContext` registra los tipos con una [duración de ámbito](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-155">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="5a3ca-156">Esto es seguro contra problemas de acceso simultáneos en ASP.NET Core aplicaciones porque solo hay un subproceso que ejecuta cada solicitud de cliente en un momento dado, y dado que cada solicitud obtiene un ámbito de inyección de dependencia independiente `DbContext` (y, por tanto, un instancia).</span><span class="sxs-lookup"><span data-stu-id="5a3ca-156">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="5a3ca-157">Sin embargo, cualquier código que ejecute explícitamente varios subprocesos en `DbContext` paralelo debe asegurarse de que nunca se tiene acceso a las instancias simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-157">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="5a3ca-158">Mediante la inserción de dependencias, esto se puede lograr registrando el contexto como ámbito y creando ámbitos (mediante `IServiceScopeFactory`) para cada subproceso, o registrando `DbContext` como transitorio (mediante la sobrecarga de `AddDbContext` que toma un `ServiceLifetime` parámetro).</span><span class="sxs-lookup"><span data-stu-id="5a3ca-158">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="5a3ca-159">Más lecturas</span><span class="sxs-lookup"><span data-stu-id="5a3ca-159">More reading</span></span>

* <span data-ttu-id="5a3ca-160">Lea [Introducción en ASP.net Core](../get-started/aspnetcore/index.md) para obtener más información sobre el uso de EF con ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-160">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="5a3ca-161">Lea [inserción](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) de dependencias para obtener más información sobre el uso de di.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-161">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="5a3ca-162">Lea [pruebas](testing/index.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="5a3ca-162">Read [Testing](testing/index.md) for more information.</span></span>
