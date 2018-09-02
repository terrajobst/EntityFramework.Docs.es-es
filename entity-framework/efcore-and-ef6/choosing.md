---
title: 'EF Core y EF6: la opción adecuada para usted'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 83ae754f899b624d322c48e8de8432b65519546e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993838"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="acb35-102">EF Core y EF6: la opción adecuada para usted</span><span class="sxs-lookup"><span data-stu-id="acb35-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="acb35-103">La información siguiente le permitirá elegir entre Entity Framework Core y Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="acb35-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="acb35-104">Guía para las aplicaciones nuevas</span><span class="sxs-lookup"><span data-stu-id="acb35-104">Guidance for new applications</span></span>

<span data-ttu-id="acb35-105">Es recomendable usar EF Core para nuevas aplicaciones si quiere aprovechar todas las funcionalidades de EF Core y si su aplicación no requiere ninguna característica que todavía no se haya implementado en EF Core.</span><span class="sxs-lookup"><span data-stu-id="acb35-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="acb35-106">EF6 requiere .NET Framework 4.0 (o una versión posterior) y solo se admite en Windows (es decir, que no se ejecuta en .NET Core y no se admite en otros sistemas operativos), pero sigue siendo una opción viable para las aplicaciones nuevas siempre que pueda cumplir con esas restricciones y si la aplicación no requiere características nuevas de EF Core que no estén disponibles en EF6.</span><span class="sxs-lookup"><span data-stu-id="acb35-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (that is, EF6 does not currently run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="acb35-107">Revise la [comparación de características](features.md) para ver si EF Core puede ser una opción correcta para su aplicación.</span><span class="sxs-lookup"><span data-stu-id="acb35-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="acb35-108">Guía para las aplicaciones existentes de EF6</span><span class="sxs-lookup"><span data-stu-id="acb35-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="acb35-109">Debido a los cambios fundamentales en EF Core, no se recomienda que intente mover una aplicación EF6 a EF Core, salvo que tenga una razón convincente para hacerlo.</span><span class="sxs-lookup"><span data-stu-id="acb35-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="acb35-110">Si desea migrar a EF Core para usar características nuevas, asegúrese de conocer las limitaciones que tiene antes de comenzar.</span><span class="sxs-lookup"><span data-stu-id="acb35-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="acb35-111">Revise la [comparación de características](features.md) para ver si EF Core puede ser una opción correcta para su aplicación.</span><span class="sxs-lookup"><span data-stu-id="acb35-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="acb35-112">**Debe ver la migración de EF6 a EF Core como un traslado y no una actualización.**</span><span class="sxs-lookup"><span data-stu-id="acb35-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="acb35-113">Para más información, consulte[Traslado de EF6 a EF Core](porting/index.md).</span><span class="sxs-lookup"><span data-stu-id="acb35-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
