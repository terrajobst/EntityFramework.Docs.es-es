---
title: 'Claves alternativas: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655771"
---
# <a name="alternate-keys"></a>Claves alternativas

Una clave alternativa sirve como identificador único alternativo para cada instancia de entidad, además de la clave principal. Las claves alternativas se pueden usar como destino de una relación. Al utilizar una base de datos relacional, se asigna al concepto de un índice o una restricción únicos en las columnas de clave alternativas y una o varias restricciones de clave externa que hacen referencia a las columnas.

> [!TIP]  
> Si solo desea exigir la unicidad de una columna, querrá un índice único en lugar de una clave alternativa, consulte [índices](indexes.md). En EF, las claves alternativas proporcionan una mayor funcionalidad que los índices únicos porque se pueden usar como destino de una clave externa.

Normalmente, se introducen claves alternativas cuando sea necesario y no es necesario configurarlas manualmente. Vea [convenciones](#conventions) para obtener más detalles.

## <a name="conventions"></a>Convenciones

Por Convención, se introduce una clave alternativa cuando se identifica una propiedad, que no es la clave principal, como destino de una relación.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a>Anotaciones de datos

Las claves alternativas no se pueden configurar con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una sola propiedad para que sea una clave alternativa.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

También puede usar la API fluida para configurar varias propiedades de forma que sean una clave alternativa (conocida como clave alternativa compuesta).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
