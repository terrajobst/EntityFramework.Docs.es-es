---
title: Actualización desde versiones anteriores a EF Core 2 - EF Core
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: dca9a3fb9e514b6eb22281a0f0140539681efb71
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949261"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Actualización de aplicaciones desde versiones anteriores a EF Core 2.0

## <a name="procedures-common-to-all-applications"></a>Los procedimientos comunes a todas las aplicaciones

Puede requerir la actualización de una aplicación existente a EF Core 2.0:

1. Actualización de la plataforma .NET de destino de la aplicación a una que admita .NET Standard 2.0. Consulte [plataformas admitidas](../platforms/index.md) para obtener más detalles.

2. Identificar un proveedor para la base de datos de destino que es compatible con EF Core 2.0. Consulte [EF Core 2.0 requiere un proveedor de base de 2.0 datos](#ef-core-20-requires-a-20-database-provider) a continuación.

3. Actualizar todos los paquetes de EF Core (tooling y en tiempo de ejecución) a la 2.0. Consulte [instalar EF Core](../get-started/install/index.md) para obtener más detalles.

4. Realice los cambios de código necesario para compensar cambios importantes. Consulte la [Breaking Changes](#breaking-changes) sección para obtener más detalles.

## <a name="aspnet-core-applications"></a>Aplicaciones ASP.NET Core

1. Vea en particular el [nuevo patrón para inicializar el proveedor de servicios de la aplicación](#new-way-of-getting-application-services) se describe a continuación.

> [!TIP]  
> La adopción de este nuevo patrón al actualizar las aplicaciones a 2.0 es muy recomendable y es necesario para características de producto como migraciones de Entity Framework Core funcione. La otra alternativa común es [implementar *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

2. Las aplicaciones para ASP.NET Core 2.0 pueden usar EF Core 2.0 sin dependencias adicionales además de proveedores de bases de datos de terceros. Sin embargo, las aplicaciones destinadas a versiones anteriores de ASP.NET Core deben actualizar a ASP.NET Core 2.0 para poder usar EF Core 2.0. Para obtener más información sobre cómo actualizar las aplicaciones ASP.NET Core 2.0, consulte [la documentación de ASP.NET Core en el asunto](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Cambios importantes

Hemos tomado la oportunidad para redefinir significativamente nuestras API existentes y los comportamientos en 2.0. Hay algunas mejoras que pueden requerir la modificación de código de aplicación existente, aunque creemos para la mayoría de las aplicaciones el impacto suele ser bajo, en la mayoría de los casos que requieren solo recompilación y unos cambios mínimos guiados para reemplazar API obsoletas.

### <a name="new-way-of-getting-application-services"></a>Nueva forma de obtener los servicios de aplicación

Se actualizó el patrón recomendado para aplicaciones web ASP.NET Core 2.0 de forma que se interrumpió la lógica en tiempo de diseño que usa EF Core en la versión 1.x. Anteriormente en tiempo de diseño, EF Core intentaría invocar `Startup.ConfigureServices` directamente con el fin de obtener acceso a proveedor de servicios de la aplicación. En ASP.NET Core 2.0, se inicializa la configuración fuera de la `Startup` clase. Las aplicaciones que usan EF Core suelen tener acceso a su cadena de conexión de configuración, por lo que `Startup` por sí mismo ya no es suficiente. Si actualiza una aplicación de ASP.NET Core 1.x, puede recibir el siguiente error al usar las herramientas de EF Core.

> No se encontró ningún constructor sin parámetros en 'ApplicationContext'. Agregue un constructor sin parámetros para 'ApplicationContext' o agregue una implementación de ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' en el mismo ensamblado que 'ApplicationContext'

Se ha agregado un nuevo enlace de tiempo de diseño en la plantilla predeterminada de ASP.NET Core 2.0. Estático `Program.BuildWebHost` método permite que EF Core tener acceso a proveedor de servicios de la aplicación en tiempo de diseño. Si va a actualizar una aplicación de ASP.NET Core 1.x, deberá actualizar le `Program` clase similar a lo siguiente.

``` csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

### <a name="idbcontextfactory-renamed"></a>Cambiar el nombre de IDbContextFactory

Con el fin de admitir patrones de aplicación diferentes y proporcionar a los usuarios más control sobre cómo sus `DbContext` se utiliza en tiempo de diseño, entregamos, en el pasado, el `IDbContextFactory<TContext>` interfaz. En tiempo de diseño, las herramientas de EF Core detectará las implementaciones de esta interfaz en el proyecto y usarlo para crear `DbContext` objetos.

Esta interfaz tenía un nombre muy general que confundir a algunos usuarios para intentar volver a usarla para otros `DbContext`-creación de escenarios. Que se ven desprevenidas cuando las herramientas de EF, a continuación, ha intentado usar su implementación en tiempo de diseño y provocó comandos como `Update-Database` o `dotnet ef database update` para producir un error.

Con el fin de comunicar la semántica de tiempo de diseño segura de esta interfaz, nos hemos cambiado el nombre a `IDesignTimeDbContextFactory<TContext>`.

Para la versión 2.0 de la versión de la `IDbContextFactory<TContext>` todavía existe, pero está marcado como obsoleto.

### <a name="dbcontextfactoryoptions-removed"></a>Quitar DbContextFactoryOptions

Debido a los cambios de ASP.NET Core 2.0 se ha descrito anteriormente, hemos encontrado que `DbContextFactoryOptions` ya no era necesario en el nuevo `IDesignTimeDbContextFactory<TContext>` interfaz. Estas son las alternativas que se debería utilizar en su lugar.

| DbContextFactoryOptions | Alternativa                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

### <a name="design-time-working-directory-changed"></a>Puede cambiar el directorio de trabajo de tiempo de diseño

Los cambios de ASP.NET Core 2.0 requieren también el directorio de trabajo usando `dotnet ef` para alinearse con el directorio de trabajo que usa Visual Studio cuando se ejecuta la aplicación. Un efecto secundario observable de esto es que SQLite los nombres de archivo están ahora en relación con el directorio del proyecto y no en el directorio de salida como los que.

### <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 requiere un proveedor de base de 2.0 datos

Para EF Core 2.0 se han realizado muchas mejoras en los proveedores de base de datos de manera y simplificaciones funcione. Esto significa que los proveedores de 1.0 y 1.1 no funcionarán con EF Core 2.0.

Los proveedores de SQL Server y SQLite enviados por el equipo de EF y 2.0 versiones estará disponibles como parte de la versión 2.0 de versión. Los proveedores de código abierto de terceros para [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), y [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) se actualizan para 2.0. Para todos los otros proveedores, póngase en contacto con el escritor de proveedores.

### <a name="logging-and-diagnostics-events-have-changed"></a>Han cambiado los eventos de registro y diagnóstico

Nota: estos cambios no deberían afectar a la mayoría de código de aplicación.

Los identificadores de evento para los mensajes enviados a un [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) han cambiado en 2.0. Los identificadores de evento ahora son únicos en el código de EF Core. Ahora, estos mensajes también siguen el patrón estándar de registro estructurado que usa, por ejemplo, MVC.

Las categorías de registrador también han cambiado. Ahora hay un conjunto conocido de categorías a las que se accede a través de [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) eventos ahora usan los mismos nombres de Id. de evento correspondiente `ILogger` mensajes. Las cargas del evento son todos los tipos nominales que se deriva de [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Id. de evento, tipos de carga y categorías se documentan en el [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) y [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) clases.

Identificadores también han movido desde Microsoft.EntityFrameworkCore.Infraestructure a nuevo espacio de nombres Microsoft.EntityFrameworkCore.Diagnostics.

### <a name="ef-core-relational-metadata-api-changes"></a>API de metadatos relacionales cambios EF Core

EF Core 2.0 ahora compila un elemento [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) diferente para cada proveedor que se va a usar. Esto suele ser transparente para la aplicación. Esto ha permitido una simplificación de las API de metadatos de nivel inferior, de modo que cualquier acceso a _conceptos de metadatos relacionales comunes_ siempre se realiza mediante una llamada a `.Relational` en lugar de a `.SqlServer`, `.Sqlite`, etc. Por ejemplo, 1.1 código similar al siguiente:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Ahora debe escribirse como esta:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

En lugar de usar métodos como `ForSqlServerToTable`, métodos de extensión ahora están disponibles para escribir código condicional basado en el proveedor actual en uso. Por ejemplo:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Tenga en cuenta que este cambio solo se aplica a las API y metadatos que se define para _todas_ proveedores relacionales. La API y los metadatos permanece invariable cuando es solo un único proveedor específico. Por ejemplo, los índices agrupados son específicos de SQL Server, por lo que `ForSqlServerIsClustered` y `.SqlServer().IsClustered()` debe seguir usándose.

### <a name="dont-take-control-of-the-ef-service-provider"></a>No tome el control del proveedor de servicios EF

EF Core usa una instancia interna `IServiceProvider` (un contenedor de inserción de dependencia) para su implementación interna. Las aplicaciones deben permitir que EF Core crear y administrar este proveedor, excepto en casos especiales. Considerar la eliminación de todas las llamadas a `UseInternalServiceProvider`. Si una aplicación necesita llamar a `UseInternalServiceProvider`, a continuación, considere la posibilidad de [archivando un problema](https://github.com/aspnet/EntityFramework/Issues) para que podamos investigarlo otras formas de controlar su escenario.

Una llamada a `AddEntityFramework`, `AddEntityFrameworkSqlServer`, etc. no es necesario el código de aplicación a menos que `UseInternalServiceProvider` también se denomina. Quite cualquier llamada existente al `AddEntityFramework` o `AddEntityFrameworkSqlServer`, etc. `AddDbContext` todavía debe usarse en la misma manera que antes.

### <a name="in-memory-databases-must-be-named"></a>Se deben llamar a las bases de datos en memoria

Se ha quitado la base de datos en memoria sin nombre global y en su lugar deben denominarse todas las bases de datos en memoria. Por ejemplo:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Esto crea y usa una base de datos con el nombre "MyDatabase". Si `UseInMemoryDatabase` se llama de nuevo con el mismo nombre, a continuación, se usará la misma base de datos en memoria, para que puedan compartirse entre varias instancias de contexto.

### <a name="read-only-api-changes"></a>Cambios en la API de solo lectura

`IsReadOnlyBeforeSave`, `IsReadOnlyAferSave`, y `IsStoreGeneratedAlways` se han obsoleto y se reemplazan por [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) y [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Estos comportamientos se aplican a cualquier propiedad (no sólo las propiedades generadas por el almacén) y determinar cómo se debe usar el valor de la propiedad cuando se insertan en una fila de la base de datos (`BeforeSaveBehavior`) o al actualizar una existente base de datos fila (`AfterSaveBehavior`).

Las propiedades marcan como [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (por ejemplo, para las columnas calculadas) omitirá de forma predeterminada cualquier valor establecido actualmente en la propiedad. Esto significa que siempre se obtendrá un valor generado por el almacén, independientemente de si cualquier valor se ha establecido o modificado en la entidad con seguimiento. Esto se puede cambiar estableciendo otro `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Nuevo comportamiento de eliminación ClientSetNull

En versiones anteriores, [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) tenía un comportamiento para las entidades seguidas por el contexto que más cerrado coincidente `SetNull` semántica. En EF Core 2.0, un nuevo `ClientSetNull` comportamiento se ha introducido como el valor predeterminado para las relaciones opcionales. Este comportamiento tiene `SetNull` semántica para las entidades sometidas a seguimiento y `Restrict` comportamiento para las bases de datos creadas con EF Core. En nuestra experiencia, estos son los comportamientos más espera/útiles para las entidades sometidas a seguimiento y la base de datos. `DeleteBehavior.Restrict` Ahora se admite para las entidades sometidas a seguimiento cuando se establece para las relaciones opcionales.

### <a name="provider-design-time-packages-removed"></a>Paquetes de tiempo de diseño del proveedor quitados

El `Microsoft.EntityFrameworkCore.Relational.Design` se ha quitado el paquete. Su contenido se consolidaron `Microsoft.EntityFrameworkCore.Relational` y `Microsoft.EntityFrameworkCore.Design`.

Esto se propaga a los paquetes de tiempo de diseño de proveedor. Los paquetes (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, etc.) se quitaron y su contenido se consolida en los paquetes de proveedor principal.

Para habilitar `Scaffold-DbContext` o `dotnet ef dbcontext scaffold` en EF Core 2.0, solo deberá hacen referencia al paquete de proveedor único:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
