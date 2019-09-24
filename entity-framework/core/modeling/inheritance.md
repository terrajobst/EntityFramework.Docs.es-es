---
title: 'Herencia: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197289"
---
# <a name="inheritance"></a>Herencia

La herencia en el modelo de EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.

## <a name="conventions"></a>Convenciones

Por Convención, depende del proveedor de base de datos determinar cómo se representará la herencia en la base de datos. Vea [herencia (base de datos relacional)](relational/inheritance.md) para ver cómo se controla con un proveedor de bases de datos relacionales.

EF solo instalará la herencia si dos o más tipos heredados se incluyen explícitamente en el modelo. EF no buscará tipos base o derivados que, de otro modo, no se incluyeron en el modelo. Puede incluir tipos en el modelo exponiendo un *DbSet<TEntity>*  para cada tipo en la jerarquía de herencia.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Si no desea exponer un *DbSet<TEntity>*  para una o varias entidades de la jerarquía, puede usar la API fluida para asegurarse de que se incluyen en el modelo.
Y si no se basa en convenciones, puede especificar el tipo base explícitamente mediante `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Puede usar `.HasBaseType((Type)null)` para quitar un tipo de entidad de la jerarquía.

## <a name="data-annotations"></a>Anotaciones de datos

No se pueden usar anotaciones de datos para configurar la herencia.

## <a name="fluent-api"></a>API fluida

La API fluida para la herencia depende del proveedor de base de datos que esté usando. Vea [herencia (base de datos relacional)](relational/inheritance.md) para ver la configuración que puede realizar para un proveedor de bases de datos relacionales.
