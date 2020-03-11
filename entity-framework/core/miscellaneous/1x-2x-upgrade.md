---
title: Actualización de versiones anteriores a EF Core 2-EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: b27c09fdb6210dd7c6aa0c8bc912a8bd183c16b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414234"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Actualización de las aplicaciones de las versiones anteriores a EF Core 2,0

Hemos aprovechado la oportunidad de refinar significativamente las API y los comportamientos existentes en 2,0. Hay algunas mejoras que pueden requerir la modificación del código de aplicación existente, aunque creemos que, para la mayoría de las aplicaciones, el impacto será bajo, en la mayoría de los casos solo requiere volver a compilar y cambios guiados mínimos para reemplazar las API obsoletas.

La actualización de una aplicación existente a EF Core 2,0 puede requerir:

1. Actualización de la implementación de .NET de destino de la aplicación a una que admite .NET Standard 2,0. Vea [implementaciones de .net compatibles](xref:core/platforms/index) para obtener más detalles.

2. Identifique un proveedor para la base de datos de destino que sea compatible con EF Core 2,0. Consulte [EF Core 2,0 requiere un proveedor de base de datos 2,0](#ef-core-20-requires-a-20-database-provider) .

3. Actualizar todos los paquetes de EF Core (tiempo de ejecución y herramientas) a 2,0. Consulte [instalación de EF Core](xref:core/get-started/install/index) para obtener más detalles.

4. Realice los cambios de código necesarios para compensar los cambios importantes descritos en el resto de este documento.

## <a name="aspnet-core-now-includes-ef-core"></a>ASP.NET Core ahora incluye EF Core

Las aplicaciones para ASP.NET Core 2.0 pueden usar EF Core 2.0 sin dependencias adicionales además de proveedores de bases de datos de terceros. Sin embargo, las aplicaciones que tienen como destino versiones anteriores de ASP.NET Core deben actualizar a ASP.NET Core 2,0 para poder usar EF Core 2,0. Para obtener más información sobre la actualización de aplicaciones ASP.NET Core a 2,0, consulte [la documentación de ASP.net Core sobre el tema](/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Nueva forma de obtener servicios de aplicación en ASP.NET Core

El patrón recomendado para las aplicaciones Web de ASP.NET Core se ha actualizado para 2,0 de forma que se interrumpió la lógica en tiempo de diseño EF Core utiliza en 1. x. Anteriormente, en tiempo de diseño, EF Core intentaría invocar `Startup.ConfigureServices` directamente para tener acceso al proveedor de servicios de la aplicación. En ASP.NET Core 2,0, la configuración se inicializa fuera de la clase `Startup`. Las aplicaciones que usan EF Core normalmente acceden a su cadena de conexión desde la configuración, por lo que `Startup` por sí misma ya no es suficiente. Si actualiza una aplicación ASP.NET Core 1. x, es posible que reciba el siguiente error al usar las herramientas de EF Core.

> No se encontró ningún constructor sin parámetros en ' ApplicationContext '. Agregue un constructor sin parámetros a ' ApplicationContext ' o agregue una implementación de ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' en el mismo ensamblado que ' ApplicationContext '

Se ha agregado un nuevo enlace en tiempo de diseño a la plantilla predeterminada de ASP.NET Core 2.0. El método `Program.BuildWebHost` estático permite a EF Core tener acceso al proveedor de servicios de la aplicación en tiempo de diseño. Si está actualizando una aplicación ASP.NET Core 1. x, deberá actualizar la clase `Program` para que se parezca a lo siguiente.

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

La adopción de este nuevo patrón al actualizar las aplicaciones a 2,0 es muy recomendable y es necesaria para que funcionen las características del producto, como Entity Framework Core las migraciones. La otra alternativa común es [implementar *IDesignTimeDbContextFactory\<TContext >* ](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

## <a name="idbcontextfactory-renamed"></a>IDbContextFactory cambiado

Con el fin de admitir diversos patrones de aplicación y proporcionar a los usuarios un mayor control sobre cómo se utiliza su `DbContext` en tiempo de diseño, en el pasado, siempre se ofrecía la interfaz `IDbContextFactory<TContext>`. En tiempo de diseño, las herramientas de EF Core detectarán las implementaciones de esta interfaz en el proyecto y la usarán para crear objetos `DbContext`.

Esta interfaz tenía un nombre muy general que engaña a algunos usuarios para intentar volver a usarlo para otros escenarios de creación de `DbContext`. No estaban protegidos cuando las herramientas de EF intentaban usar su implementación en tiempo de diseño y provocaban que se produjera un error en los comandos como `Update-Database` o `dotnet ef database update`.

Con el fin de comunicar la semántica sólida en tiempo de diseño de esta interfaz, se le ha cambiado el nombre a `IDesignTimeDbContextFactory<TContext>`.

En la versión 2,0, el `IDbContextFactory<TContext>` todavía existe pero está marcado como obsoleto.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions quitado

Debido a los cambios ASP.NET Core 2,0 descritos anteriormente, encontramos que `DbContextFactoryOptions` ya no era necesario en la nueva interfaz de `IDesignTimeDbContextFactory<TContext>`. Estas son las alternativas que debe usar en su lugar.

| DbContextFactoryOptions | Alternativa                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext. BaseDirectory                                     |
| ContentRootPath         | Directorio. GetCurrentDirectory ()                              |
| EnvironmentName         | Environment. GetEnvironmentVariable ("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Directorio de trabajo en tiempo de diseño cambiado

Los cambios ASP.NET Core 2,0 también requerían el directorio de trabajo que usa `dotnet ef` para alinearse con el directorio de trabajo que usa Visual Studio cuando se ejecuta la aplicación. Un efecto lateral observable de esto es que los nombres de archivo de SQLite ahora son relativos al directorio del proyecto y no al directorio de salida como se solían usar.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2,0 requiere un proveedor de base de datos 2,0

Por EF Core 2,0 hemos realizado muchas simplificaciones y mejoras en la forma en que funcionan los proveedores de bases de datos. Esto significa que los proveedores 1.0. x y 1.1. x no funcionarán con EF Core 2,0.

El equipo de EF envía los proveedores de SQL Server y SQLite, y las versiones 2,0 estarán disponibles como parte de la versión 2,0. Los proveedores de terceros de código abierto para [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)y [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) se están actualizando para 2,0. Para todos los demás proveedores, póngase en contacto con el escritor del proveedor.

## <a name="logging-and-diagnostics-events-have-changed"></a>Los eventos de diagnóstico y registro han cambiado

Nota: estos cambios no deben afectar a la mayoría del código de aplicación.

Los identificadores de eventos de los mensajes enviados a un [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) han cambiado en 2,0. Los identificadores de evento ahora son únicos en el código de EF Core. Ahora, estos mensajes también siguen el patrón estándar de registro estructurado que usa, por ejemplo, MVC.

Las categorías de registrador también han cambiado. Ahora hay un conjunto conocido de categorías a las que se accede a través de [DbLoggerCategory](https://github.com/aspnet/EntityFrameworkCore/blob/rel/2.0.0/src/EFCore/DbLoggerCategory.cs).

Los eventos de [DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) ahora usan los mismos nombres de ID. de evento que los mensajes de `ILogger` correspondientes. Las cargas de evento son tipos nominales derivados de [EventData](/dotnet/api/microsoft.entityframeworkcore.diagnostics.eventdata).

Los identificadores de eventos, tipos de carga y categorías se documentan en las clases [CoreEventId](/dotnet/api/microsoft.entityframeworkcore.diagnostics.coreeventid) y [RelationalEventId](/dotnet/api/microsoft.entityframeworkcore.diagnostics.relationaleventid) .

Los identificadores también se han pasado de Microsoft. EntityFrameworkCore. Infrastructure al nuevo espacio de nombres Microsoft. EntityFrameworkCore. Diagnostics.

## <a name="ef-core-relational-metadata-api-changes"></a>Cambios de la API de metadatos relacionales EF Core

EF Core 2.0 ahora compila un elemento [IModel](/dotnet/api/microsoft.entityframeworkcore.metadata.imodel) diferente para cada proveedor que se va a usar. Esto suele ser transparente para la aplicación. Esto ha facilitado la simplificación de las API de metadatos de nivel inferior, de modo que cualquier acceso a _conceptos de metadatos relacionales comunes_ siempre se realiza a través de una llamada a `.Relational` en lugar de `.SqlServer`, `.Sqlite`, etc. Por ejemplo, 1.1. x código similar al siguiente:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Ahora debe escribirse de la siguiente manera:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

En lugar de utilizar métodos como `ForSqlServerToTable`, los métodos de extensión ahora están disponibles para escribir código condicional basado en el proveedor actual en uso. Por ejemplo:

```csharp
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Tenga en cuenta que este cambio solo se aplica a las API/metadatos que se definen para _todos los_ proveedores relacionales. La API y los metadatos siguen siendo los mismos cuando son específicos de un solo proveedor. Por ejemplo, los índices clúster son específicos de SQL Server, por lo que se deben seguir `ForSqlServerIsClustered` y `.SqlServer().IsClustered()`.

## <a name="dont-take-control-of-the-ef-service-provider"></a>No tomar el control del proveedor de servicios de EF

EF Core usa una `IServiceProvider` interna (un contenedor de inserción de dependencias) para su implementación interna. Las aplicaciones deben permitir que EF Core crear y administrar este proveedor, excepto en casos especiales. Considere la posibilidad de quitar todas las llamadas a `UseInternalServiceProvider`. Si una aplicación necesita llamar a `UseInternalServiceProvider`, considere la posibilidad de [presentar un problema](https://github.com/aspnet/EntityFramework/Issues) para que podamos investigar otras maneras de controlar su escenario.

El código de aplicación no requiere la llamada a `AddEntityFramework`, `AddEntityFrameworkSqlServer`, etc. a menos que se llame también a `UseInternalServiceProvider`. Quite todas las llamadas existentes a `AddEntityFramework` o `AddEntityFrameworkSqlServer`, etc. `AddDbContext` debe seguir utilizándose de la misma manera que antes.

## <a name="in-memory-databases-must-be-named"></a>Las bases de datos en memoria deben tener nombre

La base de datos en memoria global sin nombre se ha quitado y, en su lugar, todas las bases de datos en memoria deben tener nombre. Por ejemplo:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Esto crea o usa una base de datos con el nombre "base de datos". Si se llama de nuevo a `UseInMemoryDatabase` con el mismo nombre, se usará la misma base de datos en memoria, lo que permite que varias instancias de contexto lo compartan.

## <a name="read-only-api-changes"></a>Cambios de la API de solo lectura

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`y `IsStoreGeneratedAlways` han quedado obsoletos y se han reemplazado por [BeforeSaveBehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.beforesavebehavior) y [AfterSaveBehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.aftersavebehavior). Estos comportamientos se aplican a cualquier propiedad (no solo a las propiedades generadas por el almacén) y determinan cómo se debe usar el valor de la propiedad al insertar en una fila de base de datos (`BeforeSaveBehavior`) o al actualizar una fila de base de datos existente (`AfterSaveBehavior`).

Las propiedades marcadas como [ValueGenerated. OnAddOrUpdate](/dotnet/api/microsoft.entityframeworkcore.metadata.valuegenerated) (por ejemplo, para las columnas calculadas) omitirán de forma predeterminada cualquier valor establecido actualmente en la propiedad. Esto significa que siempre se obtendrá un valor generado por el almacén independientemente de si se ha establecido o modificado algún valor en la entidad a la que se realiza el seguimiento. Esto se puede cambiar estableciendo un `Before\AfterSaveBehavior`diferente.

## <a name="new-clientsetnull-delete-behavior"></a>Nuevo comportamiento de eliminación de ClientSetNull

En versiones anteriores, [DeleteBehavior. Restrict](/dotnet/api/microsoft.entityframeworkcore.deletebehavior) tenía un comportamiento para las entidades a las que realiza un seguimiento el contexto que mejoró la semántica de `SetNull` coincidentes. En EF Core 2,0, se ha introducido un nuevo comportamiento de `ClientSetNull` como valor predeterminado para las relaciones opcionales. Este comportamiento tiene `SetNull` semántica para las entidades sometidas a seguimiento y el comportamiento de `Restrict` para las bases de datos creadas mediante EF Core. En nuestra experiencia, estos son los comportamientos más previstos y útiles para las entidades de las que se realiza un seguimiento y la base de datos. ahora se respeta `DeleteBehavior.Restrict` para las entidades de las que se ha realizado un seguimiento cuando se establece para relaciones opcionales.

## <a name="provider-design-time-packages-removed"></a>Paquetes en tiempo de diseño del proveedor quitados

Se ha quitado el paquete de `Microsoft.EntityFrameworkCore.Relational.Design`. Su contenido se consolida en `Microsoft.EntityFrameworkCore.Relational` y `Microsoft.EntityFrameworkCore.Design`.

Esto se propaga a los paquetes en tiempo de diseño del proveedor. Los paquetes (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, etc.) se han quitado y su contenido se ha consolidado en los paquetes principales del proveedor.

Para habilitar `Scaffold-DbContext` o `dotnet ef dbcontext scaffold` en EF Core 2,0, solo tiene que hacer referencia al paquete de proveedor único:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
