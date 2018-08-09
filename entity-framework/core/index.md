---
title: Descripción general - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: d2c40356fc3b37251f95b08ee8bf07ed4eab7b80
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614313"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core es una versión ligera, extensible y multiplataforma de la popular tecnología de acceso a datos Entity Framework.

EF Core puede servir como asignador relacional de objetos (O/RM), lo que permite a los desarrolladores de .NET trabajar con una base de datos mediante objetos .NET y eliminar la mayoría del código de acceso a los datos que normalmente deben escribir.

EF Core es compatible con muchos motores de base de datos; vea [Proveedores de bases de datos](providers/index.md) para más información.

Si quiere aprender mediante la escritura de código, se recomienda usar una de las guías de [introducción](get-started/index.md) para empezar a trabajar con EF Core.

## <a name="what-is-new-in-ef-core"></a>Novedades de EF Core

Si está familiarizado con EF Core y quiere ir directamente a los detalles de las versiones más recientes:

- **[Novedades de EF Core2.1](xref:core/what-is-new/ef-core-2.1)**
- **[Actualización de aplicaciones existentes a EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**


## <a name="get-entity-framework-core"></a>Obtener Entity Framework Core

[Instale el paquete NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) correspondiente al proveedor de base de datos que quiera usar. Por ejemplo, para instalar el proveedor de SQL Server en el desarrollo multiplataforma mediante la herramienta `dotnet` en la línea de comandos:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

O en Visual Studio, con la Consola del administrador de paquetes:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
Vea [Proveedores de bases de datos](providers/index.md) para más información sobre los proveedores disponibles e [Instalación de EF Core](get-started/install/index.md) para obtener pasos de instalación detallados.

## <a name="the-model"></a>El modelo

Con EF Core, el acceso a datos se realiza mediante un modelo. Un modelo se compone de clases de entidad y un contexto derivado que representa una sesión con la base de datos, lo que permite consultar y guardar los datos. Vea [Creación de un modelo](modeling/index.md) para más información.

Puede generar un modelo a partir de una base de datos existente, codificar manualmente un modelo para que coincida con la base de datos o usar migraciones de EF para crear una base de datos a partir del modelo (y que evolucione a medida que cambia el modelo).

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

## <a name="querying"></a>Consultas

Las instancias de las clases de entidad se recuperan de la base de datos mediante Language Integrated Query (LINQ). Vea [Consulta de datos](querying/index.md) para más información.

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>Guardado de datos

Los datos se crean, se eliminan y se modifican en la base de datos mediante instancias de las clases de entidad. Vea [Guardado de datos](saving/index.md) para más información.

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
