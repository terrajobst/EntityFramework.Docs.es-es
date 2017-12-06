---
title: Herencia (base de datos relacional -) EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7f697dfe2b93c7b93a2dd14945732db4f37628c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="0dd4c-102">Herencia (base de datos relacional)</span><span class="sxs-lookup"><span data-stu-id="0dd4c-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="0dd4c-103">La configuración de esta sección es aplicable a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="0dd4c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0dd4c-104">Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).</span><span class="sxs-lookup"><span data-stu-id="0dd4c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0dd4c-105">Herencia en el modelo EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0dd4c-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="0dd4c-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="0dd4c-106">Conventions</span></span>

<span data-ttu-id="0dd4c-107">Por convención, la herencia se asignarán utilizando el modelo de tabla por jerarquía (TPH).</span><span class="sxs-lookup"><span data-stu-id="0dd4c-107">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="0dd4c-108">TPH usa una sola tabla para almacenar los datos para todos los tipos en la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="0dd4c-108">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="0dd4c-109">Se usa una columna discriminadora para identificar qué tipo de cada fila representa.</span><span class="sxs-lookup"><span data-stu-id="0dd4c-109">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="0dd4c-110">EF solo configurará herencia si dos o más tipos heredados están incluidos explícitamente en el modelo (vea [herencia](../inheritance.md) para obtener más detalles).</span><span class="sxs-lookup"><span data-stu-id="0dd4c-110">EF will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="0dd4c-111">A continuación se muestra un ejemplo que muestra un escenario de herencia simple y los datos almacenados en una tabla de base de datos relacional mediante el patrón TPH.</span><span class="sxs-lookup"><span data-stu-id="0dd4c-111">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="0dd4c-112">El *discriminador* columna identifica el tipo de *Blog* se almacena en cada fila.</span><span class="sxs-lookup"><span data-stu-id="0dd4c-112">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="0dd4c-114">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="0dd4c-114">Data Annotations</span></span>

<span data-ttu-id="0dd4c-115">No puede usar anotaciones de datos para configurar la herencia.</span><span class="sxs-lookup"><span data-stu-id="0dd4c-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0dd4c-116">API fluida</span><span class="sxs-lookup"><span data-stu-id="0dd4c-116">Fluent API</span></span>

<span data-ttu-id="0dd4c-117">Puede usar la API fluida para configurar el nombre y tipo de la columna discriminadora y los valores que se usan para identificar cada tipo en la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="0dd4c-117">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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
