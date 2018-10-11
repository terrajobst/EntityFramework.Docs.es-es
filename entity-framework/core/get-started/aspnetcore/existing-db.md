---
title: 'Introducción a ASP.NET Core: base de datos existente - EF Core'
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 84e2e4bc1bdc774fa059fa893e0f8ac128931feb
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447188"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="f7eee-102">Introducción a EF Core en ASP.NET Core con una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="f7eee-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="f7eee-103">En este tutorial compilará una aplicación de MVC de ASP.NET Core que realiza el acceso a datos básicos mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="f7eee-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="f7eee-104">Para crear un modelo de Entity Framework, usará ingeniería inversa en una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="f7eee-104">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="f7eee-105">[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="f7eee-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f7eee-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f7eee-106">Prerequisites</span></span>

<span data-ttu-id="f7eee-107">Instale el software siguiente:</span><span class="sxs-lookup"><span data-stu-id="f7eee-107">Install the following software:</span></span>

* <span data-ttu-id="f7eee-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) con estas cargas de trabajo:</span><span class="sxs-lookup"><span data-stu-id="f7eee-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="f7eee-109">**Desarrollo de ASP.NET y web** (en **Web y nube**)</span><span class="sxs-lookup"><span data-stu-id="f7eee-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="f7eee-110">**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)</span><span class="sxs-lookup"><span data-stu-id="f7eee-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="f7eee-111">[SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="f7eee-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="f7eee-112">Creación de una base de datos para blogs</span><span class="sxs-lookup"><span data-stu-id="f7eee-112">Create Blogging database</span></span>

<span data-ttu-id="f7eee-113">En este tutorial se usa una base de datos para **blogs** en la instancia de LocalDB como la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="f7eee-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="f7eee-114">Si ya creó la base de datos de **blogs** como parte de otro tutorial, omita estos pasos.</span><span class="sxs-lookup"><span data-stu-id="f7eee-114">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="f7eee-115">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7eee-115">Open Visual Studio</span></span>
* <span data-ttu-id="f7eee-116">**Herramientas -> Conectar con base de datos...**</span><span class="sxs-lookup"><span data-stu-id="f7eee-116">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="f7eee-117">Seleccione **Microsoft SQL Server** y haga clic en **Continuar**</span><span class="sxs-lookup"><span data-stu-id="f7eee-117">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="f7eee-118">Escriba **(localdb)\mssqllocaldb** como el **nombre del servidor**</span><span class="sxs-lookup"><span data-stu-id="f7eee-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="f7eee-119">Escriba **maestra** como el **nombre de la base de datos** y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="f7eee-119">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="f7eee-120">La base de datos maestra ahora aparece en **Conexiones de base de datos** del **Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="f7eee-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="f7eee-121">Haga clic con el botón derecho en la base de datos en el **Explorador de servidores** y seleccione **Nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="f7eee-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="f7eee-122">En el editor de consultas, copie el script que aparece a continuación.</span><span class="sxs-lookup"><span data-stu-id="f7eee-122">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="f7eee-123">Haga clic con el botón derecho en el editor de consultas y seleccione **Ejecutar**</span><span class="sxs-lookup"><span data-stu-id="f7eee-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="f7eee-124">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="f7eee-124">Create a new project</span></span>

* <span data-ttu-id="f7eee-125">Abra Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f7eee-125">Open Visual Studio 2017</span></span>
* <span data-ttu-id="f7eee-126">**Archivo > Nuevo > Proyecto...**</span><span class="sxs-lookup"><span data-stu-id="f7eee-126">**File > New > Project...**</span></span>
* <span data-ttu-id="f7eee-127">En el menú de la izquierda, seleccione **Instalado > Visual C# > Web**.</span><span class="sxs-lookup"><span data-stu-id="f7eee-127">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="f7eee-128">Seleccione la plantilla de proyecto **Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f7eee-128">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="f7eee-129">Escriba **EFGetStarted.AspNetCore.ExistingDb** como el nombre y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="f7eee-129">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="f7eee-130">Espere que aparezca el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="f7eee-130">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="f7eee-131">Asegúrese de que el menú desplegable de la plataforma de destino esté establecido en **.NET Core** y de que el menú desplegable de versión esté establecido en **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="f7eee-131">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="f7eee-132">Seleccione la plantilla **Aplicación web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="f7eee-132">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="f7eee-133">Asegúrese de que la **autenticación** esté establecida en **Sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="f7eee-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="f7eee-134">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f7eee-134">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="f7eee-135">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f7eee-135">Install Entity Framework Core</span></span>

<span data-ttu-id="f7eee-136">Para instalar EF Core, instale el paquete de los proveedores de bases de datos de EF Core que quiera establecer como destino.</span><span class="sxs-lookup"><span data-stu-id="f7eee-136">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="f7eee-137">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="f7eee-137">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="f7eee-138">Para este tutorial no tendrá que instalar ningún paquete de proveedor porque se usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f7eee-138">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="f7eee-139">El paquete de proveedor de SQL Server está incluido en el metapaquete [Microsoft.AspNetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="f7eee-139">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="f7eee-140">Utilización de técnicas de ingeniería inversa para el modelo</span><span class="sxs-lookup"><span data-stu-id="f7eee-140">Reverse engineer your model</span></span>

<span data-ttu-id="f7eee-141">Ya es momento de crear el modelo de EF en función de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="f7eee-141">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="f7eee-142">**Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="f7eee-142">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="f7eee-143">Ejecute el comando siguiente para crear un modelo a partir de la base de datos existente:</span><span class="sxs-lookup"><span data-stu-id="f7eee-143">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="f7eee-144">Si recibe un error que indica `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, cierre Visual Studio y vuelva a abrirlo.</span><span class="sxs-lookup"><span data-stu-id="f7eee-144">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="f7eee-145">Para especificar las tablas para las cuales desea generar entidades, agregue el argumento `-Tables` al comando anterior.</span><span class="sxs-lookup"><span data-stu-id="f7eee-145">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="f7eee-146">Por ejemplo: `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="f7eee-146">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="f7eee-147">El proceso de ingeniería inversa creó clases de entidad (`Blog.cs` & `Post.cs`) y un contexto derivado (`BloggingContext.cs`) en función del esquema de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="f7eee-147">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="f7eee-148">Las clases de entidad son simples objetos de C# que representan los datos que va a consultar y guardar.</span><span class="sxs-lookup"><span data-stu-id="f7eee-148">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="f7eee-149">Estas son las clases de entidad `Blog` y `Post`:</span><span class="sxs-lookup"><span data-stu-id="f7eee-149">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="f7eee-150">Para habilitar la carga diferida, puede usar las propiedades de navegación `virtual` (Blog.Post y Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="f7eee-150">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="f7eee-151">El contexto representa una sesión con la base de datos y le permite consultar y guardar instancias de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="f7eee-151">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="f7eee-152">Registro del contenido con inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="f7eee-152">Register your context with dependency injection</span></span>

<span data-ttu-id="f7eee-153">El concepto de la inserción de dependencias es fundamental para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f7eee-153">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="f7eee-154">Los servicios (como `BloggingContext`) se registran con inserción de dependencias durante el inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f7eee-154">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="f7eee-155">Luego se proporcionan estos servicios a los componentes que los necesitan (como los controladores MVC) a través de propiedades o parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="f7eee-155">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="f7eee-156">Para más información sobre la inserción de dependencias, consulte el artículo sobre la [inserción de dependencias](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) en el sitio de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f7eee-156">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="f7eee-157">Registro y configuración del contexto en Startup.cs</span><span class="sxs-lookup"><span data-stu-id="f7eee-157">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="f7eee-158">Para que `BloggingContext` esté disponible para los controladores MVC, debe registrarlo como servicio.</span><span class="sxs-lookup"><span data-stu-id="f7eee-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="f7eee-159">Abra **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="f7eee-159">Open **Startup.cs**</span></span>
* <span data-ttu-id="f7eee-160">Agregue las instrucciones `using` siguientes en el comienzo del archivo</span><span class="sxs-lookup"><span data-stu-id="f7eee-160">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="f7eee-161">Ahora puede usar el método `AddDbContext(...)` para registrarlo como servicio.</span><span class="sxs-lookup"><span data-stu-id="f7eee-161">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="f7eee-162">Ubique el método `ConfigureServices(...)`</span><span class="sxs-lookup"><span data-stu-id="f7eee-162">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="f7eee-163">Agregue el código siguiente resaltado para registrar el contexto como servicio.</span><span class="sxs-lookup"><span data-stu-id="f7eee-163">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="f7eee-164">En una aplicación real, lo habitual sería colocar la cadena de conexión en un archivo de configuración o una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="f7eee-164">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="f7eee-165">Por simplicidad, en este tutorial se define en código.</span><span class="sxs-lookup"><span data-stu-id="f7eee-165">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="f7eee-166">Para más información, consulte [Cadenas de conexión](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="f7eee-166">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="f7eee-167">Crear un controlador y vistas</span><span class="sxs-lookup"><span data-stu-id="f7eee-167">Create a controller and views</span></span>

* <span data-ttu-id="f7eee-168">Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar -> Controlador...**</span><span class="sxs-lookup"><span data-stu-id="f7eee-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="f7eee-169">Seleccione **Controlador de MVC con vistas que usan Entity Framework** y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="f7eee-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="f7eee-170">Establezca la **clase de modelo** en **Blog** y **Clase de contexto de datos** en **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="f7eee-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="f7eee-171">Haga clic en **Agregar**</span><span class="sxs-lookup"><span data-stu-id="f7eee-171">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="f7eee-172">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="f7eee-172">Run the application</span></span>

<span data-ttu-id="f7eee-173">Ahora puede ejecutar la aplicación para verla en acción.</span><span class="sxs-lookup"><span data-stu-id="f7eee-173">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="f7eee-174">**Depurar -> Iniciar sin depuración**</span><span class="sxs-lookup"><span data-stu-id="f7eee-174">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="f7eee-175">La aplicación se compila y se abre en un explorador web.</span><span class="sxs-lookup"><span data-stu-id="f7eee-175">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="f7eee-176">Navegue a `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="f7eee-176">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="f7eee-177">Haga clic en **Crear nuevo**</span><span class="sxs-lookup"><span data-stu-id="f7eee-177">Click **Create New**</span></span>
* <span data-ttu-id="f7eee-178">Escriba una **dirección URL** para el blog nuevo y haga clic en **Crear**</span><span class="sxs-lookup"><span data-stu-id="f7eee-178">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Página Crear](_static/create.png)

  ![Página de índice](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="f7eee-181">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f7eee-181">Next steps</span></span>

<span data-ttu-id="f7eee-182">Para obtener más información sobre cómo aplicar scaffolding a un contexto y a clases de entidad, vea los siguientes artículos:</span><span class="sxs-lookup"><span data-stu-id="f7eee-182">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="f7eee-183">Referencia sobre las herramientas de Entity Framework Core (CLI de .NET)</span><span class="sxs-lookup"><span data-stu-id="f7eee-183">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="f7eee-184">Referencia sobre las herramientas de Entity Framework Core (Consola del Administrador de paquetes)</span><span class="sxs-lookup"><span data-stu-id="f7eee-184">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)