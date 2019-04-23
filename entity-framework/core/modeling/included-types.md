---
title: Inclusión y exclusión de tipos - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: f533b24312af37634ce4957e43c39ce776bf0bf0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929802"
---
# <a name="including--excluding-types"></a><span data-ttu-id="d0305-102">Inclusión y exclusión de tipos</span><span class="sxs-lookup"><span data-stu-id="d0305-102">Including & Excluding Types</span></span>

<span data-ttu-id="d0305-103">Incluir en los medios de modelo que EF tiene metadatos acerca de que escriba y se intentarán leer y escribir las instancias de la base de datos desde y hacia un tipo.</span><span class="sxs-lookup"><span data-stu-id="d0305-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="d0305-104">Convenciones</span><span class="sxs-lookup"><span data-stu-id="d0305-104">Conventions</span></span>

<span data-ttu-id="d0305-105">Por convención, los tipos que se exponen en `DbSet` se incluyan las propiedades en el contexto en el modelo.</span><span class="sxs-lookup"><span data-stu-id="d0305-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="d0305-106">Además, los tipos que se mencionan en la `OnModelCreating` método también se incluyen.</span><span class="sxs-lookup"><span data-stu-id="d0305-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="d0305-107">Por último, también se incluyen los tipos que se encuentran mediante la exploración de las propiedades de navegación de tipos descubiertos de forma recursiva en el modelo.</span><span class="sxs-lookup"><span data-stu-id="d0305-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="d0305-108">**Por ejemplo, en la lista de código siguientes se detectan los tres tipos:**</span><span class="sxs-lookup"><span data-stu-id="d0305-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="d0305-109">`Blog` Dado que se expone en un `DbSet` propiedad del contexto</span><span class="sxs-lookup"><span data-stu-id="d0305-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="d0305-110">`Post` Dado que se detecta mediante la `Blog.Posts` propiedad de navegación</span><span class="sxs-lookup"><span data-stu-id="d0305-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="d0305-111">`AuditEntry` porque se menciona en `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="d0305-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
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

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="d0305-112">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="d0305-112">Data Annotations</span></span>

<span data-ttu-id="d0305-113">Puede usar anotaciones de datos para excluir un tipo del modelo.</span><span class="sxs-lookup"><span data-stu-id="d0305-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="d0305-114">API fluida</span><span class="sxs-lookup"><span data-stu-id="d0305-114">Fluent API</span></span>

<span data-ttu-id="d0305-115">Puede usar la API Fluent para excluir un tipo del modelo.</span><span class="sxs-lookup"><span data-stu-id="d0305-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=12)]
