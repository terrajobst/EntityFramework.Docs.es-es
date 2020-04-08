---
title: Plan para Entity Framework Core 5.0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136218"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="40958-102">Plan para Entity Framework Core 5.0</span><span class="sxs-lookup"><span data-stu-id="40958-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="40958-103">Como se describe en el [proceso de planeamiento](../release-planning.md), se ha recopilado la información de las partes interesadas en un plan provisional para la versión EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="40958-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="40958-104">Este plan sigue siendo un trabajo en curso.</span><span class="sxs-lookup"><span data-stu-id="40958-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="40958-105">Nada de esto es un compromiso.</span><span class="sxs-lookup"><span data-stu-id="40958-105">Nothing here is a commitment.</span></span> <span data-ttu-id="40958-106">Este plan es un punto de partida que evolucionará a medida que se obtenga más información.</span><span class="sxs-lookup"><span data-stu-id="40958-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="40958-107">Es posible que algunos aspectos no planeados en la actualidad se incorporen a la versión 5.0.</span><span class="sxs-lookup"><span data-stu-id="40958-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="40958-108">Es posible que algunos aspectos planeados en la actualidad se eliminen de la versión 5.0.</span><span class="sxs-lookup"><span data-stu-id="40958-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="40958-109">Número de versión y fecha de lanzamiento.</span><span class="sxs-lookup"><span data-stu-id="40958-109">Version number and release date.</span></span>

