---
title: 'Cambios importantes en EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 884cc6611b986fb213d99d3d2fc69d7bebe34aa2
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565309"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="602c5-102">Cambios importantes incluidos en EF Core 3.0 (actualmente en versión preliminar)</span><span class="sxs-lookup"><span data-stu-id="602c5-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="602c5-103">Tenga en cuenta que los conjuntos de características y las programaciones de las versiones futuras siempre están sujetos a cambios y, aunque intentamos mantener esta página actualizada, es posible que no refleje nuestros planes más recientes en todo momento.</span><span class="sxs-lookup"><span data-stu-id="602c5-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="602c5-104">Es posible que los siguientes cambios de API y comportamiento interrumpan las aplicaciones desarrolladas para EF Core 2.2. x cuando se actualicen a la versión 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="602c5-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="602c5-105">Los cambios que esperamos que solo afecten a proveedores de base de datos se documentan en [Cambios para proveedores](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="602c5-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="602c5-106">Aquí no se documentan los cambios en las características nuevas presentadas de una versión preliminar 3.0 a otra.</span><span class="sxs-lookup"><span data-stu-id="602c5-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="602c5-107">Resumen</span><span class="sxs-lookup"><span data-stu-id="602c5-107">Summary</span></span>

| <span data-ttu-id="602c5-108">**Cambio importante**</span><span class="sxs-lookup"><span data-stu-id="602c5-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="602c5-109">**Impacto**</span><span class="sxs-lookup"><span data-stu-id="602c5-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="602c5-110">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="602c5-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="602c5-111">Alto</span><span class="sxs-lookup"><span data-stu-id="602c5-111">High</span></span>       |
| [<span data-ttu-id="602c5-112">EF Core 3.0 tiene como destino .NET Standard 2.1, y no .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="602c5-112">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="602c5-113">Alto</span><span class="sxs-lookup"><span data-stu-id="602c5-113">High</span></span>      |
| [<span data-ttu-id="602c5-114">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="602c5-114">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="602c5-115">Alto</span><span class="sxs-lookup"><span data-stu-id="602c5-115">High</span></span>      |
| [<span data-ttu-id="602c5-116">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="602c5-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="602c5-117">Alto</span><span class="sxs-lookup"><span data-stu-id="602c5-117">High</span></span>      |
| [<span data-ttu-id="602c5-118">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="602c5-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="602c5-119">Alto</span><span class="sxs-lookup"><span data-stu-id="602c5-119">High</span></span>      |
| [<span data-ttu-id="602c5-120">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="602c5-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="602c5-121">Medium</span><span class="sxs-lookup"><span data-stu-id="602c5-121">Medium</span></span>      |
| [<span data-ttu-id="602c5-122">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="602c5-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="602c5-123">Medium</span><span class="sxs-lookup"><span data-stu-id="602c5-123">Medium</span></span>      |
| [<span data-ttu-id="602c5-124">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="602c5-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="602c5-125">Medium</span><span class="sxs-lookup"><span data-stu-id="602c5-125">Medium</span></span>      |
| [<span data-ttu-id="602c5-126">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="602c5-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="602c5-127">Medium</span><span class="sxs-lookup"><span data-stu-id="602c5-127">Medium</span></span>      |
| [<span data-ttu-id="602c5-128">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="602c5-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="602c5-129">Medium</span><span class="sxs-lookup"><span data-stu-id="602c5-129">Medium</span></span>      |
| [<span data-ttu-id="602c5-130">Las consultas sin seguimiento ya no realizan la resolución de la identidad</span><span class="sxs-lookup"><span data-stu-id="602c5-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="602c5-131">Medium</span><span class="sxs-lookup"><span data-stu-id="602c5-131">Medium</span></span>      |
| [<span data-ttu-id="602c5-132">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="602c5-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="602c5-133">Medium</span><span class="sxs-lookup"><span data-stu-id="602c5-133">Medium</span></span>      |
| [<span data-ttu-id="602c5-134">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="602c5-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="602c5-135">Medium</span><span class="sxs-lookup"><span data-stu-id="602c5-135">Medium</span></span>      |
| [<span data-ttu-id="602c5-136">Se ha quitado el elemento UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="602c5-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="602c5-137">Medium</span><span class="sxs-lookup"><span data-stu-id="602c5-137">Medium</span></span>      |
| [<span data-ttu-id="602c5-138">Solo se pueden especificar métodos de FromSql en raíces de consulta</span><span class="sxs-lookup"><span data-stu-id="602c5-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="602c5-139">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-139">Low</span></span>      |
| [<span data-ttu-id="602c5-140">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="602c5-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="602c5-141">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-141">Low</span></span>      |
| [<span data-ttu-id="602c5-142">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="602c5-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="602c5-143">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-143">Low</span></span>      |
| [<span data-ttu-id="602c5-144">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="602c5-144">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="602c5-145">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-145">Low</span></span>      |
| [<span data-ttu-id="602c5-146">Las entidades dependientes que comparten la tabla con la entidad de seguridad son ahora opcionales</span><span class="sxs-lookup"><span data-stu-id="602c5-146">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="602c5-147">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-147">Low</span></span>      |
| [<span data-ttu-id="602c5-148">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="602c5-148">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="602c5-149">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-149">Low</span></span>      |
| [<span data-ttu-id="602c5-150">Las propiedades heredadas de tipos sin asignar se asignan ahora a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="602c5-150">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="602c5-151">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-151">Low</span></span>      |
| [<span data-ttu-id="602c5-152">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="602c5-152">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="602c5-153">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-153">Low</span></span>      |
| [<span data-ttu-id="602c5-154">La conexión de base de datos ahora se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="602c5-154">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="602c5-155">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-155">Low</span></span>      |
| [<span data-ttu-id="602c5-156">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="602c5-156">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="602c5-157">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-157">Low</span></span>      |
| [<span data-ttu-id="602c5-158">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="602c5-158">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="602c5-159">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-159">Low</span></span>      |
| [<span data-ttu-id="602c5-160">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="602c5-160">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="602c5-161">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-161">Low</span></span>      |
| [<span data-ttu-id="602c5-162">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="602c5-162">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="602c5-163">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-163">Low</span></span>      |
| [<span data-ttu-id="602c5-164">DbContext.Entry realiza ahora una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="602c5-164">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="602c5-165">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-165">Low</span></span>      |
| [<span data-ttu-id="602c5-166">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="602c5-166">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="602c5-167">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-167">Low</span></span>      |
| [<span data-ttu-id="602c5-168">ILoggerFactory es ahora un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="602c5-168">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="602c5-169">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-169">Low</span></span>      |
| [<span data-ttu-id="602c5-170">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="602c5-170">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="602c5-171">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-171">Low</span></span>      |
| [<span data-ttu-id="602c5-172">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="602c5-172">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="602c5-173">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-173">Low</span></span>      |
| [<span data-ttu-id="602c5-174">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="602c5-174">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="602c5-175">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-175">Low</span></span>      |
| [<span data-ttu-id="602c5-176">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="602c5-176">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="602c5-177">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-177">Low</span></span>      |
| [<span data-ttu-id="602c5-178">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="602c5-178">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="602c5-179">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-179">Low</span></span>      |
| [<span data-ttu-id="602c5-180">ToTable en un tipo derivado produce una excepción</span><span class="sxs-lookup"><span data-stu-id="602c5-180">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="602c5-181">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-181">Low</span></span>      |
| [<span data-ttu-id="602c5-182">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="602c5-182">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="602c5-183">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-183">Low</span></span>      |
| [<span data-ttu-id="602c5-184">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="602c5-184">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="602c5-185">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-185">Low</span></span>      |
| [<span data-ttu-id="602c5-186">Los valores GUID se almacenan ahora como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="602c5-186">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="602c5-187">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-187">Low</span></span>      |
| [<span data-ttu-id="602c5-188">Los valores char se almacenan ahora como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="602c5-188">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="602c5-189">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-189">Low</span></span>      |
| [<span data-ttu-id="602c5-190">Los id. de migración ahora se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="602c5-190">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="602c5-191">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-191">Low</span></span>      |
| [<span data-ttu-id="602c5-192">La información o los metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="602c5-192">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="602c5-193">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-193">Low</span></span>      |
| [<span data-ttu-id="602c5-194">LogQueryPossibleExceptionWithAggregateOperator ha cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="602c5-194">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="602c5-195">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-195">Low</span></span>      |
| [<span data-ttu-id="602c5-196">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="602c5-196">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="602c5-197">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-197">Low</span></span>      |
| [<span data-ttu-id="602c5-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="602c5-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="602c5-199">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-199">Low</span></span>      |
| [<span data-ttu-id="602c5-200">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="602c5-200">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="602c5-201">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-201">Low</span></span>      |
| [<span data-ttu-id="602c5-202">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="602c5-202">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="602c5-203">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-203">Low</span></span>      |
| [<span data-ttu-id="602c5-204">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="602c5-204">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="602c5-205">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-205">Low</span></span>      |
| [<span data-ttu-id="602c5-206">Se deben configurar varias relaciones de referencia automática ambiguas</span><span class="sxs-lookup"><span data-stu-id="602c5-206">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="602c5-207">Bajo</span><span class="sxs-lookup"><span data-stu-id="602c5-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="602c5-208">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="602c5-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="602c5-209">[Problema de seguimiento n.° 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Consulte también el problema n.° 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="602c5-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="602c5-210">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-210">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-211">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-211">**Old behavior**</span></span>

<span data-ttu-id="602c5-212">Antes de 3.0, cuando en EF Core no se podía convertir una expresión que formaba parte de una consulta SQL o un parámetro, la expresión se evaluaba de forma automática en el cliente.</span><span class="sxs-lookup"><span data-stu-id="602c5-212">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="602c5-213">De forma predeterminada, la evaluación de cliente de expresiones potencialmente costosas solo desencadenaba una advertencia.</span><span class="sxs-lookup"><span data-stu-id="602c5-213">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="602c5-214">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-214">**New behavior**</span></span>

<span data-ttu-id="602c5-215">A partir de 3.0, en EF Core solo se permite que se evalúen en el cliente las expresiones en la proyección de nivel superior (la última llamada a `Select()` de la consulta).</span><span class="sxs-lookup"><span data-stu-id="602c5-215">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="602c5-216">Cuando las expresiones de otra parte de la consulta no se pueden convertir en SQL o un parámetro, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="602c5-216">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="602c5-217">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-217">**Why**</span></span>

<span data-ttu-id="602c5-218">La evaluación de cliente automática de las consultas permite que se ejecuten muchas consultas incluso si no se pueden convertir elementos importantes de ellas.</span><span class="sxs-lookup"><span data-stu-id="602c5-218">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="602c5-219">Esto puede provocar un comportamiento inesperado y potencialmente dañino que es posible que solo sea evidente en entornos de producción.</span><span class="sxs-lookup"><span data-stu-id="602c5-219">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="602c5-220">Por ejemplo, una condición en una llamada a `Where()` que no se puede convertir puede provocar que todas las filas de la tabla se transfieran desde el servidor de base de datos y que el filtro se aplique en el cliente.</span><span class="sxs-lookup"><span data-stu-id="602c5-220">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="602c5-221">Esta situación puede pasar desapercibida fácilmente si la tabla solo contiene algunas filas en la fase de desarrollo, pero ser más grave cuando la aplicación pase a producción, donde la tabla puede contener millones de filas.</span><span class="sxs-lookup"><span data-stu-id="602c5-221">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="602c5-222">Las advertencias de evaluación de cliente también se suelen pasar por alto durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="602c5-222">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="602c5-223">Además de esto, la evaluación de cliente automática puede causar problemas en los que la mejora de la traducción de consultas para expresiones específicas provocaba cambios importantes no deseados entre versiones.</span><span class="sxs-lookup"><span data-stu-id="602c5-223">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="602c5-224">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-224">**Mitigations**</span></span>

<span data-ttu-id="602c5-225">Si una consulta no se puede traducir totalmente, vuelva a escribirla en un formato que se pueda traducir, o bien use `AsEnumerable()`, `ToList()` o una función similar para devolver los datos al cliente de forma explícita, donde después se puedan seguir procesando mediante LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="602c5-225">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="602c5-226">EF Core 3.0 tiene como destino .NET Standard 2.1, y no .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="602c5-226">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="602c5-227">Problema de seguimiento n.º 15498</span><span class="sxs-lookup"><span data-stu-id="602c5-227">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="602c5-228">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="602c5-228">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="602c5-229">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-229">**Old behavior**</span></span>

<span data-ttu-id="602c5-230">Antes de la versión 3.0, EF Core tenía como destino .NET Standard 2.0 y se podía ejecutar en todas las plataformas que admitieran dicho estándar, incluido .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="602c5-230">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="602c5-231">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-231">**New behavior**</span></span>

<span data-ttu-id="602c5-232">A partir de la versión 3.0, EF Core tiene como destino .NET Standard 2.1 y se puede ejecutar en todas las plataformas que admitan dicho estándar.</span><span class="sxs-lookup"><span data-stu-id="602c5-232">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="602c5-233">Esto no incluye .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="602c5-233">This does not include .NET Framework.</span></span>

<span data-ttu-id="602c5-234">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-234">**Why**</span></span>

<span data-ttu-id="602c5-235">Esto forma parte de una decisión estratégica para todas las tecnologías de .NET que tiene como objetivo centrar los esfuerzos en .NET Core y otras plataformas modernas de .NET, como Xamarin.</span><span class="sxs-lookup"><span data-stu-id="602c5-235">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="602c5-236">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-236">**Mitigations**</span></span>

<span data-ttu-id="602c5-237">Valore la posibilidad de cambiar a una plataforma moderna de .NET.</span><span class="sxs-lookup"><span data-stu-id="602c5-237">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="602c5-238">Si esto no es posible, siga usando EF Core 2.1 o EF Core 2.2, puesto que ambas versiones admiten .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="602c5-238">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="602c5-239">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="602c5-239">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="602c5-240">Anuncios del problema de seguimiento n.º 325</span><span class="sxs-lookup"><span data-stu-id="602c5-240">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="602c5-241">Este cambio se ha introducido en ASP.NET Core 3.0 (versión preliminar 1).</span><span class="sxs-lookup"><span data-stu-id="602c5-241">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="602c5-242">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-242">**Old behavior**</span></span>

<span data-ttu-id="602c5-243">Antes de ASP.NET Core 3.0, cuando se agregaba una referencia de paquete a `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, se incluía EF Core y algunos de los proveedores de datos de EF Core, como el de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="602c5-243">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="602c5-244">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-244">**New behavior**</span></span>

<span data-ttu-id="602c5-245">A partir de la versión 3.0, el marco compartido ASP.NET Core no incluye EF Core ni ningún proveedor de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="602c5-245">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="602c5-246">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-246">**Why**</span></span>

<span data-ttu-id="602c5-247">Antes de este cambio, para obtener EF Core se necesitaban varios pasos en función de si la aplicación se destinaba a ASP.NET Core y SQL Server o no.</span><span class="sxs-lookup"><span data-stu-id="602c5-247">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="602c5-248">Además, la actualización de ASP.NET Core forzaba la de EF Core y el proveedor de SQL Server, lo que no siempre es deseable.</span><span class="sxs-lookup"><span data-stu-id="602c5-248">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="602c5-249">Con este cambio, la experiencia de obtención de EF Core es la misma en todos los proveedores, implementaciones admitidas de .NET y tipos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="602c5-249">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="602c5-250">Ahora los desarrolladores también pueden controlar exactamente cuándo se actualizan EF Core y los proveedores de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="602c5-250">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="602c5-251">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-251">**Mitigations**</span></span>

<span data-ttu-id="602c5-252">Para usar EF Core en una aplicación ASP.NET Core 3.0 o cualquier otra aplicación compatible, debe agregar de forma explícita una referencia de paquete al proveedor de base de datos de EF Core que se va a usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="602c5-252">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="602c5-253">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="602c5-253">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="602c5-254">Problema de seguimiento n.º 14016</span><span class="sxs-lookup"><span data-stu-id="602c5-254">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="602c5-255">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4) y la versión correspondiente del SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="602c5-255">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="602c5-256">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-256">**Old behavior**</span></span>

<span data-ttu-id="602c5-257">Antes de 3.0, la herramienta `dotnet ef` se incluía en el SDK de .NET Core y estaba disponible para usarse desde la línea de comandos de cualquier proyecto sin necesidad de realizar pasos adicionales.</span><span class="sxs-lookup"><span data-stu-id="602c5-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="602c5-258">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-258">**New behavior**</span></span>

<span data-ttu-id="602c5-259">A partir de la versión 3.0, el SDK de .NET no incluye la herramienta `dotnet ef`, por lo que antes de poder usarla tendrá que instalarla de forma explícita como una herramienta local o global.</span><span class="sxs-lookup"><span data-stu-id="602c5-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="602c5-260">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-260">**Why**</span></span>

<span data-ttu-id="602c5-261">Este cambio nos permite distribuir y actualizar `dotnet ef` como una herramienta convencional de la CLI de .NET en NuGet, coherente con el hecho de que la versión 3.0 de EF Core también se distribuye siempre como un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="602c5-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="602c5-262">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-262">**Mitigations**</span></span>

<span data-ttu-id="602c5-263">Para poder administrar las migraciones o aplicar scaffolding a `DbContext`, instale `dotnet-ef` como herramienta global:</span><span class="sxs-lookup"><span data-stu-id="602c5-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="602c5-264">También se puede obtener una herramienta local cuando se restauran las dependencias de un proyecto que la declara como una dependencia de herramientas mediante un [archivo de manifiesto de herramientas](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="602c5-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="602c5-265">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="602c5-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="602c5-266">Problema de seguimiento n.º 10996</span><span class="sxs-lookup"><span data-stu-id="602c5-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="602c5-267">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-267">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-268">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-268">**Old behavior**</span></span>

<span data-ttu-id="602c5-269">Antes de EF Core 3.0, estos nombres de métodos se sobrecargaban para funcionar tanto con una cadena normal como con una cadena que se debería interpolar en SQL y parámetros.</span><span class="sxs-lookup"><span data-stu-id="602c5-269">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="602c5-270">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-270">**New behavior**</span></span>

<span data-ttu-id="602c5-271">A partir de la versión EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw` y `ExecuteSqlRawAsync` para crear una consulta con parámetros donde los parámetros se pasan por separado de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="602c5-271">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="602c5-272">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-272">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="602c5-273">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated` y `ExecuteSqlInterpolatedAsync` para crear una consulta con parámetros donde los parámetros se pasan como parte de una cadena de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="602c5-273">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="602c5-274">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-274">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="602c5-275">Tenga en cuenta que las dos consultas anteriores producirán el mismo código SQL parametrizado con los mismos parámetros SQL.</span><span class="sxs-lookup"><span data-stu-id="602c5-275">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="602c5-276">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-276">**Why**</span></span>

<span data-ttu-id="602c5-277">Las sobrecargas del método como esta facilitan las llamadas accidentales al método de cadena sin procesar cuando la intención era llamar al método de cadena interpolada y viceversa.</span><span class="sxs-lookup"><span data-stu-id="602c5-277">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="602c5-278">Esto podría resultar en consultas que no se parametrizan cuando deberían.</span><span class="sxs-lookup"><span data-stu-id="602c5-278">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="602c5-279">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-279">**Mitigations**</span></span>

<span data-ttu-id="602c5-280">Haga el cambio para usar los nuevos nombres de métodos.</span><span class="sxs-lookup"><span data-stu-id="602c5-280">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="602c5-281">Solo se pueden especificar métodos de FromSql en raíces de consulta.</span><span class="sxs-lookup"><span data-stu-id="602c5-281">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="602c5-282">Problema de seguimiento n.° 15704</span><span class="sxs-lookup"><span data-stu-id="602c5-282">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="602c5-283">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="602c5-283">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="602c5-284">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-284">**Old behavior**</span></span>

<span data-ttu-id="602c5-285">Antes de EF Core 3.0, el método `FromSql` podía especificarse en cualquier lugar en la consulta.</span><span class="sxs-lookup"><span data-stu-id="602c5-285">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="602c5-286">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-286">**New behavior**</span></span>

<span data-ttu-id="602c5-287">A partir de EF Core 3.0, los nuevos métodos `FromSqlRaw` y `FromSqlInterpolated` (que reemplazan a `FromSql`) solo pueden especificarse en las raíces de la consulta, es decir, directamente en `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="602c5-287">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="602c5-288">Si intenta especificarlos en cualquier otro lugar se producirá un error de compilación.</span><span class="sxs-lookup"><span data-stu-id="602c5-288">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="602c5-289">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-289">**Why**</span></span>

<span data-ttu-id="602c5-290">La especificación de `FromSql` en cualquier otro lugar diferente de `DbSet` no tenía un significado o valor agregado, y podría causar ambigüedad en ciertos escenarios.</span><span class="sxs-lookup"><span data-stu-id="602c5-290">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="602c5-291">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-291">**Mitigations**</span></span>

<span data-ttu-id="602c5-292">Las invocaciones de `FromSql` se deben mover para que estén directamente en el `DbSet` al que se aplican.</span><span class="sxs-lookup"><span data-stu-id="602c5-292">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="602c5-293">Las consultas sin seguimiento ya no realizan la resolución de la identidad</span><span class="sxs-lookup"><span data-stu-id="602c5-293">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="602c5-294">Problema de seguimiento n.º 13518</span><span class="sxs-lookup"><span data-stu-id="602c5-294">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="602c5-295">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="602c5-295">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="602c5-296">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-296">**Old behavior**</span></span>

<span data-ttu-id="602c5-297">Antes de EF Core 3.0, se usaba la misma instancia de la entidad para cada aparición de una entidad con un tipo e identificador determinados.</span><span class="sxs-lookup"><span data-stu-id="602c5-297">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="602c5-298">Este comportamiento coincide con el de las consultas de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="602c5-298">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="602c5-299">Por ejemplo, esta consulta:</span><span class="sxs-lookup"><span data-stu-id="602c5-299">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="602c5-300">Esta consulta devolverá la misma instancia de `Category` para cada elemento `Product` asociado con la categoría determinada.</span><span class="sxs-lookup"><span data-stu-id="602c5-300">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="602c5-301">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-301">**New behavior**</span></span>

<span data-ttu-id="602c5-302">A partir de EF Core 3.0, se crean distintas instancias de la entidad si se encuentra una entidad con un tipo e identificador determinados en varias ubicaciones del gráfico devuelto.</span><span class="sxs-lookup"><span data-stu-id="602c5-302">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="602c5-303">Por ejemplo, la consulta anterior ahora devolverá una nueva instancia de `Category` para cada elemento `Product` cuando haya dos productos asociados a la misma categoría.</span><span class="sxs-lookup"><span data-stu-id="602c5-303">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="602c5-304">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-304">**Why**</span></span>

<span data-ttu-id="602c5-305">La resolución de las identidades (es decir, el hecho de determinar que una entidad tiene los mismos tipo e identificador que la entidad encontrada) agrega más rendimiento y sobrecarga de memoria.</span><span class="sxs-lookup"><span data-stu-id="602c5-305">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="602c5-306">Este enfoque suele ser contrario a por qué las consultas sin seguimiento se usan en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="602c5-306">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="602c5-307">Además, aunque la resolución de las identidades a veces puede resultar útil, no es necesaria si las entidades se van a serializar y enviar a un cliente, algo habitual para las consultas sin seguimiento.</span><span class="sxs-lookup"><span data-stu-id="602c5-307">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="602c5-308">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-308">**Mitigations**</span></span>

<span data-ttu-id="602c5-309">Si se requiere la resolución de identidad, use una consulta de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="602c5-309">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="602c5-310">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="602c5-310">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="602c5-311">Problema de seguimiento n.º 14523</span><span class="sxs-lookup"><span data-stu-id="602c5-311">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="602c5-312">Este cambio se revirtió en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="602c5-312">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="602c5-313">Revertimos este cambio porque la nueva configuración de EF Core 3.0 permite a la aplicación especificar el nivel de registro para cualquier evento.</span><span class="sxs-lookup"><span data-stu-id="602c5-313">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="602c5-314">Por ejemplo, para cambiar el registro de SQL a `Debug`, configure el nivel de forma explícita en `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="602c5-314">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="602c5-315">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="602c5-315">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="602c5-316">Problema de seguimiento n.º 12378</span><span class="sxs-lookup"><span data-stu-id="602c5-316">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="602c5-317">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="602c5-317">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="602c5-318">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-318">**Old behavior**</span></span>

<span data-ttu-id="602c5-319">Antes de EF Core 3.0, los valores temporales se asignaban a todas las propiedades de clave para las que posteriormente la base de datos generaba un valor real.</span><span class="sxs-lookup"><span data-stu-id="602c5-319">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="602c5-320">Normalmente, estos valores temporales eran números negativos grandes.</span><span class="sxs-lookup"><span data-stu-id="602c5-320">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="602c5-321">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-321">**New behavior**</span></span>

<span data-ttu-id="602c5-322">A partir de la versión 3.0, en EF Core se almacena el valor de clave temporal como parte de la información de seguimiento de la entidad y la propiedad clave en sí no se modifica.</span><span class="sxs-lookup"><span data-stu-id="602c5-322">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="602c5-323">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-323">**Why**</span></span>

<span data-ttu-id="602c5-324">Este cambio se ha realizado para evitar que los valores de clave temporales se conviertan erróneamente en permanentes cuando una entidad de la que previamente una instancia de `DbContext` ha realizado el seguimiento se mueve a otra instancia de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="602c5-324">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="602c5-325">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-325">**Mitigations**</span></span>

<span data-ttu-id="602c5-326">Las aplicaciones que asignan valores de clave principal a claves externas para crear asociaciones entre entidades pueden depender del comportamiento anterior si las claves principales son generadas por el almacén y pertenecen a entidades en el estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="602c5-326">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="602c5-327">Esto se puede evitar con las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="602c5-327">This can be avoided by:</span></span>
* <span data-ttu-id="602c5-328">No se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="602c5-328">Not using store-generated keys.</span></span>
* <span data-ttu-id="602c5-329">Se establecen propiedades de navegación para crear relaciones en lugar de establecer valores de clave externa.</span><span class="sxs-lookup"><span data-stu-id="602c5-329">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="602c5-330">Se obtienen los valores de clave temporal reales de la información de seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="602c5-330">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="602c5-331">Por ejemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` devolverá el valor temporal aunque no se haya establecido `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="602c5-331">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="602c5-332">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="602c5-332">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="602c5-333">Problema de seguimiento n.º 14616</span><span class="sxs-lookup"><span data-stu-id="602c5-333">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="602c5-334">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="602c5-334">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="602c5-335">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-335">**Old behavior**</span></span>

<span data-ttu-id="602c5-336">Antes de EF Core 3.0, se realizaba el seguimiento en el estado `Added` de las entidades sin seguimiento detectadas por `DetectChanges` y se insertaban como una fila nueva cuando se llamaba a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="602c5-336">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="602c5-337">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-337">**New behavior**</span></span>

<span data-ttu-id="602c5-338">A partir de EF Core 3.0, si una entidad usa valores de clave generados y se establece un valor de clave, se realizará el seguimiento de la entidad en el estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="602c5-338">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="602c5-339">Esto significa que se supone que existe una fila para la entidad y que se actualizará cuando se llame a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="602c5-339">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="602c5-340">Si no se establece el valor de clave, o bien si el tipo de entidad no usa claves generadas, se seguirá realizando el seguimiento de la entidad nueva como `Added` al igual que en las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="602c5-340">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="602c5-341">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-341">**Why**</span></span>

<span data-ttu-id="602c5-342">Este cambio se ha realizado para que sea más sencillo y coherente trabajar con gráficos de entidades desconectadas mientras se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="602c5-342">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="602c5-343">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-343">**Mitigations**</span></span>

<span data-ttu-id="602c5-344">Este cambio puede interrumpir una aplicación si se configura un tipo de entidad para usar claves generadas, pero se establecen de forma explícita valores de clave para las instancias nuevas.</span><span class="sxs-lookup"><span data-stu-id="602c5-344">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="602c5-345">La solución consiste en configurar de forma explícita las propiedades de clave para que no usen valores generados.</span><span class="sxs-lookup"><span data-stu-id="602c5-345">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="602c5-346">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="602c5-346">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="602c5-347">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="602c5-347">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="602c5-348">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="602c5-348">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="602c5-349">Problema de seguimiento n.º 10114</span><span class="sxs-lookup"><span data-stu-id="602c5-349">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="602c5-350">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="602c5-350">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="602c5-351">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-351">**Old behavior**</span></span>

<span data-ttu-id="602c5-352">Antes de la versión 3.0, en EF Core no se aplicaban acciones en cascada (eliminación de entidades dependientes cuando se eliminaba una entidad de seguridad obligatoria o cuando se rompía la relación con una entidad de seguridad obligatoria) hasta que se llamaba a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="602c5-352">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="602c5-353">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-353">**New behavior**</span></span>

<span data-ttu-id="602c5-354">A partir de 3.0, en EF Core las acciones en cascada se aplican en cuanto se detecta la condición desencadenadora.</span><span class="sxs-lookup"><span data-stu-id="602c5-354">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="602c5-355">Por ejemplo, como resultado de la llamada a `context.Remove()` para eliminar una entidad de seguridad, todos los dependientes obligatorios relacionados de los que se realiza el seguimiento también se establecen en `Deleted` inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="602c5-355">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="602c5-356">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-356">**Why**</span></span>

<span data-ttu-id="602c5-357">Este cambio se ha realizado para mejorar la experiencia en escenarios de auditoría y enlace de datos donde es importante comprender qué entidades se van a eliminar _antes_ de llamar a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="602c5-357">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="602c5-358">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-358">**Mitigations**</span></span>

<span data-ttu-id="602c5-359">El comportamiento anterior se puede restaurar mediante opciones de `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="602c5-359">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="602c5-360">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-360">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="602c5-361">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="602c5-361">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="602c5-362">Problema de seguimiento n.º 12661</span><span class="sxs-lookup"><span data-stu-id="602c5-362">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="602c5-363">Este cambio se introdujo en EF Core 3.0 (versión preliminar 5).</span><span class="sxs-lookup"><span data-stu-id="602c5-363">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="602c5-364">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-364">**Old behavior**</span></span>

<span data-ttu-id="602c5-365">Antes de la versión 3.0, `DeleteBehavior.Restrict` creaba claves externas en la base de datos con la semántica `Restrict`, pero también realizaba una corrección interna de manera no evidente.</span><span class="sxs-lookup"><span data-stu-id="602c5-365">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="602c5-366">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-366">**New behavior**</span></span>

<span data-ttu-id="602c5-367">A partir de la versión 3.0, `DeleteBehavior.Restrict` garantiza que las claves externas se crean con la semántica `Restrict`, es decir, sin cascadas y realizando una infracción de restricción, sin llevar a cabo correcciones internas de EF.</span><span class="sxs-lookup"><span data-stu-id="602c5-367">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="602c5-368">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-368">**Why**</span></span>

<span data-ttu-id="602c5-369">Este cambio se realizó para mejorar la experiencia de uso de `DeleteBehavior` de manera intuitiva sin efectos secundarios inesperados.</span><span class="sxs-lookup"><span data-stu-id="602c5-369">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="602c5-370">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-370">**Mitigations**</span></span>

<span data-ttu-id="602c5-371">El comportamiento anterior se puede restaurar con `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="602c5-371">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="602c5-372">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="602c5-372">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="602c5-373">Problema de seguimiento n.º 14194</span><span class="sxs-lookup"><span data-stu-id="602c5-373">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="602c5-374">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="602c5-374">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="602c5-375">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-375">**Old behavior**</span></span>

<span data-ttu-id="602c5-376">Antes de EF Core 3.0, los [tipos de consulta](xref:core/modeling/query-types) eran un medio para consultar los datos que no definen una clave principal de una manera estructurada.</span><span class="sxs-lookup"><span data-stu-id="602c5-376">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="602c5-377">Es decir, un tipo de consulta se usaba para asignar tipos de entidad sin claves (más probablemente desde una vista, pero posiblemente desde una tabla), mientras que un tipo de entidad estándar se usaba cuando había una clave disponible (más probablemente desde una tabla, pero posiblemente desde una vista).</span><span class="sxs-lookup"><span data-stu-id="602c5-377">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="602c5-378">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-378">**New behavior**</span></span>

<span data-ttu-id="602c5-379">Ahora un tipo de consulta se convierte en un tipo de entidad sin una clave principal.</span><span class="sxs-lookup"><span data-stu-id="602c5-379">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="602c5-380">Los tipos de entidad sin clave tienen la misma funcionalidad que los tipos de consulta de las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="602c5-380">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="602c5-381">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-381">**Why**</span></span>

<span data-ttu-id="602c5-382">Este cambio se ha realizado para reducir la confusión en torno a la finalidad de los tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="602c5-382">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="602c5-383">En concreto, son tipos de entidad sin clave y, por ello, intrínsecamente son de solo lectura, pero no se deben usar solo porque un tipo de entidad tenga que ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="602c5-383">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="602c5-384">Del mismo modo, se suelen asignar a vistas, pero solo porque las vistas no suelen definir claves.</span><span class="sxs-lookup"><span data-stu-id="602c5-384">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="602c5-385">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-385">**Mitigations**</span></span>

<span data-ttu-id="602c5-386">Los elementos siguientes de la API ahora están obsoletos:</span><span class="sxs-lookup"><span data-stu-id="602c5-386">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="602c5-387">**`ModelBuilder.Query<>()`** : en su lugar es necesario llamar a `ModelBuilder.Entity<>().HasNoKey()` para marcar un tipo de entidad como sin claves.</span><span class="sxs-lookup"><span data-stu-id="602c5-387">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="602c5-388">Esto todavía no se configurará por convención para evitar una configuración incorrecta cuando se espera una clave principal, pero no coincide con la convención.</span><span class="sxs-lookup"><span data-stu-id="602c5-388">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="602c5-389">**`DbQuery<>`** : en su lugar se debe usar `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="602c5-389">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="602c5-390">**`DbContext.Query<>()`** : en su lugar se debe usar `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="602c5-390">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="602c5-391">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="602c5-391">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="602c5-392">[Problema de seguimiento n.º 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema de seguimiento n.º 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema de seguimiento n.º 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="602c5-392">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="602c5-393">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="602c5-393">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="602c5-394">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-394">**Old behavior**</span></span>

<span data-ttu-id="602c5-395">Antes de EF Core 3.0, la configuración de la relación de propiedad se realizaba directamente después de la llamada a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="602c5-395">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="602c5-396">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-396">**New behavior**</span></span>

<span data-ttu-id="602c5-397">A partir de EF Core 3.0, ahora hay una API fluida para configurar una propiedad de navegación para el propietario mediante `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="602c5-397">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="602c5-398">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-398">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="602c5-399">La configuración relacionada con la relación entre el propietario y lo que se posee ahora se debe encadenar después de `WithOwner()`, de forma similar a cómo se configuran otras relaciones.</span><span class="sxs-lookup"><span data-stu-id="602c5-399">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="602c5-400">Pero la configuración del propio tipo de propiedad se seguirá encadenando después de `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="602c5-400">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="602c5-401">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-401">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="602c5-402">Además, la llamada a `Entity()`, `HasOne()` o `Set()` con un tipo de propiedad de destino ahora iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="602c5-402">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="602c5-403">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-403">**Why**</span></span>

<span data-ttu-id="602c5-404">Este cambio se ha realizado para crear una separación más clara entre la configuración del propio tipo de propiedad y la _relación con_ el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="602c5-404">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="602c5-405">A su vez, esto elimina la ambigüedad y la confusión de métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="602c5-405">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="602c5-406">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-406">**Mitigations**</span></span>

<span data-ttu-id="602c5-407">Cambie la configuración de las relaciones de tipo de propiedad para usar la nueva superficie de API, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="602c5-407">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="602c5-408">Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales</span><span class="sxs-lookup"><span data-stu-id="602c5-408">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="602c5-409">Problema de seguimiento n.º 9005</span><span class="sxs-lookup"><span data-stu-id="602c5-409">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="602c5-410">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-410">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-411">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-411">**Old behavior**</span></span>

<span data-ttu-id="602c5-412">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="602c5-412">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
<span data-ttu-id="602c5-413">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, siempre era necesaria una instancia de `OrderDetails` al agregar un elemento `Order` nuevo.</span><span class="sxs-lookup"><span data-stu-id="602c5-413">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="602c5-414">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-414">**New behavior**</span></span>

<span data-ttu-id="602c5-415">A partir de la versión 3.0, EF Core permite agregar `Order` sin `OrderDetails` y asigna todas las propiedades `OrderDetails` a excepción de la clave principal a columnas que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="602c5-415">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="602c5-416">Al realizar consultas, EF Core establece `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor o si no tiene propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.</span><span class="sxs-lookup"><span data-stu-id="602c5-416">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="602c5-417">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-417">**Mitigations**</span></span>

<span data-ttu-id="602c5-418">Si el modelo tiene una tabla que comparte dependencias con todas las columnas opcionales, pero la navegación que apunta a ella no se espera que sea `null`, la aplicación debería modificarse para controlar los casos en los que la navegación sea `null`.</span><span class="sxs-lookup"><span data-stu-id="602c5-418">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="602c5-419">Si esto no es posible, debería agregarse una propiedad necesaria al tipo de entidad o, al menos, una entidad debería tener un valor distinto a `null` asignado.</span><span class="sxs-lookup"><span data-stu-id="602c5-419">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="602c5-420">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="602c5-420">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="602c5-421">Problema de seguimiento n.º 14154</span><span class="sxs-lookup"><span data-stu-id="602c5-421">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="602c5-422">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-423">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-423">**Old behavior**</span></span>

<span data-ttu-id="602c5-424">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="602c5-424">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="602c5-425">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, si solo se actualizaba `OrderDetails`, no se actualizaba el valor `Version` en el cliente y se producía un error en la próxima actualización.</span><span class="sxs-lookup"><span data-stu-id="602c5-425">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="602c5-426">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-426">**New behavior**</span></span>

<span data-ttu-id="602c5-427">A partir de la versión 3.0, EF Core propaga el nuevo valor `Version` en `Order` si posee `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="602c5-427">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="602c5-428">En caso contrario, se produce una excepción durante la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="602c5-428">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="602c5-429">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-429">**Why**</span></span>

<span data-ttu-id="602c5-430">Este cambio se realizó para evitar un valor de token de simultaneidad obsoleto cuando solo se actualiza una de las entidades asignadas a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="602c5-430">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="602c5-431">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-431">**Mitigations**</span></span>

<span data-ttu-id="602c5-432">Todas las entidades que comparten la tabla deben incluir una propiedad que se asigna a la columna del token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="602c5-432">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="602c5-433">Es posible crear una en estado reemplazado:</span><span class="sxs-lookup"><span data-stu-id="602c5-433">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="602c5-434">Ahora, las propiedades heredadas de tipos sin asignar se asignan a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="602c5-434">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="602c5-435">Problema de seguimiento n.º 13998</span><span class="sxs-lookup"><span data-stu-id="602c5-435">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="602c5-436">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-436">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-437">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-437">**Old behavior**</span></span>

<span data-ttu-id="602c5-438">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="602c5-438">Consider the following model:</span></span>
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="602c5-439">Antes de EF Core 3.0, la propiedad `ShippingAddress` se asignaba a columnas distintas para `BulkOrder` y `Order` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="602c5-439">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="602c5-440">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-440">**New behavior**</span></span>

<span data-ttu-id="602c5-441">A partir de la versión3.0, EF Core solo crea una columna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="602c5-441">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="602c5-442">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-442">**Why**</span></span>

<span data-ttu-id="602c5-443">El comportamiento anterior no era el esperado.</span><span class="sxs-lookup"><span data-stu-id="602c5-443">The old behavoir was unexpected.</span></span>

<span data-ttu-id="602c5-444">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-444">**Mitigations**</span></span>

<span data-ttu-id="602c5-445">Todavía se puede asignar explícitamente la propiedad a columnas separadas en los tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="602c5-445">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="602c5-446">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="602c5-446">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="602c5-447">Problema de seguimiento n.º 13274</span><span class="sxs-lookup"><span data-stu-id="602c5-447">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="602c5-448">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="602c5-448">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="602c5-449">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-449">**Old behavior**</span></span>

<span data-ttu-id="602c5-450">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="602c5-450">Consider the following model:</span></span>
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="602c5-451">Antes de EF Core 3.0, se podía usar la propiedad `CustomerId` para la clave externa por convención.</span><span class="sxs-lookup"><span data-stu-id="602c5-451">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="602c5-452">Pero si `Order` es un tipo de propiedad, entonces esto convertiría también a `CustomerId` en la clave principal, algo que no suele ser lo esperado.</span><span class="sxs-lookup"><span data-stu-id="602c5-452">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="602c5-453">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-453">**New behavior**</span></span>

<span data-ttu-id="602c5-454">A partir de la versión 3.0, EF Core no intenta usar las propiedades de claves externas por convención si tienen el mismo nombre que la propiedad de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="602c5-454">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="602c5-455">Los patrones de nombre de tipo de entidad de seguridad concatenado con el nombre de propiedad de la entidad de seguridad y de nombre de navegación concatenado con el nombre de propiedad de la entidad de seguridad todavía se hacen coincidir.</span><span class="sxs-lookup"><span data-stu-id="602c5-455">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="602c5-456">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-456">For example:</span></span>

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

<span data-ttu-id="602c5-457">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-457">**Why**</span></span>

<span data-ttu-id="602c5-458">Este cambio se ha realizado para evitar definir erróneamente una propiedad de clave principal en el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="602c5-458">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="602c5-459">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-459">**Mitigations**</span></span>

<span data-ttu-id="602c5-460">Si la propiedad se ha diseñado para ser la clave externa y, por tanto, parte de la clave principal, se debe configurar explícitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="602c5-460">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="602c5-461">Ahora, la conexión de base de datos se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="602c5-461">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="602c5-462">Problema de seguimiento n.º 14218</span><span class="sxs-lookup"><span data-stu-id="602c5-462">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="602c5-463">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-463">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-464">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-464">**Old behavior**</span></span>

<span data-ttu-id="602c5-465">Antes de EF Core 3.0, si el contexto abría la conexión dentro de `TransactionScope`, la conexión permanecía abierta mientras el ámbito actual `TransactionScope` estuviese activo.</span><span class="sxs-lookup"><span data-stu-id="602c5-465">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

<span data-ttu-id="602c5-466">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-466">**New behavior**</span></span>

<span data-ttu-id="602c5-467">A partir de la versión 3.0, EF Core cierra la conexión en cuanto se deja de usar.</span><span class="sxs-lookup"><span data-stu-id="602c5-467">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="602c5-468">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-468">**Why**</span></span>

<span data-ttu-id="602c5-469">Este cambio permite usar varios contextos en el mismo ámbito `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="602c5-469">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="602c5-470">El comportamiento nuevo también coincide con el de EF6.</span><span class="sxs-lookup"><span data-stu-id="602c5-470">The new behavior also matches EF6.</span></span>

<span data-ttu-id="602c5-471">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-471">**Mitigations**</span></span>

<span data-ttu-id="602c5-472">Si la conexión debe permanecer abierta, una llamada explícita a `OpenConnection()` asegurará que EF Core no la cierre de forma prematura:</span><span class="sxs-lookup"><span data-stu-id="602c5-472">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="602c5-473">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="602c5-473">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="602c5-474">Problema de seguimiento n.º 6872</span><span class="sxs-lookup"><span data-stu-id="602c5-474">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="602c5-475">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-475">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-476">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-476">**Old behavior**</span></span>

<span data-ttu-id="602c5-477">Antes de EF Core 3.0, se usaba un generador de valores compartidos para todas las propiedades de clave entera en memoria.</span><span class="sxs-lookup"><span data-stu-id="602c5-477">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="602c5-478">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-478">**New behavior**</span></span>

<span data-ttu-id="602c5-479">A partir de EF Core 3.0, cada propiedad de clave entera obtiene su propio generador de valores cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="602c5-479">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="602c5-480">Además, si se elimina la base de datos, la generación de claves se restablece para todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="602c5-480">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="602c5-481">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-481">**Why**</span></span>

<span data-ttu-id="602c5-482">Este cambio se ha realizado para alinear la generación de claves en memoria más estrechamente a la generación de claves de base de datos reales y para mejorar la capacidad para aislar las pruebas entre sí cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="602c5-482">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="602c5-483">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-483">**Mitigations**</span></span>

<span data-ttu-id="602c5-484">Esto puede interrumpir una aplicación que se base en el establecimiento de valores de clave específicos en memoria.</span><span class="sxs-lookup"><span data-stu-id="602c5-484">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="602c5-485">En su lugar, considere la posibilidad de no depender de valores de clave específicos, o bien de actualizar para que coincida con el comportamiento nuevo.</span><span class="sxs-lookup"><span data-stu-id="602c5-485">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="602c5-486">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="602c5-486">Backing fields are used by default</span></span>

[<span data-ttu-id="602c5-487">Problema de seguimiento n.º 12430</span><span class="sxs-lookup"><span data-stu-id="602c5-487">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="602c5-488">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="602c5-488">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="602c5-489">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-489">**Old behavior**</span></span>

<span data-ttu-id="602c5-490">Antes de la versión 3.0, incluso si se conocía el campo de respaldo de una propiedad, de forma predeterminada en EF Core se leía y escribía el valor de propiedad mediante los métodos captadores y establecedores de propiedades.</span><span class="sxs-lookup"><span data-stu-id="602c5-490">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="602c5-491">La excepción era la ejecución de consultas, donde el campo de respaldo se establecía directamente si se conocía.</span><span class="sxs-lookup"><span data-stu-id="602c5-491">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="602c5-492">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-492">**New behavior**</span></span>

<span data-ttu-id="602c5-493">A partir de EF Core 3.0, si se conoce el campo de respaldo para una propiedad, EF Core siempre la leerá y escribirá mediante el campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="602c5-493">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="602c5-494">Esto podría provocar una interrupción de la aplicación si depende de un comportamiento adicional codificado en los métodos captadores o establecedores.</span><span class="sxs-lookup"><span data-stu-id="602c5-494">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="602c5-495">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-495">**Why**</span></span>

<span data-ttu-id="602c5-496">Este cambio se ha realizado para evitar que EF Core desencadene erróneamente lógica de negocios de forma predeterminada al realizar operaciones de base de datos que implican entidades.</span><span class="sxs-lookup"><span data-stu-id="602c5-496">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="602c5-497">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-497">**Mitigations**</span></span>

<span data-ttu-id="602c5-498">El comportamiento anterior a la versión 3.0 se puede restaurar mediante la configuración del modo de acceso de propiedad en `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="602c5-498">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="602c5-499">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-499">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="602c5-500">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="602c5-500">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="602c5-501">Problema de seguimiento n.º 12523</span><span class="sxs-lookup"><span data-stu-id="602c5-501">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="602c5-502">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-502">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-503">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-503">**Old behavior**</span></span>

<span data-ttu-id="602c5-504">Antes de EF Core 3.0, si varios campos coincidían con las reglas para buscar el campo de respaldo de una propiedad, se elegía un campo según un orden de prioridad.</span><span class="sxs-lookup"><span data-stu-id="602c5-504">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="602c5-505">Esto podía producir que, en caso de ambigüedad, se usara el campo incorrecto.</span><span class="sxs-lookup"><span data-stu-id="602c5-505">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="602c5-506">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-506">**New behavior**</span></span>

<span data-ttu-id="602c5-507">A partir de EF Core 3.0, si varios campos coinciden con la misma propiedad, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="602c5-507">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="602c5-508">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-508">**Why**</span></span>

<span data-ttu-id="602c5-509">Este cambio se ha realizado para evitar de forma silenciosa el uso de un campo con respecto a otro cuando solo uno puede ser correcto.</span><span class="sxs-lookup"><span data-stu-id="602c5-509">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="602c5-510">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-510">**Mitigations**</span></span>

<span data-ttu-id="602c5-511">En las propiedades con campos de respaldo ambiguos se debe especificar de forma explícita el campo que se va usar.</span><span class="sxs-lookup"><span data-stu-id="602c5-511">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="602c5-512">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="602c5-512">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="602c5-513">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="602c5-513">Field-only property names should match the field name</span></span>

<span data-ttu-id="602c5-514">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-514">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-515">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-515">**Old behavior**</span></span>

<span data-ttu-id="602c5-516">Antes de EF Core 3.0, una propiedad podía especificarse con un valor de cadena y, si no había ninguna propiedad con ese nombre en el tipo CLR, EF Core intentaba hacerla coincidir con un campo mediante reglas de convención.</span><span class="sxs-lookup"><span data-stu-id="602c5-516">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="602c5-517">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-517">**New behavior**</span></span>

<span data-ttu-id="602c5-518">A partir de EF Core 3.0, una propiedad de solo campo debe coincidir exactamente con el nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="602c5-518">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="602c5-519">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-519">**Why**</span></span>

<span data-ttu-id="602c5-520">Este cambio se realizó para evitar el uso del mismo campo para dos propiedades con nombres similares. También hace que las reglas de coincidencia para propiedades solo de campo sean las mismas que para las propiedades asignadas a propiedades CLR.</span><span class="sxs-lookup"><span data-stu-id="602c5-520">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="602c5-521">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-521">**Mitigations**</span></span>

<span data-ttu-id="602c5-522">Las propiedades solo de campo deberían tener el mismo nombre que el campo al que están asignadas.</span><span class="sxs-lookup"><span data-stu-id="602c5-522">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="602c5-523">En una próxima versión preliminar de EF Core 3.0 tenemos planeado volver a habilitar la configuración explícita de un nombre de campo distinto al nombre de la propiedad:</span><span class="sxs-lookup"><span data-stu-id="602c5-523">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="602c5-524">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="602c5-524">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="602c5-525">Problema de seguimiento n.° 14756</span><span class="sxs-lookup"><span data-stu-id="602c5-525">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="602c5-526">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-526">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-527">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-527">**Old behavior**</span></span>

<span data-ttu-id="602c5-528">Antes de EF Core 3.0, llamar a `AddDbContext` o a `AddDbContextPool` también podría registrar los servicios de almacenamiento en caché y de registro con D.I. a través de llamadas a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) y a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="602c5-528">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="602c5-529">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-529">**New behavior**</span></span>

<span data-ttu-id="602c5-530">A partir de EF Core 3.0, `AddDbContext` y `AddDbContextPool` ya no registrarán estos servicios con inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="602c5-530">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="602c5-531">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-531">**Why**</span></span>

<span data-ttu-id="602c5-532">EF Core 3.0 no requiere que estos servicios estén en el contenedor de inserción de dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="602c5-532">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="602c5-533">Pero si `ILoggerFactory` se registra en el contenedor de DI de la aplicación, EF Core lo empezará a usar de todos modos.</span><span class="sxs-lookup"><span data-stu-id="602c5-533">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="602c5-534">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-534">**Mitigations**</span></span>

<span data-ttu-id="602c5-535">Si la aplicación necesita estos servicios, regístrelos de manera explícita con el contenedor de DI mediante [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="602c5-535">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="602c5-536">Ahora DbContext.Entry realiza una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="602c5-536">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="602c5-537">Problema de seguimiento n.º 13552</span><span class="sxs-lookup"><span data-stu-id="602c5-537">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="602c5-538">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="602c5-538">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="602c5-539">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-539">**Old behavior**</span></span>

<span data-ttu-id="602c5-540">Antes de EF Core 3.0, la llamada a `DbContext.Entry` provocaba que se detectaran cambios para todas las entidades con seguimiento.</span><span class="sxs-lookup"><span data-stu-id="602c5-540">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="602c5-541">Esto garantizaba que el estado expuesto en `EntityEntry` estuviera actualizado.</span><span class="sxs-lookup"><span data-stu-id="602c5-541">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="602c5-542">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-542">**New behavior**</span></span>

<span data-ttu-id="602c5-543">A partir de EF Core 3.0, ahora la llamada a `DbContext.Entry` solo intenta detectar cambios en la entidad dada y cualquier entidad de seguridad relacionada con ella de la que se haya realizado el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="602c5-543">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="602c5-544">Esto significa que es posible que la llamada a este método no haya detectado otros cambios, lo que podría tener implicaciones en el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="602c5-544">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="602c5-545">Observe que si `ChangeTracker.AutoDetectChangesEnabled` se establece en `false` incluso esta detección de cambios local se deshabilitará.</span><span class="sxs-lookup"><span data-stu-id="602c5-545">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="602c5-546">Otros métodos que provocan la detección de cambios (como `ChangeTracker.Entries` y `SaveChanges`) siguen provocando una acción `DetectChanges` completa de todas las entidades de las que se realiza el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="602c5-546">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="602c5-547">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-547">**Why**</span></span>

<span data-ttu-id="602c5-548">Este cambio se ha realizado para mejorar el rendimiento predeterminado del uso de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="602c5-548">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="602c5-549">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-549">**Mitigations**</span></span>

<span data-ttu-id="602c5-550">Llame a `ChgangeTracker.DetectChanges()` de forma explícita antes de llamar a `Entry` para garantizar el comportamiento anterior a la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="602c5-550">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="602c5-551">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="602c5-551">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="602c5-552">Problema de seguimiento n.º 14617</span><span class="sxs-lookup"><span data-stu-id="602c5-552">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="602c5-553">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-553">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-554">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-554">**Old behavior**</span></span>

<span data-ttu-id="602c5-555">Antes de EF Core 3.0, se podían usar las propiedades de clave `string` y `byte[]` sin tener que establecer de forma explícita un valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="602c5-555">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="602c5-556">En ese caso, el valor de clave se generaba en el cliente como un GUID, que se serializaba en bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="602c5-556">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="602c5-557">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-557">**New behavior**</span></span>

<span data-ttu-id="602c5-558">A partir de EF Core 3.0, se iniciará una excepción en la que indica que no se ha establecido ningún valor de clave.</span><span class="sxs-lookup"><span data-stu-id="602c5-558">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="602c5-559">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-559">**Why**</span></span>

<span data-ttu-id="602c5-560">Este cambio se ha realizado porque los valores `string`/`byte[]` generados por el cliente no suelen ser útiles, y el comportamiento predeterminado dificultaba razonar sobre los valores de clave generados de una forma habitual.</span><span class="sxs-lookup"><span data-stu-id="602c5-560">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="602c5-561">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-561">**Mitigations**</span></span>

<span data-ttu-id="602c5-562">Se puede obtener el comportamiento anterior a la versión 3.0 si se especifica de forma explícita que las propiedades de clave deben usar los valores generados si no se establece ningún otro valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="602c5-562">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="602c5-563">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="602c5-563">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="602c5-564">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="602c5-564">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="602c5-565">Ahora ILoggerFactory es un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="602c5-565">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="602c5-566">Problema de seguimiento n.º 14698</span><span class="sxs-lookup"><span data-stu-id="602c5-566">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="602c5-567">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="602c5-567">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="602c5-568">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-568">**Old behavior**</span></span>

<span data-ttu-id="602c5-569">Antes de EF Core 3.0, `ILoggerFactory` se registraba como un servicio de singleton.</span><span class="sxs-lookup"><span data-stu-id="602c5-569">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="602c5-570">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-570">**New behavior**</span></span>

<span data-ttu-id="602c5-571">A partir de EF Core 3.0, `ILoggerFactory` ahora se registra como con ámbito.</span><span class="sxs-lookup"><span data-stu-id="602c5-571">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="602c5-572">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-572">**Why**</span></span>

<span data-ttu-id="602c5-573">Este cambio se ha realizado para permitir la asociación de un registrador con una instancia de `DbContext`, lo que habilita otras funciones y quita algunos casos de comportamiento patológico como un aumento vertiginoso de los proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="602c5-573">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="602c5-574">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-574">**Mitigations**</span></span>

<span data-ttu-id="602c5-575">Este cambio no debería afectar al código de la aplicación a menos que registre y use servicios personalizados en el proveedor de servicios internos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="602c5-575">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="602c5-576">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="602c5-576">This isn't common.</span></span>
<span data-ttu-id="602c5-577">En estos casos, la mayoría de los elementos seguirá funcionando, pero cualquier servicio de singleton que dependiera de `ILoggerFactory` tendrá que cambiarse para obtener la interfaz `ILoggerFactory` de otra forma.</span><span class="sxs-lookup"><span data-stu-id="602c5-577">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="602c5-578">Si experimenta situaciones como esta, registre un problema en el [rastreador de problemas de GitHub de EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para hacernos saber cómo usa `ILoggerFactory`, para que podamos comprender mejor cómo evitar esta interrupción en el futuro.</span><span class="sxs-lookup"><span data-stu-id="602c5-578">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="602c5-579">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="602c5-579">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="602c5-580">Problema de seguimiento n.º 12780</span><span class="sxs-lookup"><span data-stu-id="602c5-580">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="602c5-581">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-581">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-582">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-582">**Old behavior**</span></span>

<span data-ttu-id="602c5-583">Antes de EF Core 3.0, una vez que se eliminaba `DbContext` no había ninguna forma de saber si una determinada propiedad de navegación de una entidad obtenida de ese contexto se había cargado completamente o no.</span><span class="sxs-lookup"><span data-stu-id="602c5-583">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="602c5-584">En su lugar, los proxies asumían que una navegación de referencia se cargaba si tenía un valor distinto de NULL, y que una navegación de colección se cargaba si no estaba vacía.</span><span class="sxs-lookup"><span data-stu-id="602c5-584">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="602c5-585">En estos casos, el intento de carga diferida era no operativo.</span><span class="sxs-lookup"><span data-stu-id="602c5-585">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="602c5-586">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-586">**New behavior**</span></span>

<span data-ttu-id="602c5-587">A partir de EF Core 3.0, los proxies realizan el seguimiento de si una propiedad de navegación se carga o no.</span><span class="sxs-lookup"><span data-stu-id="602c5-587">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="602c5-588">Esto significa que el intento de acceder a una propiedad de navegación que se carga después de que se haya eliminado el contexto siempre será no operativo, incluso cuando la navegación cargada está vacía o es NULL.</span><span class="sxs-lookup"><span data-stu-id="602c5-588">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="602c5-589">Por el contrario, el intento de acceder a una propiedad de navegación que no está cargada iniciará una excepción si el contexto se ha eliminado, incluso si la propiedad de navegación es una colección no vacía.</span><span class="sxs-lookup"><span data-stu-id="602c5-589">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="602c5-590">Si se produce esta situación, significa que el código de aplicación está intentando usar la carga diferida en un momento no válido y que se debe cambiar la aplicación para que lo no haga.</span><span class="sxs-lookup"><span data-stu-id="602c5-590">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="602c5-591">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-591">**Why**</span></span>

<span data-ttu-id="602c5-592">Este cambio se ha realizado para que el comportamiento sea coherente y correcto cuando se intenta la carga diferida de una instancia de `DbContext` eliminada.</span><span class="sxs-lookup"><span data-stu-id="602c5-592">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="602c5-593">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-593">**Mitigations**</span></span>

<span data-ttu-id="602c5-594">Actualice el código de la aplicación para que no intente la carga diferida con un contexto eliminado, o bien configúrelo para que sea no operativo, como se describe en el mensaje de la excepción.</span><span class="sxs-lookup"><span data-stu-id="602c5-594">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="602c5-595">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="602c5-595">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="602c5-596">Problema de seguimiento n.º 10236</span><span class="sxs-lookup"><span data-stu-id="602c5-596">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="602c5-597">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="602c5-597">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="602c5-598">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-598">**Old behavior**</span></span>

<span data-ttu-id="602c5-599">Antes de EF Core 3.0, se registraba una advertencia para una aplicación que creaba un número patológico de proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="602c5-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="602c5-600">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-600">**New behavior**</span></span>

<span data-ttu-id="602c5-601">A partir de EF Core 3.0, ahora esta advertencia se considera un error y se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="602c5-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="602c5-602">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-602">**Why**</span></span>

<span data-ttu-id="602c5-603">Este cambio se ha realizado para controlar mejor el código de la aplicación mediante la exposición de este caso patológico de una forma más explícita.</span><span class="sxs-lookup"><span data-stu-id="602c5-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="602c5-604">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-604">**Mitigations**</span></span>

<span data-ttu-id="602c5-605">Cuando se produce este error, la acción más adecuada consiste en comprender la causa raíz y detener la creación de tantos proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="602c5-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="602c5-606">Pero el error se puede convertir en una advertencia (u omitirse) mediante configuración en `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="602c5-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="602c5-607">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="602c5-608">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="602c5-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="602c5-609">Problema de seguimiento n.° 9171</span><span class="sxs-lookup"><span data-stu-id="602c5-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="602c5-610">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-610">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-611">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-611">**Old behavior**</span></span>

<span data-ttu-id="602c5-612">Antes de EF Core 3.0, el código para llamar a `HasOne` o `HasMany` con una cadena se interpretaba de manera confusa.</span><span class="sxs-lookup"><span data-stu-id="602c5-612">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="602c5-613">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-613">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="602c5-614">El código parece relacionar `Samurai` con otro tipo de entidad mediante la propiedad de navegación `Entrance`, que puede ser privada.</span><span class="sxs-lookup"><span data-stu-id="602c5-614">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="602c5-615">En realidad, este código intenta crear una relación con algún tipo de entidad denominada `Entrance` sin ninguna propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="602c5-615">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="602c5-616">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-616">**New behavior**</span></span>

<span data-ttu-id="602c5-617">A partir de EF Core 3.0, el código anterior ahora hace lo que parecía que debía hacer antes.</span><span class="sxs-lookup"><span data-stu-id="602c5-617">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="602c5-618">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-618">**Why**</span></span>

<span data-ttu-id="602c5-619">El comportamiento anterior era muy confuso, especialmente al leer el código de configuración y al buscar errores.</span><span class="sxs-lookup"><span data-stu-id="602c5-619">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="602c5-620">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-620">**Mitigations**</span></span>

<span data-ttu-id="602c5-621">Esto solo interrumpirá las aplicaciones que configuran de manera explícita las relaciones con cadenas para nombres de tipos y sin especificar explícitamente la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="602c5-621">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="602c5-622">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="602c5-622">This is not common.</span></span>
<span data-ttu-id="602c5-623">El comportamiento anterior se puede obtener al pasar de manera explícita `null` para el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="602c5-623">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="602c5-624">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-624">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="602c5-625">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="602c5-625">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="602c5-626">Problema de seguimiento n.º 15184</span><span class="sxs-lookup"><span data-stu-id="602c5-626">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="602c5-627">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-627">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-628">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-628">**Old behavior**</span></span>

<span data-ttu-id="602c5-629">Antes, los siguientes métodos asincrónicos devolvían `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="602c5-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="602c5-630">`ValueGenerator.NextValueAsync()` (y las clases derivadas)</span><span class="sxs-lookup"><span data-stu-id="602c5-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="602c5-631">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-631">**New behavior**</span></span>

<span data-ttu-id="602c5-632">Dichos métodos ahora devuelven `ValueTask<T>` durante el mismo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="602c5-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="602c5-633">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-633">**Why**</span></span>

<span data-ttu-id="602c5-634">Este cambio reduce el número de asignaciones de montones que se producen al invocar estos métodos, lo que mejora el rendimiento general.</span><span class="sxs-lookup"><span data-stu-id="602c5-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="602c5-635">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-635">**Mitigations**</span></span>

<span data-ttu-id="602c5-636">Las aplicaciones que simplemente esperen las API anteriores solo necesitan recompilarse, sin que sea necesario realizar cambios en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="602c5-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="602c5-637">Un uso más complejo (p. ej., pasar el valor `Task` devuelto a `Task.WhenAny()`) normalmente requiere que el valor `ValueTask<T>` devuelto se convierta en `Task<T>` mediante una llamada a `AsTask()` en él.</span><span class="sxs-lookup"><span data-stu-id="602c5-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="602c5-638">Tenga en cuenta que esto niega la reducción de asignación que implica este cambio.</span><span class="sxs-lookup"><span data-stu-id="602c5-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="602c5-639">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="602c5-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="602c5-640">Problema de seguimiento n.º 9913</span><span class="sxs-lookup"><span data-stu-id="602c5-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="602c5-641">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="602c5-641">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="602c5-642">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-642">**Old behavior**</span></span>

<span data-ttu-id="602c5-643">El nombre de anotación para las anotaciones de asignación de tipos era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="602c5-643">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="602c5-644">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-644">**New behavior**</span></span>

<span data-ttu-id="602c5-645">Ahora, el nombre de anotación para las anotaciones de asignación de tipos es "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="602c5-645">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="602c5-646">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-646">**Why**</span></span>

<span data-ttu-id="602c5-647">Ahora, las asignaciones de tipos se usan para algo más que solo para proveedores de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="602c5-647">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="602c5-648">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-648">**Mitigations**</span></span>

<span data-ttu-id="602c5-649">Esto solo interrumpirá a las aplicaciones que acceden directamente a la asignación de tipos como una anotación, lo que no es habitual.</span><span class="sxs-lookup"><span data-stu-id="602c5-649">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="602c5-650">La acción más apropiada para corregir es usar la superficie de API para acceder a las asignaciones de tipos en lugar de usar directamente la anotación.</span><span class="sxs-lookup"><span data-stu-id="602c5-650">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="602c5-651">ToTable en un tipo derivado inicia una excepción</span><span class="sxs-lookup"><span data-stu-id="602c5-651">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="602c5-652">Problema de seguimiento n.º 11811</span><span class="sxs-lookup"><span data-stu-id="602c5-652">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="602c5-653">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="602c5-653">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="602c5-654">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-654">**Old behavior**</span></span>

<span data-ttu-id="602c5-655">Antes de EF Core 3.0, la llamada a `ToTable()` en un tipo derivado se omitía, ya que la única estrategia asignación de herencia era TPH, lo que no es válido.</span><span class="sxs-lookup"><span data-stu-id="602c5-655">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="602c5-656">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-656">**New behavior**</span></span>

<span data-ttu-id="602c5-657">A partir de EF Core 3.0, y en preparación para agregar compatibilidad con TPT y TPC en una versión posterior, ahora la llamada a `ToTable()` en un tipo derivado iniciará una excepción para evitar un cambio de asignación inesperado en el futuro.</span><span class="sxs-lookup"><span data-stu-id="602c5-657">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="602c5-658">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-658">**Why**</span></span>

<span data-ttu-id="602c5-659">En la actualidad no se considera válido asignar un tipo derivado a otra tabla.</span><span class="sxs-lookup"><span data-stu-id="602c5-659">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="602c5-660">Este cambio evita interrupciones en el futuro, cuando se convierta en una operación válida.</span><span class="sxs-lookup"><span data-stu-id="602c5-660">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="602c5-661">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-661">**Mitigations**</span></span>

<span data-ttu-id="602c5-662">Quite todos los intentos de asignar tipos derivados a otras tablas.</span><span class="sxs-lookup"><span data-stu-id="602c5-662">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="602c5-663">ForSqlServerHasIndex se ha reemplazado por HasIndex</span><span class="sxs-lookup"><span data-stu-id="602c5-663">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="602c5-664">Problema de seguimiento n.º 12366</span><span class="sxs-lookup"><span data-stu-id="602c5-664">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="602c5-665">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="602c5-665">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="602c5-666">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-666">**Old behavior**</span></span>

<span data-ttu-id="602c5-667">Antes de EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` proporcionaba una manera de configurar las columnas que se usaban con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="602c5-667">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="602c5-668">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-668">**New behavior**</span></span>

<span data-ttu-id="602c5-669">A partir de EF Core 3.0, ya se admite el uso de `Include` en un índice en el nivel relacional.</span><span class="sxs-lookup"><span data-stu-id="602c5-669">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="602c5-670">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="602c5-670">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="602c5-671">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-671">**Why**</span></span>

<span data-ttu-id="602c5-672">Este cambio se ha realizado para consolidar la API para índices con `Include` en un mismo lugar para todos los proveedores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="602c5-672">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="602c5-673">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-673">**Mitigations**</span></span>

<span data-ttu-id="602c5-674">Use la API nueva, como se ha mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="602c5-674">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="602c5-675">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="602c5-675">Metadata API changes</span></span>

[<span data-ttu-id="602c5-676">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="602c5-676">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="602c5-677">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-678">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-678">**New behavior**</span></span>

<span data-ttu-id="602c5-679">Las siguientes propiedades se han convertido en métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="602c5-679">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="602c5-680">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-680">**Why**</span></span>

<span data-ttu-id="602c5-681">Este cambio simplifica la implementación de las interfaces mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="602c5-681">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="602c5-682">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-682">**Mitigations**</span></span>

<span data-ttu-id="602c5-683">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="602c5-683">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="602c5-684">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="602c5-684">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="602c5-685">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="602c5-685">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="602c5-686">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="602c5-686">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="602c5-687">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-687">**New behavior**</span></span>

<span data-ttu-id="602c5-688">Los métodos de extensión específicos del proveedor se simplificarán:</span><span class="sxs-lookup"><span data-stu-id="602c5-688">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="602c5-689">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-689">**Why**</span></span>

<span data-ttu-id="602c5-690">Este cambio simplifica la implementación de los métodos de extensión mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="602c5-690">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="602c5-691">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-691">**Mitigations**</span></span>

<span data-ttu-id="602c5-692">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="602c5-692">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="602c5-693">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="602c5-693">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="602c5-694">Problema de seguimiento n.º 12151</span><span class="sxs-lookup"><span data-stu-id="602c5-694">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="602c5-695">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="602c5-695">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="602c5-696">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-696">**Old behavior**</span></span>

<span data-ttu-id="602c5-697">Antes de EF Core 3.0, EF Core enviaba `PRAGMA foreign_keys = 1` cuando se abría una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="602c5-697">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="602c5-698">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-698">**New behavior**</span></span>

<span data-ttu-id="602c5-699">A partir de EF Core 3.0, EF Core ya no envía `PRAGMA foreign_keys = 1` cuando se abre una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="602c5-699">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="602c5-700">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-700">**Why**</span></span>

<span data-ttu-id="602c5-701">Este cambio se ha realizado porque en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3` de forma predeterminada, lo que a su vez significa que el cumplimiento de CD está activado de forma predeterminada y no es necesario habilitarlo explícitamente cada vez que se abra una conexión.</span><span class="sxs-lookup"><span data-stu-id="602c5-701">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="602c5-702">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-702">**Mitigations**</span></span>

<span data-ttu-id="602c5-703">Las claves externas se habilitan de forma predeterminada en SQLitePCLRaw.bundle_e_sqlite3, que en EF Core se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="602c5-703">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="602c5-704">Para otros casos, las claves externas se pueden habilitar mediante la especificación de `Foreign Keys=True` en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="602c5-704">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="602c5-705">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="602c5-705">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="602c5-706">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-706">**Old behavior**</span></span>

<span data-ttu-id="602c5-707">Antes de EF Core 3.0, en EF Core se usaba `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="602c5-707">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="602c5-708">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-708">**New behavior**</span></span>

<span data-ttu-id="602c5-709">A partir de EF Core 3.0, en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="602c5-709">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="602c5-710">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-710">**Why**</span></span>

<span data-ttu-id="602c5-711">Este cambio se ha realizado para que la versión de SQLite que se usa en iOS sea coherente con otras plataformas.</span><span class="sxs-lookup"><span data-stu-id="602c5-711">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="602c5-712">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-712">**Mitigations**</span></span>

<span data-ttu-id="602c5-713">Para usar la versión nativa de SQLite en iOS, configure `Microsoft.Data.Sqlite` para usar otra agrupación `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="602c5-713">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="602c5-714">Almacenamiento de valores GUID como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="602c5-714">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="602c5-715">Problema de seguimiento n.º 15078</span><span class="sxs-lookup"><span data-stu-id="602c5-715">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="602c5-716">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-716">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-717">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-717">**Old behavior**</span></span>

<span data-ttu-id="602c5-718">Antes, los valores GUID se almacenaban como valores BLOB en SQLite.</span><span class="sxs-lookup"><span data-stu-id="602c5-718">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="602c5-719">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-719">**New behavior**</span></span>

<span data-ttu-id="602c5-720">Ahora, los valores GUID se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="602c5-720">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="602c5-721">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-721">**Why**</span></span>

<span data-ttu-id="602c5-722">El formato binario de los GUID no está normalizado.</span><span class="sxs-lookup"><span data-stu-id="602c5-722">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="602c5-723">El almacenamiento de los valores como TEXT mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="602c5-723">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="602c5-724">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-724">**Mitigations**</span></span>

<span data-ttu-id="602c5-725">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="602c5-725">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="602c5-726">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="602c5-726">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="602c5-727">Microsoft.Data.Sqlite sigue siendo capaz de leer valores GUID de ambas columnas BLOB y TEXT. Sin embargo, dado que el formato predeterminado de los parámetros y las constantes ha cambiado, seguramente deberá realizar alguna acción en la mayoría de casos que impliquen el uso de valores GUID.</span><span class="sxs-lookup"><span data-stu-id="602c5-727">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="602c5-728">Ahora los valores char se almacenan como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="602c5-728">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="602c5-729">Problema de seguimiento n.º 15020</span><span class="sxs-lookup"><span data-stu-id="602c5-729">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="602c5-730">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-730">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-731">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-731">**Old behavior**</span></span>

<span data-ttu-id="602c5-732">Anteriormente los valores char se almacenaban como valores INTEGER en SQLite.</span><span class="sxs-lookup"><span data-stu-id="602c5-732">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="602c5-733">Por ejemplo, un valor char de *A* se almacenaba como el valor entero 65.</span><span class="sxs-lookup"><span data-stu-id="602c5-733">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="602c5-734">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-734">**New behavior**</span></span>

<span data-ttu-id="602c5-735">Ahora, los valores char se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="602c5-735">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="602c5-736">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-736">**Why**</span></span>

<span data-ttu-id="602c5-737">El almacenamiento de valores como TEXT es más natural y mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="602c5-737">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="602c5-738">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-738">**Mitigations**</span></span>

<span data-ttu-id="602c5-739">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="602c5-739">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="602c5-740">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="602c5-740">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="602c5-741">Microsoft.Data.Sqlite también puede leer valores de caracteres tanto de columnas INTEGER como de columnas TEXT, por lo que es posible que no deba hacer nada dependiendo de su caso.</span><span class="sxs-lookup"><span data-stu-id="602c5-741">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="602c5-742">Ahora los id. de migración se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="602c5-742">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="602c5-743">Problema de seguimiento n.º 12978</span><span class="sxs-lookup"><span data-stu-id="602c5-743">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="602c5-744">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-744">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-745">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-745">**Old behavior**</span></span>

<span data-ttu-id="602c5-746">Los identificadores de migración se generaban de forma involuntaria con el calendario de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="602c5-746">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="602c5-747">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-747">**New behavior**</span></span>

<span data-ttu-id="602c5-748">Ahora los id. de migración siempre se generan usando el calendario de la referencia cultural invariable (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="602c5-748">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="602c5-749">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-749">**Why**</span></span>

<span data-ttu-id="602c5-750">El orden de las migraciones es importante al actualizar la base de datos o al solucionar conflictos de combinación.</span><span class="sxs-lookup"><span data-stu-id="602c5-750">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="602c5-751">Al usar el calendario invariable, se evitan problemas de ordenación que pueden producirse si los miembros del equipo tienen distintos calendarios del sistema.</span><span class="sxs-lookup"><span data-stu-id="602c5-751">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="602c5-752">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-752">**Mitigations**</span></span>

<span data-ttu-id="602c5-753">Esta cambio afecta a todas las personas que usan un calendario no gregoriano en el que el año sea superior al del calendario gregoriano (como el calendario budista tailandés).</span><span class="sxs-lookup"><span data-stu-id="602c5-753">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="602c5-754">Los id. de migración existentes deberán actualizarse para que las migraciones nuevas se ordenen después de las existentes.</span><span class="sxs-lookup"><span data-stu-id="602c5-754">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="602c5-755">Puede ver el id. de migración en el atributo Migration de los archivos de diseñador de la migración.</span><span class="sxs-lookup"><span data-stu-id="602c5-755">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="602c5-756">También debe actualizarse la tabla de historial de migraciones.</span><span class="sxs-lookup"><span data-stu-id="602c5-756">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="602c5-757">Se ha quitado el elemento UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="602c5-757">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="602c5-758">Problema de seguimiento n.º 16400</span><span class="sxs-lookup"><span data-stu-id="602c5-758">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="602c5-759">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="602c5-759">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="602c5-760">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-760">**Old behavior**</span></span>

<span data-ttu-id="602c5-761">Antes de EF Core 3.0, `UseRowNumberForPaging` se podía usar para generar SQL para la paginación de forma que fuera compatible con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="602c5-761">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="602c5-762">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-762">**New behavior**</span></span>

<span data-ttu-id="602c5-763">A partir de EF Core 3.0, EF solo genera SQL para la paginación que únicamente es compatible con las versiones posteriores de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="602c5-763">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="602c5-764">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-764">**Why**</span></span>

<span data-ttu-id="602c5-765">El motivo de este cambio es que [SQL Server 2008 ya no se admite](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/). Además, la actualización de esta característica para que funcionase con los cambios en las consultas implementados en EF Core 3.0 llevaría mucho trabajo.</span><span class="sxs-lookup"><span data-stu-id="602c5-765">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="602c5-766">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-766">**Mitigations**</span></span>

<span data-ttu-id="602c5-767">Se recomienda actualizar a una versión más reciente de SQL Server, o bien utilizar un nivel de compatibilidad superior, de modo que el SQL que se genere se admita.</span><span class="sxs-lookup"><span data-stu-id="602c5-767">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="602c5-768">Dicho esto, si no puede hacerlo, [escriba un comentario en el problema de seguimiento](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con los detalles al respecto.</span><span class="sxs-lookup"><span data-stu-id="602c5-768">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="602c5-769">En función de los comentarios, es posible que volvamos a valorar esta decisión.</span><span class="sxs-lookup"><span data-stu-id="602c5-769">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="602c5-770">La información o metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="602c5-770">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="602c5-771">Problema de seguimiento n.º 16119</span><span class="sxs-lookup"><span data-stu-id="602c5-771">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="602c5-772">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="602c5-772">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="602c5-773">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-773">**Old behavior**</span></span>

<span data-ttu-id="602c5-774">`IDbContextOptionsExtension` incluía métodos para proporcionar metadatos sobre la extensión.</span><span class="sxs-lookup"><span data-stu-id="602c5-774">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="602c5-775">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-775">**New behavior**</span></span>

<span data-ttu-id="602c5-776">Estos métodos se han movido a una nueva clase base abstracta `DbContextOptionsExtensionInfo`, que se devuelve desde una nueva propiedad `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="602c5-776">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="602c5-777">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-777">**Why**</span></span>

<span data-ttu-id="602c5-778">Al lanzarse las versiones 2.0 y 3.0, tuvimos que agregar o cambiar estos métodos varias veces.</span><span class="sxs-lookup"><span data-stu-id="602c5-778">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="602c5-779">Su división en una nueva clase base abstracta facilitará la realización de este tipo de cambios sin interrumpir las extensiones existentes.</span><span class="sxs-lookup"><span data-stu-id="602c5-779">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="602c5-780">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-780">**Mitigations**</span></span>

<span data-ttu-id="602c5-781">Actualice las extensiones para seguir el nuevo patrón.</span><span class="sxs-lookup"><span data-stu-id="602c5-781">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="602c5-782">Encontrará ejemplos en las muchas implementaciones de `IDbContextOptionsExtension` para los diferentes tipos de extensiones en el código fuente de EF Core.</span><span class="sxs-lookup"><span data-stu-id="602c5-782">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="602c5-783">Cambio de nombre de LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="602c5-783">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="602c5-784">Problema de seguimiento n.º 10985</span><span class="sxs-lookup"><span data-stu-id="602c5-784">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="602c5-785">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-785">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-786">**Cambio**</span><span class="sxs-lookup"><span data-stu-id="602c5-786">**Change**</span></span>

<span data-ttu-id="602c5-787">Se ha cambiado el nombre de `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="602c5-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="602c5-788">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-788">**Why**</span></span>

<span data-ttu-id="602c5-789">Conviene alinear el nombre de este evento de advertencia con el del resto de eventos de advertencia.</span><span class="sxs-lookup"><span data-stu-id="602c5-789">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="602c5-790">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-790">**Mitigations**</span></span>

<span data-ttu-id="602c5-791">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="602c5-791">Use the new name.</span></span> <span data-ttu-id="602c5-792">(Tenga en cuenta que el número de id. evento sigue siendo el mismo).</span><span class="sxs-lookup"><span data-stu-id="602c5-792">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="602c5-793">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="602c5-793">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="602c5-794">Problema de seguimiento n.º 10730</span><span class="sxs-lookup"><span data-stu-id="602c5-794">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="602c5-795">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-795">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-796">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-796">**Old behavior**</span></span>

<span data-ttu-id="602c5-797">Antes de EF Core 3.0, se utilizaba simplemente el término "nombre" para hacer referencia a los nombres de las restricciones de claves externas.</span><span class="sxs-lookup"><span data-stu-id="602c5-797">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="602c5-798">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-798">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="602c5-799">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-799">**New behavior**</span></span>

<span data-ttu-id="602c5-800">A partir de EF Core 3.0, el término con el que se hace referencia a los nombres de las restricciones de claves externas es "nombre de la restricción".</span><span class="sxs-lookup"><span data-stu-id="602c5-800">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="602c5-801">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-801">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="602c5-802">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-802">**Why**</span></span>

<span data-ttu-id="602c5-803">Este cambio permite mejorar la coherencia relativa a la nomenclatura en este aspecto y aclarar que se trata del nombre de una restricción de clave externa, y no del de la columna o propiedad en la que está definida la clave externa.</span><span class="sxs-lookup"><span data-stu-id="602c5-803">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="602c5-804">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-804">**Mitigations**</span></span>

<span data-ttu-id="602c5-805">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="602c5-805">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="602c5-806">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="602c5-806">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="602c5-807">Problema de seguimiento n.° 15997</span><span class="sxs-lookup"><span data-stu-id="602c5-807">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="602c5-808">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="602c5-808">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="602c5-809">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-809">**Old behavior**</span></span>

<span data-ttu-id="602c5-810">Antes de EF Core 3.0, estos métodos estaban protegidos.</span><span class="sxs-lookup"><span data-stu-id="602c5-810">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="602c5-811">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-811">**New behavior**</span></span>

<span data-ttu-id="602c5-812">Desde EF Core 3.0, estos métodos son públicos.</span><span class="sxs-lookup"><span data-stu-id="602c5-812">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="602c5-813">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-813">**Why**</span></span>

<span data-ttu-id="602c5-814">EF usa estos métodos para determinar si se ha creado una base de datos, pero está vacía.</span><span class="sxs-lookup"><span data-stu-id="602c5-814">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="602c5-815">Esto también puede resultar útil fuera de EF al determinar si se deben aplicar migraciones o no.</span><span class="sxs-lookup"><span data-stu-id="602c5-815">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="602c5-816">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-816">**Mitigations**</span></span>

<span data-ttu-id="602c5-817">Cambie la accesibilidad de cualquier invalidación.</span><span class="sxs-lookup"><span data-stu-id="602c5-817">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="602c5-818">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="602c5-818">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="602c5-819">Problema de seguimiento n.° 11506</span><span class="sxs-lookup"><span data-stu-id="602c5-819">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="602c5-820">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="602c5-820">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="602c5-821">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-821">**Old behavior**</span></span>

<span data-ttu-id="602c5-822">Antes de EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un paquete NuGet regular con un ensamblado al que podían hacer referencia los proyectos que dependían de él.</span><span class="sxs-lookup"><span data-stu-id="602c5-822">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="602c5-823">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-823">**New behavior**</span></span>

<span data-ttu-id="602c5-824">Desde EF Core 3.0, es un paquete DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="602c5-824">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="602c5-825">Esto significa que la dependencia no fluirá de manera transitiva en otros proyectos y que ya no puede, de forma predeterminada, hacer referencia a su ensamblado.</span><span class="sxs-lookup"><span data-stu-id="602c5-825">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="602c5-826">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-826">**Why**</span></span>

<span data-ttu-id="602c5-827">Este paquete solo está destinado a usarse en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="602c5-827">This package is only intended to be used at design time.</span></span> <span data-ttu-id="602c5-828">Las aplicaciones implementadas no deben hacer referencia al mismo.</span><span class="sxs-lookup"><span data-stu-id="602c5-828">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="602c5-829">Hacer que el paquete sea DevelopmentDependency refuerza esta recomendación.</span><span class="sxs-lookup"><span data-stu-id="602c5-829">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="602c5-830">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-830">**Mitigations**</span></span>

<span data-ttu-id="602c5-831">Si tiene que hacer referencia a este paquete para invalidar el comportamiento en tiempo de diseño de EF Core, puede actualizar los metadatos de elementos PackageReference del proyecto.</span><span class="sxs-lookup"><span data-stu-id="602c5-831">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="602c5-832">Si se hace referencia al paquete de manera transitiva a través de Microsoft.EntityFrameworkCore.Tools, tendrá que agregar una PackageReference explícita al paquete para cambiar sus metadatos.</span><span class="sxs-lookup"><span data-stu-id="602c5-832">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="602c5-833">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="602c5-833">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="602c5-834">Problema de seguimiento n.° 14824</span><span class="sxs-lookup"><span data-stu-id="602c5-834">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="602c5-835">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="602c5-835">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="602c5-836">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-836">**Old behavior**</span></span>

<span data-ttu-id="602c5-837">Microsoft.EntityFrameworkCore.Sqlite dependía anteriormente de la versión 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="602c5-837">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="602c5-838">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-838">**New behavior**</span></span>

<span data-ttu-id="602c5-839">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="602c5-839">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="602c5-840">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-840">**Why**</span></span>

<span data-ttu-id="602c5-841">La versión 2.0.0 de SQLitePCL.raw selecciona .NET Standard 2.0 como destino.</span><span class="sxs-lookup"><span data-stu-id="602c5-841">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="602c5-842">Anteriormente seleccionaba .NET Standard 1.1 como destino, que requería el cierre a gran escala de paquetes transitivos para su funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="602c5-842">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="602c5-843">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-843">**Mitigations**</span></span>

<span data-ttu-id="602c5-844">En la versión 2.0.0 de SQLitePCL.raw se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="602c5-844">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="602c5-845">Consulte las [notas de la versión](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="602c5-845">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="602c5-846">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="602c5-846">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="602c5-847">Problema de seguimiento n.° 14825</span><span class="sxs-lookup"><span data-stu-id="602c5-847">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="602c5-848">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="602c5-848">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="602c5-849">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-849">**Old behavior**</span></span>

<span data-ttu-id="602c5-850">Los paquetes espaciales anteriormente dependían de la versión 1.15.1 de NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="602c5-850">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="602c5-851">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-851">**New behavior**</span></span>

<span data-ttu-id="602c5-852">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="602c5-852">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="602c5-853">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-853">**Why**</span></span>

<span data-ttu-id="602c5-854">La versión 2.0.0 de NetTopologySuite pretende resolver varios problemas de usabilidad que encontraron los usuarios de EF Core.</span><span class="sxs-lookup"><span data-stu-id="602c5-854">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="602c5-855">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-855">**Mitigations**</span></span>

<span data-ttu-id="602c5-856">En la versión 2.0.0 de NetTopologySuite se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="602c5-856">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="602c5-857">Consulte las [notas de la versión](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="602c5-857">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="602c5-858">Se deben configurar varias relaciones de referencia automática ambiguas</span><span class="sxs-lookup"><span data-stu-id="602c5-858">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="602c5-859">Problema de seguimiento n.º 13573</span><span class="sxs-lookup"><span data-stu-id="602c5-859">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="602c5-860">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="602c5-860">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="602c5-861">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="602c5-861">**Old behavior**</span></span>

<span data-ttu-id="602c5-862">Un tipo de entidad con varias propiedades de navegación unidireccional de referencia automática y claves externas coincidentes se configuró incorrectamente como una única relación.</span><span class="sxs-lookup"><span data-stu-id="602c5-862">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="602c5-863">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-863">For example:</span></span>

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="602c5-864">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="602c5-864">**New behavior**</span></span>

<span data-ttu-id="602c5-865">Este escenario se detecta ahora en la generación del modelo y se produce una excepción que indica que el modelo es ambiguo.</span><span class="sxs-lookup"><span data-stu-id="602c5-865">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="602c5-866">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="602c5-866">**Why**</span></span>

<span data-ttu-id="602c5-867">El modelo resultante era ambiguo, y lo más probable es que sea incorrecto en este caso.</span><span class="sxs-lookup"><span data-stu-id="602c5-867">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="602c5-868">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="602c5-868">**Mitigations**</span></span>

<span data-ttu-id="602c5-869">Utilice la configuración completa de la relación.</span><span class="sxs-lookup"><span data-stu-id="602c5-869">Use full configuration of the relationship.</span></span> <span data-ttu-id="602c5-870">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="602c5-870">For example:</span></span>

```C#
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```
