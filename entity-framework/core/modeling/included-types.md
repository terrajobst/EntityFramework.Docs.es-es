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
# <a name="including--excluding-types"></a>Inclusión y exclusión de tipos

Incluir en los medios de modelo que EF tiene metadatos acerca de que escriba y se intentarán leer y escribir las instancias de la base de datos desde y hacia un tipo.

## <a name="conventions"></a>Convenciones

Por convención, los tipos que se exponen en `DbSet` se incluyan las propiedades en el contexto en el modelo. Además, los tipos que se mencionan en la `OnModelCreating` método también se incluyen. Por último, también se incluyen los tipos que se encuentran mediante la exploración de las propiedades de navegación de tipos descubiertos de forma recursiva en el modelo.

**Por ejemplo, en la lista de código siguientes se detectan los tres tipos:**

* `Blog` Dado que se expone en un `DbSet` propiedad del contexto

* `Post` Dado que se detecta mediante la `Blog.Posts` propiedad de navegación

* `AuditEntry` porque se menciona en `OnModelCreating`

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

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para excluir un tipo del modelo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para excluir un tipo del modelo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=12)]
