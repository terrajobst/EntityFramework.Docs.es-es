---
title: "Actualización desde versiones anteriores a EF Core 2 - Core EF"
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 380f27c9f00943a2909ec7b876e151572a67dc37
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Actualizar aplicaciones de versiones anteriores a EF Core 2.0

## <a name="procedures-common-to-all-applications"></a>Procedimientos comunes a todas las aplicaciones

Puede requerir la actualización de una aplicación existente a EF Core 2.0:

1. Actualización de la plataforma de .NET de destino de la aplicación a una que admita el estándar de .NET 2.0. Vea [Supported Platforms](../platforms/index.md) para obtener más detalles.

2. Identificar un proveedor para la base de datos de destino que sea compatible con EF Core 2.0. Vea [EF Core 2.0 requiere un proveedor de base de 2.0 datos](#ef-core-20-requires-a-20-database-provider) a continuación.

3. Actualizar todos los paquetes de EF principales (en tiempo de ejecución y herramientas) 2.0. Hacer referencia a [instalar EF Core](../get-started/install/index.md) para obtener más detalles.

4. Realice los cambios de código necesarios para compensar cambios importantes. Consulte la [Breaking Changes](#breaking-changes) sección para obtener más detalles.

## <a name="aspnet-core-applications"></a>Aplicaciones de ASP.NET Core

1. Vea en particular el [nuevo patrón para inicializar el proveedor de servicio de la aplicación](#new-way-of-getting-application-services) se describe a continuación.

> [!TIP]  
> La adopción de este nuevo patrón al actualizar las aplicaciones 2.0 es muy recomendable y es necesario para características de producto como Entity Framework Core migraciones para que funcione. La otra alternativa común es [implementar *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

2. Las aplicaciones para ASP.NET Core 2.0 pueden usar EF Core 2.0 sin dependencias adicionales además de proveedores de bases de datos de terceros. Sin embargo, las aplicaciones destinadas a versiones anteriores de ASP.NET Core deben actualizar a ASP.NET Core 2.0 para poder usar EF Core 2.0. Para obtener más información sobre cómo actualizar las aplicaciones principales de ASP.NET 2.0, consulte [la documentación principal de ASP.NET en el asunto](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Cambios importantes

Le hemos llevado la oportunidad para redefinir notablemente nuestra API existentes y los comportamientos en 2.0. Existen algunas mejoras que pueden requerir modificar el código de aplicación existente, aunque creemos que para la mayoría de las aplicaciones el impacto será bajo, en la mayoría de los casos que requieren simplemente recompilación y unos cambios mínimos guiados para reemplazar API obsoletas.

### <a name="new-way-of-getting-application-services"></a>Nueva manera de obtener los servicios de aplicaciones

Se ha actualizado el patrón recomendado para las aplicaciones web de ASP.NET Core para 2.0 de forma que la lógica de tiempo de diseño que principales de EF utilizada en 1.x se interrumpió. Anteriormente en tiempo de diseño, Core EF intentaría invocar `Startup.ConfigureServices` directamente con el fin de obtener acceso a proveedor de servicios de la aplicación. En el núcleo de ASP.NET 2.0, se inicializa configuración fuera de la `Startup` clase. Las aplicaciones que usan EF Core suelen tener acceso a sus cadenas de conexión de configuración, por lo que `Startup` por sí solo no es suficiente. Si actualiza una aplicación de ASP.NET Core 1.x, puede recibir el error siguiente al usar las herramientas de EF Core.

> No se encontró ningún constructor sin parámetros en 'ApplicationContext'. Agregue un constructor sin parámetros al 'ApplicationContext' o agregue una implementación de ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' en el mismo ensamblado que 'ApplicationContext'

Se ha agregado un nuevo enlace de tiempo de diseño en la plantilla predeterminada de ASP.NET Core 2.0. El método estático `Program.BuildWebHost` método permite que el núcleo de EF acceder al proveedor de servicios de la aplicación en tiempo de diseño. Si va a actualizar una aplicación de ASP.NET Core 1.x, debe mantenerlo `Program` clase similares a lo siguiente.

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

Con el fin de admitir patrones de diversas de la aplicación y proporcionar a los usuarios más control sobre cómo sus `DbContext` se utiliza en tiempo de diseño, tenemos, en el pasado, siempre que el `IDbContextFactory<TContext>` interfaz. En tiempo de diseño, las herramientas principales de EF detectará las implementaciones de esta interfaz en el proyecto y usarlo para crear `DbContext` objetos.

Esta interfaz tenía un nombre muy general que desorientar a algunos usuarios para intentar volver a utilizarlo para otros `DbContext`-crear escenarios. Se precavido cuando las herramientas de EF, a continuación, intentó usar su implementación en tiempo de diseño y se hizo comandos como `Update-Database` o `dotnet ef database update` un error.

Para comunicar la semántica de tiempo de diseño segura de esta interfaz, hemos hemos cambiado el nombre a `IDesignTimeDbContextFactory<TContext>`.

Para la versión 2.0 de la versión de la `IDbContextFactory<TContext>` todavía existe, pero está marcado como obsoleto.

### <a name="dbcontextfactoryoptions-removed"></a>Quitar DbContextFactoryOptions

Debido a los cambios de núcleo de ASP.NET 2.0 se ha descrito anteriormente, descubrimos que `DbContextFactoryOptions` ya no se necesita en el nuevo `IDesignTimeDbContextFactory<TContext>` interfaz. Estas son las alternativas que se debe usar en su lugar.

DbContextFactoryOptions | Alternativa
--- | ---
ApplicationBasePath | AppContext.BaseDirectory
ContentRootPath | Directory.GetCurrentDirectory()
EnvironmentName | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")

### <a name="design-time-working-directory-changed"></a>Cambiar el directorio de trabajo de tiempo de diseño

Los cambios principales de ASP.NET 2.0 también requieren el directorio de trabajo utilizado por `dotnet ef` para alinearse con el directorio de trabajo utilizado por Visual Studio cuando se ejecuta la aplicación. Un efecto secundario observable de esto es que SQLite nombres de archivo están ahora en relación con el directorio del proyecto y no en el directorio de salida como solía ser.

### <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 requiere un proveedor de base de 2.0 datos

Para EF Core 2.0 hemos realizado muchas mejoras en los proveedores de base de datos de manera y simplificación de trabajo. Esto significa que los proveedores 1.0. x y 1.1.x no funcionará con EF Core 2.0.

Los proveedores de SQL Server y SQLite se distribuyen por el equipo de EF y 2.0 versiones estarán disponibles como parte de la versión 2.0 de la versión. Los proveedores de código abierto de terceros para [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), y [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) se actualizan para 2.0. Para todos los otros proveedores, póngase en contacto con el sistema de escritura de proveedor.

### <a name="logging-and-diagnostics-events-have-changed"></a>Eventos de registro y diagnóstico han cambiado

Nota: estos cambios no deberían afectar la mayoría del código de aplicación.

Los identificadores de evento para los mensajes enviados a un [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) han cambiado en 2.0. Los identificadores de evento ahora son únicos en el código de EF Core. Ahora, estos mensajes también siguen el patrón estándar de registro estructurado que usa, por ejemplo, MVC.

Las categorías de registrador también han cambiado. Ahora hay un conjunto conocido de categorías a las que se accede a través de [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) eventos ahora utilizan los mismos nombres de Id. de evento como el correspondiente `ILogger` mensajes. Las cargas del evento son todos los tipos nominales derivados [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Id. de evento, tipos de carga y categorías se documentan en el [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) y [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) clases.

Id. también ha movido desde Microsoft.EntityFrameworkCore.Infraestructure para el nuevo espacio de nombres Microsoft.EntityFrameworkCore.Diagnostics.

### <a name="ef-core-relational-metadata-api-changes"></a>Cambios de metadatos relacionales API principales EF

EF Core 2.0 ahora compila un elemento [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) diferente para cada proveedor que se va a usar. Esto suele ser transparente para la aplicación. Esto ha permitido una simplificación de las API de metadatos de nivel inferior, de modo que cualquier acceso a _conceptos de metadatos relacionales comunes_ siempre se realiza mediante una llamada a `.Relational` en lugar de a `.SqlServer`, `.Sqlite`, etc. Por ejemplo, 1.1.x código similar al siguiente:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Ahora debe escribirse como el siguiente:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

En lugar de usar métodos como `ForSqlServerToTable`, métodos de extensión ahora están disponibles para escribir código condicional basado en el proveedor actual en uso. Por ejemplo:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Tenga en cuenta que este cambio sólo se aplica a las API y metadatos que se define para _todos los_ proveedores relacional. La API y los metadatos permanece invariable cuando es específico a solo un único proveedor. Por ejemplo, los índices agrupados son específicos de SQL Server, por lo que `ForSqlServerIsClustered` y `.SqlServer().IsClustered()` debe seguir usándose.

### <a name="dont-take-control-of-the-ef-service-provider"></a>No tome el control del proveedor de servicios EF

EF Core utiliza un interno `IServiceProvider` (es decir, un contenedor de inyección de dependencia) para su implementación interna. Las aplicaciones deben permitir EF principal crear y administrar este proveedor excepto en casos especiales. Considerar seriamente la posibilidad de quitar todas las llamadas a `UseInternalServiceProvider`. Si una aplicación necesita llamar a `UseInternalServiceProvider`, a continuación, considere la posibilidad de [presentar un problema](https://github.com/aspnet/EntityFramework/Issues) que nos permita investigar otras formas de controlar el escenario.

Al llamar a `AddEntityFramework`, `AddEntityFrameworkSqlServer`, etc. no es necesario el código de aplicación a menos que `UseInternalServiceProvider` también se denomina. Quite todas las llamadas existentes a `AddEntityFramework` o `AddEntityFrameworkSqlServer`, etc. `AddDbContext` todavía debe usarse en la misma manera que antes.

### <a name="in-memory-databases-must-be-named"></a>Deben denominarse bases de datos en memoria

Se ha quitado la base de datos en memoria sin nombre global y en su lugar deben denominarse todas las bases de datos en memoria. Por ejemplo:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Esto crea y usa una base de datos con el nombre "MyDatabase". Si `UseInMemoryDatabase` se llama de nuevo con el mismo nombre, se utilizará la misma base de datos en memoria, lo que le permite ser compartida por varias instancias de contexto.

### <a name="read-only-api-changes"></a>Cambios en la API de solo lectura

`IsReadOnlyBeforeSave`, `IsReadOnlyAferSave`, y `IsStoreGeneratedAlways` se han obsoleta y se ha reemplazado por [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) y [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Estos comportamientos se aplican a cualquier propiedad (no sólo las propiedades generadas por el almacenamiento) y determinar cómo se debe usar el valor de la propiedad al insertar en una fila de la base de datos (`BeforeSaveBehavior`) o al actualizar a otra base de datos fila (`AfterSaveBehavior`).

Propiedades marcadas como [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (por ejemplo, para las columnas calculadas) predeterminada omitirá cualquier valor establecido actualmente en la propiedad. Esto significa que un valor generado por el almacén se obtendrán siempre, independientemente de si se ha establecido o modificado en la entidad de seguimiento cualquier valor. Esto se puede cambiar estableciendo otro `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Nuevo comportamiento de eliminación de ClientSetNull

En versiones anteriores, [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) tenía un comportamiento de entidades hace un seguimiento mediante el contexto que más cerrado coincidente `SetNull` semántica. En EF Core 2.0, un nuevo `ClientSetNull` comportamiento se ha introducido como el valor predeterminado para las relaciones opcionales. Este comportamiento tiene `SetNull` semántica para entidades sometidas a seguimiento y `Restrict` comportamiento para bases de datos creadas mediante el uso EF Core. En nuestra experiencia, estos son los comportamientos más espera/útiles para entidades sometidas a seguimiento y la base de datos. `DeleteBehavior.Restrict`Ahora se admite para las entidades de seguimiento cuando se establece por relaciones opcionales.

### <a name="provider-design-time-packages-removed"></a>Paquetes de tiempo de diseño de proveedor quitados

El `Microsoft.EntityFrameworkCore.Relational.Design` se ha quitado el paquete. Su contenido se consolida en `Microsoft.EntityFrameworkCore.Relational` y `Microsoft.EntityFrameworkCore.Design`.

Esto se propaga a los paquetes de tiempo de diseño de proveedor. Los paquetes (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, etc.) se quitaron y su contenido se consolida en los paquetes de proveedores principal.

Para habilitar `Scaffold-DbContext` o `dotnet ef dbcontext scaffold` en EF Core 2.0, solo necesita hacen referencia al paquete de proveedor único:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
