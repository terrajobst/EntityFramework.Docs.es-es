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
# <a name="inheritance"></a><span data-ttu-id="53a61-102">Herencia</span><span class="sxs-lookup"><span data-stu-id="53a61-102">Inheritance</span></span>

<span data-ttu-id="53a61-103">Herencia en el modelo de EF se usa para controlar cómo se representa la herencia en las clases de entidad en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="53a61-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="53a61-104">Convenciones</span><span class="sxs-lookup"><span data-stu-id="53a61-104">Conventions</span></span>

<span data-ttu-id="53a61-105">Por convención, es responsabilidad del proveedor de base de datos para determinar cómo se representará la herencia en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="53a61-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="53a61-106">Consulte [herencia (base de datos relacional)](relational/inheritance.md) para cómo esto se controla con un proveedor de base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="53a61-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="53a61-107">EF solo configurará herencia si dos o más tipos heredados están incluidos explícitamente en el modelo.</span><span class="sxs-lookup"><span data-stu-id="53a61-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="53a61-108">EF no examinará los tipos bases o derivados que no se incluyeron en caso contrario, en el modelo.</span><span class="sxs-lookup"><span data-stu-id="53a61-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="53a61-109">Puede incluir tipos en el modelo mediante la exposición de un *DbSet<TEntity>*  para cada tipo de la jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="53a61-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="53a61-110">Si no desea exponer un *DbSet<TEntity>*  para uno o más entidades de la jerarquía, puede usar la API Fluent para garantizar que se incluyen en el modelo.</span><span class="sxs-lookup"><span data-stu-id="53a61-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="53a61-111">Y si no confía en convenciones, puede especificar el tipo base explícitamente mediante `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="53a61-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="53a61-112">Puede usar `.HasBaseType((Type)null)` para quitar un tipo de entidad de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="53a61-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="53a61-113">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="53a61-113">Data Annotations</span></span>

<span data-ttu-id="53a61-114">No se puede usar anotaciones de datos para configurar la herencia.</span><span class="sxs-lookup"><span data-stu-id="53a61-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="53a61-115">API fluida</span><span class="sxs-lookup"><span data-stu-id="53a61-115">Fluent API</span></span>

<span data-ttu-id="53a61-116">La API Fluent para herencia depende de que usa el proveedor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="53a61-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="53a61-117">Consulte [herencia (base de datos relacional)](relational/inheritance.md) para la configuración puede realizar para un proveedor de base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="53a61-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
