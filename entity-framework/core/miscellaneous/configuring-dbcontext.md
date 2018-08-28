---
title: Configuración de un elemento DbContext - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 393349c05ffaf42c6d2520e73abce23def6becc0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995943"
---
# <a name="configuring-a-dbcontext"></a>Configuración de un DbContext

En este artículo se muestra los patrones básicos para configurar un `DbContext` a través de un `DbContextOptions` para conectarse a una base de datos mediante un proveedor de EF Core específico y comportamientos opcionales.

## <a name="design-time-dbcontext-configuration"></a>Configuración de DbContext de tiempo de diseño

Tiempo de diseño EF Core herramientas como [migraciones](xref:core/managing-schemas/migrations/index) debe ser capaz de detectar y crear una instancia de trabajo de un `DbContext` tipo con el fin de recopilar detalles acerca de los tipos de entidad de la aplicación y cómo se asignan a un esquema de base de datos. Este proceso puede ser automática siempre que la herramienta puede crear fácilmente el `DbContext` de tal manera que se configurará de forma similar a cómo se podría configurar en tiempo de ejecución.

Aunque cualquier patrón que proporciona la información de configuración necesaria para la `DbContext` puede funcionar en tiempo de ejecución, las herramientas que requieren el uso de un `DbContext` solo puede funcionar en tiempo de diseño con un número limitado de patrones. Se tratan con más detalle en la [creación del contexto de tiempo de diseño](xref:core/miscellaneous/cli/dbcontext-creation) sección.

## <a name="configuring-dbcontextoptions"></a>Configuración de DbContextOptions

`DbContext` debe tener una instancia de `DbContextOptions` con el fin de realizar cualquier trabajo. El `DbContextOptions` instancia lleva información de configuración, como:

- Normalmente se selecciona el proveedor de base de datos debe usar, invocando un método como `UseSqlServer` o `UseSqlite`
- Cualquier cadena de conexión necesaria o el identificador de la instancia de base de datos, pasa normalmente como un argumento al método de selección del proveedor se ha mencionado anteriormente
- Selectores de cualquier comportamiento opcional de nivel de proveedor, normalmente también encadenados dentro de la llamada al método de selección del proveedor
- Cualquier selectores de comportamiento generales de EF Core, normalmente encadenados después o antes del método de selector de proveedor

El ejemplo siguiente configura el `DbContextOptions` para utilizar el proveedor de SQL Server, una conexión contenida en el `connectionString` variable, un tiempo de espera de comando de nivel de proveedor y un selector de comportamiento de EF Core que realiza todas las consultas ejecutadas en la `DbContext` [no realizar un seguimiento](xref:core/querying/tracking#no-tracking-queries) de forma predeterminada:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Métodos de selector de proveedor y otros métodos de selector de comportamiento mencionados anteriormente son métodos de extensión en `DbContextOptions` o clases de opciones específicas del proveedor. Para tener acceso a estos métodos de extensión, es posible que deba tener un espacio de nombres (normalmente `Microsoft.EntityFrameworkCore`) en el ámbito e incluyen las dependencias de paquetes adicionales en el proyecto.

El `DbContextOptions` puede suministrarse a la `DbContext` invalidando el `OnConfiguring` método o externamente mediante un argumento de constructor.

Si se usan ambos, `OnConfiguring` se aplica en último lugar y se puede sobrescribir las opciones proporcionadas para el argumento del constructor.

### <a name="constructor-argument"></a>Argumento de constructor

Código del contexto con el constructor:

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
> El constructor de DbContext base también acepta la versión no genérica de `DbContextOptions`, pero no se recomienda usar la versión no genérica para aplicaciones con varios tipos de contexto.

Código de aplicación para inicializar a partir del argumento de constructor:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

Código del contexto con `OnConfiguring`:

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

Código de aplicación para inicializar un `DbContext` que usa `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Este enfoque no se presta para las pruebas, a menos que la base de datos completa de las pruebas de destino.

### <a name="using-dbcontext-with-dependency-injection"></a>Uso de DbContext con inserción de dependencias

EF Core admite el uso de `DbContext` con un contenedor de inserción de dependencia. El tipo de DbContext se puede agregar al contenedor de servicios mediante el `AddDbContext<TContext>` método.

`AddDbContext<TContext>` hará que tanto el tipo de DbContext, `TContext`y la correspondiente `DbContextOptions<TContext>` disponibles para la inserción del contenedor de servicios.

Consulte [leer más](#more-reading) a continuación para obtener más información sobre la inserción de dependencia.

Agregar el `Dbcontext` para inserción de dependencias:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Esto requiere la adición de un [argumento del constructor](#constructor-argument) para el tipo de DbContext que acepta `DbContextOptions<TContext>`.

Código del contexto:

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

Código de la aplicación (mediante ServiceProvider directamente, menos común):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Leer más

* Lectura [Introducción a ASP.NET Core](../get-started/aspnetcore/index.md) para obtener más información sobre el uso de EF con ASP.NET Core.
* Lectura [inserción de dependencias](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para obtener más información sobre el uso de DI.
* Lectura [pruebas](testing/index.md) para obtener más información.
