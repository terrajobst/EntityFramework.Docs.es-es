---
title: "EF Core y EF6: la opción adecuada para usted"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="2a32e-102">EF Core y EF6: la opción adecuada para usted</span><span class="sxs-lookup"><span data-stu-id="2a32e-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="2a32e-103">La información siguiente le permitirá elegir entre Entity Framework Core y Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2a32e-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="2a32e-104">Guía para las aplicaciones nuevas</span><span class="sxs-lookup"><span data-stu-id="2a32e-104">Guidance for new applications</span></span>

<span data-ttu-id="2a32e-105">Como EF Core es un producto nuevo y todavía carece de algunas características O/RM fundamentales, EF6 seguirá siendo la opción más adecuada para muchas aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="2a32e-105">Because EF Core is a new product, and still lacks some critical O/RM features, EF6 will still be the most suitable choice for many applications.</span></span>

<span data-ttu-id="2a32e-106">**Estos son los tipos de aplicaciones para las que recomendaríamos usar EF Core:**</span><span class="sxs-lookup"><span data-stu-id="2a32e-106">**These are the types of applications we would recommend using EF Core for:**</span></span>

* <span data-ttu-id="2a32e-107">Aplicaciones nuevas que no necesiten características que todavía no estén implementadas en EF Core.</span><span class="sxs-lookup"><span data-stu-id="2a32e-107">New applications that do not require features that are not yet implemented in EF Core.</span></span> <span data-ttu-id="2a32e-108">Revise la [comparación de características](features.md) para ver si EF Core puede ser una opción correcta para su aplicación.</span><span class="sxs-lookup"><span data-stu-id="2a32e-108">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

* <span data-ttu-id="2a32e-109">Aplicaciones que tienen como destino .NET Core, como Plataforma universal de Windows (UWP) y aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2a32e-109">Applications that target .NET Core, such as Universal Windows Platform (UWP) and ASP.NET Core applications.</span></span> <span data-ttu-id="2a32e-110">Estas aplicaciones no pueden usar EF6 porque requiere .NET Framework (es decir, .NET Framework 4.5).</span><span class="sxs-lookup"><span data-stu-id="2a32e-110">These applications can not use EF6 as it requires the .NET Framework (i.e. .NET Framework 4.5).</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="2a32e-111">Guía para las aplicaciones existentes de EF6</span><span class="sxs-lookup"><span data-stu-id="2a32e-111">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="2a32e-112">Debido a los cambios fundamentales en EF Core, no se recomienda que intente mover una aplicación EF6 a EF Core, salvo que tenga una razón convincente para hacerlo.</span><span class="sxs-lookup"><span data-stu-id="2a32e-112">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="2a32e-113">Si desea migrar a EF Core para usar características nuevas, asegúrese de conocer las limitaciones que tiene antes de comenzar.</span><span class="sxs-lookup"><span data-stu-id="2a32e-113">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="2a32e-114">Revise la [comparación de características](features.md) para ver si EF Core puede ser una opción correcta para su aplicación.</span><span class="sxs-lookup"><span data-stu-id="2a32e-114">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="2a32e-115">**Debe ver la migración de EF6 a EF Core como un traslado y no una actualización.**</span><span class="sxs-lookup"><span data-stu-id="2a32e-115">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="2a32e-116">Para más información, consulte[Traslado de EF6 a EF Core](porting/index.md).</span><span class="sxs-lookup"><span data-stu-id="2a32e-116">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
