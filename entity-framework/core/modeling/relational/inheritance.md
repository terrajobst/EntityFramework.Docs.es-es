---
title: Herencia (base de datos relacional -) EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152371"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="133e8-102">Herencia (base de datos relacional)</span><span class="sxs-lookup"><span data-stu-id="133e8-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="133e8-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="133e8-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="133e8-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="133e8-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="133e8-105">Herencia en el modelo EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="133e8-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="133e8-106">Actualmente, solo el patrón de tabla por jerarquía (TPH) se implementa en EF Core.</span><span class="sxs-lookup"><span data-stu-id="133e8-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="133e8-107">Otros patrones comunes como tabla por tipo (TPT) y tabla-por-hormigón-type (TCP) no están disponibles.</span><span class="sxs-lookup"><span data-stu-id="133e8-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="133e8-108">Convenciones</span><span class="sxs-lookup"><span data-stu-id="133e8-108">Conventions</span></span>

<span data-ttu-id="133e8-109">Por convención, la herencia se asignarán utilizando el modelo de tabla por jerarquía (TPH).</span><span class="sxs-lookup"><span data-stu-id="133e8-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="133e8-110">TPH usa una sola tabla para almacenar los datos para todos los tipos en la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="133e8-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="133e8-111">Se usa una columna discriminadora para identificar qué tipo de cada fila representa.</span><span class="sxs-lookup"><span data-stu-id="133e8-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="133e8-112">EF Core solo configurará herencia si dos o más tipos heredados están incluidos explícitamente en el modelo (vea [herencia](../inheritance.md) para obtener más detalles).</span><span class="sxs-lookup"><span data-stu-id="133e8-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="133e8-113">A continuación se muestra un ejemplo que muestra un escenario de herencia simple y los datos almacenados en una tabla de base de datos relacional mediante el patrón TPH.</span><span class="sxs-lookup"><span data-stu-id="133e8-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="133e8-114">El *discriminador* columna identifica el tipo de *Blog* se almacena en cada fila.</span><span class="sxs-lookup"><span data-stu-id="133e8-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![imagen](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a><span data-ttu-id="133e8-116">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="133e8-116">Data Annotations</span></span>

<span data-ttu-id="133e8-117">No puede usar anotaciones de datos para configurar la herencia.</span><span class="sxs-lookup"><span data-stu-id="133e8-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="133e8-118">API fluida</span><span class="sxs-lookup"><span data-stu-id="133e8-118">Fluent API</span></span>

<span data-ttu-id="133e8-119">Puede usar la API fluida para configurar el nombre y tipo de la columna discriminadora y los valores que se usan para identificar cada tipo en la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="133e8-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="133e8-120">Configurar la propiedad discriminador</span><span class="sxs-lookup"><span data-stu-id="133e8-120">Configuring the discriminator property</span></span>

<span data-ttu-id="133e8-121">En los ejemplos anteriores, se crea el discriminador como un [sombrear a la propiedad](xref:core/modeling/shadow-properties) en la entidad base de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="133e8-121">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="133e8-122">Puesto que es una propiedad en el modelo, se puede configurar igual que otras propiedades.</span><span class="sxs-lookup"><span data-stu-id="133e8-122">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="133e8-123">Por ejemplo, para establecer la longitud máxima, cuando se usa el valor predeterminado, discriminador por convención:</span><span class="sxs-lookup"><span data-stu-id="133e8-123">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="133e8-124">El discriminador también puede asignarse a una propiedad CLR real en la entidad.</span><span class="sxs-lookup"><span data-stu-id="133e8-124">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="133e8-125">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="133e8-125">For example:</span></span>
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

<span data-ttu-id="133e8-126">Combinar estos dos cosas es posible tanto del discriminador se asignan a una propiedad real y configurarlo:</span><span class="sxs-lookup"><span data-stu-id="133e8-126">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
