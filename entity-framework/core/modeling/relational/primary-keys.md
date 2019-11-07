---
title: 'Claves principales: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656049"
---
# <a name="primary-keys"></a>Claves principales

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Se introduce una restricción primary key para la clave de cada tipo de entidad.

## <a name="conventions"></a>Convenciones

Por Convención, la clave principal de la base de datos se denominará `PK_<type name>`.

## <a name="data-annotations"></a>Anotaciones de datos

No se puede configurar ningún aspecto específico de la base de datos relacional con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar el nombre de la restricción primary key en la base de datos.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
