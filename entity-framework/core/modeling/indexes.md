---
title: "Índices - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a>Índices

Los índices son un concepto común entre varios almacenes de datos. Aunque puede variar en su implementación en el almacén de datos se utilizan para realizar búsquedas basadas en una columna (o conjunto de columnas) más eficaz.

## <a name="conventions"></a>Convenciones

Por convención, se crea un índice de cada propiedad (o conjunto de propiedades) que se usan como clave externa.

## <a name="data-annotations"></a>Anotaciones de datos

No se pueden crear índices con las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para especificar un índice en una sola propiedad. De forma predeterminada, los índices no son únicas.

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

También puede especificar que un índice debe ser único, lo que significa que no hay dos entidades pueden tener el mismo valores para la propiedad determinada.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

También puede especificar un índice sobre más de una columna.

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
> Hay solo un índice por cada conjunto de propiedades distinto. Si utiliza la API fluida para configurar un índice de un conjunto de propiedades que ya tiene un índice definido, ya sea mediante la convención o la configuración anterior, a continuación, va a cambiar la definición del índice. Esto es útil si desea seguir configurando un índice que se creó por convención.
