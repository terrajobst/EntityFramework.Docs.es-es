---
title: Alternando entre varios modelos con el mismo tipo de DbContext - EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 1d87efb668c12a2862583fba16a6c444b3cda9de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994991"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="92a8d-102">Alternando entre varios modelos con el mismo tipo de DbContext</span><span class="sxs-lookup"><span data-stu-id="92a8d-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="92a8d-103">El modelo creado `OnModelCreating` podría usar una propiedad en el contexto para cambiar cómo se crea el modelo.</span><span class="sxs-lookup"><span data-stu-id="92a8d-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="92a8d-104">Por ejemplo, se podría usar para excluir una propiedad determinada:</span><span class="sxs-lookup"><span data-stu-id="92a8d-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="92a8d-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="92a8d-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="92a8d-106">Sin embargo si ha intentado realizar los pasos anteriores sin cambios adicionales obtendría el mismo modelo cada vez que se crea un nuevo contexto para cualquier valor de `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="92a8d-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="92a8d-107">Esto se debe a que el modelo de almacenamiento en caché el mecanismo de EF se usa para mejorar el rendimiento mediante la invocación de sólo `OnModelCreating` una vez y el modelo de almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="92a8d-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="92a8d-108">De forma predeterminada, EF supone que para cualquier contexto dado el modelo de tipo será el mismo.</span><span class="sxs-lookup"><span data-stu-id="92a8d-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="92a8d-109">Para realizar esta acción en la implementación predeterminada de `IModelCacheKeyFactory` devuelve una clave que solo contiene el tipo de contexto.</span><span class="sxs-lookup"><span data-stu-id="92a8d-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="92a8d-110">Para cambiar esto es necesario reemplazar el `IModelCacheKeyFactory` service.</span><span class="sxs-lookup"><span data-stu-id="92a8d-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="92a8d-111">La nueva implementación debe devolver un objeto que se pueda comparar a otras claves del modelo mediante el `Equals` método que tiene en cuenta todas las variables que afectan al modelo:</span><span class="sxs-lookup"><span data-stu-id="92a8d-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
