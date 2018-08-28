---
title: Asignación de tabla - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 32c5e3cc0e498005ce8e6be1f1ee7e8ddf9b510d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994142"
---
# <a name="table-mapping"></a><span data-ttu-id="60549-102">Asignación de tabla</span><span class="sxs-lookup"><span data-stu-id="60549-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="60549-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="60549-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="60549-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="60549-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="60549-105">Asignación de tabla identifica qué datos de la tabla deben consultarse desde y se guardan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="60549-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="60549-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="60549-106">Conventions</span></span>

<span data-ttu-id="60549-107">Por convención, se configurará cada entidad para asignar a una tabla con el mismo nombre que el `DbSet<TEntity>` propiedad que expone la entidad en el contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="60549-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="60549-108">Si no hay ningún `DbSet<TEntity>` se incluye para la entidad determinada, se usa el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="60549-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="60549-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="60549-109">Data Annotations</span></span>

<span data-ttu-id="60549-110">Puede usar anotaciones de datos para configurar la tabla que se asigna un tipo a.</span><span class="sxs-lookup"><span data-stu-id="60549-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="60549-111">También puede especificar un esquema al que pertenece la tabla.</span><span class="sxs-lookup"><span data-stu-id="60549-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="60549-112">API fluida</span><span class="sxs-lookup"><span data-stu-id="60549-112">Fluent API</span></span>

<span data-ttu-id="60549-113">Puede usar la API Fluent para configurar la tabla que se asigna un tipo a.</span><span class="sxs-lookup"><span data-stu-id="60549-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="60549-114">También puede especificar un esquema al que pertenece la tabla.</span><span class="sxs-lookup"><span data-stu-id="60549-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
