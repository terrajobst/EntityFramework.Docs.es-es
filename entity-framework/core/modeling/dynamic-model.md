---
title: Alternando entre varios modelos con el mismo tipo de DbContext - Core EF
author: AndriySvyryd
uid: core/modeling/dynamic-model
ms.openlocfilehash: e6828c62c55ae6f48d46a20528268264c3f22b26
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternando entre varios modelos con el mismo tipo de DbContext

El modelo se basa `OnModelCreating` podría usar una propiedad en el contexto para cambiar cómo se crea el modelo. Por ejemplo puede usarse para excluir una propiedad determinada:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Sin embargo si intenta realizar los pasos anteriores sin cambios adicionales obtendría el mismo modelo cada vez que se crea un nuevo contexto para cualquier valor de `IgnoreIntProperty`. Esto se debe a que el modelo de almacenamiento en caché mecanismo EF se utiliza para mejorar el rendimiento mediante la invocación de sólo `OnModelCreating` una vez y el almacenamiento en caché del modelo.

De forma predeterminada EF asume que en un contexto determinado tipo de modelo será el mismo. Para ello, la implementación predeterminada de `IModelCacheKeyFactory` devuelve una clave que solo contiene el tipo de contexto. Para cambiar esto es necesario reemplazar el `IModelCacheKeyFactory` service. La nueva implementación debe devolver un objeto que se pueda comparar con otras claves de modelo mediante el `Equals` método que tiene en cuenta todas las variables que afectan al modelo:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
