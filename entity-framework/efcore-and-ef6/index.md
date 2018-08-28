---
title: Comparar EF Core y EF6
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 09ffd8408ea8575ea367eaf2bdab4002db5c619e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997129"
---
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="3b277-102">Comparar EF Core y EF6</span><span class="sxs-lookup"><span data-stu-id="3b277-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="3b277-103">Hay dos versiones distintas de Entity Framework: Entity Framework Core y Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="3b277-103">There are two different versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="3b277-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="3b277-104">Entity Framework 6</span></span>

<span data-ttu-id="3b277-105">Entity Framework 6 (EF6) es una tecnología de acceso a datos probada con muchos años de características y estabilización.</span><span class="sxs-lookup"><span data-stu-id="3b277-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="3b277-106">Se publicó por primera vez en 2008, como parte de .NET Framework 3.5 SP1 y Visual Studio 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="3b277-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="3b277-107">A partir de la versión EF4.1, se ha distribuido como el [paquete NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/), actualmente uno de los paquetes más populares en NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="3b277-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently one of the most popular packages on NuGet.org.</span></span>

<span data-ttu-id="3b277-108">EF6 sigue siendo un producto que recibe soporte técnico para el que seguirá habiendo correcciones de errores y pequeñas mejoras durante algún tiempo.</span><span class="sxs-lookup"><span data-stu-id="3b277-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="3b277-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3b277-109">Entity Framework Core</span></span>

<span data-ttu-id="3b277-110">Entity Framework Core (EF Core) es una versión ligera, extensible y multiplataforma de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3b277-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="3b277-111">EF Core presenta numerosas mejoras y nuevas características en comparación con EF6.</span><span class="sxs-lookup"><span data-stu-id="3b277-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="3b277-112">Al mismo tiempo, EF Core es un nuevo código base y no está tan maduro como EF6.</span><span class="sxs-lookup"><span data-stu-id="3b277-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="3b277-113">EF Core mantiene la experiencia del desarrollador de EF6 y, además, la mayoría de las API de nivel superior siguen siendo las mismas, por lo que EF Core resultará muy familiar a los usuarios que hayan usado EF6.</span><span class="sxs-lookup"><span data-stu-id="3b277-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="3b277-114">Al mismo tiempo, EF Core se basa en un conjunto completamente nuevo de componentes principales.</span><span class="sxs-lookup"><span data-stu-id="3b277-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="3b277-115">Esto significa que EF Core no hereda automáticamente todas las características de EF6.</span><span class="sxs-lookup"><span data-stu-id="3b277-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="3b277-116">Algunas de estas características aparecerán en versiones futuras, mientras que otras de uso menos frecuente no se implementarán en EF Core.</span><span class="sxs-lookup"><span data-stu-id="3b277-116">While some of these features will show up in future releases, other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="3b277-117">El núcleo nuevo, que es extensible y ligero, también nos ha permitido agregar algunas características a EF Core que no se implementarán en EF6, como las claves alternativas, las actualizaciones por lotes y la evaluación mixta de cliente/base de datos en consultas LINQ.</span><span class="sxs-lookup"><span data-stu-id="3b277-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys, batch updates, and mixed client/database evaluation in LINQ queries).</span></span>
