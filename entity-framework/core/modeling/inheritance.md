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
# <a name="inheritance"></a><span data-ttu-id="88d27-102">Herencia</span><span class="sxs-lookup"><span data-stu-id="88d27-102">Inheritance</span></span>

<span data-ttu-id="88d27-103">La herencia en el modelo de EF se utiliza para controlar cómo se representa la herencia en las clases de entidad en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="88d27-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="88d27-104">Convenciones</span><span class="sxs-lookup"><span data-stu-id="88d27-104">Conventions</span></span>

<span data-ttu-id="88d27-105">Por Convención, depende del proveedor de base de datos determinar cómo se representará la herencia en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="88d27-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="88d27-106">Vea [herencia (base de datos relacional)](relational/inheritance.md) para ver cómo se controla con un proveedor de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="88d27-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="88d27-107">EF solo instalará la herencia si dos o más tipos heredados se incluyen explícitamente en el modelo.</span><span class="sxs-lookup"><span data-stu-id="88d27-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="88d27-108">EF no buscará tipos base o derivados que, de otro modo, no se incluyeron en el modelo.</span><span class="sxs-lookup"><span data-stu-id="88d27-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="88d27-109">Puede incluir tipos en el modelo exponiendo un *DbSet<TEntity>*  para cada tipo en la jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="88d27-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="88d27-110">Si no desea exponer un *DbSet<TEntity>*  para una o varias entidades de la jerarquía, puede usar la API fluida para asegurarse de que se incluyen en el modelo.</span><span class="sxs-lookup"><span data-stu-id="88d27-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="88d27-111">Y si no se basa en convenciones, puede especificar el tipo base explícitamente mediante `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="88d27-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="88d27-112">Puede usar `.HasBaseType((Type)null)` para quitar un tipo de entidad de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="88d27-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="88d27-113">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="88d27-113">Data Annotations</span></span>

<span data-ttu-id="88d27-114">No se pueden usar anotaciones de datos para configurar la herencia.</span><span class="sxs-lookup"><span data-stu-id="88d27-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="88d27-115">API fluida</span><span class="sxs-lookup"><span data-stu-id="88d27-115">Fluent API</span></span>

<span data-ttu-id="88d27-116">La API fluida para la herencia depende del proveedor de base de datos que esté usando.</span><span class="sxs-lookup"><span data-stu-id="88d27-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="88d27-117">Vea [herencia (base de datos relacional)](relational/inheritance.md) para ver la configuración que puede realizar para un proveedor de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="88d27-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
