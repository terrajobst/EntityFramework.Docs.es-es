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
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core y EF6: la opción adecuada para usted

La información siguiente le permitirá elegir entre Entity Framework Core y Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Guía para las aplicaciones nuevas

Como EF Core es un producto nuevo y todavía carece de algunas características O/RM fundamentales, EF6 seguirá siendo la opción más adecuada para muchas aplicaciones.

**Estos son los tipos de aplicaciones para las que recomendaríamos usar EF Core:**

* Aplicaciones nuevas que no necesiten características que todavía no estén implementadas en EF Core. Revise la [comparación de características](features.md) para ver si EF Core puede ser una opción correcta para su aplicación.

* Aplicaciones que tienen como destino .NET Core, como Plataforma universal de Windows (UWP) y aplicaciones de ASP.NET Core. Estas aplicaciones no pueden usar EF6 porque requiere .NET Framework (es decir, .NET Framework 4.5).

## <a name="guidance-for-existing-ef6-applications"></a>Guía para las aplicaciones existentes de EF6

Debido a los cambios fundamentales en EF Core, no se recomienda que intente mover una aplicación EF6 a EF Core, salvo que tenga una razón convincente para hacerlo. Si desea migrar a EF Core para usar características nuevas, asegúrese de conocer las limitaciones que tiene antes de comenzar. Revise la [comparación de características](features.md) para ver si EF Core puede ser una opción correcta para su aplicación.

**Debe ver la migración de EF6 a EF Core como un traslado y no una actualización.** Para más información, consulte[Traslado de EF6 a EF Core](porting/index.md).
