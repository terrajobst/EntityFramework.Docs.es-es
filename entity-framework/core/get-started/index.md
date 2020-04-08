---
title: Introducción - EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 0e7a1ee159cdf5b72448fe6d73c972975b1ab95b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412870"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="9cb67-102">Introducción a EF Core</span><span class="sxs-lookup"><span data-stu-id="9cb67-102">Getting Started with EF Core</span></span>

<span data-ttu-id="9cb67-103">En este tutorial se crea una aplicación de consola de .NET Core que realiza el acceso a datos en una base de datos SQLite mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9cb67-103">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="9cb67-104">Puede seguir el tutorial con Visual Studio en Windows o mediante la CLI de .NET Core en Windows, macOS o Linux.</span><span class="sxs-lookup"><span data-stu-id="9cb67-104">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="9cb67-105">[Vea un ejemplo de este artículo en GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="9cb67-105">[View this article's sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9cb67-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="9cb67-106">Prerequisites</span></span>

<span data-ttu-id="9cb67-107">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="9cb67-107">Install the following software:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9cb67-108">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="9cb67-108">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="9cb67-109">[SDK de .NET Core](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="9cb67-109">[.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="9cb67-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9cb67-110">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9cb67-111">[Visual Studio 2019, versión 16.3 o posterior](https://www.visualstudio.com/downloads/), con esta carga de trabajo:</span><span class="sxs-lookup"><span data-stu-id="9cb67-111">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="9cb67-112">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="9cb67-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="9cb67-113">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="9cb67-113">Create a new project</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9cb67-114">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="9cb67-114">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[<span data-ttu-id="9cb67-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9cb67-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9cb67-116">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9cb67-116">Open Visual Studio</span></span>
* <span data-ttu-id="9cb67-117">Haga clic en **Crear un proyecto nuevo**</span><span class="sxs-lookup"><span data-stu-id="9cb67-117">Click **Create a new project**</span></span>
* <span data-ttu-id="9cb67-118">Seleccione **Aplicación de consola (.NET Core)** con la etiqueta **C#** y haga clic en **Siguiente**</span><span class="sxs-lookup"><span data-stu-id="9cb67-118">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="9cb67-119">Escriba **EFGetStarted** para el nombre y haga clic en **Crear**</span><span class="sxs-lookup"><span data-stu-id="9cb67-119">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="9cb67-120">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9cb67-120">Install Entity Framework Core</span></span>

<span data-ttu-id="9cb67-121">Para instalar EF Core, instale el paquete de los proveedores de bases de datos de EF Core que quiera establecer como destino.</span><span class="sxs-lookup"><span data-stu-id="9cb67-121">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="9cb67-122">Este tutorial usa SQLite porque se ejecuta en todas las plataformas compatibles con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9cb67-122">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="9cb67-123">Para obtener una lista de proveedores disponibles, vea [Proveedores de bases de datos](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9cb67-123">For a list of available providers, see [Database Providers](../providers/index.md).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9cb67-124">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="9cb67-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="9cb67-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9cb67-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9cb67-126">**Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="9cb67-126">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="9cb67-127">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="9cb67-127">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="9cb67-128">Sugerencia: También puede instalar paquetes si hace clic con el botón derecho en el proyecto y selecciona **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9cb67-128">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="9cb67-129">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="9cb67-129">Create the model</span></span>

<span data-ttu-id="9cb67-130">Defina una clase de contexto y clases de entidad que conformen el modelo.</span><span class="sxs-lookup"><span data-stu-id="9cb67-130">Define a context class and entity classes that make up the model.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9cb67-131">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="9cb67-131">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="9cb67-132">En el directorio del proyecto, cree **Model.cs** con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="9cb67-132">In the project directory, create **Model.cs** with the following code</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="9cb67-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9cb67-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9cb67-134">Haga clic con el botón derecho en el proyecto y seleccione **Agregar > Clase**</span><span class="sxs-lookup"><span data-stu-id="9cb67-134">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="9cb67-135">Escriba **Model.cs** como el nombre y haga clic en **Agregar**</span><span class="sxs-lookup"><span data-stu-id="9cb67-135">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="9cb67-136">Reemplace el contenido del archivo por el código siguiente</span><span class="sxs-lookup"><span data-stu-id="9cb67-136">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="9cb67-137">EF Core también puede [aplicar ingeniería inversa](../managing-schemas/scaffolding.md) en un modelo desde una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="9cb67-137">EF Core can also [reverse engineer](../managing-schemas/scaffolding.md) a model from an existing database.</span></span>

<span data-ttu-id="9cb67-138">Sugerencia: En una aplicación real, lo habitual sería colocar cada clase en un archivo independiente y la [cadena de conexión](../miscellaneous/connection-strings.md), en un archivo de configuración o una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="9cb67-138">Tip: In a real app, you put each class in a separate file and put the [connection string](../miscellaneous/connection-strings.md) in a configuration file or environment variable.</span></span> <span data-ttu-id="9cb67-139">Para que el tutorial sea sencillo, todo está incluido en un archivo.</span><span class="sxs-lookup"><span data-stu-id="9cb67-139">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="9cb67-140">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="9cb67-140">Create the database</span></span>

<span data-ttu-id="9cb67-141">Los pasos siguientes usan [migraciones](xref:core/managing-schemas/migrations/index) para crear una base de datos.</span><span class="sxs-lookup"><span data-stu-id="9cb67-141">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9cb67-142">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="9cb67-142">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="9cb67-143">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="9cb67-143">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="9cb67-144">Esto instala [dotnet ef](../miscellaneous/cli/dotnet.md) y el paquete de diseño necesario para ejecutar el comando en un proyecto.</span><span class="sxs-lookup"><span data-stu-id="9cb67-144">This installs [dotnet ef](../miscellaneous/cli/dotnet.md) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="9cb67-145">El comando `migrations` aplica la técnica scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="9cb67-145">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="9cb67-146">El comando `database update` crea la base de datos y le aplica la nueva migración.</span><span class="sxs-lookup"><span data-stu-id="9cb67-146">The `database update` command creates the database and applies the new migration to it.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="9cb67-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9cb67-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9cb67-148">Ejecute los comandos siguientes en la **Consola del administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="9cb67-148">Run the following commands in **Package Manager Console**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="9cb67-149">Esto instala las [herramientas de PMC para EF Core](../miscellaneous/cli/powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9cb67-149">This installs the [PMC tools for EF Core](../miscellaneous/cli/powershell.md).</span></span> <span data-ttu-id="9cb67-150">El comando `Add-Migration` aplica la técnica scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="9cb67-150">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="9cb67-151">El comando `Update-Database` crea la base de datos y le aplica la nueva migración.</span><span class="sxs-lookup"><span data-stu-id="9cb67-151">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="9cb67-152">Creación, lectura, actualización y eliminación</span><span class="sxs-lookup"><span data-stu-id="9cb67-152">Create, read, update & delete</span></span>

* <span data-ttu-id="9cb67-153">Abra *Program.cs* y reemplace el contenido por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="9cb67-153">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="9cb67-154">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="9cb67-154">Run the app</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="9cb67-155">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="9cb67-155">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[<span data-ttu-id="9cb67-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9cb67-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9cb67-157">Visual Studio usa un directorio de trabajo incoherente al ejecutar las aplicaciones de consola de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9cb67-157">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="9cb67-158">(consulte [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)). Esto hace que se genere una excepción: *no such table: Blogs* (No existe dicha tabla: blogs).</span><span class="sxs-lookup"><span data-stu-id="9cb67-158">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="9cb67-159">Para actualizar el directorio de trabajo:</span><span class="sxs-lookup"><span data-stu-id="9cb67-159">To update the working directory:</span></span>

* <span data-ttu-id="9cb67-160">Haga clic en el proyecto y seleccione **Editar archivo del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="9cb67-160">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="9cb67-161">Justo debajo de la propiedad *TargetFramework*, agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9cb67-161">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="9cb67-162">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="9cb67-162">Save the file</span></span>

<span data-ttu-id="9cb67-163">Ahora puede ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="9cb67-163">Now you can run the app:</span></span>

* <span data-ttu-id="9cb67-164">**Depurar > Iniciar sin depuración**</span><span class="sxs-lookup"><span data-stu-id="9cb67-164">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="9cb67-165">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="9cb67-165">Next steps</span></span>

* <span data-ttu-id="9cb67-166">Siga el [tutorial de ASP.NET Core](/aspnet/core/data/ef-rp/intro) para usar EF Core en una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="9cb67-166">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="9cb67-167">Obtenga más información sobre las [expresiones de consulta LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).</span><span class="sxs-lookup"><span data-stu-id="9cb67-167">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="9cb67-168">[Configure su modelo](xref:core/modeling/index) para especificar aspectos como [requerido](xref:core/modeling/entity-properties#required-and-optional-properties) y [longitud máxima](xref:core/modeling/entity-properties#maximum-length).</span><span class="sxs-lookup"><span data-stu-id="9cb67-168">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/entity-properties#required-and-optional-properties) and [maximum length](xref:core/modeling/entity-properties#maximum-length)</span></span>
* <span data-ttu-id="9cb67-169">Use [Migraciones](xref:core/managing-schemas/migrations/index) para actualizar el esquema de la base de datos después de cambiar el modelo.</span><span class="sxs-lookup"><span data-stu-id="9cb67-169">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
