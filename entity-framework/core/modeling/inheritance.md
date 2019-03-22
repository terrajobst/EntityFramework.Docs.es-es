---
title: 'Herencia: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: f6b5c8f5a398ac1e28e29bc17f0674c5b76d7b20
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319132"
---
# <a name="inheritance"></a>Herencia

Herencia en el modelo de EF se usa para controlar cómo se representa la herencia en las clases de entidad en la base de datos.

## <a name="conventions"></a>Convenciones

Por convención, es responsabilidad del proveedor de base de datos para determinar cómo se representará la herencia en la base de datos. Consulte [herencia (base de datos relacional)](relational/inheritance.md) para cómo esto se controla con un proveedor de base de datos relacional.

EF solo configurará herencia si dos o más tipos heredados están incluidos explícitamente en el modelo. EF no examinará los tipos bases o derivados que no se incluyeron en caso contrario, en el modelo. Puede incluir tipos en el modelo mediante la exposición de un *DbSet<TEntity>*  para cada tipo de la jerarquía de herencia.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Si no desea exponer un *DbSet<TEntity>*  para uno o más entidades de la jerarquía, puede usar la API Fluent para garantizar que se incluyen en el modelo.
Y si no confía en convenciones, puede especificar el tipo base explícitamente mediante `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Puede usar `.HasBaseType((Type)null)` para quitar un tipo de entidad de la jerarquía.

## <a name="data-annotations"></a>Anotaciones de datos

No se puede usar anotaciones de datos para configurar la herencia.

## <a name="fluent-api"></a>API fluida

La API Fluent para herencia depende de que usa el proveedor de base de datos. Consulte [herencia (base de datos relacional)](relational/inheritance.md) para la configuración puede realizar para un proveedor de base de datos relacional.
