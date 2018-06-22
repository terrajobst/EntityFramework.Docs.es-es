---
title: 'Introducción a .NET Framework: base de datos nueva - EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812526"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="a6145-102">Introducción a EF Core en .NET Framework con una base de datos nueva</span><span class="sxs-lookup"><span data-stu-id="a6145-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="a6145-103">En este tutorial, compilará una aplicación de consola que realiza el acceso a datos básicos en una base de datos de Microsoft SQL Server mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a6145-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="a6145-104">Usará las migraciones para crear la base de datos a partir del modelo.</span><span class="sxs-lookup"><span data-stu-id="a6145-104">You will use migrations to create the database from your model.</span></span>

> [!TIP]  
> <span data-ttu-id="a6145-105">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="a6145-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6145-106">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="a6145-106">Prerequisites</span></span>

<span data-ttu-id="a6145-107">Deberá cumplir los requisitos previos siguientes para completar este tutorial:</span><span class="sxs-lookup"><span data-stu-id="a6145-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="a6145-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="a6145-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="a6145-109">La versión más reciente del Administrador de paquetes de NuGet</span><span class="sxs-lookup"><span data-stu-id="a6145-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="a6145-110">La versión más reciente de Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="a6145-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a><span data-ttu-id="a6145-111">Crear un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="a6145-111">Create a new project</span></span>

* <span data-ttu-id="a6145-112">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6145-112">Open Visual Studio</span></span>

* <span data-ttu-id="a6145-113">Archivo > Nuevo > Proyecto...</span><span class="sxs-lookup"><span data-stu-id="a6145-113">File > New > Project...</span></span>

* <span data-ttu-id="a6145-114">En el menú de la izquierda, seleccione Plantillas > Visual C# > Escritorio clásico de Windows</span><span class="sxs-lookup"><span data-stu-id="a6145-114">From the left menu select Templates > Visual C# > Windows Classic Desktop</span></span>

* <span data-ttu-id="a6145-115">Seleccione la plantilla del proyecto **Aplicación de consola (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="a6145-115">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="a6145-116">Asegúrese de tener como destino **.NET Framework 4.5.1** o una versión posterior</span><span class="sxs-lookup"><span data-stu-id="a6145-116">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="a6145-117">Asigne un nombre al proyecto y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="a6145-117">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="a6145-118">Instalación de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="a6145-118">Install Entity Framework</span></span>

<span data-ttu-id="a6145-119">Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse.</span><span class="sxs-lookup"><span data-stu-id="a6145-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="a6145-120">Este tutorial usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a6145-120">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="a6145-121">Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="a6145-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="a6145-122">Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="a6145-122">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="a6145-123">Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="a6145-123">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="a6145-124">Más adelante en este tutorial también usaremos parte de Entity Framework Tools para mantener la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a6145-124">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="a6145-125">Por lo tanto, también instalaremos el paquete de herramientas.</span><span class="sxs-lookup"><span data-stu-id="a6145-125">So we will install the tools package as well.</span></span>

* <span data-ttu-id="a6145-126">Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="a6145-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="a6145-127">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="a6145-127">Create your model</span></span>

<span data-ttu-id="a6145-128">Es momento de definir un contexto y clases de entidad que constituyan el modelo.</span><span class="sxs-lookup"><span data-stu-id="a6145-128">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="a6145-129">Proyecto > Agregar clase...</span><span class="sxs-lookup"><span data-stu-id="a6145-129">Project > Add Class...</span></span>

* <span data-ttu-id="a6145-130">Escriba *Model.cs* como el nombre y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="a6145-130">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="a6145-131">Reemplace el contenido del archivo por el código siguiente</span><span class="sxs-lookup"><span data-stu-id="a6145-131">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> <span data-ttu-id="a6145-132">En una aplicación real, colocaría cada clase en un archivo independiente y la cadena de conexión en el archivo `App.Config` y léala con `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="a6145-132">In a real application you would put each class in a separate file and put the connection string in the `App.Config` file and read it out using `ConfigurationManager`.</span></span> <span data-ttu-id="a6145-133">Por simplicidad, en este tutorial colocamos todos estos elementos en un solo archivo de código.</span><span class="sxs-lookup"><span data-stu-id="a6145-133">For the sake of simplicity, we are putting everything in a single code file for this tutorial.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="a6145-134">Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="a6145-134">Create your database</span></span>

<span data-ttu-id="a6145-135">Ahora que ya tiene un modelo, puede usar las migraciones para crear una base de datos para usted.</span><span class="sxs-lookup"><span data-stu-id="a6145-135">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="a6145-136">Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="a6145-136">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="a6145-137">Ejecute `Add-Migration MyFirstMigration` para aplicar scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.</span><span class="sxs-lookup"><span data-stu-id="a6145-137">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

* <span data-ttu-id="a6145-138">Ejecute `Update-Database` para aplicar la migración nueva a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a6145-138">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="a6145-139">Como la base de datos todavía no existe, se creará antes de que se aplique la migración.</span><span class="sxs-lookup"><span data-stu-id="a6145-139">Because your database doesn't exist yet, it will be created for you before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="a6145-140">Si hace cambios en el modelo a futuro, puede usar el comando `Add-Migration` para aplicar scaffolding a una migración nueva con el fin de hacer los cambios de esquema correspondientes a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a6145-140">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="a6145-141">Una vez que compruebe el código con scaffold (y haya hecho los cambios necesarios), puede usar el comando `Update-Database` para aplicar los cambios a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a6145-141">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
><span data-ttu-id="a6145-142">EF usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a6145-142">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="a6145-143">Uso del modelo</span><span class="sxs-lookup"><span data-stu-id="a6145-143">Use your model</span></span>

<span data-ttu-id="a6145-144">Ahora puede usar el modelo para realizar el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="a6145-144">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="a6145-145">Abra *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="a6145-145">Open *Program.cs*</span></span>

* <span data-ttu-id="a6145-146">Reemplace el contenido del archivo por el código siguiente</span><span class="sxs-lookup"><span data-stu-id="a6145-146">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="a6145-147">Depurar > Iniciar sin depuración</span><span class="sxs-lookup"><span data-stu-id="a6145-147">Debug > Start Without Debugging</span></span>

<span data-ttu-id="a6145-148">Verá que un blog se guarda en la base de datos y luego los detalles de todos los blogs se imprimen en la consola.</span><span class="sxs-lookup"><span data-stu-id="a6145-148">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![imagen](_static/output-new-db.png)
