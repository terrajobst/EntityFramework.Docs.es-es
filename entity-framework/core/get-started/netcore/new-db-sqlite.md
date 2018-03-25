---
title: 'Introducción a .NET Core: base de datos nueva - EF Core'
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Introducción a .NET Core con Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 2511dfa3f3262bb12c2058dc1c402b7dcc4c670d
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="8d175-104">Introducción a EF Core en la aplicación de consola de .NET Core con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="8d175-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="8d175-105">En este tutorial, creará una aplicación de consola de .NET Core que realiza el acceso a datos básicos en una base de datos SQLite mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8d175-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="8d175-106">Usará las migraciones para crear la base de datos a partir del modelo.</span><span class="sxs-lookup"><span data-stu-id="8d175-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="8d175-107">Consulte [ASP.NET Core: base de datos nueva](xref:core/get-started/aspnetcore/new-db) para una versión de Visual Studio mediante ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="8d175-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="8d175-108">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="8d175-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d175-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8d175-109">Prerequisites</span></span>

<span data-ttu-id="8d175-110">Deberá cumplir los requisitos previos siguientes para completar este tutorial:</span><span class="sxs-lookup"><span data-stu-id="8d175-110">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="8d175-111">Un sistema operativo compatible con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8d175-111">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="8d175-112">El [SDK 2.0 de .NET Core](https://www.microsoft.com/net/core) (a pesar de que las instrucciones se pueden usar para crear una aplicación con una versión anterior con muy pocas modificaciones).</span><span class="sxs-lookup"><span data-stu-id="8d175-112">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="8d175-113">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="8d175-113">Create a new project</span></span>

* <span data-ttu-id="8d175-114">Cree una carpeta `ConsoleApp.SQLite` nueva para el proyecto y use el comando `dotnet` para rellenarlo con una aplicación de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8d175-114">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="8d175-115">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8d175-115">Install Entity Framework Core</span></span>

<span data-ttu-id="8d175-116">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="8d175-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="8d175-117">Este tutorial usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="8d175-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="8d175-118">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="8d175-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="8d175-119">Instale Microsoft.EntityFrameworkCore.Sqlite y Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="8d175-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="8d175-120">Edite `ConsoleApp.SQLite.csproj` manualmente para agregar DotNetCliToolReference a Microsoft.EntityFrameworkCore.Tools.DotNet:</span><span class="sxs-lookup"><span data-stu-id="8d175-120">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

<span data-ttu-id="8d175-121">`ConsoleApp.SQLite.csproj` ahora debe contener lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8d175-121">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="8d175-122">Nota: Los números de versión anteriormente usados eran los correctos en el momento de la publicación.</span><span class="sxs-lookup"><span data-stu-id="8d175-122">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="8d175-123">Ejecute `dotnet restore` para instalar los paquetes nuevos.</span><span class="sxs-lookup"><span data-stu-id="8d175-123">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="8d175-124">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="8d175-124">Create the model</span></span>

<span data-ttu-id="8d175-125">Defina un contexto y clases de entidad que constituirán el modelo.</span><span class="sxs-lookup"><span data-stu-id="8d175-125">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="8d175-126">Cree un archivo *Model.cs* nuevo con el contenido siguiente.</span><span class="sxs-lookup"><span data-stu-id="8d175-126">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="8d175-127">Sugerencia: En una aplicación real, colocaría cada clase en un archivo independiente y la cadena de conexión en un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="8d175-127">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="8d175-128">Para la simplicidad del tutorial, colocaremos todos estos elementos en un solo archivo.</span><span class="sxs-lookup"><span data-stu-id="8d175-128">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="8d175-129">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="8d175-129">Create the database</span></span>

<span data-ttu-id="8d175-130">Una vez que ya tiene un modelo, puede usar las [migraciones](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d175-130">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="8d175-131">Ejecute `dotnet ef migrations add InitialCreate` para aplicar scaffolding a una migración y crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="8d175-131">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="8d175-132">Ejecute `dotnet ef database update` para aplicar la migración nueva a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d175-132">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="8d175-133">Este comando crea la base de datos antes de aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="8d175-133">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="8d175-134">Cuando use rutas de acceso relativas con SQLite, la ruta de acceso será relativa al ensamblado principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d175-134">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="8d175-135">En este ejemplo, el binario principal es `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, por lo que la base de datos SQLite estará en `bin/Debug/netcoreapp2.0/blogging.db`.</span><span class="sxs-lookup"><span data-stu-id="8d175-135">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="8d175-136">Uso del modelo</span><span class="sxs-lookup"><span data-stu-id="8d175-136">Use your model</span></span>

* <span data-ttu-id="8d175-137">Abra *Program.cs* y reemplace el contenido por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="8d175-137">Open *Program.cs* and replace the contents with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="8d175-138">Pruebe la aplicación:</span><span class="sxs-lookup"><span data-stu-id="8d175-138">Test the app:</span></span>

 `dotnet run`

 <span data-ttu-id="8d175-139">Un blog se guarda en la base de datos y los detalles de todos los blogs se muestran en la consola.</span><span class="sxs-lookup"><span data-stu-id="8d175-139">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="8d175-140">Cambios del modelo:</span><span class="sxs-lookup"><span data-stu-id="8d175-140">Changing the model:</span></span>

- <span data-ttu-id="8d175-141">Si hace cambios en el modelo, puede usar el comando `dotnet ef migrations add` para aplicar scaffolding a una [migración](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) nueva con el fin de hacer los cambios de esquema correspondientes a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d175-141">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="8d175-142">Una vez que compruebe el código con scaffold (y haya hecho los cambios necesarios), puede usar el comando `dotnet ef database update` para aplicar los cambios a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d175-142">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="8d175-143">EF usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d175-143">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="8d175-144">SQLite no admite todas las migraciones (cambios de esquema) debido a las limitaciones de SQLite.</span><span class="sxs-lookup"><span data-stu-id="8d175-144">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="8d175-145">Consulte [Limitaciones de SQLite](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="8d175-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="8d175-146">En el caso de un desarrollo nuevo, considere eliminar la base de datos y cree una nueva en lugar de usar migraciones cuando cambie el modelo.</span><span class="sxs-lookup"><span data-stu-id="8d175-146">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8d175-147">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8d175-147">Additional Resources</span></span>

* <span data-ttu-id="8d175-148">[.NET Core: base de datos nueva con SQLite](xref:core/get-started/netcore/new-db-sqlite), un tutorial de EF para la consola multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="8d175-148">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="8d175-149">Introducción a ASP.NET Core MVC en Mac o Linux</span><span class="sxs-lookup"><span data-stu-id="8d175-149">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="8d175-150">Introducción a ASP.NET Core MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d175-150">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="8d175-151">Introducción a ASP.NET Core y Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8d175-151">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
