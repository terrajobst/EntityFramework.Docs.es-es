---
title: Servicios en tiempo de diseño-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414210"
---
# <a name="design-time-services"></a><span data-ttu-id="52ca0-102">Servicios en tiempo de diseño</span><span class="sxs-lookup"><span data-stu-id="52ca0-102">Design-time services</span></span>

<span data-ttu-id="52ca0-103">Algunos servicios usados por las herramientas solo se usan en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="52ca0-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="52ca0-104">Estos servicios se administran de forma independiente de los servicios en tiempo de ejecución de EF Core para evitar que se implementen con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="52ca0-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="52ca0-105">Para invalidar uno de estos servicios (por ejemplo, el servicio para generar archivos de migración), agregue una implementación de `IDesignTimeServices` al proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="52ca0-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
