---
title: 'Introducción a ASP.NET Core: base de datos nueva - EF Core'
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: fbc1a00d6d6d0624bcbbfa1e51f4e21a915baaaa
ms.sourcegitcommit: f277883a5ed28eba57d14aaaf17405bc1ae9cf94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/18/2019
ms.locfileid: "65874576"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="32d2a-102">Introducción a EF Core en ASP.NET Core con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="32d2a-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="32d2a-103">En este tutorial compilará una aplicación de MVC de ASP.NET Core que realiza el acceso a datos básicos mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="32d2a-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="32d2a-104">El tutorial usa migraciones para crear la base de datos a partir del modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="32d2a-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="32d2a-105">Puede seguir el tutorial con Visual Studio 2017 en Windows o mediante la CLI de .NET Core en Windows, macOS o Linux.</span><span class="sxs-lookup"><span data-stu-id="32d2a-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="32d2a-106">Vea el ejemplo de este artículo en GitHub:</span><span class="sxs-lookup"><span data-stu-id="32d2a-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="32d2a-107">Visual Studio 2017 con SQL Server</span><span class="sxs-lookup"><span data-stu-id="32d2a-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="32d2a-108">[CLI de .NET Core con SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="32d2a-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="32d2a-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="32d2a-109">Prerequisites</span></span>

<span data-ttu-id="32d2a-110">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="32d2a-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32d2a-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32d2a-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="32d2a-112">[Visual Studio 2017 versión 15.7 o posterior](https://www.visualstudio.com/downloads/) con estas cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="32d2a-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="32d2a-113">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="32d2a-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="32d2a-114">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="32d2a-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="32d2a-115">[SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="32d2a-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32d2a-116">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="32d2a-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="32d2a-117">[SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="32d2a-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="32d2a-118">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="32d2a-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32d2a-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32d2a-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="32d2a-120">Abra Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="32d2a-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="32d2a-121">**Archivo > Nuevo > Proyecto**</span><span class="sxs-lookup"><span data-stu-id="32d2a-121">**File > New > Project**</span></span>
* <span data-ttu-id="32d2a-122">En el menú izquierdo, seleccione **Instalado > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="32d2a-123">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="32d2a-124">Escriba **EFGetStarted.AspNetCore.NewDb** como el nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="32d2a-125">En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="32d2a-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="32d2a-126">Asegúrese de que **.NET Core** y **ASP.NET Core 2.1** estén seleccionados en las listas desplegables</span><span class="sxs-lookup"><span data-stu-id="32d2a-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="32d2a-127">Seleccione la plantilla de proyecto **Aplicación web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="32d2a-128">Asegúrese de que **Autenticación** esté establecido en **Sin autenticación**</span><span class="sxs-lookup"><span data-stu-id="32d2a-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="32d2a-129">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-129">Click **OK**</span></span>

<span data-ttu-id="32d2a-130">Advertencia: Si usa **Cuentas de usuario individuales** en lugar de **Ninguno** para **Autenticación**, se agrega un modelo de Entity Framework Core al proyecto en `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="32d2a-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="32d2a-131">Con las técnicas que aprenderá en este tutorial podrá elegir agregar un segundo modelo o extender este modelo existente para contener las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="32d2a-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32d2a-132">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="32d2a-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="32d2a-133">Ejecute el siguiente comando para crear un proyecto de MVC:</span><span class="sxs-lookup"><span data-stu-id="32d2a-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="32d2a-134">Cambie al directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="32d2a-134">Change to the project directory.</span></span> <span data-ttu-id="32d2a-135">Los siguientes comandos que escriba deben ejecutarse en el nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="32d2a-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="32d2a-136">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="32d2a-136">Install Entity Framework Core</span></span>

<span data-ttu-id="32d2a-137">Para instalar EF Core, instale el paquete de los proveedores de bases de datos de EF Core que quiera establecer como destino.</span><span class="sxs-lookup"><span data-stu-id="32d2a-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="32d2a-138">Para obtener una lista de proveedores disponibles, vea [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="32d2a-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32d2a-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32d2a-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="32d2a-140">Para este tutorial no tendrá que instalar ningún paquete de proveedor porque se usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="32d2a-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="32d2a-141">El paquete de proveedor de SQL Server está incluido en el metapaquete [Microsoft.AspNetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="32d2a-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32d2a-142">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="32d2a-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="32d2a-143">Este tutorial usa SQLite porque se ejecuta en todas las plataformas compatibles con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="32d2a-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="32d2a-144">Ejecute el siguiente comando para instalar el proveedor de SQLite:</span><span class="sxs-lookup"><span data-stu-id="32d2a-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="32d2a-145">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="32d2a-145">Create the model</span></span>

<span data-ttu-id="32d2a-146">Defina una clase de contexto y clases de entidad que conformen el modelo.</span><span class="sxs-lookup"><span data-stu-id="32d2a-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32d2a-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32d2a-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="32d2a-148">Haga clic con el botón derecho en la carpeta **Modelos** y seleccione **Agregar > Clase**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="32d2a-149">Escriba **Model.cs** como el nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="32d2a-150">Reemplace el contenido del archivo por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="32d2a-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32d2a-151">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="32d2a-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="32d2a-152">En la carpeta **Modelos**, cree **Model.cs** con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="32d2a-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="32d2a-153">Una aplicación de producción normalmente colocaría cada clase en un archivo independiente.</span><span class="sxs-lookup"><span data-stu-id="32d2a-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="32d2a-154">Por simplicidad, este tutorial coloca estas clases en un archivo.</span><span class="sxs-lookup"><span data-stu-id="32d2a-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="32d2a-155">Registro del contexto con inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="32d2a-155">Register the context with dependency injection</span></span>

<span data-ttu-id="32d2a-156">Para que `BloggingContext` esté disponible para los controladores MVC, debe registrarlo como servicio en `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="32d2a-156">To make `BloggingContext` available to MVC controllers, register it as a service in `Startup.cs`.</span></span>

<span data-ttu-id="32d2a-157">Los servicios (como `BloggingContext`), se registran con la [inyección de dependencias](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante el inicio de la aplicación para que puedan proporcionarse automáticamente a los componentes que consumen servicios (como los controladores MVC) mediante las propiedades y los parámetros del constructor.</span><span class="sxs-lookup"><span data-stu-id="32d2a-157">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup so that they can be provided automatically to components that consume services (such as MVC controllers) via constructor parameters and properties.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32d2a-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32d2a-158">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="32d2a-159">En **Startup.cs**, agregue las siguientes instrucciones `using`:</span><span class="sxs-lookup"><span data-stu-id="32d2a-159">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="32d2a-160">Agregue el siguiente código resaltado al método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="32d2a-160">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32d2a-161">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="32d2a-161">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="32d2a-162">En **Startup.cs**, agregue las siguientes instrucciones `using`:</span><span class="sxs-lookup"><span data-stu-id="32d2a-162">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="32d2a-163">Agregue el siguiente código resaltado al método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="32d2a-163">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="32d2a-164">Una aplicación de producción normalmente colocaría la cadena de conexión en una variable de entorno o un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="32d2a-164">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="32d2a-165">Por simplicidad, en este tutorial se define en código.</span><span class="sxs-lookup"><span data-stu-id="32d2a-165">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="32d2a-166">Para más información, consulte [Cadenas de conexión](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="32d2a-166">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="32d2a-167">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="32d2a-167">Create the database</span></span>

<span data-ttu-id="32d2a-168">Los pasos siguientes usan [migraciones](xref:core/managing-schemas/migrations/index) para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="32d2a-168">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32d2a-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32d2a-169">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="32d2a-170">**Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="32d2a-170">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="32d2a-171">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="32d2a-171">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="32d2a-172">Si se produce un error que indica `The term 'add-migration' is not recognized as the name of a cmdlet`, cierre y vuelva a abrir Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32d2a-172">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="32d2a-173">El comando `Add-Migration` aplica la técnica scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="32d2a-173">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="32d2a-174">El comando `Update-Database` crea la base de datos y le aplica la nueva migración.</span><span class="sxs-lookup"><span data-stu-id="32d2a-174">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32d2a-175">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="32d2a-175">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="32d2a-176">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="32d2a-176">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="32d2a-177">El comando `migrations` aplica la técnica scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="32d2a-177">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="32d2a-178">El comando `database update` crea la base de datos y le aplica la nueva migración.</span><span class="sxs-lookup"><span data-stu-id="32d2a-178">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="32d2a-179">Creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="32d2a-179">Create a controller</span></span>

<span data-ttu-id="32d2a-180">Aplique scaffolding a un controlador y a las vistas de la entidad `Blog`.</span><span class="sxs-lookup"><span data-stu-id="32d2a-180">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32d2a-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32d2a-181">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="32d2a-182">Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-182">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="32d2a-183">Seleccione **Controlador de MVC con vistas que usan Entity Framework** y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-183">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="32d2a-184">Establezca la **clase de modelo** en **Blog** y **Clase de contexto de datos** en **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-184">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="32d2a-185">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-185">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32d2a-186">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="32d2a-186">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="32d2a-187">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="32d2a-187">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="32d2a-188">Los comandos `tool install` y `add package` instalan las herramientas que pueden aplicar la técnica scaffolding a controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="32d2a-188">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="32d2a-189">El comando `restore` garantiza que se descarguen todos los paquetes del proyecto, mientras que el comando `aspnet-codegenerator` aplica la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="32d2a-189">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="32d2a-190">El motor de scaffolding crea los siguientes archivos:</span><span class="sxs-lookup"><span data-stu-id="32d2a-190">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="32d2a-191">Un controlador (*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="32d2a-191">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="32d2a-192">Vistas de Razor para las páginas Create, Delete, Details, Edit e Index (_Views/Blogs/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="32d2a-192">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Blogs/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="32d2a-193">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="32d2a-193">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="32d2a-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="32d2a-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="32d2a-195">**Depurar** > **Iniciar sin depurar**</span><span class="sxs-lookup"><span data-stu-id="32d2a-195">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="32d2a-196">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="32d2a-196">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="32d2a-197">Navegue a `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="32d2a-197">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="32d2a-198">Use el vínculo **Crear nuevo** para crear algunas entradas de blog.</span><span class="sxs-lookup"><span data-stu-id="32d2a-198">Use the **Create New** link to create some blog entries.</span></span>

  ![Página Crear](_static/create.png)

* <span data-ttu-id="32d2a-200">Pruebe los vínculos **Detalles**, **Editar** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="32d2a-200">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Página de índice](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="32d2a-202">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="32d2a-202">Additional Resources</span></span>

* [<span data-ttu-id="32d2a-203">Tutorial: Introducción a EF Core en la aplicación de consola de .NET Core con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="32d2a-203">Tutorial: Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* <span data-ttu-id="32d2a-204">[Introducción a las páginas de Razor en ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start) o [Introducción a ASP.NET Core MVC](/aspnet/core/tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="32d2a-204">[Get started with Razor Pages in ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start) or [Get started with ASP.NET Core MVC ](/aspnet/core/tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="32d2a-205">[Tutorial: Páginas de Razor con Entity Framework Core en ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro) o [Introducción a EF Core en una aplicación web de ASP.NET Core MVC](/aspnet/core/data/ef-mvc/intro)</span><span class="sxs-lookup"><span data-stu-id="32d2a-205">[Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro) or [Get started with EF Core in an ASP.NET MVC web app](/aspnet/core/data/ef-mvc/intro)</span></span>
