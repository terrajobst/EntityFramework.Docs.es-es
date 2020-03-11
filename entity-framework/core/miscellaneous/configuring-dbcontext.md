---
title: Configuración de DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 3ab90d46b7a4476044e5ea38eaf04f995708e7bf
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414162"
---
# <a name="configuring-a-dbcontext"></a>Configuración de un DbContext

En este artículo se muestran los patrones básicos para configurar un `DbContext` a través de un `DbContextOptions` para conectarse a una base de datos mediante un proveedor de EF Core específico y comportamientos opcionales.

## <a name="design-time-dbcontext-configuration"></a>Configuración de DbContext en tiempo de diseño

EF Core herramientas en tiempo de diseño como las [migraciones](xref:core/managing-schemas/migrations/index) deben ser capaces de detectar y crear una instancia de trabajo de un tipo de `DbContext` para recopilar detalles sobre los tipos de entidad de la aplicación y cómo se asignan a un esquema de base de datos. Este proceso puede ser automático siempre y cuando la herramienta pueda crear fácilmente el `DbContext` de manera que se configurará de forma similar a como se configuraría en tiempo de ejecución.

Aunque cualquier patrón que proporcione la información de configuración necesaria para el `DbContext` puede funcionar en tiempo de ejecución, las herramientas que requieren el uso de un `DbContext` en tiempo de diseño solo pueden funcionar con un número limitado de patrones. Estos se describen con más detalle en la sección [creación de contexto en tiempo de diseño](xref:core/miscellaneous/cli/dbcontext-creation) .

## <a name="configuring-dbcontextoptions"></a>Configuración de DbContextOptions

`DbContext` debe tener una instancia de `DbContextOptions` para poder realizar cualquier trabajo. La instancia de `DbContextOptions` contiene información de configuración como:

- Proveedor de base de datos que se va a usar, normalmente seleccionado mediante la invocación de un método como `UseSqlServer` o `UseSqlite`. Estos métodos de extensión requieren el paquete de proveedor correspondiente, como `Microsoft.EntityFrameworkCore.SqlServer` o `Microsoft.EntityFrameworkCore.Sqlite`. Los métodos se definen en el espacio de nombres `Microsoft.EntityFrameworkCore`.
- Cualquier cadena de conexión o identificador de la instancia de base de datos que sea necesario, normalmente se pasa como argumento al método de selección de proveedor mencionado anteriormente
- Cualquier selector de comportamiento opcional de nivel de proveedor, normalmente también encadenado dentro de la llamada al método de selección de proveedor.
- Los selectores de comportamiento de EF Core general, normalmente encadenados después o antes del método de selector de proveedor

