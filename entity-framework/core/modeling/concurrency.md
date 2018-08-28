---
title: 'Tokens de simultaneidad: EF Core'
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 0051d416544a11385f99d36e45843c5b20725af7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994231"
---
# <a name="concurrency-tokens"></a>Tokens de simultaneidad

> [!NOTE]
> Esta página describe cómo configurar los tokens de simultaneidad. Consulte [controlar los conflictos de simultaneidad](../saving/concurrency.md) para obtener una explicación detallada de cómo funciona el control de simultaneidad en EF Core y ejemplos de cómo controlar los conflictos de simultaneidad en la aplicación.

Las propiedades configuradas como tokens de simultaneidad se usan para implementar el control de simultaneidad optimista.

## <a name="conventions"></a>Convenciones

Por convención, las propiedades nunca se configuran como tokens de simultaneidad.

## <a name="data-annotations"></a>Anotaciones de datos

Puede utilizar las anotaciones de datos para configurar una propiedad como un token de simultaneidad.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para configurar una propiedad como un token de simultaneidad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Versión de fila o marca de tiempo

Una marca de tiempo es una propiedad donde se genera un nuevo valor de la base de datos cada vez que se inserta o actualiza una fila. La propiedad también se trata como un token de simultaneidad. Esto garantiza que obtendrá una excepción si otra persona ha modificado una fila que está intentando actualizar desde que consultan los datos.

Forma de conseguirlo es responsabilidad del proveedor de base de datos que se va a usar. Para SQL Server, marca de tiempo se utiliza normalmente en un *byte []* propiedad, que será de instalación como un *ROWVERSION* columna en la base de datos.

### <a name="conventions"></a>Convenciones

Por convención, las propiedades nunca se configuran como marcas de tiempo.

### <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para configurar una propiedad como una marca de tiempo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para configurar una propiedad como una marca de tiempo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
