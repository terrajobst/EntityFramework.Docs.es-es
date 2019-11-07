---
title: 'Columnas calculadas: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655915"
---
# <a name="computed-columns"></a>Columnas calculadas

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Una columna calculada es una columna cuyo valor se calcula en la base de datos. Una columna calculada puede utilizar otras columnas de la tabla para calcular su valor.

## <a name="conventions"></a>Convenciones

Por Convención, las columnas calculadas no se crean en el modelo.

## <a name="data-annotations"></a>Anotaciones de datos

Las columnas calculadas no se pueden configurar con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para especificar que una propiedad se debe asignar a una columna calculada.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
