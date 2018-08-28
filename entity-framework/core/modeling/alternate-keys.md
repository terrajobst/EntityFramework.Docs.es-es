---
title: Claves alternativas - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996976"
---
# <a name="alternate-keys"></a>Claves alternativas

Una clave alternativa actúa como un identificador exclusivo alternativo para cada instancia de entidad además de la clave principal. Claves alternativas se pueden usar como destino de una relación. Cuando se usa una base de datos relacional se asigna al concepto de un índice o restricción unique en las columnas de clave alternativas y uno o más restricciones foreign key que hacen referencia a las columnas.

> [!TIP]  
> Si desea exigir la unicidad de una columna, entonces le interesará un índice único en lugar de una clave alternativa, consulte [índices](indexes.md). En EF, las claves alternativas proporcionan mayor funcionalidad que los índices únicos porque se pueden usar como destino de una clave externa.

Claves alternativas normalmente se introducen automáticamente cuando sea necesario y no es necesario configurarlos manualmente. Consulte [convenciones](#conventions) para obtener más detalles.

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

Puede usar la API Fluent para configurar una propiedad única para que sea una clave alternativa.

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

También puede usar la API Fluent para configurar varias propiedades para que sea una clave alternativa (conocida como una clave compuesta alternativa).

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
