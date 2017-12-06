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
# <a name="connection-strings"></a><span data-ttu-id="3849a-102">Cadenas de conexión</span><span class="sxs-lookup"><span data-stu-id="3849a-102">Connection Strings</span></span>

<span data-ttu-id="3849a-103">La mayoría de los proveedores de base de datos requieren algún tipo de cadena de conexión para conectarse a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3849a-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="3849a-104">A veces, esta cadena de conexión contiene información confidencial que necesita ser protegido.</span><span class="sxs-lookup"><span data-stu-id="3849a-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="3849a-105">También debe cambiar la cadena de conexión como la aplicación se desplazan entre entornos, como desarrollo, pruebas y producción.</span><span class="sxs-lookup"><span data-stu-id="3849a-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="3849a-106">Aplicaciones de .NET framework</span><span class="sxs-lookup"><span data-stu-id="3849a-106">.NET Framework Applications</span></span>

<span data-ttu-id="3849a-107">Aplicaciones de .NET framework, como formularios Windows Forms, WPF, consola y ASP.NET 4, cuentan con un patrón de la cadena de conexión probados y.</span><span class="sxs-lookup"><span data-stu-id="3849a-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="3849a-108">La cadena de conexión debe agregarse al archivo App.config de aplicaciones (Web.config si usa ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="3849a-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="3849a-109">Si la cadena de conexión contiene información confidencial, como el nombre de usuario y una contraseña, puede proteger el contenido del archivo de configuración utilizando [configuración protegida](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="3849a-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

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
> <span data-ttu-id="3849a-110">El `providerName` configuración no es necesaria en las cadenas de conexión de EF Core almacenadas en un archivo App.config, porque el proveedor de base de datos se configura a través del código.</span><span class="sxs-lookup"><span data-stu-id="3849a-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="3849a-111">A continuación, puede leer la cadena de conexión mediante el `ConfigurationManager` API en su contexto `OnConfiguring` método.</span><span class="sxs-lookup"><span data-stu-id="3849a-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="3849a-112">Puede que necesite agregar una referencia a la `System.Configuration` ensamblado de framework para poder usar esta API.</span><span class="sxs-lookup"><span data-stu-id="3849a-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="3849a-113">Plataforma universal de Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="3849a-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="3849a-114">Cadenas de conexión en una aplicación de UWP suelen ser una conexión de SQLite que solo especifica un nombre de archivo local.</span><span class="sxs-lookup"><span data-stu-id="3849a-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="3849a-115">Normalmente no contienen información confidencial y no deben cambiarse tal y como se implementa una aplicación.</span><span class="sxs-lookup"><span data-stu-id="3849a-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="3849a-116">Por lo tanto, estas cadenas de conexión son suele funcionar correctamente puede dejarse en el código, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3849a-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="3849a-117">Si desea volver a moverlos fuera del código UWP admite el concepto de configuración, consulte la [sección de configuración de la aplicación de la documentación de UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="3849a-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="3849a-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3849a-118">ASP.NET Core</span></span>

<span data-ttu-id="3849a-119">En ASP.NET Core, el sistema de configuración es muy flexible y la cadena de conexión se puede almacenar en `appsettings.json`, una variable de entorno, el almacén secreto de usuario u otro origen de configuración.</span><span class="sxs-lookup"><span data-stu-id="3849a-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="3849a-120">Consulte la [sección de configuración de la documentación de ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="3849a-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="3849a-121">En el ejemplo siguiente se muestra la cadena de conexión almacenada en `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="3849a-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="3849a-122">El contexto se configura normalmente en `Startup.cs` con la cadena de conexión que se está leyendo datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="3849a-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="3849a-123">Tenga en cuenta el `GetConnectionString()` método busca un valor de configuración cuya clave es `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="3849a-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
