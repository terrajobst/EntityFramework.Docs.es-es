---
title: Claves alternativas (restricciones únicas) - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052795"
---
# <a name="alternate-keys-unique-constraints"></a>Claves alternativas (restricciones únicas)

> [!NOTE]  
> La configuración de esta sección es aplicable a bases de datos relacionales en general. Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).

Una restricción unique se introdujo para cada clave alternativa en el modelo.

## <a name="conventions"></a>Convenciones

Por convención, se llamará el índice y la restricción que se introducen para una clave alternativa `AK_<type name>_<property name>`. Para las claves compuestas de alternativas `<property name>` se convierte en una lista de subrayado separado de los nombres de propiedad.

## <a name="data-annotations"></a>Anotaciones de datos

Restricciones UNIQUE no se pueden configurar mediante las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar el nombre de índice y de restricción para una clave alternativa.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
