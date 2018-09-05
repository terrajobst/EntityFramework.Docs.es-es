---
title: 'Introducción a .NET Core: base de datos nueva - EF Core'
author: rick-anderson
ms.author: riande
description: Introducción a .NET Core con Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 51f5752eebce5603c663072f7b36dfecd4ddf227
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993697"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Introducción a EF Core en la aplicación de consola de .NET Core con una base de datos nueva

En este tutorial se crea una aplicación de consola de .NET Core que realiza el acceso a datos en una base de datos SQLite mediante Entity Framework Core. Se usan migraciones para crear la base de datos a partir del modelo. Consulte [ASP.NET Core: base de datos nueva](xref:core/get-started/aspnetcore/new-db) para una versión de Visual Studio mediante ASP.NET Core MVC.

Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>Requisitos previos

* [SDK de .NET Core 2.1](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Crear un proyecto nuevo

* Cree un nuevo proyecto de consola:

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
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

Una vez que tenga un modelo, puede usar [migraciones](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para crear una base de datos.

* Ejecute `dotnet ef migrations add InitialCreate` para aplicar scaffolding a una migración y crear el conjunto inicial de tablas para el modelo.
* Ejecute `dotnet ef database update` para aplicar la migración nueva a la base de datos. Este comando crea la base de datos antes de aplicar las migraciones.

*blogging.db** SQLite DB está en el directorio del proyecto.

## <a name="use-the-model"></a>Uso del modelo

* Abra *Program.cs* y reemplace el contenido por el código siguiente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Pruebe la aplicación:

  `dotnet run`

  Un blog se guarda en la base de datos y los detalles de todos los blogs se muestran en la consola.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Cambios del modelo:

- Si realiza cambios en el modelo, puede usar el comando `dotnet ef migrations add` para aplicar el scaffolding a una [migración](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) nueva. Una vez que compruebe el código con scaffolding (y haya hecho los cambios necesarios), puede usar el comando `dotnet ef database update` para aplicar los cambios de esquema a la base de datos.
- EF Core usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.
- El motor de base de datos de SQLite no admite algunos cambios de esquema que sí son compatibles con la mayoría de las otras bases de datos relacionales. Por ejemplo, no se admite la operación `DropColumn`. Las migraciones de EF Core generarán el código para estas operaciones. Sin embargo, si intenta aplicarlas a una base de datos o generar un script, EF Core producirá excepciones. Consulte [Limitaciones de SQLite](../../providers/sqlite/limitations.md). En el caso de un desarrollo nuevo, considere eliminar la base de datos y cree una nueva en lugar de usar migraciones cuando cambie el modelo.

## <a name="additional-resources"></a>Recursos adicionales

* [Introducción a ASP.NET Core MVC en Mac o Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Introducción a ASP.NET Core MVC con Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Introducción a ASP.NET Core y Entity Framework Core con Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
