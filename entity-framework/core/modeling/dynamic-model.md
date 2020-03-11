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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternar entre varios modelos con el mismo tipo DbContext

El modelo integrado `OnModelCreating` puede utilizar una propiedad en el contexto para cambiar la forma en que se compila el modelo. Por ejemplo, supongamos que desea configurar una entidad de forma diferente en función de alguna propiedad:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

Desafortunadamente, este código no funcionaría tal cual, ya que EF crea el modelo y se ejecuta `OnModelCreating` una sola vez, con lo que se almacena en caché el resultado por motivos de rendimiento. Sin embargo, puede enlazar con el mecanismo de almacenamiento en caché del modelo para que EF tenga en cuenta la propiedad que genera distintos modelos.

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

EF utiliza el `IModelCacheKeyFactory` para generar claves de caché para los modelos; de forma predeterminada, EF supone que para un tipo de contexto dado, el modelo será el mismo, por lo que la implementación predeterminada de este servicio devuelve una clave que solo contiene el tipo de contexto. Para generar diferentes modelos a partir del mismo tipo de contexto, debe reemplazar el servicio `IModelCacheKeyFactory` con la implementación correcta. la clave generada se comparará con otras claves del modelo mediante el método `Equals`, teniendo en cuenta todas las variables que afectan al modelo:

La siguiente implementación tiene en cuenta el `IgnoreIntProperty` al generar una clave de caché del modelo:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

Por último, registre la nueva `IModelCacheKeyFactory` en el `OnConfiguring`de contexto:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

Vea el [proyecto de ejemplo completo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) para obtener más contexto.
