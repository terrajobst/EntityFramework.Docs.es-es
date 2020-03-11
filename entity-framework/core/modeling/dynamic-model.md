---
title: Alternar entre varios modelos con el mismo tipo de DbContext EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: a160f0d382ee2a3ac7130ce1ac98eb24b3f79394
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413916"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="af8a7-102">Alternar entre varios modelos con el mismo tipo DbContext</span><span class="sxs-lookup"><span data-stu-id="af8a7-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="af8a7-103">El modelo integrado `OnModelCreating` puede utilizar una propiedad en el contexto para cambiar la forma en que se compila el modelo.</span><span class="sxs-lookup"><span data-stu-id="af8a7-103">The model built in `OnModelCreating` can use a property on the context to change how the model is built.</span></span> <span data-ttu-id="af8a7-104">Por ejemplo, supongamos que desea configurar una entidad de forma diferente en función de alguna propiedad:</span><span class="sxs-lookup"><span data-stu-id="af8a7-104">For example, suppose you wanted to configure an entity differently based on some property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

<span data-ttu-id="af8a7-105">Desafortunadamente, este código no funcionaría tal cual, ya que EF crea el modelo y se ejecuta `OnModelCreating` una sola vez, con lo que se almacena en caché el resultado por motivos de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="af8a7-105">Unfortunately, this code wouldn't work as-is, since EF builds the model and runs `OnModelCreating` only once, caching the result for performance reasons.</span></span> <span data-ttu-id="af8a7-106">Sin embargo, puede enlazar con el mecanismo de almacenamiento en caché del modelo para que EF tenga en cuenta la propiedad que genera distintos modelos.</span><span class="sxs-lookup"><span data-stu-id="af8a7-106">However, you can hook into the model caching mechanism to make EF aware of the property producing different models.</span></span>

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="af8a7-107">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="af8a7-107">IModelCacheKeyFactory</span></span>

<span data-ttu-id="af8a7-108">EF utiliza el `IModelCacheKeyFactory` para generar claves de caché para los modelos; de forma predeterminada, EF supone que para un tipo de contexto dado, el modelo será el mismo, por lo que la implementación predeterminada de este servicio devuelve una clave que solo contiene el tipo de contexto.</span><span class="sxs-lookup"><span data-stu-id="af8a7-108">EF uses the `IModelCacheKeyFactory` to generate cache keys for models; by default, EF assumes that for any given context type the model will be the same, so the default implementation of this service returns a key that just contains the context type.</span></span> <span data-ttu-id="af8a7-109">Para generar diferentes modelos a partir del mismo tipo de contexto, debe reemplazar el servicio `IModelCacheKeyFactory` con la implementación correcta. la clave generada se comparará con otras claves del modelo mediante el método `Equals`, teniendo en cuenta todas las variables que afectan al modelo:</span><span class="sxs-lookup"><span data-stu-id="af8a7-109">To produce different models from the same context type, you need to replace the `IModelCacheKeyFactory` service with the correct  implementation; the generated key will be compared to other model keys using the `Equals` method, taking into account all the variables that affect the model:</span></span>

<span data-ttu-id="af8a7-110">La siguiente implementación tiene en cuenta el `IgnoreIntProperty` al generar una clave de caché del modelo:</span><span class="sxs-lookup"><span data-stu-id="af8a7-110">The following implementation takes the `IgnoreIntProperty` into account when producing a model cache key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

<span data-ttu-id="af8a7-111">Por último, registre la nueva `IModelCacheKeyFactory` en el `OnConfiguring`de contexto:</span><span class="sxs-lookup"><span data-stu-id="af8a7-111">Finally, register your new `IModelCacheKeyFactory` in your context's `OnConfiguring`:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

<span data-ttu-id="af8a7-112">Vea el [proyecto de ejemplo completo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) para obtener más contexto.</span><span class="sxs-lookup"><span data-stu-id="af8a7-112">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) for more context.</span></span>
