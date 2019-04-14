---
title: Tabla dividir - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562595"
---
# <a name="table-splitting"></a>División de tablas

>[!NOTE]
> Esta característica es nueva en EF Core 2.0.

EF Core permite asignar dos o más entidades a una sola fila. Esto se denomina _división de tabla_ o _uso compartido de la tabla_.

## <a name="configuration"></a>Configuración

Para usar los tipos de entidad deben asignarse a la misma tabla de la división de tablas, tiene las claves principales que se asignan a las mismas columnas y al menos una relación configurado entre la clave principal de un tipo de entidad y otra en la misma tabla.

Un escenario común para la división de tablas está utilizando solo un subconjunto de las columnas de la tabla para mayor rendimiento o encapsulación.

En este ejemplo `Order` representa un subconjunto de `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Además de la configuración necesaria que llamamos `HasBaseType((string)null)` para evitar la asignación `DetailedOrder` en la misma jerarquía que `Order`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

Consulte la [proyecto de ejemplo completa](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) para obtener más contexto.

## <a name="usage"></a>Uso

Guardar y consultar entidades mediante la división de tablas se realiza en la misma manera que otras entidades, la única diferencia es que se deben realizar un seguimiento de todas las entidades de uso compartido de una fila para la inserción.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Tokens de simultaneidad

Si alguno de los tipos de entidad uso compartido de una tabla tiene un token de simultaneidad, a continuación, se debe incluir en todos los demás tipos de entidad para evitar un valor de token de simultaneidad obsoletos cuando se actualiza solo una de las entidades que se asigna a la misma tabla.

Para evitar la exposición a que el código utilizado es posible crear uno en estado de la sombra.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]