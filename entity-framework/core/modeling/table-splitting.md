---
title: 'División de tablas: EF Core'
description: Cómo configurar la división de tablas mediante Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
uid: core/modeling/table-splitting
ms.openlocfilehash: 0e48c516de43cdc2b54c56f1a96f5e01f9fbbbc4
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824555"
---
# <a name="table-splitting"></a>División de tablas

>[!NOTE]
> Esta característica es nueva en EF Core 2,0.

EF Core permite asignar dos o más entidades a una sola fila. Esto se denomina _División de tablas_ o _uso compartido de tablas_.

## <a name="configuration"></a>Configuración de

Para usar la división de tablas, los tipos de entidad deben asignarse a la misma tabla, tener las claves principales asignadas a las mismas columnas y al menos una relación configurada entre la clave principal de un tipo de entidad y otra en la misma tabla.

Un escenario común para la división de tablas es usar solo un subconjunto de las columnas de la tabla para un mayor rendimiento o encapsulación.

En este ejemplo `Order` representa un subconjunto de `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Además de la configuración necesaria, llamamos `Property(o => o.Status).HasColumnName("Status")` para asignar `DetailedOrder.Status` a la misma columna que `Order.Status`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> Vea el [proyecto de ejemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) para obtener más contexto.

## <a name="usage"></a>Usage

Guardar y consultar entidades mediante la división de tablas se realiza de la misma manera que otras entidades. Y a partir de EF Core 3,0 se puede `null`la referencia de entidad dependiente. Si todas las columnas utilizadas por la entidad dependiente son `NULL` es la base de datos, no se creará ninguna instancia para ella cuando se realice la consulta. Esto también ocurre cuando todas las propiedades son opcionales y se establecen en `null`, lo que podría no ser el esperado.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Tokens de simultaneidad

Si alguno de los tipos de entidad que comparten una tabla tiene un token de simultaneidad, debe estar incluido en todos los demás tipos de entidad para evitar un valor de token de simultaneidad obsoleto cuando solo se actualiza una de las entidades asignadas a la misma tabla.

Para evitar que se exponga al código de consumo, es posible crear uno en el estado de sombra.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
