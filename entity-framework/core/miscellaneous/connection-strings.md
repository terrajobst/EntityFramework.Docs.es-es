---
title: Cadenas de conexión - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: c306f9ca7a51fc9e3db18e883fd44f56dd1a3cb4
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286448"
---
# <a name="connection-strings"></a>Cadenas de conexión

La mayoría de los proveedores de base de datos requiere algún tipo de cadena de conexión para conectarse a la base de datos. A veces, esta cadena de conexión contiene información confidencial que debe protegerse. También es posible que deba cambiar la cadena de conexión como mover la aplicación entre entornos, como desarrollo, pruebas y producción.

## <a name="net-framework-applications"></a>Aplicaciones de .NET framework

Aplicaciones de .NET framework, como WPF, WinForms, consola y ASP.NET 4, tienen un patrón de cadena de conexión probadas y comprobadas. La cadena de conexión debe agregarse al archivo App.config de la aplicación (Web.config si está utilizando ASP.NET). Si la cadena de conexión contiene información confidencial, como el nombre de usuario y contraseña, puede proteger el contenido del archivo de configuración utilizando [configuración protegida](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> El `providerName` configuración no es necesaria en las cadenas de conexión de EF Core almacenadas en el archivo App.config porque se configura el proveedor de base de datos a través del código.

A continuación, puede leer la cadena de conexión mediante el `ConfigurationManager` API en su contexto `OnConfiguring` método. Es posible que deba agregar una referencia a la `System.Configuration` ensamblado de marco de trabajo para poder usar esta API.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="universal-windows-platform-uwp"></a>Plataforma universal de Windows (UWP)

Las cadenas de conexión en una aplicación de UWP suelen ser una conexión de SQLite que solo especifica un nombre de archivo local. Normalmente no contienen información confidencial y no deben cambiarse según se implementa una aplicación. Por lo tanto, estas cadenas de conexión suelen funcionar correctamente al dejarse en el código, como se muestra a continuación. Si desea moverlas fuera del código UWP admite el concepto de configuración, consulte el [sección de configuración de la aplicación de la documentación de UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) para obtener más información.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a>ASP.NET Core

En ASP.NET Core, el sistema de configuración es muy flexible y la cadena de conexión se pueden almacenar en `appsettings.json`, una variable de entorno, el almacén secreto de usuario u otro origen de configuración. Consulte la [sección de configuración de la documentación de ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) para obtener más detalles. El ejemplo siguiente muestra la cadena de conexión almacenada en `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Normalmente se configura en el contexto `Startup.cs` con la cadena de conexión que se leen de la configuración. Tenga en cuenta la `GetConnectionString()` método busca un valor de configuración cuya clave es `ConnectionStrings:<connection string name>`. Debe importar el [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) espacio de nombres para utilizar este método de extensión.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
