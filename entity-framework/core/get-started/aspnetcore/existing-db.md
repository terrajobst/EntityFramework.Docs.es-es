---
title: 'Introducción a ASP.NET Core: base de datos existente - EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949158"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="c04fa-102">Introducción a EF Core en ASP.NET Core con una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="c04fa-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="c04fa-103">En este tutorial, compilará una aplicación de MVC de ASP.NET Core que realiza el acceso a datos básicos mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c04fa-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="c04fa-104">Usará técnicas de ingeniería inversa para crear un modelo de Entity Framework en función de una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="c04fa-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="c04fa-105">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="c04fa-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c04fa-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c04fa-106">Prerequisites</span></span>

<span data-ttu-id="c04fa-107">Deberá cumplir los requisitos previos siguientes para completar este tutorial:</span><span class="sxs-lookup"><span data-stu-id="c04fa-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="c04fa-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) con estas cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="c04fa-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="c04fa-109">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="c04fa-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="c04fa-110">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="c04fa-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="c04fa-111">[SDK de .NET Core 2.0](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="c04fa-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="c04fa-112">Base de datos para blogs</span><span class="sxs-lookup"><span data-stu-id="c04fa-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="c04fa-113">Base de datos para blogs</span><span class="sxs-lookup"><span data-stu-id="c04fa-113">Blogging database</span></span>

<span data-ttu-id="c04fa-114">En este tutorial se usa una base de datos para **blogs** en la instancia de LocalDB como la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="c04fa-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="c04fa-115">Si ya creó la base de datos de **blogs** como parte de otro tutorial, puede omitir estos pasos.</span><span class="sxs-lookup"><span data-stu-id="c04fa-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="c04fa-116">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c04fa-116">Open Visual Studio</span></span>
* <span data-ttu-id="c04fa-117">**Herramientas -> Conectar con base de datos...**</span><span class="sxs-lookup"><span data-stu-id="c04fa-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="c04fa-118">Seleccione **Microsoft SQL Server** y haga clic en **Continuar**</span><span class="sxs-lookup"><span data-stu-id="c04fa-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="c04fa-119">Escriba **(localdb)\mssqllocaldb** como el **nombre del servidor**</span><span class="sxs-lookup"><span data-stu-id="c04fa-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="c04fa-120">Escriba **maestra** como el **nombre de la base de datos** y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="c04fa-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="c04fa-121">La base de datos maestra ahora aparece en **Conexiones de base de datos** del **Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="c04fa-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="c04fa-122">Haga clic con el botón derecho en la base de datos en el **Explorador de servidores** y seleccione **Nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="c04fa-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="c04fa-123">En el editor de consultas, copie el script que aparece a continuación.</span><span class="sxs-lookup"><span data-stu-id="c04fa-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="c04fa-124">Haga clic con el botón derecho en el editor de consultas y seleccione **Ejecutar**</span><span class="sxs-lookup"><span data-stu-id="c04fa-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="c04fa-125">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="c04fa-125">Create a new project</span></span>

* <span data-ttu-id="c04fa-126">Abra Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c04fa-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="c04fa-127">**Archivo -> Nuevo -> Proyecto...**</span><span class="sxs-lookup"><span data-stu-id="c04fa-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="c04fa-128">En el menú de la izquierda, seleccione **Instalado -> Plantillas -> Visual C# -> Web**</span><span class="sxs-lookup"><span data-stu-id="c04fa-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="c04fa-129">Seleccione la plantilla de proyecto **Aplicación web de ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="c04fa-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="c04fa-130">Escriba **EFGetStarted.AspNetCore.ExistingDb** como el nombre y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="c04fa-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="c04fa-131">Espere que aparezca el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="c04fa-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="c04fa-132">En **ASP.NET Core Templates 2.0** (Plantillas 2.0 de ASP.NET Core), seleccione la **Aplicación web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="c04fa-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="c04fa-133">Asegúrese de que la **autenticación** esté establecida en **Sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="c04fa-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="c04fa-134">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c04fa-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="c04fa-135">Instalación de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c04fa-135">Install Entity Framework</span></span>

