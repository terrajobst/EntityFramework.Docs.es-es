---
title: Inclusión y exclusión de propiedades - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929830"
---
# <a name="including--excluding-properties"></a>Inclusión y exclusión de propiedades

Incluye una propiedad en el modelo, significa que EF tiene metadatos sobre esa propiedad e intentará leer y escribir valores desde y hacia la base de datos.

## <a name="conventions"></a>Convenciones

Por convención, las propiedades públicas con un captador y un establecedor se incluirán en el modelo.

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para excluir una propiedad del modelo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para excluir una propiedad del modelo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
