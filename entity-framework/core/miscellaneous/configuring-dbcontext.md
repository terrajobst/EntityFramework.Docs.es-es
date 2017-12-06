---
title: Configurar un DbContext - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a>Configurar un DbContext

En este artículo se muestra patrones para configurar un `DbContext` con `DbContextOptions`. Opciones se utilizan principalmente para seleccionar y configurar el almacén de datos.

## <a name="configuring-dbcontextoptions"></a>Configurar DbContextOptions

`DbContext`debe tener una instancia de `DbContextOptions` para ejecutarse. Esto se puede configurar mediante la invalidación `OnConfiguring`, o externamente proporcionado a través de un argumento de constructor.

Si se utilizan ambos, `OnConfiguring` se ejecuta en las opciones proporcionadas, lo que significa se suma y puede sobrescribir opciones proporcionadas para el argumento del constructor.

### <a name="constructor-argument"></a>Argumentos de constructor

Código del contexto con el constructor

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
> El constructor base de DbContext acepta también la versión no genérica de `DbContextOptions`. No se recomienda usar la versión no genérica para aplicaciones con varios tipos de contexto.

Código de aplicación para inicializar desde el argumento del constructor

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

> [!WARNING]  
> `OnConfiguring`se produce una último y pueden sobrescribir opciones obtenidos desde el constructor o DI. Este enfoque no se presta a las pruebas (a menos que el destino es la base de datos completa).

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

Código de la aplicación para inicializar con `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a>Uso de DbContext con inyección de dependencia

EF admite el uso de `DbContext` con un contenedor de inyección de dependencia. El tipo de DbContext se puede agregar al contenedor de servicios mediante `AddDbContext<TContext>`.

`AddDbContext`hará que tanto el tipo de DbContext, `TContext`, y `DbContextOptions<TContext>` disponibles para la inyección del contenedor de servicios.

Vea [leer más](#more-reading) a continuación para obtener información sobre la inserción de dependencias.

Agregar dbcontext a la inyección de dependencia

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Esto es necesario agregar una [argumento del constructor](#constructor-argument) a su tipo de DbContext que acepta `DbContextOptions`.

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
public MyController(BloggingContext context)
```

Código de la aplicación (mediante ServiceProvider directamente, menos común):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a>Uso de`IDesignTimeDbContextFactory<TContext>`

Como alternativa a las opciones anteriores, también puede proporcionar una implementación de `IDesignTimeDbContextFactory<TContext>`. Herramientas EF pueden utilizar este generador para crear una instancia de su DbContext. Esto puede ser necesario para permitir que las experiencias de tiempo de diseño específicas, como las migraciones.

Implemente esta interfaz para habilitar los servicios de tiempo de diseño para los tipos de contexto que no tiene un constructor predeterminado público. Servicios en tiempo de diseño detecta automáticamente implementaciones de esta interfaz que se encuentran en el mismo ensamblado que el contexto derivado.

Ejemplo:

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a>Leer más

* Lectura [Introducción a ASP.NET Core](../get-started/aspnetcore/index.md) para obtener más información sobre el uso de EF con ASP.NET Core.
* Lectura [inyección de dependencia](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) para obtener información adicional acerca del uso DI.
* Lectura [pruebas](testing/index.md) para obtener más información.
