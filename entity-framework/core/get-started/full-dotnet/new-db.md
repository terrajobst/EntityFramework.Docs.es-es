---
title: 'Introducción a .NET Framework: base de datos nueva - EF Core'
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 088ac915041489242eb8090e7bf3a2bdc8036534
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614433"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="c8acc-102">Introducción a EF Core en .NET Framework con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="c8acc-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="c8acc-103">En este tutorial, compilará una aplicación de consola que realiza el acceso a datos básicos en una base de datos de Microsoft SQL Server mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c8acc-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="c8acc-104">Se usan migraciones para crear la base de datos a partir del modelo.</span><span class="sxs-lookup"><span data-stu-id="c8acc-104">You use migrations to create the database from a model.</span></span>

<span data-ttu-id="c8acc-105">[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span><span class="sxs-lookup"><span data-stu-id="c8acc-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8acc-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c8acc-106">Prerequisites</span></span>

* [<span data-ttu-id="c8acc-107">Visual Studio 2017 versión 15.7 o posterior</span><span class="sxs-lookup"><span data-stu-id="c8acc-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a><span data-ttu-id="c8acc-108">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="c8acc-108">Create a new project</span></span>

* <span data-ttu-id="c8acc-109">Abra Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="c8acc-109">Open Visual Studio 2017</span></span>

* <span data-ttu-id="c8acc-110">**Archivo > Nuevo > Proyecto...**</span><span class="sxs-lookup"><span data-stu-id="c8acc-110">**File > New > Project...**</span></span>

* <span data-ttu-id="c8acc-111">En el menú izquierdo, seleccione **Instalado > Visual C# > Escritorio de Windows**.</span><span class="sxs-lookup"><span data-stu-id="c8acc-111">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="c8acc-112">Seleccione la plantilla del proyecto **Aplicación de consola (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="c8acc-112">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="c8acc-113">Asegúrese de que el proyecto tenga **.NET Framework 4.6.1** o posterior como destino.</span><span class="sxs-lookup"><span data-stu-id="c8acc-113">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="c8acc-114">Asigne el nombre *ConsoleApp.NewDb* al proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c8acc-114">Name the project *ConsoleApp.NewDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="c8acc-115">Instalación de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c8acc-115">Install Entity Framework</span></span>

<span data-ttu-id="c8acc-116">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="c8acc-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="c8acc-117">Este tutorial usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c8acc-117">This tutorial uses SQL Server.</span></span> <span data-ttu-id="c8acc-118">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c8acc-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="c8acc-119">Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="c8acc-119">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="c8acc-120">Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="c8acc-120">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="c8acc-121">Más adelante en este tutorial usará parte de Entity Framework Tools para mantener la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8acc-121">Later in this tutorial you use some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="c8acc-122">Por lo tanto, instale el paquete de herramientas.</span><span class="sxs-lookup"><span data-stu-id="c8acc-122">So install the tools package as well.</span></span>

* <span data-ttu-id="c8acc-123">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="c8acc-123">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="c8acc-124">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="c8acc-124">Create the model</span></span>

<span data-ttu-id="c8acc-125">Ahora hay que definir un contexto y clases de entidad que constituyan el modelo.</span><span class="sxs-lookup"><span data-stu-id="c8acc-125">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="c8acc-126">**Proyecto > Agregar clase...**</span><span class="sxs-lookup"><span data-stu-id="c8acc-126">**Project > Add Class...**</span></span>

* <span data-ttu-id="c8acc-127">Escriba *Model.cs* como el nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c8acc-127">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="c8acc-128">Reemplace el contenido del archivo por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="c8acc-128">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> <span data-ttu-id="c8acc-129">En una aplicación real, lo habitual sería colocar cada clase en un archivo independiente, y la cadena de conexión, en un archivo de configuración o una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="c8acc-129">In a real application you would put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="c8acc-130">Por simplicidad, en este tutorial todos estos elementos están en un solo archivo de código.</span><span class="sxs-lookup"><span data-stu-id="c8acc-130">For the sake of simplicity, everything is in a single code file for this tutorial.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="c8acc-131">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="c8acc-131">Create the database</span></span>

<span data-ttu-id="c8acc-132">Ahora que ya tiene un modelo, puede usar las migraciones para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8acc-132">Now that you have a model, you can use migrations to create a database.</span></span>

* <span data-ttu-id="c8acc-133">**Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="c8acc-133">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="c8acc-134">Ejecute `Add-Migration InitialCreate` para aplicar scaffolding a una migración y, así, crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="c8acc-134">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for the model.</span></span>

* <span data-ttu-id="c8acc-135">Ejecute `Update-Database` para aplicar la migración nueva a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8acc-135">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="c8acc-136">Como la base de datos todavía no existe, se creará antes de aplicar la migración.</span><span class="sxs-lookup"><span data-stu-id="c8acc-136">Because the database doesn't exist yet, it will be created before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="c8acc-137">Si hace cambios en el modelo, puede usar el comando `Add-Migration` para aplicar scaffolding a una migración nueva con el fin de hacer los cambios de esquema correspondientes a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8acc-137">If you make changes to the model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="c8acc-138">Una vez que compruebe el código con scaffold (y haya hecho los cambios necesarios), puede usar el comando `Update-Database` para aplicar los cambios a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8acc-138">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
> <span data-ttu-id="c8acc-139">EF usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8acc-139">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="c8acc-140">Uso del modelo</span><span class="sxs-lookup"><span data-stu-id="c8acc-140">Use the model</span></span>

<span data-ttu-id="c8acc-141">Ahora puede usar el modelo para realizar el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="c8acc-141">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="c8acc-142">Abra *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="c8acc-142">Open *Program.cs*</span></span>

* <span data-ttu-id="c8acc-143">Reemplace el contenido del archivo por el código siguiente</span><span class="sxs-lookup"><span data-stu-id="c8acc-143">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* <span data-ttu-id="c8acc-144">**Depurar > Iniciar sin depuración**</span><span class="sxs-lookup"><span data-stu-id="c8acc-144">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="c8acc-145">Verá que un blog se guarda en la base de datos y, luego, los detalles de todos los blogs se imprimen en la consola.</span><span class="sxs-lookup"><span data-stu-id="c8acc-145">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![imagen](_static/output-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="c8acc-147">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c8acc-147">Additional Resources</span></span>

* [<span data-ttu-id="c8acc-148">EF Core en .NET Framework con una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="c8acc-148">EF Core on .NET Framework with an existing database</span></span>](xref:core/get-started/full-dotnet/existing-db)
* <span data-ttu-id="c8acc-149">[EF Core en .NET Core con una base de datos nueva (SQLite)](xref:core/get-started/netcore/new-db-sqlite), un tutorial de EF para la consola multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="c8acc-149">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
