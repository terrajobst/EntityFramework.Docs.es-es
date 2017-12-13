---
title: "Introducción a .NET Core: base de datos nueva - EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: "Introducción a .NET Core con Entity Framework Core"
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 22fc0446dee71dd0d2402b47d76cc8b7307fbe5f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Introducción a EF Core en la aplicación de consola de .NET Core con una base de datos nueva

En este tutorial, creará una aplicación de consola de .NET Core que realiza el acceso a datos básicos en una base de datos SQLite mediante Entity Framework. Usará las migraciones para crear la base de datos a partir del modelo. Consulte [ASP.NET Core: base de datos nueva](xref:core/get-started/aspnetcore/new-db) para una versión de Visual Studio mediante ASP.NET Core MVC.

> [!NOTE]  
> El [SDK de .NET Core](https://www.microsoft.com/net/download/core) ya no es compatible con `project.json` ni con Visual Studio 2015. Se recomienda [migrar de project.json a csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Si usa Visual Studio, se recomienda migrar a [Visual Studio 2017](https://www.visualstudio.com/downloads/).

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) de este artículo en GitHub.

## <a name="prerequisites"></a>Requisitos previos

Deberá cumplir los requisitos previos siguientes para completar este tutorial:
* Un sistema operativo compatible con .NET Core.
* El [SDK 2.0 de .NET Core](https://www.microsoft.com/net/core) (a pesar de que las instrucciones se pueden usar para crear una aplicación con una versión anterior con muy pocas modificaciones).

## <a name="create-a-new-project"></a>Crear un proyecto nuevo

* Cree una carpeta `ConsoleApp.SQLite` nueva para el proyecto y use el comando `dotnet` para rellenarlo con una aplicación de .NET Core.

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a>Instalación de Entity Framework Core

Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse. Este tutorial usa SQLite. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).

* Instale Microsoft.EntityFrameworkCore.Sqlite y Microsoft.EntityFrameworkCore.Design

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Edite `ConsoleApp.SQLite.csproj` manualmente para agregar DotNetCliToolReference a Microsoft.EntityFrameworkCore.Tools.DotNet:

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

 Nota: Una versión futura de `dotnet` admitirá DotNetCliToolReferences a través de `dotnet add tool`

`ConsoleApp.SQLite.csproj` ahora debe contener lo siguiente:

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 Nota: Los números de versión anteriormente usados eran los correctos en el momento de la publicación.

*  Ejecute `dotnet restore` para instalar los paquetes nuevos.

## <a name="create-the-model"></a>Creación del modelo

Defina un contexto y clases de entidad que constituirán el modelo.

* Cree un archivo *Model.cs* nuevo con el contenido siguiente.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Sugerencia: En una aplicación real, colocaría cada clase en un archivo independiente y la cadena de conexión en un archivo de configuración. Para la simplicidad del tutorial, colocaremos todos estos elementos en un solo archivo.

## <a name="create-the-database"></a>Crear la base de datos

Una vez que ya tiene un modelo, puede usar las [migraciones](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para crear una base de datos.

* Ejecute `dotnet ef migrations add InitialCreate` para aplicar scaffolding a una migración y crear el conjunto inicial de tablas para el modelo.
* Ejecute `dotnet ef database update` para aplicar la migración nueva a la base de datos. Este comando crea la base de datos antes de aplicar las migraciones.

> [!NOTE]  
> Cuando use rutas de acceso relativas con SQLite, la ruta de acceso será relativa al ensamblado principal de la aplicación. En este ejemplo, el binario principal es `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, por lo que la base de datos SQLite estará en `bin/Debug/netcoreapp2.0/blogging.db`.

## <a name="use-your-model"></a>Uso del modelo

* Abra *Program.cs* y reemplace el contenido por el código siguiente:

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Pruebe la aplicación:

 `dotnet run`

 Un blog se guarda en la base de datos y los detalles de todos los blogs se muestran en la consola.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Cambios del modelo:

- Si hace cambios en el modelo, puede usar el comando `dotnet ef migrations add` para aplicar scaffolding a una [migración](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) nueva con el fin de hacer los cambios de esquema correspondientes a la base de datos. Una vez que compruebe el código con scaffold (y haya hecho los cambios necesarios), puede usar el comando `dotnet ef database update` para aplicar los cambios a la base de datos.
- EF usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.
- SQLite no admite todas las migraciones (cambios de esquema) debido a las limitaciones de SQLite. Consulte [Limitaciones de SQLite](../../providers/sqlite/limitations.md). En el caso de un desarrollo nuevo, considere eliminar la base de datos y cree una nueva en lugar de usar migraciones cuando cambie el modelo.

## <a name="additional-resources"></a>Recursos adicionales

* [.NET Core: base de datos nueva con SQLite](xref:core/get-started/netcore/new-db-sqlite), un tutorial de EF para la consola multiplataforma.
* [Introducción a ASP.NET Core MVC en Mac o Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Introducción a ASP.NET Core MVC con Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Introducción a ASP.NET Core y Entity Framework Core con Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
