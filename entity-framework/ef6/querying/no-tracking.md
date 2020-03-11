---
title: 'Consultas sin seguimiento: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414480"
---
# <a name="no-tracking-queries"></a>consultas de no seguimiento
En ocasiones, es posible que desee obtener las entidades de una consulta, pero el contexto no puede realizar el seguimiento de esas entidades. Esto puede dar lugar a un mejor rendimiento cuando se consulta un gran número de entidades en escenarios de solo lectura. Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.  

Un nuevo método de extensión AsNoTracking permite ejecutar cualquier consulta de esta manera. Por ejemplo:  

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
