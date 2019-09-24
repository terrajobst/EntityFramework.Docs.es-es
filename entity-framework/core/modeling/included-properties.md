---
title: Incluir & excluyendo propiedades-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197414"
---
# <a name="including--excluding-properties"></a>Incluir & excluyendo propiedades

La inclusión de una propiedad en el modelo significa que EF tiene metadatos sobre esa propiedad e intentará leer y escribir valores desde y hacia la base de datos.

## <a name="conventions"></a>Convenciones

Por Convención, las propiedades públicas con un captador y un establecedor se incluirán en el modelo.

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para excluir una propiedad del modelo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para excluir una propiedad del modelo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
