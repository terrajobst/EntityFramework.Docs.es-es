---
title: Actualización de EF Core 1.0 RC1 a RC2 - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: e76886729248101ccac024568cf9abcd945fca33
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678632"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Actualización de EF Core 1.0 RC1 a 1.0 RC2

En este artículo se proporciona instrucciones para mover una aplicación compilada con los paquetes de RC1 a RC2.

## <a name="package-names-and-versions"></a>Versiones y los nombres de paquete

Entre RC1 y RC2, se cambia de "Entity Framework 7" a "Entity Framework Core". Puede obtener más información sobre los motivos para que el cambio en [esta entrada de Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Debido a este cambio, cambia los nombres de nuestro paquete de `EntityFramework.*` a `Microsoft.EntityFrameworkCore.*` y las versiones de `7.0.0-rc1-final` a `1.0.0-rc2-final` (o `1.0.0-preview1-final` para herramientas).

**Debe quitar completamente los paquetes RC1 y, a continuación, instale el RC2 unos.** Esta es la asignación de algunos paquetes comunes.

| Paquete de RC1                                               | Equivalente de RC2                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite                    7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | No está disponible todavía para RC2                                            |
| EntityFramework.Commands                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>Espacios de nombres

Junto con los nombres de paquete, los espacios de nombres se cambió de `Microsoft.Data.Entity.*` a `Microsoft.EntityFrameworkCore.*`. Puede controlar este cambio con una operación de búsqueda y reemplazo de `using Microsoft.Data.Entity` con `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Tabla de cambios de convención de nomenclatura

Un cambio significativo de funcional se llevaron a cabo en RC2 consistía en utilizar el nombre de la `DbSet<TEntity>` propiedad para una entidad determinada como el nombre de tabla se asigna, en lugar de simplemente el nombre de clase. Puede leer más sobre este cambio en [el problema de presentación relacionados](https://github.com/aspnet/Announcements/issues/167).

Para las aplicaciones existentes de RC1, se recomienda agregar el código siguiente al principio de su `OnModelCreating` método para mantener la estrategia de nomenclatura de RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Si desea adoptar la nueva estrategia de nomenclatura, le recomendamos correctamente cambia el nombre de completar el resto de los pasos de actualización y, a continuación, eliminar el código y crear una migración para aplicar la tabla.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs cambia (solo para proyectos de ASP.NET Core)

En RC1, tenía que agregar servicios de Entity Framework para el proveedor de servicios de aplicación - en `Startup.ConfigureServices(...)`:

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

También debe agregar un constructor, el contexto de derivada, que toma las opciones de contexto y los pasa al constructor base. Esto es necesario porque se ha quitado la magia terror que había introducido en segundo plano:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Pasar un IServiceProvider

Si tiene código RC1 que pasa una `IServiceProvider` en el contexto, esto ahora se movió a `DbContextOptions`, en lugar de ser un parámetro de constructor independiente. Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` para establecer el proveedor de servicios.

### <a name="testing"></a>Pruebas

El escenario más común para esto era controlar el ámbito de una base de datos InMemory al probar. Vea actualizado [pruebas](testing/index.md) artículo para obtener un ejemplo de hacerlo con RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Resolver servicios internos de proveedor de servicios de aplicación (solo para proyectos de ASP.NET Core)

Si tiene una aplicación de ASP.NET Core y desea EF para resolver los servicios internos desde el proveedor de servicios de aplicación, hay una sobrecarga de `AddDbContext` que le permite configurar esto:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Se recomienda permitir EF internamente administrar sus propios servicios, a menos que tenga una razón para combinar los servicios EF internos en su proveedor de servicios de aplicación. La razón principal, puede que desee hacerlo es utilizar el proveedor de servicios de aplicación para reemplazar los servicios que EF utiliza internamente

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Comandos DNX = > .NET CLI (solo para proyectos de ASP.NET Core)

Si anteriormente utilizaba la `dnx ef` comandos para los proyectos de ASP.NET 5, estos ahora han movido a `dotnet ef` comandos. La misma sintaxis de comando sigue siendo válido. Puede usar `dotnet ef --help` para obtener información de sintaxis.

Ha cambiado la manera en que se registran los comandos de RC2, debido a que será reemplazada por .NET CLI de DNX. Comandos ahora están registrados en un `tools` sección `project.json`:

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
> Si utiliza Visual Studio, ahora puede usar la consola de administrador de paquetes para ejecutar comandos EF para los proyectos de ASP.NET Core (Esto no se admite en RC1). Se sigue siendo necesario registrar los comandos en el `tools` sección de `project.json` a hacerlo.

## <a name="package-manager-commands-require-powershell-5"></a>Comandos del Administrador de paquetes requieren PowerShell 5

Si usa los comandos de Entity Framework en la consola de administrador de paquetes en Visual Studio, a continuación, debe asegurarse de que tener instalado el 5 de PowerShell. Se trata de un requisito temporal que se quitará en la próxima versión (consulte [emitir #5327](https://github.com/aspnet/EntityFramework/issues/5327) para obtener más detalles).

## <a name="using-imports-in-projectjson"></a>Uso de "importaciones" en project.json

Algunas de las dependencias del núcleo de EF todavía no compatible con .NET estándar. Núcleo EF en proyectos de .NET estándar y .NET Core puede requerir agregar "importa" en project.json como solución temporal.

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

La solución consiste en importar manualmente el perfil portátil "portable net451 + win8". Esta fuerza NuGet para tratar este archivos binarios que coinciden con este proporciona un marco de trabajo compatible con el estándar. NET, incluso aunque no son. Aunque "portable net451 + win8" no es compatible con .NET estándar al 100%, es lo suficientemente compatible para la transición de PCL a .NET estándar. Importaciones pueden quitarse cuando las dependencias del EF finalmente la actualización a .NET estándar.

Varios marcos pueden agregarse a la "importación" en la sintaxis de matriz. Otras importaciones pueden ser necesarios si agrega bibliotecas adicionales a su proyecto.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Vea [emitir #5176](https://github.com/aspnet/EntityFramework/issues/5176).
