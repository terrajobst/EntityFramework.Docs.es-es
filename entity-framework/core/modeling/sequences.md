---
title: 'Secuencias: EF Core'
author: roji
ms.date: 12/18/2019
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/sequences
ms.openlocfilehash: 6343e58dd79837cc69308a070c9814030505d7dd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414732"
---
# <a name="sequences"></a>Secuencias

> [!NOTE]  
> Las secuencias son una característica que normalmente solo admiten las bases de datos relacionales. Si utiliza una base de datos no relacional como Cosmos, consulte la documentación de la base de datos para generar valores únicos.

Una secuencia genera valores numéricos únicos y secuenciales en la base de datos. Las secuencias no están asociadas a una tabla específica y se pueden configurar varias tablas para que dibujen valores de la misma secuencia.

## <a name="basic-usage"></a>Uso básico

Puede configurar una secuencia en el modelo y, a continuación, utilizarla para generar valores para las propiedades:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

Tenga en cuenta que el SQL específico que se usa para generar un valor a partir de una secuencia es específico de la base de datos; el ejemplo anterior funciona en SQL Server pero producirá un error en otras bases de datos. Consulte la documentación específica de su base de datos para obtener más información.

## <a name="configuring-sequence-settings"></a>Configuración de las opciones de secuencia

También puede configurar aspectos adicionales de la secuencia, como su esquema, valor inicial, incremento, etc.:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
