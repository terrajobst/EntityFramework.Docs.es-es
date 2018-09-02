---
title: 'Introducción a .NET Framework: base de datos existente - EF Core'
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: edcdc0b76394c4d604cf43fc170424e474532b17
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993423"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="3fbc3-102">Introducción a EF Core en .NET Framework con una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="3fbc3-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="3fbc3-103">En este tutorial, compilará una aplicación de consola que realiza el acceso a datos básicos en una base de datos de Microsoft SQL Server mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="3fbc3-104">Para crear un modelo de Entity Framework, usará ingeniería inversa en una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="3fbc3-105">[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="3fbc3-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3fbc3-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3fbc3-106">Prerequisites</span></span>

* [<span data-ttu-id="3fbc3-107">Visual Studio 2017 versión 15.7 o posterior</span><span class="sxs-lookup"><span data-stu-id="3fbc3-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="3fbc3-108">Creación de una base de datos para blogs</span><span class="sxs-lookup"><span data-stu-id="3fbc3-108">Create Blogging database</span></span>

<span data-ttu-id="3fbc3-109">En este tutorial se usa una base de datos para **blogs** en la instancia de LocalDB como la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="3fbc3-110">Si ya creó la base de datos de **blogs** como parte de otro tutorial, omita estos pasos.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="3fbc3-111">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3fbc3-111">Open Visual Studio</span></span>

* <span data-ttu-id="3fbc3-112">**Herramientas -> Conectar con base de datos...**</span><span class="sxs-lookup"><span data-stu-id="3fbc3-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="3fbc3-113">Seleccione **Microsoft SQL Server** y haga clic en **Continuar**</span><span class="sxs-lookup"><span data-stu-id="3fbc3-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="3fbc3-114">Escriba **(localdb)\mssqllocaldb** como el **nombre del servidor**</span><span class="sxs-lookup"><span data-stu-id="3fbc3-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="3fbc3-115">Escriba **maestra** como el **nombre de la base de datos** y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="3fbc3-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="3fbc3-116">La base de datos maestra ahora aparece en **Conexiones de base de datos** del **Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="3fbc3-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="3fbc3-117">Haga clic con el botón derecho en la base de datos en el **Explorador de servidores** y seleccione **Nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="3fbc3-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="3fbc3-118">En el editor de consultas, copie el script que aparece a continuación.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="3fbc3-119">Haga clic con el botón derecho en el editor de consultas y seleccione **Ejecutar**</span><span class="sxs-lookup"><span data-stu-id="3fbc3-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="3fbc3-120">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="3fbc3-120">Create a new project</span></span>

* <span data-ttu-id="3fbc3-121">Abra Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3fbc3-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="3fbc3-122">**Archivo > Nuevo > Proyecto...**</span><span class="sxs-lookup"><span data-stu-id="3fbc3-122">**File > New > Project...**</span></span>

* <span data-ttu-id="3fbc3-123">En el menú izquierdo, seleccione **Instalado > Visual C# > Escritorio de Windows**.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="3fbc3-124">Seleccione la plantilla del proyecto **Aplicación de consola (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="3fbc3-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="3fbc3-125">Asegúrese de que el proyecto tenga **.NET Framework 4.6.1** o posterior como destino.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="3fbc3-126">Asigne el nombre *ConsoleApp.ExistingDb* al proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="3fbc3-127">Instalación de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3fbc3-127">Install Entity Framework</span></span>

<span data-ttu-id="3fbc3-128">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="3fbc3-129">Este tutorial usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="3fbc3-130">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="3fbc3-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="3fbc3-131">**Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="3fbc3-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="3fbc3-132">Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="3fbc3-133">En el paso siguiente, usará parte de Entity Framework Tools para aplicar ingeniería inversa en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="3fbc3-134">Por lo tanto, instale el paquete de herramientas.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-134">So install the tools package as well.</span></span>

* <span data-ttu-id="3fbc3-135">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="3fbc3-136">Aplicación de ingeniería inversa en el modelo</span><span class="sxs-lookup"><span data-stu-id="3fbc3-136">Reverse engineer the model</span></span>

<span data-ttu-id="3fbc3-137">Ahora hay que crear el modelo de EF en función de una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="3fbc3-138">**Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="3fbc3-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="3fbc3-139">Ejecute el comando siguiente para crear un modelo a partir de la base de datos existente</span><span class="sxs-lookup"><span data-stu-id="3fbc3-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="3fbc3-140">Para especificar las tablas para generar entidades, agregue el argumento `-Tables` al comando anterior.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="3fbc3-141">Por ejemplo: `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="3fbc3-142">El proceso de ingeniería inversa creó clases de entidad (`Blog` y `Post`) y un contexto derivado (`BloggingContext`) en función del esquema de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="3fbc3-143">Las clases de entidad son simples objetos de C# que representan los datos que va a consultar y guardar.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="3fbc3-144">Estas son las clases de entidad `Blog` y `Post`:</span><span class="sxs-lookup"><span data-stu-id="3fbc3-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="3fbc3-145">Para habilitar la carga diferida, puede usar las propiedades de navegación `virtual` (Blog.Post y Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="3fbc3-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="3fbc3-146">El contexto representa una sesión con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-146">The context represents a session with the database.</span></span> <span data-ttu-id="3fbc3-147">Tiene métodos que puede usar para consultar y guardar instancias de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="3fbc3-148">Uso del modelo</span><span class="sxs-lookup"><span data-stu-id="3fbc3-148">Use the model</span></span>

<span data-ttu-id="3fbc3-149">Ahora puede usar el modelo para realizar el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="3fbc3-150">Abra *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-150">Open *Program.cs*</span></span>

* <span data-ttu-id="3fbc3-151">Reemplace el contenido del archivo por el código siguiente</span><span class="sxs-lookup"><span data-stu-id="3fbc3-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="3fbc3-152">Depurar > Iniciar sin depuración</span><span class="sxs-lookup"><span data-stu-id="3fbc3-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="3fbc3-153">Verá que un blog se guarda en la base de datos y, luego, los detalles de todos los blogs se imprimen en la consola.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![imagen](_static/output-existing-db.png)

## <a name="additional-resources"></a><span data-ttu-id="3fbc3-155">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3fbc3-155">Additional Resources</span></span>

* [<span data-ttu-id="3fbc3-156">EF Core en .NET Framework con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="3fbc3-156">EF Core on .NET Framework with a new database</span></span>](xref:core/get-started/full-dotnet/new-db)
* <span data-ttu-id="3fbc3-157">[EF Core en .NET Core con una base de datos nueva (SQLite)](xref:core/get-started/netcore/new-db-sqlite), un tutorial de EF para la consola multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="3fbc3-157">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
