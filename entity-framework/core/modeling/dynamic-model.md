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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternando entre varios modelos con el mismo tipo de DbContext

El modelo se basa `OnModelCreating` podría usar una propiedad en el contexto para cambiar cómo se crea el modelo. Por ejemplo puede usarse para excluir una propiedad determinada:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Sin embargo si intenta realizar los pasos anteriores sin cambios adicionales obtendría el mismo modelo cada vez que se crea un nuevo contexto para cualquier valor de `IgnoreIntProperty`. Esto se debe a que el modelo de almacenamiento en caché mecanismo EF se utiliza para mejorar el rendimiento mediante la invocación de sólo `OnModelCreating` una vez y el almacenamiento en caché del modelo.

De forma predeterminada EF asume que en un contexto determinado tipo de modelo será el mismo. Para ello, la implementación predeterminada de `IModelCacheKeyFactory` devuelve una clave que solo contiene el tipo de contexto. Para cambiar esto es necesario reemplazar el `IModelCacheKeyFactory` service. La nueva implementación debe devolver un objeto que se pueda comparar con otras claves de modelo mediante el `Equals` método que tiene en cuenta todas las variables que afectan al modelo:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
