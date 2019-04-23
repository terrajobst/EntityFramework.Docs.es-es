---
title: Asignación de columnas - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929867"
---
# <a name="column-mapping"></a>Asignación de columnas

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Asignación de columna identifica qué datos de columna deben consultarse desde y guardas en la base de datos.

## <a name="conventions"></a>Convenciones

Por convención, cada propiedad se configurará para asignar a una columna con el mismo nombre que la propiedad.

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para configurar la columna a la que se asigna una propiedad.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para configurar la columna a la que se asigna una propiedad.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
