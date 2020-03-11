---
title: 'Cadenas de conexión: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414096"
---
# <a name="connection-strings"></a>Cadenas de conexión

La mayoría de los proveedores de bases de datos requieren algún tipo de cadena de conexión para conectarse a la base de datos. A veces, esta cadena de conexión contiene información confidencial que debe protegerse. También es posible que necesite cambiar la cadena de conexión a medida que mueva la aplicación entre entornos, como desarrollo, pruebas y producción.

## <a name="winforms--wpf-applications"></a>Aplicaciones de WinForms & WPF

Las aplicaciones WinForms, WPF y ASP.NET 4 tienen un patrón de cadena de conexión probado y probado. La cadena de conexión debe agregarse al archivo app. config de la aplicación (Web. config si usa ASP.NET). Si la cadena de conexión contiene información confidencial, como el nombre de usuario y la contraseña, puede proteger el contenido del archivo de configuración mediante la [herramienta Administrador de secretos](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).

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
> El valor `providerName` no es necesario EF Core en las cadenas de conexión almacenadas en el archivo app. config porque el proveedor de base de datos se configura mediante código.

Después, puede leer la cadena de conexión mediante el `ConfigurationManager` API en el método de `OnConfiguring` del contexto. Es posible que tenga que agregar una referencia al ensamblado de `System.Configuration` Framework para poder usar esta API.

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

Las cadenas de conexión en una aplicación de UWP suelen ser una conexión de SQLite que solo especifica un nombre de archivo local. Normalmente no contienen información confidencial y no es necesario cambiarla a medida que se implementa una aplicación. Por lo tanto, estas cadenas de conexión suelen funcionar correctamente al dejarse en el código, como se muestra a continuación. Si quiere moverlos fuera del código, UWP admite el concepto de configuración, consulte la [sección configuración de la aplicación de la documentación de UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) para obtener más información.

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

En ASP.NET Core el sistema de configuración es muy flexible y la cadena de conexión puede almacenarse en `appsettings.json`, una variable de entorno, el almacén de secretos de usuario u otro origen de configuración. Consulte la [sección configuración de la documentación de ASP.net Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) para obtener más detalles. En el ejemplo siguiente se muestra la cadena de conexión almacenada en `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Normalmente, el contexto se configura en `Startup.cs` con la cadena de conexión que se lee de la configuración. Tenga en cuenta que el método `GetConnectionString()` busca un valor de configuración cuya clave sea `ConnectionStrings:<connection string name>`. Debe importar el espacio de nombres [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) para usar este método de extensión.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
