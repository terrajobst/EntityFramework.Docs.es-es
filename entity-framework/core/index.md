---
title: Descripción general - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 3befcbd3ff3da5dd159e6e6cb5fe7140c81317c2
ms.sourcegitcommit: a2b38dedc88ca3ccbfe7b1db9602ca02da8294cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2018
ms.locfileid: "34686667"
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="38cd1-102">Descripción general de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="38cd1-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="38cd1-103">Entity Framework (EF) Core es una versión ligera, extensible y multiplataforma de la popular tecnología de acceso a datos Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="38cd1-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="38cd1-104">EF Core puede servir como asignador relacional de objetos (O/RM), lo que permite a los desarrolladores de .NET trabajar con una base de datos mediante objetos .NET y eliminar la mayoría del código de acceso a los datos que normalmente deben escribir.</span><span class="sxs-lookup"><span data-stu-id="38cd1-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span> 

<span data-ttu-id="38cd1-105">EF Core es compatible con muchos motores de base de datos; vea [Proveedores de bases de datos](providers/index.md) para más información.</span><span class="sxs-lookup"><span data-stu-id="38cd1-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="38cd1-106">Si quiere aprender mediante la escritura de código, se recomienda usar una de las guías de [introducción](get-started/index.md) para empezar a trabajar con EF Core.</span><span class="sxs-lookup"><span data-stu-id="38cd1-106">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="what-is-new-in-ef-core"></a><span data-ttu-id="38cd1-107">Novedades de EF Core</span><span class="sxs-lookup"><span data-stu-id="38cd1-107">What is new in EF Core</span></span>

<span data-ttu-id="38cd1-108">Si está familiarizado con EF Core y quiere ir directamente a los detalles de las versiones más recientes:</span><span class="sxs-lookup"><span data-stu-id="38cd1-108">If you are familiar with EF Core and want to jump straight into the details of the latest releases:</span></span>

- <span data-ttu-id="38cd1-109">**[Novedades de EF Core2.1](xref:core/what-is-new/ef-core-2.1)**</span><span class="sxs-lookup"><span data-stu-id="38cd1-109">**[What is new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**</span></span>
- <span data-ttu-id="38cd1-110">**[Actualización de aplicaciones existentes a EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span><span class="sxs-lookup"><span data-stu-id="38cd1-110">**[Upgrading existing applications to EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span></span>


## <a name="get-entity-framework-core"></a><span data-ttu-id="38cd1-111">Obtener Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="38cd1-111">Get Entity Framework Core</span></span>

<span data-ttu-id="38cd1-112">[Instale el paquete NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) correspondiente al proveedor de base de datos que quiera usar.</span><span class="sxs-lookup"><span data-stu-id="38cd1-112">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="38cd1-113">P. ej.,</span><span class="sxs-lookup"><span data-stu-id="38cd1-113">E.g.</span></span> <span data-ttu-id="38cd1-114">para instalar el proveedor de SQL Server en el desarrollo multiplataforma mediante la herramienta `dotnet` en la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="38cd1-114">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="38cd1-115">O en Visual Studio, con la Consola del administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="38cd1-115">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="38cd1-116">Vea [Proveedores de bases de datos](providers/index.md) para más información sobre los proveedores disponibles e [Instalación de EF Core](get-started/install/index.md) para obtener pasos de instalación detallados.</span><span class="sxs-lookup"><span data-stu-id="38cd1-116">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="38cd1-117">El modelo</span><span class="sxs-lookup"><span data-stu-id="38cd1-117">The Model</span></span>

<span data-ttu-id="38cd1-118">Con EF Core, el acceso a datos se realiza mediante un modelo.</span><span class="sxs-lookup"><span data-stu-id="38cd1-118">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="38cd1-119">Un modelo se compone de clases de entidad y un contexto derivado que representa una sesión con la base de datos, lo que permite consultar y guardar los datos.</span><span class="sxs-lookup"><span data-stu-id="38cd1-119">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="38cd1-120">Vea [Creación de un modelo](modeling/index.md) para más información.</span><span class="sxs-lookup"><span data-stu-id="38cd1-120">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="38cd1-121">Puede generar un modelo a partir de una base de datos existente, codificar manualmente un modelo para que coincida con la base de datos o usar migraciones de EF para crear una base de datos a partir del modelo (y que evolucione a medida que cambia el modelo).</span><span class="sxs-lookup"><span data-stu-id="38cd1-121">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
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

## <a name="querying"></a><span data-ttu-id="38cd1-122">Consultas</span><span class="sxs-lookup"><span data-stu-id="38cd1-122">Querying</span></span>

<span data-ttu-id="38cd1-123">Las instancias de las clases de entidad se recuperan de la base de datos mediante Language Integrated Query (LINQ).</span><span class="sxs-lookup"><span data-stu-id="38cd1-123">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="38cd1-124">Vea [Consulta de datos](querying/index.md) para más información.</span><span class="sxs-lookup"><span data-stu-id="38cd1-124">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="38cd1-125">Guardado de datos</span><span class="sxs-lookup"><span data-stu-id="38cd1-125">Saving Data</span></span>

<span data-ttu-id="38cd1-126">Los datos se crean, se eliminan y se modifican en la base de datos mediante instancias de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="38cd1-126">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="38cd1-127">Vea [Guardado de datos](saving/index.md) para más información.</span><span class="sxs-lookup"><span data-stu-id="38cd1-127">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
