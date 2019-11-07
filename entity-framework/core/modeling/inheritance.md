---
title: 'Herencia: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: abc1caa4d3839b7cdb52b316bcfc8f648b609b70
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655685"
---
# <a name="inheritance"></a><span data-ttu-id="5ca5e-102">Herencia</span><span class="sxs-lookup"><span data-stu-id="5ca5e-102">Inheritance</span></span>

<span data-ttu-id="5ca5e-103">La herencia en el modelo de EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="5ca5e-104">Convenciones</span><span class="sxs-lookup"><span data-stu-id="5ca5e-104">Conventions</span></span>

<span data-ttu-id="5ca5e-105">Por Convención, depende del proveedor de base de datos determinar cómo se representará la herencia en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="5ca5e-106">Vea [herencia (base de datos relacional)](relational/inheritance.md) para ver cómo se controla con un proveedor de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="5ca5e-107">EF solo instalará la herencia si dos o más tipos heredados se incluyen explícitamente en el modelo.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="5ca5e-108">EF no buscará tipos base o derivados que, de otro modo, no se incluyeron en el modelo.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="5ca5e-109">Puede incluir tipos en el modelo exponiendo un > de *\<de DbSet* para cada tipo de la jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-109">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="5ca5e-110">Si no desea exponer un > de *\<de DbSet* para una o varias entidades de la jerarquía, puede usar la API fluida para asegurarse de que se incluyen en el modelo.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-110">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="5ca5e-111">Y si no se basa en las convenciones, puede especificar explícitamente el tipo base mediante `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="5ca5e-112">Puede usar `.HasBaseType((Type)null)` para quitar un tipo de entidad de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5ca5e-113">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="5ca5e-113">Data Annotations</span></span>

<span data-ttu-id="5ca5e-114">No se pueden usar anotaciones de datos para configurar la herencia.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5ca5e-115">API fluida</span><span class="sxs-lookup"><span data-stu-id="5ca5e-115">Fluent API</span></span>

<span data-ttu-id="5ca5e-116">La API fluida para la herencia depende del proveedor de base de datos que esté usando.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="5ca5e-117">Vea [herencia (base de datos relacional)](relational/inheritance.md) para ver la configuración que puede realizar para un proveedor de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="5ca5e-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
