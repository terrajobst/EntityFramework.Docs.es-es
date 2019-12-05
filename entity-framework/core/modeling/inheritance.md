---
title: 'Herencia: EF Core'
description: Cómo configurar la herencia de tipos de entidad mediante Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824678"
---
# <a name="inheritance"></a>Herencia

La herencia en el modelo de EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.

## <a name="conventions"></a>Convenciones

De forma predeterminada, depende del proveedor de base de datos determinar cómo se representará la herencia en la base de datos. Vea [herencia (base de datos relacional)](relational/inheritance.md) para ver cómo se controla con un proveedor de bases de datos relacionales.

EF solo instalará la herencia si dos o más tipos heredados se incluyen explícitamente en el modelo. EF no buscará tipos base o derivados que, de otro modo, no se incluyeron en el modelo. Puede incluir tipos en el modelo exponiendo un > de *\<de DbSet* para cada tipo de la jerarquía de herencia.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Si no desea exponer un > de *\<de DbSet* para una o varias entidades de la jerarquía, puede usar la API fluida para asegurarse de que se incluyen en el modelo.
Y si no se basa en las convenciones, puede especificar explícitamente el tipo base mediante `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Puede usar `.HasBaseType((Type)null)` para quitar un tipo de entidad de la jerarquía.

## <a name="data-annotations"></a>Anotaciones de datos

No se pueden usar anotaciones de datos para configurar la herencia.

## <a name="fluent-api"></a>API fluida

La API fluida para la herencia depende del proveedor de base de datos que esté usando. Vea [herencia (base de datos relacional)](relational/inheritance.md) para ver la configuración que puede realizar para un proveedor de bases de datos relacionales.
