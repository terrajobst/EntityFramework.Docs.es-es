---
title: Claves alternativas - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys"></a>Claves alternativas

Una clave alternativa actúa como un identificador exclusivo alternativo para cada instancia de entidad además de la clave principal. Las claves alternativas pueden utilizarse como destino de una relación. Cuando se usa una base de datos relacional se asigna al concepto de un índice o restricción unique en las columnas de clave alternativas y uno o más restricciones de clave externa que hacen referencia a las columnas.

> [!TIP]  
> Si desea exigir la unicidad de una columna, a continuación, desea que un índice único en lugar de una clave alternativa, consulte [índices](indexes.md). En EF, claves alternativas de las proporcionan mayor funcionalidad que los índices únicos, ya que pueden utilizarse como destino de una clave externa.

Las claves alternativas normalmente se introducen automáticamente cuando sea necesario y no es necesario configurarlos manualmente. Vea [convenciones](#conventions) para obtener más detalles.

## <a name="conventions"></a>Convenciones

Por convención, se introdujo una clave alternativa automáticamente cuando se identifica una propiedad, que no es la clave principal, como el destino de una relación.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Anotaciones de datos

No se pueden configurar las claves alternativas usando anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una sola propiedad para que sea una clave alternativa.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

También puede usar la API fluida para configurar varias propiedades para que sea una clave alternativa (conocida como una clave compuesta alternativa).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
