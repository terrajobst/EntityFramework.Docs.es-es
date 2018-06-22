---
title: Asignación de columna - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
ms.technology: entity-framework-core
uid: core/modeling/relational/columns
ms.openlocfilehash: 697b966dbac892e332fe65feaa4dd11f00dd8298
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052905"
---
# <a name="column-mapping"></a><span data-ttu-id="d13a2-102">Asignación de columna</span><span class="sxs-lookup"><span data-stu-id="d13a2-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="d13a2-103">La configuración de esta sección es aplicable a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="d13a2-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d13a2-104">Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).</span><span class="sxs-lookup"><span data-stu-id="d13a2-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d13a2-105">Asignación de columna identifica qué datos de la columna deben consultar desde y se guardan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d13a2-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="d13a2-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="d13a2-106">Conventions</span></span>

<span data-ttu-id="d13a2-107">Por convención, cada propiedad se configurará para asignar a una columna con el mismo nombre que la propiedad.</span><span class="sxs-lookup"><span data-stu-id="d13a2-107">By convention, each property will be setup to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d13a2-108">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="d13a2-108">Data Annotations</span></span>

<span data-ttu-id="d13a2-109">Puede usar anotaciones de datos para configurar la columna a la que está asignada una propiedad.</span><span class="sxs-lookup"><span data-stu-id="d13a2-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="d13a2-110">API fluida</span><span class="sxs-lookup"><span data-stu-id="d13a2-110">Fluent API</span></span>

<span data-ttu-id="d13a2-111">Puede usar la API fluida para configurar la columna a la que está asignada una propiedad.</span><span class="sxs-lookup"><span data-stu-id="d13a2-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.BlogId)
            .HasColumnName("blog_id");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
