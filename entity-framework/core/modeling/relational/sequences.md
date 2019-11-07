---
title: 'Secuencias: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656111"
---
# <a name="sequences"></a>Secuencias

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Una secuencia genera valores numéricos secuenciales en la base de datos. Las secuencias no están asociadas a una tabla específica.

## <a name="conventions"></a>Convenciones

Por Convención, las secuencias no se incluyen en el modelo.

## <a name="data-annotations"></a>Anotaciones de datos

No se puede configurar una secuencia mediante anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para crear una secuencia en el modelo.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

También puede configurar el aspecto adicional de la secuencia, como su esquema, valor inicial e incremento.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

Una vez que se introduce una secuencia, puede usarla para generar valores para las propiedades del modelo. Por ejemplo, puede usar [los valores predeterminados](default-values.md) para insertar el siguiente valor de la secuencia.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