<span data-ttu-id="c04fa-136">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="c04fa-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="c04fa-137">Este tutorial usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c04fa-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="c04fa-138">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c04fa-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="c04fa-139">**Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="c04fa-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="c04fa-140">Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="c04fa-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="c04fa-141">Usaremos parte de Entity Framework Tools para crear un modelo desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c04fa-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="c04fa-142">Por lo tanto, también instalaremos el paquete de herramientas:</span><span class="sxs-lookup"><span data-stu-id="c04fa-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="c04fa-143">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="c04fa-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="c04fa-144">Usaremos las herramientas de scaffolding de ASP.NET Core para crear más adelante controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="c04fa-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="c04fa-145">Por lo tanto, también instalaremos el paquete de diseño:</span><span class="sxs-lookup"><span data-stu-id="c04fa-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="c04fa-146">Ejecute `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`.</span><span class="sxs-lookup"><span data-stu-id="c04fa-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="c04fa-147">Utilización de técnicas de ingeniería inversa para el modelo</span><span class="sxs-lookup"><span data-stu-id="c04fa-147">Reverse engineer your model</span></span>

<span data-ttu-id="c04fa-148">Ya es momento de crear el modelo de EF en función de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="c04fa-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="c04fa-149">**Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="c04fa-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="c04fa-150">Ejecute el comando siguiente para crear un modelo a partir de la base de datos existente:</span><span class="sxs-lookup"><span data-stu-id="c04fa-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="c04fa-151">Si recibe un error que indica `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, cierre Visual Studio y vuelva a abrirlo.</span><span class="sxs-lookup"><span data-stu-id="c04fa-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="c04fa-152">Para especificar las tablas para las cuales desea generar entidades, agregue el argumento `-Tables` al comando anterior.</span><span class="sxs-lookup"><span data-stu-id="c04fa-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="c04fa-153">Por ejemplo: `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="c04fa-153">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="c04fa-154">El proceso de ingeniería inversa creó clases de entidad (`Blog.cs` & `Post.cs`) y un contexto derivado (`BloggingContext.cs`) en función del esquema de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="c04fa-154">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="c04fa-155">Las clases de entidad son simples objetos de C# que representan los datos que va a consultar y guardar.</span><span class="sxs-lookup"><span data-stu-id="c04fa-155">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="c04fa-156">El contexto representa una sesión con la base de datos y le permite consultar y guardar instancias de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="c04fa-156">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="c04fa-157">Registro del contenido con inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="c04fa-157">Register your context with dependency injection</span></span>

<span data-ttu-id="c04fa-158">El concepto de la inserción de dependencias es fundamental para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c04fa-158">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="c04fa-159">Los servicios (como `BloggingContext`) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c04fa-159">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c04fa-160">Luego se proporcionan estos servicios a los componentes que los necesitan (como los controladores MVC) a través de propiedades o parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="c04fa-160">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="c04fa-161">Para más información sobre la inserción de dependencias, consulte el artículo sobre la [inserción de dependencias](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) en el sitio de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c04fa-161">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="c04fa-162">Eliminación de la configuración en contexto alineada</span><span class="sxs-lookup"><span data-stu-id="c04fa-162">Remove inline context configuration</span></span>

