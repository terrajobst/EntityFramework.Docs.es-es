---
title: "Cadenas de conexión - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: b4ed01f0452d74ac49d3fde780caa5f1b25a6e97
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="connection-strings"></a>Cadenas de conexión

La mayoría de los proveedores de base de datos requieren algún tipo de cadena de conexión para conectarse a la base de datos. A veces, esta cadena de conexión contiene información confidencial que necesita ser protegido. También debe cambiar la cadena de conexión como la aplicación se desplazan entre entornos, como desarrollo, pruebas y producción.

## <a name="net-framework-applications"></a>Aplicaciones de .NET framework

Aplicaciones de .NET framework, como formularios Windows Forms, WPF, consola y ASP.NET 4, cuentan con un patrón de la cadena de conexión probados y. La cadena de conexión debe agregarse al archivo App.config de aplicaciones (Web.config si usa ASP.NET). Si la cadena de conexión contiene información confidencial, como el nombre de usuario y una contraseña, puede proteger el contenido del archivo de configuración utilizando [configuración protegida](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> El `providerName` configuración no es necesaria en las cadenas de conexión de EF Core almacenadas en un archivo App.config, porque el proveedor de base de datos se configura a través del código.

A continuación, puede leer la cadena de conexión mediante el `ConfigurationManager` API en su contexto `OnConfiguring` método. Puede que necesite agregar una referencia a la `System.Configuration` ensamblado de framework para poder usar esta API.

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

Cadenas de conexión en una aplicación de UWP suelen ser una conexión de SQLite que solo especifica un nombre de archivo local. Normalmente no contienen información confidencial y no deben cambiarse tal y como se implementa una aplicación. Por lo tanto, estas cadenas de conexión son suele funcionar correctamente puede dejarse en el código, tal y como se muestra a continuación. Si desea volver a moverlos fuera del código UWP admite el concepto de configuración, consulte la [sección de configuración de la aplicación de la documentación de UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) para obtener más información.

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

En ASP.NET Core, el sistema de configuración es muy flexible y la cadena de conexión se puede almacenar en `appsettings.json`, una variable de entorno, el almacén secreto de usuario u otro origen de configuración. Consulte la [sección de configuración de la documentación de ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) para obtener más detalles. En el ejemplo siguiente se muestra la cadena de conexión almacenada en `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

El contexto se configura normalmente en `Startup.cs` con la cadena de conexión que se está leyendo datos de configuración. Tenga en cuenta el `GetConnectionString()` método busca un valor de configuración cuya clave es `ConnectionStrings:<connection string name>`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
