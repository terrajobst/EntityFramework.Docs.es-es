---
title: 'Cambios importantes en EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: f02825f5303959997dca6e14e4efe64020b3cb22
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655877"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="c8ba1-102">Cambios importantes incluidos en EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c8ba1-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="c8ba1-103">Es posible que los siguientes cambios de API y comportamiento interrumpan las aplicaciones actuales cuando se actualicen a la versión 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="c8ba1-104">Los cambios que esperamos que solo afecten a proveedores de base de datos se documentan en [Cambios para proveedores](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="c8ba1-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="c8ba1-105">Resumen</span><span class="sxs-lookup"><span data-stu-id="c8ba1-105">Summary</span></span>

| <span data-ttu-id="c8ba1-106">**Cambio importante**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="c8ba1-107">**Impacto**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="c8ba1-108">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="c8ba1-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="c8ba1-109">Alto</span><span class="sxs-lookup"><span data-stu-id="c8ba1-109">High</span></span>       |
| [<span data-ttu-id="c8ba1-110">EF Core 3.0 tiene como destino .NET Standard 2.1, y no .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="c8ba1-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="c8ba1-111">Alto</span><span class="sxs-lookup"><span data-stu-id="c8ba1-111">High</span></span>      |
| [<span data-ttu-id="c8ba1-112">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="c8ba1-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="c8ba1-113">Alto</span><span class="sxs-lookup"><span data-stu-id="c8ba1-113">High</span></span>      |
| [<span data-ttu-id="c8ba1-114">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="c8ba1-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="c8ba1-115">Alto</span><span class="sxs-lookup"><span data-stu-id="c8ba1-115">High</span></span>      |
| [<span data-ttu-id="c8ba1-116">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="c8ba1-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="c8ba1-117">Alto</span><span class="sxs-lookup"><span data-stu-id="c8ba1-117">High</span></span>      |
| [<span data-ttu-id="c8ba1-118">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="c8ba1-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="c8ba1-119">Alto</span><span class="sxs-lookup"><span data-stu-id="c8ba1-119">High</span></span>      |
| [<span data-ttu-id="c8ba1-120">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8ba1-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="c8ba1-121">Medium</span><span class="sxs-lookup"><span data-stu-id="c8ba1-121">Medium</span></span>      |
| [<span data-ttu-id="c8ba1-122">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="c8ba1-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="c8ba1-123">Medium</span><span class="sxs-lookup"><span data-stu-id="c8ba1-123">Medium</span></span>      |
| [<span data-ttu-id="c8ba1-124">La carga diligente de entidades relacionadas ahora se realiza en una sola consulta</span><span class="sxs-lookup"><span data-stu-id="c8ba1-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="c8ba1-125">Medium</span><span class="sxs-lookup"><span data-stu-id="c8ba1-125">Medium</span></span>      |
| [<span data-ttu-id="c8ba1-126">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="c8ba1-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="c8ba1-127">Medium</span><span class="sxs-lookup"><span data-stu-id="c8ba1-127">Medium</span></span>      |
| [<span data-ttu-id="c8ba1-128">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="c8ba1-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="c8ba1-129">Medium</span><span class="sxs-lookup"><span data-stu-id="c8ba1-129">Medium</span></span>      |
| [<span data-ttu-id="c8ba1-130">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="c8ba1-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="c8ba1-131">Medium</span><span class="sxs-lookup"><span data-stu-id="c8ba1-131">Medium</span></span>      |
| [<span data-ttu-id="c8ba1-132">Las consultas sin seguimiento ya no realizan la resolución de la identidad</span><span class="sxs-lookup"><span data-stu-id="c8ba1-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="c8ba1-133">Medium</span><span class="sxs-lookup"><span data-stu-id="c8ba1-133">Medium</span></span>      |
| [<span data-ttu-id="c8ba1-134">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="c8ba1-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="c8ba1-135">Medium</span><span class="sxs-lookup"><span data-stu-id="c8ba1-135">Medium</span></span>      |
| [<span data-ttu-id="c8ba1-136">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="c8ba1-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="c8ba1-137">Medium</span><span class="sxs-lookup"><span data-stu-id="c8ba1-137">Medium</span></span>      |
| [<span data-ttu-id="c8ba1-138">Se ha quitado el elemento UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="c8ba1-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="c8ba1-139">Medium</span><span class="sxs-lookup"><span data-stu-id="c8ba1-139">Medium</span></span>      |
| [<span data-ttu-id="c8ba1-140">Cuando el método FromSql se usa con un procedimiento almacenado no se puede redactar</span><span class="sxs-lookup"><span data-stu-id="c8ba1-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="c8ba1-141">Medium</span><span class="sxs-lookup"><span data-stu-id="c8ba1-141">Medium</span></span>      |
| [<span data-ttu-id="c8ba1-142">Solo se pueden especificar métodos de FromSql en raíces de consulta</span><span class="sxs-lookup"><span data-stu-id="c8ba1-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="c8ba1-143">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-143">Low</span></span>      |
| [<span data-ttu-id="c8ba1-144">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="c8ba1-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="c8ba1-145">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-145">Low</span></span>      |
| [<span data-ttu-id="c8ba1-146">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="c8ba1-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="c8ba1-147">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-147">Low</span></span>      |
| [<span data-ttu-id="c8ba1-148">Las entidades dependientes que comparten la tabla con la entidad de seguridad son ahora opcionales</span><span class="sxs-lookup"><span data-stu-id="c8ba1-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="c8ba1-149">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-149">Low</span></span>      |
| [<span data-ttu-id="c8ba1-150">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="c8ba1-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="c8ba1-151">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-151">Low</span></span>      |
| [<span data-ttu-id="c8ba1-152">Las propiedades heredadas de tipos sin asignar se asignan ahora a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="c8ba1-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="c8ba1-153">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-153">Low</span></span>      |
| [<span data-ttu-id="c8ba1-154">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="c8ba1-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="c8ba1-155">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-155">Low</span></span>      |
| [<span data-ttu-id="c8ba1-156">La conexión de base de datos ahora se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="c8ba1-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="c8ba1-157">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-157">Low</span></span>      |
| [<span data-ttu-id="c8ba1-158">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="c8ba1-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="c8ba1-159">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-159">Low</span></span>      |
| [<span data-ttu-id="c8ba1-160">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="c8ba1-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="c8ba1-161">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-161">Low</span></span>      |
| [<span data-ttu-id="c8ba1-162">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="c8ba1-163">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-163">Low</span></span>      |
| [<span data-ttu-id="c8ba1-164">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="c8ba1-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="c8ba1-165">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-165">Low</span></span>      |
| [<span data-ttu-id="c8ba1-166">DbContext.Entry realiza ahora una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="c8ba1-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="c8ba1-167">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-167">Low</span></span>      |
| [<span data-ttu-id="c8ba1-168">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="c8ba1-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="c8ba1-169">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-169">Low</span></span>      |
| [<span data-ttu-id="c8ba1-170">ILoggerFactory es ahora un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="c8ba1-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="c8ba1-171">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-171">Low</span></span>      |
| [<span data-ttu-id="c8ba1-172">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="c8ba1-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="c8ba1-173">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-173">Low</span></span>      |
| [<span data-ttu-id="c8ba1-174">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="c8ba1-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="c8ba1-175">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-175">Low</span></span>      |
| [<span data-ttu-id="c8ba1-176">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="c8ba1-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="c8ba1-177">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-177">Low</span></span>      |
| [<span data-ttu-id="c8ba1-178">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="c8ba1-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="c8ba1-179">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-179">Low</span></span>      |
| [<span data-ttu-id="c8ba1-180">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="c8ba1-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="c8ba1-181">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-181">Low</span></span>      |
| [<span data-ttu-id="c8ba1-182">ToTable en un tipo derivado produce una excepción</span><span class="sxs-lookup"><span data-stu-id="c8ba1-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="c8ba1-183">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-183">Low</span></span>      |
| [<span data-ttu-id="c8ba1-184">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="c8ba1-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="c8ba1-185">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-185">Low</span></span>      |
| [<span data-ttu-id="c8ba1-186">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="c8ba1-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="c8ba1-187">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-187">Low</span></span>      |
| [<span data-ttu-id="c8ba1-188">Los valores GUID se almacenan ahora como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="c8ba1-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="c8ba1-189">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-189">Low</span></span>      |
| [<span data-ttu-id="c8ba1-190">Los valores char se almacenan ahora como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="c8ba1-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="c8ba1-191">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-191">Low</span></span>      |
| [<span data-ttu-id="c8ba1-192">Los id. de migración ahora se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="c8ba1-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="c8ba1-193">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-193">Low</span></span>      |
| [<span data-ttu-id="c8ba1-194">La información o los metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="c8ba1-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="c8ba1-195">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-195">Low</span></span>      |
| [<span data-ttu-id="c8ba1-196">LogQueryPossibleExceptionWithAggregateOperator ha cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="c8ba1-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="c8ba1-197">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-197">Low</span></span>      |
| [<span data-ttu-id="c8ba1-198">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="c8ba1-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="c8ba1-199">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-199">Low</span></span>      |
| [<span data-ttu-id="c8ba1-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="c8ba1-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="c8ba1-201">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-201">Low</span></span>      |
| [<span data-ttu-id="c8ba1-202">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="c8ba1-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="c8ba1-203">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-203">Low</span></span>      |
| [<span data-ttu-id="c8ba1-204">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ba1-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="c8ba1-205">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-205">Low</span></span>      |
| [<span data-ttu-id="c8ba1-206">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ba1-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="c8ba1-207">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-207">Low</span></span>      |
| [<span data-ttu-id="c8ba1-208">Se usa Microsoft.Data.SqlClient en lugar de System.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="c8ba1-208">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="c8ba1-209">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-209">Low</span></span>      |
| [<span data-ttu-id="c8ba1-210">Se deben configurar varias relaciones de referencia automática ambiguas</span><span class="sxs-lookup"><span data-stu-id="c8ba1-210">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="c8ba1-211">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-211">Low</span></span>      |
| [<span data-ttu-id="c8ba1-212">DbFunction.Schema es NULL o la cadena vacía lo configura para estar en el esquema predeterminado del modelo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-212">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="c8ba1-213">Bajo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-213">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="c8ba1-214">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="c8ba1-214">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="c8ba1-215">[Problema de seguimiento n.° 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Consulte también el problema n.° 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="c8ba1-215">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="c8ba1-216">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-216">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-217">Antes de 3.0, cuando en EF Core no se podía convertir una expresión que formaba parte de una consulta SQL o un parámetro, la expresión se evaluaba de forma automática en el cliente.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-217">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="c8ba1-218">De forma predeterminada, la evaluación de cliente de expresiones potencialmente costosas solo desencadenaba una advertencia.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-218">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="c8ba1-219">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-219">**New behavior**</span></span>

<span data-ttu-id="c8ba1-220">A partir de 3.0, en EF Core solo se permite que se evalúen en el cliente las expresiones en la proyección de nivel superior (la última llamada a `Select()` de la consulta).</span><span class="sxs-lookup"><span data-stu-id="c8ba1-220">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="c8ba1-221">Cuando las expresiones de otra parte de la consulta no se pueden convertir en SQL o un parámetro, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-221">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="c8ba1-222">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-222">**Why**</span></span>

<span data-ttu-id="c8ba1-223">La evaluación de cliente automática de las consultas permite que se ejecuten muchas consultas incluso si no se pueden convertir elementos importantes de ellas.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-223">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="c8ba1-224">Esto puede provocar un comportamiento inesperado y potencialmente dañino que es posible que solo sea evidente en entornos de producción.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-224">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="c8ba1-225">Por ejemplo, una condición en una llamada a `Where()` que no se puede convertir puede provocar que todas las filas de la tabla se transfieran desde el servidor de base de datos y que el filtro se aplique en el cliente.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-225">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="c8ba1-226">Esta situación puede pasar desapercibida fácilmente si la tabla solo contiene algunas filas en la fase de desarrollo, pero ser más grave cuando la aplicación pase a producción, donde la tabla puede contener millones de filas.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-226">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="c8ba1-227">Las advertencias de evaluación de cliente también se suelen pasar por alto durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-227">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="c8ba1-228">Además de esto, la evaluación de cliente automática puede causar problemas en los que la mejora de la traducción de consultas para expresiones específicas provocaba cambios importantes no deseados entre versiones.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-228">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="c8ba1-229">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-229">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-230">Si una consulta no se puede traducir totalmente, vuelva a escribirla en un formato que se pueda traducir, o bien use `AsEnumerable()`, `ToList()` o una función similar para devolver los datos al cliente de forma explícita, donde después se puedan seguir procesando mediante LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-230">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="c8ba1-231">EF Core 3.0 tiene como destino .NET Standard 2.1, y no .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="c8ba1-231">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="c8ba1-232">Problema de seguimiento n.º 15498</span><span class="sxs-lookup"><span data-stu-id="c8ba1-232">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="c8ba1-233">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-233">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-234">Antes de la versión 3.0, EF Core tenía como destino .NET Standard 2.0 y se podía ejecutar en todas las plataformas que admitieran dicho estándar, incluido .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-234">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="c8ba1-235">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-235">**New behavior**</span></span>

<span data-ttu-id="c8ba1-236">A partir de la versión 3.0, EF Core tiene como destino .NET Standard 2.1 y se puede ejecutar en todas las plataformas que admitan dicho estándar.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-236">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="c8ba1-237">Esto no incluye .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-237">This does not include .NET Framework.</span></span>

<span data-ttu-id="c8ba1-238">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-238">**Why**</span></span>

<span data-ttu-id="c8ba1-239">Esto forma parte de una decisión estratégica para todas las tecnologías de .NET que tiene como objetivo centrar los esfuerzos en .NET Core y otras plataformas modernas de .NET, como Xamarin.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-239">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="c8ba1-240">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-240">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-241">Valore la posibilidad de cambiar a una plataforma moderna de .NET.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-241">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="c8ba1-242">Si esto no es posible, siga usando EF Core 2.1 o EF Core 2.2, puesto que ambas versiones admiten .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-242">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="c8ba1-243">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8ba1-243">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="c8ba1-244">Anuncios del problema de seguimiento n.º 325</span><span class="sxs-lookup"><span data-stu-id="c8ba1-244">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="c8ba1-245">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-245">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-246">Antes de ASP.NET Core 3.0, cuando se agregaba una referencia de paquete a `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, se incluía EF Core y algunos de los proveedores de datos de EF Core, como el de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-246">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="c8ba1-247">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-247">**New behavior**</span></span>

<span data-ttu-id="c8ba1-248">A partir de la versión 3.0, el marco compartido ASP.NET Core no incluye EF Core ni ningún proveedor de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-248">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="c8ba1-249">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-249">**Why**</span></span>

<span data-ttu-id="c8ba1-250">Antes de este cambio, para obtener EF Core se necesitaban varios pasos en función de si la aplicación se destinaba a ASP.NET Core y SQL Server o no.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-250">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="c8ba1-251">Además, la actualización de ASP.NET Core forzaba la de EF Core y el proveedor de SQL Server, lo que no siempre es deseable.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-251">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="c8ba1-252">Con este cambio, la experiencia de obtención de EF Core es la misma en todos los proveedores, implementaciones admitidas de .NET y tipos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-252">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="c8ba1-253">Ahora los desarrolladores también pueden controlar exactamente cuándo se actualizan EF Core y los proveedores de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-253">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="c8ba1-254">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-254">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-255">Para usar EF Core en una aplicación ASP.NET Core 3.0 o cualquier otra aplicación compatible, debe agregar de forma explícita una referencia de paquete al proveedor de base de datos de EF Core que se va a usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-255">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="c8ba1-256">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="c8ba1-256">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="c8ba1-257">Problema de seguimiento n.º 14016</span><span class="sxs-lookup"><span data-stu-id="c8ba1-257">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="c8ba1-258">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-258">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-259">Antes de 3.0, la herramienta `dotnet ef` se incluía en el SDK de .NET Core y estaba disponible para usarse desde la línea de comandos de cualquier proyecto sin necesidad de realizar pasos adicionales.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-259">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="c8ba1-260">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-260">**New behavior**</span></span>

<span data-ttu-id="c8ba1-261">A partir de la versión 3.0, el SDK de .NET no incluye la herramienta `dotnet ef`, por lo que antes de poder usarla tendrá que instalarla de forma explícita como una herramienta local o global.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-261">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="c8ba1-262">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-262">**Why**</span></span>

<span data-ttu-id="c8ba1-263">Este cambio nos permite distribuir y actualizar `dotnet ef` como una herramienta convencional de la CLI de .NET en NuGet, coherente con el hecho de que la versión 3.0 de EF Core también se distribuye siempre como un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-263">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="c8ba1-264">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-264">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-265">Para poder administrar las migraciones o aplicar scaffolding a `DbContext`, instale `dotnet-ef` como herramienta global:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-265">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="c8ba1-266">También se puede obtener una herramienta local cuando se restauran las dependencias de un proyecto que la declara como una dependencia de herramientas mediante un [archivo de manifiesto de herramientas](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="c8ba1-266">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="c8ba1-267">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="c8ba1-267">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="c8ba1-268">Problema de seguimiento n.º 10996</span><span class="sxs-lookup"><span data-stu-id="c8ba1-268">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="c8ba1-269">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-269">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-270">Antes de EF Core 3.0, estos nombres de métodos se sobrecargaban para funcionar tanto con una cadena normal como con una cadena que se debería interpolar en SQL y parámetros.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-270">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="c8ba1-271">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-271">**New behavior**</span></span>

<span data-ttu-id="c8ba1-272">A partir de la versión EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw` y `ExecuteSqlRawAsync` para crear una consulta con parámetros donde los parámetros se pasan por separado de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-272">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="c8ba1-273">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-273">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="c8ba1-274">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated` y `ExecuteSqlInterpolatedAsync` para crear una consulta con parámetros donde los parámetros se pasan como parte de una cadena de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-274">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="c8ba1-275">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-275">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="c8ba1-276">Tenga en cuenta que las dos consultas anteriores producirán el mismo código SQL parametrizado con los mismos parámetros SQL.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-276">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="c8ba1-277">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-277">**Why**</span></span>

<span data-ttu-id="c8ba1-278">Las sobrecargas del método como esta facilitan las llamadas accidentales al método de cadena sin procesar cuando la intención era llamar al método de cadena interpolada y viceversa.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-278">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="c8ba1-279">Esto podría resultar en consultas que no se parametrizan cuando deberían.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-279">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="c8ba1-280">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-280">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-281">Haga el cambio para usar los nuevos nombres de métodos.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-281">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="c8ba1-282">Cuando el método FromSql se usa con un procedimiento almacenado no se puede redactar</span><span class="sxs-lookup"><span data-stu-id="c8ba1-282">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="c8ba1-283">Problema de seguimiento n.° 15392</span><span class="sxs-lookup"><span data-stu-id="c8ba1-283">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="c8ba1-284">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-284">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-285">Antes de EF Core 3.0, el método FromSql intentaba detectar si se podía redactar en el código SQL pasado.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-285">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="c8ba1-286">Cuando el código SQL no se podía redactar, como un procedimiento almacenado, realizaba la evaluación de cliente.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-286">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="c8ba1-287">La consulta siguiente funcionaba al ejecutar el procedimiento almacenado en el servidor y aplicar FirstOrDefault en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-287">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="c8ba1-288">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-288">**New behavior**</span></span>

<span data-ttu-id="c8ba1-289">A partir de EF Core 3.0, EF Core no intentará analizar el código SQL.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-289">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="c8ba1-290">Por tanto, si va a redactar después de FromSqlRaw/FromSqlInterpolated, EF Core redactará el código SQL generando una subconsulta.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-290">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="c8ba1-291">Por tanto, si usa un procedimiento almacenado con la redacción, obtendrá una excepción de sintaxis de SQL no válida.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-291">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="c8ba1-292">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-292">**Why**</span></span>

<span data-ttu-id="c8ba1-293">EF Core 3.0 no admite la evaluación automática de cliente, ya que era propenso a errores, como se explica [aquí](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="c8ba1-293">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="c8ba1-294">**Mitigación**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-294">**Mitigation**</span></span>

<span data-ttu-id="c8ba1-295">Si usa un procedimiento almacenado en FromSqlRaw/FromSqlInterpolated, sabe que no se puede redactar, por lo que puede agregar __AsEnumerable/AsAsyncEnumerable__ justo después de la llamada al método FromSql para evitar cualquier redacción en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-295">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="c8ba1-296">Solo se pueden especificar métodos de FromSql en raíces de consulta.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-296">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="c8ba1-297">Problema de seguimiento n.° 15704</span><span class="sxs-lookup"><span data-stu-id="c8ba1-297">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="c8ba1-298">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-298">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-299">Antes de EF Core 3.0, el método `FromSql` podía especificarse en cualquier lugar en la consulta.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-299">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="c8ba1-300">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-300">**New behavior**</span></span>

<span data-ttu-id="c8ba1-301">A partir de EF Core 3.0, los nuevos métodos `FromSqlRaw` y `FromSqlInterpolated` (que reemplazan a `FromSql`) solo pueden especificarse en las raíces de la consulta, es decir, directamente en `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-301">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="c8ba1-302">Si intenta especificarlos en cualquier otro lugar se producirá un error de compilación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-302">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="c8ba1-303">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-303">**Why**</span></span>

<span data-ttu-id="c8ba1-304">La especificación de `FromSql` en cualquier otro lugar diferente de `DbSet` no tenía un significado o valor agregado, y podría causar ambigüedad en ciertos escenarios.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-304">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="c8ba1-305">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-305">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-306">Las invocaciones de `FromSql` se deben mover para que estén directamente en el `DbSet` al que se aplican.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-306">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="c8ba1-307">Las consultas sin seguimiento ya no realizan la resolución de la identidad</span><span class="sxs-lookup"><span data-stu-id="c8ba1-307">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="c8ba1-308">Problema de seguimiento n.º 13518</span><span class="sxs-lookup"><span data-stu-id="c8ba1-308">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="c8ba1-309">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-309">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-310">Antes de EF Core 3.0, se usaba la misma instancia de la entidad para cada aparición de una entidad con un tipo e identificador determinados.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-310">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="c8ba1-311">Este comportamiento coincide con el de las consultas de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-311">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="c8ba1-312">Por ejemplo, esta consulta:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-312">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="c8ba1-313">Esta consulta devolverá la misma instancia de `Category` para cada elemento `Product` asociado con la categoría determinada.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-313">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="c8ba1-314">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-314">**New behavior**</span></span>

<span data-ttu-id="c8ba1-315">A partir de EF Core 3.0, se crean distintas instancias de la entidad si se encuentra una entidad con un tipo e identificador determinados en varias ubicaciones del gráfico devuelto.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-315">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="c8ba1-316">Por ejemplo, la consulta anterior ahora devolverá una nueva instancia de `Category` para cada elemento `Product` cuando haya dos productos asociados a la misma categoría.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-316">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="c8ba1-317">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-317">**Why**</span></span>

<span data-ttu-id="c8ba1-318">La resolución de las identidades (es decir, el hecho de determinar que una entidad tiene los mismos tipo e identificador que la entidad encontrada) agrega más rendimiento y sobrecarga de memoria.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-318">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="c8ba1-319">Este enfoque suele ser contrario a por qué las consultas sin seguimiento se usan en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-319">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="c8ba1-320">Además, aunque la resolución de las identidades a veces puede resultar útil, no es necesaria si las entidades se van a serializar y enviar a un cliente, algo habitual para las consultas sin seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-320">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="c8ba1-321">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-321">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-322">Si se requiere la resolución de identidad, use una consulta de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-322">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="c8ba1-323">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="c8ba1-323">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="c8ba1-324">Problema de seguimiento n.º 14523</span><span class="sxs-lookup"><span data-stu-id="c8ba1-324">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="c8ba1-325">Revertimos este cambio porque la nueva configuración de EF Core 3.0 permite a la aplicación especificar el nivel de registro para cualquier evento.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-325">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="c8ba1-326">Por ejemplo, para cambiar el registro de SQL a `Debug`, configure el nivel de forma explícita en `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-326">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="c8ba1-327">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="c8ba1-327">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="c8ba1-328">Problema de seguimiento n.º 12378</span><span class="sxs-lookup"><span data-stu-id="c8ba1-328">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="c8ba1-329">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-329">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-330">Antes de EF Core 3.0, los valores temporales se asignaban a todas las propiedades de clave para las que posteriormente la base de datos generaba un valor real.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-330">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="c8ba1-331">Normalmente, estos valores temporales eran números negativos grandes.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-331">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="c8ba1-332">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-332">**New behavior**</span></span>

<span data-ttu-id="c8ba1-333">A partir de la versión 3.0, en EF Core se almacena el valor de clave temporal como parte de la información de seguimiento de la entidad y la propiedad clave en sí no se modifica.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-333">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="c8ba1-334">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-334">**Why**</span></span>

<span data-ttu-id="c8ba1-335">Este cambio se ha realizado para evitar que los valores de clave temporales se conviertan erróneamente en permanentes cuando una entidad de la que previamente una instancia de `DbContext` ha realizado el seguimiento se mueve a otra instancia de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-335">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="c8ba1-336">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-336">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-337">Las aplicaciones que asignan valores de clave principal a claves externas para crear asociaciones entre entidades pueden depender del comportamiento anterior si las claves principales son generadas por el almacén y pertenecen a entidades en el estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-337">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="c8ba1-338">Esto se puede evitar con las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-338">This can be avoided by:</span></span>
* <span data-ttu-id="c8ba1-339">No se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-339">Not using store-generated keys.</span></span>
* <span data-ttu-id="c8ba1-340">Se establecen propiedades de navegación para crear relaciones en lugar de establecer valores de clave externa.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-340">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="c8ba1-341">Se obtienen los valores de clave temporal reales de la información de seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-341">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="c8ba1-342">Por ejemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` devolverá el valor temporal aunque no se haya establecido `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-342">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="c8ba1-343">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="c8ba1-343">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="c8ba1-344">Problema de seguimiento n.º 14616</span><span class="sxs-lookup"><span data-stu-id="c8ba1-344">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="c8ba1-345">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-345">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-346">Antes de EF Core 3.0, se realizaba el seguimiento en el estado `Added` de las entidades sin seguimiento detectadas por `DetectChanges` y se insertaban como una fila nueva cuando se llamaba a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-346">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="c8ba1-347">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-347">**New behavior**</span></span>

<span data-ttu-id="c8ba1-348">A partir de EF Core 3.0, si una entidad usa valores de clave generados y se establece un valor de clave, se realizará el seguimiento de la entidad en el estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-348">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="c8ba1-349">Esto significa que se supone que existe una fila para la entidad y que se actualizará cuando se llame a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-349">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="c8ba1-350">Si no se establece el valor de clave, o bien si el tipo de entidad no usa claves generadas, se seguirá realizando el seguimiento de la entidad nueva como `Added` al igual que en las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-350">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="c8ba1-351">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-351">**Why**</span></span>

<span data-ttu-id="c8ba1-352">Este cambio se ha realizado para que sea más sencillo y coherente trabajar con gráficos de entidades desconectadas mientras se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-352">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="c8ba1-353">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-353">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-354">Este cambio puede interrumpir una aplicación si se configura un tipo de entidad para usar claves generadas, pero se establecen de forma explícita valores de clave para las instancias nuevas.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-354">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="c8ba1-355">La solución consiste en configurar de forma explícita las propiedades de clave para que no usen valores generados.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-355">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="c8ba1-356">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-356">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="c8ba1-357">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-357">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="c8ba1-358">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="c8ba1-358">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="c8ba1-359">Problema de seguimiento n.º 10114</span><span class="sxs-lookup"><span data-stu-id="c8ba1-359">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="c8ba1-360">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-360">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-361">Antes de la versión 3.0, en EF Core no se aplicaban acciones en cascada (eliminación de entidades dependientes cuando se eliminaba una entidad de seguridad obligatoria o cuando se rompía la relación con una entidad de seguridad obligatoria) hasta que se llamaba a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-361">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="c8ba1-362">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-362">**New behavior**</span></span>

<span data-ttu-id="c8ba1-363">A partir de 3.0, en EF Core las acciones en cascada se aplican en cuanto se detecta la condición desencadenadora.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-363">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="c8ba1-364">Por ejemplo, como resultado de la llamada a `context.Remove()` para eliminar una entidad de seguridad, todos los dependientes obligatorios relacionados de los que se realiza el seguimiento también se establecen en `Deleted` inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-364">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="c8ba1-365">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-365">**Why**</span></span>

<span data-ttu-id="c8ba1-366">Este cambio se ha realizado para mejorar la experiencia en escenarios de auditoría y enlace de datos donde es importante comprender qué entidades se van a eliminar _antes_ de llamar a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-366">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="c8ba1-367">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-367">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-368">El comportamiento anterior se puede restaurar mediante opciones de `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-368">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="c8ba1-369">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-369">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="c8ba1-370">La carga diligente de entidades relacionadas ahora se realiza en una sola consulta</span><span class="sxs-lookup"><span data-stu-id="c8ba1-370">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="c8ba1-371">Problema de seguimiento n.º 18022</span><span class="sxs-lookup"><span data-stu-id="c8ba1-371">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="c8ba1-372">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-372">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-373">Antes de la versión 3.0, la carga diligente de navegaciones de colección a través de operadores `Include` provocaba la generación de varias consultas en la base de datos relacional, una para cada tipo de entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-373">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="c8ba1-374">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-374">**New behavior**</span></span>

<span data-ttu-id="c8ba1-375">A partir de la versión 3.0, EF Core genera una sola consulta con operadores JOIN en las bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-375">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="c8ba1-376">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-376">**Why**</span></span>

<span data-ttu-id="c8ba1-377">La emisión de varias consultas para implementar una única consulta LINQ provocaba numerosos problemas, incluido el rendimiento negativo, ya que se necesitaban varios recorridos de ida y vuelta a la base de datos, y problemas de coherencia de datos, ya que cada consulta podía observar un estado distinto de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-377">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="c8ba1-378">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-378">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-379">Aunque técnicamente esto no es un cambio importante, podría tener un efecto considerable en el rendimiento de la aplicación cuando una sola consulta contiene un gran número de operadores `Include` en las navegaciones de la colección.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-379">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="c8ba1-380">[Vea este comentario](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) para obtener más información y para volver a escribir las consultas de una manera más eficaz.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-380">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="c8ba1-381">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="c8ba1-381">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="c8ba1-382">Problema de seguimiento n.º 12661</span><span class="sxs-lookup"><span data-stu-id="c8ba1-382">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="c8ba1-383">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-383">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-384">Antes de la versión 3.0, `DeleteBehavior.Restrict` creaba claves externas en la base de datos con la semántica `Restrict`, pero también realizaba una corrección interna de manera no evidente.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-384">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="c8ba1-385">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-385">**New behavior**</span></span>

<span data-ttu-id="c8ba1-386">A partir de la versión 3.0, `DeleteBehavior.Restrict` garantiza que las claves externas se crean con la semántica `Restrict`, es decir, sin cascadas y realizando una infracción de restricción, sin llevar a cabo correcciones internas de EF.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-386">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="c8ba1-387">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-387">**Why**</span></span>

<span data-ttu-id="c8ba1-388">Este cambio se realizó para mejorar la experiencia de uso de `DeleteBehavior` de manera intuitiva sin efectos secundarios inesperados.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-388">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="c8ba1-389">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-389">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-390">El comportamiento anterior se puede restaurar con `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-390">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="c8ba1-391">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="c8ba1-391">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="c8ba1-392">Problema de seguimiento n.º 14194</span><span class="sxs-lookup"><span data-stu-id="c8ba1-392">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="c8ba1-393">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-393">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-394">Antes de EF Core 3.0, los [tipos de consulta](xref:core/modeling/keyless-entity-types) eran un medio para consultar los datos que no definen una clave principal de una manera estructurada.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-394">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="c8ba1-395">Es decir, un tipo de consulta se usaba para asignar tipos de entidad sin claves (más probablemente desde una vista, pero posiblemente desde una tabla), mientras que un tipo de entidad estándar se usaba cuando había una clave disponible (más probablemente desde una tabla, pero posiblemente desde una vista).</span><span class="sxs-lookup"><span data-stu-id="c8ba1-395">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="c8ba1-396">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-396">**New behavior**</span></span>

<span data-ttu-id="c8ba1-397">Ahora un tipo de consulta se convierte en un tipo de entidad sin una clave principal.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-397">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="c8ba1-398">Los tipos de entidad sin clave tienen la misma funcionalidad que los tipos de consulta de las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-398">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="c8ba1-399">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-399">**Why**</span></span>

<span data-ttu-id="c8ba1-400">Este cambio se ha realizado para reducir la confusión en torno a la finalidad de los tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-400">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="c8ba1-401">En concreto, son tipos de entidad sin clave y, por ello, intrínsecamente son de solo lectura, pero no se deben usar solo porque un tipo de entidad tenga que ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-401">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="c8ba1-402">Del mismo modo, se suelen asignar a vistas, pero solo porque las vistas no suelen definir claves.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-402">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="c8ba1-403">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-403">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-404">Los elementos siguientes de la API ahora están obsoletos:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-404">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="c8ba1-405">**`ModelBuilder.Query<>()`** : en su lugar es necesario llamar a `ModelBuilder.Entity<>().HasNoKey()` para marcar un tipo de entidad como sin claves.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-405">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="c8ba1-406">Esto todavía no se configurará por convención para evitar una configuración incorrecta cuando se espera una clave principal, pero no coincide con la convención.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-406">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="c8ba1-407">**`DbQuery<>`** : en su lugar se debe usar `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-407">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="c8ba1-408">**`DbContext.Query<>()`** : en su lugar se debe usar `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-408">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="c8ba1-409">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="c8ba1-409">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="c8ba1-410">[Problema de seguimiento n.º 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema de seguimiento n.º 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema de seguimiento n.º 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="c8ba1-410">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="c8ba1-411">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-411">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-412">Antes de EF Core 3.0, la configuración de la relación de propiedad se realizaba directamente después de la llamada a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-412">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="c8ba1-413">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-413">**New behavior**</span></span>

<span data-ttu-id="c8ba1-414">A partir de EF Core 3.0, ahora hay una API fluida para configurar una propiedad de navegación para el propietario mediante `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-414">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="c8ba1-415">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-415">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="c8ba1-416">La configuración relacionada con la relación entre el propietario y lo que se posee ahora se debe encadenar después de `WithOwner()`, de forma similar a cómo se configuran otras relaciones.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-416">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="c8ba1-417">Pero la configuración del propio tipo de propiedad se seguirá encadenando después de `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-417">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="c8ba1-418">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-418">For example:</span></span>

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

<span data-ttu-id="c8ba1-419">Además, la llamada a `Entity()`, `HasOne()` o `Set()` con un tipo de propiedad de destino ahora iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-419">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="c8ba1-420">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-420">**Why**</span></span>

<span data-ttu-id="c8ba1-421">Este cambio se ha realizado para crear una separación más clara entre la configuración del propio tipo de propiedad y la _relación con_ el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-421">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="c8ba1-422">A su vez, esto elimina la ambigüedad y la confusión de métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-422">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="c8ba1-423">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-423">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-424">Cambie la configuración de las relaciones de tipo de propiedad para usar la nueva superficie de API, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-424">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="c8ba1-425">Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales</span><span class="sxs-lookup"><span data-stu-id="c8ba1-425">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="c8ba1-426">Problema de seguimiento n.º 9005</span><span class="sxs-lookup"><span data-stu-id="c8ba1-426">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="c8ba1-427">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-427">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-428">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-428">Consider the following model:</span></span>
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
<span data-ttu-id="c8ba1-429">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, siempre era necesaria una instancia de `OrderDetails` al agregar un elemento `Order` nuevo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-429">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="c8ba1-430">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-430">**New behavior**</span></span>

<span data-ttu-id="c8ba1-431">A partir de la versión 3.0, EF Core permite agregar `Order` sin `OrderDetails` y asigna todas las propiedades `OrderDetails` a excepción de la clave principal a columnas que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-431">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="c8ba1-432">Al realizar consultas, EF Core establece `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor o si no tiene propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-432">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="c8ba1-433">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-433">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-434">Si el modelo tiene una tabla que comparte dependencias con todas las columnas opcionales, pero la navegación que apunta a ella no se espera que sea `null`, la aplicación debería modificarse para controlar los casos en los que la navegación sea `null`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-434">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="c8ba1-435">Si esto no es posible, debería agregarse una propiedad necesaria al tipo de entidad o, al menos, una entidad debería tener un valor distinto a `null` asignado.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-435">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="c8ba1-436">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="c8ba1-436">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="c8ba1-437">Problema de seguimiento n.º 14154</span><span class="sxs-lookup"><span data-stu-id="c8ba1-437">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="c8ba1-438">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-438">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-439">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-439">Consider the following model:</span></span>
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
<span data-ttu-id="c8ba1-440">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, si solo se actualizaba `OrderDetails`, no se actualizaba el valor `Version` en el cliente y se producía un error en la próxima actualización.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-440">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="c8ba1-441">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-441">**New behavior**</span></span>

<span data-ttu-id="c8ba1-442">A partir de la versión 3.0, EF Core propaga el nuevo valor `Version` en `Order` si posee `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-442">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="c8ba1-443">En caso contrario, se produce una excepción durante la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-443">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="c8ba1-444">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-444">**Why**</span></span>

<span data-ttu-id="c8ba1-445">Este cambio se realizó para evitar un valor de token de simultaneidad obsoleto cuando solo se actualiza una de las entidades asignadas a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-445">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="c8ba1-446">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-446">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-447">Todas las entidades que comparten la tabla deben incluir una propiedad que se asigna a la columna del token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-447">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="c8ba1-448">Es posible crear una en estado reemplazado:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-448">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="c8ba1-449">Ahora, las propiedades heredadas de tipos sin asignar se asignan a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="c8ba1-449">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="c8ba1-450">Problema de seguimiento n.º 13998</span><span class="sxs-lookup"><span data-stu-id="c8ba1-450">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="c8ba1-451">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-451">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-452">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-452">Consider the following model:</span></span>
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

<span data-ttu-id="c8ba1-453">Antes de EF Core 3.0, la propiedad `ShippingAddress` se asignaba a columnas distintas para `BulkOrder` y `Order` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-453">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="c8ba1-454">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-454">**New behavior**</span></span>

<span data-ttu-id="c8ba1-455">A partir de la versión3.0, EF Core solo crea una columna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-455">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="c8ba1-456">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-456">**Why**</span></span>

<span data-ttu-id="c8ba1-457">El comportamiento anterior no era el esperado.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-457">The old behavoir was unexpected.</span></span>

<span data-ttu-id="c8ba1-458">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-458">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-459">Todavía se puede asignar explícitamente la propiedad a columnas separadas en los tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-459">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="c8ba1-460">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="c8ba1-460">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="c8ba1-461">Problema de seguimiento n.º 13274</span><span class="sxs-lookup"><span data-stu-id="c8ba1-461">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="c8ba1-462">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-462">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-463">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-463">Consider the following model:</span></span>
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
<span data-ttu-id="c8ba1-464">Antes de EF Core 3.0, se podía usar la propiedad `CustomerId` para la clave externa por convención.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-464">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="c8ba1-465">Pero si `Order` es un tipo de propiedad, entonces esto convertiría también a `CustomerId` en la clave principal, algo que no suele ser lo esperado.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-465">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="c8ba1-466">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-466">**New behavior**</span></span>

<span data-ttu-id="c8ba1-467">A partir de la versión 3.0, EF Core no intenta usar las propiedades de claves externas por convención si tienen el mismo nombre que la propiedad de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-467">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="c8ba1-468">Los patrones de nombre de tipo de entidad de seguridad concatenado con el nombre de propiedad de la entidad de seguridad y de nombre de navegación concatenado con el nombre de propiedad de la entidad de seguridad todavía se hacen coincidir.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-468">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="c8ba1-469">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-469">For example:</span></span>

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

<span data-ttu-id="c8ba1-470">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-470">**Why**</span></span>

<span data-ttu-id="c8ba1-471">Este cambio se ha realizado para evitar definir erróneamente una propiedad de clave principal en el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-471">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="c8ba1-472">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-472">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-473">Si la propiedad se ha diseñado para ser la clave externa y, por tanto, parte de la clave principal, se debe configurar explícitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-473">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="c8ba1-474">Ahora, la conexión de base de datos se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="c8ba1-474">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="c8ba1-475">Problema de seguimiento n.º 14218</span><span class="sxs-lookup"><span data-stu-id="c8ba1-475">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="c8ba1-476">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-476">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-477">Antes de EF Core 3.0, si el contexto abría la conexión dentro de `TransactionScope`, la conexión permanecía abierta mientras el ámbito actual `TransactionScope` estuviese activo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-477">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="c8ba1-478">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-478">**New behavior**</span></span>

<span data-ttu-id="c8ba1-479">A partir de la versión 3.0, EF Core cierra la conexión en cuanto se deja de usar.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-479">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="c8ba1-480">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-480">**Why**</span></span>

<span data-ttu-id="c8ba1-481">Este cambio permite usar varios contextos en el mismo ámbito `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-481">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="c8ba1-482">El comportamiento nuevo también coincide con el de EF6.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-482">The new behavior also matches EF6.</span></span>

<span data-ttu-id="c8ba1-483">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-483">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-484">Si la conexión debe permanecer abierta, una llamada explícita a `OpenConnection()` asegurará que EF Core no la cierre de forma prematura:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-484">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="c8ba1-485">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="c8ba1-485">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="c8ba1-486">Problema de seguimiento n.º 6872</span><span class="sxs-lookup"><span data-stu-id="c8ba1-486">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="c8ba1-487">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-487">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-488">Antes de EF Core 3.0, se usaba un generador de valores compartidos para todas las propiedades de clave entera en memoria.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-488">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="c8ba1-489">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-489">**New behavior**</span></span>

<span data-ttu-id="c8ba1-490">A partir de EF Core 3.0, cada propiedad de clave entera obtiene su propio generador de valores cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-490">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="c8ba1-491">Además, si se elimina la base de datos, la generación de claves se restablece para todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-491">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="c8ba1-492">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-492">**Why**</span></span>

<span data-ttu-id="c8ba1-493">Este cambio se ha realizado para alinear la generación de claves en memoria más estrechamente a la generación de claves de base de datos reales y para mejorar la capacidad para aislar las pruebas entre sí cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-493">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="c8ba1-494">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-494">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-495">Esto puede interrumpir una aplicación que se base en el establecimiento de valores de clave específicos en memoria.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-495">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="c8ba1-496">En su lugar, considere la posibilidad de no depender de valores de clave específicos, o bien de actualizar para que coincida con el comportamiento nuevo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-496">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="c8ba1-497">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="c8ba1-497">Backing fields are used by default</span></span>

[<span data-ttu-id="c8ba1-498">Problema de seguimiento n.º 12430</span><span class="sxs-lookup"><span data-stu-id="c8ba1-498">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="c8ba1-499">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-499">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-500">Antes de la versión 3.0, incluso si se conocía el campo de respaldo de una propiedad, de forma predeterminada en EF Core se leía y escribía el valor de propiedad mediante los métodos captadores y establecedores de propiedades.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-500">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="c8ba1-501">La excepción era la ejecución de consultas, donde el campo de respaldo se establecía directamente si se conocía.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-501">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="c8ba1-502">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-502">**New behavior**</span></span>

<span data-ttu-id="c8ba1-503">A partir de EF Core 3.0, si se conoce el campo de respaldo para una propiedad, EF Core siempre la leerá y escribirá mediante el campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-503">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="c8ba1-504">Esto podría provocar una interrupción de la aplicación si depende de un comportamiento adicional codificado en los métodos captadores o establecedores.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-504">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="c8ba1-505">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-505">**Why**</span></span>

<span data-ttu-id="c8ba1-506">Este cambio se ha realizado para evitar que EF Core desencadene erróneamente lógica de negocios de forma predeterminada al realizar operaciones de base de datos que implican entidades.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-506">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="c8ba1-507">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-507">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-508">El comportamiento anterior a la versión 3.0 se puede restaurar mediante la configuración del modo de acceso de propiedad en `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-508">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="c8ba1-509">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-509">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="c8ba1-510">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="c8ba1-510">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="c8ba1-511">Problema de seguimiento n.º 12523</span><span class="sxs-lookup"><span data-stu-id="c8ba1-511">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="c8ba1-512">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-512">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-513">Antes de EF Core 3.0, si varios campos coincidían con las reglas para buscar el campo de respaldo de una propiedad, se elegía un campo según un orden de prioridad.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-513">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="c8ba1-514">Esto podía producir que, en caso de ambigüedad, se usara el campo incorrecto.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-514">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="c8ba1-515">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-515">**New behavior**</span></span>

<span data-ttu-id="c8ba1-516">A partir de EF Core 3.0, si varios campos coinciden con la misma propiedad, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-516">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="c8ba1-517">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-517">**Why**</span></span>

<span data-ttu-id="c8ba1-518">Este cambio se ha realizado para evitar de forma silenciosa el uso de un campo con respecto a otro cuando solo uno puede ser correcto.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-518">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="c8ba1-519">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-519">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-520">En las propiedades con campos de respaldo ambiguos se debe especificar de forma explícita el campo que se va usar.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-520">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="c8ba1-521">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-521">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="c8ba1-522">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-522">Field-only property names should match the field name</span></span>

<span data-ttu-id="c8ba1-523">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-523">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-524">Antes de EF Core 3.0, una propiedad podía especificarse con un valor de cadena y, si no había ninguna propiedad con ese nombre en el tipo .NET, EF Core intentaba hacerla coincidir con un campo mediante reglas de convención.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-524">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="c8ba1-525">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-525">**New behavior**</span></span>

<span data-ttu-id="c8ba1-526">A partir de EF Core 3.0, una propiedad de solo campo debe coincidir exactamente con el nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-526">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="c8ba1-527">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-527">**Why**</span></span>

<span data-ttu-id="c8ba1-528">Este cambio se realizó para evitar el uso del mismo campo para dos propiedades con nombres similares. También hace que las reglas de coincidencia para propiedades solo de campo sean las mismas que para las propiedades asignadas a propiedades CLR.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-528">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="c8ba1-529">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-529">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-530">Las propiedades solo de campo deberían tener el mismo nombre que el campo al que están asignadas.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-530">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="c8ba1-531">En una próxima versión de EF Core 3.0 tenemos planeado volver a habilitar la configuración explícita de un nombre de campo distinto al nombre de la propiedad (vea el problema [n.° 15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="c8ba1-531">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="c8ba1-532">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="c8ba1-532">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="c8ba1-533">Problema de seguimiento n.° 14756</span><span class="sxs-lookup"><span data-stu-id="c8ba1-533">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="c8ba1-534">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-534">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-535">Antes de EF Core 3.0, llamar a `AddDbContext` o a `AddDbContextPool` también podría registrar los servicios de almacenamiento en caché y de registro con D.I. a través de llamadas a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) y a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="c8ba1-535">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="c8ba1-536">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-536">**New behavior**</span></span>

<span data-ttu-id="c8ba1-537">A partir de EF Core 3.0, `AddDbContext` y `AddDbContextPool` ya no registrarán estos servicios con inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="c8ba1-537">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="c8ba1-538">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-538">**Why**</span></span>

<span data-ttu-id="c8ba1-539">EF Core 3.0 no requiere que estos servicios estén en el contenedor de inserción de dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-539">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="c8ba1-540">Pero si `ILoggerFactory` se registra en el contenedor de DI de la aplicación, EF Core lo empezará a usar de todos modos.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-540">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="c8ba1-541">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-541">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-542">Si la aplicación necesita estos servicios, regístrelos de manera explícita con el contenedor de DI mediante [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="c8ba1-542">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="c8ba1-543">Ahora DbContext.Entry realiza una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="c8ba1-543">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="c8ba1-544">Problema de seguimiento n.º 13552</span><span class="sxs-lookup"><span data-stu-id="c8ba1-544">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="c8ba1-545">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-545">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-546">Antes de EF Core 3.0, la llamada a `DbContext.Entry` provocaba que se detectaran cambios para todas las entidades con seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-546">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="c8ba1-547">Esto garantizaba que el estado expuesto en `EntityEntry` estuviera actualizado.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-547">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="c8ba1-548">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-548">**New behavior**</span></span>

<span data-ttu-id="c8ba1-549">A partir de EF Core 3.0, ahora la llamada a `DbContext.Entry` solo intenta detectar cambios en la entidad dada y cualquier entidad de seguridad relacionada con ella de la que se haya realizado el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-549">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="c8ba1-550">Esto significa que es posible que la llamada a este método no haya detectado otros cambios, lo que podría tener implicaciones en el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-550">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="c8ba1-551">Observe que si `ChangeTracker.AutoDetectChangesEnabled` se establece en `false` incluso esta detección de cambios local se deshabilitará.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-551">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="c8ba1-552">Otros métodos que provocan la detección de cambios (como `ChangeTracker.Entries` y `SaveChanges`) siguen provocando una acción `DetectChanges` completa de todas las entidades de las que se realiza el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-552">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="c8ba1-553">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-553">**Why**</span></span>

<span data-ttu-id="c8ba1-554">Este cambio se ha realizado para mejorar el rendimiento predeterminado del uso de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-554">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="c8ba1-555">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-555">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-556">Llame a `ChgangeTracker.DetectChanges()` de forma explícita antes de llamar a `Entry` para garantizar el comportamiento anterior a la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-556">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="c8ba1-557">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="c8ba1-557">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="c8ba1-558">Problema de seguimiento n.º 14617</span><span class="sxs-lookup"><span data-stu-id="c8ba1-558">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="c8ba1-559">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-559">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-560">Antes de EF Core 3.0, se podían usar las propiedades de clave `string` y `byte[]` sin tener que establecer de forma explícita un valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-560">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="c8ba1-561">En ese caso, el valor de clave se generaba en el cliente como un GUID, que se serializaba en bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-561">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="c8ba1-562">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-562">**New behavior**</span></span>

<span data-ttu-id="c8ba1-563">A partir de EF Core 3.0, se iniciará una excepción en la que indica que no se ha establecido ningún valor de clave.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-563">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="c8ba1-564">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-564">**Why**</span></span>

<span data-ttu-id="c8ba1-565">Este cambio se ha realizado porque los valores `string`/`byte[]` generados por el cliente no suelen ser útiles, y el comportamiento predeterminado dificultaba razonar sobre los valores de clave generados de una forma habitual.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-565">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="c8ba1-566">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-566">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-567">Se puede obtener el comportamiento anterior a la versión 3.0 si se especifica de forma explícita que las propiedades de clave deben usar los valores generados si no se establece ningún otro valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-567">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="c8ba1-568">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-568">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="c8ba1-569">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-569">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="c8ba1-570">Ahora ILoggerFactory es un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="c8ba1-570">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="c8ba1-571">Problema de seguimiento n.º 14698</span><span class="sxs-lookup"><span data-stu-id="c8ba1-571">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="c8ba1-572">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-572">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-573">Antes de EF Core 3.0, `ILoggerFactory` se registraba como un servicio de singleton.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-573">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="c8ba1-574">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-574">**New behavior**</span></span>

<span data-ttu-id="c8ba1-575">A partir de EF Core 3.0, `ILoggerFactory` ahora se registra como con ámbito.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-575">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="c8ba1-576">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-576">**Why**</span></span>

<span data-ttu-id="c8ba1-577">Este cambio se ha realizado para permitir la asociación de un registrador con una instancia de `DbContext`, lo que habilita otras funciones y quita algunos casos de comportamiento patológico como un aumento vertiginoso de los proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-577">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="c8ba1-578">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-578">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-579">Este cambio no debería afectar al código de la aplicación a menos que registre y use servicios personalizados en el proveedor de servicios internos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-579">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="c8ba1-580">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-580">This isn't common.</span></span>
<span data-ttu-id="c8ba1-581">En estos casos, la mayoría de los elementos seguirá funcionando, pero cualquier servicio de singleton que dependiera de `ILoggerFactory` tendrá que cambiarse para obtener la interfaz `ILoggerFactory` de otra forma.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-581">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="c8ba1-582">Si experimenta situaciones como esta, registre un problema en el [rastreador de problemas de GitHub de EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para hacernos saber cómo usa `ILoggerFactory`, para que podamos comprender mejor cómo evitar esta interrupción en el futuro.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-582">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="c8ba1-583">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="c8ba1-583">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="c8ba1-584">Problema de seguimiento n.º 12780</span><span class="sxs-lookup"><span data-stu-id="c8ba1-584">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="c8ba1-585">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-585">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-586">Antes de EF Core 3.0, una vez que se eliminaba `DbContext` no había ninguna forma de saber si una determinada propiedad de navegación de una entidad obtenida de ese contexto se había cargado completamente o no.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-586">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="c8ba1-587">En su lugar, los proxies asumían que una navegación de referencia se cargaba si tenía un valor distinto de NULL, y que una navegación de colección se cargaba si no estaba vacía.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-587">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="c8ba1-588">En estos casos, el intento de carga diferida era no operativo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-588">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="c8ba1-589">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-589">**New behavior**</span></span>

<span data-ttu-id="c8ba1-590">A partir de EF Core 3.0, los proxies realizan el seguimiento de si una propiedad de navegación se carga o no.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-590">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="c8ba1-591">Esto significa que el intento de acceder a una propiedad de navegación que se carga después de que se haya eliminado el contexto siempre será no operativo, incluso cuando la navegación cargada está vacía o es NULL.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-591">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="c8ba1-592">Por el contrario, el intento de acceder a una propiedad de navegación que no está cargada iniciará una excepción si el contexto se ha eliminado, incluso si la propiedad de navegación es una colección no vacía.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-592">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="c8ba1-593">Si se produce esta situación, significa que el código de aplicación está intentando usar la carga diferida en un momento no válido y que se debe cambiar la aplicación para que lo no haga.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-593">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="c8ba1-594">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-594">**Why**</span></span>

<span data-ttu-id="c8ba1-595">Este cambio se ha realizado para que el comportamiento sea coherente y correcto cuando se intenta la carga diferida de una instancia de `DbContext` eliminada.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-595">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="c8ba1-596">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-596">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-597">Actualice el código de la aplicación para que no intente la carga diferida con un contexto eliminado, o bien configúrelo para que sea no operativo, como se describe en el mensaje de la excepción.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-597">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="c8ba1-598">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="c8ba1-598">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="c8ba1-599">Problema de seguimiento n.º 10236</span><span class="sxs-lookup"><span data-stu-id="c8ba1-599">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="c8ba1-600">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-600">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-601">Antes de EF Core 3.0, se registraba una advertencia para una aplicación que creaba un número patológico de proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-601">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="c8ba1-602">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-602">**New behavior**</span></span>

<span data-ttu-id="c8ba1-603">A partir de EF Core 3.0, ahora esta advertencia se considera un error y se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-603">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="c8ba1-604">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-604">**Why**</span></span>

<span data-ttu-id="c8ba1-605">Este cambio se ha realizado para controlar mejor el código de la aplicación mediante la exposición de este caso patológico de una forma más explícita.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-605">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="c8ba1-606">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-606">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-607">Cuando se produce este error, la acción más adecuada consiste en comprender la causa raíz y detener la creación de tantos proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-607">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="c8ba1-608">Pero el error se puede convertir en una advertencia (u omitirse) mediante configuración en `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-608">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="c8ba1-609">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-609">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="c8ba1-610">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="c8ba1-610">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="c8ba1-611">Problema de seguimiento n.° 9171</span><span class="sxs-lookup"><span data-stu-id="c8ba1-611">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="c8ba1-612">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-612">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-613">Antes de EF Core 3.0, el código para llamar a `HasOne` o `HasMany` con una cadena se interpretaba de manera confusa.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-613">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="c8ba1-614">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-614">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="c8ba1-615">El código parece relacionar `Samurai` con otro tipo de entidad mediante la propiedad de navegación `Entrance`, que puede ser privada.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-615">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="c8ba1-616">En realidad, este código intenta crear una relación con algún tipo de entidad denominada `Entrance` sin ninguna propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-616">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="c8ba1-617">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-617">**New behavior**</span></span>

<span data-ttu-id="c8ba1-618">A partir de EF Core 3.0, el código anterior ahora hace lo que parecía que debía hacer antes.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-618">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="c8ba1-619">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-619">**Why**</span></span>

<span data-ttu-id="c8ba1-620">El comportamiento anterior era muy confuso, especialmente al leer el código de configuración y al buscar errores.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-620">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="c8ba1-621">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-621">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-622">Esto solo interrumpirá las aplicaciones que configuran de manera explícita las relaciones con cadenas para nombres de tipos y sin especificar explícitamente la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-622">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="c8ba1-623">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-623">This is not common.</span></span>
<span data-ttu-id="c8ba1-624">El comportamiento anterior se puede obtener al pasar de manera explícita `null` para el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-624">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="c8ba1-625">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-625">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="c8ba1-626">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="c8ba1-626">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="c8ba1-627">Problema de seguimiento n.º 15184</span><span class="sxs-lookup"><span data-stu-id="c8ba1-627">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="c8ba1-628">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-628">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-629">Antes, los siguientes métodos asincrónicos devolvían `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="c8ba1-630">`ValueGenerator.NextValueAsync()` (y las clases derivadas)</span><span class="sxs-lookup"><span data-stu-id="c8ba1-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="c8ba1-631">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-631">**New behavior**</span></span>

<span data-ttu-id="c8ba1-632">Dichos métodos ahora devuelven `ValueTask<T>` durante el mismo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="c8ba1-633">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-633">**Why**</span></span>

<span data-ttu-id="c8ba1-634">Este cambio reduce el número de asignaciones de montones que se producen al invocar estos métodos, lo que mejora el rendimiento general.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="c8ba1-635">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-635">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-636">Las aplicaciones que simplemente esperen las API anteriores solo necesitan recompilarse, sin que sea necesario realizar cambios en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="c8ba1-637">Un uso más complejo (p. ej., pasar el valor `Task` devuelto a `Task.WhenAny()`) normalmente requiere que el valor `ValueTask<T>` devuelto se convierta en `Task<T>` mediante una llamada a `AsTask()` en él.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="c8ba1-638">Tenga en cuenta que esto niega la reducción de asignación que implica este cambio.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="c8ba1-639">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="c8ba1-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="c8ba1-640">Problema de seguimiento n.º 9913</span><span class="sxs-lookup"><span data-stu-id="c8ba1-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="c8ba1-641">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-641">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-642">El nombre de anotación para las anotaciones de asignación de tipos era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="c8ba1-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="c8ba1-643">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-643">**New behavior**</span></span>

<span data-ttu-id="c8ba1-644">Ahora, el nombre de anotación para las anotaciones de asignación de tipos es "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="c8ba1-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="c8ba1-645">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-645">**Why**</span></span>

<span data-ttu-id="c8ba1-646">Ahora, las asignaciones de tipos se usan para algo más que solo para proveedores de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="c8ba1-647">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-647">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-648">Esto solo interrumpirá a las aplicaciones que acceden directamente a la asignación de tipos como una anotación, lo que no es habitual.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="c8ba1-649">La acción más apropiada para corregir es usar la superficie de API para acceder a las asignaciones de tipos en lugar de usar directamente la anotación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="c8ba1-650">ToTable en un tipo derivado inicia una excepción</span><span class="sxs-lookup"><span data-stu-id="c8ba1-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="c8ba1-651">Problema de seguimiento n.º 11811</span><span class="sxs-lookup"><span data-stu-id="c8ba1-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="c8ba1-652">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-652">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-653">Antes de EF Core 3.0, la llamada a `ToTable()` en un tipo derivado se omitía, ya que la única estrategia asignación de herencia era TPH, lo que no es válido.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-653">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="c8ba1-654">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-654">**New behavior**</span></span>

<span data-ttu-id="c8ba1-655">A partir de EF Core 3.0, y en preparación para agregar compatibilidad con TPT y TPC en una versión posterior, ahora la llamada a `ToTable()` en un tipo derivado iniciará una excepción para evitar un cambio de asignación inesperado en el futuro.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-655">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="c8ba1-656">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-656">**Why**</span></span>

<span data-ttu-id="c8ba1-657">En la actualidad no se considera válido asignar un tipo derivado a otra tabla.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-657">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="c8ba1-658">Este cambio evita interrupciones en el futuro, cuando se convierta en una operación válida.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-658">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="c8ba1-659">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-659">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-660">Quite todos los intentos de asignar tipos derivados a otras tablas.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-660">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="c8ba1-661">ForSqlServerHasIndex se ha reemplazado por HasIndex</span><span class="sxs-lookup"><span data-stu-id="c8ba1-661">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="c8ba1-662">Problema de seguimiento n.º 12366</span><span class="sxs-lookup"><span data-stu-id="c8ba1-662">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="c8ba1-663">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-663">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-664">Antes de EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` proporcionaba una manera de configurar las columnas que se usaban con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-664">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="c8ba1-665">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-665">**New behavior**</span></span>

<span data-ttu-id="c8ba1-666">A partir de EF Core 3.0, ya se admite el uso de `Include` en un índice en el nivel relacional.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-666">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="c8ba1-667">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-667">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="c8ba1-668">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-668">**Why**</span></span>

<span data-ttu-id="c8ba1-669">Este cambio se ha realizado para consolidar la API para índices con `Include` en un mismo lugar para todos los proveedores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-669">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="c8ba1-670">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-670">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-671">Use la API nueva, como se ha mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-671">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="c8ba1-672">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="c8ba1-672">Metadata API changes</span></span>

[<span data-ttu-id="c8ba1-673">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="c8ba1-673">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="c8ba1-674">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-674">**New behavior**</span></span>

<span data-ttu-id="c8ba1-675">Las siguientes propiedades se han convertido en métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-675">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="c8ba1-676">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-676">**Why**</span></span>

<span data-ttu-id="c8ba1-677">Este cambio simplifica la implementación de las interfaces mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-677">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="c8ba1-678">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-678">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-679">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-679">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="c8ba1-680">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="c8ba1-680">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="c8ba1-681">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="c8ba1-681">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="c8ba1-682">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-682">**New behavior**</span></span>

<span data-ttu-id="c8ba1-683">Los métodos de extensión específicos del proveedor se simplificarán:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-683">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="c8ba1-684">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-684">**Why**</span></span>

<span data-ttu-id="c8ba1-685">Este cambio simplifica la implementación de los métodos de extensión mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-685">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="c8ba1-686">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-686">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-687">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-687">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="c8ba1-688">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="c8ba1-688">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="c8ba1-689">Problema de seguimiento n.º 12151</span><span class="sxs-lookup"><span data-stu-id="c8ba1-689">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="c8ba1-690">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-690">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-691">Antes de EF Core 3.0, EF Core enviaba `PRAGMA foreign_keys = 1` cuando se abría una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-691">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="c8ba1-692">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-692">**New behavior**</span></span>

<span data-ttu-id="c8ba1-693">A partir de EF Core 3.0, EF Core ya no envía `PRAGMA foreign_keys = 1` cuando se abre una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-693">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="c8ba1-694">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-694">**Why**</span></span>

<span data-ttu-id="c8ba1-695">Este cambio se ha realizado porque en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3` de forma predeterminada, lo que a su vez significa que el cumplimiento de CD está activado de forma predeterminada y no es necesario habilitarlo explícitamente cada vez que se abra una conexión.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-695">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="c8ba1-696">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-696">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-697">Las claves externas se habilitan de forma predeterminada en SQLitePCLRaw.bundle_e_sqlite3, que en EF Core se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-697">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="c8ba1-698">Para otros casos, las claves externas se pueden habilitar mediante la especificación de `Foreign Keys=True` en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-698">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="c8ba1-699">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="c8ba1-699">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="c8ba1-700">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-700">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-701">Antes de EF Core 3.0, en EF Core se usaba `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-701">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="c8ba1-702">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-702">**New behavior**</span></span>

<span data-ttu-id="c8ba1-703">A partir de EF Core 3.0, en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-703">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="c8ba1-704">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-704">**Why**</span></span>

<span data-ttu-id="c8ba1-705">Este cambio se ha realizado para que la versión de SQLite que se usa en iOS sea coherente con otras plataformas.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-705">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="c8ba1-706">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-706">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-707">Para usar la versión nativa de SQLite en iOS, configure `Microsoft.Data.Sqlite` para usar otra agrupación `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-707">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="c8ba1-708">Almacenamiento de valores GUID como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="c8ba1-708">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="c8ba1-709">Problema de seguimiento n.º 15078</span><span class="sxs-lookup"><span data-stu-id="c8ba1-709">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="c8ba1-710">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-710">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-711">Antes, los valores GUID se almacenaban como valores BLOB en SQLite.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-711">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="c8ba1-712">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-712">**New behavior**</span></span>

<span data-ttu-id="c8ba1-713">Ahora, los valores GUID se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-713">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="c8ba1-714">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-714">**Why**</span></span>

<span data-ttu-id="c8ba1-715">El formato binario de los GUID no está normalizado.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-715">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="c8ba1-716">El almacenamiento de los valores como TEXT mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-716">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="c8ba1-717">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-717">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-718">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-718">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="c8ba1-719">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-719">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="c8ba1-720">Microsoft.Data.Sqlite sigue siendo capaz de leer valores GUID de ambas columnas BLOB y TEXT. Sin embargo, dado que el formato predeterminado de los parámetros y las constantes ha cambiado, seguramente deberá realizar alguna acción en la mayoría de casos que impliquen el uso de valores GUID.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-720">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="c8ba1-721">Ahora los valores char se almacenan como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="c8ba1-721">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="c8ba1-722">Problema de seguimiento n.º 15020</span><span class="sxs-lookup"><span data-stu-id="c8ba1-722">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="c8ba1-723">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-723">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-724">Anteriormente los valores char se almacenaban como valores INTEGER en SQLite.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-724">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="c8ba1-725">Por ejemplo, un valor char de *A* se almacenaba como el valor entero 65.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-725">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="c8ba1-726">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-726">**New behavior**</span></span>

<span data-ttu-id="c8ba1-727">Ahora, los valores char se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-727">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="c8ba1-728">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-728">**Why**</span></span>

<span data-ttu-id="c8ba1-729">El almacenamiento de valores como TEXT es más natural y mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-729">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="c8ba1-730">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-730">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-731">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-731">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="c8ba1-732">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-732">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="c8ba1-733">Microsoft.Data.Sqlite también puede leer valores de caracteres tanto de columnas INTEGER como de columnas TEXT, por lo que es posible que no deba hacer nada dependiendo de su caso.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-733">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="c8ba1-734">Ahora los id. de migración se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="c8ba1-734">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="c8ba1-735">Problema de seguimiento n.º 12978</span><span class="sxs-lookup"><span data-stu-id="c8ba1-735">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="c8ba1-736">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-736">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-737">Los identificadores de migración se generaban de forma involuntaria con el calendario de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-737">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="c8ba1-738">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-738">**New behavior**</span></span>

<span data-ttu-id="c8ba1-739">Ahora los id. de migración siempre se generan usando el calendario de la referencia cultural invariable (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="c8ba1-739">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="c8ba1-740">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-740">**Why**</span></span>

<span data-ttu-id="c8ba1-741">El orden de las migraciones es importante al actualizar la base de datos o al solucionar conflictos de combinación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-741">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="c8ba1-742">Al usar el calendario invariable, se evitan problemas de ordenación que pueden producirse si los miembros del equipo tienen distintos calendarios del sistema.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-742">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="c8ba1-743">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-743">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-744">Esta cambio afecta a todas las personas que usan un calendario no gregoriano en el que el año sea superior al del calendario gregoriano (como el calendario budista tailandés).</span><span class="sxs-lookup"><span data-stu-id="c8ba1-744">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="c8ba1-745">Los id. de migración existentes deberán actualizarse para que las migraciones nuevas se ordenen después de las existentes.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-745">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="c8ba1-746">Puede ver el id. de migración en el atributo Migration de los archivos de diseñador de la migración.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-746">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="c8ba1-747">También debe actualizarse la tabla de historial de migraciones.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-747">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="c8ba1-748">Se ha quitado el elemento UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="c8ba1-748">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="c8ba1-749">Problema de seguimiento n.º 16400</span><span class="sxs-lookup"><span data-stu-id="c8ba1-749">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="c8ba1-750">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-750">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-751">Antes de EF Core 3.0, `UseRowNumberForPaging` se podía usar para generar SQL para la paginación de forma que fuera compatible con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-751">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="c8ba1-752">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-752">**New behavior**</span></span>

<span data-ttu-id="c8ba1-753">A partir de EF Core 3.0, EF solo genera SQL para la paginación que únicamente es compatible con las versiones posteriores de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-753">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="c8ba1-754">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-754">**Why**</span></span>

<span data-ttu-id="c8ba1-755">El motivo de este cambio es que [SQL Server 2008 ya no se admite](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/). Además, la actualización de esta característica para que funcionase con los cambios en las consultas implementados en EF Core 3.0 llevaría mucho trabajo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-755">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="c8ba1-756">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-756">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-757">Se recomienda actualizar a una versión más reciente de SQL Server, o bien utilizar un nivel de compatibilidad superior, de modo que el SQL que se genere se admita.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-757">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="c8ba1-758">Dicho esto, si no puede hacerlo, [escriba un comentario en el problema de seguimiento](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con los detalles al respecto.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-758">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="c8ba1-759">En función de los comentarios, es posible que volvamos a valorar esta decisión.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-759">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="c8ba1-760">La información o metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="c8ba1-760">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="c8ba1-761">Problema de seguimiento n.º 16119</span><span class="sxs-lookup"><span data-stu-id="c8ba1-761">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="c8ba1-762">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-762">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-763">`IDbContextOptionsExtension` incluía métodos para proporcionar metadatos sobre la extensión.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-763">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="c8ba1-764">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-764">**New behavior**</span></span>

<span data-ttu-id="c8ba1-765">Estos métodos se han movido a una nueva clase base abstracta `DbContextOptionsExtensionInfo`, que se devuelve desde una nueva propiedad `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-765">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="c8ba1-766">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-766">**Why**</span></span>

<span data-ttu-id="c8ba1-767">Al lanzarse las versiones 2.0 y 3.0, tuvimos que agregar o cambiar estos métodos varias veces.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-767">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="c8ba1-768">Su división en una nueva clase base abstracta facilitará la realización de este tipo de cambios sin interrumpir las extensiones existentes.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-768">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="c8ba1-769">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-769">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-770">Actualice las extensiones para seguir el nuevo patrón.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-770">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="c8ba1-771">Encontrará ejemplos en las muchas implementaciones de `IDbContextOptionsExtension` para los diferentes tipos de extensiones en el código fuente de EF Core.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-771">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="c8ba1-772">Cambio de nombre de LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="c8ba1-772">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="c8ba1-773">Problema de seguimiento n.º 10985</span><span class="sxs-lookup"><span data-stu-id="c8ba1-773">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="c8ba1-774">**Cambio**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-774">**Change**</span></span>

<span data-ttu-id="c8ba1-775">Se ha cambiado el nombre de `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="c8ba1-776">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-776">**Why**</span></span>

<span data-ttu-id="c8ba1-777">Conviene alinear el nombre de este evento de advertencia con el del resto de eventos de advertencia.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-777">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="c8ba1-778">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-778">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-779">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-779">Use the new name.</span></span> <span data-ttu-id="c8ba1-780">(Tenga en cuenta que el número de id. evento sigue siendo el mismo).</span><span class="sxs-lookup"><span data-stu-id="c8ba1-780">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="c8ba1-781">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="c8ba1-781">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="c8ba1-782">Problema de seguimiento n.º 10730</span><span class="sxs-lookup"><span data-stu-id="c8ba1-782">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="c8ba1-783">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-783">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-784">Antes de EF Core 3.0, se utilizaba simplemente el término "nombre" para hacer referencia a los nombres de las restricciones de claves externas.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-784">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="c8ba1-785">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-785">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="c8ba1-786">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-786">**New behavior**</span></span>

<span data-ttu-id="c8ba1-787">A partir de EF Core 3.0, el término con el que se hace referencia a los nombres de las restricciones de claves externas es "nombre de la restricción".</span><span class="sxs-lookup"><span data-stu-id="c8ba1-787">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="c8ba1-788">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-788">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="c8ba1-789">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-789">**Why**</span></span>

<span data-ttu-id="c8ba1-790">Este cambio permite mejorar la coherencia relativa a la nomenclatura en este aspecto y aclarar que se trata del nombre de una restricción de clave externa, y no del de la columna o propiedad en la que está definida la clave externa.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-790">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="c8ba1-791">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-791">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-792">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-792">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="c8ba1-793">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="c8ba1-793">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="c8ba1-794">Problema de seguimiento n.° 15997</span><span class="sxs-lookup"><span data-stu-id="c8ba1-794">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="c8ba1-795">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-795">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-796">Antes de EF Core 3.0, estos métodos estaban protegidos.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-796">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="c8ba1-797">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-797">**New behavior**</span></span>

<span data-ttu-id="c8ba1-798">Desde EF Core 3.0, estos métodos son públicos.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-798">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="c8ba1-799">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-799">**Why**</span></span>

<span data-ttu-id="c8ba1-800">EF usa estos métodos para determinar si se ha creado una base de datos, pero está vacía.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-800">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="c8ba1-801">Esto también puede resultar útil fuera de EF al determinar si se deben aplicar migraciones o no.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-801">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="c8ba1-802">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-802">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-803">Cambie la accesibilidad de cualquier invalidación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-803">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="c8ba1-804">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="c8ba1-804">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="c8ba1-805">Problema de seguimiento n.° 11506</span><span class="sxs-lookup"><span data-stu-id="c8ba1-805">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="c8ba1-806">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-806">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-807">Antes de EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un paquete NuGet regular con un ensamblado al que podían hacer referencia los proyectos que dependían de él.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-807">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="c8ba1-808">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-808">**New behavior**</span></span>

<span data-ttu-id="c8ba1-809">Desde EF Core 3.0, es un paquete DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-809">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="c8ba1-810">Esto significa que la dependencia no fluirá de manera transitiva en otros proyectos y que ya no puede, de forma predeterminada, hacer referencia a su ensamblado.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-810">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="c8ba1-811">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-811">**Why**</span></span>

<span data-ttu-id="c8ba1-812">Este paquete solo está destinado a usarse en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-812">This package is only intended to be used at design time.</span></span> <span data-ttu-id="c8ba1-813">Las aplicaciones implementadas no deben hacer referencia al mismo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-813">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="c8ba1-814">Hacer que el paquete sea DevelopmentDependency refuerza esta recomendación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-814">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="c8ba1-815">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-815">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-816">Si tiene que hacer referencia a este paquete para invalidar el comportamiento en tiempo de diseño de EF Core, puede actualizar los metadatos de elementos PackageReference del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-816">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="c8ba1-817">Si se hace referencia al paquete de manera transitiva a través de Microsoft.EntityFrameworkCore.Tools, tendrá que agregar una PackageReference explícita al paquete para cambiar sus metadatos.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-817">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="c8ba1-818">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ba1-818">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="c8ba1-819">Problema de seguimiento n.° 14824</span><span class="sxs-lookup"><span data-stu-id="c8ba1-819">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="c8ba1-820">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-820">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-821">Microsoft.EntityFrameworkCore.Sqlite dependía anteriormente de la versión 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-821">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="c8ba1-822">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-822">**New behavior**</span></span>

<span data-ttu-id="c8ba1-823">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-823">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="c8ba1-824">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-824">**Why**</span></span>

<span data-ttu-id="c8ba1-825">La versión 2.0.0 de SQLitePCL.raw selecciona .NET Standard 2.0 como destino.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-825">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="c8ba1-826">Anteriormente seleccionaba .NET Standard 1.1 como destino, que requería el cierre a gran escala de paquetes transitivos para su funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-826">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="c8ba1-827">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-827">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-828">En la versión 2.0.0 de SQLitePCL.raw se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-828">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="c8ba1-829">Consulte las [notas de la versión](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-829">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="c8ba1-830">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="c8ba1-830">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="c8ba1-831">Problema de seguimiento n.° 14825</span><span class="sxs-lookup"><span data-stu-id="c8ba1-831">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="c8ba1-832">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-832">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-833">Los paquetes espaciales anteriormente dependían de la versión 1.15.1 de NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-833">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="c8ba1-834">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-834">**New behavior**</span></span>

<span data-ttu-id="c8ba1-835">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-835">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="c8ba1-836">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-836">**Why**</span></span>

<span data-ttu-id="c8ba1-837">La versión 2.0.0 de NetTopologySuite pretende resolver varios problemas de usabilidad que encontraron los usuarios de EF Core.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-837">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="c8ba1-838">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-838">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-839">En la versión 2.0.0 de NetTopologySuite se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-839">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="c8ba1-840">Consulte las [notas de la versión](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-840">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="c8ba1-841">Se usa Microsoft.Data.SqlClient en lugar de System.Data.SqlClient</span><span class="sxs-lookup"><span data-stu-id="c8ba1-841">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="c8ba1-842">Problema de seguimiento n.º 15636</span><span class="sxs-lookup"><span data-stu-id="c8ba1-842">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="c8ba1-843">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-843">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-844">Microsoft.EntityFrameworkCore.SqlServer dependía anteriormente de la versión System.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-844">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="c8ba1-845">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-845">**New behavior**</span></span>

<span data-ttu-id="c8ba1-846">Hemos actualizado nuestro paquete para que dependa de Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-846">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="c8ba1-847">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-847">**Why**</span></span>

<span data-ttu-id="c8ba1-848">A partir de ahora, Microsoft.Data.SqlClient es el controlador de acceso a datos insignia para SQL Server y System.Data.SqlClient ya no es el centro de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-848">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="c8ba1-849">Algunas características importantes, como Always Encrypted, solo están disponibles en Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-849">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="c8ba1-850">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-850">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-851">Si el código toma una dependencia directa en System.Data.SqlClient, debe cambiarla para que haga referencia a Microsoft.Data.SqlClient en su lugar. Dado que los dos paquetes mantienen un grado muy alto de compatibilidad con la API, solo debería ser un paquete simple y un cambio de espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-851">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="c8ba1-852">Se deben configurar varias relaciones de referencia automática ambiguas</span><span class="sxs-lookup"><span data-stu-id="c8ba1-852">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="c8ba1-853">Problema de seguimiento n.º 13573</span><span class="sxs-lookup"><span data-stu-id="c8ba1-853">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="c8ba1-854">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-854">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-855">Un tipo de entidad con varias propiedades de navegación unidireccional de referencia automática y claves externas coincidentes se configuró incorrectamente como una única relación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-855">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="c8ba1-856">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-856">For example:</span></span>

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

<span data-ttu-id="c8ba1-857">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-857">**New behavior**</span></span>

<span data-ttu-id="c8ba1-858">Este escenario se detecta ahora en la generación del modelo y se produce una excepción que indica que el modelo es ambiguo.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-858">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="c8ba1-859">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-859">**Why**</span></span>

<span data-ttu-id="c8ba1-860">El modelo resultante era ambiguo, y lo más probable es que sea incorrecto en este caso.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-860">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="c8ba1-861">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-861">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-862">Utilice la configuración completa de la relación.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-862">Use full configuration of the relationship.</span></span> <span data-ttu-id="c8ba1-863">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c8ba1-863">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="c8ba1-864">DbFunction.Schema es NULL o la cadena vacía lo configura para estar en el esquema predeterminado del modelo</span><span class="sxs-lookup"><span data-stu-id="c8ba1-864">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="c8ba1-865">Problema de seguimiento n.º 12757</span><span class="sxs-lookup"><span data-stu-id="c8ba1-865">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="c8ba1-866">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-866">**Old behavior**</span></span>

<span data-ttu-id="c8ba1-867">Una función DbFunction configurada con el esquema como una cadena vacía se trataba como una función integrada sin un esquema.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-867">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="c8ba1-868">Por ejemplo, el código siguiente asignará la función CLR `DatePart` a la función integrada `DATEPART` en SqlServer.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-868">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="c8ba1-869">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-869">**New behavior**</span></span>

<span data-ttu-id="c8ba1-870">Todas las asignaciones de DbFunction se consideran asignadas a funciones definidas por el usuario.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-870">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="c8ba1-871">Por lo tanto, el valor de cadena vacía colocaría la función dentro del esquema predeterminado del modelo,</span><span class="sxs-lookup"><span data-stu-id="c8ba1-871">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="c8ba1-872">que podría ser el esquema configurado de forma explícita mediante `modelBuilder.HasDefaultSchema()` de la API fluida o `dbo` en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-872">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="c8ba1-873">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-873">**Why**</span></span>

<span data-ttu-id="c8ba1-874">Anteriormente, el esquema vacío era una manera de indicar que la función estaba integrada, pero esa lógica solo es aplicable a SqlServer, donde las funciones integradas no pertenecen a ningún esquema.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-874">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="c8ba1-875">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="c8ba1-875">**Mitigations**</span></span>

<span data-ttu-id="c8ba1-876">Configure la traslación de DbFunction manualmente para asignarla a una función integrada.</span><span class="sxs-lookup"><span data-stu-id="c8ba1-876">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
