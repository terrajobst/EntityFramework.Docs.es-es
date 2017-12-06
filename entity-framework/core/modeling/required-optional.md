---
title: Propiedades obligatorias y opcionales - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="3f532-102">Propiedades necesarias y opcionales</span><span class="sxs-lookup"><span data-stu-id="3f532-102">Required and Optional Properties</span></span>

<span data-ttu-id="3f532-103">Una propiedad se considera opcional si es válido para que pueda contener `null`.</span><span class="sxs-lookup"><span data-stu-id="3f532-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="3f532-104">Si `null` no es un valor válido que se asignará a una propiedad, a continuación, se considera que una propiedad necesaria.</span><span class="sxs-lookup"><span data-stu-id="3f532-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="3f532-105">Convenciones</span><span class="sxs-lookup"><span data-stu-id="3f532-105">Conventions</span></span>

<span data-ttu-id="3f532-106">Por convención, una propiedad cuyo tipo CLR puede contener valores nulos se configurarán como opcionales (`string`, `int?`, `byte[]`, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="3f532-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="3f532-107">Propiedades cuyo tipo CLR no puede contener valores nulos se configurará según sea necesario (`int`, `decimal`, `bool`, etcetera.).</span><span class="sxs-lookup"><span data-stu-id="3f532-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="3f532-108">Una propiedad cuyo tipo CLR no puede contener null no se puede configurar como opcionales.</span><span class="sxs-lookup"><span data-stu-id="3f532-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="3f532-109">La propiedad siempre se considerarán requeridos por Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3f532-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3f532-110">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="3f532-110">Data Annotations</span></span>

<span data-ttu-id="3f532-111">Puede usar anotaciones de datos para indicar que una propiedad es obligatoria.</span><span class="sxs-lookup"><span data-stu-id="3f532-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="3f532-112">API fluida</span><span class="sxs-lookup"><span data-stu-id="3f532-112">Fluent API</span></span>

<span data-ttu-id="3f532-113">Puede usar la API fluida para indicar que una propiedad es obligatoria.</span><span class="sxs-lookup"><span data-stu-id="3f532-113">You can use the Fluent API to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
