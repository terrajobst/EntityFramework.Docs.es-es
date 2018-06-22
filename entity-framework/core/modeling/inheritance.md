---
title: Herencia - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052785"
---
# <a name="inheritance"></a>Herencia

Herencia en el modelo EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.

## <a name="conventions"></a>Convenciones

Por convención, es responsabilidad del proveedor de base de datos para determinar cómo se representará la herencia en la base de datos. Vea [herencia (base de datos relacional)](relational/inheritance.md) de cómo esto se controla con un proveedor de base de datos relacional.

EF solo configurará herencia si dos o más tipos heredados están incluidos explícitamente en el modelo. EF no se examinará para los tipos base o derivados que no se incluyeron en caso contrario, en el modelo. Pueden incluir tipos en el modelo mediante la exposición de un *DbSet<TEntity>*  para cada tipo de la jerarquía de herencia.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Si no desea exponer una *DbSet<TEntity>*  para una o más entidades de la jerarquía, puede usar la API fluida para asegurarse de que se incluyen en el modelo.
Y si no confía en convenciones puede especificar el tipo base explícitamente mediante `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Puede usar `.HasBaseType((Type)null)` para quitar un tipo de entidad de la jerarquía.

## <a name="data-annotations"></a>Anotaciones de datos

No puede usar anotaciones de datos para configurar la herencia.

## <a name="fluent-api"></a>API fluida

La API fluida de herencia depende el proveedor de base de datos que esté utilizando. Vea [herencia (base de datos relacional)](relational/inheritance.md) para la configuración puede realizar para un proveedor de base de datos relacional.
