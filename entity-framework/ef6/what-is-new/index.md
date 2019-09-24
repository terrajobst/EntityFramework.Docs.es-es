---
title: 'Novedades: EF6'
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149119"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="0acb7-102">Novedades de EF6</span><span class="sxs-lookup"><span data-stu-id="0acb7-102">What's New in EF6</span></span>

<span data-ttu-id="0acb7-103">Se recomienda encarecidamente usar la versión más reciente de Entity Framework para asegurarse de obtener las últimas características y la mayor estabilidad.</span><span class="sxs-lookup"><span data-stu-id="0acb7-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="0acb7-104">Pero es posible que deba usar una versión anterior o que quiera experimentar con las nuevas mejoras de la última versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="0acb7-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="0acb7-105">Para instalar versiones concretas de EF, vea [Get Entity Framework](~/ef6/fundamentals/install.md) (Obtener Entity Framework).</span><span class="sxs-lookup"><span data-stu-id="0acb7-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="0acb7-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="0acb7-106">EF 6.3.0</span></span>

<span data-ttu-id="0acb7-107">El runtime de EF 6.3.0 para NuGet se publicó en septiembre de 2019.</span><span class="sxs-lookup"><span data-stu-id="0acb7-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="0acb7-108">El objetivo principal de esta versión era facilitar la migración de las aplicaciones existentes que usan EF 6 a .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="0acb7-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="0acb7-109">La comunidad también ha contribuido con varias correcciones de errores y mejoras.</span><span class="sxs-lookup"><span data-stu-id="0acb7-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="0acb7-110">Consulte los problemas cerrados en cada [hito](https://github.com/aspnet/EntityFramework6/milestones?state=closed) de la versión 6.3.0 para información detallada.</span><span class="sxs-lookup"><span data-stu-id="0acb7-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="0acb7-111">Estos son algunos de los más importantes:</span><span class="sxs-lookup"><span data-stu-id="0acb7-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="0acb7-112">Compatibilidad con .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="0acb7-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="0acb7-113">El paquete EntityFramework ahora tiene como destino .NET Standard 2.1 además de .NET Framework 4.x.</span><span class="sxs-lookup"><span data-stu-id="0acb7-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="0acb7-114">Se han vuelto a escribir los comandos de migración para ejecutarlos fuera de proceso y que trabajen con proyectos de estilo de SDK.</span><span class="sxs-lookup"><span data-stu-id="0acb7-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="0acb7-115">Compatibilidad con HierarchyId de SQL Server</span><span class="sxs-lookup"><span data-stu-id="0acb7-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="0acb7-116">Compatibilidad mejorada con PackageReference de Roslyn y NuGet</span><span class="sxs-lookup"><span data-stu-id="0acb7-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="0acb7-117">Se agregó ef6.exe para habilitar, agregar, generar scripts y aplicar migraciones desde los ensamblados.</span><span class="sxs-lookup"><span data-stu-id="0acb7-117">Added ef6.exe for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="0acb7-118">Esto reemplaza a migrate.exe.</span><span class="sxs-lookup"><span data-stu-id="0acb7-118">This replaces migrate.exe</span></span>

## <a name="past-releases"></a><span data-ttu-id="0acb7-119">Versiones anteriores</span><span class="sxs-lookup"><span data-stu-id="0acb7-119">Past Releases</span></span>

<span data-ttu-id="0acb7-120">La página [Versiones anteriores](past-releases.md) contiene un archivo de todas las versiones anteriores de EF y de las características principales incluidas en cada versión.</span><span class="sxs-lookup"><span data-stu-id="0acb7-120">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
