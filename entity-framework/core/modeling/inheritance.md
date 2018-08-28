---
title: 'Herencia: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995901"
---
# <a name="inheritance"></a>Herencia

Herencia en el modelo de EF se usa para controlar cómo se representa la herencia en las clases de entidad en la base de datos.

## <a name="conventions"></a>Convenciones

Por convención, es responsabilidad del proveedor de base de datos para determinar cómo se representará la herencia en la base de datos. Consulte [herencia (base de datos relacional)](relational/inheritance.md) para cómo esto se controla con un proveedor de base de datos relacional.

EF solo configurará herencia si dos o más tipos heredados están incluidos explícitamente en el modelo. EF no examinará los tipos bases o derivados que no se incluyeron en caso contrario, en el modelo. Puede incluir tipos en el modelo mediante la exposición de un *DbSet<TEntity>*  para cada tipo de la jerarquía de herencia.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Si no desea exponer un *DbSet<TEntity>*  para uno o más entidades de la jerarquía, puede usar la API Fluent para garantizar que se incluyen en el modelo.
Y si no dispone de las convenciones se puede especificar el tipo base explícitamente mediante `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Puede usar `.HasBaseType((Type)null)` para quitar un tipo de entidad de la jerarquía.

## <a name="data-annotations"></a>Anotaciones de datos

No se puede usar anotaciones de datos para configurar la herencia.

## <a name="fluent-api"></a>API fluida

La API Fluent para herencia depende de que usa el proveedor de base de datos. Consulte [herencia (base de datos relacional)](relational/inheritance.md) para la configuración puede realizar para un proveedor de base de datos relacional.
