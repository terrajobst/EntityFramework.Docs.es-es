---
title: 'Introducción a .NET Core: base de datos nueva - EF Core'
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Introducción a .NET Core con Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911507"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="39172-104">Introducción a EF Core en la aplicación de consola de .NET Core con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="39172-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="39172-105">En este tutorial se crea una aplicación de consola de .NET Core que realiza el acceso a datos en una base de datos SQLite mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="39172-105">In this walkthrough, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="39172-106">Se usan migraciones para crear la base de datos a partir del modelo.</span><span class="sxs-lookup"><span data-stu-id="39172-106">You use migrations to create the database from the model.</span></span> <span data-ttu-id="39172-107">Consulte [ASP.NET Core: base de datos nueva](xref:core/get-started/aspnetcore/new-db) para una versión de Visual Studio mediante ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="39172-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="39172-108">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="39172-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="39172-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="39172-109">Prerequisites</span></span>

<span data-ttu-id="39172-110">[SDK de .NET Core](https://www.microsoft.com/net/core) 2.1</span><span class="sxs-lookup"><span data-stu-id="39172-110">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="39172-111">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="39172-111">Create a new project</span></span>

* <span data-ttu-id="39172-112">Cree un nuevo proyecto de consola:</span><span class="sxs-lookup"><span data-stu-id="39172-112">Create a new console project:</span></span>

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="39172-113">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="39172-113">Install Entity Framework Core</span></span>

<span data-ttu-id="39172-114">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="39172-114">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="39172-115">Este tutorial usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="39172-115">This walkthrough uses SQLite.</span></span> <span data-ttu-id="39172-116">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="39172-116">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="39172-117">Instale Microsoft.EntityFrameworkCore.Sqlite y Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="39172-117">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="39172-118">Ejecute `dotnet restore` para instalar los paquetes nuevos.</span><span class="sxs-lookup"><span data-stu-id="39172-118">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="39172-119">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="39172-119">Create the model</span></span>

<span data-ttu-id="39172-120">Defina un contexto y clases de entidad que constituirán el modelo.</span><span class="sxs-lookup"><span data-stu-id="39172-120">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="39172-121">Cree un archivo *Model.cs* nuevo con el contenido siguiente.</span><span class="sxs-lookup"><span data-stu-id="39172-121">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="39172-122">Sugerencia: En una aplicación real, coloque cada clase en un archivo independiente y la cadena de conexión en un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="39172-122">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="39172-123">Para que el tutorial sea sencillo, todo está incluido en un archivo.</span><span class="sxs-lookup"><span data-stu-id="39172-123">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="39172-124">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="39172-124">Create the database</span></span>

<span data-ttu-id="39172-125">Una vez que tenga un modelo, puede usar [migraciones](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="39172-125">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="39172-126">Ejecute `dotnet ef migrations add InitialCreate` para aplicar scaffolding a una migración y crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="39172-126">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="39172-127">Ejecute `dotnet ef database update` para aplicar la migración nueva a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="39172-127">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="39172-128">Este comando crea la base de datos antes de aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="39172-128">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="39172-129">*blogging.db*\* SQLite DB está en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="39172-129">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="39172-130">Uso del modelo</span><span class="sxs-lookup"><span data-stu-id="39172-130">Use your model</span></span>

* <span data-ttu-id="39172-131">Abra *Program.cs* y reemplace el contenido por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="39172-131">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="39172-132">Pruebe la aplicación:</span><span class="sxs-lookup"><span data-stu-id="39172-132">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="39172-133">Un blog se guarda en la base de datos y los detalles de todos los blogs se muestran en la consola.</span><span class="sxs-lookup"><span data-stu-id="39172-133">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="39172-134">Cambios del modelo:</span><span class="sxs-lookup"><span data-stu-id="39172-134">Changing the model:</span></span>

- <span data-ttu-id="39172-135">Si hace cambios en el modelo, puede usar el comando `dotnet ef migrations add` para aplicar scaffolding a una [migración](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) nueva con el fin de hacer los cambios de esquema correspondientes a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="39172-135">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="39172-136">Una vez que compruebe el código con scaffold (y haya hecho los cambios necesarios), puede usar el comando `dotnet ef database update` para aplicar los cambios a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="39172-136">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="39172-137">EF usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="39172-137">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="39172-138">SQLite no admite todas las migraciones (cambios de esquema) debido a las limitaciones de SQLite.</span><span class="sxs-lookup"><span data-stu-id="39172-138">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="39172-139">Consulte [Limitaciones de SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="39172-139">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="39172-140">En el caso de un desarrollo nuevo, considere eliminar la base de datos y cree una nueva en lugar de usar migraciones cuando cambie el modelo.</span><span class="sxs-lookup"><span data-stu-id="39172-140">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39172-141">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="39172-141">Additional Resources</span></span>

* <span data-ttu-id="39172-142">[.NET Core: base de datos nueva con SQLite](xref:core/get-started/netcore/new-db-sqlite), un tutorial de EF para la consola multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="39172-142">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="39172-143">Introducción a ASP.NET Core MVC en Mac o Linux</span><span class="sxs-lookup"><span data-stu-id="39172-143">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="39172-144">Introducción a ASP.NET Core MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39172-144">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="39172-145">Introducción a ASP.NET Core y Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39172-145">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
