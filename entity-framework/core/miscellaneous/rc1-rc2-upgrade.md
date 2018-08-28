---
title: Actualización de EF Core 1.0 RC1 a RC2 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996902"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="f4cfe-102">Actualización de EF Core 1.0 RC1 a 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="f4cfe-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="f4cfe-103">Este artículo proporcionan instrucciones para mover una aplicación compilada con los paquetes de RC1 a RC2.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="f4cfe-104">Las versiones y los nombres de paquete</span><span class="sxs-lookup"><span data-stu-id="f4cfe-104">Package Names and Versions</span></span>

<span data-ttu-id="f4cfe-105">Entre RC1 y RC2, se cambió de "Entity Framework 7" a "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="f4cfe-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="f4cfe-106">Puede leer más acerca de las razones del cambio en [esta publicación Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="f4cfe-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="f4cfe-107">Debido a este cambio, puede cambiar los nombres de nuestro paquete de `EntityFramework.*` a `Microsoft.EntityFrameworkCore.*` y las versiones de `7.0.0-rc1-final` a `1.0.0-rc2-final` (o `1.0.0-preview1-final` para las herramientas).</span><span class="sxs-lookup"><span data-stu-id="f4cfe-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="f4cfe-108">**Deberá quitar completamente los paquetes de RC1 y, a continuación, instale el RC2 aquellos.**</span><span class="sxs-lookup"><span data-stu-id="f4cfe-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="f4cfe-109">Aquí es la asignación de algunos paquetes comunes.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="f4cfe-110">Paquete de RC1</span><span class="sxs-lookup"><span data-stu-id="f4cfe-110">RC1 Package</span></span>                                               | <span data-ttu-id="f4cfe-111">Equivalente de RC2</span><span class="sxs-lookup"><span data-stu-id="f4cfe-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="f4cfe-112">EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="f4cfe-113">Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="f4cfe-114">EntityFramework.SQLite 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="f4cfe-115">Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="f4cfe-116">EntityFramework7.Npgsql 3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="f4cfe-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="f4cfe-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="f4cfe-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="f4cfe-118">EntityFramework.SqlServerCompact35 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="f4cfe-119">EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="f4cfe-120">EntityFramework.SqlServerCompact40 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="f4cfe-121">EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="f4cfe-122">EntityFramework.InMemory 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="f4cfe-123">Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="f4cfe-124">EntityFramework.IBMDataServer 7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="f4cfe-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="f4cfe-125">Todavía no está disponible para RC2</span><span class="sxs-lookup"><span data-stu-id="f4cfe-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="f4cfe-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="f4cfe-127">Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="f4cfe-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="f4cfe-129">Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="f4cfe-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="f4cfe-130">Espacios de nombres</span><span class="sxs-lookup"><span data-stu-id="f4cfe-130">Namespaces</span></span>

<span data-ttu-id="f4cfe-131">Junto con los nombres de paquete, los espacios de nombres ha cambiado de `Microsoft.Data.Entity.*` a `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="f4cfe-132">Puede controlar este cambio con un búsqueda y reemplazo de `using Microsoft.Data.Entity` con `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="f4cfe-133">Tabla de cambios de convención de nomenclatura</span><span class="sxs-lookup"><span data-stu-id="f4cfe-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="f4cfe-134">Un cambio significativo de funcional se tardó en RC2 consistía en utilizar el nombre de la `DbSet<TEntity>` propiedad para una entidad determinada como el nombre de tabla se asigna, en lugar de simplemente el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="f4cfe-135">Puede leer más sobre este cambio en [el problema de presentación relacionados](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="f4cfe-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="f4cfe-136">Para las aplicaciones existentes de RC1, se recomienda agregar el código siguiente al principio de su `OnModelCreating` método para mantener la estrategia de nomenclatura de RC1:</span><span class="sxs-lookup"><span data-stu-id="f4cfe-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="f4cfe-137">Si desea adoptar la nueva estrategia de nomenclatura, recomendamos correctamente cambia el nombre de completar el resto de los pasos de actualización y, a continuación, quitar el código y crear una migración para aplicar la tabla.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="f4cfe-138">AddDbContext / Startup.cs cambia (solo para proyectos de ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="f4cfe-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="f4cfe-139">En RC1, tenía que agregar servicios de Entity Framework para el proveedor de servicios de aplicación: en `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="f4cfe-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="f4cfe-140">En RC2, puede quitar las llamadas a `AddEntityFramework()`, `AddSqlServer()`, etcetera.:</span><span class="sxs-lookup"><span data-stu-id="f4cfe-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="f4cfe-141">También deberá agregar un constructor, a su contexto derivado, que toma opciones de contexto y los pasa al constructor base.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="f4cfe-142">Esto es necesario porque se ha quitado de la magia de miedo que hurtadillas en segundo plano:</span><span class="sxs-lookup"><span data-stu-id="f4cfe-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="f4cfe-143">Pasar un IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="f4cfe-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="f4cfe-144">Si tiene código RC1 que pasa un `IServiceProvider` al contexto, esto se ha movido a `DbContextOptions`, en lugar de ser un parámetro de constructor independiente.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="f4cfe-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` para establecer el proveedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="f4cfe-146">Pruebas</span><span class="sxs-lookup"><span data-stu-id="f4cfe-146">Testing</span></span>

<span data-ttu-id="f4cfe-147">Era controlar el ámbito de una base de datos InMemory al probar el escenario más común para realizar esta acción.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="f4cfe-148">Consulte la actualización [pruebas](testing/index.md) artículo para obtener un ejemplo de hacerlo con RC2.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="f4cfe-149">Resolver los servicios internos de proveedor de servicios de aplicación (solo para proyectos de ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="f4cfe-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="f4cfe-150">Si tiene una aplicación de ASP.NET Core y desea que EF para resolver servicios internos del proveedor de servicios de aplicación, hay una sobrecarga de `AddDbContext` que le permite configurar esto:</span><span class="sxs-lookup"><span data-stu-id="f4cfe-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="f4cfe-151">Se recomienda permitir que EF internamente administrar sus propios servicios, a menos que tenga un motivo para combinar los servicios internos de EF en su proveedor de servicios de aplicación.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="f4cfe-152">La razón principal que desea hacer esto consiste en usar el proveedor de servicios de aplicación para reemplazar los servicios que EF usa internamente</span><span class="sxs-lookup"><span data-stu-id="f4cfe-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="f4cfe-153">Los comandos de DNX = > CLI de .NET (solo para proyectos de ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="f4cfe-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="f4cfe-154">Si anteriormente usaba la `dnx ef` comandos para los proyectos ASP.NET 5, estos se han movido al `dotnet ef` comandos.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="f4cfe-155">Todavía se aplica la misma sintaxis del comando.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-155">The same command syntax still applies.</span></span> <span data-ttu-id="f4cfe-156">Puede usar `dotnet ef --help` para obtener información de sintaxis.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="f4cfe-157">Ha cambiado la manera en que se registran los comandos en RC2, debido a que será reemplazada por la CLI de .NET de DNX.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="f4cfe-158">Los comandos están registrados en un `tools` sección `project.json`:</span><span class="sxs-lookup"><span data-stu-id="f4cfe-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> <span data-ttu-id="f4cfe-159">Si usa Visual Studio, ahora puede usar la consola de administrador de paquetes para ejecutar comandos EF para proyectos de ASP.NET Core (Esto no se admite en RC1).</span><span class="sxs-lookup"><span data-stu-id="f4cfe-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="f4cfe-160">Deberá registrar los comandos en el `tools` sección de `project.json` para hacerlo.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="f4cfe-161">Comandos del Administrador de paquetes requiere PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="f4cfe-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="f4cfe-162">Si usa los comandos de Entity Framework en la consola de administrador de paquetes en Visual Studio, deberá asegurarse de que tener instalado el 5 de PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="f4cfe-163">Este es un requisito temporal que se quitará en la próxima versión (consulte [emitir #5327](https://github.com/aspnet/EntityFramework/issues/5327) para obtener más detalles).</span><span class="sxs-lookup"><span data-stu-id="f4cfe-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="f4cfe-164">Uso de "importaciones" en project.json</span><span class="sxs-lookup"><span data-stu-id="f4cfe-164">Using "imports" in project.json</span></span>

<span data-ttu-id="f4cfe-165">Algunas de las dependencias de EF Core aún no se admite .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="f4cfe-166">EF Core en proyectos de .NET Standard y .NET Core puede requerir agregar "importaciones" a project.json como solución temporal.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="f4cfe-167">Al agregar EF, restauración de NuGet mostrará este mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="f4cfe-167">When adding EF, NuGet restore will display this error message:</span></span>

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

<span data-ttu-id="f4cfe-168">La solución consiste en importar manualmente el perfil portable "portable-net451 + win8".</span><span class="sxs-lookup"><span data-stu-id="f4cfe-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="f4cfe-169">Esta fuerza que NuGet para tratar este archivos binarios que coinciden con esta proporciona como un marco de trabajo compatible con .NET Standard, aunque no estén.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="f4cfe-170">Aunque no es 100% compatible con .NET Standard "portable-net451 + win8", es lo suficientemente compatible para la transición de PCL a .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="f4cfe-171">Las importaciones se pueden quitar cuando las dependencias de EF, finalmente, actualización a .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="f4cfe-172">Varios marcos de trabajo se pueden agregar a "importaciones" en la sintaxis de la matriz.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="f4cfe-173">Otras importaciones pueden ser necesarios si agrega bibliotecas adicionales a su proyecto.</span><span class="sxs-lookup"><span data-stu-id="f4cfe-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="f4cfe-174">Consulte [emitir #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="f4cfe-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
