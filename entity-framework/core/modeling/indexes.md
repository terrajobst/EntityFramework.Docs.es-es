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
# <a name="indexes"></a><span data-ttu-id="c6085-102">Índices</span><span class="sxs-lookup"><span data-stu-id="c6085-102">Indexes</span></span>

<span data-ttu-id="c6085-103">Los índices son un concepto común entre varios almacenes de datos.</span><span class="sxs-lookup"><span data-stu-id="c6085-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="c6085-104">Aunque puede variar en su implementación en el almacén de datos se utilizan para realizar búsquedas basadas en una columna (o conjunto de columnas) más eficaz.</span><span class="sxs-lookup"><span data-stu-id="c6085-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="c6085-105">Convenciones</span><span class="sxs-lookup"><span data-stu-id="c6085-105">Conventions</span></span>

<span data-ttu-id="c6085-106">Por convención, se crea un índice de cada propiedad (o conjunto de propiedades) que se usan como clave externa.</span><span class="sxs-lookup"><span data-stu-id="c6085-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c6085-107">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="c6085-107">Data Annotations</span></span>

<span data-ttu-id="c6085-108">No se pueden crear índices con las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="c6085-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c6085-109">API fluida</span><span class="sxs-lookup"><span data-stu-id="c6085-109">Fluent API</span></span>

<span data-ttu-id="c6085-110">Puede usar la API fluida para especificar un índice en una sola propiedad.</span><span class="sxs-lookup"><span data-stu-id="c6085-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="c6085-111">De forma predeterminada, los índices no son únicas.</span><span class="sxs-lookup"><span data-stu-id="c6085-111">By default, indexes are non-unique.</span></span>

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

<span data-ttu-id="c6085-112">También puede especificar que un índice debe ser único, lo que significa que no hay dos entidades pueden tener el mismo valores para la propiedad determinada.</span><span class="sxs-lookup"><span data-stu-id="c6085-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="c6085-113">También puede especificar un índice sobre más de una columna.</span><span class="sxs-lookup"><span data-stu-id="c6085-113">You can also specify an index over more than one column.</span></span>

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
> <span data-ttu-id="c6085-114">Hay solo un índice por cada conjunto de propiedades distinto.</span><span class="sxs-lookup"><span data-stu-id="c6085-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="c6085-115">Si utiliza la API fluida para configurar un índice de un conjunto de propiedades que ya tiene un índice definido, ya sea mediante la convención o la configuración anterior, a continuación, va a cambiar la definición del índice.</span><span class="sxs-lookup"><span data-stu-id="c6085-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="c6085-116">Esto es útil si desea seguir configurando un índice que se creó por convención.</span><span class="sxs-lookup"><span data-stu-id="c6085-116">This is useful if you want to further configure an index that was created by convention.</span></span>
