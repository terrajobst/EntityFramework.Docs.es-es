---
title: 'EF Core y EF6: la opción adecuada para usted'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: f0a632902384a65ea3cddf752ad262c7a2e89e2e
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2018
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core y EF6: la opción adecuada para usted

La información siguiente le permitirá elegir entre Entity Framework Core y Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Guía para las aplicaciones nuevas

Es recomendable usar EF Core para nuevas aplicaciones si quiere aprovechar todas las funcionalidades de EF Core y si su aplicación no requiere ninguna característica que todavía no se haya implementado en EF Core.

EF6 requiere .NET Framework 4.0 (o una versión posterior) y solo se admite en Windows (es decir, que no se ejecuta en .NET Core y no se admite en otros sistemas operativos), pero sigue siendo una opción viable para las aplicaciones nuevas siempre que pueda cumplir con esas restricciones y si la aplicación no requiere características nuevas de EF Core que no estén disponibles en EF6.

Revise la [comparación de características](features.md) para ver si EF Core puede ser una opción correcta para su aplicación.

## <a name="guidance-for-existing-ef6-applications"></a>Guía para las aplicaciones existentes de EF6

Debido a los cambios fundamentales en EF Core, no se recomienda que intente mover una aplicación EF6 a EF Core, salvo que tenga una razón convincente para hacerlo. Si desea migrar a EF Core para usar características nuevas, asegúrese de conocer las limitaciones que tiene antes de comenzar. Revise la [comparación de características](features.md) para ver si EF Core puede ser una opción correcta para su aplicación.

**Debe ver la migración de EF6 a EF Core como un traslado y no una actualización.** Para más información, consulte[Traslado de EF6 a EF Core](porting/index.md).
