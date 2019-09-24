---
title: Claves alternativas (restricciones únicas)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197614"
---
# <a name="alternate-keys-unique-constraints"></a>Claves alternativas (restricciones UNIQUE)

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Se introduce una restricción UNIQUE para cada clave alternativa del modelo.

## <a name="conventions"></a>Convenciones

Por Convención, el índice y la restricción que se introducen para una clave alternativa se `AK_<type name>_<property name>`denominarán. Para las claves `<property name>` alternativas compuestas se convierte en una lista de nombres de propiedad separados por guiones bajos.

## <a name="data-annotations"></a>Anotaciones de datos

Las restricciones UNIQUE no se pueden configurar con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar el índice y el nombre de la restricción para una clave alternativa.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
