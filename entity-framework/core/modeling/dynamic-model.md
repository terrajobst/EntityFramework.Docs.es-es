---
title: Alternando entre varios modelos con el mismo tipo de DbContext - Core EF
author: AndriySvyryd
ms.author: divega
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/dynamic-model
ms.openlocfilehash: 57136802001124ebf9ae7682e33f8dc7826fc2b0
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678727"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="fc5b9-102">Alternando entre varios modelos con el mismo tipo de DbContext</span><span class="sxs-lookup"><span data-stu-id="fc5b9-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="fc5b9-103">El modelo se basa `OnModelCreating` podría usar una propiedad en el contexto para cambiar cómo se crea el modelo.</span><span class="sxs-lookup"><span data-stu-id="fc5b9-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="fc5b9-104">Por ejemplo puede usarse para excluir una propiedad determinada:</span><span class="sxs-lookup"><span data-stu-id="fc5b9-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="fc5b9-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="fc5b9-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="fc5b9-106">Sin embargo si intenta realizar los pasos anteriores sin cambios adicionales obtendría el mismo modelo cada vez que se crea un nuevo contexto para cualquier valor de `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="fc5b9-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="fc5b9-107">Esto se debe a que el modelo de almacenamiento en caché mecanismo EF se utiliza para mejorar el rendimiento mediante la invocación de sólo `OnModelCreating` una vez y el almacenamiento en caché del modelo.</span><span class="sxs-lookup"><span data-stu-id="fc5b9-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="fc5b9-108">De forma predeterminada EF asume que en un contexto determinado tipo de modelo será el mismo.</span><span class="sxs-lookup"><span data-stu-id="fc5b9-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="fc5b9-109">Para ello, la implementación predeterminada de `IModelCacheKeyFactory` devuelve una clave que solo contiene el tipo de contexto.</span><span class="sxs-lookup"><span data-stu-id="fc5b9-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="fc5b9-110">Para cambiar esto es necesario reemplazar el `IModelCacheKeyFactory` service.</span><span class="sxs-lookup"><span data-stu-id="fc5b9-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="fc5b9-111">La nueva implementación debe devolver un objeto que se pueda comparar con otras claves de modelo mediante el `Equals` método que tiene en cuenta todas las variables que afectan al modelo:</span><span class="sxs-lookup"><span data-stu-id="fc5b9-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
