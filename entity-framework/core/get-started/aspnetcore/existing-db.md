---
title: "Introducción a ASP.NET Core: base de datos existente - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: afd99d68d2ba25ce58a21dc48d2c7ce27f208807
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="2b0fa-102">Introducción a EF Core en ASP.NET Core con una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="2b0fa-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="2b0fa-103">El [SDK de .NET Core](https://www.microsoft.com/net/download/core) ya no es compatible con `project.json` ni con Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-103">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="2b0fa-104">Se recomienda que todos quienes desarrollan en .NET Core [migren de project.json a csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) y [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="2b0fa-104">Everyone doing .NET Core development is encouraged to [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) and [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

<span data-ttu-id="2b0fa-105">En este tutorial, compilará una aplicación de MVC de ASP.NET Core que realiza el acceso a datos básicos mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-105">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="2b0fa-106">Usará técnicas de ingeniería inversa para crear un modelo de Entity Framework en función de una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-106">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="2b0fa-107">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-107">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2b0fa-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="2b0fa-108">Prerequisites</span></span>

<span data-ttu-id="2b0fa-109">Deberá cumplir los requisitos previos siguientes para completar este tutorial:</span><span class="sxs-lookup"><span data-stu-id="2b0fa-109">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="2b0fa-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) con estas cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="2b0fa-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="2b0fa-111">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="2b0fa-111">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="2b0fa-112">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="2b0fa-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="2b0fa-113">[SDK de .NET Core 2.0](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="2b0fa-113">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="2b0fa-114">Base de datos para blogs</span><span class="sxs-lookup"><span data-stu-id="2b0fa-114">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="2b0fa-115">Base de datos para blogs</span><span class="sxs-lookup"><span data-stu-id="2b0fa-115">Blogging database</span></span>

<span data-ttu-id="2b0fa-116">En este tutorial se usa una base de datos para **blogs** en la instancia de LocalDB como la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-116">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="2b0fa-117">Si ya creó la base de datos de **blogs** como parte de otro tutorial, puede omitir estos pasos.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-117">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="2b0fa-118">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b0fa-118">Open Visual Studio</span></span>
* <span data-ttu-id="2b0fa-119">**Herramientas -> Conectar con base de datos...**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-119">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="2b0fa-120">Seleccione **Microsoft SQL Server** y haga clic en **Continuar**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-120">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="2b0fa-121">Escriba **(localdb)\mssqllocaldb** como el **nombre del servidor**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-121">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="2b0fa-122">Escriba **maestra** como el **nombre de la base de datos** y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-122">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="2b0fa-123">La base de datos maestra ahora aparece en **Conexiones de base de datos** del **Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-123">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="2b0fa-124">Haga clic con el botón derecho en la base de datos en el **Explorador de servidores** y seleccione **Nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-124">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="2b0fa-125">En el editor de consultas, copie el script que aparece a continuación.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-125">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="2b0fa-126">Haga clic con el botón derecho en el editor de consultas y seleccione **Ejecutar**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-126">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="2b0fa-127">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="2b0fa-127">Create a new project</span></span>

* <span data-ttu-id="2b0fa-128">Abra Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2b0fa-128">Open Visual Studio 2017</span></span>
* <span data-ttu-id="2b0fa-129">**Archivo -> Nuevo -> Proyecto...**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-129">**File -> New -> Project...**</span></span>
* <span data-ttu-id="2b0fa-130">En el menú de la izquierda, seleccione **Instalado -> Plantillas -> Visual C# -> Web**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-130">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="2b0fa-131">Seleccione la plantilla de proyecto **Aplicación web de ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-131">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="2b0fa-132">Escriba **EFGetStarted.AspNetCore.ExistingDb** como el nombre y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-132">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="2b0fa-133">Espere que aparezca el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-133">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="2b0fa-134">En **ASP.NET Core Templates 2.0** (Plantillas 2.0 de ASP.NET Core), seleccione la **Aplicación web (Model-View-Controller)**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-134">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="2b0fa-135">Asegúrese de que la **autenticación** esté establecida en **Sin autenticación**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-135">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="2b0fa-136">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-136">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="2b0fa-137">Instalación de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2b0fa-137">Install Entity Framework</span></span>

<span data-ttu-id="2b0fa-138">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-138">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="2b0fa-139">Este tutorial usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-139">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="2b0fa-140">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="2b0fa-140">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="2b0fa-141">**Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-141">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="2b0fa-142">Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-142">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="2b0fa-143">Usaremos parte de Entity Framework Tools para crear un modelo desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-143">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="2b0fa-144">Por lo tanto, también instalaremos el paquete de herramientas:</span><span class="sxs-lookup"><span data-stu-id="2b0fa-144">So we will install the tools package as well:</span></span>

* <span data-ttu-id="2b0fa-145">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-145">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="2b0fa-146">Usaremos las herramientas de scaffolding de ASP.NET Core para crear más adelante controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-146">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="2b0fa-147">Por lo tanto, también instalaremos el paquete de diseño:</span><span class="sxs-lookup"><span data-stu-id="2b0fa-147">So we will install this design package as well:</span></span>

* <span data-ttu-id="2b0fa-148">Ejecute `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-148">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="2b0fa-149">Utilización de técnicas de ingeniería inversa para el modelo</span><span class="sxs-lookup"><span data-stu-id="2b0fa-149">Reverse engineer your model</span></span>

<span data-ttu-id="2b0fa-150">Ya es momento de crear el modelo de EF en función de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-150">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="2b0fa-151">**Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-151">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="2b0fa-152">Ejecute el comando siguiente para crear un modelo a partir de la base de datos existente:</span><span class="sxs-lookup"><span data-stu-id="2b0fa-152">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="2b0fa-153">Si recibe un error que indica `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, cierre Visual Studio y vuelva a abrirlo.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-153">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="2b0fa-154">Para especificar las tablas para las cuales desea generar entidades, agregue el argumento `-Tables` al comando anterior.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-154">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="2b0fa-155">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2b0fa-155">E.g.</span></span> <span data-ttu-id="2b0fa-156">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-156">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="2b0fa-157">El proceso de ingeniería inversa creó clases de entidad (`Blog.cs` & `Post.cs`) y un contexto derivado (`BloggingContext.cs`) en función del esquema de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-157">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="2b0fa-158">Las clases de entidad son simples objetos de C# que representan los datos que va a consultar y guardar.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-158">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="2b0fa-159">El contexto representa una sesión con la base de datos y le permite consultar y guardar instancias de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-159">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="2b0fa-160">Registro del contenido con inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="2b0fa-160">Register your context with dependency injection</span></span>

<span data-ttu-id="2b0fa-161">El concepto de la inserción de dependencias es fundamental para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-161">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="2b0fa-162">Los servicios (como `BloggingContext`) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-162">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="2b0fa-163">Luego se proporcionan estos servicios a los componentes que los necesitan (como los controladores MVC) a través de propiedades o parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-163">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="2b0fa-164">Para más información sobre la inserción de dependencias, consulte el artículo sobre la [inserción de dependencias](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) en el sitio de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-164">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="2b0fa-165">Eliminación de la configuración en contexto alineada</span><span class="sxs-lookup"><span data-stu-id="2b0fa-165">Remove inline context configuration</span></span>

<span data-ttu-id="2b0fa-166">En ASP.NET Core, la configuración se realiza generalmente en **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-166">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="2b0fa-167">Para cumplir con este patrón, migraremos la configuración del proveedor de bases de datos a **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-167">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="2b0fa-168">Abrir `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="2b0fa-168">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="2b0fa-169">Eliminación del método `OnConfiguring(...)`</span><span class="sxs-lookup"><span data-stu-id="2b0fa-169">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="2b0fa-170">Agregue el constructor siguiente, lo que permitirá que la inserción de dependencias pase la configuración al contexto</span><span class="sxs-lookup"><span data-stu-id="2b0fa-170">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="2b0fa-171">Registro y configuración del contexto en Startup.cs</span><span class="sxs-lookup"><span data-stu-id="2b0fa-171">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="2b0fa-172">Con el fin de que los controladores MVC usen `BloggingContext`, lo registraremos como servicio.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-172">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="2b0fa-173">Abra **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-173">Open **Startup.cs**</span></span>
* <span data-ttu-id="2b0fa-174">Agregue las instrucciones `using` siguientes en el comienzo del archivo</span><span class="sxs-lookup"><span data-stu-id="2b0fa-174">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="2b0fa-175">Ahora podemos usar el método `AddDbContext(...)` para registrarlo como servicio.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-175">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="2b0fa-176">Ubique el método `ConfigureServices(...)`</span><span class="sxs-lookup"><span data-stu-id="2b0fa-176">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="2b0fa-177">Agregue el código siguiente para registrar el contexto como servicio</span><span class="sxs-lookup"><span data-stu-id="2b0fa-177">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="2b0fa-178">En una aplicación real, habitualmente colocaría la cadena de conexión en un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-178">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="2b0fa-179">Por simplicidad, se define en el código.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-179">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="2b0fa-180">Para más información, consulte [Cadenas de conexión](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="2b0fa-180">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="2b0fa-181">Creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="2b0fa-181">Create a controller</span></span>

<span data-ttu-id="2b0fa-182">A continuación, habilitaremos scaffolding en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-182">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="2b0fa-183">Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar -> Controlador...**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="2b0fa-184">Seleccione **Dependencias completas** y haga clic en **Agregar**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-184">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="2b0fa-185">Puede omitir las instrucciones que aparecen en el archivo `ScaffoldingReadMe.txt` que se abre</span><span class="sxs-lookup"><span data-stu-id="2b0fa-185">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="2b0fa-186">Ahora que se habilitó scaffolding, podemos aplicarlo a un controlador de la entidad `Blog`.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-186">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="2b0fa-187">Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar -> Controlador...**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-187">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="2b0fa-188">Seleccione **Controlador de MVC con vistas que usan Entity Framework** y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-188">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="2b0fa-189">Establezca la **clase de modelo** en **Blog** y **Clase de contexto de datos** en **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-189">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="2b0fa-190">Haga clic en **Agregar**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-190">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="2b0fa-191">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="2b0fa-191">Run the application</span></span>

<span data-ttu-id="2b0fa-192">Ahora puede ejecutar la aplicación para verla en acción.</span><span class="sxs-lookup"><span data-stu-id="2b0fa-192">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="2b0fa-193">**Depurar -> Iniciar sin depuración**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-193">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="2b0fa-194">La aplicación se compilará y abrirá en un explorador web</span><span class="sxs-lookup"><span data-stu-id="2b0fa-194">The application will build and open in a web browser</span></span>
* <span data-ttu-id="2b0fa-195">Navegue a `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="2b0fa-195">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="2b0fa-196">Haga clic en **Crear nuevo**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-196">Click **Create New**</span></span>
* <span data-ttu-id="2b0fa-197">Escriba una **dirección URL** para el blog nuevo y haga clic en **Crear**</span><span class="sxs-lookup"><span data-stu-id="2b0fa-197">Enter a **Url** for the new blog and click **Create**</span></span>

![imagen](_static/create.png)

![imagen](_static/index-existing-db.png)
