---
title: Alternar entre varios modelos con el mismo tipo de DbContext EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 034076b1595894e80b98467354f6c9f139bd7426
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655726"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="1a247-102">Alternar entre varios modelos con el mismo tipo DbContext</span><span class="sxs-lookup"><span data-stu-id="1a247-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="1a247-103">El modelo compilado en `OnModelCreating` podría usar una propiedad en el contexto para cambiar el modo en que se compila el modelo.</span><span class="sxs-lookup"><span data-stu-id="1a247-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="1a247-104">Por ejemplo, podría usarse para excluir una determinada propiedad:</span><span class="sxs-lookup"><span data-stu-id="1a247-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="1a247-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="1a247-105">IModelCacheKeyFactory</span></span>

<span data-ttu-id="1a247-106">Sin embargo, si ha intentado hacer lo anterior sin realizar más cambios, obtendrá el mismo modelo cada vez que se cree un nuevo contexto para cualquier valor de `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="1a247-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="1a247-107">Esto se debe a que el mecanismo de almacenamiento en caché del modelo EF usa para mejorar el rendimiento mediante la invocación solo de `OnModelCreating` una vez y el almacenamiento en caché del modelo.</span><span class="sxs-lookup"><span data-stu-id="1a247-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="1a247-108">De forma predeterminada, EF presupone que en cualquier tipo de contexto dado el modelo será el mismo.</span><span class="sxs-lookup"><span data-stu-id="1a247-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="1a247-109">Para ello, la implementación predeterminada de `IModelCacheKeyFactory` devuelve una clave que solo contiene el tipo de contexto.</span><span class="sxs-lookup"><span data-stu-id="1a247-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="1a247-110">Para cambiar esto, debe reemplazar el servicio `IModelCacheKeyFactory`.</span><span class="sxs-lookup"><span data-stu-id="1a247-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="1a247-111">La nueva implementación debe devolver un objeto que se pueda comparar con otras claves del modelo mediante el método `Equals` que tiene en cuenta todas las variables que afectan al modelo:</span><span class="sxs-lookup"><span data-stu-id="1a247-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
