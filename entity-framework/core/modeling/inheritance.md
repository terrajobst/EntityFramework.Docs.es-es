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
# <a name="inheritance"></a><span data-ttu-id="5c6f6-103">Herencia</span><span class="sxs-lookup"><span data-stu-id="5c6f6-103">Inheritance</span></span>

<span data-ttu-id="5c6f6-104">La herencia en el modelo de EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-104">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="5c6f6-105">Convenciones</span><span class="sxs-lookup"><span data-stu-id="5c6f6-105">Conventions</span></span>

<span data-ttu-id="5c6f6-106">De forma predeterminada, depende del proveedor de base de datos determinar cómo se representará la herencia en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-106">By default, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="5c6f6-107">Vea [herencia (base de datos relacional)](relational/inheritance.md) para ver cómo se controla con un proveedor de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-107">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="5c6f6-108">EF solo instalará la herencia si dos o más tipos heredados se incluyen explícitamente en el modelo.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-108">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="5c6f6-109">EF no buscará tipos base o derivados que, de otro modo, no se incluyeron en el modelo.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-109">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="5c6f6-110">Puede incluir tipos en el modelo exponiendo un > de *\<de DbSet* para cada tipo de la jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-110">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="5c6f6-111">Si no desea exponer un > de *\<de DbSet* para una o varias entidades de la jerarquía, puede usar la API fluida para asegurarse de que se incluyen en el modelo.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-111">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="5c6f6-112">Y si no se basa en las convenciones, puede especificar explícitamente el tipo base mediante `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-112">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="5c6f6-113">Puede usar `.HasBaseType((Type)null)` para quitar un tipo de entidad de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-113">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5c6f6-114">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="5c6f6-114">Data Annotations</span></span>

<span data-ttu-id="5c6f6-115">No se pueden usar anotaciones de datos para configurar la herencia.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5c6f6-116">API fluida</span><span class="sxs-lookup"><span data-stu-id="5c6f6-116">Fluent API</span></span>

<span data-ttu-id="5c6f6-117">La API fluida para la herencia depende del proveedor de base de datos que esté usando.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-117">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="5c6f6-118">Vea [herencia (base de datos relacional)](relational/inheritance.md) para ver la configuración que puede realizar para un proveedor de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="5c6f6-118">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
