---
title: Tipos de datos - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2017
---
# <a name="data-types"></a>Tipos de datos

> [!NOTE]  
> La configuración de esta sección es aplicable a bases de datos relacionales en general. Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).

Tipo de datos hace referencia para el tipo específico de la base de datos de la columna a la que está asignada una propiedad.

## <a name="conventions"></a>Convenciones

Por convención, el proveedor de base de datos selecciona un tipo de datos basado en el tipo CLR de la propiedad. También tiene en cuenta otros metadatos, como los configurados [longitud máxima](../max-length.md), si la propiedad forma parte de una clave principal, etcetera.

Por ejemplo, SQL Server usa `datetime2(7)` para `DateTime` propiedades, y `nvarchar(max)` para `string` propiedades (o `nvarchar(450)` para `string` propiedades que se utilizan como una clave).

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para especificar un tipo de datos exacto para una columna.

Por ejemplo, el siguiente código configura `Url` como una cadena no unicode con una longitud máxima de `200` y `Rating` como decimal con una precisión de `5` y escalar de `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>API fluida

También puede utilizar la API fluida para especificar los mismos tipos de datos para las columnas.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
