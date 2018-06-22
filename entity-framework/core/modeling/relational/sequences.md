---
title: Secuencias - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
ms.technology: entity-framework-core
uid: core/modeling/relational/sequences
ms.openlocfilehash: 98a40aeecbec0fd9fb9cc108d6b5f98178dea403
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052715"
---
# <a name="sequences"></a><span data-ttu-id="f459e-102">Secuencias</span><span class="sxs-lookup"><span data-stu-id="f459e-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="f459e-103">La configuración de esta sección es aplicable a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="f459e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f459e-104">Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).</span><span class="sxs-lookup"><span data-stu-id="f459e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f459e-105">Una secuencia genera un secuenciales valores numéricos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f459e-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="f459e-106">Las secuencias no se asocian con una tabla específica.</span><span class="sxs-lookup"><span data-stu-id="f459e-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="f459e-107">Convenciones</span><span class="sxs-lookup"><span data-stu-id="f459e-107">Conventions</span></span>

<span data-ttu-id="f459e-108">Por convención, las secuencias no se incluyeron al modelo.</span><span class="sxs-lookup"><span data-stu-id="f459e-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f459e-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="f459e-109">Data Annotations</span></span>

<span data-ttu-id="f459e-110">No se puede configurar una secuencia mediante las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="f459e-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f459e-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="f459e-111">Fluent API</span></span>

<span data-ttu-id="f459e-112">Puede usar la API fluida para crear una secuencia en el modelo.</span><span class="sxs-lookup"><span data-stu-id="f459e-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="f459e-113">También puede configurar los aspectos adicionales de la secuencia, como su esquema, el valor inicial y el incremento.</span><span class="sxs-lookup"><span data-stu-id="f459e-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

<span data-ttu-id="f459e-114">Una vez que se introduce una secuencia, se puede usar para generar valores de propiedades en el modelo.</span><span class="sxs-lookup"><span data-stu-id="f459e-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="f459e-115">Por ejemplo, puede usar [valores predeterminados](default-values.md) para insertar el valor siguiente de la secuencia.</span><span class="sxs-lookup"><span data-stu-id="f459e-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
