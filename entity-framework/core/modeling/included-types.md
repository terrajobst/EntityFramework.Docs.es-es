---
title: Incluir & excluyendo tipos EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655742"
---
# <a name="including--excluding-types"></a>Inclusión y exclusión de tipos

Incluir un tipo en el modelo significa que EF tiene metadatos sobre ese tipo e intentará leer y escribir instancias en la base de datos.

## <a name="conventions"></a>Convenciones

Por Convención, los tipos que se exponen en `DbSet` propiedades del contexto se incluyen en el modelo. Además, también se incluyen los tipos que se mencionan en el método `OnModelCreating`. Por último, en el modelo también se incluyen los tipos que se encuentran al explorar de forma recursiva las propiedades de navegación de los tipos detectados.

**Por ejemplo, en el código siguiente se detectan los tres tipos:**

* `Blog` porque se expone en una propiedad `DbSet` en el contexto

* `Post` porque se detecta a través de la propiedad de navegación `Blog.Posts`

* `AuditEntry` porque se menciona en `OnModelCreating`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para excluir un tipo del modelo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para excluir un tipo del modelo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
