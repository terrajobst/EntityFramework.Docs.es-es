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
# <a name="indexes"></a><span data-ttu-id="10c60-102">Índices</span><span class="sxs-lookup"><span data-stu-id="10c60-102">Indexes</span></span>

<span data-ttu-id="10c60-103">Los índices son un concepto común en muchos almacenes de datos.</span><span class="sxs-lookup"><span data-stu-id="10c60-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="10c60-104">Aunque su implementación en el almacén de datos puede variar, se usan para realizar búsquedas basadas en una columna (o conjunto de columnas) más eficaces.</span><span class="sxs-lookup"><span data-stu-id="10c60-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="10c60-105">Convenciones</span><span class="sxs-lookup"><span data-stu-id="10c60-105">Conventions</span></span>

<span data-ttu-id="10c60-106">Por Convención, se crea un índice en cada propiedad (o conjunto de propiedades) que se usa como clave externa.</span><span class="sxs-lookup"><span data-stu-id="10c60-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="10c60-107">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="10c60-107">Data Annotations</span></span>

<span data-ttu-id="10c60-108">Los índices no se pueden crear con anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="10c60-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="10c60-109">API fluida</span><span class="sxs-lookup"><span data-stu-id="10c60-109">Fluent API</span></span>

<span data-ttu-id="10c60-110">Puede usar la API fluida para especificar un índice en una sola propiedad.</span><span class="sxs-lookup"><span data-stu-id="10c60-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="10c60-111">De forma predeterminada, los índices no son únicos.</span><span class="sxs-lookup"><span data-stu-id="10c60-111">By default, indexes are non-unique.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

<span data-ttu-id="10c60-112">También puede especificar que un índice debe ser único, lo que significa que dos entidades no pueden tener los mismos valores para las propiedades especificadas.</span><span class="sxs-lookup"><span data-stu-id="10c60-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

<span data-ttu-id="10c60-113">También puede especificar un índice en más de una columna.</span><span class="sxs-lookup"><span data-stu-id="10c60-113">You can also specify an index over more than one column.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> <span data-ttu-id="10c60-114">Solo hay un índice por cada conjunto de propiedades.</span><span class="sxs-lookup"><span data-stu-id="10c60-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="10c60-115">Si usa la API fluida para configurar un índice en un conjunto de propiedades que ya tiene definido un índice, ya sea por Convención o por configuración anterior, cambiará la definición de ese índice.</span><span class="sxs-lookup"><span data-stu-id="10c60-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="10c60-116">Esto resulta útil si desea seguir configurando un índice creado por la Convención.</span><span class="sxs-lookup"><span data-stu-id="10c60-116">This is useful if you want to further configure an index that was created by convention.</span></span>
