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
# <a name="inheritance"></a><span data-ttu-id="6e12b-102">Herencia</span><span class="sxs-lookup"><span data-stu-id="6e12b-102">Inheritance</span></span>

<span data-ttu-id="6e12b-103">Herencia en el modelo EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6e12b-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="6e12b-104">Convenciones</span><span class="sxs-lookup"><span data-stu-id="6e12b-104">Conventions</span></span>

<span data-ttu-id="6e12b-105">Por convención, es responsabilidad del proveedor de base de datos para determinar cómo se representará la herencia en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6e12b-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="6e12b-106">Vea [herencia (base de datos relacional)](relational/inheritance.md) de cómo esto se controla con un proveedor de base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="6e12b-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="6e12b-107">EF solo configurará herencia si dos o más tipos heredados están incluidos explícitamente en el modelo.</span><span class="sxs-lookup"><span data-stu-id="6e12b-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="6e12b-108">EF no se examinará para los tipos base o derivados que no se incluyeron en caso contrario, en el modelo.</span><span class="sxs-lookup"><span data-stu-id="6e12b-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="6e12b-109">Pueden incluir tipos en el modelo mediante la exposición de un *DbSet<TEntity>*  para cada tipo de la jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="6e12b-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="6e12b-110">Si no desea exponer una *DbSet<TEntity>*  para una o más entidades de la jerarquía, puede usar la API fluida para asegurarse de que se incluyen en el modelo.</span><span class="sxs-lookup"><span data-stu-id="6e12b-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="6e12b-111">Y si no confía en convenciones puede especificar el tipo base explícitamente mediante `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="6e12b-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="6e12b-112">Puede usar `.HasBaseType((Type)null)` para quitar un tipo de entidad de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="6e12b-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="6e12b-113">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="6e12b-113">Data Annotations</span></span>

<span data-ttu-id="6e12b-114">No puede usar anotaciones de datos para configurar la herencia.</span><span class="sxs-lookup"><span data-stu-id="6e12b-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="6e12b-115">API fluida</span><span class="sxs-lookup"><span data-stu-id="6e12b-115">Fluent API</span></span>

<span data-ttu-id="6e12b-116">La API fluida de herencia depende el proveedor de base de datos que esté utilizando.</span><span class="sxs-lookup"><span data-stu-id="6e12b-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="6e12b-117">Vea [herencia (base de datos relacional)](relational/inheritance.md) para la configuración puede realizar para un proveedor de base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="6e12b-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
