---
title: 'Introducción a .NET Core: base de datos nueva - EF Core'
author: rick-anderson
ms.author: riande
description: Introducción a .NET Core con Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: a0df80a8fe96be4f8cc3177919e2b087e14cb49c
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325332"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="479ab-103">Introducción a EF Core en la aplicación de consola de .NET Core con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="479ab-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="479ab-104">En este tutorial se crea una aplicación de consola de .NET Core que realiza el acceso a datos en una base de datos SQLite mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="479ab-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="479ab-105">Se usan migraciones para crear la base de datos a partir del modelo.</span><span class="sxs-lookup"><span data-stu-id="479ab-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="479ab-106">Consulte [ASP.NET Core: base de datos nueva](xref:core/get-started/aspnetcore/new-db) para una versión de Visual Studio mediante ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="479ab-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="479ab-107">[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="479ab-107">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="479ab-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="479ab-108">Prerequisites</span></span>

* [<span data-ttu-id="479ab-109">SDK de .NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="479ab-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="479ab-110">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="479ab-110">Create a new project</span></span>

* <span data-ttu-id="479ab-111">Cree un nuevo proyecto de consola:</span><span class="sxs-lookup"><span data-stu-id="479ab-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a><span data-ttu-id="479ab-112">Cambiar el directorio actual</span><span class="sxs-lookup"><span data-stu-id="479ab-112">Change the current directory</span></span>

<span data-ttu-id="479ab-113">En los siguientes pasos, hay que emitir comandos `dotnet` en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="479ab-113">In subsequent steps, we need to issue `dotnet` commands against the application.</span></span>

* <span data-ttu-id="479ab-114">Se cambia el directorio actual al de la aplicación de este modo:</span><span class="sxs-lookup"><span data-stu-id="479ab-114">We change the current directory to the application's directory like this:</span></span>

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a><span data-ttu-id="479ab-115">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="479ab-115">Install Entity Framework Core</span></span>

<span data-ttu-id="479ab-116">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="479ab-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="479ab-117">Este tutorial usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="479ab-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="479ab-118">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="479ab-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="479ab-119">Instale Microsoft.EntityFrameworkCore.Sqlite y Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="479ab-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="479ab-120">Ejecute `dotnet restore` para instalar los paquetes nuevos.</span><span class="sxs-lookup"><span data-stu-id="479ab-120">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="479ab-121">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="479ab-121">Create the model</span></span>

<span data-ttu-id="479ab-122">Defina un contexto y clases de entidad que constituirán el modelo.</span><span class="sxs-lookup"><span data-stu-id="479ab-122">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="479ab-123">Cree un archivo *Model.cs* nuevo con el contenido siguiente.</span><span class="sxs-lookup"><span data-stu-id="479ab-123">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="479ab-124">Sugerencia: En una aplicación real, lo habitual sería colocar cada clase en un archivo independiente, y la cadena de conexión, en un archivo de configuración o una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="479ab-124">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="479ab-125">Para que el tutorial sea sencillo, todo está incluido en un archivo.</span><span class="sxs-lookup"><span data-stu-id="479ab-125">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="479ab-126">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="479ab-126">Create the database</span></span>

<span data-ttu-id="479ab-127">Una vez que tenga un modelo, puede usar [migraciones](xref:core/managing-schemas/migrations/index) para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="479ab-127">Once you have a model, you use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

* <span data-ttu-id="479ab-128">Ejecute `dotnet ef migrations add InitialCreate` para aplicar scaffolding a una migración y crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="479ab-128">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="479ab-129">Ejecute `dotnet ef database update` para aplicar la migración nueva a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="479ab-129">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="479ab-130">Este comando crea la base de datos antes de aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="479ab-130">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="479ab-131">*blogging.db* SQLite DB está en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="479ab-131">The *blogging.db* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="479ab-132">Uso del modelo</span><span class="sxs-lookup"><span data-stu-id="479ab-132">Use the model</span></span>

* <span data-ttu-id="479ab-133">Abra *Program.cs* y reemplace el contenido por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="479ab-133">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="479ab-134">Pruebe la aplicación desde la consola.</span><span class="sxs-lookup"><span data-stu-id="479ab-134">Test the app from the console.</span></span> <span data-ttu-id="479ab-135">Vea la [nota de Visual Studio](#vs) para ejecutar la aplicación desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="479ab-135">See the [Visual Studio note](#vs) to run the app from Visual Studio.</span></span>

  `dotnet run`

  <span data-ttu-id="479ab-136">Un blog se guarda en la base de datos y los detalles de todos los blogs se muestran en la consola.</span><span class="sxs-lookup"><span data-stu-id="479ab-136">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="479ab-137">Cambios del modelo:</span><span class="sxs-lookup"><span data-stu-id="479ab-137">Changing the model:</span></span>

- <span data-ttu-id="479ab-138">Si realiza cambios en el modelo, puede usar el comando `dotnet ef migrations add` para aplicar el scaffolding a una [migración](xref:core/managing-schemas/migrations/index) nueva.</span><span class="sxs-lookup"><span data-stu-id="479ab-138">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](xref:core/managing-schemas/migrations/index).</span></span> <span data-ttu-id="479ab-139">Una vez que compruebe el código con scaffolding (y haya hecho los cambios necesarios), puede usar el comando `dotnet ef database update` para aplicar los cambios de esquema a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="479ab-139">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="479ab-140">EF Core usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="479ab-140">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="479ab-141">El motor de base de datos de SQLite no admite algunos cambios de esquema que sí son compatibles con la mayoría de las otras bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="479ab-141">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="479ab-142">Por ejemplo, no se admite la operación `DropColumn`.</span><span class="sxs-lookup"><span data-stu-id="479ab-142">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="479ab-143">Las migraciones de EF Core generarán el código para estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="479ab-143">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="479ab-144">Sin embargo, si intenta aplicarlas a una base de datos o generar un script, EF Core producirá excepciones.</span><span class="sxs-lookup"><span data-stu-id="479ab-144">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="479ab-145">Consulte [Limitaciones de SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="479ab-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="479ab-146">En el caso de un desarrollo nuevo, considere eliminar la base de datos y cree una nueva en lugar de usar migraciones cuando cambie el modelo.</span><span class="sxs-lookup"><span data-stu-id="479ab-146">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

<a name="vs"></a>
### <a name="run-from-visual-studio"></a><span data-ttu-id="479ab-147">Ejecutar desde Visual Studio</span><span class="sxs-lookup"><span data-stu-id="479ab-147">Run from Visual Studio</span></span>

<span data-ttu-id="479ab-148">Para ejecutar este ejemplo desde Visual Studio, debe establecer el directorio de trabajo como raíz del proyecto de forma manual.</span><span class="sxs-lookup"><span data-stu-id="479ab-148">To run this sample from Visual Studio, you must set the working directory manually to be the root of the project.</span></span> <span data-ttu-id="479ab-149">Si no establece el directorio de trabajo, se produce la siguiente excepción `Microsoft.Data.Sqlite.SqliteException`: `SQLite Error 1: 'no such table: Blogs'`.</span><span class="sxs-lookup"><span data-stu-id="479ab-149">If  you don't set the working directory, the following `Microsoft.Data.Sqlite.SqliteException` is thrown: `SQLite Error 1: 'no such table: Blogs'`.</span></span>

<span data-ttu-id="479ab-150">Para establecer el directorio de trabajo:</span><span class="sxs-lookup"><span data-stu-id="479ab-150">To set the working directory:</span></span>

* <span data-ttu-id="479ab-151">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="479ab-151">In **Solution Explorer**, right click the project and then select **Properties**.</span></span>
* <span data-ttu-id="479ab-152">Seleccione la pestaña **Depurar** en el panel izquierdo.</span><span class="sxs-lookup"><span data-stu-id="479ab-152">Select the **Debug** tab in the left pane.</span></span>
* <span data-ttu-id="479ab-153">Establezca **Directorio de trabajo** en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="479ab-153">Set **Working directory** to the project directory.</span></span>
* <span data-ttu-id="479ab-154">Guarde los cambios.</span><span class="sxs-lookup"><span data-stu-id="479ab-154">Save the changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="479ab-155">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="479ab-155">Additional Resources</span></span>

* [<span data-ttu-id="479ab-156">Tutorial: Introducción a EF Core en ASP.NET Core con una base de datos nueva mediante SQLite</span><span class="sxs-lookup"><span data-stu-id="479ab-156">Tutorial: Get started with EF Core on ASP.NET Core with a new database using SQLite</span></span>](xref:core/get-started/aspnetcore/new-db)
* [<span data-ttu-id="479ab-157">Tutorial: Introducción a Razor Pages en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="479ab-157">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="479ab-158">Tutorial: Razor Pages con Entity Framework Core en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="479ab-158">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
