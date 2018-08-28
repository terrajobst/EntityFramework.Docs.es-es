---
title: Claves alternativas (restricciones únicas) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994196"
---
# <a name="alternate-keys-unique-constraints"></a>Claves alternativas (restricciones únicas)

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Se introdujo una restricción única para cada clave alternativa en el modelo.

## <a name="conventions"></a>Convenciones

Por convención, se denominará el índice y la restricción que se presentan para una clave alternativa `AK_<type name>_<property name>`. Para las claves alternativas compuestas `<property name>` se convierte en una lista separada por un carácter de subrayado de los nombres de propiedad.

## <a name="data-annotations"></a>Anotaciones de datos

Restricciones UNIQUE no se pueden configurar mediante las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para configurar el nombre de índice y de restricción para una clave alternativa.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
