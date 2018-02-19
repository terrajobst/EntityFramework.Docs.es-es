---
title: "Introducción a ASP.NET Core: base de datos nueva - EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: f6ed19d3c5d2ae8d1f5756558e50c1f0dddd2f07
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="37ef6-102">Introducción a EF Core en ASP.NET Core con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="37ef6-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="37ef6-103">En este tutorial, compilará una aplicación de MVC de ASP.NET Core que realiza el acceso a datos básicos mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="37ef6-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="37ef6-104">Usará las migraciones para crear la base de datos a partir del modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="37ef6-104">You will use migrations to create the database from your EF Core model.</span></span> <span data-ttu-id="37ef6-105">Consulte [Recursos adicionales](#additional-resources) para más tutoriales de Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="37ef6-105">See [Additional Resources](#additional-resources) for more Entity Framework Core tutorials.</span></span>

<span data-ttu-id="37ef6-106">En este tutorial se necesita:</span><span class="sxs-lookup"><span data-stu-id="37ef6-106">This tutorial requires:</span></span>
* <span data-ttu-id="37ef6-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) con estas cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="37ef6-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="37ef6-108">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="37ef6-108">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="37ef6-109">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="37ef6-109">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="37ef6-110">[SDK de .NET Core 2.0](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="37ef6-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

> [!TIP]  
> <span data-ttu-id="37ef6-111">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="37ef6-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) on GitHub.</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="37ef6-112">Creación de un proyecto nuevo en Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="37ef6-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="37ef6-113">**Archivo > Nuevo > Proyecto**</span><span class="sxs-lookup"><span data-stu-id="37ef6-113">**File > New > Project**</span></span>
* <span data-ttu-id="37ef6-114">En el menú de la izquierda, seleccione **Instalado > Plantillas > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-114">From the left menu select **Installed > Templates > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="37ef6-115">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-115">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="37ef6-116">Escriba **EFGetStarted.AspNetCore.NewDb** como el nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-116">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="37ef6-117">En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37ef6-117">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="37ef6-118">Asegúrese de que las opciones **.NET Core** y **ASP.NET Core 2.0** estén seleccionadas en las listas desplegables.</span><span class="sxs-lookup"><span data-stu-id="37ef6-118">Ensure the options **.NET Core** and **ASP.NET Core 2.0** are selected in the drop down lists</span></span>
  * <span data-ttu-id="37ef6-119">Seleccione la plantilla de proyecto **Aplicación web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-119">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="37ef6-120">Asegúrese de que la **autenticación** esté establecida en **Sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-120">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="37ef6-121">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-121">Click **OK**</span></span>

<span data-ttu-id="37ef6-122">Advertencia: Si usa **Cuentas de usuario individuales** en lugar de **Ninguna** para **Autenticación**, se agregará un modelo de Entity Framework Core al proyecto en `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="37ef6-122">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="37ef6-123">Con las técnicas que aprenderá en este tutorial podrá elegir agregar un segundo modelo o extender este modelo existente para contener las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="37ef6-123">Using the techniques you will learn in this walkthrough, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="37ef6-124">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="37ef6-124">Install Entity Framework Core</span></span>

<span data-ttu-id="37ef6-125">Instale el paquete para los proveedores de bases de datos de EF Core a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="37ef6-125">Install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="37ef6-126">Este tutorial usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="37ef6-126">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="37ef6-127">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="37ef6-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="37ef6-128">**Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="37ef6-128">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="37ef6-129">Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="37ef6-129">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="37ef6-130">Usaremos parte de Entity Framework Tools para crear una base de datos desde el modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="37ef6-130">We will be using some Entity Framework Core Tools to create a database from your EF Core model.</span></span> <span data-ttu-id="37ef6-131">Por lo tanto, también instalaremos el paquete de herramientas:</span><span class="sxs-lookup"><span data-stu-id="37ef6-131">So we will install the tools package as well:</span></span>

* <span data-ttu-id="37ef6-132">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="37ef6-132">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="37ef6-133">Usaremos las herramientas de scaffolding de ASP.NET Core para crear más adelante controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="37ef6-133">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="37ef6-134">Por lo tanto, también instalaremos el paquete de diseño:</span><span class="sxs-lookup"><span data-stu-id="37ef6-134">So we will install this design package as well:</span></span>

* <span data-ttu-id="37ef6-135">Ejecute `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`.</span><span class="sxs-lookup"><span data-stu-id="37ef6-135">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="37ef6-136">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="37ef6-136">Create the model</span></span>

<span data-ttu-id="37ef6-137">Defina un contexto y clases de entidad que constituirán el modelo:</span><span class="sxs-lookup"><span data-stu-id="37ef6-137">Define a context and entity classes that make up the model:</span></span>

* <span data-ttu-id="37ef6-138">Haga clic con el botón derecho en la carpeta **Modelos** y seleccione **Agregar > Clase**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-138">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="37ef6-139">Escriba **Model.cs** como el nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-139">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="37ef6-140">Reemplace el contenido del archivo por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="37ef6-140">Replace the contents of the file with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="37ef6-141">Nota: En una aplicación real, habitualmente colocaría cada clase del modelo en un archivo independiente.</span><span class="sxs-lookup"><span data-stu-id="37ef6-141">Note: In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="37ef6-142">Por simplicidad, en este tutorial colocamos todas las clases en un solo archivo.</span><span class="sxs-lookup"><span data-stu-id="37ef6-142">For the sake of simplicity, we are putting all the classes in one file for this tutorial.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="37ef6-143">Registro del contenido con inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="37ef6-143">Register your context with dependency injection</span></span>

<span data-ttu-id="37ef6-144">Los servicios (como `BloggingContext`) se registran con [inserción de dependencias](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="37ef6-144">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="37ef6-145">Luego se proporcionan estos servicios a los componentes que los necesitan (como los controladores MVC) a través de propiedades o parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="37ef6-145">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="37ef6-146">Con el fin de que los controladores MVC usen `BloggingContext`, lo registraremos como servicio.</span><span class="sxs-lookup"><span data-stu-id="37ef6-146">In order for our MVC controllers to make use of `BloggingContext` we will register it as a service.</span></span>

* <span data-ttu-id="37ef6-147">Abra **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="37ef6-147">Open **Startup.cs**</span></span>
* <span data-ttu-id="37ef6-148">Agregue las instrucciones `using` siguientes:</span><span class="sxs-lookup"><span data-stu-id="37ef6-148">Add the following `using` statements:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="37ef6-149">Agregue el método `AddDbContext` para registrarlo como servicio:</span><span class="sxs-lookup"><span data-stu-id="37ef6-149">Add the `AddDbContext` method to register it as a service:</span></span>

* <span data-ttu-id="37ef6-150">Agregue el código siguiente al método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="37ef6-150">Add the following code to the `ConfigureServices` method:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

<span data-ttu-id="37ef6-151">Nota: Una aplicación real habitualmente colocaría la cadena de conexión en un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="37ef6-151">Note: A real app would generally put the connection string in a configuration file.</span></span> <span data-ttu-id="37ef6-152">Por simplicidad, se define en el código.</span><span class="sxs-lookup"><span data-stu-id="37ef6-152">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="37ef6-153">Para más información, consulte [Cadenas de conexión](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="37ef6-153">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="37ef6-154">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="37ef6-154">Create your database</span></span>

<span data-ttu-id="37ef6-155">Una vez que ya tiene un modelo, puede usar las [migraciones](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="37ef6-155">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="37ef6-156">Abra la PMC:</span><span class="sxs-lookup"><span data-stu-id="37ef6-156">Open the PMC:</span></span>

  <span data-ttu-id="37ef6-157">**Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="37ef6-157">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="37ef6-158">Ejecute `Add-Migration InitialCreate` para aplicar scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="37ef6-158">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="37ef6-159">Si recibe un error que indica `The term 'add-migration' is not recognized as the name of a cmdlet`, cierre Visual Studio y vuelva a abrirlo.</span><span class="sxs-lookup"><span data-stu-id="37ef6-159">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="37ef6-160">Ejecute `Update-Database` para aplicar la migración nueva a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="37ef6-160">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="37ef6-161">Este comando crea la base de datos antes de aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="37ef6-161">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="37ef6-162">Creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="37ef6-162">Create a controller</span></span>

<span data-ttu-id="37ef6-163">Habilitación de scaffolding en el proyecto:</span><span class="sxs-lookup"><span data-stu-id="37ef6-163">Enable scaffolding in the project:</span></span>

* <span data-ttu-id="37ef6-164">Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-164">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="37ef6-165">Seleccione **Dependencias mínimas** y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-165">Select **Minimal Dependencies** and click **Add**.</span></span>
* <span data-ttu-id="37ef6-166">Puede omitir o eliminar el archivo *ScaffoldingReadMe.txt*.</span><span class="sxs-lookup"><span data-stu-id="37ef6-166">You can ignore or delete the *ScaffoldingReadMe.txt* file.</span></span>

<span data-ttu-id="37ef6-167">Ahora que se habilitó scaffolding, podemos aplicarlo a un controlador de la entidad `Blog`.</span><span class="sxs-lookup"><span data-stu-id="37ef6-167">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="37ef6-168">Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="37ef6-169">Seleccione **Controlador de MVC con vistas que usan Entity Framework** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**.</span></span>
* <span data-ttu-id="37ef6-170">Establezca la **clase de modelo** en **Blog** y **Clase de contexto de datos** en **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="37ef6-171">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="37ef6-171">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="37ef6-172">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="37ef6-172">Run the application</span></span>

<span data-ttu-id="37ef6-173">Presione F5 para ejecutar y probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="37ef6-173">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="37ef6-174">Navegue a `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="37ef6-174">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="37ef6-175">Use el vínculo de creación para crear algunas entradas de blog.</span><span class="sxs-lookup"><span data-stu-id="37ef6-175">Use the create link to create some blog entries.</span></span> <span data-ttu-id="37ef6-176">Pruebe los detalles y elimine los vínculos.</span><span class="sxs-lookup"><span data-stu-id="37ef6-176">Test the details and delete links.</span></span>

![imagen](_static/create.png)

![imagen](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="37ef6-179">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="37ef6-179">Additional Resources</span></span>

* <span data-ttu-id="37ef6-180">[EF: base de datos nueva con SQLite](xref:core/get-started/netcore/new-db-sqlite), un tutorial de EF para la consola multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="37ef6-180">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="37ef6-181">Introducción a ASP.NET Core MVC en Mac o Linux</span><span class="sxs-lookup"><span data-stu-id="37ef6-181">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="37ef6-182">Introducción a ASP.NET Core MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37ef6-182">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="37ef6-183">Introducción a ASP.NET Core y Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37ef6-183">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
