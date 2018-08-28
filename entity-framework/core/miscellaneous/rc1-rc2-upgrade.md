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
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Actualización de EF Core 1.0 RC1 a 1.0 RC2

Este artículo proporcionan instrucciones para mover una aplicación compilada con los paquetes de RC1 a RC2.

## <a name="package-names-and-versions"></a>Las versiones y los nombres de paquete

Entre RC1 y RC2, se cambió de "Entity Framework 7" a "Entity Framework Core". Puede leer más acerca de las razones del cambio en [esta publicación Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Debido a este cambio, puede cambiar los nombres de nuestro paquete de `EntityFramework.*` a `Microsoft.EntityFrameworkCore.*` y las versiones de `7.0.0-rc1-final` a `1.0.0-rc2-final` (o `1.0.0-preview1-final` para las herramientas).

**Deberá quitar completamente los paquetes de RC1 y, a continuación, instale el RC2 aquellos.** Aquí es la asignación de algunos paquetes comunes.

| Paquete de RC1                                               | Equivalente de RC2                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final      |
| EntityFramework.SQLite 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final      |
| EntityFramework7.Npgsql 3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final      |
| EntityFramework.InMemory 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final      |
| EntityFramework.IBMDataServer 7.0.0-beta1     | Todavía no está disponible para RC2                                            |
| EntityFramework.Commands 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final      |

## <a name="namespaces"></a>Espacios de nombres

Junto con los nombres de paquete, los espacios de nombres ha cambiado de `Microsoft.Data.Entity.*` a `Microsoft.EntityFrameworkCore.*`. Puede controlar este cambio con un búsqueda y reemplazo de `using Microsoft.Data.Entity` con `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Tabla de cambios de convención de nomenclatura

Un cambio significativo de funcional se tardó en RC2 consistía en utilizar el nombre de la `DbSet<TEntity>` propiedad para una entidad determinada como el nombre de tabla se asigna, en lugar de simplemente el nombre de clase. Puede leer más sobre este cambio en [el problema de presentación relacionados](https://github.com/aspnet/Announcements/issues/167).

Para las aplicaciones existentes de RC1, se recomienda agregar el código siguiente al principio de su `OnModelCreating` método para mantener la estrategia de nomenclatura de RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Si desea adoptar la nueva estrategia de nomenclatura, recomendamos correctamente cambia el nombre de completar el resto de los pasos de actualización y, a continuación, quitar el código y crear una migración para aplicar la tabla.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs cambia (solo para proyectos de ASP.NET Core)

En RC1, tenía que agregar servicios de Entity Framework para el proveedor de servicios de aplicación: en `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

En RC2, puede quitar las llamadas a `AddEntityFramework()`, `AddSqlServer()`, etcetera.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

También deberá agregar un constructor, a su contexto derivado, que toma opciones de contexto y los pasa al constructor base. Esto es necesario porque se ha quitado de la magia de miedo que hurtadillas en segundo plano:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Pasar un IServiceProvider

Si tiene código RC1 que pasa un `IServiceProvider` al contexto, esto se ha movido a `DbContextOptions`, en lugar de ser un parámetro de constructor independiente. Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` para establecer el proveedor de servicios.

### <a name="testing"></a>Pruebas

Era controlar el ámbito de una base de datos InMemory al probar el escenario más común para realizar esta acción. Consulte la actualización [pruebas](testing/index.md) artículo para obtener un ejemplo de hacerlo con RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Resolver los servicios internos de proveedor de servicios de aplicación (solo para proyectos de ASP.NET Core)

Si tiene una aplicación de ASP.NET Core y desea que EF para resolver servicios internos del proveedor de servicios de aplicación, hay una sobrecarga de `AddDbContext` que le permite configurar esto:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Se recomienda permitir que EF internamente administrar sus propios servicios, a menos que tenga un motivo para combinar los servicios internos de EF en su proveedor de servicios de aplicación. La razón principal que desea hacer esto consiste en usar el proveedor de servicios de aplicación para reemplazar los servicios que EF usa internamente

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Los comandos de DNX = > CLI de .NET (solo para proyectos de ASP.NET Core)

Si anteriormente usaba la `dnx ef` comandos para los proyectos ASP.NET 5, estos se han movido al `dotnet ef` comandos. Todavía se aplica la misma sintaxis del comando. Puede usar `dotnet ef --help` para obtener información de sintaxis.

Ha cambiado la manera en que se registran los comandos en RC2, debido a que será reemplazada por la CLI de .NET de DNX. Los comandos están registrados en un `tools` sección `project.json`:

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
> Si usa Visual Studio, ahora puede usar la consola de administrador de paquetes para ejecutar comandos EF para proyectos de ASP.NET Core (Esto no se admite en RC1). Deberá registrar los comandos en el `tools` sección de `project.json` para hacerlo.

## <a name="package-manager-commands-require-powershell-5"></a>Comandos del Administrador de paquetes requiere PowerShell 5

Si usa los comandos de Entity Framework en la consola de administrador de paquetes en Visual Studio, deberá asegurarse de que tener instalado el 5 de PowerShell. Este es un requisito temporal que se quitará en la próxima versión (consulte [emitir #5327](https://github.com/aspnet/EntityFramework/issues/5327) para obtener más detalles).

## <a name="using-imports-in-projectjson"></a>Uso de "importaciones" en project.json

Algunas de las dependencias de EF Core aún no se admite .NET Standard. EF Core en proyectos de .NET Standard y .NET Core puede requerir agregar "importaciones" a project.json como solución temporal.

Al agregar EF, restauración de NuGet mostrará este mensaje de error:

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

La solución consiste en importar manualmente el perfil portable "portable-net451 + win8". Esta fuerza que NuGet para tratar este archivos binarios que coinciden con esta proporciona como un marco de trabajo compatible con .NET Standard, aunque no estén. Aunque no es 100% compatible con .NET Standard "portable-net451 + win8", es lo suficientemente compatible para la transición de PCL a .NET Standard. Las importaciones se pueden quitar cuando las dependencias de EF, finalmente, actualización a .NET Standard.

Varios marcos de trabajo se pueden agregar a "importaciones" en la sintaxis de la matriz. Otras importaciones pueden ser necesarios si agrega bibliotecas adicionales a su proyecto.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Consulte [emitir #5176](https://github.com/aspnet/EntityFramework/issues/5176).
