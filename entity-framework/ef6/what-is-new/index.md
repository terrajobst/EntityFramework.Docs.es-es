---
title: 'Novedades: EF6'
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
caps.latest.revision: 3
ms.openlocfilehash: 0da2ce778a765037ecacd0726cbb7cda08b5683f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911709"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="675f7-102">Novedades de EF6</span><span class="sxs-lookup"><span data-stu-id="675f7-102">What's New in EF6</span></span>

<span data-ttu-id="675f7-103">Se recomienda encarecidamente usar la versión más reciente de Entity Framework para asegurarse de obtener las últimas características y la mayor estabilidad.</span><span class="sxs-lookup"><span data-stu-id="675f7-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="675f7-104">Pero es posible que deba usar una versión anterior o que quiera experimentar con las nuevas mejoras de la última versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="675f7-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="675f7-105">Para instalar versiones concretas de EF, vea [Get Entity Framework](~/ef6/fundamentals/install.md) (Obtener Entity Framework).</span><span class="sxs-lookup"><span data-stu-id="675f7-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="675f7-106">En esta página se documentan las características incluidas en cada nueva versión.</span><span class="sxs-lookup"><span data-stu-id="675f7-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="675f7-107">Versiones recientes</span><span class="sxs-lookup"><span data-stu-id="675f7-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="675f7-108">Actualización de EF Tools para Visual Studio 2017 15.7</span><span class="sxs-lookup"><span data-stu-id="675f7-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="675f7-109">En mayo de 2018 se ha publicado una versión actualizada de EF6 Tools para Visual Studio 2017 15.7.</span><span class="sxs-lookup"><span data-stu-id="675f7-109">In May 2018, we released an updated version of the EF6 Tools in Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="675f7-110">Incluye mejoras en algunas áreas problemáticas habituales:</span><span class="sxs-lookup"><span data-stu-id="675f7-110">It includes improvements in some common pain areas:</span></span>

- <span data-ttu-id="675f7-111">Revisión de la accesibilidad de la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="675f7-111">An overhaul to the accessibility of the user interface</span></span>
- <span data-ttu-id="675f7-112">Solución alternativa para la regresión de rendimiento de SQL Server sobre la ingeniería inversa [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="675f7-112">Workaround for SQL Server performance regression on reverse engineering [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="675f7-113">Actualización del modelo de compatibilidad de base de datos para modelos más grandes en SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="675f7-113">Update model from database support for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="675f7-114">Esta nueva versión de EF Tools instala el runtime de EF 6.2 al crear un modelo en un proyecto nuevo.</span><span class="sxs-lookup"><span data-stu-id="675f7-114">This new version of EF Tools installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="675f7-115">Con versiones anteriores de Visual Studio, es posible usar el runtime de EF 6.2 (así como cualquier versión anterior de EF) mediante la instalación de la versión correspondiente del paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="675f7-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="675f7-116">Runtime de EF 6.2</span><span class="sxs-lookup"><span data-stu-id="675f7-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="675f7-117">El runtime de EF 6.2 para NuGet se publicó en octubre de 2017.</span><span class="sxs-lookup"><span data-stu-id="675f7-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="675f7-118">Gracias en gran medida a los esfuerzos de la comunidad de colaboradores de código abierto, EF 6.2 incluye bastantes [correcciones de errores](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) y [mejoras de producto](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span><span class="sxs-lookup"><span data-stu-id="675f7-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="675f7-119">Esta es una breve lista de los cambios más importantes que afectan al runtime de EF 6.2:</span><span class="sxs-lookup"><span data-stu-id="675f7-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="675f7-120">Reducción del tiempo de inicio al cargar los primeros modelos de código finalizados desde una caché persistente [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="675f7-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="675f7-121">API fluida para definir índices [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="675f7-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="675f7-122">DbFunctions.Like() para habilitar la escritura de consultas LINQ que se traducen en LIKE en SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="675f7-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="675f7-123">Migrate.exe ahora admite la opción -script [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="675f7-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="675f7-124">EF6 ahora puede trabajar con valores de clave generados por una secuencia en SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="675f7-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="675f7-125">Lista de actualización de errores transitorios de la estrategia de ejecución de SQL Azure [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="675f7-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="675f7-126">Error: al volver a intentar consultas o comandos SQL, se produce un error "SqlParameter ya está incluido en otro elemento SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="675f7-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="675f7-127">Error: la evaluación de DbQuery.ToString() a menudo agota el tiempo de espera en el depurador [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="675f7-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="675f7-128">Próximas versiones</span><span class="sxs-lookup"><span data-stu-id="675f7-128">Future Releases</span></span>

<span data-ttu-id="675f7-129">Para obtener información sobre las próximas versiones de EF6, vea el [plan de desarrollo](roadmap.md).</span><span class="sxs-lookup"><span data-stu-id="675f7-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="675f7-130">Versiones anteriores</span><span class="sxs-lookup"><span data-stu-id="675f7-130">Past Releases</span></span>

<span data-ttu-id="675f7-131">La página [Versiones anteriores](past-releases.md) contiene un archivo de todas las versiones anteriores de EF y de las características principales incluidas en cada versión.</span><span class="sxs-lookup"><span data-stu-id="675f7-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span> 
