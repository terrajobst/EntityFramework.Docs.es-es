---
title: 'Introducción a UWP: base de datos nueva - EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2017
ms.locfileid: "26049838"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="4b17a-102">Introducción a EF Core en Plataforma universal de Windows (UWP) con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="4b17a-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

> [!NOTE]
> <span data-ttu-id="4b17a-103">En este tutorial se usa EF Core 2.0.1 (que se publicó junto con ASP.NET Core y el SDK 2.0.3 de .NET Core).</span><span class="sxs-lookup"><span data-stu-id="4b17a-103">This tutorial uses EF Core 2.0.1 (released alongside ASP.NET Core and .NET Core SDK 2.0.3).</span></span> <span data-ttu-id="4b17a-104">EF Core 2.0.0 carece de algunas correcciones de errores fundamentales necesarias para una buena experiencia con UWP.</span><span class="sxs-lookup"><span data-stu-id="4b17a-104">EF Core 2.0.0 lacks some crucial bug fixes required for a good UWP experience.</span></span>

<span data-ttu-id="4b17a-105">En este tutorial, compilará una aplicación de Plataforma universal de Windows que realiza el acceso a datos básicos en una base de datos SQLite local mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4b17a-105">In this walkthrough, you will build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4b17a-106">**Considere la posibilidad de evitar los tipos anónimos en las consultas LINQ en UWP**.</span><span class="sxs-lookup"><span data-stu-id="4b17a-106">**Consider avoiding anonymous types in LINQ queries on UWP**.</span></span> <span data-ttu-id="4b17a-107">Para implementar una aplicación de UWP en la tienda de aplicaciones, la aplicación debe estar compilada con .NET Native.</span><span class="sxs-lookup"><span data-stu-id="4b17a-107">Deploying a UWP application to the app store requires your application to be compiled with .NET Native.</span></span> <span data-ttu-id="4b17a-108">Las consultas con tipos anónimos tienen un menor rendimiento en .NET Native.</span><span class="sxs-lookup"><span data-stu-id="4b17a-108">Queries with anonymous types have worse performance on .NET Native.</span></span>

> [!TIP]
> <span data-ttu-id="4b17a-109">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="4b17a-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b17a-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4b17a-110">Prerequisites</span></span>

<span data-ttu-id="4b17a-111">Necesitará estos elementos para llevar a cabo este tutorial:</span><span class="sxs-lookup"><span data-stu-id="4b17a-111">The following items are required to complete this walkthrough:</span></span>

* <span data-ttu-id="4b17a-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span><span class="sxs-lookup"><span data-stu-id="4b17a-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span></span>

