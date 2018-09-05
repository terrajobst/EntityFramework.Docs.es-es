---
title: 'Introducción a UWP: base de datos nueva - EF Core'
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996915"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="7d224-102">Introducción a EF Core en Plataforma universal de Windows (UWP) con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="7d224-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="7d224-103">En este tutorial, compilará una aplicación de Plataforma universal de Windows (UWP) que accede a datos básicos en una base de datos SQLite local mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7d224-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="7d224-104">[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="7d224-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d224-105">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7d224-105">Prerequisites</span></span>

* <span data-ttu-id="7d224-106">[Windows 10 Fall Creators Update (10.0; compilación 16299) o versiones posteriores](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="7d224-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="7d224-107">[Visual Studio 2017 versión 15.7 o versiones posteriores](https://www.visualstudio.com/downloads/) con la carga de trabajo **Desarrollo de la Plataforma universal de Windows**.</span><span class="sxs-lookup"><span data-stu-id="7d224-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="7d224-108">[SDK de .NET Core 2.1 o versiones posteriores](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="7d224-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-model-project"></a><span data-ttu-id="7d224-109">Creación de un proyecto de modelo</span><span class="sxs-lookup"><span data-stu-id="7d224-109">Create a model project</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7d224-110">Debido a las limitaciones en el modo en que las herramientas de .NET Core interactúan con los proyectos de UWP, el modelo debe ubicarse en un proyecto no de UWP para poder ejecutar los comandos de migraciones en la **consola del Administrador de paquetes** (PMC).</span><span class="sxs-lookup"><span data-stu-id="7d224-110">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the **Package Manager Console** (PMC)</span></span>

* <span data-ttu-id="7d224-111">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d224-111">Open Visual Studio</span></span>

* <span data-ttu-id="7d224-112">**Archivo > Nuevo > Proyecto**</span><span class="sxs-lookup"><span data-stu-id="7d224-112">**File > New > Project**</span></span>

* <span data-ttu-id="7d224-113">En el menú de la izquierda, seleccione **Instalado > Visual C# > .NET Standard**.</span><span class="sxs-lookup"><span data-stu-id="7d224-113">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="7d224-114">Seleccione la plantilla **Biblioteca de clases (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="7d224-114">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="7d224-115">Asigne el nombre *Blogging.Model* al proyecto.</span><span class="sxs-lookup"><span data-stu-id="7d224-115">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="7d224-116">Asigne el nombre *Blogging* a la solución.</span><span class="sxs-lookup"><span data-stu-id="7d224-116">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="7d224-117">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7d224-117">Click **OK**.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="7d224-118">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7d224-118">Install Entity Framework Core</span></span>

<span data-ttu-id="7d224-119">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="7d224-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="7d224-120">En este tutorial se usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="7d224-120">This tutorial uses SQLite.</span></span> <span data-ttu-id="7d224-121">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="7d224-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="7d224-122">**Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="7d224-122">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="7d224-123">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="7d224-123">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="7d224-124">Más adelante en este tutorial, usará parte de las herramientas de Entity Framework Core para mantener la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d224-124">Later in this tutorial you will be using some Entity Framework Core tools to maintain the database.</span></span> <span data-ttu-id="7d224-125">Por lo tanto, instale el paquete de herramientas.</span><span class="sxs-lookup"><span data-stu-id="7d224-125">So install the tools package as well.</span></span>

* <span data-ttu-id="7d224-126">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="7d224-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="7d224-127">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="7d224-127">Create the model</span></span>

<span data-ttu-id="7d224-128">Ahora hay que definir un contexto y clases de entidad que constituyan el modelo.</span><span class="sxs-lookup"><span data-stu-id="7d224-128">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="7d224-129">Elimine *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="7d224-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="7d224-130">Cree *Model.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7d224-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="7d224-131">Creación de un proyecto de UWP nuevo</span><span class="sxs-lookup"><span data-stu-id="7d224-131">Create a new UWP project</span></span>

* <span data-ttu-id="7d224-132">En el **Explorador de soluciones**, haga clic con el botón derecho en la solución y elija **Agregar > Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="7d224-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="7d224-133">En el menú izquierdo, seleccione **Instalado > Visual C# > Universal de Windows**.</span><span class="sxs-lookup"><span data-stu-id="7d224-133">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="7d224-134">Seleccione la plantilla de proyecto **Aplicación en blanco (Universal de Windows)**.</span><span class="sxs-lookup"><span data-stu-id="7d224-134">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="7d224-135">Asigne el nombre *Blogging.UWP* al proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7d224-135">Name the project *Blogging.UWP*, and click **OK**</span></span>

* <span data-ttu-id="7d224-136">Establezca la versión de destino y la versión mínima al menos en **Windows 10 Fall Creators Update (10.0; compilación 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="7d224-136">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="7d224-137">Creación de la migración inicial</span><span class="sxs-lookup"><span data-stu-id="7d224-137">Create the initial migration</span></span>

<span data-ttu-id="7d224-138">Ahora que tiene un modelo, configure la aplicación para que se cree una base de datos la primera vez que se ejecute.</span><span class="sxs-lookup"><span data-stu-id="7d224-138">Now that you have a model, set up the app to create a database the first time it runs.</span></span> <span data-ttu-id="7d224-139">En esta sección, creará la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="7d224-139">In this section, you create the initial migration.</span></span> <span data-ttu-id="7d224-140">En la sección siguiente, agregue código que se aplique a esta migración cuando se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7d224-140">In the following section, you add code that applies this migration when the app starts.</span></span>

<span data-ttu-id="7d224-141">Las herramientas de migración requieren un proyecto de inicio que no sea de UWP, por lo que primero debe crearlo.</span><span class="sxs-lookup"><span data-stu-id="7d224-141">Migrations tools require a non-UWP startup project, so create that first.</span></span>

* <span data-ttu-id="7d224-142">En el **Explorador de soluciones**, haga clic con el botón derecho en la solución y elija **Agregar > Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="7d224-142">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="7d224-143">En el menú de la izquierda, seleccione **Instalado > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="7d224-143">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="7d224-144">Seleccione la plantilla del proyecto **Aplicación de consola (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="7d224-144">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="7d224-145">Asigne el nombre *Blogging.Migrations.Startup* al proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7d224-145">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="7d224-146">Agregue una referencia al proyecto *Blogging.Model* al proyecto *Blogging.Migrations.Startup*.</span><span class="sxs-lookup"><span data-stu-id="7d224-146">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

<span data-ttu-id="7d224-147">Ya puede crear su migración inicial.</span><span class="sxs-lookup"><span data-stu-id="7d224-147">Now you can create your initial migration.</span></span>

* <span data-ttu-id="7d224-148">**Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="7d224-148">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="7d224-149">Seleccione el proyecto *Blogging.Model* como **proyecto predeterminado**.</span><span class="sxs-lookup"><span data-stu-id="7d224-149">Select the *Blogging.Model* project as the **Default project**.</span></span>

* <span data-ttu-id="7d224-150">En el **Explorador de soluciones**, establezca el proyecto *Blogging.Migrations.Startup* como proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="7d224-150">In **Solution Explorer**, set the *Blogging.Migrations.Startup* project as the startup project.</span></span>

* <span data-ttu-id="7d224-151">Ejecute `Add-Migration InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="7d224-151">Run `Add-Migration InitialCreate`.</span></span>

  <span data-ttu-id="7d224-152">Este comando aplica el scaffolding a una migración que crea el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="7d224-152">This command scaffolds a migration that creates the initial set of tables for your model.</span></span>

## <a name="create-the-database-on-app-startup"></a><span data-ttu-id="7d224-153">Creación de la base de datos al iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="7d224-153">Create the database on app startup</span></span>

<span data-ttu-id="7d224-154">Puesto que quiere crear la base de datos en el dispositivo donde se ejecuta la aplicación, debe agregar código para aplicar cualquier migración pendiente a la base de datos local en el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7d224-154">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="7d224-155">La primera vez que se ejecute la aplicación, esta se encargará de crear la base de datos local.</span><span class="sxs-lookup"><span data-stu-id="7d224-155">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="7d224-156">Agregue una referencia del proyecto *Blogging.UWP* al proyecto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="7d224-156">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="7d224-157">Abra *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="7d224-157">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="7d224-158">Agregue el código resaltado para aplicar cualquier migración pendiente.</span><span class="sxs-lookup"><span data-stu-id="7d224-158">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="7d224-159">Si modifica el modelo, use el comando `Add-Migration` para aplicar el scaffolding a una migración nueva con el fin de aplicar los cambios correspondientes a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d224-159">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="7d224-160">Toda migración pendiente se aplicará a la base de datos local de cada dispositivo cuando se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7d224-160">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="7d224-161">EF usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d224-161">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="7d224-162">Uso del modelo</span><span class="sxs-lookup"><span data-stu-id="7d224-162">Use the model</span></span>

<span data-ttu-id="7d224-163">Ahora puede usar el modelo para realizar el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="7d224-163">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="7d224-164">Abra *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="7d224-164">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="7d224-165">Agregue el controlador de carga de página y el contenido de UI que aparece resaltado a continuación</span><span class="sxs-lookup"><span data-stu-id="7d224-165">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="7d224-166">Ahora, agregue código para conectar la interfaz de usuario con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7d224-166">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="7d224-167">Abra *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="7d224-167">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="7d224-168">Agregue el código resaltado de la lista siguiente:</span><span class="sxs-lookup"><span data-stu-id="7d224-168">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="7d224-169">Ahora puede ejecutar la aplicación para verla en acción.</span><span class="sxs-lookup"><span data-stu-id="7d224-169">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="7d224-170">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto *Blogging.UWP* y seleccione **Implementar**.</span><span class="sxs-lookup"><span data-stu-id="7d224-170">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="7d224-171">Establezca *Blogging.UWP* como el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="7d224-171">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="7d224-172">**Depurar > Iniciar sin depuración**</span><span class="sxs-lookup"><span data-stu-id="7d224-172">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="7d224-173">La aplicación se compilará y ejecutará.</span><span class="sxs-lookup"><span data-stu-id="7d224-173">The app builds and runs.</span></span>

* <span data-ttu-id="7d224-174">Escriba una dirección URL y haga clic en el botón **Agregar**</span><span class="sxs-lookup"><span data-stu-id="7d224-174">Enter a URL and click the **Add** button</span></span>

  ![imagen](_static/create.png)

  ![imagen](_static/list.png)

  <span data-ttu-id="7d224-177">¡Y listo!</span><span class="sxs-lookup"><span data-stu-id="7d224-177">Tada!</span></span> <span data-ttu-id="7d224-178">Ahora tiene una aplicación de UWP sencilla que ejecuta Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7d224-178">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d224-179">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7d224-179">Next steps</span></span>

<span data-ttu-id="7d224-180">Para obtener información sobre la compatibilidad y el rendimiento que debe conocer al usar EF Core con UWP, vea [Implementaciones de .NET compatibles con EF Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="7d224-180">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="7d224-181">Consulte otros artículos de esta documentación para obtener más información sobre las características de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7d224-181">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