<span data-ttu-id="c04fa-163">En ASP.NET Core, la configuración se realiza generalmente en **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="c04fa-163">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="c04fa-164">Para cumplir con este patrón, migraremos la configuración del proveedor de bases de datos a **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="c04fa-164">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="c04fa-165">Abrir `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="c04fa-165">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="c04fa-166">Eliminación del método `OnConfiguring(...)`</span><span class="sxs-lookup"><span data-stu-id="c04fa-166">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="c04fa-167">Agregue el constructor siguiente, lo que permitirá que la inserción de dependencias pase la configuración al contexto</span><span class="sxs-lookup"><span data-stu-id="c04fa-167">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="c04fa-168">Registro y configuración del contexto en Startup.cs</span><span class="sxs-lookup"><span data-stu-id="c04fa-168">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="c04fa-169">Con el fin de que los controladores MVC usen `BloggingContext`, lo registraremos como servicio.</span><span class="sxs-lookup"><span data-stu-id="c04fa-169">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="c04fa-170">Abra **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="c04fa-170">Open **Startup.cs**</span></span>
* <span data-ttu-id="c04fa-171">Agregue las instrucciones `using` siguientes en el comienzo del archivo</span><span class="sxs-lookup"><span data-stu-id="c04fa-171">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="c04fa-172">Ahora podemos usar el método `AddDbContext(...)` para registrarlo como servicio.</span><span class="sxs-lookup"><span data-stu-id="c04fa-172">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="c04fa-173">Ubique el método `ConfigureServices(...)`</span><span class="sxs-lookup"><span data-stu-id="c04fa-173">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="c04fa-174">Agregue el código siguiente para registrar el contexto como servicio</span><span class="sxs-lookup"><span data-stu-id="c04fa-174">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="c04fa-175">En una aplicación real, habitualmente colocaría la cadena de conexión en un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="c04fa-175">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="c04fa-176">Por simplicidad, se define en el código.</span><span class="sxs-lookup"><span data-stu-id="c04fa-176">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="c04fa-177">Para más información, consulte [Cadenas de conexión](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="c04fa-177">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="c04fa-178">Creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="c04fa-178">Create a controller</span></span>

<span data-ttu-id="c04fa-179">A continuación, habilitaremos scaffolding en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c04fa-179">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="c04fa-180">Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar -> Controlador...**</span><span class="sxs-lookup"><span data-stu-id="c04fa-180">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="c04fa-181">Seleccione **Dependencias completas** y haga clic en **Agregar**</span><span class="sxs-lookup"><span data-stu-id="c04fa-181">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="c04fa-182">Puede omitir las instrucciones que aparecen en el archivo `ScaffoldingReadMe.txt` que se abre</span><span class="sxs-lookup"><span data-stu-id="c04fa-182">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="c04fa-183">Ahora que se habilitó scaffolding, podemos aplicarlo a un controlador de la entidad `Blog`.</span><span class="sxs-lookup"><span data-stu-id="c04fa-183">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="c04fa-184">Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar -> Controlador...**</span><span class="sxs-lookup"><span data-stu-id="c04fa-184">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="c04fa-185">Seleccione **Controlador de MVC con vistas que usan Entity Framework** y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="c04fa-185">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="c04fa-186">Establezca la **clase de modelo** en **Blog** y **Clase de contexto de datos** en **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="c04fa-186">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="c04fa-187">Haga clic en **Agregar**</span><span class="sxs-lookup"><span data-stu-id="c04fa-187">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="c04fa-188">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c04fa-188">Run the application</span></span>

<span data-ttu-id="c04fa-189">Ahora puede ejecutar la aplicación para verla en acción.</span><span class="sxs-lookup"><span data-stu-id="c04fa-189">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="c04fa-190">**Depurar -> Iniciar sin depuración**</span><span class="sxs-lookup"><span data-stu-id="c04fa-190">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="c04fa-191">La aplicación se compilará y abrirá en un explorador web</span><span class="sxs-lookup"><span data-stu-id="c04fa-191">The application will build and open in a web browser</span></span>
* <span data-ttu-id="c04fa-192">Navegue a `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="c04fa-192">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="c04fa-193">Haga clic en **Crear nuevo**</span><span class="sxs-lookup"><span data-stu-id="c04fa-193">Click **Create New**</span></span>
* <span data-ttu-id="c04fa-194">Escriba una **dirección URL** para el blog nuevo y haga clic en **Crear**</span><span class="sxs-lookup"><span data-stu-id="c04fa-194">Enter a **Url** for the new blog and click **Create**</span></span>

![imagen](_static/create.png)

![imagen](_static/index-existing-db.png)
