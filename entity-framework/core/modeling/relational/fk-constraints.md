---
title: Restricciones Foreign Key - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
ms.technology: entity-framework-core
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 726f03e2ee4cd3ec851c9a861b75dd12f9203e9c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052745"
---
# <a name="foreign-key-constraints"></a>Restricciones de clave externa

> [!NOTE]  
> La configuración de esta sección es aplicable a bases de datos relacionales en general. Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).

Una restricción foreign key se introdujo para cada relación en el modelo.

## <a name="conventions"></a>Convenciones

Por convención, se denominan restricciones foreign key `FK_<dependent type name>_<principal type name>_<foreign key property name>`. Para las claves externas compuestas `<foreign key property name>` se convierte en una lista de subrayado separado de los nombres de propiedad de clave externa.

## <a name="data-annotations"></a>Anotaciones de datos

Nombres de restricción de clave externa no se puede configurar mediante las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar el nombre de restricción de clave externa de una relación.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/RelationshipConstraintName.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
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
```
