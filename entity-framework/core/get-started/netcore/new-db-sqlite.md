---
title: 'Introducción a .NET Core: base de datos nueva - EF Core'
author: rick-anderson
ms.author: riande
description: Introducción a .NET Core con Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 6cebe14e179cb6998592f5d3823c114b3bda0138
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022316"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Introducción a EF Core en la aplicación de consola de .NET Core con una base de datos nueva

En este tutorial se crea una aplicación de consola de .NET Core que realiza el acceso a datos en una base de datos SQLite mediante Entity Framework Core. Se usan migraciones para crear la base de datos a partir del modelo. Consulte [ASP.NET Core: base de datos nueva](xref:core/get-started/aspnetcore/new-db) para una versión de Visual Studio mediante ASP.NET Core MVC.

[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>Requisitos previos

* [SDK de .NET Core 2.1](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Crear un proyecto nuevo

* Cree un nuevo proyecto de consola:

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a>Cambiar el directorio actual

En los siguientes pasos, hay que emitir comandos `dotnet` en la aplicación.

* Se cambia el directorio actual al de la aplicación de este modo:

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a>Instalación de Entity Framework Core

Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse. Este tutorial usa SQLite. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).

* Instale Microsoft.EntityFrameworkCore.Sqlite y Microsoft.EntityFrameworkCore.Design

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* Ejecute `dotnet restore` para instalar los paquetes nuevos.

## <a name="create-the-model"></a>Creación del modelo

Defina un contexto y clases de entidad que constituirán el modelo.

* Cree un archivo *Model.cs* nuevo con el contenido siguiente.

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Consejo: En una aplicación real, lo habitual sería colocar cada clase en un archivo independiente, y la cadena de conexión, en un archivo de configuración o una variable de entorno. Para que el tutorial sea sencillo, todo está incluido en un archivo.

## <a name="create-the-database"></a>Creación de la base de datos

Una vez que tenga un modelo, puede usar [migraciones](xref:core/managing-schemas/migrations/index) para crear una base de datos.

* Ejecute `dotnet ef migrations add InitialCreate` para aplicar scaffolding a una migración y crear el conjunto inicial de tablas para el modelo.
* Ejecute `dotnet ef database update` para aplicar la migración nueva a la base de datos. Este comando crea la base de datos antes de aplicar las migraciones.

*blogging.db** SQLite DB está en el directorio del proyecto.

## <a name="use-the-model"></a>Uso del modelo

* Abra *Program.cs* y reemplace el contenido por el código siguiente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Pruebe la aplicación desde la consola. Vea la [nota de Visual Studio](#vs) para ejecutar la aplicación desde Visual Studio.

  `dotnet run`

  Un blog se guarda en la base de datos y los detalles de todos los blogs se muestran en la consola.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Cambios del modelo:

- Si realiza cambios en el modelo, puede usar el comando `dotnet ef migrations add` para aplicar el scaffolding a una [migración](xref:core/managing-schemas/migrations/index) nueva. Una vez que compruebe el código con scaffolding (y haya hecho los cambios necesarios), puede usar el comando `dotnet ef database update` para aplicar los cambios de esquema a la base de datos.
- EF Core usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.
- El motor de base de datos de SQLite no admite algunos cambios de esquema que sí son compatibles con la mayoría de las otras bases de datos relacionales. Por ejemplo, no se admite la operación `DropColumn`. Las migraciones de EF Core generarán el código para estas operaciones. Sin embargo, si intenta aplicarlas a una base de datos o generar un script, EF Core producirá excepciones. Consulte [Limitaciones de SQLite](../../providers/sqlite/limitations.md). En el caso de un desarrollo nuevo, considere eliminar la base de datos y cree una nueva en lugar de usar migraciones cuando cambie el modelo.

<a name="vs"></a>
### <a name="run-from-visual-studio"></a>Ejecutar desde Visual Studio

Para ejecutar este ejemplo desde Visual Studio, debe establecer el directorio de trabajo como raíz del proyecto de forma manual. Si no establece el directorio de trabajo, se produce la siguiente excepción `Microsoft.Data.Sqlite.SqliteException`: `SQLite Error 1: 'no such table: Blogs'`.

Para establecer el directorio de trabajo:

* En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.
* Seleccione la pestaña **Depurar** en el panel izquierdo.
* Establezca **Directorio de trabajo** en el directorio del proyecto.
* Guarde los cambios.

## <a name="additional-resources"></a>Recursos adicionales

* [Tutorial: Introducción a EF Core en ASP.NET Core con una base de datos nueva](xref:core/get-started/aspnetcore/new-db)
* [Tutorial: Introducción a Razor Pages en ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Tutorial: Razor Pages con Entity Framework Core en ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
