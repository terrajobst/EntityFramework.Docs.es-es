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
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="7aec4-103">Introducción a EF Core en la aplicación de consola de .NET Core con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="7aec4-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="7aec4-104">En este tutorial se crea una aplicación de consola de .NET Core que realiza el acceso a datos en una base de datos SQLite mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7aec4-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="7aec4-105">Se usan migraciones para crear la base de datos a partir del modelo.</span><span class="sxs-lookup"><span data-stu-id="7aec4-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="7aec4-106">Consulte [ASP.NET Core: base de datos nueva](xref:core/get-started/aspnetcore/new-db) para una versión de Visual Studio mediante ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="7aec4-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="7aec4-107">Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="7aec4-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7aec4-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7aec4-108">Prerequisites</span></span>

* [<span data-ttu-id="7aec4-109">SDK de .NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="7aec4-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="7aec4-110">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="7aec4-110">Create a new project</span></span>

* <span data-ttu-id="7aec4-111">Cree un nuevo proyecto de consola:</span><span class="sxs-lookup"><span data-stu-id="7aec4-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a><span data-ttu-id="7aec4-112">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7aec4-112">Install Entity Framework Core</span></span>

<span data-ttu-id="7aec4-113">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="7aec4-113">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="7aec4-114">Este tutorial usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="7aec4-114">This walkthrough uses SQLite.</span></span> <span data-ttu-id="7aec4-115">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="7aec4-115">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="7aec4-116">Instale Microsoft.EntityFrameworkCore.Sqlite y Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="7aec4-116">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="7aec4-117">Ejecute `dotnet restore` para instalar los paquetes nuevos.</span><span class="sxs-lookup"><span data-stu-id="7aec4-117">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="7aec4-118">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="7aec4-118">Create the model</span></span>

<span data-ttu-id="7aec4-119">Defina un contexto y clases de entidad que constituirán el modelo.</span><span class="sxs-lookup"><span data-stu-id="7aec4-119">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="7aec4-120">Cree un archivo *Model.cs* nuevo con el contenido siguiente.</span><span class="sxs-lookup"><span data-stu-id="7aec4-120">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="7aec4-121">Consejo: En una aplicación real, lo habitual sería colocar cada clase en un archivo independiente, y la cadena de conexión, en un archivo de configuración o una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="7aec4-121">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="7aec4-122">Para que el tutorial sea sencillo, todo está incluido en un archivo.</span><span class="sxs-lookup"><span data-stu-id="7aec4-122">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="7aec4-123">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="7aec4-123">Create the database</span></span>

<span data-ttu-id="7aec4-124">Una vez que tenga un modelo, puede usar [migraciones](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="7aec4-124">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="7aec4-125">Ejecute `dotnet ef migrations add InitialCreate` para aplicar scaffolding a una migración y crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="7aec4-125">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="7aec4-126">Ejecute `dotnet ef database update` para aplicar la migración nueva a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7aec4-126">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="7aec4-127">Este comando crea la base de datos antes de aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="7aec4-127">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="7aec4-128">*blogging.db*\* SQLite DB está en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7aec4-128">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="7aec4-129">Uso del modelo</span><span class="sxs-lookup"><span data-stu-id="7aec4-129">Use the model</span></span>

* <span data-ttu-id="7aec4-130">Abra *Program.cs* y reemplace el contenido por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7aec4-130">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="7aec4-131">Pruebe la aplicación:</span><span class="sxs-lookup"><span data-stu-id="7aec4-131">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="7aec4-132">Un blog se guarda en la base de datos y los detalles de todos los blogs se muestran en la consola.</span><span class="sxs-lookup"><span data-stu-id="7aec4-132">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="7aec4-133">Cambios del modelo:</span><span class="sxs-lookup"><span data-stu-id="7aec4-133">Changing the model:</span></span>

- <span data-ttu-id="7aec4-134">Si realiza cambios en el modelo, puede usar el comando `dotnet ef migrations add` para aplicar el scaffolding a una [migración](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) nueva.</span><span class="sxs-lookup"><span data-stu-id="7aec4-134">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span></span> <span data-ttu-id="7aec4-135">Una vez que compruebe el código con scaffolding (y haya hecho los cambios necesarios), puede usar el comando `dotnet ef database update` para aplicar los cambios de esquema a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7aec4-135">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="7aec4-136">EF Core usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7aec4-136">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="7aec4-137">El motor de base de datos de SQLite no admite algunos cambios de esquema que sí son compatibles con la mayoría de las otras bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="7aec4-137">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="7aec4-138">Por ejemplo, no se admite la operación `DropColumn`.</span><span class="sxs-lookup"><span data-stu-id="7aec4-138">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="7aec4-139">Las migraciones de EF Core generarán el código para estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="7aec4-139">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="7aec4-140">Sin embargo, si intenta aplicarlas a una base de datos o generar un script, EF Core producirá excepciones.</span><span class="sxs-lookup"><span data-stu-id="7aec4-140">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="7aec4-141">Consulte [Limitaciones de SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="7aec4-141">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="7aec4-142">En el caso de un desarrollo nuevo, considere eliminar la base de datos y cree una nueva en lugar de usar migraciones cuando cambie el modelo.</span><span class="sxs-lookup"><span data-stu-id="7aec4-142">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7aec4-143">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7aec4-143">Additional Resources</span></span>

* [<span data-ttu-id="7aec4-144">Introducción a ASP.NET Core MVC en Mac o Linux</span><span class="sxs-lookup"><span data-stu-id="7aec4-144">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="7aec4-145">Introducción a ASP.NET Core MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7aec4-145">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="7aec4-146">Introducción a ASP.NET Core y Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7aec4-146">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
