---
title: 'Tipos de datos: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: d667cbcb821e321faed36d097b531c7c55b81248
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149159"
---
# <a name="data-types"></a>Tipos de datos

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

El tipo de datos hace referencia al tipo específico de la base de datos de la columna a la que está asignada una propiedad.

## <a name="conventions"></a>Convenciones

Por Convención, el proveedor de base de datos selecciona un tipo de datos basado en el tipo .NET de la propiedad. También tiene en cuenta otros metadatos, como la [longitud máxima](../max-length.md)configurada, si la propiedad forma parte de una clave principal, etc.

Por ejemplo, SQL Server usa `datetime2(7)` para `DateTime` las propiedades y `nvarchar(max)` para `string` las propiedades ( `nvarchar(450)` o `string` para las propiedades que se usan como clave).

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para especificar un tipo de datos exacto para una columna.

Por ejemplo, el código siguiente configura `Url` como una cadena no Unicode con una longitud máxima de `200` y `Rating` como decimal con la precisión `5` y la escala de `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>API fluida

También puede usar la API fluida para especificar los mismos tipos de datos para las columnas.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
