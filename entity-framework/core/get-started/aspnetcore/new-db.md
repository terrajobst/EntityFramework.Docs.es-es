---
title: 'Introducción a ASP.NET Core: base de datos nueva - EF Core'
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996069"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="3b8ea-102">Introducción a EF Core en ASP.NET Core con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="3b8ea-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="3b8ea-103">En este tutorial compilará una aplicación de MVC de ASP.NET Core que realiza el acceso a datos básicos mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="3b8ea-104">Usará las migraciones para crear la base de datos a partir del modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="3b8ea-105">[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="3b8ea-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b8ea-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3b8ea-106">Prerequisites</span></span>

<span data-ttu-id="3b8ea-107">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="3b8ea-107">Install the following software:</span></span>

* <span data-ttu-id="3b8ea-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) con estas cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="3b8ea-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="3b8ea-109">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="3b8ea-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="3b8ea-110">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="3b8ea-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="3b8ea-111">[SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="3b8ea-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="3b8ea-112">Creación de un proyecto nuevo en Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3b8ea-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="3b8ea-113">Abra Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3b8ea-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="3b8ea-114">**Archivo > Nuevo > Proyecto**</span><span class="sxs-lookup"><span data-stu-id="3b8ea-114">**File > New > Project**</span></span>
* <span data-ttu-id="3b8ea-115">En el menú de la izquierda, seleccione **Instalado > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="3b8ea-116">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="3b8ea-117">Escriba **EFGetStarted.AspNetCore.NewDb** como el nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="3b8ea-118">En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3b8ea-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="3b8ea-119">Asegúrese de que las opciones **.NET Core** y **ASP.NET Core 2.1** estén seleccionadas en las listas desplegables.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="3b8ea-120">Seleccione la plantilla de proyecto **Aplicación web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="3b8ea-121">Asegúrese de que la **autenticación** esté establecida en **Sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="3b8ea-122">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-122">Click **OK**</span></span>

<span data-ttu-id="3b8ea-123">Advertencia: Si usa **Cuentas de usuario individuales** en lugar de **Ninguna** para **Autenticación**, se agregará un modelo de Entity Framework Core al proyecto en `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="3b8ea-124">Con las técnicas que aprenderá en este tutorial podrá elegir agregar un segundo modelo o extender este modelo existente para contener las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="3b8ea-125">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3b8ea-125">Install Entity Framework Core</span></span>

<span data-ttu-id="3b8ea-126">Para instalar EF Core, instale el paquete de los proveedores de bases de datos de EF Core que quiera establecer como destino.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="3b8ea-127">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="3b8ea-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="3b8ea-128">Para este tutorial no tendrá que instalar ningún paquete de proveedor porque se usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="3b8ea-129">El paquete de proveedor de SQL Server está incluido en el metapaquete [Microsoft.AspNetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="3b8ea-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="3b8ea-130">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="3b8ea-130">Create the model</span></span>

<span data-ttu-id="3b8ea-131">Defina una clase contexto y las clases de entidad que constituirán el modelo:</span><span class="sxs-lookup"><span data-stu-id="3b8ea-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="3b8ea-132">Haga clic con el botón derecho en la carpeta **Modelos** y seleccione **Agregar > Clase**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="3b8ea-133">Escriba **Model.cs** como el nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="3b8ea-134">Reemplace el contenido del archivo por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3b8ea-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="3b8ea-135">En una aplicación real, lo habitual sería colocar cada clase del modelo en un archivo independiente.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="3b8ea-136">Por simplicidad, en este tutorial todas las clases están en un solo archivo.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="3b8ea-137">Registro del contenido con inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="3b8ea-137">Register your context with dependency injection</span></span>

<span data-ttu-id="3b8ea-138">Los servicios (como `BloggingContext`) se registran con [inserción de dependencias](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="3b8ea-139">Luego se proporcionan estos servicios a los componentes que los necesitan (como los controladores MVC) a través de propiedades o parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="3b8ea-140">Para que `BloggingContext` esté disponible para los controladores MVC, debe registrarlo como servicio.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="3b8ea-141">Abra **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="3b8ea-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="3b8ea-142">Agregue las instrucciones `using` siguientes:</span><span class="sxs-lookup"><span data-stu-id="3b8ea-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="3b8ea-143">Llame al método `AddDbContext` para registrar el contexto como servicio.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="3b8ea-144">Agregue el siguiente código resaltado al método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3b8ea-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="3b8ea-145">Nota: En una aplicación real, lo habitual sería colocar la cadena de conexión en un archivo de configuración o una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="3b8ea-146">Por simplicidad, en este tutorial se define en código.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="3b8ea-147">Para más información, consulte [Cadenas de conexión](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="3b8ea-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="3b8ea-148">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="3b8ea-148">Create the database</span></span>

<span data-ttu-id="3b8ea-149">Una vez que ya tiene un modelo, puede usar las [migraciones](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="3b8ea-150">**Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="3b8ea-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="3b8ea-151">Ejecute `Add-Migration InitialCreate` para aplicar scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="3b8ea-152">Si recibe un error que indica `The term 'add-migration' is not recognized as the name of a cmdlet`, cierre Visual Studio y vuelva a abrirlo.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="3b8ea-153">Ejecute `Update-Database` para aplicar la migración nueva a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="3b8ea-154">Este comando crea la base de datos antes de aplicar las migraciones.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="3b8ea-155">Creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="3b8ea-155">Create a controller</span></span>

<span data-ttu-id="3b8ea-156">Aplique scaffolding a un controlador y a las vistas de la entidad `Blog`.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="3b8ea-157">Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="3b8ea-158">Seleccione **Controlador de MVC con vistas que usan Entity Framework** y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="3b8ea-159">Establezca la **clase de modelo** en **Blog** y **Clase de contexto de datos** en **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="3b8ea-160">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="3b8ea-161">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="3b8ea-161">Run the application</span></span>

<span data-ttu-id="3b8ea-162">Presione F5 para ejecutar y probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="3b8ea-163">Navegue a `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="3b8ea-164">Use el vínculo de creación para crear algunas entradas de blog.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="3b8ea-165">Pruebe los detalles y elimine los vínculos.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-165">Test the details and delete links.</span></span>

![imagen](_static/create.png)

![imagen](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="3b8ea-168">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3b8ea-168">Additional Resources</span></span>

* <span data-ttu-id="3b8ea-169">[EF: base de datos nueva con SQLite](xref:core/get-started/netcore/new-db-sqlite), un tutorial de EF para la consola multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="3b8ea-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="3b8ea-170">Introducción a ASP.NET Core MVC en Mac o Linux</span><span class="sxs-lookup"><span data-stu-id="3b8ea-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="3b8ea-171">Introducción a ASP.NET Core MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3b8ea-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="3b8ea-172">Introducción a ASP.NET Core y Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3b8ea-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
