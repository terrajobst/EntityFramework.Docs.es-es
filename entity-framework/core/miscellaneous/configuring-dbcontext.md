---
title: Configuración de DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 3ab90d46b7a4476044e5ea38eaf04f995708e7bf
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655803"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="77b79-102">Configuración de un DbContext</span><span class="sxs-lookup"><span data-stu-id="77b79-102">Configuring a DbContext</span></span>

<span data-ttu-id="77b79-103">En este artículo se muestran los patrones básicos para configurar un `DbContext` a través de un `DbContextOptions` para conectarse a una base de datos mediante un proveedor de EF Core específico y comportamientos opcionales.</span><span class="sxs-lookup"><span data-stu-id="77b79-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="77b79-104">Configuración de DbContext en tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="77b79-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="77b79-105">EF Core herramientas en tiempo de diseño, como las [migraciones](xref:core/managing-schemas/migrations/index) , deben ser capaces de detectar y crear una instancia de trabajo de un tipo `DbContext` con el fin de recopilar detalles sobre los tipos de entidad de la aplicación y cómo se asignan a un esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="77b79-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="77b79-106">Este proceso puede ser automático siempre que la herramienta pueda crear fácilmente el `DbContext` de manera que se configurará de forma similar a como se configuraría en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="77b79-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="77b79-107">Aunque cualquier patrón que proporcione la información de configuración necesaria al `DbContext` puede funcionar en tiempo de ejecución, las herramientas que requieren el uso de un `DbContext` en tiempo de diseño solo pueden funcionar con un número limitado de patrones.</span><span class="sxs-lookup"><span data-stu-id="77b79-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="77b79-108">Estos se describen con más detalle en la sección [creación de contexto en tiempo de diseño](xref:core/miscellaneous/cli/dbcontext-creation) .</span><span class="sxs-lookup"><span data-stu-id="77b79-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="77b79-109">Configuración de DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="77b79-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="77b79-110">`DbContext` debe tener una instancia de `DbContextOptions` para poder realizar cualquier trabajo.</span><span class="sxs-lookup"><span data-stu-id="77b79-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="77b79-111">La instancia de `DbContextOptions` contiene información de configuración como:</span><span class="sxs-lookup"><span data-stu-id="77b79-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="77b79-112">Proveedor de base de datos que se va a usar, normalmente seleccionado mediante la invocación de un método como `UseSqlServer` o `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="77b79-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="77b79-113">Estos métodos de extensión requieren el paquete de proveedor correspondiente, como `Microsoft.EntityFrameworkCore.SqlServer` o `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="77b79-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="77b79-114">Los métodos se definen en el espacio de nombres `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="77b79-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="77b79-115">Cualquier cadena de conexión o identificador de la instancia de base de datos que sea necesario, normalmente se pasa como argumento al método de selección de proveedor mencionado anteriormente</span><span class="sxs-lookup"><span data-stu-id="77b79-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="77b79-116">Cualquier selector de comportamiento opcional de nivel de proveedor, normalmente también encadenado dentro de la llamada al método de selección de proveedor.</span><span class="sxs-lookup"><span data-stu-id="77b79-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="77b79-117">Los selectores de comportamiento de EF Core general, normalmente encadenados después o antes del método de selector de proveedor</span><span class="sxs-lookup"><span data-stu-id="77b79-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="77b79-118">En el ejemplo siguiente se configura el `DbContextOptions` para usar el proveedor de SQL Server, una conexión contenida en la variable `connectionString`, un tiempo de espera de comando de nivel de proveedor y un selector de comportamiento de EF Core que hace que todas las consultas se ejecuten en la `DbContext` [sin seguimiento](xref:core/querying/tracking#no-tracking-queries) de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="77b79-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="77b79-119">Los métodos de selector de proveedor y otros métodos de selector de comportamiento mencionados anteriormente son métodos de extensión en `DbContextOptions` o clases de opciones específicas del proveedor.</span><span class="sxs-lookup"><span data-stu-id="77b79-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="77b79-120">Para tener acceso a estos métodos de extensión, puede que necesite tener un espacio de nombres (normalmente `Microsoft.EntityFrameworkCore`) en el ámbito e incluir dependencias de paquetes adicionales en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="77b79-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="77b79-121">Se puede proporcionar el `DbContextOptions` al `DbContext` invalidando el método `OnConfiguring` o externamente a través de un argumento de constructor.</span><span class="sxs-lookup"><span data-stu-id="77b79-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="77b79-122">Si se usan ambos, `OnConfiguring` se aplica en último lugar y puede sobrescribir las opciones proporcionadas al argumento del constructor.</span><span class="sxs-lookup"><span data-stu-id="77b79-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="77b79-123">Argumento de constructor</span><span class="sxs-lookup"><span data-stu-id="77b79-123">Constructor argument</span></span>

<span data-ttu-id="77b79-124">El constructor puede aceptar simplemente un `DbContextOptions` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="77b79-124">Your constructor can simply accept a `DbContextOptions` as follows:</span></span>

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
> <span data-ttu-id="77b79-125">El constructor base de DbContext también acepta la versión no genérica de `DbContextOptions`, pero no se recomienda usar la versión no genérica para las aplicaciones con varios tipos de contexto.</span><span class="sxs-lookup"><span data-stu-id="77b79-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="77b79-126">Ahora la aplicación puede pasar el `DbContextOptions` al crear una instancia de un contexto, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="77b79-126">Your application can now pass the `DbContextOptions` when instantiating a context, as follows:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="77b79-127">Configuración</span><span class="sxs-lookup"><span data-stu-id="77b79-127">OnConfiguring</span></span>

<span data-ttu-id="77b79-128">También puede inicializar el `DbContextOptions` en el propio contexto.</span><span class="sxs-lookup"><span data-stu-id="77b79-128">You can also initialize the `DbContextOptions` within the context itself.</span></span> <span data-ttu-id="77b79-129">Aunque puede usar esta técnica para la configuración básica, normalmente tendrá que obtener determinados detalles de configuración del exterior, por ejemplo, una cadena de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="77b79-129">While you can use this technique for basic configuration, you will typically still need to get certain configuration details from the outside, e.g. a database connection string.</span></span> <span data-ttu-id="77b79-130">Esto puede hacerse con una API de configuración o con cualquier otro medio.</span><span class="sxs-lookup"><span data-stu-id="77b79-130">This can be done with a configuration API or any other means.</span></span>

<span data-ttu-id="77b79-131">Para inicializar `DbContextOptions` en el contexto, invalide el método `OnConfiguring` y llame a los métodos en el `DbContextOptionsBuilder` proporcionado:</span><span class="sxs-lookup"><span data-stu-id="77b79-131">To initialize `DbContextOptions` within the context, override the `OnConfiguring` method and call the methods on the provided `DbContextOptionsBuilder`:</span></span>

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

<span data-ttu-id="77b79-132">Una aplicación simplemente puede crear una instancia de este tipo de contexto sin pasar nada a su constructor:</span><span class="sxs-lookup"><span data-stu-id="77b79-132">An application can simply instantiate such a context without passing anything to its constructor:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="77b79-133">Este enfoque no se presta a las pruebas, a menos que las pruebas tengan como destino la base de datos completa.</span><span class="sxs-lookup"><span data-stu-id="77b79-133">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="77b79-134">Usar DbContext con la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="77b79-134">Using DbContext with dependency injection</span></span>

<span data-ttu-id="77b79-135">EF Core admite el uso de `DbContext` con un contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="77b79-135">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="77b79-136">El tipo DbContext se puede Agregar al contenedor de servicio mediante el método `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="77b79-136">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="77b79-137">`AddDbContext<TContext>` hará que el tipo DbContext, el `TContext`y el `DbContextOptions<TContext>` correspondiente estén disponibles para la inserción desde el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="77b79-137">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="77b79-138">Vea [más](#more-reading) información a continuación para obtener más información sobre la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="77b79-138">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="77b79-139">Agregar el `DbContext` a la inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="77b79-139">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="77b79-140">Esto requiere agregar un [argumento de constructor](#constructor-argument) al tipo DbContext que acepte `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="77b79-140">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="77b79-141">Código de contexto:</span><span class="sxs-lookup"><span data-stu-id="77b79-141">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="77b79-142">Código de la aplicación (en ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="77b79-142">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="77b79-143">Código de aplicación (mediante ServiceProvider directamente, menos común):</span><span class="sxs-lookup"><span data-stu-id="77b79-143">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="77b79-144">Evitar problemas de subprocesos DbContext</span><span class="sxs-lookup"><span data-stu-id="77b79-144">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="77b79-145">Entity Framework Core no admite varias operaciones en paralelo que se ejecutan en la misma instancia de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="77b79-145">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="77b79-146">Esto incluye la ejecución paralela de consultas asincrónicas y cualquier uso simultáneo explícito de varios subprocesos.</span><span class="sxs-lookup"><span data-stu-id="77b79-146">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="77b79-147">Por lo tanto, las llamadas asincrónicas siempre `await` de forma inmediata, o utilizan instancias de `DbContext` independientes para las operaciones que se ejecutan en paralelo.</span><span class="sxs-lookup"><span data-stu-id="77b79-147">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="77b79-148">Cuando EF Core detecta un intento de usar una instancia de `DbContext` simultáneamente, verá un `InvalidOperationException` con un mensaje similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="77b79-148">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span>

> <span data-ttu-id="77b79-149">Se inició una segunda operación en este contexto antes de que se completara una operación anterior.</span><span class="sxs-lookup"><span data-stu-id="77b79-149">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="77b79-150">Esto se debe normalmente a distintos subprocesos que usan la misma instancia de DbContext; sin embargo, no se garantiza que los miembros de instancia sean seguros para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="77b79-150">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="77b79-151">Cuando el acceso simultáneo no se detecta, puede dar lugar a un comportamiento indefinido, a bloqueos de la aplicación y a daños en los datos.</span><span class="sxs-lookup"><span data-stu-id="77b79-151">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="77b79-152">Hay errores comunes que pueden provocar involuntariamente el acceso simultáneo a la misma instancia de `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="77b79-152">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="77b79-153">Se olvida esperar la finalización de una operación asincrónica antes de iniciar cualquier otra operación en el mismo DbContext</span><span class="sxs-lookup"><span data-stu-id="77b79-153">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="77b79-154">Los métodos asincrónicos permiten a EF Core iniciar las operaciones que tienen acceso a la base de datos sin bloqueos.</span><span class="sxs-lookup"><span data-stu-id="77b79-154">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="77b79-155">Pero si un llamador no espera la finalización de uno de estos métodos y continúa realizando otras operaciones en el `DbContext`, el estado de `DbContext` puede ser, (y probablemente, estará dañado).</span><span class="sxs-lookup"><span data-stu-id="77b79-155">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span>

<span data-ttu-id="77b79-156">Esperar siempre EF Core métodos asincrónicos inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="77b79-156">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="77b79-157">Compartir instancias de DbContext implícitamente entre varios subprocesos mediante la inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="77b79-157">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="77b79-158">El método [de extensión `AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registra los tipos `DbContext` con una [duración de ámbito](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="77b79-158">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span>

<span data-ttu-id="77b79-159">Esto es seguro contra problemas de acceso simultáneos en ASP.NET Core aplicaciones porque solo hay un subproceso que ejecuta cada solicitud de cliente en un momento dado, y dado que cada solicitud obtiene un ámbito de inyección de dependencia independiente (y, por tanto, una instancia de `DbContext` independiente).</span><span class="sxs-lookup"><span data-stu-id="77b79-159">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="77b79-160">Sin embargo, cualquier código que ejecute explícitamente varios subprocesos en paralelo debe asegurarse de que no se tenga acceso a las instancias `DbContext` simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="77b79-160">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="77b79-161">Mediante la inserción de dependencias, esto se puede lograr registrando el contexto como ámbito y creando ámbitos (mediante `IServiceScopeFactory`) para cada subproceso, o registrando el `DbContext` como transitorio (mediante la sobrecarga de `AddDbContext` que toma un parámetro `ServiceLifetime`).</span><span class="sxs-lookup"><span data-stu-id="77b79-161">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="77b79-162">Más lecturas</span><span class="sxs-lookup"><span data-stu-id="77b79-162">More reading</span></span>

- <span data-ttu-id="77b79-163">Lea [inserción de dependencias](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para obtener más información sobre el uso de di.</span><span class="sxs-lookup"><span data-stu-id="77b79-163">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
- <span data-ttu-id="77b79-164">Lea [pruebas](testing/index.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="77b79-164">Read [Testing](testing/index.md) for more information.</span></span>
