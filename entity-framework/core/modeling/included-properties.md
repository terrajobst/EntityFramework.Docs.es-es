---
title: Inclusión y exclusión de propiedades - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 07b70e4517b67490e04a9ec9fa22b9b5d5217681
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998260"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="147c4-102">Inclusión y exclusión de propiedades</span><span class="sxs-lookup"><span data-stu-id="147c4-102">Including & Excluding Properties</span></span>

<span data-ttu-id="147c4-103">Incluye una propiedad en el modelo, significa que EF tiene metadatos sobre esa propiedad e intentará leer y escribir valores desde y hacia la base de datos.</span><span class="sxs-lookup"><span data-stu-id="147c4-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="147c4-104">Convenciones</span><span class="sxs-lookup"><span data-stu-id="147c4-104">Conventions</span></span>

<span data-ttu-id="147c4-105">Por convención, las propiedades públicas con un captador y un establecedor se incluirán en el modelo.</span><span class="sxs-lookup"><span data-stu-id="147c4-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="147c4-106">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="147c4-106">Data Annotations</span></span>

<span data-ttu-id="147c4-107">Puede usar anotaciones de datos para excluir una propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="147c4-107">You can use Data Annotations to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=6)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    [NotMapped]
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="147c4-108">API fluida</span><span class="sxs-lookup"><span data-stu-id="147c4-108">Fluent API</span></span>

<span data-ttu-id="147c4-109">Puede usar la API Fluent para excluir una propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="147c4-109">You can use the Fluent API to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Ignore(b => b.LoadedFromDatabase);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public DateTime LoadedFromDatabase { get; set; }
}
```
