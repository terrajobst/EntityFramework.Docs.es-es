---
title: Índices - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995485"
---
# <a name="indexes"></a>Índices

Los índices son un concepto común en muchos almacenes de datos. Aunque puede variar en su implementación en el almacén de datos se usan para realizar búsquedas basadas en una columna (o conjunto de columnas) más eficaz.

## <a name="conventions"></a>Convenciones

Por convención, se crea un índice en cada propiedad (o conjunto de propiedades) que se usan como clave externa.

## <a name="data-annotations"></a>Anotaciones de datos

No se pueden crear índices usando anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para especificar un índice en una sola propiedad. De forma predeterminada, los índices no son únicas.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

También puede especificar que un índice debe ser único, lo que significa que dos entidades no pueden tener el mismos o los valores para la propiedad especificada.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

También puede especificar un índice a través de más de una columna.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> Hay solo un índice por cada conjunto distinto de propiedades. Si usa la API Fluent para configurar un índice en un conjunto de propiedades que ya tiene un índice definido, por convención o configuración anterior, a continuación, va a cambiar la definición de ese índice. Esto es útil si desea seguir configurando un índice que se creó por convención.
