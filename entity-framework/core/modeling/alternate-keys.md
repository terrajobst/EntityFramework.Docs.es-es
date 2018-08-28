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
# <a name="alternate-keys"></a><span data-ttu-id="93f61-102">Claves alternativas</span><span class="sxs-lookup"><span data-stu-id="93f61-102">Alternate Keys</span></span>

<span data-ttu-id="93f61-103">Una clave alternativa actúa como un identificador exclusivo alternativo para cada instancia de entidad además de la clave principal.</span><span class="sxs-lookup"><span data-stu-id="93f61-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="93f61-104">Claves alternativas se pueden usar como destino de una relación.</span><span class="sxs-lookup"><span data-stu-id="93f61-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="93f61-105">Cuando se usa una base de datos relacional se asigna al concepto de un índice o restricción unique en las columnas de clave alternativas y uno o más restricciones foreign key que hacen referencia a las columnas.</span><span class="sxs-lookup"><span data-stu-id="93f61-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="93f61-106">Si desea exigir la unicidad de una columna, entonces le interesará un índice único en lugar de una clave alternativa, consulte [índices](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="93f61-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="93f61-107">En EF, las claves alternativas proporcionan mayor funcionalidad que los índices únicos porque se pueden usar como destino de una clave externa.</span><span class="sxs-lookup"><span data-stu-id="93f61-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="93f61-108">Claves alternativas normalmente se introducen automáticamente cuando sea necesario y no es necesario configurarlos manualmente.</span><span class="sxs-lookup"><span data-stu-id="93f61-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="93f61-109">Consulte [convenciones](#conventions) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="93f61-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="93f61-110">Convenciones</span><span class="sxs-lookup"><span data-stu-id="93f61-110">Conventions</span></span>

<span data-ttu-id="93f61-111">Por convención, se introdujo una clave alternativa automáticamente cuando se identifica una propiedad, que no es la clave principal, como el destino de una relación.</span><span class="sxs-lookup"><span data-stu-id="93f61-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="93f61-112">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="93f61-112">Data Annotations</span></span>

<span data-ttu-id="93f61-113">No se pueden configurar las claves alternativas usando anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="93f61-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="93f61-114">API fluida</span><span class="sxs-lookup"><span data-stu-id="93f61-114">Fluent API</span></span>

<span data-ttu-id="93f61-115">Puede usar la API Fluent para configurar una propiedad única para que sea una clave alternativa.</span><span class="sxs-lookup"><span data-stu-id="93f61-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

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

<span data-ttu-id="93f61-116">También puede usar la API Fluent para configurar varias propiedades para que sea una clave alternativa (conocida como una clave compuesta alternativa).</span><span class="sxs-lookup"><span data-stu-id="93f61-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

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
