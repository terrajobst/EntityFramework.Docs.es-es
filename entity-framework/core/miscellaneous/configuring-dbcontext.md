---
title: Configurar un DbContext - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152395"
---
# <a name="configuring-a-dbcontext"></a>Configurar un DbContext

Este artículo muestra los patrones básicos para configurar un `DbContext` a través de un `DbContextOptions` para conectarse a una base de datos mediante un proveedor de EF principales específico y comportamientos opcionales.

## <a name="design-time-dbcontext-configuration"></a>Configuración de DbContext de tiempo de diseño

Tiempo de diseño EF principales herramientas como [migraciones](xref:core/managing-schemas/migrations/index) necesita ser capaz de detectar y crear una instancia de trabajo de un `DbContext` tipo con el fin de obtener detalles sobre los tipos de entidad de la aplicación y cómo se asignan a un esquema de base de datos. Este proceso puede ser automática siempre que la herramienta puede crear fácilmente el `DbContext` de tal manera que se configurará con forma similar a cómo se puede configurar en tiempo de ejecución.

Mientras cualquier patrón que proporciona la información de configuración necesaria para la `DbContext` puede trabajar en tiempo de ejecución, herramientas que requieren el uso de un `DbContext` en tiempo de diseño sólo puede trabajar con un número limitado de patrones. Se tratan con más detalle en la [creación del contexto de tiempo de diseño](xref:core/miscellaneous/cli/dbcontext-creation) sección.

## <a name="configuring-dbcontextoptions"></a>Configurar DbContextOptions

`DbContext` debe tener una instancia de `DbContextOptions` para realizar cualquier trabajo. El `DbContextOptions` instancia lleva información de configuración como:

- El proveedor de base de datos que se utiliza normalmente selecciona invocando un método como `UseSqlServer` o `UseSqlite`
- Cualquier cadena de conexión necesaria o el identificador de la instancia de base de datos, pasa normalmente como un argumento para el método de selección de proveedor mencionado anteriormente
- Los selectores de comportamiento opcional de nivel del proveedor, normalmente también encadenados dentro de la llamada al método de selección de proveedor
- Los selectores de comportamiento EF Core generales normalmente encadenados después o antes del método de selector de proveedor

En el ejemplo siguiente se configura el `DbContextOptions` para usar el proveedor de SQL Server, una conexión incluidos en el `connectionString` variable, un tiempo de espera de comando de nivel del proveedor y un selector de comportamiento de EF Core que hace que todas las consultas ejecutadas en la `DbContext` [seguimiento no](xref:core/querying/tracking#no-tracking-queries) de forma predeterminada:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Métodos de selector de proveedor y otros métodos de selector de comportamiento mencionados anteriormente son métodos de extensión en `DbContextOptions` o clases de opciones específicas del proveedor. Para tener acceso a estos métodos de extensión debe tener un espacio de nombres (normalmente `Microsoft.EntityFrameworkCore`) en el ámbito e incluir las dependencias del paquete adicional en el proyecto.

El `DbContextOptions` pueden proporcionar a la `DbContext` invalidando el `OnConfiguring` método o externamente a través de un argumento de constructor.

Si se utilizan ambos, `OnConfiguring` se aplica en último lugar y pueden sobrescribir opciones proporcionadas para el argumento del constructor.

### <a name="constructor-argument"></a>Argumentos de constructor

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
> El constructor base de DbContext acepta también la versión no genérica de `DbContextOptions`, pero no se recomienda utilizar la versión no genérica para aplicaciones con varios tipos de contexto.

Código de la aplicación para inicializar de argumentos de constructor:

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

Código de la aplicación para inicializar un `DbContext` que utiliza `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Este enfoque no se presta a las pruebas, a menos que la base de datos completa de las pruebas de destino.

### <a name="using-dbcontext-with-dependency-injection"></a>Uso de DbContext con inyección de dependencia

Núcleo EF admite el uso de `DbContext` con un contenedor de inyección de dependencia. El tipo de DbContext se puede agregar al contenedor de servicios mediante la `AddDbContext<TContext>` método.

`AddDbContext<TContext>` hará que tanto el tipo de DbContext, `TContext`y la correspondiente `DbContextOptions<TContext>` disponibles para la inyección del contenedor de servicios.

Vea [leer más](#more-reading) a continuación para obtener más información sobre la inserción de dependencias.

Agregar el `Dbcontext` a la inyección de dependencia:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Esto es necesario agregar una [argumento del constructor](#constructor-argument) a su tipo de DbContext que acepta `DbContextOptions<TContext>`.

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
* Lectura [inyección de dependencia](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) para obtener información adicional acerca del uso DI.
* Lectura [pruebas](testing/index.md) para obtener más información.