<span data-ttu-id="40958-110">En la actualidad, el lanzamiento de EF Core 5.0 está programado al [mismo tiempo que .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="40958-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="40958-111">Se ha elegido la versión "5.0" para la alineación con .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="40958-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="40958-112">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="40958-112">Supported platforms</span></span>

<span data-ttu-id="40958-113">Está previsto que EF Core 5.0 se ejecute en cualquier plataforma .NET 5.0 en función de la [ convergencia de estas plataformas a .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="40958-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="40958-114">Lo que esto significa en términos de .NET Standard y el TFM real que se usa todavía está por determinar.</span><span class="sxs-lookup"><span data-stu-id="40958-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="40958-115">EF Core 5.0 no se ejecutará en .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="40958-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="40958-116">Cambios importantes</span><span class="sxs-lookup"><span data-stu-id="40958-116">Breaking changes</span></span>

<span data-ttu-id="40958-117">EF Core 5.0 contendrá algunos cambios importantes, pero serán mucho menos graves que en el caso de EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="40958-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="40958-118">El objetivo es permitir que la gran mayoría de las aplicaciones se actualicen sin interrupciones.</span><span class="sxs-lookup"><span data-stu-id="40958-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="40958-119">Se espera que haya algunos cambios importantes para los proveedores de bases de datos, especialmente relacionados con la compatibilidad con TPT.</span><span class="sxs-lookup"><span data-stu-id="40958-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="40958-120">Pero se espera que el trabajo para actualizar un proveedor para 5.0 sea menor que el necesario para 3.0.</span><span class="sxs-lookup"><span data-stu-id="40958-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="40958-121">Temas</span><span class="sxs-lookup"><span data-stu-id="40958-121">Themes</span></span>

<span data-ttu-id="40958-122">Hemos extraído algunas áreas o temas importantes que formarán la base de las grandes inversiones en EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="40958-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="40958-123">Propiedades de navegación de varios a varios (también denominado "omitir navegaciones")</span><span class="sxs-lookup"><span data-stu-id="40958-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="40958-124">Jefes de desarrollo: @smitpatel y @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="40958-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="40958-125">Seguimiento realizado por [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="40958-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="40958-126">Talla de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="40958-126">T-shirt size: L</span></span>

<span data-ttu-id="40958-127">Estado: En curso</span><span class="sxs-lookup"><span data-stu-id="40958-127">Status: In-progress</span></span>

<span data-ttu-id="40958-128">Varios a varios es la [característica más solicitada](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (407 votos aproximadamente) en el trabajo pendiente de GitHub.</span><span class="sxs-lookup"><span data-stu-id="40958-128">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="40958-129">Se realiza un seguimiento de la compatibilidad con las relaciones de varios a varios en su totalidad como [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span><span class="sxs-lookup"><span data-stu-id="40958-129">Support for many-to-many relationships in their entirety is tracked as [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span></span> <span data-ttu-id="40958-130">Puede dividirse en tres áreas principales:</span><span class="sxs-lookup"><span data-stu-id="40958-130">This can be broken down into three major areas:</span></span>

* <span data-ttu-id="40958-131">Omitir propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="40958-131">Skip navigation properties.</span></span> <span data-ttu-id="40958-132">Permiten usar el modelo para las consultas, etc., sin hacer referencia a la entidad de la tabla de combinación subyacente.</span><span class="sxs-lookup"><span data-stu-id="40958-132">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span> <span data-ttu-id="40958-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span><span class="sxs-lookup"><span data-stu-id="40958-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span></span>
* <span data-ttu-id="40958-134">Tipos de entidad de contenedor de propiedades.</span><span class="sxs-lookup"><span data-stu-id="40958-134">Property-bag entity types.</span></span> <span data-ttu-id="40958-135">Permiten usar un tipo CLR estándar (por ejemplo, `Dictionary`) para las instancias de entidad, de modo que no se necesite un tipo CLR explícito para cada tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="40958-135">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="40958-136">(Stretch para la versión 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)).</span><span class="sxs-lookup"><span data-stu-id="40958-136">(Stretch for 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span></span>
* <span data-ttu-id="40958-137">Facilitar la configuración de relaciones varios a varios.</span><span class="sxs-lookup"><span data-stu-id="40958-137">Sugar for easy configuration of many-to-many relationships.</span></span> <span data-ttu-id="40958-138">(Stretch para la versión 5.0).</span><span class="sxs-lookup"><span data-stu-id="40958-138">(Stretch for 5.0.)</span></span>

<span data-ttu-id="40958-139">Creemos que el principal obstáculo para los que quieren que se admitan las relaciones varios a varios es la imposibilidad de usar relaciones "naturales", sin hacer referencia a la tabla de combinación, en la lógica de negocios, como las consultas.</span><span class="sxs-lookup"><span data-stu-id="40958-139">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="40958-140">Es posible que el tipo de entidad de tabla de combinación siga existiendo, pero no debería interferir con la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="40958-140">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="40958-141">Por este motivo hemos optado por abordar la omisión de propiedades de navegación en la versión 5.0.</span><span class="sxs-lookup"><span data-stu-id="40958-141">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="40958-142">En este momento, los demás elementos de las relaciones varios a varios se han convertido en un objetivo de extensión para EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="40958-142">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="40958-143">Esto significa que actualmente no se incluyen en el plan para la versión 5.0, pero si todo va bien esperamos incorporarlos.</span><span class="sxs-lookup"><span data-stu-id="40958-143">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="40958-144">Asignación de herencia de tabla por tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="40958-144">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="40958-145">Jefe de desarrollo: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="40958-145">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="40958-146">Seguimiento realizado por [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="40958-146">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="40958-147">Talla de camiseta: XL</span><span class="sxs-lookup"><span data-stu-id="40958-147">T-shirt size: XL</span></span>

<span data-ttu-id="40958-148">Estado: En curso</span><span class="sxs-lookup"><span data-stu-id="40958-148">Status: In-progress</span></span>

<span data-ttu-id="40958-149">TPT se va a incluir porque se trata de una característica muy solicitada (aproximadamente 254 votos; en tercera posición) y porque requiere algunos cambios de bajo nivel que consideramos adecuados para la naturaleza fundamental del plan general de .NET 5.</span><span class="sxs-lookup"><span data-stu-id="40958-149">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="40958-150">Esperamos que esto genere cambios importantes para los proveedores de bases de datos, aunque deberían ser mucho menos graves que los necesarios para la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="40958-150">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="40958-151">Inclusión filtrada</span><span class="sxs-lookup"><span data-stu-id="40958-151">Filtered Include</span></span>

<span data-ttu-id="40958-152">Jefe de desarrollo: @maumar</span><span class="sxs-lookup"><span data-stu-id="40958-152">Lead developer: @maumar</span></span>

<span data-ttu-id="40958-153">Seguimiento realizado por [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="40958-153">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="40958-154">Talla de camiseta: M</span><span class="sxs-lookup"><span data-stu-id="40958-154">T-shirt size: M</span></span>

<span data-ttu-id="40958-155">Estado: En curso</span><span class="sxs-lookup"><span data-stu-id="40958-155">Status: In-progress</span></span>

<span data-ttu-id="40958-156">La inclusión filtrada es una característica muy solicitada (aproximadamente 317 votos; en segunda posición) que no requiere demasiado trabajo y que creemos que desbloqueará o facilitará escenarios que actualmente requieren filtros de nivel de modelo o consultas más complejas.</span><span class="sxs-lookup"><span data-stu-id="40958-156">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="40958-157">Racionalización de ToTable, ToQuery, ToView, FromSql, etc.</span><span class="sxs-lookup"><span data-stu-id="40958-157">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="40958-158">Jefes de desarrollo: @maumar y @smitpatel</span><span class="sxs-lookup"><span data-stu-id="40958-158">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="40958-159">Seguimiento realizado por [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="40958-159">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="40958-160">Talla de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="40958-160">T-shirt size: L</span></span>

<span data-ttu-id="40958-161">Estado: En curso</span><span class="sxs-lookup"><span data-stu-id="40958-161">Status: In-progress</span></span>

<span data-ttu-id="40958-162">Se han realizado avances en versiones anteriores hacia la compatibilidad con SQL sin procesar, tipos sin clave y áreas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="40958-162">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="40958-163">Pero hay brechas e incoherencias en el funcionamiento en conjunto de todos los elementos.</span><span class="sxs-lookup"><span data-stu-id="40958-163">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="40958-164">El objetivo para la versión 5.0 es corregirlos y crear una buena experiencia para definir, migrar y usar otros tipos de entidades y sus consultas y artefactos de base de datos asociados.</span><span class="sxs-lookup"><span data-stu-id="40958-164">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="40958-165">Esto también puede implicar actualizaciones de la API de consulta compilada.</span><span class="sxs-lookup"><span data-stu-id="40958-165">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="40958-166">Tenga en cuenta que este elemento puede dar lugar a algunos cambios importantes en el nivel de la aplicación, ya que algunas de las funcionalidades actuales son demasiado permisivas, lo que puede conducir rápidamente a que los usuarios cometan errores.</span><span class="sxs-lookup"><span data-stu-id="40958-166">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="40958-167">Lo más probable es que parte de esta funcionalidad se bloquee y se proporcionen instrucciones sobre lo que se debe hacer en su lugar.</span><span class="sxs-lookup"><span data-stu-id="40958-167">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="40958-168">Mejoras generales de consultas</span><span class="sxs-lookup"><span data-stu-id="40958-168">General query enhancements</span></span>

<span data-ttu-id="40958-169">Jefes de desarrollo: @smitpatel y @maumar</span><span class="sxs-lookup"><span data-stu-id="40958-169">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="40958-170">Seguimiento por [problemas etiquetados con `area-query` en el hito 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="40958-170">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="40958-171">Talla de camiseta: XL</span><span class="sxs-lookup"><span data-stu-id="40958-171">T-shirt size: XL</span></span>

<span data-ttu-id="40958-172">Estado: En curso</span><span class="sxs-lookup"><span data-stu-id="40958-172">Status: In-progress</span></span>

<span data-ttu-id="40958-173">El código de traducción de consultas se ha reescrito de forma exhaustiva para EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="40958-173">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="40958-174">Por este motivo, el código de consulta tiene un estado mucho más robusto.</span><span class="sxs-lookup"><span data-stu-id="40958-174">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="40958-175">Para la versión 5.0, no se planean cambios importantes en las consultas más allá de los necesarios para admitir TPT y la omisión de propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="40958-175">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="40958-176">Pero se necesita un trabajo importante para corregir las deudas técnicas generadas tras la revisión de la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="40958-176">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="40958-177">También tenemos previsto corregir muchos errores e implementar pequeñas mejoras para mejorar aún más la experiencia general de las consultas.</span><span class="sxs-lookup"><span data-stu-id="40958-177">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="40958-178">Migraciones y experiencia de implementación</span><span class="sxs-lookup"><span data-stu-id="40958-178">Migrations and deployment experience</span></span>

<span data-ttu-id="40958-179">Jefes de desarrollo: @bricelam</span><span class="sxs-lookup"><span data-stu-id="40958-179">Lead developers: @bricelam</span></span>

<span data-ttu-id="40958-180">Seguimiento realizado por [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="40958-180">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="40958-181">Talla de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="40958-181">T-shirt size: L</span></span>

<span data-ttu-id="40958-182">Estado: En curso</span><span class="sxs-lookup"><span data-stu-id="40958-182">Status: In-progress</span></span>

<span data-ttu-id="40958-183">En la actualidad, muchos desarrolladores migran sus bases de datos en el momento de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="40958-183">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="40958-184">Esto es fácil, pero no se recomienda porque:</span><span class="sxs-lookup"><span data-stu-id="40958-184">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="40958-185">Es posible que varios subprocesos, procesos o servidores intenten migrar la base de datos simultáneamente</span><span class="sxs-lookup"><span data-stu-id="40958-185">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="40958-186">Es posible que las aplicaciones intenten acceder a un estado incoherente mientras se produce esta operación</span><span class="sxs-lookup"><span data-stu-id="40958-186">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="40958-187">Normalmente, no se deben conceder los permisos de base de datos para modificar el esquema para la ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="40958-187">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="40958-188">Es difícil revertir a un estado limpio si se produce algún error</span><span class="sxs-lookup"><span data-stu-id="40958-188">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="40958-189">Queremos ofrecer una mejor experiencia que permita migrar la base de datos de forma sencilla en el momento de la implementación.</span><span class="sxs-lookup"><span data-stu-id="40958-189">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="40958-190">Esto debería:</span><span class="sxs-lookup"><span data-stu-id="40958-190">This should:</span></span>

* <span data-ttu-id="40958-191">Funcionar en Linux, Mac y Windows</span><span class="sxs-lookup"><span data-stu-id="40958-191">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="40958-192">Ser una experiencia positiva en la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="40958-192">Be a good experience on the command line</span></span>
* <span data-ttu-id="40958-193">Admitir escenarios con contenedores</span><span class="sxs-lookup"><span data-stu-id="40958-193">Support scenarios with containers</span></span>
* <span data-ttu-id="40958-194">Funcionar con flujos y herramientas de implementación del mundo real que se usan habitualmente</span><span class="sxs-lookup"><span data-stu-id="40958-194">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="40958-195">Integrarse al menos en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40958-195">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="40958-196">Lo más probable es que el resultado sea un gran número de pequeñas mejoras en EF Core (por ejemplo, mejores migraciones en SQLite), junto con instrucciones y colaboraciones a largo plazo con otros equipos para mejorar las experiencias de un extremo a otro que van más allá de EF exclusivamente.</span><span class="sxs-lookup"><span data-stu-id="40958-196">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="40958-197">Experiencia de las plataformas de EF Core</span><span class="sxs-lookup"><span data-stu-id="40958-197">EF Core platforms experience</span></span> 

<span data-ttu-id="40958-198">Jefes de desarrollo: @roji y @bricelam</span><span class="sxs-lookup"><span data-stu-id="40958-198">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="40958-199">Seguimiento realizado por [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="40958-199">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="40958-200">Talla de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="40958-200">T-shirt size: L</span></span>

<span data-ttu-id="40958-201">Estado: Sin iniciar</span><span class="sxs-lookup"><span data-stu-id="40958-201">Status: Not started</span></span>

<span data-ttu-id="40958-202">Disponemos de instrucciones de calidad para usar EF Core en aplicaciones web tradicionales similares a MVC.</span><span class="sxs-lookup"><span data-stu-id="40958-202">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="40958-203">Las instrucciones para otras plataformas y modelos de aplicación no existen o no están actualizadas.</span><span class="sxs-lookup"><span data-stu-id="40958-203">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="40958-204">Para EF Core 5.0, el objetivo es investigar, mejorar y documentar la experiencia de uso de EF Core con:</span><span class="sxs-lookup"><span data-stu-id="40958-204">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="40958-205">Blazor</span><span class="sxs-lookup"><span data-stu-id="40958-205">Blazor</span></span>
* <span data-ttu-id="40958-206">Xamarin, incluido el uso del artículo de AOT o vinculador</span><span class="sxs-lookup"><span data-stu-id="40958-206">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="40958-207">WinForms, WPF, WinUI y posiblemente otras interfaces de usuario</span><span class="sxs-lookup"><span data-stu-id="40958-207">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="40958-208">frameworks</span><span class="sxs-lookup"><span data-stu-id="40958-208">frameworks</span></span>

<span data-ttu-id="40958-209">Lo más probable es que el resultado sea un gran número de pequeñas mejoras en EF Core, junto con instrucciones y colaboraciones a largo plazo con otros equipos para mejorar las experiencias de un extremo a otro que van más allá de EF exclusivamente.</span><span class="sxs-lookup"><span data-stu-id="40958-209">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="40958-210">Las áreas concretas que tenemos previsto examinar son las siguientes:</span><span class="sxs-lookup"><span data-stu-id="40958-210">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="40958-211">Implementación, incluida la experiencia en el uso de herramientas de EF como para migraciones</span><span class="sxs-lookup"><span data-stu-id="40958-211">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="40958-212">Modelos de aplicación, como Xamarin y Blazor, y probablemente otros</span><span class="sxs-lookup"><span data-stu-id="40958-212">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="40958-213">Experiencias de SQLite, incluida la experiencia espacial y las recompilaciones de tabla</span><span class="sxs-lookup"><span data-stu-id="40958-213">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="40958-214">Experiencias de AOT y vinculación</span><span class="sxs-lookup"><span data-stu-id="40958-214">AOT and linking experiences</span></span>
* <span data-ttu-id="40958-215">Integración de diagnósticos, incluidos los contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="40958-215">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="40958-216">Rendimiento</span><span class="sxs-lookup"><span data-stu-id="40958-216">Performance</span></span>

<span data-ttu-id="40958-217">Jefe de desarrollo: @roji</span><span class="sxs-lookup"><span data-stu-id="40958-217">Lead developer: @roji</span></span>

<span data-ttu-id="40958-218">Seguimiento por [problemas etiquetados con `area-perf` en el hito 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="40958-218">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="40958-219">Talla de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="40958-219">T-shirt size: L</span></span>

<span data-ttu-id="40958-220">Estado: En curso</span><span class="sxs-lookup"><span data-stu-id="40958-220">Status: In-progress</span></span>

<span data-ttu-id="40958-221">Para EF Core, el plan es mejorar nuestro conjunto de pruebas comparativas de rendimiento y realizar mejoras de rendimiento dirigidas al tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="40958-221">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="40958-222">Además, tenemos previsto completar la nueva API de procesamiento por lotes de ADO.NET, que se ha creado como prototipo durante el ciclo de versiones de 3.0.</span><span class="sxs-lookup"><span data-stu-id="40958-222">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="40958-223">En el nivel de ADO.NET también se planean mejoras de rendimiento adicionales para el proveedor Npgsql.</span><span class="sxs-lookup"><span data-stu-id="40958-223">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="40958-224">Como parte de este trabajo, también está previsto agregar contadores de rendimiento de ADO.NET y EF Core, y otros diagnósticos según corresponda.</span><span class="sxs-lookup"><span data-stu-id="40958-224">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="40958-225">Documentación de arquitectura y colaboradores</span><span class="sxs-lookup"><span data-stu-id="40958-225">Architectural/contributor documentation</span></span>

<span data-ttu-id="40958-226">Jefe de documentación: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="40958-226">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="40958-227">Seguimiento realizado por [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="40958-227">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="40958-228">Talla de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="40958-228">T-shirt size: L</span></span>

<span data-ttu-id="40958-229">Estado: En curso</span><span class="sxs-lookup"><span data-stu-id="40958-229">Status: In-progress</span></span>

<span data-ttu-id="40958-230">La idea es facilitar la comprensión de lo que sucede dentro de EF Core.</span><span class="sxs-lookup"><span data-stu-id="40958-230">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="40958-231">Esto puede ser útil para cualquiera que utilice EF Core, pero la motivación principal es facilitarlo para los usuarios externos:</span><span class="sxs-lookup"><span data-stu-id="40958-231">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="40958-232">Contribuir al código de EF Core</span><span class="sxs-lookup"><span data-stu-id="40958-232">Contribute to the EF Core code</span></span>
* <span data-ttu-id="40958-233">Crear proveedores de bases de datos</span><span class="sxs-lookup"><span data-stu-id="40958-233">Create database providers</span></span>
* <span data-ttu-id="40958-234">Compilar otras extensiones</span><span class="sxs-lookup"><span data-stu-id="40958-234">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="40958-235">Documentación de Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="40958-235">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="40958-236">Jefe de documentación: @bricelam</span><span class="sxs-lookup"><span data-stu-id="40958-236">Lead documenter: @bricelam</span></span>

<span data-ttu-id="40958-237">Seguimiento realizado por [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="40958-237">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="40958-238">Talla de camiseta: M</span><span class="sxs-lookup"><span data-stu-id="40958-238">T-shirt size: M</span></span>

<span data-ttu-id="40958-239">Estado: Completado.</span><span class="sxs-lookup"><span data-stu-id="40958-239">Status: Completed.</span></span> <span data-ttu-id="40958-240">La nueva documentación está [activa en el sitio de documentación de Microsoft](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="40958-240">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="40958-241">El equipo de EF también posee el proveedor de ADO.NET Microsoft.Data.Sqlite.</span><span class="sxs-lookup"><span data-stu-id="40958-241">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="40958-242">Tenemos previsto documentar completamente este proveedor como parte de la versión 5.0.</span><span class="sxs-lookup"><span data-stu-id="40958-242">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="40958-243">Documentación general</span><span class="sxs-lookup"><span data-stu-id="40958-243">General documentation</span></span>

<span data-ttu-id="40958-244">Jefe de documentación: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="40958-244">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="40958-245">Seguimiento mediante [problemas en el repositorio de documentación del hito 5.0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="40958-245">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="40958-246">Talla de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="40958-246">T-shirt size: L</span></span>

<span data-ttu-id="40958-247">Estado: En curso</span><span class="sxs-lookup"><span data-stu-id="40958-247">Status: In-progress</span></span>

<span data-ttu-id="40958-248">Ya se ha iniciado el proceso de actualización de la documentación de las versiones 3.0 y 3.1.</span><span class="sxs-lookup"><span data-stu-id="40958-248">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="40958-249">También se está trabajando en:</span><span class="sxs-lookup"><span data-stu-id="40958-249">We are also working on:</span></span>
  * <span data-ttu-id="40958-250">Una revisión de la documentación de introducción para que sea más fácil de seguir</span><span class="sxs-lookup"><span data-stu-id="40958-250">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="40958-251">La reorganización de la documentación para facilitar la búsqueda y la adición de referencias cruzadas</span><span class="sxs-lookup"><span data-stu-id="40958-251">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="40958-252">La incorporación de más detalles y aclaraciones a la documentación existente</span><span class="sxs-lookup"><span data-stu-id="40958-252">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="40958-253">La actualización de los ejemplos y la incorporación de otros nuevos</span><span class="sxs-lookup"><span data-stu-id="40958-253">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="40958-254">Corrección de errores</span><span class="sxs-lookup"><span data-stu-id="40958-254">Fixing bugs</span></span>

<span data-ttu-id="40958-255">Seguimiento por [problemas etiquetados con `type-bug` en el hito 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="40958-255">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="40958-256">Desarrolladores: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="40958-256">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="40958-257">Talla de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="40958-257">T-shirt size: L</span></span>

<span data-ttu-id="40958-258">Estado: En curso</span><span class="sxs-lookup"><span data-stu-id="40958-258">Status: In-progress</span></span>

<span data-ttu-id="40958-259">En el momento de escribir este documento, se han evaluado 135 errores para corregirlos en la versión 5.0 (ya se han corregido 62), pero hay una superposición significativa con la sección anterior _Mejoras generales de consultas_.</span><span class="sxs-lookup"><span data-stu-id="40958-259">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="40958-260">La velocidad de entrada (problemas que acaban como trabajo en un hito) fue de aproximadamente 23 problemas al mes en el transcurso de la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="40958-260">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="40958-261">No todos se tendrán que corregir en la versión 5.0.</span><span class="sxs-lookup"><span data-stu-id="40958-261">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="40958-262">Como estimación aproximada, tenemos previsto corregir unos 150 problemas adicionales para la versión 5.0.</span><span class="sxs-lookup"><span data-stu-id="40958-262">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="40958-263">Mejoras menores</span><span class="sxs-lookup"><span data-stu-id="40958-263">Small enhancements</span></span>

<span data-ttu-id="40958-264">Seguimiento por [problemas etiquetados con `type-enhancement` en el hito 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="40958-264">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="40958-265">Desarrolladores: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="40958-265">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="40958-266">Talla de camiseta: L</span><span class="sxs-lookup"><span data-stu-id="40958-266">T-shirt size: L</span></span>

<span data-ttu-id="40958-267">Estado: En curso</span><span class="sxs-lookup"><span data-stu-id="40958-267">Status: In-progress</span></span>

<span data-ttu-id="40958-268">Además de las características más importantes descritas antes, también hay muchas mejoras más pequeñas programadas para la versión 5.0 a fin de corregir los "elementos excluidos".</span><span class="sxs-lookup"><span data-stu-id="40958-268">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="40958-269">Tenga en cuenta que muchas de estas mejoras también se incluyen en los temas más generales descritos antes.</span><span class="sxs-lookup"><span data-stu-id="40958-269">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="40958-270">Nivel inferior</span><span class="sxs-lookup"><span data-stu-id="40958-270">Below-the-line</span></span>

<span data-ttu-id="40958-271">Seguimiento por [problemas etiquetados con `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="40958-271">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="40958-272">Se trata de correcciones de errores y mejoras **no** programadas actualmente para la versión 5.0, pero que se considerarán objetivos de extensión en función del progreso realizado en el trabajo anterior.</span><span class="sxs-lookup"><span data-stu-id="40958-272">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="40958-273">Además, durante la planeación siempre se tienen en cuenta los [problemas más votados](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="40958-273">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="40958-274">Excluir cualquiera de estos problemas de una versión siempre es complicado, pero necesitamos un plan realista para los recursos que tenemos.</span><span class="sxs-lookup"><span data-stu-id="40958-274">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="40958-275">Comentarios</span><span class="sxs-lookup"><span data-stu-id="40958-275">Feedback</span></span>

<span data-ttu-id="40958-276">Sus comentarios sobre la planeación son importantes.</span><span class="sxs-lookup"><span data-stu-id="40958-276">Your feedback on planning is important.</span></span> <span data-ttu-id="40958-277">La mejor manera de indicar la importancia de un problema es votar (pulgar) por ese problema en GitHub.</span><span class="sxs-lookup"><span data-stu-id="40958-277">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="40958-278">Estos datos se introducirán después en el [proceso de planeación](../release-planning.md) de la próxima versión.</span><span class="sxs-lookup"><span data-stu-id="40958-278">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
