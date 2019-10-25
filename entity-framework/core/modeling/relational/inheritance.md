---
title: Herencia (base de datos relacional)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: c660107619470a726fe13ad8eee2850749e6dcd9
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812084"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="2a208-102">Herencia (base de datos relacional)</span><span class="sxs-lookup"><span data-stu-id="2a208-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="2a208-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="2a208-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2a208-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="2a208-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2a208-105">La herencia en el modelo de EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2a208-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="2a208-106">Actualmente, solo el patrón de tabla por jerarquía (TPH) se implementa en EF Core.</span><span class="sxs-lookup"><span data-stu-id="2a208-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="2a208-107">Otros patrones comunes como tabla por tipo (TPT) y tabla por tipo específico (TPC) no están disponibles todavía.</span><span class="sxs-lookup"><span data-stu-id="2a208-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="2a208-108">Convenciones</span><span class="sxs-lookup"><span data-stu-id="2a208-108">Conventions</span></span>

<span data-ttu-id="2a208-109">Por Convención, la herencia se asignará mediante el patrón de tabla por jerarquía (TPH).</span><span class="sxs-lookup"><span data-stu-id="2a208-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="2a208-110">TPH usa una sola tabla para almacenar los datos de todos los tipos de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="2a208-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="2a208-111">Una columna de discriminador se utiliza para identificar el tipo que representa cada fila.</span><span class="sxs-lookup"><span data-stu-id="2a208-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="2a208-112">EF Core solo configurará la herencia si dos o más tipos heredados se incluyen explícitamente en el modelo (consulte [herencia](../inheritance.md) para obtener más detalles).</span><span class="sxs-lookup"><span data-stu-id="2a208-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="2a208-113">A continuación se muestra un ejemplo en el que se muestra un escenario de herencia simple y los datos almacenados en una tabla de base de datos relacional mediante el patrón TPH.</span><span class="sxs-lookup"><span data-stu-id="2a208-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="2a208-114">La columna *Discriminator* identifica el tipo de *blog* que se almacena en cada fila.</span><span class="sxs-lookup"><span data-stu-id="2a208-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/InheritanceDbSets.cs)] -->
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

>[!NOTE]
> <span data-ttu-id="2a208-116">Las columnas de la base de datos se convierten en NULL automáticamente según sea necesario al usar la asignación TPH.</span><span class="sxs-lookup"><span data-stu-id="2a208-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2a208-117">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="2a208-117">Data Annotations</span></span>

<span data-ttu-id="2a208-118">No se pueden usar anotaciones de datos para configurar la herencia.</span><span class="sxs-lookup"><span data-stu-id="2a208-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2a208-119">API fluida</span><span class="sxs-lookup"><span data-stu-id="2a208-119">Fluent API</span></span>

<span data-ttu-id="2a208-120">Puede usar la API fluida para configurar el nombre y el tipo de la columna discriminadora y los valores que se usan para identificar cada tipo en la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="2a208-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="2a208-121">Configurar la propiedad Discriminator</span><span class="sxs-lookup"><span data-stu-id="2a208-121">Configuring the discriminator property</span></span>

<span data-ttu-id="2a208-122">En los ejemplos anteriores, el discriminador se crea como una [propiedad Shadow](xref:core/modeling/shadow-properties) en la entidad base de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="2a208-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="2a208-123">Dado que es una propiedad del modelo, se puede configurar igual que otras propiedades.</span><span class="sxs-lookup"><span data-stu-id="2a208-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="2a208-124">Por ejemplo, para establecer la longitud máxima cuando se usa el discriminador predeterminado, por Convención:</span><span class="sxs-lookup"><span data-stu-id="2a208-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="2a208-125">El discriminador también se puede asignar a una propiedad de CLR real en la entidad.</span><span class="sxs-lookup"><span data-stu-id="2a208-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="2a208-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2a208-126">For example:</span></span>

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

<span data-ttu-id="2a208-127">Combinando estas dos cosas, es posible asignar el discriminador a una propiedad real y configurarlo:</span><span class="sxs-lookup"><span data-stu-id="2a208-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
