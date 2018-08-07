---
title: 'Introducción a .NET Framework: base de datos existente - EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 39e77ab8c124df67458cc5fa6db2882b65943ebe
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388473"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="0d81f-102">Introducción a EF Core en .NET Framework con una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="0d81f-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="0d81f-103">En este tutorial, compilará una aplicación de consola que realiza el acceso a datos básicos en una base de datos de Microsoft SQL Server mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0d81f-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="0d81f-104">Usará técnicas de ingeniería inversa para crear un modelo de Entity Framework en función de una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="0d81f-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="0d81f-105">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="0d81f-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d81f-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="0d81f-106">Prerequisites</span></span>

<span data-ttu-id="0d81f-107">Deberá cumplir los requisitos previos siguientes para completar este tutorial:</span><span class="sxs-lookup"><span data-stu-id="0d81f-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="0d81f-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/): al menos la versión 15.3</span><span class="sxs-lookup"><span data-stu-id="0d81f-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) - at least version 15.3</span></span>

* [<span data-ttu-id="0d81f-109">La versión más reciente del Administrador de paquetes de NuGet</span><span class="sxs-lookup"><span data-stu-id="0d81f-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="0d81f-110">La versión más reciente de Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="0d81f-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [<span data-ttu-id="0d81f-111">Base de datos para blogs</span><span class="sxs-lookup"><span data-stu-id="0d81f-111">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="0d81f-112">Base de datos para blogs</span><span class="sxs-lookup"><span data-stu-id="0d81f-112">Blogging database</span></span>

<span data-ttu-id="0d81f-113">En este tutorial se usa una base de datos para **blogs** en la instancia de LocalDB como la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="0d81f-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="0d81f-114">Si ya creó la base de datos de **blogs** como parte de otro tutorial, puede omitir estos pasos.</span><span class="sxs-lookup"><span data-stu-id="0d81f-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="0d81f-115">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d81f-115">Open Visual Studio</span></span>

* <span data-ttu-id="0d81f-116">Herramientas -> Conectar con base de datos...</span><span class="sxs-lookup"><span data-stu-id="0d81f-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="0d81f-117">Seleccione **Microsoft SQL Server** y haga clic en **Continuar**</span><span class="sxs-lookup"><span data-stu-id="0d81f-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="0d81f-118">Escriba **(localdb)\mssqllocaldb** como el **nombre del servidor**</span><span class="sxs-lookup"><span data-stu-id="0d81f-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="0d81f-119">Escriba **maestra** como el **nombre de la base de datos** y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="0d81f-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="0d81f-120">La base de datos maestra ahora aparece en **Conexiones de base de datos** del **Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="0d81f-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="0d81f-121">Haga clic con el botón derecho en la base de datos en el **Explorador de servidores** y seleccione **Nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="0d81f-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="0d81f-122">En el editor de consultas, copie el script que aparece a continuación.</span><span class="sxs-lookup"><span data-stu-id="0d81f-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="0d81f-123">Haga clic con el botón derecho en el editor de consultas y seleccione **Ejecutar**</span><span class="sxs-lookup"><span data-stu-id="0d81f-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="0d81f-124">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="0d81f-124">Create a new project</span></span>

* <span data-ttu-id="0d81f-125">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d81f-125">Open Visual Studio</span></span>

* <span data-ttu-id="0d81f-126">Archivo > Nuevo > Proyecto...</span><span class="sxs-lookup"><span data-stu-id="0d81f-126">File > New > Project...</span></span>

* <span data-ttu-id="0d81f-127">En el menú de la izquierda, seleccione Plantillas > Visual C# > Windows</span><span class="sxs-lookup"><span data-stu-id="0d81f-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="0d81f-128">Seleccione la plantilla de proyecto **Aplicación de consola**.</span><span class="sxs-lookup"><span data-stu-id="0d81f-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="0d81f-129">Asegúrese de tener como destino **.NET Framework 4.6.1** o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="0d81f-129">Ensure you are targeting **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="0d81f-130">Asigne un nombre al proyecto y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="0d81f-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="0d81f-131">Instalación de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0d81f-131">Install Entity Framework</span></span>

<span data-ttu-id="0d81f-132">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="0d81f-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="0d81f-133">Este tutorial usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0d81f-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="0d81f-134">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="0d81f-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="0d81f-135">Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="0d81f-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="0d81f-136">Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="0d81f-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="0d81f-137">Para habilitar técnicas de ingeniería inversa desde una base de datos existente, es necesario instalar también un par de otros paquetes.</span><span class="sxs-lookup"><span data-stu-id="0d81f-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="0d81f-138">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="0d81f-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="0d81f-139">Utilización de técnicas de ingeniería inversa para el modelo</span><span class="sxs-lookup"><span data-stu-id="0d81f-139">Reverse engineer your model</span></span>

<span data-ttu-id="0d81f-140">Ya es momento de crear el modelo de EF en función de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="0d81f-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="0d81f-141">Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="0d81f-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="0d81f-142">Ejecute el comando siguiente para crear un modelo a partir de la base de datos existente</span><span class="sxs-lookup"><span data-stu-id="0d81f-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="0d81f-143">El proceso de ingeniería inversa creó clases de entidad y un contexto derivado en función del esquema de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="0d81f-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="0d81f-144">Las clases de entidad son simples objetos de C# que representan los datos que va a consultar y guardar.</span><span class="sxs-lookup"><span data-stu-id="0d81f-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

<span data-ttu-id="0d81f-145">El contexto representa una sesión con la base de datos y le permite consultar y guardar instancias de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="0d81f-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
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

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a><span data-ttu-id="0d81f-146">Uso del modelo</span><span class="sxs-lookup"><span data-stu-id="0d81f-146">Use your model</span></span>

<span data-ttu-id="0d81f-147">Ahora puede usar el modelo para realizar el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="0d81f-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="0d81f-148">Abra *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="0d81f-148">Open *Program.cs*</span></span>

* <span data-ttu-id="0d81f-149">Reemplace el contenido del archivo por el código siguiente</span><span class="sxs-lookup"><span data-stu-id="0d81f-149">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="0d81f-150">Depurar > Iniciar sin depuración</span><span class="sxs-lookup"><span data-stu-id="0d81f-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="0d81f-151">Verá que un blog se guarda en la base de datos y luego los detalles de todos los blogs se imprimen en la consola.</span><span class="sxs-lookup"><span data-stu-id="0d81f-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![imagen](_static/output-existing-db.png)
