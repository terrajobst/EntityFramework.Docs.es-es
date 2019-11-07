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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternar entre varios modelos con el mismo tipo DbContext

El modelo compilado en `OnModelCreating` podría usar una propiedad en el contexto para cambiar el modo en que se compila el modelo. Por ejemplo, podría usarse para excluir una determinada propiedad:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

Sin embargo, si ha intentado hacer lo anterior sin realizar más cambios, obtendrá el mismo modelo cada vez que se cree un nuevo contexto para cualquier valor de `IgnoreIntProperty`. Esto se debe a que el mecanismo de almacenamiento en caché del modelo EF usa para mejorar el rendimiento mediante la invocación solo de `OnModelCreating` una vez y el almacenamiento en caché del modelo.

De forma predeterminada, EF presupone que en cualquier tipo de contexto dado el modelo será el mismo. Para ello, la implementación predeterminada de `IModelCacheKeyFactory` devuelve una clave que solo contiene el tipo de contexto. Para cambiar esto, debe reemplazar el servicio `IModelCacheKeyFactory`. La nueva implementación debe devolver un objeto que se pueda comparar con otras claves del modelo mediante el método `Equals` que tiene en cuenta todas las variables que afectan al modelo:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
