---
title: 'Introducción a ASP.NET Core: base de datos nueva - EF Core'
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 878478099878e4a0bc65c44fef0609d28f39f2b8
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834778"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="f2750-102">Introducción a EF Core en ASP.NET Core con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="f2750-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="f2750-103">En este tutorial compilará una aplicación de MVC de ASP.NET Core que realiza el acceso a datos básicos mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="f2750-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="f2750-104">El tutorial usa migraciones para crear la base de datos a partir del modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="f2750-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="f2750-105">Puede seguir el tutorial con Visual Studio 2017 en Windows o mediante la CLI de .NET Core en Windows, macOS o Linux.</span><span class="sxs-lookup"><span data-stu-id="f2750-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="f2750-106">Vea el ejemplo de este artículo en GitHub:</span><span class="sxs-lookup"><span data-stu-id="f2750-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="f2750-107">Visual Studio 2017 con SQL Server</span><span class="sxs-lookup"><span data-stu-id="f2750-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="f2750-108">[CLI de .NET Core con SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="f2750-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f2750-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f2750-109">Prerequisites</span></span>

<span data-ttu-id="f2750-110">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="f2750-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2750-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2750-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2750-112">[Visual Studio 2017 versión 15.7 o posterior](https://www.visualstudio.com/downloads/) con estas cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="f2750-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="f2750-113">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="f2750-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="f2750-114">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="f2750-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="f2750-115">[SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="f2750-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2750-116">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f2750-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="f2750-117">[SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="f2750-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="f2750-118">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="f2750-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2750-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2750-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2750-120">Abra Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f2750-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="f2750-121">**Archivo > Nuevo > Proyecto**</span><span class="sxs-lookup"><span data-stu-id="f2750-121">**File > New > Project**</span></span>
* <span data-ttu-id="f2750-122">En el menú izquierdo, seleccione **Instalado > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f2750-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="f2750-123">Seleccione **Aplicación web de ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f2750-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="f2750-124">Escriba **EFGetStarted.AspNetCore.NewDb** como el nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f2750-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="f2750-125">En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f2750-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="f2750-126">Asegúrese de que **.NET Core** y **ASP.NET Core 2.1** estén seleccionados en las listas desplegables</span><span class="sxs-lookup"><span data-stu-id="f2750-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="f2750-127">Seleccione la plantilla de proyecto **Aplicación web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="f2750-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="f2750-128">Asegúrese de que **Autenticación** esté establecido en **Sin autenticación**</span><span class="sxs-lookup"><span data-stu-id="f2750-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="f2750-129">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f2750-129">Click **OK**</span></span>

<span data-ttu-id="f2750-130">Advertencia: Si usa **Cuentas de usuario individuales** en lugar de **Ninguno** para **Autenticación**, se agrega un modelo de Entity Framework Core al proyecto en `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="f2750-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="f2750-131">Con las técnicas que aprenderá en este tutorial podrá elegir agregar un segundo modelo o extender este modelo existente para contener las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="f2750-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2750-132">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f2750-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="f2750-133">Ejecute el siguiente comando para crear un proyecto de MVC:</span><span class="sxs-lookup"><span data-stu-id="f2750-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="f2750-134">Cambie al directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f2750-134">Change to the project directory.</span></span> <span data-ttu-id="f2750-135">Los siguientes comandos que escriba deben ejecutarse en el nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="f2750-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="f2750-136">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f2750-136">Install Entity Framework Core</span></span>

<span data-ttu-id="f2750-137">Para instalar EF Core, instale el paquete de los proveedores de bases de datos de EF Core que quiera establecer como destino.</span><span class="sxs-lookup"><span data-stu-id="f2750-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="f2750-138">Para obtener una lista de proveedores disponibles, vea [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="f2750-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2750-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2750-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f2750-140">Para este tutorial no tendrá que instalar ningún paquete de proveedor porque se usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f2750-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="f2750-141">El paquete de proveedor de SQL Server está incluido en el metapaquete [Microsoft.AspNetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="f2750-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2750-142">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f2750-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f2750-143">Este tutorial usa SQLite porque se ejecuta en todas las plataformas compatibles con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2750-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="f2750-144">Ejecute el siguiente comando para instalar el proveedor de SQLite:</span><span class="sxs-lookup"><span data-stu-id="f2750-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="f2750-145">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="f2750-145">Create the model</span></span>

<span data-ttu-id="f2750-146">Defina una clase de contexto y clases de entidad que conformen el modelo.</span><span class="sxs-lookup"><span data-stu-id="f2750-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2750-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2750-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2750-148">Haga clic con el botón derecho en la carpeta **Modelos** y seleccione **Agregar > Clase**.</span><span class="sxs-lookup"><span data-stu-id="f2750-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="f2750-149">Escriba **Model.cs** como el nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f2750-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="f2750-150">Reemplace el contenido del archivo por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f2750-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2750-151">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f2750-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="f2750-152">En la carpeta **Modelos**, cree **Model.cs** con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f2750-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="f2750-153">Una aplicación de producción normalmente colocaría cada clase en un archivo independiente.</span><span class="sxs-lookup"><span data-stu-id="f2750-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="f2750-154">Por simplicidad, este tutorial coloca estas clases en un archivo.</span><span class="sxs-lookup"><span data-stu-id="f2750-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="f2750-155">Registro del contexto con inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="f2750-155">Register the context with dependency injection</span></span>

<span data-ttu-id="f2750-156">Los servicios (como `BloggingContext`) se registran con [inserción de dependencias](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f2750-156">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="f2750-157">A los componentes que requieren estos servicios (por ejemplo, los controladores MVC), se les proporcionan a través de parámetros o propiedades de constructor.</span><span class="sxs-lookup"><span data-stu-id="f2750-157">Components that require these services (such as MVC controllers) are provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="f2750-158">Para que `BloggingContext` esté disponible para los controladores MVC, debe registrarlo como servicio.</span><span class="sxs-lookup"><span data-stu-id="f2750-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2750-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2750-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2750-160">En **Startup.cs**, agregue las siguientes instrucciones `using`:</span><span class="sxs-lookup"><span data-stu-id="f2750-160">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="f2750-161">Agregue el siguiente código resaltado al método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f2750-161">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2750-162">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f2750-162">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="f2750-163">En **Startup.cs**, agregue las siguientes instrucciones `using`:</span><span class="sxs-lookup"><span data-stu-id="f2750-163">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="f2750-164">Agregue el siguiente código resaltado al método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f2750-164">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="f2750-165">Una aplicación de producción normalmente colocaría la cadena de conexión en una variable de entorno o un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="f2750-165">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="f2750-166">Por simplicidad, en este tutorial se define en código.</span><span class="sxs-lookup"><span data-stu-id="f2750-166">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="f2750-167">Para más información, consulte [Cadenas de conexión](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="f2750-167">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="f2750-168">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="f2750-168">Create the database</span></span>

<span data-ttu-id="f2750-169">Los pasos siguientes usan [migraciones](xref:core/managing-schemas/migrations/index) para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="f2750-169">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2750-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2750-170">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2750-171">**Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="f2750-171">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="f2750-172">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f2750-172">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="f2750-173">Si se produce un error que indica `The term 'add-migration' is not recognized as the name of a cmdlet`, cierre y vuelva a abrir Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f2750-173">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="f2750-174">El comando `Add-Migration` aplica la técnica scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="f2750-174">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="f2750-175">El comando `Update-Database` crea la base de datos y le aplica la nueva migración.</span><span class="sxs-lookup"><span data-stu-id="f2750-175">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2750-176">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f2750-176">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="f2750-177">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f2750-177">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="f2750-178">El comando `migrations` aplica la técnica scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="f2750-178">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="f2750-179">El comando `database update` crea la base de datos y le aplica la nueva migración.</span><span class="sxs-lookup"><span data-stu-id="f2750-179">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="f2750-180">Creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="f2750-180">Create a controller</span></span>

<span data-ttu-id="f2750-181">Aplique scaffolding a un controlador y a las vistas de la entidad `Blog`.</span><span class="sxs-lookup"><span data-stu-id="f2750-181">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2750-182">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2750-182">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2750-183">Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar > Controlador**.</span><span class="sxs-lookup"><span data-stu-id="f2750-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="f2750-184">Seleccione **Controlador de MVC con vistas que usan Entity Framework** y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f2750-184">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="f2750-185">Establezca la **clase de modelo** en **Blog** y **Clase de contexto de datos** en **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="f2750-185">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="f2750-186">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f2750-186">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2750-187">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f2750-187">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="f2750-188">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f2750-188">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="f2750-189">Los comandos `tool install` y `add package` instalan las herramientas que pueden aplicar la técnica scaffolding a controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="f2750-189">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="f2750-190">El comando `restore` garantiza que se descarguen todos los paquetes del proyecto, mientras que el comando `aspnet-codegenerator` aplica la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f2750-190">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="f2750-191">El motor de scaffolding crea los siguientes archivos:</span><span class="sxs-lookup"><span data-stu-id="f2750-191">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="f2750-192">Un controlador (*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="f2750-192">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="f2750-193">Vistas de Razor para las páginas Create, Delete, Details, Edit e Index (_Views/Movies/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="f2750-193">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Movies/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="f2750-194">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="f2750-194">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2750-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2750-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f2750-196">**Depurar** > **Iniciar sin depurar**</span><span class="sxs-lookup"><span data-stu-id="f2750-196">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2750-197">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f2750-197">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="f2750-198">Navegue a `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="f2750-198">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="f2750-199">Use el vínculo **Crear nuevo** para crear algunas entradas de blog.</span><span class="sxs-lookup"><span data-stu-id="f2750-199">Use the **Create New** link to create some blog entries.</span></span>

  ![Página Crear](_static/create.png)

* <span data-ttu-id="f2750-201">Pruebe los vínculos **Detalles**, **Editar** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="f2750-201">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Página de índice](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="f2750-203">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f2750-203">Additional Resources</span></span>

* [<span data-ttu-id="f2750-204">Tutorial: Introducción a EF Core en la aplicación de consola de .NET Core con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="f2750-204">Tutorial: Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* [<span data-ttu-id="f2750-205">Tutorial: Introducción a Razor Pages en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f2750-205">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="f2750-206">Tutorial: Razor Pages con Entity Framework Core en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f2750-206">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
