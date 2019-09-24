---
title: 'Tokens de simultaneidad: EF Core'
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: db768c1de99000be91d33764ccd3c3924237f8bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197450"
---
# <a name="concurrency-tokens"></a>Tokens de simultaneidad

> [!NOTE]
> En esta página se documenta cómo configurar los tokens de simultaneidad. Vea [controlar los conflictos de simultaneidad](../saving/concurrency.md) para obtener una explicación detallada de cómo funciona el control de simultaneidad en EF Core y ejemplos de cómo controlar los conflictos de simultaneidad en la aplicación.

Las propiedades configuradas como tokens de simultaneidad se usan para implementar el control de simultaneidad optimista.

## <a name="conventions"></a>Convenciones

Por Convención, las propiedades nunca se configuran como tokens de simultaneidad.

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar las anotaciones de datos para configurar una propiedad como un token de simultaneidad.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una propiedad como un token de simultaneidad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Marca de tiempo/versión de fila

Una marca de tiempo es una propiedad donde la base de datos genera un nuevo valor cada vez que se inserta o se actualiza una fila. La propiedad también se trata como un token de simultaneidad. Esto garantiza que obtendrá una excepción si alguien más ha modificado una fila que está intentando actualizar desde que ha consultado los datos.

La forma de conseguirlo es el proveedor de base de datos que se está usando. Por SQL Server, la marca de tiempo se usa normalmente en una propiedad de *Byte []* , que se configurará como una columna *ROWVERSION* en la base de datos.

### <a name="conventions"></a>Convenciones

Por Convención, las propiedades nunca se configuran como marcas de tiempo.

### <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para configurar una propiedad como una marca de tiempo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una propiedad como una marca de tiempo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs#ConfigureTimestampFluent)]
