---
title: 'Asignación de columnas: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197204"
---
# <a name="column-mapping"></a>Asignación de columnas

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

La asignación de columnas identifica qué datos de columna se deben consultar y guardar en la base de datos.

## <a name="conventions"></a>Convenciones

Por Convención, cada propiedad se configurará para asignarse a una columna con el mismo nombre que la propiedad.

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para configurar la columna a la que está asignada una propiedad.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar la columna a la que está asignada una propiedad.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
