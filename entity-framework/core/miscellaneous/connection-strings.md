---
title: 'Cadenas de conexión: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149114"
---
# <a name="connection-strings"></a><span data-ttu-id="bc029-102">Cadenas de conexión</span><span class="sxs-lookup"><span data-stu-id="bc029-102">Connection Strings</span></span>

<span data-ttu-id="bc029-103">La mayoría de los proveedores de bases de datos requieren algún tipo de cadena de conexión para conectarse a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bc029-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="bc029-104">A veces, esta cadena de conexión contiene información confidencial que debe protegerse.</span><span class="sxs-lookup"><span data-stu-id="bc029-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="bc029-105">También es posible que necesite cambiar la cadena de conexión a medida que mueva la aplicación entre entornos, como desarrollo, pruebas y producción.</span><span class="sxs-lookup"><span data-stu-id="bc029-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="winforms--wpf-applications"></a><span data-ttu-id="bc029-106">Aplicaciones de WinForms & WPF</span><span class="sxs-lookup"><span data-stu-id="bc029-106">WinForms & WPF Applications</span></span>

<span data-ttu-id="bc029-107">Las aplicaciones WinForms, WPF y ASP.NET 4 tienen un patrón de cadena de conexión probado y probado.</span><span class="sxs-lookup"><span data-stu-id="bc029-107">WinForms, WPF, and ASP.NET 4 applications have a tried and tested connection string pattern.</span></span> <span data-ttu-id="bc029-108">La cadena de conexión debe agregarse al archivo app. config de la aplicación (Web. config si usa ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="bc029-108">The connection string should be added to your application's App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="bc029-109">Si la cadena de conexión contiene información confidencial, como el nombre de usuario y la contraseña, puede proteger el contenido del archivo de configuración mediante la [herramienta Administrador de secretos](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="bc029-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span></span>

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
> <span data-ttu-id="bc029-110">La `providerName` configuración no es necesaria en EF Core cadenas de conexión almacenadas en el archivo app. config porque el proveedor de base de datos se configura mediante código.</span><span class="sxs-lookup"><span data-stu-id="bc029-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="bc029-111">Después, puede leer la cadena de conexión con `ConfigurationManager` la API en el método `OnConfiguring` del contexto.</span><span class="sxs-lookup"><span data-stu-id="bc029-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="bc029-112">Es posible que tenga que agregar una referencia al `System.Configuration` ensamblado de .NET Framework para poder usar esta API.</span><span class="sxs-lookup"><span data-stu-id="bc029-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="bc029-113">Plataforma universal de Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="bc029-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="bc029-114">Las cadenas de conexión en una aplicación de UWP suelen ser una conexión de SQLite que solo especifica un nombre de archivo local.</span><span class="sxs-lookup"><span data-stu-id="bc029-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="bc029-115">Normalmente no contienen información confidencial y no es necesario cambiarla a medida que se implementa una aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc029-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="bc029-116">Por lo tanto, estas cadenas de conexión suelen funcionar correctamente al dejarse en el código, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="bc029-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="bc029-117">Si quiere moverlos fuera del código, UWP admite el concepto de configuración, consulte la [sección configuración de la aplicación de la documentación de UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="bc029-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="bc029-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc029-118">ASP.NET Core</span></span>

<span data-ttu-id="bc029-119">En ASP.net Core el sistema de configuración es muy flexible y la cadena de conexión podría almacenarse `appsettings.json`en, una variable de entorno, el almacén de secretos de usuario u otro origen de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc029-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="bc029-120">Consulte la [sección configuración de la documentación de ASP.net Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="bc029-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="bc029-121">En el ejemplo siguiente se muestra la cadena de `appsettings.json`conexión almacenada en.</span><span class="sxs-lookup"><span data-stu-id="bc029-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="bc029-122">Normalmente, el contexto se configura `Startup.cs` en con la cadena de conexión que se lee de la configuración.</span><span class="sxs-lookup"><span data-stu-id="bc029-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="bc029-123">Tenga en `GetConnectionString()` cuenta que el método busca un valor de configuración `ConnectionStrings:<connection string name>`cuya clave sea.</span><span class="sxs-lookup"><span data-stu-id="bc029-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="bc029-124">Debe importar el espacio de nombres [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) para usar este método de extensión.</span><span class="sxs-lookup"><span data-stu-id="bc029-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