* <span data-ttu-id="4b17a-113">[SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="4b17a-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

* <span data-ttu-id="4b17a-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versión 15.4 o posterior con la carga de trabajo **Desarrollo de la Plataforma universal de Windows**.</span><span class="sxs-lookup"><span data-stu-id="4b17a-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 or later with the **Universal Windows Platform Development** workload.</span></span>

## <a name="create-a-new-model-project"></a><span data-ttu-id="4b17a-115">Creación de un proyecto de modelo nuevo</span><span class="sxs-lookup"><span data-stu-id="4b17a-115">Create a new model project</span></span>

> [!WARNING]
> <span data-ttu-id="4b17a-116">Debido a las limitaciones en el modo en que las herramientas de .NET Core interactúan con los proyectos de UWP, el modelo debe ubicarse en un proyecto no de UWP para poder ejecutar los comandos de migraciones en la consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="4b17a-116">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the Package Manager Console</span></span>

* <span data-ttu-id="4b17a-117">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4b17a-117">Open Visual Studio</span></span>

* <span data-ttu-id="4b17a-118">Archivo > Nuevo> Proyecto...</span><span class="sxs-lookup"><span data-stu-id="4b17a-118">File > New > Project...</span></span>

* <span data-ttu-id="4b17a-119">En el menú de la izquierda, seleccione Plantillas > Visual C#</span><span class="sxs-lookup"><span data-stu-id="4b17a-119">From the left menu select Templates > Visual C#</span></span>

* <span data-ttu-id="4b17a-120">Seleccione la plantilla del proyecto **Biblioteca de clases (.NET Standard)**</span><span class="sxs-lookup"><span data-stu-id="4b17a-120">Select the **Class Library (.NET Standard)** project template</span></span>

* <span data-ttu-id="4b17a-121">Asigne un nombre al proyecto y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="4b17a-121">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="4b17a-122">Instalación de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="4b17a-122">Install Entity Framework</span></span>

<span data-ttu-id="4b17a-123">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="4b17a-123">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="4b17a-124">Este tutorial usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="4b17a-124">This walkthrough uses SQLite.</span></span> <span data-ttu-id="4b17a-125">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="4b17a-125">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="4b17a-126">Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="4b17a-126">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="4b17a-127">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="4b17a-127">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="4b17a-128">Más adelante en este tutorial también usaremos parte de Entity Framework Tools para mantener la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4b17a-128">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="4b17a-129">Por lo tanto, también instalaremos el paquete de herramientas.</span><span class="sxs-lookup"><span data-stu-id="4b17a-129">So we will install the tools package as well.</span></span>

* <span data-ttu-id="4b17a-130">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="4b17a-130">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

* <span data-ttu-id="4b17a-131">Edite el archivo .csproj y reemplace `<TargetFramework>netstandard2.0</TargetFramework>` por `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span><span class="sxs-lookup"><span data-stu-id="4b17a-131">Edit the .csproj file and replace `<TargetFramework>netstandard2.0</TargetFramework>` with `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="4b17a-132">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="4b17a-132">Create your model</span></span>

<span data-ttu-id="4b17a-133">Es momento de definir un contexto y clases de entidad que constituyan el modelo.</span><span class="sxs-lookup"><span data-stu-id="4b17a-133">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="4b17a-134">Proyecto > Agregar clase...</span><span class="sxs-lookup"><span data-stu-id="4b17a-134">Project > Add Class...</span></span>

* <span data-ttu-id="4b17a-135">Escriba *Model.cs* como el nombre y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="4b17a-135">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="4b17a-136">Reemplace el contenido del archivo por el código siguiente</span><span class="sxs-lookup"><span data-stu-id="4b17a-136">Replace the contents of the file with the following code</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="4b17a-137">Creación de un proyecto de UWP nuevo</span><span class="sxs-lookup"><span data-stu-id="4b17a-137">Create a new UWP project</span></span>

* <span data-ttu-id="4b17a-138">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4b17a-138">Open Visual Studio</span></span>

* <span data-ttu-id="4b17a-139">Archivo > Nuevo> Proyecto...</span><span class="sxs-lookup"><span data-stu-id="4b17a-139">File > New > Project...</span></span>

* <span data-ttu-id="4b17a-140">En el menú de la izquierda, seleccione Plantillas > Visual C# > Windows Universal</span><span class="sxs-lookup"><span data-stu-id="4b17a-140">From the left menu select Templates > Visual C# > Windows Universal</span></span>

* <span data-ttu-id="4b17a-141">Seleccione la plantilla de proyecto **Aplicación en blanco (Universal Windows)**</span><span class="sxs-lookup"><span data-stu-id="4b17a-141">Select the **Blank App (Universal Windows)** project template</span></span>

* <span data-ttu-id="4b17a-142">Asigne un nombre al proyecto y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="4b17a-142">Give the project a name and click **OK**</span></span>

* <span data-ttu-id="4b17a-143">Establezca la versión de destino y la versión mínima al menos en `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span><span class="sxs-lookup"><span data-stu-id="4b17a-143">Set the target and minimum versions to at least `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span></span>

## <a name="create-your-database"></a><span data-ttu-id="4b17a-144">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="4b17a-144">Create your database</span></span>

<span data-ttu-id="4b17a-145">Ahora que ya tiene un modelo, puede usar las migraciones para crear una base de datos para usted.</span><span class="sxs-lookup"><span data-stu-id="4b17a-145">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="4b17a-146">Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="4b17a-146">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="4b17a-147">Seleccione el proyecto de modelo como el proyecto predeterminado y establézcalo como el proyecto de inicio</span><span class="sxs-lookup"><span data-stu-id="4b17a-147">Select the model project as the Default project and set it as the startup project</span></span>

* <span data-ttu-id="4b17a-148">Ejecute `Add-Migration MyFirstMigration` para aplicar scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="4b17a-148">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

<span data-ttu-id="4b17a-149">Como deseamos crear la base de datos en el dispositivo donde se ejecuta la aplicación, agregaremos código para aplicar cualquier migración pendiente a la base de datos local en el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4b17a-149">Since we want the database to be created on the device that the app runs on, we will add some code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="4b17a-150">La primera vez que se ejecute la aplicación, se encargará de crear la base de datos local.</span><span class="sxs-lookup"><span data-stu-id="4b17a-150">The first time that the app runs, this will take care of creating the local database for us.</span></span>

* <span data-ttu-id="4b17a-151">Haga clic con el botón derecho en **App.xaml** en el **Explorador de soluciones** y seleccione **Ver código**.</span><span class="sxs-lookup"><span data-stu-id="4b17a-151">Right-click on **App.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="4b17a-152">Agregue el texto resaltado en el inicio del archivo.</span><span class="sxs-lookup"><span data-stu-id="4b17a-152">Add the highlighted using to the start of the file</span></span>

* <span data-ttu-id="4b17a-153">Agregue el código resaltado para aplicar cualquier migración pendiente.</span><span class="sxs-lookup"><span data-stu-id="4b17a-153">Add the highlighted code to apply any pending migrations</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> <span data-ttu-id="4b17a-154">Si hace cambios en el modelo en el futuro, puede usar el comando `Add-Migration` para aplicar scaffolding a una migración nueva con el fin de aplicar los cambios correspondientes a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4b17a-154">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="4b17a-155">Toda migración pendiente se aplicará a la base de datos local de cada dispositivo cuando se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4b17a-155">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="4b17a-156">EF usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4b17a-156">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="4b17a-157">Uso del modelo</span><span class="sxs-lookup"><span data-stu-id="4b17a-157">Use your model</span></span>

<span data-ttu-id="4b17a-158">Ahora puede usar el modelo para realizar el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="4b17a-158">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="4b17a-159">Abra *MainPage.xaml*</span><span class="sxs-lookup"><span data-stu-id="4b17a-159">Open *MainPage.xaml*</span></span>

* <span data-ttu-id="4b17a-160">Agregue el controlador de carga de página y el contenido de UI que aparece resaltado a continuación</span><span class="sxs-lookup"><span data-stu-id="4b17a-160">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="4b17a-161">Ahora agregaremos código para conectar la interfaz de usuario con la base de datos</span><span class="sxs-lookup"><span data-stu-id="4b17a-161">Now we'll add code to wire up the UI with the database</span></span>

* <span data-ttu-id="4b17a-162">Haga clic con el botón derecho en **MainPage.xaml** en el **Explorador de soluciones** y seleccione **Ver código**.</span><span class="sxs-lookup"><span data-stu-id="4b17a-162">Right-click **MainPage.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="4b17a-163">Agregue el código resaltado de la lista siguiente.</span><span class="sxs-lookup"><span data-stu-id="4b17a-163">Add the highlighted code from the following listing</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

<span data-ttu-id="4b17a-164">Ahora puede ejecutar la aplicación para verla en acción.</span><span class="sxs-lookup"><span data-stu-id="4b17a-164">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="4b17a-165">Depurar > Iniciar sin depuración</span><span class="sxs-lookup"><span data-stu-id="4b17a-165">Debug > Start Without Debugging</span></span>

* <span data-ttu-id="4b17a-166">La aplicación se compilará e iniciará</span><span class="sxs-lookup"><span data-stu-id="4b17a-166">The application will build and launch</span></span>

* <span data-ttu-id="4b17a-167">Escriba una dirección URL y haga clic en el botón **Agregar**</span><span class="sxs-lookup"><span data-stu-id="4b17a-167">Enter a URL and click the **Add** button</span></span>

![imagen](_static/create.png)

![imagen](_static/list.png)

## <a name="next-steps"></a><span data-ttu-id="4b17a-170">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="4b17a-170">Next steps</span></span>

> [!TIP]
> <span data-ttu-id="4b17a-171">Es posible mejorar el rendimiento de `SaveChanges()` mediante la implementación de [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) en los tipos de entidad y mediante el uso de `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span><span class="sxs-lookup"><span data-stu-id="4b17a-171">`SaveChanges()` performance can be improved by implementing [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types and using `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span></span>

<span data-ttu-id="4b17a-172">¡Y listo!</span><span class="sxs-lookup"><span data-stu-id="4b17a-172">Tada!</span></span> <span data-ttu-id="4b17a-173">Ahora tiene una aplicación de UWP sencilla que ejecuta Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4b17a-173">You now have a simple UWP app running Entity Framework.</span></span>

<span data-ttu-id="4b17a-174">Consulte otros artículos de esta documentación para más información sobre las características de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4b17a-174">Check out other articles in this documentation to learn more about Entity Framework's features.</span></span>
