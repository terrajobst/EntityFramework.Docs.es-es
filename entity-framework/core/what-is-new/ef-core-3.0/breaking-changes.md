---
title: 'Cambios importantes en EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 0dd4c5c4aa1a5d241fb48abf1372a678d0f7a7a3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813621"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="bc61f-102">Cambios importantes incluidos en EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="bc61f-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="bc61f-103">Es posible que los siguientes cambios de API y comportamiento interrumpan las aplicaciones actuales cuando se actualicen a la versión 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="bc61f-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="bc61f-104">Los cambios que esperamos que solo afecten a proveedores de base de datos se documentan en [Cambios para proveedores](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="bc61f-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="bc61f-105">Resumen</span><span class="sxs-lookup"><span data-stu-id="bc61f-105">Summary</span></span>

| <span data-ttu-id="bc61f-106">**Cambio importante**</span><span class="sxs-lookup"><span data-stu-id="bc61f-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="bc61f-107">**Impacto**</span><span class="sxs-lookup"><span data-stu-id="bc61f-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="bc61f-108">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="bc61f-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="bc61f-109">Alto</span><span class="sxs-lookup"><span data-stu-id="bc61f-109">High</span></span>       |
| [<span data-ttu-id="bc61f-110">EF Core 3.0 tiene como destino .NET Standard 2.1, y no .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="bc61f-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="bc61f-111">Alto</span><span class="sxs-lookup"><span data-stu-id="bc61f-111">High</span></span>      |
| [<span data-ttu-id="bc61f-112">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="bc61f-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="bc61f-113">Alto</span><span class="sxs-lookup"><span data-stu-id="bc61f-113">High</span></span>      |
| [<span data-ttu-id="bc61f-114">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="bc61f-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="bc61f-115">Alto</span><span class="sxs-lookup"><span data-stu-id="bc61f-115">High</span></span>      |
| [<span data-ttu-id="bc61f-116">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="bc61f-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="bc61f-117">Alto</span><span class="sxs-lookup"><span data-stu-id="bc61f-117">High</span></span>      |
| [<span data-ttu-id="bc61f-118">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="bc61f-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="bc61f-119">Alto</span><span class="sxs-lookup"><span data-stu-id="bc61f-119">High</span></span>      |
| [<span data-ttu-id="bc61f-120">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc61f-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="bc61f-121">Medium</span><span class="sxs-lookup"><span data-stu-id="bc61f-121">Medium</span></span>      |
| [<span data-ttu-id="bc61f-122">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="bc61f-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="bc61f-123">Medium</span><span class="sxs-lookup"><span data-stu-id="bc61f-123">Medium</span></span>      |
| [<span data-ttu-id="bc61f-124">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="bc61f-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="bc61f-125">Medium</span><span class="sxs-lookup"><span data-stu-id="bc61f-125">Medium</span></span>      |
| [<span data-ttu-id="bc61f-126">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="bc61f-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="bc61f-127">Medium</span><span class="sxs-lookup"><span data-stu-id="bc61f-127">Medium</span></span>      |
| [<span data-ttu-id="bc61f-128">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="bc61f-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="bc61f-129">Medium</span><span class="sxs-lookup"><span data-stu-id="bc61f-129">Medium</span></span>      |
| [<span data-ttu-id="bc61f-130">Las consultas sin seguimiento ya no realizan la resolución de la identidad</span><span class="sxs-lookup"><span data-stu-id="bc61f-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="bc61f-131">Medium</span><span class="sxs-lookup"><span data-stu-id="bc61f-131">Medium</span></span>      |
| [<span data-ttu-id="bc61f-132">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="bc61f-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="bc61f-133">Medium</span><span class="sxs-lookup"><span data-stu-id="bc61f-133">Medium</span></span>      |
| [<span data-ttu-id="bc61f-134">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="bc61f-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="bc61f-135">Medium</span><span class="sxs-lookup"><span data-stu-id="bc61f-135">Medium</span></span>      |
| [<span data-ttu-id="bc61f-136">Se ha quitado el elemento UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="bc61f-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="bc61f-137">Medium</span><span class="sxs-lookup"><span data-stu-id="bc61f-137">Medium</span></span>      |
| [<span data-ttu-id="bc61f-138">Solo se pueden especificar métodos de FromSql en raíces de consulta</span><span class="sxs-lookup"><span data-stu-id="bc61f-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="bc61f-139">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-139">Low</span></span>      |
| [<span data-ttu-id="bc61f-140">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="bc61f-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="bc61f-141">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-141">Low</span></span>      |
| [<span data-ttu-id="bc61f-142">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="bc61f-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="bc61f-143">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-143">Low</span></span>      |
| [<span data-ttu-id="bc61f-144">Las entidades dependientes que comparten la tabla con la entidad de seguridad son ahora opcionales</span><span class="sxs-lookup"><span data-stu-id="bc61f-144">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="bc61f-145">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-145">Low</span></span>      |
| [<span data-ttu-id="bc61f-146">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="bc61f-146">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="bc61f-147">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-147">Low</span></span>      |
| [<span data-ttu-id="bc61f-148">Las propiedades heredadas de tipos sin asignar se asignan ahora a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="bc61f-148">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="bc61f-149">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-149">Low</span></span>      |
| [<span data-ttu-id="bc61f-150">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="bc61f-150">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="bc61f-151">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-151">Low</span></span>      |
| [<span data-ttu-id="bc61f-152">La conexión de base de datos ahora se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="bc61f-152">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="bc61f-153">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-153">Low</span></span>      |
| [<span data-ttu-id="bc61f-154">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="bc61f-154">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="bc61f-155">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-155">Low</span></span>      |
| [<span data-ttu-id="bc61f-156">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="bc61f-156">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="bc61f-157">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-157">Low</span></span>      |
| [<span data-ttu-id="bc61f-158">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="bc61f-158">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="bc61f-159">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-159">Low</span></span>      |
| [<span data-ttu-id="bc61f-160">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="bc61f-160">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="bc61f-161">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-161">Low</span></span>      |
| [<span data-ttu-id="bc61f-162">DbContext.Entry realiza ahora una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="bc61f-162">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="bc61f-163">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-163">Low</span></span>      |
| [<span data-ttu-id="bc61f-164">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="bc61f-164">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="bc61f-165">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-165">Low</span></span>      |
| [<span data-ttu-id="bc61f-166">ILoggerFactory es ahora un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="bc61f-166">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="bc61f-167">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-167">Low</span></span>      |
| [<span data-ttu-id="bc61f-168">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="bc61f-168">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="bc61f-169">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-169">Low</span></span>      |
| [<span data-ttu-id="bc61f-170">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="bc61f-170">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="bc61f-171">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-171">Low</span></span>      |
| [<span data-ttu-id="bc61f-172">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="bc61f-172">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="bc61f-173">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-173">Low</span></span>      |
| [<span data-ttu-id="bc61f-174">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="bc61f-174">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="bc61f-175">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-175">Low</span></span>      |
| [<span data-ttu-id="bc61f-176">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="bc61f-176">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="bc61f-177">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-177">Low</span></span>      |
| [<span data-ttu-id="bc61f-178">ToTable en un tipo derivado produce una excepción</span><span class="sxs-lookup"><span data-stu-id="bc61f-178">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="bc61f-179">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-179">Low</span></span>      |
| [<span data-ttu-id="bc61f-180">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="bc61f-180">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="bc61f-181">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-181">Low</span></span>      |
| [<span data-ttu-id="bc61f-182">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="bc61f-182">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="bc61f-183">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-183">Low</span></span>      |
| [<span data-ttu-id="bc61f-184">Los valores GUID se almacenan ahora como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="bc61f-184">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="bc61f-185">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-185">Low</span></span>      |
| [<span data-ttu-id="bc61f-186">Los valores char se almacenan ahora como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="bc61f-186">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="bc61f-187">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-187">Low</span></span>      |
| [<span data-ttu-id="bc61f-188">Los id. de migración ahora se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="bc61f-188">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="bc61f-189">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-189">Low</span></span>      |
| [<span data-ttu-id="bc61f-190">La información o los metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="bc61f-190">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="bc61f-191">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-191">Low</span></span>      |
| [<span data-ttu-id="bc61f-192">LogQueryPossibleExceptionWithAggregateOperator ha cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="bc61f-192">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="bc61f-193">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-193">Low</span></span>      |
| [<span data-ttu-id="bc61f-194">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="bc61f-194">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="bc61f-195">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-195">Low</span></span>      |
| [<span data-ttu-id="bc61f-196">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="bc61f-196">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="bc61f-197">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-197">Low</span></span>      |
| [<span data-ttu-id="bc61f-198">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="bc61f-198">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="bc61f-199">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-199">Low</span></span>      |
| [<span data-ttu-id="bc61f-200">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bc61f-200">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="bc61f-201">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-201">Low</span></span>      |
| [<span data-ttu-id="bc61f-202">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bc61f-202">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="bc61f-203">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-203">Low</span></span>      |
| [<span data-ttu-id="bc61f-204">Se deben configurar varias relaciones de referencia automática ambiguas</span><span class="sxs-lookup"><span data-stu-id="bc61f-204">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="bc61f-205">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-205">Low</span></span>      |
| [<span data-ttu-id="bc61f-206">DbFunction.Schema es NULL o la cadena vacía lo configura para estar en el esquema predeterminado del modelo</span><span class="sxs-lookup"><span data-stu-id="bc61f-206">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="bc61f-207">Bajo</span><span class="sxs-lookup"><span data-stu-id="bc61f-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="bc61f-208">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="bc61f-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="bc61f-209">[Problema de seguimiento n.° 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Consulte también el problema n.° 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="bc61f-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="bc61f-210">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-210">**Old behavior**</span></span>

<span data-ttu-id="bc61f-211">Antes de 3.0, cuando en EF Core no se podía convertir una expresión que formaba parte de una consulta SQL o un parámetro, la expresión se evaluaba de forma automática en el cliente.</span><span class="sxs-lookup"><span data-stu-id="bc61f-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="bc61f-212">De forma predeterminada, la evaluación de cliente de expresiones potencialmente costosas solo desencadenaba una advertencia.</span><span class="sxs-lookup"><span data-stu-id="bc61f-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="bc61f-213">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-213">**New behavior**</span></span>

<span data-ttu-id="bc61f-214">A partir de 3.0, en EF Core solo se permite que se evalúen en el cliente las expresiones en la proyección de nivel superior (la última llamada a `Select()` de la consulta).</span><span class="sxs-lookup"><span data-stu-id="bc61f-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="bc61f-215">Cuando las expresiones de otra parte de la consulta no se pueden convertir en SQL o un parámetro, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="bc61f-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="bc61f-216">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-216">**Why**</span></span>

<span data-ttu-id="bc61f-217">La evaluación de cliente automática de las consultas permite que se ejecuten muchas consultas incluso si no se pueden convertir elementos importantes de ellas.</span><span class="sxs-lookup"><span data-stu-id="bc61f-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="bc61f-218">Esto puede provocar un comportamiento inesperado y potencialmente dañino que es posible que solo sea evidente en entornos de producción.</span><span class="sxs-lookup"><span data-stu-id="bc61f-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="bc61f-219">Por ejemplo, una condición en una llamada a `Where()` que no se puede convertir puede provocar que todas las filas de la tabla se transfieran desde el servidor de base de datos y que el filtro se aplique en el cliente.</span><span class="sxs-lookup"><span data-stu-id="bc61f-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="bc61f-220">Esta situación puede pasar desapercibida fácilmente si la tabla solo contiene algunas filas en la fase de desarrollo, pero ser más grave cuando la aplicación pase a producción, donde la tabla puede contener millones de filas.</span><span class="sxs-lookup"><span data-stu-id="bc61f-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="bc61f-221">Las advertencias de evaluación de cliente también se suelen pasar por alto durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="bc61f-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="bc61f-222">Además de esto, la evaluación de cliente automática puede causar problemas en los que la mejora de la traducción de consultas para expresiones específicas provocaba cambios importantes no deseados entre versiones.</span><span class="sxs-lookup"><span data-stu-id="bc61f-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="bc61f-223">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-223">**Mitigations**</span></span>

<span data-ttu-id="bc61f-224">Si una consulta no se puede traducir totalmente, vuelva a escribirla en un formato que se pueda traducir, o bien use `AsEnumerable()`, `ToList()` o una función similar para devolver los datos al cliente de forma explícita, donde después se puedan seguir procesando mediante LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="bc61f-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="bc61f-225">EF Core 3.0 tiene como destino .NET Standard 2.1, y no .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="bc61f-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="bc61f-226">Problema de seguimiento n.º 15498</span><span class="sxs-lookup"><span data-stu-id="bc61f-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="bc61f-227">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-227">**Old behavior**</span></span>

<span data-ttu-id="bc61f-228">Antes de la versión 3.0, EF Core tenía como destino .NET Standard 2.0 y se podía ejecutar en todas las plataformas que admitieran dicho estándar, incluido .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bc61f-228">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="bc61f-229">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-229">**New behavior**</span></span>

<span data-ttu-id="bc61f-230">A partir de la versión 3.0, EF Core tiene como destino .NET Standard 2.1 y se puede ejecutar en todas las plataformas que admitan dicho estándar.</span><span class="sxs-lookup"><span data-stu-id="bc61f-230">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="bc61f-231">Esto no incluye .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bc61f-231">This does not include .NET Framework.</span></span>

<span data-ttu-id="bc61f-232">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-232">**Why**</span></span>

<span data-ttu-id="bc61f-233">Esto forma parte de una decisión estratégica para todas las tecnologías de .NET que tiene como objetivo centrar los esfuerzos en .NET Core y otras plataformas modernas de .NET, como Xamarin.</span><span class="sxs-lookup"><span data-stu-id="bc61f-233">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="bc61f-234">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-234">**Mitigations**</span></span>

<span data-ttu-id="bc61f-235">Valore la posibilidad de cambiar a una plataforma moderna de .NET.</span><span class="sxs-lookup"><span data-stu-id="bc61f-235">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="bc61f-236">Si esto no es posible, siga usando EF Core 2.1 o EF Core 2.2, puesto que ambas versiones admiten .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bc61f-236">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="bc61f-237">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc61f-237">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="bc61f-238">Anuncios del problema de seguimiento n.º 325</span><span class="sxs-lookup"><span data-stu-id="bc61f-238">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="bc61f-239">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-239">**Old behavior**</span></span>

<span data-ttu-id="bc61f-240">Antes de ASP.NET Core 3.0, cuando se agregaba una referencia de paquete a `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, se incluía EF Core y algunos de los proveedores de datos de EF Core, como el de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bc61f-240">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="bc61f-241">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-241">**New behavior**</span></span>

<span data-ttu-id="bc61f-242">A partir de la versión 3.0, el marco compartido ASP.NET Core no incluye EF Core ni ningún proveedor de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="bc61f-242">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="bc61f-243">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-243">**Why**</span></span>

<span data-ttu-id="bc61f-244">Antes de este cambio, para obtener EF Core se necesitaban varios pasos en función de si la aplicación se destinaba a ASP.NET Core y SQL Server o no.</span><span class="sxs-lookup"><span data-stu-id="bc61f-244">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="bc61f-245">Además, la actualización de ASP.NET Core forzaba la de EF Core y el proveedor de SQL Server, lo que no siempre es deseable.</span><span class="sxs-lookup"><span data-stu-id="bc61f-245">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="bc61f-246">Con este cambio, la experiencia de obtención de EF Core es la misma en todos los proveedores, implementaciones admitidas de .NET y tipos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-246">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="bc61f-247">Ahora los desarrolladores también pueden controlar exactamente cuándo se actualizan EF Core y los proveedores de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="bc61f-247">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="bc61f-248">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-248">**Mitigations**</span></span>

<span data-ttu-id="bc61f-249">Para usar EF Core en una aplicación ASP.NET Core 3.0 o cualquier otra aplicación compatible, debe agregar de forma explícita una referencia de paquete al proveedor de base de datos de EF Core que se va a usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-249">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="bc61f-250">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="bc61f-250">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="bc61f-251">Problema de seguimiento n.º 14016</span><span class="sxs-lookup"><span data-stu-id="bc61f-251">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="bc61f-252">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-252">**Old behavior**</span></span>

<span data-ttu-id="bc61f-253">Antes de 3.0, la herramienta `dotnet ef` se incluía en el SDK de .NET Core y estaba disponible para usarse desde la línea de comandos de cualquier proyecto sin necesidad de realizar pasos adicionales.</span><span class="sxs-lookup"><span data-stu-id="bc61f-253">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="bc61f-254">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-254">**New behavior**</span></span>

<span data-ttu-id="bc61f-255">A partir de la versión 3.0, el SDK de .NET no incluye la herramienta `dotnet ef`, por lo que antes de poder usarla tendrá que instalarla de forma explícita como una herramienta local o global.</span><span class="sxs-lookup"><span data-stu-id="bc61f-255">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="bc61f-256">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-256">**Why**</span></span>

<span data-ttu-id="bc61f-257">Este cambio nos permite distribuir y actualizar `dotnet ef` como una herramienta convencional de la CLI de .NET en NuGet, coherente con el hecho de que la versión 3.0 de EF Core también se distribuye siempre como un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="bc61f-257">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="bc61f-258">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-258">**Mitigations**</span></span>

<span data-ttu-id="bc61f-259">Para poder administrar las migraciones o aplicar scaffolding a `DbContext`, instale `dotnet-ef` como herramienta global:</span><span class="sxs-lookup"><span data-stu-id="bc61f-259">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="bc61f-260">También se puede obtener una herramienta local cuando se restauran las dependencias de un proyecto que la declara como una dependencia de herramientas mediante un [archivo de manifiesto de herramientas](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="bc61f-260">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="bc61f-261">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="bc61f-261">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="bc61f-262">Problema de seguimiento n.º 10996</span><span class="sxs-lookup"><span data-stu-id="bc61f-262">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="bc61f-263">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-263">**Old behavior**</span></span>

<span data-ttu-id="bc61f-264">Antes de EF Core 3.0, estos nombres de métodos se sobrecargaban para funcionar tanto con una cadena normal como con una cadena que se debería interpolar en SQL y parámetros.</span><span class="sxs-lookup"><span data-stu-id="bc61f-264">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="bc61f-265">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-265">**New behavior**</span></span>

<span data-ttu-id="bc61f-266">A partir de la versión EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw` y `ExecuteSqlRawAsync` para crear una consulta con parámetros donde los parámetros se pasan por separado de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="bc61f-266">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="bc61f-267">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-267">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="bc61f-268">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated` y `ExecuteSqlInterpolatedAsync` para crear una consulta con parámetros donde los parámetros se pasan como parte de una cadena de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="bc61f-268">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="bc61f-269">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-269">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="bc61f-270">Tenga en cuenta que las dos consultas anteriores producirán el mismo código SQL parametrizado con los mismos parámetros SQL.</span><span class="sxs-lookup"><span data-stu-id="bc61f-270">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="bc61f-271">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-271">**Why**</span></span>

<span data-ttu-id="bc61f-272">Las sobrecargas del método como esta facilitan las llamadas accidentales al método de cadena sin procesar cuando la intención era llamar al método de cadena interpolada y viceversa.</span><span class="sxs-lookup"><span data-stu-id="bc61f-272">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="bc61f-273">Esto podría resultar en consultas que no se parametrizan cuando deberían.</span><span class="sxs-lookup"><span data-stu-id="bc61f-273">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="bc61f-274">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-274">**Mitigations**</span></span>

<span data-ttu-id="bc61f-275">Haga el cambio para usar los nuevos nombres de métodos.</span><span class="sxs-lookup"><span data-stu-id="bc61f-275">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="bc61f-276">Solo se pueden especificar métodos de FromSql en raíces de consulta.</span><span class="sxs-lookup"><span data-stu-id="bc61f-276">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="bc61f-277">Problema de seguimiento n.° 15704</span><span class="sxs-lookup"><span data-stu-id="bc61f-277">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="bc61f-278">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-278">**Old behavior**</span></span>

<span data-ttu-id="bc61f-279">Antes de EF Core 3.0, el método `FromSql` podía especificarse en cualquier lugar en la consulta.</span><span class="sxs-lookup"><span data-stu-id="bc61f-279">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="bc61f-280">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-280">**New behavior**</span></span>

<span data-ttu-id="bc61f-281">A partir de EF Core 3.0, los nuevos métodos `FromSqlRaw` y `FromSqlInterpolated` (que reemplazan a `FromSql`) solo pueden especificarse en las raíces de la consulta, es decir, directamente en `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-281">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="bc61f-282">Si intenta especificarlos en cualquier otro lugar se producirá un error de compilación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-282">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="bc61f-283">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-283">**Why**</span></span>

<span data-ttu-id="bc61f-284">La especificación de `FromSql` en cualquier otro lugar diferente de `DbSet` no tenía un significado o valor agregado, y podría causar ambigüedad en ciertos escenarios.</span><span class="sxs-lookup"><span data-stu-id="bc61f-284">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="bc61f-285">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-285">**Mitigations**</span></span>

<span data-ttu-id="bc61f-286">Las invocaciones de `FromSql` se deben mover para que estén directamente en el `DbSet` al que se aplican.</span><span class="sxs-lookup"><span data-stu-id="bc61f-286">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="bc61f-287">Las consultas sin seguimiento ya no realizan la resolución de la identidad</span><span class="sxs-lookup"><span data-stu-id="bc61f-287">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="bc61f-288">Problema de seguimiento n.º 13518</span><span class="sxs-lookup"><span data-stu-id="bc61f-288">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="bc61f-289">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-289">**Old behavior**</span></span>

<span data-ttu-id="bc61f-290">Antes de EF Core 3.0, se usaba la misma instancia de la entidad para cada aparición de una entidad con un tipo e identificador determinados.</span><span class="sxs-lookup"><span data-stu-id="bc61f-290">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="bc61f-291">Este comportamiento coincide con el de las consultas de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="bc61f-291">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="bc61f-292">Por ejemplo, esta consulta:</span><span class="sxs-lookup"><span data-stu-id="bc61f-292">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="bc61f-293">Esta consulta devolverá la misma instancia de `Category` para cada elemento `Product` asociado con la categoría determinada.</span><span class="sxs-lookup"><span data-stu-id="bc61f-293">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="bc61f-294">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-294">**New behavior**</span></span>

<span data-ttu-id="bc61f-295">A partir de EF Core 3.0, se crean distintas instancias de la entidad si se encuentra una entidad con un tipo e identificador determinados en varias ubicaciones del gráfico devuelto.</span><span class="sxs-lookup"><span data-stu-id="bc61f-295">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="bc61f-296">Por ejemplo, la consulta anterior ahora devolverá una nueva instancia de `Category` para cada elemento `Product` cuando haya dos productos asociados a la misma categoría.</span><span class="sxs-lookup"><span data-stu-id="bc61f-296">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="bc61f-297">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-297">**Why**</span></span>

<span data-ttu-id="bc61f-298">La resolución de las identidades (es decir, el hecho de determinar que una entidad tiene los mismos tipo e identificador que la entidad encontrada) agrega más rendimiento y sobrecarga de memoria.</span><span class="sxs-lookup"><span data-stu-id="bc61f-298">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="bc61f-299">Este enfoque suele ser contrario a por qué las consultas sin seguimiento se usan en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="bc61f-299">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="bc61f-300">Además, aunque la resolución de las identidades a veces puede resultar útil, no es necesaria si las entidades se van a serializar y enviar a un cliente, algo habitual para las consultas sin seguimiento.</span><span class="sxs-lookup"><span data-stu-id="bc61f-300">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="bc61f-301">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-301">**Mitigations**</span></span>

<span data-ttu-id="bc61f-302">Si se requiere la resolución de identidad, use una consulta de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="bc61f-302">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="bc61f-303">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="bc61f-303">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="bc61f-304">Problema de seguimiento n.º 14523</span><span class="sxs-lookup"><span data-stu-id="bc61f-304">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="bc61f-305">Revertimos este cambio porque la nueva configuración de EF Core 3.0 permite a la aplicación especificar el nivel de registro para cualquier evento.</span><span class="sxs-lookup"><span data-stu-id="bc61f-305">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="bc61f-306">Por ejemplo, para cambiar el registro de SQL a `Debug`, configure el nivel de forma explícita en `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="bc61f-306">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="bc61f-307">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="bc61f-307">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="bc61f-308">Problema de seguimiento n.º 12378</span><span class="sxs-lookup"><span data-stu-id="bc61f-308">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="bc61f-309">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-309">**Old behavior**</span></span>

<span data-ttu-id="bc61f-310">Antes de EF Core 3.0, los valores temporales se asignaban a todas las propiedades de clave para las que posteriormente la base de datos generaba un valor real.</span><span class="sxs-lookup"><span data-stu-id="bc61f-310">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="bc61f-311">Normalmente, estos valores temporales eran números negativos grandes.</span><span class="sxs-lookup"><span data-stu-id="bc61f-311">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="bc61f-312">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-312">**New behavior**</span></span>

<span data-ttu-id="bc61f-313">A partir de la versión 3.0, en EF Core se almacena el valor de clave temporal como parte de la información de seguimiento de la entidad y la propiedad clave en sí no se modifica.</span><span class="sxs-lookup"><span data-stu-id="bc61f-313">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="bc61f-314">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-314">**Why**</span></span>

<span data-ttu-id="bc61f-315">Este cambio se ha realizado para evitar que los valores de clave temporales se conviertan erróneamente en permanentes cuando una entidad de la que previamente una instancia de `DbContext` ha realizado el seguimiento se mueve a otra instancia de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-315">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="bc61f-316">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-316">**Mitigations**</span></span>

<span data-ttu-id="bc61f-317">Las aplicaciones que asignan valores de clave principal a claves externas para crear asociaciones entre entidades pueden depender del comportamiento anterior si las claves principales son generadas por el almacén y pertenecen a entidades en el estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-317">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="bc61f-318">Esto se puede evitar con las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="bc61f-318">This can be avoided by:</span></span>
* <span data-ttu-id="bc61f-319">No se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="bc61f-319">Not using store-generated keys.</span></span>
* <span data-ttu-id="bc61f-320">Se establecen propiedades de navegación para crear relaciones en lugar de establecer valores de clave externa.</span><span class="sxs-lookup"><span data-stu-id="bc61f-320">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="bc61f-321">Se obtienen los valores de clave temporal reales de la información de seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="bc61f-321">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="bc61f-322">Por ejemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` devolverá el valor temporal aunque no se haya establecido `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-322">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="bc61f-323">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="bc61f-323">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="bc61f-324">Problema de seguimiento n.º 14616</span><span class="sxs-lookup"><span data-stu-id="bc61f-324">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="bc61f-325">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-325">**Old behavior**</span></span>

<span data-ttu-id="bc61f-326">Antes de EF Core 3.0, se realizaba el seguimiento en el estado `Added` de las entidades sin seguimiento detectadas por `DetectChanges` y se insertaban como una fila nueva cuando se llamaba a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-326">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="bc61f-327">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-327">**New behavior**</span></span>

<span data-ttu-id="bc61f-328">A partir de EF Core 3.0, si una entidad usa valores de clave generados y se establece un valor de clave, se realizará el seguimiento de la entidad en el estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-328">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="bc61f-329">Esto significa que se supone que existe una fila para la entidad y que se actualizará cuando se llame a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-329">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="bc61f-330">Si no se establece el valor de clave, o bien si el tipo de entidad no usa claves generadas, se seguirá realizando el seguimiento de la entidad nueva como `Added` al igual que en las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="bc61f-330">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="bc61f-331">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-331">**Why**</span></span>

<span data-ttu-id="bc61f-332">Este cambio se ha realizado para que sea más sencillo y coherente trabajar con gráficos de entidades desconectadas mientras se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="bc61f-332">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="bc61f-333">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-333">**Mitigations**</span></span>

<span data-ttu-id="bc61f-334">Este cambio puede interrumpir una aplicación si se configura un tipo de entidad para usar claves generadas, pero se establecen de forma explícita valores de clave para las instancias nuevas.</span><span class="sxs-lookup"><span data-stu-id="bc61f-334">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="bc61f-335">La solución consiste en configurar de forma explícita las propiedades de clave para que no usen valores generados.</span><span class="sxs-lookup"><span data-stu-id="bc61f-335">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="bc61f-336">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="bc61f-336">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="bc61f-337">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="bc61f-337">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="bc61f-338">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="bc61f-338">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="bc61f-339">Problema de seguimiento n.º 10114</span><span class="sxs-lookup"><span data-stu-id="bc61f-339">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="bc61f-340">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-340">**Old behavior**</span></span>

<span data-ttu-id="bc61f-341">Antes de la versión 3.0, en EF Core no se aplicaban acciones en cascada (eliminación de entidades dependientes cuando se eliminaba una entidad de seguridad obligatoria o cuando se rompía la relación con una entidad de seguridad obligatoria) hasta que se llamaba a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="bc61f-341">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="bc61f-342">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-342">**New behavior**</span></span>

<span data-ttu-id="bc61f-343">A partir de 3.0, en EF Core las acciones en cascada se aplican en cuanto se detecta la condición desencadenadora.</span><span class="sxs-lookup"><span data-stu-id="bc61f-343">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="bc61f-344">Por ejemplo, como resultado de la llamada a `context.Remove()` para eliminar una entidad de seguridad, todos los dependientes obligatorios relacionados de los que se realiza el seguimiento también se establecen en `Deleted` inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="bc61f-344">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="bc61f-345">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-345">**Why**</span></span>

<span data-ttu-id="bc61f-346">Este cambio se ha realizado para mejorar la experiencia en escenarios de auditoría y enlace de datos donde es importante comprender qué entidades se van a eliminar _antes_ de llamar a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-346">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="bc61f-347">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-347">**Mitigations**</span></span>

<span data-ttu-id="bc61f-348">El comportamiento anterior se puede restaurar mediante opciones de `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-348">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="bc61f-349">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-349">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="bc61f-350">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="bc61f-350">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="bc61f-351">Problema de seguimiento n.º 12661</span><span class="sxs-lookup"><span data-stu-id="bc61f-351">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="bc61f-352">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-352">**Old behavior**</span></span>

<span data-ttu-id="bc61f-353">Antes de la versión 3.0, `DeleteBehavior.Restrict` creaba claves externas en la base de datos con la semántica `Restrict`, pero también realizaba una corrección interna de manera no evidente.</span><span class="sxs-lookup"><span data-stu-id="bc61f-353">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="bc61f-354">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-354">**New behavior**</span></span>

<span data-ttu-id="bc61f-355">A partir de la versión 3.0, `DeleteBehavior.Restrict` garantiza que las claves externas se crean con la semántica `Restrict`, es decir, sin cascadas y realizando una infracción de restricción, sin llevar a cabo correcciones internas de EF.</span><span class="sxs-lookup"><span data-stu-id="bc61f-355">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="bc61f-356">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-356">**Why**</span></span>

<span data-ttu-id="bc61f-357">Este cambio se realizó para mejorar la experiencia de uso de `DeleteBehavior` de manera intuitiva sin efectos secundarios inesperados.</span><span class="sxs-lookup"><span data-stu-id="bc61f-357">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="bc61f-358">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-358">**Mitigations**</span></span>

<span data-ttu-id="bc61f-359">El comportamiento anterior se puede restaurar con `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-359">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="bc61f-360">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="bc61f-360">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="bc61f-361">Problema de seguimiento n.º 14194</span><span class="sxs-lookup"><span data-stu-id="bc61f-361">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="bc61f-362">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-362">**Old behavior**</span></span>

<span data-ttu-id="bc61f-363">Antes de EF Core 3.0, los [tipos de consulta](xref:core/modeling/keyless-entity-types) eran un medio para consultar los datos que no definen una clave principal de una manera estructurada.</span><span class="sxs-lookup"><span data-stu-id="bc61f-363">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="bc61f-364">Es decir, un tipo de consulta se usaba para asignar tipos de entidad sin claves (más probablemente desde una vista, pero posiblemente desde una tabla), mientras que un tipo de entidad estándar se usaba cuando había una clave disponible (más probablemente desde una tabla, pero posiblemente desde una vista).</span><span class="sxs-lookup"><span data-stu-id="bc61f-364">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="bc61f-365">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-365">**New behavior**</span></span>

<span data-ttu-id="bc61f-366">Ahora un tipo de consulta se convierte en un tipo de entidad sin una clave principal.</span><span class="sxs-lookup"><span data-stu-id="bc61f-366">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="bc61f-367">Los tipos de entidad sin clave tienen la misma funcionalidad que los tipos de consulta de las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="bc61f-367">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="bc61f-368">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-368">**Why**</span></span>

<span data-ttu-id="bc61f-369">Este cambio se ha realizado para reducir la confusión en torno a la finalidad de los tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="bc61f-369">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="bc61f-370">En concreto, son tipos de entidad sin clave y, por ello, intrínsecamente son de solo lectura, pero no se deben usar solo porque un tipo de entidad tenga que ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="bc61f-370">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="bc61f-371">Del mismo modo, se suelen asignar a vistas, pero solo porque las vistas no suelen definir claves.</span><span class="sxs-lookup"><span data-stu-id="bc61f-371">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="bc61f-372">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-372">**Mitigations**</span></span>

<span data-ttu-id="bc61f-373">Los elementos siguientes de la API ahora están obsoletos:</span><span class="sxs-lookup"><span data-stu-id="bc61f-373">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="bc61f-374">**`ModelBuilder.Query<>()`** : en su lugar es necesario llamar a `ModelBuilder.Entity<>().HasNoKey()` para marcar un tipo de entidad como sin claves.</span><span class="sxs-lookup"><span data-stu-id="bc61f-374">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="bc61f-375">Esto todavía no se configurará por convención para evitar una configuración incorrecta cuando se espera una clave principal, pero no coincide con la convención.</span><span class="sxs-lookup"><span data-stu-id="bc61f-375">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="bc61f-376">**`DbQuery<>`** : en su lugar se debe usar `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-376">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="bc61f-377">**`DbContext.Query<>()`** : en su lugar se debe usar `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-377">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="bc61f-378">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="bc61f-378">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="bc61f-379">[Problema de seguimiento n.º 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema de seguimiento n.º 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema de seguimiento n.º 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="bc61f-379">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="bc61f-380">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-380">**Old behavior**</span></span>

<span data-ttu-id="bc61f-381">Antes de EF Core 3.0, la configuración de la relación de propiedad se realizaba directamente después de la llamada a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-381">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="bc61f-382">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-382">**New behavior**</span></span>

<span data-ttu-id="bc61f-383">A partir de EF Core 3.0, ahora hay una API fluida para configurar una propiedad de navegación para el propietario mediante `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-383">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="bc61f-384">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-384">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="bc61f-385">La configuración relacionada con la relación entre el propietario y lo que se posee ahora se debe encadenar después de `WithOwner()`, de forma similar a cómo se configuran otras relaciones.</span><span class="sxs-lookup"><span data-stu-id="bc61f-385">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="bc61f-386">Pero la configuración del propio tipo de propiedad se seguirá encadenando después de `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-386">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="bc61f-387">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-387">For example:</span></span>

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

<span data-ttu-id="bc61f-388">Además, la llamada a `Entity()`, `HasOne()` o `Set()` con un tipo de propiedad de destino ahora iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="bc61f-388">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="bc61f-389">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-389">**Why**</span></span>

<span data-ttu-id="bc61f-390">Este cambio se ha realizado para crear una separación más clara entre la configuración del propio tipo de propiedad y la _relación con_ el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="bc61f-390">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="bc61f-391">A su vez, esto elimina la ambigüedad y la confusión de métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-391">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="bc61f-392">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-392">**Mitigations**</span></span>

<span data-ttu-id="bc61f-393">Cambie la configuración de las relaciones de tipo de propiedad para usar la nueva superficie de API, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="bc61f-393">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="bc61f-394">Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales</span><span class="sxs-lookup"><span data-stu-id="bc61f-394">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="bc61f-395">Problema de seguimiento n.º 9005</span><span class="sxs-lookup"><span data-stu-id="bc61f-395">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="bc61f-396">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-396">**Old behavior**</span></span>

<span data-ttu-id="bc61f-397">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc61f-397">Consider the following model:</span></span>
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
<span data-ttu-id="bc61f-398">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, siempre era necesaria una instancia de `OrderDetails` al agregar un elemento `Order` nuevo.</span><span class="sxs-lookup"><span data-stu-id="bc61f-398">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="bc61f-399">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-399">**New behavior**</span></span>

<span data-ttu-id="bc61f-400">A partir de la versión 3.0, EF Core permite agregar `Order` sin `OrderDetails` y asigna todas las propiedades `OrderDetails` a excepción de la clave principal a columnas que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="bc61f-400">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="bc61f-401">Al realizar consultas, EF Core establece `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor o si no tiene propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-401">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="bc61f-402">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-402">**Mitigations**</span></span>

<span data-ttu-id="bc61f-403">Si el modelo tiene una tabla que comparte dependencias con todas las columnas opcionales, pero la navegación que apunta a ella no se espera que sea `null`, la aplicación debería modificarse para controlar los casos en los que la navegación sea `null`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-403">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="bc61f-404">Si esto no es posible, debería agregarse una propiedad necesaria al tipo de entidad o, al menos, una entidad debería tener un valor distinto a `null` asignado.</span><span class="sxs-lookup"><span data-stu-id="bc61f-404">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="bc61f-405">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="bc61f-405">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="bc61f-406">Problema de seguimiento n.º 14154</span><span class="sxs-lookup"><span data-stu-id="bc61f-406">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="bc61f-407">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-407">**Old behavior**</span></span>

<span data-ttu-id="bc61f-408">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc61f-408">Consider the following model:</span></span>
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
<span data-ttu-id="bc61f-409">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, si solo se actualizaba `OrderDetails`, no se actualizaba el valor `Version` en el cliente y se producía un error en la próxima actualización.</span><span class="sxs-lookup"><span data-stu-id="bc61f-409">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="bc61f-410">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-410">**New behavior**</span></span>

<span data-ttu-id="bc61f-411">A partir de la versión 3.0, EF Core propaga el nuevo valor `Version` en `Order` si posee `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-411">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="bc61f-412">En caso contrario, se produce una excepción durante la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="bc61f-412">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="bc61f-413">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-413">**Why**</span></span>

<span data-ttu-id="bc61f-414">Este cambio se realizó para evitar un valor de token de simultaneidad obsoleto cuando solo se actualiza una de las entidades asignadas a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="bc61f-414">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="bc61f-415">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-415">**Mitigations**</span></span>

<span data-ttu-id="bc61f-416">Todas las entidades que comparten la tabla deben incluir una propiedad que se asigna a la columna del token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="bc61f-416">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="bc61f-417">Es posible crear una en estado reemplazado:</span><span class="sxs-lookup"><span data-stu-id="bc61f-417">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="bc61f-418">Ahora, las propiedades heredadas de tipos sin asignar se asignan a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="bc61f-418">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="bc61f-419">Problema de seguimiento n.º 13998</span><span class="sxs-lookup"><span data-stu-id="bc61f-419">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="bc61f-420">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-420">**Old behavior**</span></span>

<span data-ttu-id="bc61f-421">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc61f-421">Consider the following model:</span></span>
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

<span data-ttu-id="bc61f-422">Antes de EF Core 3.0, la propiedad `ShippingAddress` se asignaba a columnas distintas para `BulkOrder` y `Order` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="bc61f-422">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="bc61f-423">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-423">**New behavior**</span></span>

<span data-ttu-id="bc61f-424">A partir de la versión3.0, EF Core solo crea una columna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-424">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="bc61f-425">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-425">**Why**</span></span>

<span data-ttu-id="bc61f-426">El comportamiento anterior no era el esperado.</span><span class="sxs-lookup"><span data-stu-id="bc61f-426">The old behavoir was unexpected.</span></span>

<span data-ttu-id="bc61f-427">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-427">**Mitigations**</span></span>

<span data-ttu-id="bc61f-428">Todavía se puede asignar explícitamente la propiedad a columnas separadas en los tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="bc61f-428">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="bc61f-429">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="bc61f-429">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="bc61f-430">Problema de seguimiento n.º 13274</span><span class="sxs-lookup"><span data-stu-id="bc61f-430">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="bc61f-431">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-431">**Old behavior**</span></span>

<span data-ttu-id="bc61f-432">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc61f-432">Consider the following model:</span></span>
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
<span data-ttu-id="bc61f-433">Antes de EF Core 3.0, se podía usar la propiedad `CustomerId` para la clave externa por convención.</span><span class="sxs-lookup"><span data-stu-id="bc61f-433">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="bc61f-434">Pero si `Order` es un tipo de propiedad, entonces esto convertiría también a `CustomerId` en la clave principal, algo que no suele ser lo esperado.</span><span class="sxs-lookup"><span data-stu-id="bc61f-434">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="bc61f-435">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-435">**New behavior**</span></span>

<span data-ttu-id="bc61f-436">A partir de la versión 3.0, EF Core no intenta usar las propiedades de claves externas por convención si tienen el mismo nombre que la propiedad de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="bc61f-436">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="bc61f-437">Los patrones de nombre de tipo de entidad de seguridad concatenado con el nombre de propiedad de la entidad de seguridad y de nombre de navegación concatenado con el nombre de propiedad de la entidad de seguridad todavía se hacen coincidir.</span><span class="sxs-lookup"><span data-stu-id="bc61f-437">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="bc61f-438">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-438">For example:</span></span>

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

<span data-ttu-id="bc61f-439">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-439">**Why**</span></span>

<span data-ttu-id="bc61f-440">Este cambio se ha realizado para evitar definir erróneamente una propiedad de clave principal en el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="bc61f-440">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="bc61f-441">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-441">**Mitigations**</span></span>

<span data-ttu-id="bc61f-442">Si la propiedad se ha diseñado para ser la clave externa y, por tanto, parte de la clave principal, se debe configurar explícitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="bc61f-442">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="bc61f-443">Ahora, la conexión de base de datos se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="bc61f-443">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="bc61f-444">Problema de seguimiento n.º 14218</span><span class="sxs-lookup"><span data-stu-id="bc61f-444">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="bc61f-445">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-445">**Old behavior**</span></span>

<span data-ttu-id="bc61f-446">Antes de EF Core 3.0, si el contexto abría la conexión dentro de `TransactionScope`, la conexión permanecía abierta mientras el ámbito actual `TransactionScope` estuviese activo.</span><span class="sxs-lookup"><span data-stu-id="bc61f-446">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="bc61f-447">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-447">**New behavior**</span></span>

<span data-ttu-id="bc61f-448">A partir de la versión 3.0, EF Core cierra la conexión en cuanto se deja de usar.</span><span class="sxs-lookup"><span data-stu-id="bc61f-448">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="bc61f-449">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-449">**Why**</span></span>

<span data-ttu-id="bc61f-450">Este cambio permite usar varios contextos en el mismo ámbito `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-450">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="bc61f-451">El comportamiento nuevo también coincide con el de EF6.</span><span class="sxs-lookup"><span data-stu-id="bc61f-451">The new behavior also matches EF6.</span></span>

<span data-ttu-id="bc61f-452">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-452">**Mitigations**</span></span>

<span data-ttu-id="bc61f-453">Si la conexión debe permanecer abierta, una llamada explícita a `OpenConnection()` asegurará que EF Core no la cierre de forma prematura:</span><span class="sxs-lookup"><span data-stu-id="bc61f-453">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="bc61f-454">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="bc61f-454">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="bc61f-455">Problema de seguimiento n.º 6872</span><span class="sxs-lookup"><span data-stu-id="bc61f-455">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="bc61f-456">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-456">**Old behavior**</span></span>

<span data-ttu-id="bc61f-457">Antes de EF Core 3.0, se usaba un generador de valores compartidos para todas las propiedades de clave entera en memoria.</span><span class="sxs-lookup"><span data-stu-id="bc61f-457">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="bc61f-458">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-458">**New behavior**</span></span>

<span data-ttu-id="bc61f-459">A partir de EF Core 3.0, cada propiedad de clave entera obtiene su propio generador de valores cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="bc61f-459">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="bc61f-460">Además, si se elimina la base de datos, la generación de claves se restablece para todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="bc61f-460">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="bc61f-461">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-461">**Why**</span></span>

<span data-ttu-id="bc61f-462">Este cambio se ha realizado para alinear la generación de claves en memoria más estrechamente a la generación de claves de base de datos reales y para mejorar la capacidad para aislar las pruebas entre sí cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="bc61f-462">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="bc61f-463">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-463">**Mitigations**</span></span>

<span data-ttu-id="bc61f-464">Esto puede interrumpir una aplicación que se base en el establecimiento de valores de clave específicos en memoria.</span><span class="sxs-lookup"><span data-stu-id="bc61f-464">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="bc61f-465">En su lugar, considere la posibilidad de no depender de valores de clave específicos, o bien de actualizar para que coincida con el comportamiento nuevo.</span><span class="sxs-lookup"><span data-stu-id="bc61f-465">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="bc61f-466">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="bc61f-466">Backing fields are used by default</span></span>

[<span data-ttu-id="bc61f-467">Problema de seguimiento n.º 12430</span><span class="sxs-lookup"><span data-stu-id="bc61f-467">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="bc61f-468">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-468">**Old behavior**</span></span>

<span data-ttu-id="bc61f-469">Antes de la versión 3.0, incluso si se conocía el campo de respaldo de una propiedad, de forma predeterminada en EF Core se leía y escribía el valor de propiedad mediante los métodos captadores y establecedores de propiedades.</span><span class="sxs-lookup"><span data-stu-id="bc61f-469">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="bc61f-470">La excepción era la ejecución de consultas, donde el campo de respaldo se establecía directamente si se conocía.</span><span class="sxs-lookup"><span data-stu-id="bc61f-470">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="bc61f-471">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-471">**New behavior**</span></span>

<span data-ttu-id="bc61f-472">A partir de EF Core 3.0, si se conoce el campo de respaldo para una propiedad, EF Core siempre la leerá y escribirá mediante el campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="bc61f-472">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="bc61f-473">Esto podría provocar una interrupción de la aplicación si depende de un comportamiento adicional codificado en los métodos captadores o establecedores.</span><span class="sxs-lookup"><span data-stu-id="bc61f-473">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="bc61f-474">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-474">**Why**</span></span>

<span data-ttu-id="bc61f-475">Este cambio se ha realizado para evitar que EF Core desencadene erróneamente lógica de negocios de forma predeterminada al realizar operaciones de base de datos que implican entidades.</span><span class="sxs-lookup"><span data-stu-id="bc61f-475">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="bc61f-476">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-476">**Mitigations**</span></span>

<span data-ttu-id="bc61f-477">El comportamiento anterior a la versión 3.0 se puede restaurar mediante la configuración del modo de acceso de propiedad en `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-477">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="bc61f-478">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-478">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="bc61f-479">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="bc61f-479">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="bc61f-480">Problema de seguimiento n.º 12523</span><span class="sxs-lookup"><span data-stu-id="bc61f-480">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="bc61f-481">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-481">**Old behavior**</span></span>

<span data-ttu-id="bc61f-482">Antes de EF Core 3.0, si varios campos coincidían con las reglas para buscar el campo de respaldo de una propiedad, se elegía un campo según un orden de prioridad.</span><span class="sxs-lookup"><span data-stu-id="bc61f-482">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="bc61f-483">Esto podía producir que, en caso de ambigüedad, se usara el campo incorrecto.</span><span class="sxs-lookup"><span data-stu-id="bc61f-483">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="bc61f-484">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-484">**New behavior**</span></span>

<span data-ttu-id="bc61f-485">A partir de EF Core 3.0, si varios campos coinciden con la misma propiedad, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="bc61f-485">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="bc61f-486">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-486">**Why**</span></span>

<span data-ttu-id="bc61f-487">Este cambio se ha realizado para evitar de forma silenciosa el uso de un campo con respecto a otro cuando solo uno puede ser correcto.</span><span class="sxs-lookup"><span data-stu-id="bc61f-487">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="bc61f-488">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-488">**Mitigations**</span></span>

<span data-ttu-id="bc61f-489">En las propiedades con campos de respaldo ambiguos se debe especificar de forma explícita el campo que se va usar.</span><span class="sxs-lookup"><span data-stu-id="bc61f-489">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="bc61f-490">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="bc61f-490">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="bc61f-491">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="bc61f-491">Field-only property names should match the field name</span></span>

<span data-ttu-id="bc61f-492">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-492">**Old behavior**</span></span>

<span data-ttu-id="bc61f-493">Antes de EF Core 3.0, una propiedad podía especificarse con un valor de cadena y, si no había ninguna propiedad con ese nombre en el tipo .NET, EF Core intentaba hacerla coincidir con un campo mediante reglas de convención.</span><span class="sxs-lookup"><span data-stu-id="bc61f-493">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="bc61f-494">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-494">**New behavior**</span></span>

<span data-ttu-id="bc61f-495">A partir de EF Core 3.0, una propiedad de solo campo debe coincidir exactamente con el nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="bc61f-495">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="bc61f-496">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-496">**Why**</span></span>

<span data-ttu-id="bc61f-497">Este cambio se realizó para evitar el uso del mismo campo para dos propiedades con nombres similares. También hace que las reglas de coincidencia para propiedades solo de campo sean las mismas que para las propiedades asignadas a propiedades CLR.</span><span class="sxs-lookup"><span data-stu-id="bc61f-497">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="bc61f-498">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-498">**Mitigations**</span></span>

<span data-ttu-id="bc61f-499">Las propiedades solo de campo deberían tener el mismo nombre que el campo al que están asignadas.</span><span class="sxs-lookup"><span data-stu-id="bc61f-499">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="bc61f-500">En una próxima versión de EF Core 3.0 tenemos planeado volver a habilitar la configuración explícita de un nombre de campo distinto al nombre de la propiedad (vea el problema [n.° 15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="bc61f-500">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="bc61f-501">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="bc61f-501">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="bc61f-502">Problema de seguimiento n.° 14756</span><span class="sxs-lookup"><span data-stu-id="bc61f-502">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="bc61f-503">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-503">**Old behavior**</span></span>

<span data-ttu-id="bc61f-504">Antes de EF Core 3.0, llamar a `AddDbContext` o a `AddDbContextPool` también podría registrar los servicios de almacenamiento en caché y de registro con D.I. a través de llamadas a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) y a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="bc61f-504">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="bc61f-505">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-505">**New behavior**</span></span>

<span data-ttu-id="bc61f-506">A partir de EF Core 3.0, `AddDbContext` y `AddDbContextPool` ya no registrarán estos servicios con inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="bc61f-506">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="bc61f-507">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-507">**Why**</span></span>

<span data-ttu-id="bc61f-508">EF Core 3.0 no requiere que estos servicios estén en el contenedor de inserción de dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-508">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="bc61f-509">Pero si `ILoggerFactory` se registra en el contenedor de DI de la aplicación, EF Core lo empezará a usar de todos modos.</span><span class="sxs-lookup"><span data-stu-id="bc61f-509">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="bc61f-510">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-510">**Mitigations**</span></span>

<span data-ttu-id="bc61f-511">Si la aplicación necesita estos servicios, regístrelos de manera explícita con el contenedor de DI mediante [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="bc61f-511">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="bc61f-512">Ahora DbContext.Entry realiza una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="bc61f-512">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="bc61f-513">Problema de seguimiento n.º 13552</span><span class="sxs-lookup"><span data-stu-id="bc61f-513">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="bc61f-514">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-514">**Old behavior**</span></span>

<span data-ttu-id="bc61f-515">Antes de EF Core 3.0, la llamada a `DbContext.Entry` provocaba que se detectaran cambios para todas las entidades con seguimiento.</span><span class="sxs-lookup"><span data-stu-id="bc61f-515">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="bc61f-516">Esto garantizaba que el estado expuesto en `EntityEntry` estuviera actualizado.</span><span class="sxs-lookup"><span data-stu-id="bc61f-516">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="bc61f-517">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-517">**New behavior**</span></span>

<span data-ttu-id="bc61f-518">A partir de EF Core 3.0, ahora la llamada a `DbContext.Entry` solo intenta detectar cambios en la entidad dada y cualquier entidad de seguridad relacionada con ella de la que se haya realizado el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="bc61f-518">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="bc61f-519">Esto significa que es posible que la llamada a este método no haya detectado otros cambios, lo que podría tener implicaciones en el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-519">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="bc61f-520">Observe que si `ChangeTracker.AutoDetectChangesEnabled` se establece en `false` incluso esta detección de cambios local se deshabilitará.</span><span class="sxs-lookup"><span data-stu-id="bc61f-520">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="bc61f-521">Otros métodos que provocan la detección de cambios (como `ChangeTracker.Entries` y `SaveChanges`) siguen provocando una acción `DetectChanges` completa de todas las entidades de las que se realiza el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="bc61f-521">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="bc61f-522">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-522">**Why**</span></span>

<span data-ttu-id="bc61f-523">Este cambio se ha realizado para mejorar el rendimiento predeterminado del uso de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-523">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="bc61f-524">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-524">**Mitigations**</span></span>

<span data-ttu-id="bc61f-525">Llame a `ChgangeTracker.DetectChanges()` de forma explícita antes de llamar a `Entry` para garantizar el comportamiento anterior a la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="bc61f-525">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="bc61f-526">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="bc61f-526">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="bc61f-527">Problema de seguimiento n.º 14617</span><span class="sxs-lookup"><span data-stu-id="bc61f-527">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="bc61f-528">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-528">**Old behavior**</span></span>

<span data-ttu-id="bc61f-529">Antes de EF Core 3.0, se podían usar las propiedades de clave `string` y `byte[]` sin tener que establecer de forma explícita un valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="bc61f-529">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="bc61f-530">En ese caso, el valor de clave se generaba en el cliente como un GUID, que se serializaba en bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-530">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="bc61f-531">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-531">**New behavior**</span></span>

<span data-ttu-id="bc61f-532">A partir de EF Core 3.0, se iniciará una excepción en la que indica que no se ha establecido ningún valor de clave.</span><span class="sxs-lookup"><span data-stu-id="bc61f-532">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="bc61f-533">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-533">**Why**</span></span>

<span data-ttu-id="bc61f-534">Este cambio se ha realizado porque los valores `string`/`byte[]` generados por el cliente no suelen ser útiles, y el comportamiento predeterminado dificultaba razonar sobre los valores de clave generados de una forma habitual.</span><span class="sxs-lookup"><span data-stu-id="bc61f-534">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="bc61f-535">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-535">**Mitigations**</span></span>

<span data-ttu-id="bc61f-536">Se puede obtener el comportamiento anterior a la versión 3.0 si se especifica de forma explícita que las propiedades de clave deben usar los valores generados si no se establece ningún otro valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="bc61f-536">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="bc61f-537">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="bc61f-537">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="bc61f-538">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="bc61f-538">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="bc61f-539">Ahora ILoggerFactory es un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="bc61f-539">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="bc61f-540">Problema de seguimiento n.º 14698</span><span class="sxs-lookup"><span data-stu-id="bc61f-540">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="bc61f-541">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-541">**Old behavior**</span></span>

<span data-ttu-id="bc61f-542">Antes de EF Core 3.0, `ILoggerFactory` se registraba como un servicio de singleton.</span><span class="sxs-lookup"><span data-stu-id="bc61f-542">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="bc61f-543">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-543">**New behavior**</span></span>

<span data-ttu-id="bc61f-544">A partir de EF Core 3.0, `ILoggerFactory` ahora se registra como con ámbito.</span><span class="sxs-lookup"><span data-stu-id="bc61f-544">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="bc61f-545">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-545">**Why**</span></span>

<span data-ttu-id="bc61f-546">Este cambio se ha realizado para permitir la asociación de un registrador con una instancia de `DbContext`, lo que habilita otras funciones y quita algunos casos de comportamiento patológico como un aumento vertiginoso de los proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="bc61f-546">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="bc61f-547">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-547">**Mitigations**</span></span>

<span data-ttu-id="bc61f-548">Este cambio no debería afectar al código de la aplicación a menos que registre y use servicios personalizados en el proveedor de servicios internos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="bc61f-548">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="bc61f-549">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="bc61f-549">This isn't common.</span></span>
<span data-ttu-id="bc61f-550">En estos casos, la mayoría de los elementos seguirá funcionando, pero cualquier servicio de singleton que dependiera de `ILoggerFactory` tendrá que cambiarse para obtener la interfaz `ILoggerFactory` de otra forma.</span><span class="sxs-lookup"><span data-stu-id="bc61f-550">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="bc61f-551">Si experimenta situaciones como esta, registre un problema en el [rastreador de problemas de GitHub de EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para hacernos saber cómo usa `ILoggerFactory`, para que podamos comprender mejor cómo evitar esta interrupción en el futuro.</span><span class="sxs-lookup"><span data-stu-id="bc61f-551">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="bc61f-552">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="bc61f-552">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="bc61f-553">Problema de seguimiento n.º 12780</span><span class="sxs-lookup"><span data-stu-id="bc61f-553">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="bc61f-554">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-554">**Old behavior**</span></span>

<span data-ttu-id="bc61f-555">Antes de EF Core 3.0, una vez que se eliminaba `DbContext` no había ninguna forma de saber si una determinada propiedad de navegación de una entidad obtenida de ese contexto se había cargado completamente o no.</span><span class="sxs-lookup"><span data-stu-id="bc61f-555">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="bc61f-556">En su lugar, los proxies asumían que una navegación de referencia se cargaba si tenía un valor distinto de NULL, y que una navegación de colección se cargaba si no estaba vacía.</span><span class="sxs-lookup"><span data-stu-id="bc61f-556">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="bc61f-557">En estos casos, el intento de carga diferida era no operativo.</span><span class="sxs-lookup"><span data-stu-id="bc61f-557">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="bc61f-558">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-558">**New behavior**</span></span>

<span data-ttu-id="bc61f-559">A partir de EF Core 3.0, los proxies realizan el seguimiento de si una propiedad de navegación se carga o no.</span><span class="sxs-lookup"><span data-stu-id="bc61f-559">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="bc61f-560">Esto significa que el intento de acceder a una propiedad de navegación que se carga después de que se haya eliminado el contexto siempre será no operativo, incluso cuando la navegación cargada está vacía o es NULL.</span><span class="sxs-lookup"><span data-stu-id="bc61f-560">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="bc61f-561">Por el contrario, el intento de acceder a una propiedad de navegación que no está cargada iniciará una excepción si el contexto se ha eliminado, incluso si la propiedad de navegación es una colección no vacía.</span><span class="sxs-lookup"><span data-stu-id="bc61f-561">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="bc61f-562">Si se produce esta situación, significa que el código de aplicación está intentando usar la carga diferida en un momento no válido y que se debe cambiar la aplicación para que lo no haga.</span><span class="sxs-lookup"><span data-stu-id="bc61f-562">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="bc61f-563">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-563">**Why**</span></span>

<span data-ttu-id="bc61f-564">Este cambio se ha realizado para que el comportamiento sea coherente y correcto cuando se intenta la carga diferida de una instancia de `DbContext` eliminada.</span><span class="sxs-lookup"><span data-stu-id="bc61f-564">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="bc61f-565">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-565">**Mitigations**</span></span>

<span data-ttu-id="bc61f-566">Actualice el código de la aplicación para que no intente la carga diferida con un contexto eliminado, o bien configúrelo para que sea no operativo, como se describe en el mensaje de la excepción.</span><span class="sxs-lookup"><span data-stu-id="bc61f-566">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="bc61f-567">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="bc61f-567">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="bc61f-568">Problema de seguimiento n.º 10236</span><span class="sxs-lookup"><span data-stu-id="bc61f-568">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="bc61f-569">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-569">**Old behavior**</span></span>

<span data-ttu-id="bc61f-570">Antes de EF Core 3.0, se registraba una advertencia para una aplicación que creaba un número patológico de proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="bc61f-570">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="bc61f-571">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-571">**New behavior**</span></span>

<span data-ttu-id="bc61f-572">A partir de EF Core 3.0, ahora esta advertencia se considera un error y se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="bc61f-572">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="bc61f-573">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-573">**Why**</span></span>

<span data-ttu-id="bc61f-574">Este cambio se ha realizado para controlar mejor el código de la aplicación mediante la exposición de este caso patológico de una forma más explícita.</span><span class="sxs-lookup"><span data-stu-id="bc61f-574">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="bc61f-575">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-575">**Mitigations**</span></span>

<span data-ttu-id="bc61f-576">Cuando se produce este error, la acción más adecuada consiste en comprender la causa raíz y detener la creación de tantos proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="bc61f-576">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="bc61f-577">Pero el error se puede convertir en una advertencia (u omitirse) mediante configuración en `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-577">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="bc61f-578">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-578">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="bc61f-579">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="bc61f-579">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="bc61f-580">Problema de seguimiento n.° 9171</span><span class="sxs-lookup"><span data-stu-id="bc61f-580">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="bc61f-581">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-581">**Old behavior**</span></span>

<span data-ttu-id="bc61f-582">Antes de EF Core 3.0, el código para llamar a `HasOne` o `HasMany` con una cadena se interpretaba de manera confusa.</span><span class="sxs-lookup"><span data-stu-id="bc61f-582">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="bc61f-583">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-583">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="bc61f-584">El código parece relacionar `Samurai` con otro tipo de entidad mediante la propiedad de navegación `Entrance`, que puede ser privada.</span><span class="sxs-lookup"><span data-stu-id="bc61f-584">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="bc61f-585">En realidad, este código intenta crear una relación con algún tipo de entidad denominada `Entrance` sin ninguna propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-585">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="bc61f-586">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-586">**New behavior**</span></span>

<span data-ttu-id="bc61f-587">A partir de EF Core 3.0, el código anterior ahora hace lo que parecía que debía hacer antes.</span><span class="sxs-lookup"><span data-stu-id="bc61f-587">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="bc61f-588">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-588">**Why**</span></span>

<span data-ttu-id="bc61f-589">El comportamiento anterior era muy confuso, especialmente al leer el código de configuración y al buscar errores.</span><span class="sxs-lookup"><span data-stu-id="bc61f-589">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="bc61f-590">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-590">**Mitigations**</span></span>

<span data-ttu-id="bc61f-591">Esto solo interrumpirá las aplicaciones que configuran de manera explícita las relaciones con cadenas para nombres de tipos y sin especificar explícitamente la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-591">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="bc61f-592">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="bc61f-592">This is not common.</span></span>
<span data-ttu-id="bc61f-593">El comportamiento anterior se puede obtener al pasar de manera explícita `null` para el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-593">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="bc61f-594">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-594">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="bc61f-595">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="bc61f-595">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="bc61f-596">Problema de seguimiento n.º 15184</span><span class="sxs-lookup"><span data-stu-id="bc61f-596">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="bc61f-597">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-597">**Old behavior**</span></span>

<span data-ttu-id="bc61f-598">Antes, los siguientes métodos asincrónicos devolvían `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="bc61f-598">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="bc61f-599">`ValueGenerator.NextValueAsync()` (y las clases derivadas)</span><span class="sxs-lookup"><span data-stu-id="bc61f-599">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="bc61f-600">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-600">**New behavior**</span></span>

<span data-ttu-id="bc61f-601">Dichos métodos ahora devuelven `ValueTask<T>` durante el mismo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="bc61f-601">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="bc61f-602">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-602">**Why**</span></span>

<span data-ttu-id="bc61f-603">Este cambio reduce el número de asignaciones de montones que se producen al invocar estos métodos, lo que mejora el rendimiento general.</span><span class="sxs-lookup"><span data-stu-id="bc61f-603">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="bc61f-604">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-604">**Mitigations**</span></span>

<span data-ttu-id="bc61f-605">Las aplicaciones que simplemente esperen las API anteriores solo necesitan recompilarse, sin que sea necesario realizar cambios en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="bc61f-605">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="bc61f-606">Un uso más complejo (p. ej., pasar el valor `Task` devuelto a `Task.WhenAny()`) normalmente requiere que el valor `ValueTask<T>` devuelto se convierta en `Task<T>` mediante una llamada a `AsTask()` en él.</span><span class="sxs-lookup"><span data-stu-id="bc61f-606">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="bc61f-607">Tenga en cuenta que esto niega la reducción de asignación que implica este cambio.</span><span class="sxs-lookup"><span data-stu-id="bc61f-607">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="bc61f-608">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="bc61f-608">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="bc61f-609">Problema de seguimiento n.º 9913</span><span class="sxs-lookup"><span data-stu-id="bc61f-609">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="bc61f-610">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-610">**Old behavior**</span></span>

<span data-ttu-id="bc61f-611">El nombre de anotación para las anotaciones de asignación de tipos era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="bc61f-611">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="bc61f-612">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-612">**New behavior**</span></span>

<span data-ttu-id="bc61f-613">Ahora, el nombre de anotación para las anotaciones de asignación de tipos es "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="bc61f-613">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="bc61f-614">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-614">**Why**</span></span>

<span data-ttu-id="bc61f-615">Ahora, las asignaciones de tipos se usan para algo más que solo para proveedores de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="bc61f-615">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="bc61f-616">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-616">**Mitigations**</span></span>

<span data-ttu-id="bc61f-617">Esto solo interrumpirá a las aplicaciones que acceden directamente a la asignación de tipos como una anotación, lo que no es habitual.</span><span class="sxs-lookup"><span data-stu-id="bc61f-617">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="bc61f-618">La acción más apropiada para corregir es usar la superficie de API para acceder a las asignaciones de tipos en lugar de usar directamente la anotación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-618">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="bc61f-619">ToTable en un tipo derivado inicia una excepción</span><span class="sxs-lookup"><span data-stu-id="bc61f-619">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="bc61f-620">Problema de seguimiento n.º 11811</span><span class="sxs-lookup"><span data-stu-id="bc61f-620">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="bc61f-621">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-621">**Old behavior**</span></span>

<span data-ttu-id="bc61f-622">Antes de EF Core 3.0, la llamada a `ToTable()` en un tipo derivado se omitía, ya que la única estrategia asignación de herencia era TPH, lo que no es válido.</span><span class="sxs-lookup"><span data-stu-id="bc61f-622">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="bc61f-623">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-623">**New behavior**</span></span>

<span data-ttu-id="bc61f-624">A partir de EF Core 3.0, y en preparación para agregar compatibilidad con TPT y TPC en una versión posterior, ahora la llamada a `ToTable()` en un tipo derivado iniciará una excepción para evitar un cambio de asignación inesperado en el futuro.</span><span class="sxs-lookup"><span data-stu-id="bc61f-624">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="bc61f-625">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-625">**Why**</span></span>

<span data-ttu-id="bc61f-626">En la actualidad no se considera válido asignar un tipo derivado a otra tabla.</span><span class="sxs-lookup"><span data-stu-id="bc61f-626">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="bc61f-627">Este cambio evita interrupciones en el futuro, cuando se convierta en una operación válida.</span><span class="sxs-lookup"><span data-stu-id="bc61f-627">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="bc61f-628">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-628">**Mitigations**</span></span>

<span data-ttu-id="bc61f-629">Quite todos los intentos de asignar tipos derivados a otras tablas.</span><span class="sxs-lookup"><span data-stu-id="bc61f-629">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="bc61f-630">ForSqlServerHasIndex se ha reemplazado por HasIndex</span><span class="sxs-lookup"><span data-stu-id="bc61f-630">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="bc61f-631">Problema de seguimiento n.º 12366</span><span class="sxs-lookup"><span data-stu-id="bc61f-631">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="bc61f-632">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-632">**Old behavior**</span></span>

<span data-ttu-id="bc61f-633">Antes de EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` proporcionaba una manera de configurar las columnas que se usaban con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="bc61f-634">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-634">**New behavior**</span></span>

<span data-ttu-id="bc61f-635">A partir de EF Core 3.0, ya se admite el uso de `Include` en un índice en el nivel relacional.</span><span class="sxs-lookup"><span data-stu-id="bc61f-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="bc61f-636">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="bc61f-637">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-637">**Why**</span></span>

<span data-ttu-id="bc61f-638">Este cambio se ha realizado para consolidar la API para índices con `Include` en un mismo lugar para todos los proveedores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="bc61f-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="bc61f-639">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-639">**Mitigations**</span></span>

<span data-ttu-id="bc61f-640">Use la API nueva, como se ha mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="bc61f-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="bc61f-641">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="bc61f-641">Metadata API changes</span></span>

[<span data-ttu-id="bc61f-642">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="bc61f-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="bc61f-643">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-643">**New behavior**</span></span>

<span data-ttu-id="bc61f-644">Las siguientes propiedades se han convertido en métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="bc61f-644">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="bc61f-645">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-645">**Why**</span></span>

<span data-ttu-id="bc61f-646">Este cambio simplifica la implementación de las interfaces mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="bc61f-646">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="bc61f-647">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-647">**Mitigations**</span></span>

<span data-ttu-id="bc61f-648">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="bc61f-648">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="bc61f-649">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="bc61f-649">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="bc61f-650">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="bc61f-650">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="bc61f-651">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-651">**New behavior**</span></span>

<span data-ttu-id="bc61f-652">Los métodos de extensión específicos del proveedor se simplificarán:</span><span class="sxs-lookup"><span data-stu-id="bc61f-652">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="bc61f-653">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-653">**Why**</span></span>

<span data-ttu-id="bc61f-654">Este cambio simplifica la implementación de los métodos de extensión mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="bc61f-654">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="bc61f-655">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-655">**Mitigations**</span></span>

<span data-ttu-id="bc61f-656">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="bc61f-656">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="bc61f-657">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="bc61f-657">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="bc61f-658">Problema de seguimiento n.º 12151</span><span class="sxs-lookup"><span data-stu-id="bc61f-658">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="bc61f-659">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-659">**Old behavior**</span></span>

<span data-ttu-id="bc61f-660">Antes de EF Core 3.0, EF Core enviaba `PRAGMA foreign_keys = 1` cuando se abría una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="bc61f-660">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="bc61f-661">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-661">**New behavior**</span></span>

<span data-ttu-id="bc61f-662">A partir de EF Core 3.0, EF Core ya no envía `PRAGMA foreign_keys = 1` cuando se abre una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="bc61f-662">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="bc61f-663">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-663">**Why**</span></span>

<span data-ttu-id="bc61f-664">Este cambio se ha realizado porque en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3` de forma predeterminada, lo que a su vez significa que el cumplimiento de CD está activado de forma predeterminada y no es necesario habilitarlo explícitamente cada vez que se abra una conexión.</span><span class="sxs-lookup"><span data-stu-id="bc61f-664">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="bc61f-665">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-665">**Mitigations**</span></span>

<span data-ttu-id="bc61f-666">Las claves externas se habilitan de forma predeterminada en SQLitePCLRaw.bundle_e_sqlite3, que en EF Core se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="bc61f-666">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="bc61f-667">Para otros casos, las claves externas se pueden habilitar mediante la especificación de `Foreign Keys=True` en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="bc61f-667">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="bc61f-668">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="bc61f-668">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="bc61f-669">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-669">**Old behavior**</span></span>

<span data-ttu-id="bc61f-670">Antes de EF Core 3.0, en EF Core se usaba `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-670">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="bc61f-671">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-671">**New behavior**</span></span>

<span data-ttu-id="bc61f-672">A partir de EF Core 3.0, en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-672">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="bc61f-673">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-673">**Why**</span></span>

<span data-ttu-id="bc61f-674">Este cambio se ha realizado para que la versión de SQLite que se usa en iOS sea coherente con otras plataformas.</span><span class="sxs-lookup"><span data-stu-id="bc61f-674">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="bc61f-675">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-675">**Mitigations**</span></span>

<span data-ttu-id="bc61f-676">Para usar la versión nativa de SQLite en iOS, configure `Microsoft.Data.Sqlite` para usar otra agrupación `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-676">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="bc61f-677">Almacenamiento de valores GUID como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="bc61f-677">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="bc61f-678">Problema de seguimiento n.º 15078</span><span class="sxs-lookup"><span data-stu-id="bc61f-678">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="bc61f-679">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-679">**Old behavior**</span></span>

<span data-ttu-id="bc61f-680">Antes, los valores GUID se almacenaban como valores BLOB en SQLite.</span><span class="sxs-lookup"><span data-stu-id="bc61f-680">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="bc61f-681">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-681">**New behavior**</span></span>

<span data-ttu-id="bc61f-682">Ahora, los valores GUID se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="bc61f-682">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="bc61f-683">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-683">**Why**</span></span>

<span data-ttu-id="bc61f-684">El formato binario de los GUID no está normalizado.</span><span class="sxs-lookup"><span data-stu-id="bc61f-684">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="bc61f-685">El almacenamiento de los valores como TEXT mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="bc61f-685">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="bc61f-686">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-686">**Mitigations**</span></span>

<span data-ttu-id="bc61f-687">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="bc61f-687">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="bc61f-688">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="bc61f-688">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="bc61f-689">Microsoft.Data.Sqlite sigue siendo capaz de leer valores GUID de ambas columnas BLOB y TEXT. Sin embargo, dado que el formato predeterminado de los parámetros y las constantes ha cambiado, seguramente deberá realizar alguna acción en la mayoría de casos que impliquen el uso de valores GUID.</span><span class="sxs-lookup"><span data-stu-id="bc61f-689">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="bc61f-690">Ahora los valores char se almacenan como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="bc61f-690">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="bc61f-691">Problema de seguimiento n.º 15020</span><span class="sxs-lookup"><span data-stu-id="bc61f-691">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="bc61f-692">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-692">**Old behavior**</span></span>

<span data-ttu-id="bc61f-693">Anteriormente los valores char se almacenaban como valores INTEGER en SQLite.</span><span class="sxs-lookup"><span data-stu-id="bc61f-693">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="bc61f-694">Por ejemplo, un valor char de *A* se almacenaba como el valor entero 65.</span><span class="sxs-lookup"><span data-stu-id="bc61f-694">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="bc61f-695">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-695">**New behavior**</span></span>

<span data-ttu-id="bc61f-696">Ahora, los valores char se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="bc61f-696">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="bc61f-697">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-697">**Why**</span></span>

<span data-ttu-id="bc61f-698">El almacenamiento de valores como TEXT es más natural y mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="bc61f-698">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="bc61f-699">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-699">**Mitigations**</span></span>

<span data-ttu-id="bc61f-700">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="bc61f-700">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="bc61f-701">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="bc61f-701">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="bc61f-702">Microsoft.Data.Sqlite también puede leer valores de caracteres tanto de columnas INTEGER como de columnas TEXT, por lo que es posible que no deba hacer nada dependiendo de su caso.</span><span class="sxs-lookup"><span data-stu-id="bc61f-702">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="bc61f-703">Ahora los id. de migración se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="bc61f-703">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="bc61f-704">Problema de seguimiento n.º 12978</span><span class="sxs-lookup"><span data-stu-id="bc61f-704">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="bc61f-705">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-705">**Old behavior**</span></span>

<span data-ttu-id="bc61f-706">Los identificadores de migración se generaban de forma involuntaria con el calendario de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="bc61f-706">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="bc61f-707">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-707">**New behavior**</span></span>

<span data-ttu-id="bc61f-708">Ahora los id. de migración siempre se generan usando el calendario de la referencia cultural invariable (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="bc61f-708">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="bc61f-709">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-709">**Why**</span></span>

<span data-ttu-id="bc61f-710">El orden de las migraciones es importante al actualizar la base de datos o al solucionar conflictos de combinación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-710">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="bc61f-711">Al usar el calendario invariable, se evitan problemas de ordenación que pueden producirse si los miembros del equipo tienen distintos calendarios del sistema.</span><span class="sxs-lookup"><span data-stu-id="bc61f-711">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="bc61f-712">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-712">**Mitigations**</span></span>

<span data-ttu-id="bc61f-713">Esta cambio afecta a todas las personas que usan un calendario no gregoriano en el que el año sea superior al del calendario gregoriano (como el calendario budista tailandés).</span><span class="sxs-lookup"><span data-stu-id="bc61f-713">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="bc61f-714">Los id. de migración existentes deberán actualizarse para que las migraciones nuevas se ordenen después de las existentes.</span><span class="sxs-lookup"><span data-stu-id="bc61f-714">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="bc61f-715">Puede ver el id. de migración en el atributo Migration de los archivos de diseñador de la migración.</span><span class="sxs-lookup"><span data-stu-id="bc61f-715">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="bc61f-716">También debe actualizarse la tabla de historial de migraciones.</span><span class="sxs-lookup"><span data-stu-id="bc61f-716">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="bc61f-717">Se ha quitado el elemento UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="bc61f-717">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="bc61f-718">Problema de seguimiento n.º 16400</span><span class="sxs-lookup"><span data-stu-id="bc61f-718">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="bc61f-719">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-719">**Old behavior**</span></span>

<span data-ttu-id="bc61f-720">Antes de EF Core 3.0, `UseRowNumberForPaging` se podía usar para generar SQL para la paginación de forma que fuera compatible con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="bc61f-720">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="bc61f-721">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-721">**New behavior**</span></span>

<span data-ttu-id="bc61f-722">A partir de EF Core 3.0, EF solo genera SQL para la paginación que únicamente es compatible con las versiones posteriores de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bc61f-722">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="bc61f-723">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-723">**Why**</span></span>

<span data-ttu-id="bc61f-724">El motivo de este cambio es que [SQL Server 2008 ya no se admite](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/). Además, la actualización de esta característica para que funcionase con los cambios en las consultas implementados en EF Core 3.0 llevaría mucho trabajo.</span><span class="sxs-lookup"><span data-stu-id="bc61f-724">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="bc61f-725">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-725">**Mitigations**</span></span>

<span data-ttu-id="bc61f-726">Se recomienda actualizar a una versión más reciente de SQL Server, o bien utilizar un nivel de compatibilidad superior, de modo que el SQL que se genere se admita.</span><span class="sxs-lookup"><span data-stu-id="bc61f-726">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="bc61f-727">Dicho esto, si no puede hacerlo, [escriba un comentario en el problema de seguimiento](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con los detalles al respecto.</span><span class="sxs-lookup"><span data-stu-id="bc61f-727">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="bc61f-728">En función de los comentarios, es posible que volvamos a valorar esta decisión.</span><span class="sxs-lookup"><span data-stu-id="bc61f-728">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="bc61f-729">La información o metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="bc61f-729">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="bc61f-730">Problema de seguimiento n.º 16119</span><span class="sxs-lookup"><span data-stu-id="bc61f-730">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="bc61f-731">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-731">**Old behavior**</span></span>

<span data-ttu-id="bc61f-732">`IDbContextOptionsExtension` incluía métodos para proporcionar metadatos sobre la extensión.</span><span class="sxs-lookup"><span data-stu-id="bc61f-732">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="bc61f-733">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-733">**New behavior**</span></span>

<span data-ttu-id="bc61f-734">Estos métodos se han movido a una nueva clase base abstracta `DbContextOptionsExtensionInfo`, que se devuelve desde una nueva propiedad `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-734">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="bc61f-735">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-735">**Why**</span></span>

<span data-ttu-id="bc61f-736">Al lanzarse las versiones 2.0 y 3.0, tuvimos que agregar o cambiar estos métodos varias veces.</span><span class="sxs-lookup"><span data-stu-id="bc61f-736">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="bc61f-737">Su división en una nueva clase base abstracta facilitará la realización de este tipo de cambios sin interrumpir las extensiones existentes.</span><span class="sxs-lookup"><span data-stu-id="bc61f-737">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="bc61f-738">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-738">**Mitigations**</span></span>

<span data-ttu-id="bc61f-739">Actualice las extensiones para seguir el nuevo patrón.</span><span class="sxs-lookup"><span data-stu-id="bc61f-739">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="bc61f-740">Encontrará ejemplos en las muchas implementaciones de `IDbContextOptionsExtension` para los diferentes tipos de extensiones en el código fuente de EF Core.</span><span class="sxs-lookup"><span data-stu-id="bc61f-740">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="bc61f-741">Cambio de nombre de LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="bc61f-741">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="bc61f-742">Problema de seguimiento n.º 10985</span><span class="sxs-lookup"><span data-stu-id="bc61f-742">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="bc61f-743">**Cambio**</span><span class="sxs-lookup"><span data-stu-id="bc61f-743">**Change**</span></span>

<span data-ttu-id="bc61f-744">Se ha cambiado el nombre de `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="bc61f-744">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="bc61f-745">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-745">**Why**</span></span>

<span data-ttu-id="bc61f-746">Conviene alinear el nombre de este evento de advertencia con el del resto de eventos de advertencia.</span><span class="sxs-lookup"><span data-stu-id="bc61f-746">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="bc61f-747">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-747">**Mitigations**</span></span>

<span data-ttu-id="bc61f-748">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="bc61f-748">Use the new name.</span></span> <span data-ttu-id="bc61f-749">(Tenga en cuenta que el número de id. evento sigue siendo el mismo).</span><span class="sxs-lookup"><span data-stu-id="bc61f-749">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="bc61f-750">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="bc61f-750">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="bc61f-751">Problema de seguimiento n.º 10730</span><span class="sxs-lookup"><span data-stu-id="bc61f-751">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="bc61f-752">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-752">**Old behavior**</span></span>

<span data-ttu-id="bc61f-753">Antes de EF Core 3.0, se utilizaba simplemente el término "nombre" para hacer referencia a los nombres de las restricciones de claves externas.</span><span class="sxs-lookup"><span data-stu-id="bc61f-753">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="bc61f-754">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-754">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="bc61f-755">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-755">**New behavior**</span></span>

<span data-ttu-id="bc61f-756">A partir de EF Core 3.0, el término con el que se hace referencia a los nombres de las restricciones de claves externas es "nombre de la restricción".</span><span class="sxs-lookup"><span data-stu-id="bc61f-756">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="bc61f-757">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-757">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="bc61f-758">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-758">**Why**</span></span>

<span data-ttu-id="bc61f-759">Este cambio permite mejorar la coherencia relativa a la nomenclatura en este aspecto y aclarar que se trata del nombre de una restricción de clave externa, y no del de la columna o propiedad en la que está definida la clave externa.</span><span class="sxs-lookup"><span data-stu-id="bc61f-759">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="bc61f-760">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-760">**Mitigations**</span></span>

<span data-ttu-id="bc61f-761">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="bc61f-761">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="bc61f-762">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="bc61f-762">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="bc61f-763">Problema de seguimiento n.° 15997</span><span class="sxs-lookup"><span data-stu-id="bc61f-763">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="bc61f-764">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-764">**Old behavior**</span></span>

<span data-ttu-id="bc61f-765">Antes de EF Core 3.0, estos métodos estaban protegidos.</span><span class="sxs-lookup"><span data-stu-id="bc61f-765">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="bc61f-766">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-766">**New behavior**</span></span>

<span data-ttu-id="bc61f-767">Desde EF Core 3.0, estos métodos son públicos.</span><span class="sxs-lookup"><span data-stu-id="bc61f-767">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="bc61f-768">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-768">**Why**</span></span>

<span data-ttu-id="bc61f-769">EF usa estos métodos para determinar si se ha creado una base de datos, pero está vacía.</span><span class="sxs-lookup"><span data-stu-id="bc61f-769">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="bc61f-770">Esto también puede resultar útil fuera de EF al determinar si se deben aplicar migraciones o no.</span><span class="sxs-lookup"><span data-stu-id="bc61f-770">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="bc61f-771">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-771">**Mitigations**</span></span>

<span data-ttu-id="bc61f-772">Cambie la accesibilidad de cualquier invalidación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-772">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="bc61f-773">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="bc61f-773">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="bc61f-774">Problema de seguimiento n.° 11506</span><span class="sxs-lookup"><span data-stu-id="bc61f-774">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="bc61f-775">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-775">**Old behavior**</span></span>

<span data-ttu-id="bc61f-776">Antes de EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un paquete NuGet regular con un ensamblado al que podían hacer referencia los proyectos que dependían de él.</span><span class="sxs-lookup"><span data-stu-id="bc61f-776">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="bc61f-777">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-777">**New behavior**</span></span>

<span data-ttu-id="bc61f-778">Desde EF Core 3.0, es un paquete DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="bc61f-778">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="bc61f-779">Esto significa que la dependencia no fluirá de manera transitiva en otros proyectos y que ya no puede, de forma predeterminada, hacer referencia a su ensamblado.</span><span class="sxs-lookup"><span data-stu-id="bc61f-779">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="bc61f-780">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-780">**Why**</span></span>

<span data-ttu-id="bc61f-781">Este paquete solo está destinado a usarse en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="bc61f-781">This package is only intended to be used at design time.</span></span> <span data-ttu-id="bc61f-782">Las aplicaciones implementadas no deben hacer referencia al mismo.</span><span class="sxs-lookup"><span data-stu-id="bc61f-782">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="bc61f-783">Hacer que el paquete sea DevelopmentDependency refuerza esta recomendación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-783">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="bc61f-784">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-784">**Mitigations**</span></span>

<span data-ttu-id="bc61f-785">Si tiene que hacer referencia a este paquete para invalidar el comportamiento en tiempo de diseño de EF Core, puede actualizar los metadatos de elementos PackageReference del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bc61f-785">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="bc61f-786">Si se hace referencia al paquete de manera transitiva a través de Microsoft.EntityFrameworkCore.Tools, tendrá que agregar una PackageReference explícita al paquete para cambiar sus metadatos.</span><span class="sxs-lookup"><span data-stu-id="bc61f-786">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="bc61f-787">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bc61f-787">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="bc61f-788">Problema de seguimiento n.° 14824</span><span class="sxs-lookup"><span data-stu-id="bc61f-788">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="bc61f-789">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-789">**Old behavior**</span></span>

<span data-ttu-id="bc61f-790">Microsoft.EntityFrameworkCore.Sqlite dependía anteriormente de la versión 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="bc61f-790">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="bc61f-791">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-791">**New behavior**</span></span>

<span data-ttu-id="bc61f-792">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="bc61f-792">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="bc61f-793">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-793">**Why**</span></span>

<span data-ttu-id="bc61f-794">La versión 2.0.0 de SQLitePCL.raw selecciona .NET Standard 2.0 como destino.</span><span class="sxs-lookup"><span data-stu-id="bc61f-794">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="bc61f-795">Anteriormente seleccionaba .NET Standard 1.1 como destino, que requería el cierre a gran escala de paquetes transitivos para su funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="bc61f-795">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="bc61f-796">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-796">**Mitigations**</span></span>

<span data-ttu-id="bc61f-797">En la versión 2.0.0 de SQLitePCL.raw se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="bc61f-797">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="bc61f-798">Consulte las [notas de la versión](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="bc61f-798">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="bc61f-799">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="bc61f-799">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="bc61f-800">Problema de seguimiento n.° 14825</span><span class="sxs-lookup"><span data-stu-id="bc61f-800">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="bc61f-801">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-801">**Old behavior**</span></span>

<span data-ttu-id="bc61f-802">Los paquetes espaciales anteriormente dependían de la versión 1.15.1 de NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="bc61f-802">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="bc61f-803">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-803">**New behavior**</span></span>

<span data-ttu-id="bc61f-804">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="bc61f-804">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="bc61f-805">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-805">**Why**</span></span>

<span data-ttu-id="bc61f-806">La versión 2.0.0 de NetTopologySuite pretende resolver varios problemas de usabilidad que encontraron los usuarios de EF Core.</span><span class="sxs-lookup"><span data-stu-id="bc61f-806">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="bc61f-807">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-807">**Mitigations**</span></span>

<span data-ttu-id="bc61f-808">En la versión 2.0.0 de NetTopologySuite se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="bc61f-808">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="bc61f-809">Consulte las [notas de la versión](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="bc61f-809">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="bc61f-810">Se deben configurar varias relaciones de referencia automática ambiguas</span><span class="sxs-lookup"><span data-stu-id="bc61f-810">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="bc61f-811">Problema de seguimiento n.º 13573</span><span class="sxs-lookup"><span data-stu-id="bc61f-811">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="bc61f-812">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-812">**Old behavior**</span></span>

<span data-ttu-id="bc61f-813">Un tipo de entidad con varias propiedades de navegación unidireccional de referencia automática y claves externas coincidentes se configuró incorrectamente como una única relación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-813">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="bc61f-814">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-814">For example:</span></span>

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

<span data-ttu-id="bc61f-815">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-815">**New behavior**</span></span>

<span data-ttu-id="bc61f-816">Este escenario se detecta ahora en la generación del modelo y se produce una excepción que indica que el modelo es ambiguo.</span><span class="sxs-lookup"><span data-stu-id="bc61f-816">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="bc61f-817">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-817">**Why**</span></span>

<span data-ttu-id="bc61f-818">El modelo resultante era ambiguo, y lo más probable es que sea incorrecto en este caso.</span><span class="sxs-lookup"><span data-stu-id="bc61f-818">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="bc61f-819">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-819">**Mitigations**</span></span>

<span data-ttu-id="bc61f-820">Utilice la configuración completa de la relación.</span><span class="sxs-lookup"><span data-stu-id="bc61f-820">Use full configuration of the relationship.</span></span> <span data-ttu-id="bc61f-821">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc61f-821">For example:</span></span>

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

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="bc61f-822">DbFunction.Schema es NULL o la cadena vacía lo configura para estar en el esquema predeterminado del modelo</span><span class="sxs-lookup"><span data-stu-id="bc61f-822">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="bc61f-823">Problema de seguimiento n.º 12757</span><span class="sxs-lookup"><span data-stu-id="bc61f-823">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="bc61f-824">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="bc61f-824">**Old behavior**</span></span>

<span data-ttu-id="bc61f-825">Una función DbFunction configurada con el esquema como una cadena vacía se trataba como una función integrada sin un esquema.</span><span class="sxs-lookup"><span data-stu-id="bc61f-825">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="bc61f-826">Por ejemplo, el código siguiente asignará la función CLR `DatePart` a la función integrada `DATEPART` en SqlServer.</span><span class="sxs-lookup"><span data-stu-id="bc61f-826">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="bc61f-827">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="bc61f-827">**New behavior**</span></span>

<span data-ttu-id="bc61f-828">Todas las asignaciones de DbFunction se consideran asignadas a funciones definidas por el usuario.</span><span class="sxs-lookup"><span data-stu-id="bc61f-828">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="bc61f-829">Por lo tanto, el valor de cadena vacía colocaría la función dentro del esquema predeterminado del modelo,</span><span class="sxs-lookup"><span data-stu-id="bc61f-829">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="bc61f-830">que podría ser el esquema configurado de forma explícita mediante `modelBuilder.HasDefaultSchema()` de la API fluida o `dbo` en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="bc61f-830">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="bc61f-831">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="bc61f-831">**Why**</span></span>

<span data-ttu-id="bc61f-832">Anteriormente, el esquema vacío era una manera de indicar que la función estaba integrada, pero esa lógica solo es aplicable a SqlServer, donde las funciones integradas no pertenecen a ningún esquema.</span><span class="sxs-lookup"><span data-stu-id="bc61f-832">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="bc61f-833">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="bc61f-833">**Mitigations**</span></span>

<span data-ttu-id="bc61f-834">Configure la traslación de DbFunction manualmente para asignarla a una función integrada.</span><span class="sxs-lookup"><span data-stu-id="bc61f-834">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
