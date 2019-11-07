---
title: Incluir & excluyendo tipos EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655742"
---
# <a name="including--excluding-types"></a><span data-ttu-id="148d9-102">Inclusión y exclusión de tipos</span><span class="sxs-lookup"><span data-stu-id="148d9-102">Including & Excluding Types</span></span>

<span data-ttu-id="148d9-103">Incluir un tipo en el modelo significa que EF tiene metadatos sobre ese tipo e intentará leer y escribir instancias en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="148d9-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="148d9-104">Convenciones</span><span class="sxs-lookup"><span data-stu-id="148d9-104">Conventions</span></span>

<span data-ttu-id="148d9-105">Por Convención, los tipos que se exponen en `DbSet` propiedades del contexto se incluyen en el modelo.</span><span class="sxs-lookup"><span data-stu-id="148d9-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="148d9-106">Además, también se incluyen los tipos que se mencionan en el método `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="148d9-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="148d9-107">Por último, en el modelo también se incluyen los tipos que se encuentran al explorar de forma recursiva las propiedades de navegación de los tipos detectados.</span><span class="sxs-lookup"><span data-stu-id="148d9-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="148d9-108">**Por ejemplo, en el código siguiente se detectan los tres tipos:**</span><span class="sxs-lookup"><span data-stu-id="148d9-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="148d9-109">`Blog` porque se expone en una propiedad `DbSet` en el contexto</span><span class="sxs-lookup"><span data-stu-id="148d9-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="148d9-110">`Post` porque se detecta a través de la propiedad de navegación `Blog.Posts`</span><span class="sxs-lookup"><span data-stu-id="148d9-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="148d9-111">`AuditEntry` porque se menciona en `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="148d9-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a><span data-ttu-id="148d9-112">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="148d9-112">Data Annotations</span></span>

<span data-ttu-id="148d9-113">Puede usar anotaciones de datos para excluir un tipo del modelo.</span><span class="sxs-lookup"><span data-stu-id="148d9-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="148d9-114">API fluida</span><span class="sxs-lookup"><span data-stu-id="148d9-114">Fluent API</span></span>

<span data-ttu-id="148d9-115">Puede usar la API fluida para excluir un tipo del modelo.</span><span class="sxs-lookup"><span data-stu-id="148d9-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
