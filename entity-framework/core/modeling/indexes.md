---
title: 'Índices: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655696"
---
# <a name="indexes"></a>Índices

Los índices son un concepto común en muchos almacenes de datos. Aunque su implementación en el almacén de datos puede variar, se usan para realizar búsquedas basadas en una columna (o conjunto de columnas) más eficaces.

## <a name="conventions"></a>Convenciones

Por Convención, se crea un índice en cada propiedad (o conjunto de propiedades) que se usa como clave externa.

## <a name="data-annotations"></a>Anotaciones de datos

Los índices no se pueden crear con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para especificar un índice en una sola propiedad. De forma predeterminada, los índices no son únicos.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

También puede especificar que un índice debe ser único, lo que significa que dos entidades no pueden tener los mismos valores para las propiedades especificadas.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

También puede especificar un índice en más de una columna.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> Solo hay un índice por cada conjunto de propiedades. Si usa la API fluida para configurar un índice en un conjunto de propiedades que ya tiene definido un índice, ya sea por Convención o por configuración anterior, cambiará la definición de ese índice. Esto resulta útil si desea seguir configurando un índice creado por la Convención.