En el ejemplo siguiente se configura el `DbContextOptions` para utilizar el proveedor de SQL Server, una conexión contenida en la variable `connectionString`, un tiempo de espera de comando de nivel de proveedor y un selector de comportamiento de EF Core que hace que todas las consultas ejecutadas en el `DbContext` [sin seguimiento](xref:core/querying/tracking#no-tracking-queries) de forma predeterminada:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Los métodos de selector de proveedor y otros métodos de selector de comportamiento mencionados anteriormente son métodos de extensión en `DbContextOptions` o clases de opciones específicas del proveedor. Para tener acceso a estos métodos de extensión, puede que necesite tener un espacio de nombres (normalmente `Microsoft.EntityFrameworkCore`) en el ámbito e incluir dependencias de paquetes adicionales en el proyecto.

El `DbContextOptions` se puede proporcionar al `DbContext` invalidando el método de `OnConfiguring` o externamente a través de un argumento de constructor.

Si se usan ambos, `OnConfiguring` se aplica en último lugar y puede sobrescribir las opciones proporcionadas al argumento del constructor.

### <a name="constructor-argument"></a>Argumento de constructor

El constructor simplemente puede aceptar un `DbContextOptions` como se indica a continuación:

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
> El constructor base de DbContext también acepta la versión no genérica de `DbContextOptions`, pero no se recomienda usar la versión no genérica para las aplicaciones con varios tipos de contexto.

Ahora la aplicación puede pasar el `DbContextOptions` al crear una instancia de un contexto, como se indica a continuación:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Configuración

También puede inicializar la `DbContextOptions` dentro del propio contexto. Aunque puede usar esta técnica para la configuración básica, normalmente tendrá que obtener determinados detalles de configuración del exterior, por ejemplo, una cadena de conexión de base de datos. Esto puede hacerse con una API de configuración o con cualquier otro medio.

Para inicializar `DbContextOptions` dentro del contexto, invalide el método `OnConfiguring` y llame a los métodos en el `DbContextOptionsBuilder`proporcionado:

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

Una aplicación simplemente puede crear una instancia de este tipo de contexto sin pasar nada a su constructor:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Este enfoque no se presta a las pruebas, a menos que las pruebas tengan como destino la base de datos completa.

### <a name="using-dbcontext-with-dependency-injection"></a>Usar DbContext con la inserción de dependencias

EF Core admite el uso de `DbContext` con un contenedor de inserción de dependencias. El tipo DbContext se puede Agregar al contenedor de servicio mediante el método `AddDbContext<TContext>`.

`AddDbContext<TContext>` hará que el tipo DbContext, el `TContext`y el `DbContextOptions<TContext>` correspondiente estén disponibles para la inserción desde el contenedor de servicios.

Vea [más](#more-reading) información a continuación para obtener más información sobre la inserción de dependencias.

Agregar el `DbContext` a la inserción de dependencias:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Esto requiere agregar un [argumento de constructor](#constructor-argument) al tipo DbContext que acepte `DbContextOptions<TContext>`.

Código de contexto:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Código de la aplicación (en ASP.NET Core):

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

Código de aplicación (mediante ServiceProvider directamente, menos común):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a>Evitar problemas de subprocesos DbContext

Entity Framework Core no admite varias operaciones paralelas que se ejecutan en la misma instancia de `DbContext`. Esto incluye la ejecución paralela de consultas asincrónicas y cualquier uso simultáneo explícito de varios subprocesos. Por lo tanto, siempre `await` llamadas asincrónicas inmediatamente, o bien usar instancias de `DbContext` independientes para las operaciones que se ejecutan en paralelo.

Cuando EF Core detecta un intento de usar una instancia de `DbContext` simultáneamente, verá un `InvalidOperationException` con un mensaje similar al siguiente:

> Se inició una segunda operación en este contexto antes de que se completara una operación anterior. Esto se debe normalmente a distintos subprocesos que usan la misma instancia de DbContext; sin embargo, no se garantiza que los miembros de instancia sean seguros para subprocesos.

Cuando el acceso simultáneo no se detecta, puede dar lugar a un comportamiento indefinido, a bloqueos de la aplicación y a daños en los datos.

Hay errores comunes que pueden provocar involuntariamente el acceso simultáneo en la misma instancia de `DbContext`:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Se olvida esperar la finalización de una operación asincrónica antes de iniciar cualquier otra operación en el mismo DbContext

Los métodos asincrónicos permiten a EF Core iniciar las operaciones que tienen acceso a la base de datos sin bloqueos. Pero si el autor de la llamada no espera la finalización de uno de estos métodos y continúa realizando otras operaciones en el `DbContext`, el estado de la `DbContext` puede ser (y probablemente estará) dañado.

Esperar siempre EF Core métodos asincrónicos inmediatamente.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Compartir instancias de DbContext implícitamente entre varios subprocesos mediante la inserción de dependencias

El método de extensión [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registra los tipos de `DbContext` con una [duración de ámbito](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) de forma predeterminada.

Esto es seguro contra problemas de acceso simultáneos en ASP.NET Core aplicaciones porque solo hay un subproceso que ejecuta cada solicitud de cliente en un momento dado, y dado que cada solicitud obtiene un ámbito de inyección de dependencia independiente (y, por tanto, una instancia de `DbContext` independiente).

Sin embargo, cualquier código que ejecute explícitamente varios subprocesos en paralelo debe asegurarse de que no se tenga acceso a las instancias de `DbContext` simultáneamente.

Mediante la inserción de dependencias, esto se puede lograr registrando el contexto como ámbito y creando ámbitos (mediante `IServiceScopeFactory`) para cada subproceso, o registrando el `DbContext` como transitorio (mediante la sobrecarga de `AddDbContext` que toma un parámetro `ServiceLifetime`).

## <a name="more-reading"></a>Más lecturas

- Lea [inserción de dependencias](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para obtener más información sobre el uso de di.
- Lea [pruebas](testing/index.md) para obtener más información.
