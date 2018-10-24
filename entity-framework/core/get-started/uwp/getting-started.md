---
title: 'Introducción a UWP: base de datos nueva - EF Core'
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 48d26adbe17e4734753a7ada547b9c13317bef0d
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315625"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="75c13-102">Introducción a EF Core en Plataforma universal de Windows (UWP) con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="75c13-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="75c13-103">En este tutorial, compilará una aplicación de Plataforma universal de Windows (UWP) que accede a datos básicos en una base de datos SQLite local mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="75c13-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="75c13-104">[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="75c13-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75c13-105">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="75c13-105">Prerequisites</span></span>

* <span data-ttu-id="75c13-106">[Windows 10 Fall Creators Update (10.0; compilación 16299) o versiones posteriores](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="75c13-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="75c13-107">[Visual Studio 2017 versión 15.7 o versiones posteriores](https://www.visualstudio.com/downloads/) con la carga de trabajo **Desarrollo de la Plataforma universal de Windows**.</span><span class="sxs-lookup"><span data-stu-id="75c13-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="75c13-108">[SDK de .NET Core 2.1 o versiones posteriores](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="75c13-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75c13-109">En este tutorial se usan comandos de [migraciones](xref:core/managing-schemas/migrations/index) de Entity Framework Core para crear y actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="75c13-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="75c13-110">Estos comandos no funcionan directamente con proyectos de UWP.</span><span class="sxs-lookup"><span data-stu-id="75c13-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="75c13-111">Por este motivo, el modelo de datos de la aplicación se coloca en un proyecto de biblioteca compartida y se usa una aplicación de consola .NET Core independiente para ejecutar los comandos.</span><span class="sxs-lookup"><span data-stu-id="75c13-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="75c13-112">Crear un proyecto de biblioteca que contenga el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="75c13-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="75c13-113">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="75c13-113">Open Visual Studio</span></span>

* <span data-ttu-id="75c13-114">**Archivo > Nuevo > Proyecto**</span><span class="sxs-lookup"><span data-stu-id="75c13-114">**File > New > Project**</span></span>

* <span data-ttu-id="75c13-115">En el menú de la izquierda, seleccione **Instalado > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="75c13-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="75c13-116">Seleccione la plantilla **Biblioteca de clases (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="75c13-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="75c13-117">Asigne el nombre *Blogging.Model* al proyecto.</span><span class="sxs-lookup"><span data-stu-id="75c13-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="75c13-118">Asigne el nombre *Blogging* a la solución.</span><span class="sxs-lookup"><span data-stu-id="75c13-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="75c13-119">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="75c13-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="75c13-120">Instalar el runtime de Entity Framework Core en el proyecto de modelo de datos</span><span class="sxs-lookup"><span data-stu-id="75c13-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="75c13-121">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="75c13-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="75c13-122">En este tutorial se usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="75c13-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="75c13-123">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="75c13-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="75c13-124">**Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="75c13-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="75c13-125">Asegúrese de que el proyecto de biblioteca *Blogging.Model* esté seleccionado como **Proyecto predeterminado** en la consola del administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="75c13-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="75c13-126">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="75c13-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="75c13-127">Crear el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="75c13-127">Create the data model</span></span>

<span data-ttu-id="75c13-128">Ahora hay que definir el elemento *DbContext* y las clases de entidad que conforman el modelo.</span><span class="sxs-lookup"><span data-stu-id="75c13-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="75c13-129">Elimine *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="75c13-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="75c13-130">Cree *Model.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="75c13-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="75c13-131">Crear un nuevo proyecto de consola para ejecutar comandos de migraciones</span><span class="sxs-lookup"><span data-stu-id="75c13-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="75c13-132">En el **Explorador de soluciones**, haga clic con el botón derecho en la solución y elija **Agregar > Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="75c13-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="75c13-133">En el menú de la izquierda, seleccione **Instalado > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="75c13-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="75c13-134">Seleccione la plantilla del proyecto **Aplicación de consola (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="75c13-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="75c13-135">Asigne el nombre *Blogging.Migrations.Startup* al proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="75c13-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="75c13-136">Agregue una referencia al proyecto *Blogging.Model* al proyecto *Blogging.Migrations.Startup*.</span><span class="sxs-lookup"><span data-stu-id="75c13-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="75c13-137">Instalar las herramientas de Entity Framework Core en el proyecto de inicio de migraciones</span><span class="sxs-lookup"><span data-stu-id="75c13-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="75c13-138">Para habilitar los comandos de migraciones de EF Core en la consola del administrador de paquetes, instale el paquete de herramientas de EF Core en la aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="75c13-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="75c13-139">**Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="75c13-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="75c13-140">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`.</span><span class="sxs-lookup"><span data-stu-id="75c13-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="75c13-141">Creación de la migración inicial</span><span class="sxs-lookup"><span data-stu-id="75c13-141">Create the initial migration</span></span>

 <span data-ttu-id="75c13-142">Cree la migración inicial al especificar la aplicación de consola como proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="75c13-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="75c13-143">Ejecute `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`.</span><span class="sxs-lookup"><span data-stu-id="75c13-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="75c13-144">Este comando aplica scaffolding a una migración que crea el conjunto inicial de tablas de base de datos para el modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="75c13-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="75c13-145">Crear el proyecto de UWP</span><span class="sxs-lookup"><span data-stu-id="75c13-145">Create the UWP project</span></span>

* <span data-ttu-id="75c13-146">En el **Explorador de soluciones**, haga clic con el botón derecho en la solución y elija **Agregar > Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="75c13-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="75c13-147">En el menú izquierdo, seleccione **Instalado > Visual C# > Universal de Windows**.</span><span class="sxs-lookup"><span data-stu-id="75c13-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="75c13-148">Seleccione la plantilla de proyecto **Aplicación en blanco (Universal de Windows)**.</span><span class="sxs-lookup"><span data-stu-id="75c13-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="75c13-149">Asigne el nombre *Blogging.UWP* al proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="75c13-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75c13-150">Establezca la versión de destino y la versión mínima al menos en **Windows 10 Fall Creators Update (10.0; compilación 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="75c13-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="75c13-151">Las versiones anteriores de Windows 10 no admiten .NET Standard 2.0, que Entity Framework Core necesita.</span><span class="sxs-lookup"><span data-stu-id="75c13-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="75c13-152">Agregar código para crear la base de datos al inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="75c13-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="75c13-153">Puesto que quiere crear la base de datos en el dispositivo donde se ejecuta la aplicación, debe agregar código para aplicar cualquier migración pendiente a la base de datos local en el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="75c13-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="75c13-154">La primera vez que se ejecute la aplicación, esta se encargará de crear la base de datos local.</span><span class="sxs-lookup"><span data-stu-id="75c13-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="75c13-155">Agregue una referencia del proyecto *Blogging.UWP* al proyecto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="75c13-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="75c13-156">Abra *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="75c13-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="75c13-157">Agregue el código resaltado para aplicar cualquier migración pendiente.</span><span class="sxs-lookup"><span data-stu-id="75c13-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="75c13-158">Si modifica el modelo, use el comando `Add-Migration` para aplicar el scaffolding a una migración nueva con el fin de aplicar los cambios correspondientes a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="75c13-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="75c13-159">Toda migración pendiente se aplicará a la base de datos local de cada dispositivo cuando se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="75c13-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="75c13-160">EF Core usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="75c13-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="75c13-161">Usar el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="75c13-161">Use the data model</span></span>

<span data-ttu-id="75c13-162">Ahora puede usar EF Core para realizar el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="75c13-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="75c13-163">Abra *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="75c13-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="75c13-164">Agregue el controlador de carga de página y el contenido de UI que aparece resaltado a continuación</span><span class="sxs-lookup"><span data-stu-id="75c13-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="75c13-165">Ahora, agregue código para conectar la interfaz de usuario con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="75c13-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="75c13-166">Abra *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="75c13-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="75c13-167">Agregue el código resaltado de la lista siguiente:</span><span class="sxs-lookup"><span data-stu-id="75c13-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="75c13-168">Ahora puede ejecutar la aplicación para verla en acción.</span><span class="sxs-lookup"><span data-stu-id="75c13-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="75c13-169">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto *Blogging.UWP* y seleccione **Implementar**.</span><span class="sxs-lookup"><span data-stu-id="75c13-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="75c13-170">Establezca *Blogging.UWP* como el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="75c13-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="75c13-171">**Depurar > Iniciar sin depuración**</span><span class="sxs-lookup"><span data-stu-id="75c13-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="75c13-172">La aplicación se compilará y ejecutará.</span><span class="sxs-lookup"><span data-stu-id="75c13-172">The app builds and runs.</span></span>

* <span data-ttu-id="75c13-173">Escriba una dirección URL y haga clic en el botón **Agregar**</span><span class="sxs-lookup"><span data-stu-id="75c13-173">Enter a URL and click the **Add** button</span></span>

  ![imagen](_static/create.png)

  ![imagen](_static/list.png)

  <span data-ttu-id="75c13-176">¡Y listo!</span><span class="sxs-lookup"><span data-stu-id="75c13-176">Tada!</span></span> <span data-ttu-id="75c13-177">Ahora tiene una aplicación de UWP sencilla que ejecuta Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="75c13-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="75c13-178">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="75c13-178">Next steps</span></span>

<span data-ttu-id="75c13-179">Para obtener información sobre la compatibilidad y el rendimiento que debe conocer al usar EF Core con UWP, vea [Implementaciones de .NET compatibles con EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="75c13-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="75c13-180">Consulte otros artículos de esta documentación para obtener más información sobre las características de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="75c13-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
