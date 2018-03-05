---
title: Tokens de simultaneidad - Core EF
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: f3cf28d5c54e63aa76058e9fe1d9f3de5b37d579
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2018
---
# <a name="concurrency-tokens"></a>Tokens de simultaneidad

> [!NOTE]
> Esta página documenta cómo configurar los tokens de simultaneidad. Vea [controlar los conflictos de simultaneidad](../saving/concurrency.md) para obtener una explicación detallada de cómo funciona el control de simultaneidad en EF Core y ejemplos de cómo tratar los conflictos de simultaneidad en la aplicación.

Propiedades configuradas como tokens de simultaneidad se usan para implementar el control de simultaneidad optimista.

## <a name="conventions"></a>Convenciones

Por convención, propiedades nunca se configuran como tokens de simultaneidad.

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar las anotaciones de datos para configurar una propiedad como un token de simultaneidad.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una propiedad como un token de simultaneidad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Versión de fila/marca de tiempo

Una marca de tiempo es una propiedad que se genera un nuevo valor de la base de datos cada vez que se inserta o actualiza una fila. La propiedad también se trata como un token de simultaneidad. Esto garantiza que obtendrá una excepción si nadie ha modificado una fila que está intentando actualizar ya que la consulta para los datos.

Cómo esto se consigue es responsabilidad del proveedor de base de datos que se va a usar. Para SQL Server, marca de tiempo se utiliza normalmente en un *byte []* propiedad, que será el programa de instalación como un *ROWVERSION* columna en la base de datos.

### <a name="conventions"></a>Convenciones

Por convención, propiedades nunca se configuran como marcas de tiempo.

### <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para configurar una propiedad como una marca de tiempo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una propiedad como una marca de tiempo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
