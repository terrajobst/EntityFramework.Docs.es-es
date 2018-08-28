---
title: Consultas de no seguimiento - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: dba4127ade9481b40d4fd3c4323532ddfedf6980
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994245"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="f9845-102">consultas de no seguimiento</span><span class="sxs-lookup"><span data-stu-id="f9845-102">No-Tracking Queries</span></span>
<span data-ttu-id="f9845-103">A veces es posible que desee obtener entidades de una consulta, pero no tiene esas entidades se realiza el seguimiento del contexto.</span><span class="sxs-lookup"><span data-stu-id="f9845-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="f9845-104">Esto puede producir un mejor rendimiento cuando se consulta para un gran número de entidades en escenarios de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="f9845-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="f9845-105">Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.</span><span class="sxs-lookup"><span data-stu-id="f9845-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="f9845-106">Un nuevo método de extensión AsNoTracking permite cualquier consulta que se ejecutará en este modo.</span><span class="sxs-lookup"><span data-stu-id="f9845-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="f9845-107">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f9845-107">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs without tracking them
    var blogs1 = context.Blogs.AsNoTracking();

    // Query for some blogs without tracking them
    var blogs2 = context.Blogs
                        .Where(b => b.Name.Contains(".NET"))
                        .AsNoTracking()
                        .ToList();
}
```  
