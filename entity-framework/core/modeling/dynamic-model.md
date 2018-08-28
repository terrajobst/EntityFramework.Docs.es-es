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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternando entre varios modelos con el mismo tipo de DbContext

El modelo creado `OnModelCreating` podría usar una propiedad en el contexto para cambiar cómo se crea el modelo. Por ejemplo, se podría usar para excluir una propiedad determinada:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Sin embargo si ha intentado realizar los pasos anteriores sin cambios adicionales obtendría el mismo modelo cada vez que se crea un nuevo contexto para cualquier valor de `IgnoreIntProperty`. Esto se debe a que el modelo de almacenamiento en caché el mecanismo de EF se usa para mejorar el rendimiento mediante la invocación de sólo `OnModelCreating` una vez y el modelo de almacenamiento en caché.

De forma predeterminada, EF supone que para cualquier contexto dado el modelo de tipo será el mismo. Para realizar esta acción en la implementación predeterminada de `IModelCacheKeyFactory` devuelve una clave que solo contiene el tipo de contexto. Para cambiar esto es necesario reemplazar el `IModelCacheKeyFactory` service. La nueva implementación debe devolver un objeto que se pueda comparar a otras claves del modelo mediante el `Equals` método que tiene en cuenta todas las variables que afectan al modelo:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
