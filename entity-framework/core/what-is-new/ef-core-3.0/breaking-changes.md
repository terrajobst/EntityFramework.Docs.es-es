---
title: 'Cambios importantes en EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 690c7828cfe5019f4e7ae904c92430fab4726cb9
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72446018"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="2fa64-102">Cambios importantes incluidos en EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="2fa64-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="2fa64-103">Es posible que los siguientes cambios de API y comportamiento interrumpan las aplicaciones actuales cuando se actualicen a la versión 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="2fa64-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="2fa64-104">Los cambios que esperamos que solo afecten a proveedores de base de datos se documentan en [Cambios para proveedores](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="2fa64-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="2fa64-105">Resumen</span><span class="sxs-lookup"><span data-stu-id="2fa64-105">Summary</span></span>

| <span data-ttu-id="2fa64-106">**Cambio importante**</span><span class="sxs-lookup"><span data-stu-id="2fa64-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="2fa64-107">**Impacto**</span><span class="sxs-lookup"><span data-stu-id="2fa64-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="2fa64-108">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="2fa64-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="2fa64-109">Alto</span><span class="sxs-lookup"><span data-stu-id="2fa64-109">High</span></span>       |
| [<span data-ttu-id="2fa64-110">EF Core 3.0 tiene como destino .NET Standard 2.1, y no .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="2fa64-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="2fa64-111">Alto</span><span class="sxs-lookup"><span data-stu-id="2fa64-111">High</span></span>      |
| [<span data-ttu-id="2fa64-112">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="2fa64-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="2fa64-113">Alto</span><span class="sxs-lookup"><span data-stu-id="2fa64-113">High</span></span>      |
| [<span data-ttu-id="2fa64-114">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="2fa64-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="2fa64-115">Alto</span><span class="sxs-lookup"><span data-stu-id="2fa64-115">High</span></span>      |
| [<span data-ttu-id="2fa64-116">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="2fa64-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="2fa64-117">Alto</span><span class="sxs-lookup"><span data-stu-id="2fa64-117">High</span></span>      |
| [<span data-ttu-id="2fa64-118">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="2fa64-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="2fa64-119">Alto</span><span class="sxs-lookup"><span data-stu-id="2fa64-119">High</span></span>      |
| [<span data-ttu-id="2fa64-120">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2fa64-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="2fa64-121">Medium</span><span class="sxs-lookup"><span data-stu-id="2fa64-121">Medium</span></span>      |
| [<span data-ttu-id="2fa64-122">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="2fa64-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="2fa64-123">Medium</span><span class="sxs-lookup"><span data-stu-id="2fa64-123">Medium</span></span>      |
| [<span data-ttu-id="2fa64-124">La carga diligente de entidades relacionadas ahora se realiza en una sola consulta</span><span class="sxs-lookup"><span data-stu-id="2fa64-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="2fa64-125">Medium</span><span class="sxs-lookup"><span data-stu-id="2fa64-125">Medium</span></span>      |
| [<span data-ttu-id="2fa64-126">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="2fa64-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="2fa64-127">Medium</span><span class="sxs-lookup"><span data-stu-id="2fa64-127">Medium</span></span>      |
| [<span data-ttu-id="2fa64-128">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="2fa64-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="2fa64-129">Medium</span><span class="sxs-lookup"><span data-stu-id="2fa64-129">Medium</span></span>      |
| [<span data-ttu-id="2fa64-130">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="2fa64-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="2fa64-131">Medium</span><span class="sxs-lookup"><span data-stu-id="2fa64-131">Medium</span></span>      |
| [<span data-ttu-id="2fa64-132">Las consultas sin seguimiento ya no realizan la resolución de la identidad</span><span class="sxs-lookup"><span data-stu-id="2fa64-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="2fa64-133">Medium</span><span class="sxs-lookup"><span data-stu-id="2fa64-133">Medium</span></span>      |
| [<span data-ttu-id="2fa64-134">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="2fa64-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="2fa64-135">Medium</span><span class="sxs-lookup"><span data-stu-id="2fa64-135">Medium</span></span>      |
| [<span data-ttu-id="2fa64-136">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="2fa64-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="2fa64-137">Medium</span><span class="sxs-lookup"><span data-stu-id="2fa64-137">Medium</span></span>      |
| [<span data-ttu-id="2fa64-138">Se ha quitado el elemento UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="2fa64-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="2fa64-139">Medium</span><span class="sxs-lookup"><span data-stu-id="2fa64-139">Medium</span></span>      |
| [<span data-ttu-id="2fa64-140">Cuando el método FromSql se usa con un procedimiento almacenado no se puede redactar</span><span class="sxs-lookup"><span data-stu-id="2fa64-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="2fa64-141">Medium</span><span class="sxs-lookup"><span data-stu-id="2fa64-141">Medium</span></span>      |
| [<span data-ttu-id="2fa64-142">Solo se pueden especificar métodos de FromSql en raíces de consulta</span><span class="sxs-lookup"><span data-stu-id="2fa64-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="2fa64-143">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-143">Low</span></span>      |
| [<span data-ttu-id="2fa64-144">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="2fa64-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="2fa64-145">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-145">Low</span></span>      |
| [<span data-ttu-id="2fa64-146">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="2fa64-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="2fa64-147">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-147">Low</span></span>      |
| [<span data-ttu-id="2fa64-148">Las entidades dependientes que comparten la tabla con la entidad de seguridad son ahora opcionales</span><span class="sxs-lookup"><span data-stu-id="2fa64-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="2fa64-149">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-149">Low</span></span>      |
| [<span data-ttu-id="2fa64-150">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="2fa64-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="2fa64-151">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-151">Low</span></span>      |
| [<span data-ttu-id="2fa64-152">Las propiedades heredadas de tipos sin asignar se asignan ahora a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="2fa64-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="2fa64-153">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-153">Low</span></span>      |
| [<span data-ttu-id="2fa64-154">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="2fa64-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="2fa64-155">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-155">Low</span></span>      |
| [<span data-ttu-id="2fa64-156">La conexión de base de datos ahora se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="2fa64-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="2fa64-157">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-157">Low</span></span>      |
| [<span data-ttu-id="2fa64-158">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="2fa64-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="2fa64-159">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-159">Low</span></span>      |
| [<span data-ttu-id="2fa64-160">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="2fa64-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="2fa64-161">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-161">Low</span></span>      |
| [<span data-ttu-id="2fa64-162">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="2fa64-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="2fa64-163">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-163">Low</span></span>      |
| [<span data-ttu-id="2fa64-164">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="2fa64-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="2fa64-165">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-165">Low</span></span>      |
| [<span data-ttu-id="2fa64-166">DbContext.Entry realiza ahora una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="2fa64-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="2fa64-167">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-167">Low</span></span>      |
| [<span data-ttu-id="2fa64-168">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="2fa64-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="2fa64-169">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-169">Low</span></span>      |
| [<span data-ttu-id="2fa64-170">ILoggerFactory es ahora un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="2fa64-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="2fa64-171">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-171">Low</span></span>      |
| [<span data-ttu-id="2fa64-172">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="2fa64-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="2fa64-173">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-173">Low</span></span>      |
| [<span data-ttu-id="2fa64-174">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="2fa64-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="2fa64-175">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-175">Low</span></span>      |
| [<span data-ttu-id="2fa64-176">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="2fa64-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="2fa64-177">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-177">Low</span></span>      |
| [<span data-ttu-id="2fa64-178">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="2fa64-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="2fa64-179">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-179">Low</span></span>      |
| [<span data-ttu-id="2fa64-180">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2fa64-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="2fa64-181">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-181">Low</span></span>      |
| [<span data-ttu-id="2fa64-182">ToTable en un tipo derivado produce una excepción</span><span class="sxs-lookup"><span data-stu-id="2fa64-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="2fa64-183">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-183">Low</span></span>      |
| [<span data-ttu-id="2fa64-184">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="2fa64-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="2fa64-185">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-185">Low</span></span>      |
| [<span data-ttu-id="2fa64-186">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="2fa64-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="2fa64-187">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-187">Low</span></span>      |
| [<span data-ttu-id="2fa64-188">Los valores GUID se almacenan ahora como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="2fa64-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="2fa64-189">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-189">Low</span></span>      |
| [<span data-ttu-id="2fa64-190">Los valores char se almacenan ahora como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="2fa64-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="2fa64-191">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-191">Low</span></span>      |
| [<span data-ttu-id="2fa64-192">Los id. de migración ahora se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="2fa64-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="2fa64-193">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-193">Low</span></span>      |
| [<span data-ttu-id="2fa64-194">La información o los metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="2fa64-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="2fa64-195">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-195">Low</span></span>      |
| [<span data-ttu-id="2fa64-196">LogQueryPossibleExceptionWithAggregateOperator ha cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="2fa64-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="2fa64-197">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-197">Low</span></span>      |
| [<span data-ttu-id="2fa64-198">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="2fa64-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="2fa64-199">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-199">Low</span></span>      |
| [<span data-ttu-id="2fa64-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="2fa64-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="2fa64-201">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-201">Low</span></span>      |
| [<span data-ttu-id="2fa64-202">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="2fa64-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="2fa64-203">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-203">Low</span></span>      |
| [<span data-ttu-id="2fa64-204">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2fa64-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="2fa64-205">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-205">Low</span></span>      |
| [<span data-ttu-id="2fa64-206">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2fa64-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="2fa64-207">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-207">Low</span></span>      |
| [<span data-ttu-id="2fa64-208">Se deben configurar varias relaciones de referencia automática ambiguas</span><span class="sxs-lookup"><span data-stu-id="2fa64-208">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="2fa64-209">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-209">Low</span></span>      |
| [<span data-ttu-id="2fa64-210">DbFunction.Schema es NULL o la cadena vacía lo configura para estar en el esquema predeterminado del modelo</span><span class="sxs-lookup"><span data-stu-id="2fa64-210">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="2fa64-211">Bajo</span><span class="sxs-lookup"><span data-stu-id="2fa64-211">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="2fa64-212">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="2fa64-212">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="2fa64-213">[Problema de seguimiento n.° 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Consulte también el problema n.° 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="2fa64-213">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="2fa64-214">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-214">**Old behavior**</span></span>

<span data-ttu-id="2fa64-215">Antes de 3.0, cuando en EF Core no se podía convertir una expresión que formaba parte de una consulta SQL o un parámetro, la expresión se evaluaba de forma automática en el cliente.</span><span class="sxs-lookup"><span data-stu-id="2fa64-215">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="2fa64-216">De forma predeterminada, la evaluación de cliente de expresiones potencialmente costosas solo desencadenaba una advertencia.</span><span class="sxs-lookup"><span data-stu-id="2fa64-216">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="2fa64-217">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-217">**New behavior**</span></span>

<span data-ttu-id="2fa64-218">A partir de 3.0, en EF Core solo se permite que se evalúen en el cliente las expresiones en la proyección de nivel superior (la última llamada a `Select()` de la consulta).</span><span class="sxs-lookup"><span data-stu-id="2fa64-218">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="2fa64-219">Cuando las expresiones de otra parte de la consulta no se pueden convertir en SQL o un parámetro, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="2fa64-219">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="2fa64-220">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-220">**Why**</span></span>

<span data-ttu-id="2fa64-221">La evaluación de cliente automática de las consultas permite que se ejecuten muchas consultas incluso si no se pueden convertir elementos importantes de ellas.</span><span class="sxs-lookup"><span data-stu-id="2fa64-221">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="2fa64-222">Esto puede provocar un comportamiento inesperado y potencialmente dañino que es posible que solo sea evidente en entornos de producción.</span><span class="sxs-lookup"><span data-stu-id="2fa64-222">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="2fa64-223">Por ejemplo, una condición en una llamada a `Where()` que no se puede convertir puede provocar que todas las filas de la tabla se transfieran desde el servidor de base de datos y que el filtro se aplique en el cliente.</span><span class="sxs-lookup"><span data-stu-id="2fa64-223">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="2fa64-224">Esta situación puede pasar desapercibida fácilmente si la tabla solo contiene algunas filas en la fase de desarrollo, pero ser más grave cuando la aplicación pase a producción, donde la tabla puede contener millones de filas.</span><span class="sxs-lookup"><span data-stu-id="2fa64-224">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="2fa64-225">Las advertencias de evaluación de cliente también se suelen pasar por alto durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="2fa64-225">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="2fa64-226">Además de esto, la evaluación de cliente automática puede causar problemas en los que la mejora de la traducción de consultas para expresiones específicas provocaba cambios importantes no deseados entre versiones.</span><span class="sxs-lookup"><span data-stu-id="2fa64-226">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="2fa64-227">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-227">**Mitigations**</span></span>

<span data-ttu-id="2fa64-228">Si una consulta no se puede traducir totalmente, vuelva a escribirla en un formato que se pueda traducir, o bien use `AsEnumerable()`, `ToList()` o una función similar para devolver los datos al cliente de forma explícita, donde después se puedan seguir procesando mediante LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="2fa64-228">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="2fa64-229">EF Core 3.0 tiene como destino .NET Standard 2.1, y no .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="2fa64-229">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="2fa64-230">Problema de seguimiento n.º 15498</span><span class="sxs-lookup"><span data-stu-id="2fa64-230">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="2fa64-231">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-231">**Old behavior**</span></span>

<span data-ttu-id="2fa64-232">Antes de la versión 3.0, EF Core tenía como destino .NET Standard 2.0 y se podía ejecutar en todas las plataformas que admitieran dicho estándar, incluido .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2fa64-232">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="2fa64-233">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-233">**New behavior**</span></span>

<span data-ttu-id="2fa64-234">A partir de la versión 3.0, EF Core tiene como destino .NET Standard 2.1 y se puede ejecutar en todas las plataformas que admitan dicho estándar.</span><span class="sxs-lookup"><span data-stu-id="2fa64-234">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="2fa64-235">Esto no incluye .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2fa64-235">This does not include .NET Framework.</span></span>

<span data-ttu-id="2fa64-236">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-236">**Why**</span></span>

<span data-ttu-id="2fa64-237">Esto forma parte de una decisión estratégica para todas las tecnologías de .NET que tiene como objetivo centrar los esfuerzos en .NET Core y otras plataformas modernas de .NET, como Xamarin.</span><span class="sxs-lookup"><span data-stu-id="2fa64-237">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="2fa64-238">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-238">**Mitigations**</span></span>

<span data-ttu-id="2fa64-239">Valore la posibilidad de cambiar a una plataforma moderna de .NET.</span><span class="sxs-lookup"><span data-stu-id="2fa64-239">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="2fa64-240">Si esto no es posible, siga usando EF Core 2.1 o EF Core 2.2, puesto que ambas versiones admiten .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2fa64-240">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="2fa64-241">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2fa64-241">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="2fa64-242">Anuncios del problema de seguimiento n.º 325</span><span class="sxs-lookup"><span data-stu-id="2fa64-242">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="2fa64-243">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-243">**Old behavior**</span></span>

<span data-ttu-id="2fa64-244">Antes de ASP.NET Core 3.0, cuando se agregaba una referencia de paquete a `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, se incluía EF Core y algunos de los proveedores de datos de EF Core, como el de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2fa64-244">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="2fa64-245">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-245">**New behavior**</span></span>

<span data-ttu-id="2fa64-246">A partir de la versión 3.0, el marco compartido ASP.NET Core no incluye EF Core ni ningún proveedor de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="2fa64-246">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="2fa64-247">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-247">**Why**</span></span>

<span data-ttu-id="2fa64-248">Antes de este cambio, para obtener EF Core se necesitaban varios pasos en función de si la aplicación se destinaba a ASP.NET Core y SQL Server o no.</span><span class="sxs-lookup"><span data-stu-id="2fa64-248">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="2fa64-249">Además, la actualización de ASP.NET Core forzaba la de EF Core y el proveedor de SQL Server, lo que no siempre es deseable.</span><span class="sxs-lookup"><span data-stu-id="2fa64-249">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="2fa64-250">Con este cambio, la experiencia de obtención de EF Core es la misma en todos los proveedores, implementaciones admitidas de .NET y tipos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-250">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="2fa64-251">Ahora los desarrolladores también pueden controlar exactamente cuándo se actualizan EF Core y los proveedores de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="2fa64-251">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="2fa64-252">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-252">**Mitigations**</span></span>

<span data-ttu-id="2fa64-253">Para usar EF Core en una aplicación ASP.NET Core 3.0 o cualquier otra aplicación compatible, debe agregar de forma explícita una referencia de paquete al proveedor de base de datos de EF Core que se va a usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-253">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="2fa64-254">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="2fa64-254">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="2fa64-255">Problema de seguimiento n.º 14016</span><span class="sxs-lookup"><span data-stu-id="2fa64-255">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="2fa64-256">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-256">**Old behavior**</span></span>

<span data-ttu-id="2fa64-257">Antes de 3.0, la herramienta `dotnet ef` se incluía en el SDK de .NET Core y estaba disponible para usarse desde la línea de comandos de cualquier proyecto sin necesidad de realizar pasos adicionales.</span><span class="sxs-lookup"><span data-stu-id="2fa64-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="2fa64-258">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-258">**New behavior**</span></span>

<span data-ttu-id="2fa64-259">A partir de la versión 3.0, el SDK de .NET no incluye la herramienta `dotnet ef`, por lo que antes de poder usarla tendrá que instalarla de forma explícita como una herramienta local o global.</span><span class="sxs-lookup"><span data-stu-id="2fa64-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="2fa64-260">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-260">**Why**</span></span>

<span data-ttu-id="2fa64-261">Este cambio nos permite distribuir y actualizar `dotnet ef` como una herramienta convencional de la CLI de .NET en NuGet, coherente con el hecho de que la versión 3.0 de EF Core también se distribuye siempre como un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="2fa64-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="2fa64-262">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-262">**Mitigations**</span></span>

<span data-ttu-id="2fa64-263">Para poder administrar las migraciones o aplicar scaffolding a `DbContext`, instale `dotnet-ef` como herramienta global:</span><span class="sxs-lookup"><span data-stu-id="2fa64-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="2fa64-264">También se puede obtener una herramienta local cuando se restauran las dependencias de un proyecto que la declara como una dependencia de herramientas mediante un [archivo de manifiesto de herramientas](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="2fa64-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="2fa64-265">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="2fa64-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="2fa64-266">Problema de seguimiento n.º 10996</span><span class="sxs-lookup"><span data-stu-id="2fa64-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="2fa64-267">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-267">**Old behavior**</span></span>

<span data-ttu-id="2fa64-268">Antes de EF Core 3.0, estos nombres de métodos se sobrecargaban para funcionar tanto con una cadena normal como con una cadena que se debería interpolar en SQL y parámetros.</span><span class="sxs-lookup"><span data-stu-id="2fa64-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="2fa64-269">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-269">**New behavior**</span></span>

<span data-ttu-id="2fa64-270">A partir de la versión EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw` y `ExecuteSqlRawAsync` para crear una consulta con parámetros donde los parámetros se pasan por separado de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="2fa64-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="2fa64-271">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="2fa64-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated` y `ExecuteSqlInterpolatedAsync` para crear una consulta con parámetros donde los parámetros se pasan como parte de una cadena de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="2fa64-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="2fa64-273">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="2fa64-274">Tenga en cuenta que las dos consultas anteriores producirán el mismo código SQL parametrizado con los mismos parámetros SQL.</span><span class="sxs-lookup"><span data-stu-id="2fa64-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="2fa64-275">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-275">**Why**</span></span>

<span data-ttu-id="2fa64-276">Las sobrecargas del método como esta facilitan las llamadas accidentales al método de cadena sin procesar cuando la intención era llamar al método de cadena interpolada y viceversa.</span><span class="sxs-lookup"><span data-stu-id="2fa64-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="2fa64-277">Esto podría resultar en consultas que no se parametrizan cuando deberían.</span><span class="sxs-lookup"><span data-stu-id="2fa64-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="2fa64-278">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-278">**Mitigations**</span></span>

<span data-ttu-id="2fa64-279">Haga el cambio para usar los nuevos nombres de métodos.</span><span class="sxs-lookup"><span data-stu-id="2fa64-279">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="2fa64-280">Cuando el método FromSql se usa con un procedimiento almacenado no se puede redactar</span><span class="sxs-lookup"><span data-stu-id="2fa64-280">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="2fa64-281">Problema de seguimiento n.° 15392</span><span class="sxs-lookup"><span data-stu-id="2fa64-281">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="2fa64-282">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-282">**Old behavior**</span></span>

<span data-ttu-id="2fa64-283">Antes de EF Core 3.0, el método FromSql intentaba detectar si se podía redactar en el código SQL pasado.</span><span class="sxs-lookup"><span data-stu-id="2fa64-283">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="2fa64-284">Cuando el código SQL no se podía redactar, como un procedimiento almacenado, realizaba la evaluación de cliente.</span><span class="sxs-lookup"><span data-stu-id="2fa64-284">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="2fa64-285">La consulta siguiente funcionaba al ejecutar el procedimiento almacenado en el servidor y aplicar FirstOrDefault en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="2fa64-285">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="2fa64-286">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-286">**New behavior**</span></span>

<span data-ttu-id="2fa64-287">A partir de EF Core 3.0, EF Core no intentará analizar el código SQL.</span><span class="sxs-lookup"><span data-stu-id="2fa64-287">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="2fa64-288">Por tanto, si va a redactar después de FromSqlRaw/FromSqlInterpolated, EF Core redactará el código SQL generando una subconsulta.</span><span class="sxs-lookup"><span data-stu-id="2fa64-288">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="2fa64-289">Por tanto, si usa un procedimiento almacenado con la redacción, obtendrá una excepción de sintaxis de SQL no válida.</span><span class="sxs-lookup"><span data-stu-id="2fa64-289">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="2fa64-290">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-290">**Why**</span></span>

<span data-ttu-id="2fa64-291">EF Core 3.0 no admite la evaluación automática de cliente, ya que era propenso a errores, como se explica [aquí](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="2fa64-291">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="2fa64-292">**Mitigación**</span><span class="sxs-lookup"><span data-stu-id="2fa64-292">**Mitigation**</span></span>

<span data-ttu-id="2fa64-293">Si usa un procedimiento almacenado en FromSqlRaw/FromSqlInterpolated, sabe que no se puede redactar, por lo que puede agregar __AsEnumerable/AsAsyncEnumerable__ justo después de la llamada al método FromSql para evitar cualquier redacción en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="2fa64-293">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="2fa64-294">Solo se pueden especificar métodos de FromSql en raíces de consulta.</span><span class="sxs-lookup"><span data-stu-id="2fa64-294">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="2fa64-295">Problema de seguimiento n.° 15704</span><span class="sxs-lookup"><span data-stu-id="2fa64-295">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="2fa64-296">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-296">**Old behavior**</span></span>

<span data-ttu-id="2fa64-297">Antes de EF Core 3.0, el método `FromSql` podía especificarse en cualquier lugar en la consulta.</span><span class="sxs-lookup"><span data-stu-id="2fa64-297">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="2fa64-298">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-298">**New behavior**</span></span>

<span data-ttu-id="2fa64-299">A partir de EF Core 3.0, los nuevos métodos `FromSqlRaw` y `FromSqlInterpolated` (que reemplazan a `FromSql`) solo pueden especificarse en las raíces de la consulta, es decir, directamente en `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-299">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="2fa64-300">Si intenta especificarlos en cualquier otro lugar se producirá un error de compilación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-300">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="2fa64-301">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-301">**Why**</span></span>

<span data-ttu-id="2fa64-302">La especificación de `FromSql` en cualquier otro lugar diferente de `DbSet` no tenía un significado o valor agregado, y podría causar ambigüedad en ciertos escenarios.</span><span class="sxs-lookup"><span data-stu-id="2fa64-302">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="2fa64-303">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-303">**Mitigations**</span></span>

<span data-ttu-id="2fa64-304">Las invocaciones de `FromSql` se deben mover para que estén directamente en el `DbSet` al que se aplican.</span><span class="sxs-lookup"><span data-stu-id="2fa64-304">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="2fa64-305">Las consultas sin seguimiento ya no realizan la resolución de la identidad</span><span class="sxs-lookup"><span data-stu-id="2fa64-305">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="2fa64-306">Problema de seguimiento n.º 13518</span><span class="sxs-lookup"><span data-stu-id="2fa64-306">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="2fa64-307">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-307">**Old behavior**</span></span>

<span data-ttu-id="2fa64-308">Antes de EF Core 3.0, se usaba la misma instancia de la entidad para cada aparición de una entidad con un tipo e identificador determinados.</span><span class="sxs-lookup"><span data-stu-id="2fa64-308">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="2fa64-309">Este comportamiento coincide con el de las consultas de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="2fa64-309">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="2fa64-310">Por ejemplo, esta consulta:</span><span class="sxs-lookup"><span data-stu-id="2fa64-310">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="2fa64-311">Esta consulta devolverá la misma instancia de `Category` para cada elemento `Product` asociado con la categoría determinada.</span><span class="sxs-lookup"><span data-stu-id="2fa64-311">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="2fa64-312">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-312">**New behavior**</span></span>

<span data-ttu-id="2fa64-313">A partir de EF Core 3.0, se crean distintas instancias de la entidad si se encuentra una entidad con un tipo e identificador determinados en varias ubicaciones del gráfico devuelto.</span><span class="sxs-lookup"><span data-stu-id="2fa64-313">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="2fa64-314">Por ejemplo, la consulta anterior ahora devolverá una nueva instancia de `Category` para cada elemento `Product` cuando haya dos productos asociados a la misma categoría.</span><span class="sxs-lookup"><span data-stu-id="2fa64-314">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="2fa64-315">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-315">**Why**</span></span>

<span data-ttu-id="2fa64-316">La resolución de las identidades (es decir, el hecho de determinar que una entidad tiene los mismos tipo e identificador que la entidad encontrada) agrega más rendimiento y sobrecarga de memoria.</span><span class="sxs-lookup"><span data-stu-id="2fa64-316">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="2fa64-317">Este enfoque suele ser contrario a por qué las consultas sin seguimiento se usan en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="2fa64-317">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="2fa64-318">Además, aunque la resolución de las identidades a veces puede resultar útil, no es necesaria si las entidades se van a serializar y enviar a un cliente, algo habitual para las consultas sin seguimiento.</span><span class="sxs-lookup"><span data-stu-id="2fa64-318">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="2fa64-319">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-319">**Mitigations**</span></span>

<span data-ttu-id="2fa64-320">Si se requiere la resolución de identidad, use una consulta de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="2fa64-320">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="2fa64-321">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="2fa64-321">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="2fa64-322">Problema de seguimiento n.º 14523</span><span class="sxs-lookup"><span data-stu-id="2fa64-322">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="2fa64-323">Revertimos este cambio porque la nueva configuración de EF Core 3.0 permite a la aplicación especificar el nivel de registro para cualquier evento.</span><span class="sxs-lookup"><span data-stu-id="2fa64-323">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="2fa64-324">Por ejemplo, para cambiar el registro de SQL a `Debug`, configure el nivel de forma explícita en `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="2fa64-324">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="2fa64-325">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="2fa64-325">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="2fa64-326">Problema de seguimiento n.º 12378</span><span class="sxs-lookup"><span data-stu-id="2fa64-326">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="2fa64-327">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-327">**Old behavior**</span></span>

<span data-ttu-id="2fa64-328">Antes de EF Core 3.0, los valores temporales se asignaban a todas las propiedades de clave para las que posteriormente la base de datos generaba un valor real.</span><span class="sxs-lookup"><span data-stu-id="2fa64-328">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="2fa64-329">Normalmente, estos valores temporales eran números negativos grandes.</span><span class="sxs-lookup"><span data-stu-id="2fa64-329">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="2fa64-330">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-330">**New behavior**</span></span>

<span data-ttu-id="2fa64-331">A partir de la versión 3.0, en EF Core se almacena el valor de clave temporal como parte de la información de seguimiento de la entidad y la propiedad clave en sí no se modifica.</span><span class="sxs-lookup"><span data-stu-id="2fa64-331">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="2fa64-332">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-332">**Why**</span></span>

<span data-ttu-id="2fa64-333">Este cambio se ha realizado para evitar que los valores de clave temporales se conviertan erróneamente en permanentes cuando una entidad de la que previamente una instancia de `DbContext` ha realizado el seguimiento se mueve a otra instancia de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-333">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="2fa64-334">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-334">**Mitigations**</span></span>

<span data-ttu-id="2fa64-335">Las aplicaciones que asignan valores de clave principal a claves externas para crear asociaciones entre entidades pueden depender del comportamiento anterior si las claves principales son generadas por el almacén y pertenecen a entidades en el estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-335">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="2fa64-336">Esto se puede evitar con las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="2fa64-336">This can be avoided by:</span></span>
* <span data-ttu-id="2fa64-337">No se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="2fa64-337">Not using store-generated keys.</span></span>
* <span data-ttu-id="2fa64-338">Se establecen propiedades de navegación para crear relaciones en lugar de establecer valores de clave externa.</span><span class="sxs-lookup"><span data-stu-id="2fa64-338">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="2fa64-339">Se obtienen los valores de clave temporal reales de la información de seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="2fa64-339">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="2fa64-340">Por ejemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` devolverá el valor temporal aunque no se haya establecido `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-340">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="2fa64-341">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="2fa64-341">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="2fa64-342">Problema de seguimiento n.º 14616</span><span class="sxs-lookup"><span data-stu-id="2fa64-342">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="2fa64-343">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-343">**Old behavior**</span></span>

<span data-ttu-id="2fa64-344">Antes de EF Core 3.0, se realizaba el seguimiento en el estado `Added` de las entidades sin seguimiento detectadas por `DetectChanges` y se insertaban como una fila nueva cuando se llamaba a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-344">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="2fa64-345">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-345">**New behavior**</span></span>

<span data-ttu-id="2fa64-346">A partir de EF Core 3.0, si una entidad usa valores de clave generados y se establece un valor de clave, se realizará el seguimiento de la entidad en el estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-346">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="2fa64-347">Esto significa que se supone que existe una fila para la entidad y que se actualizará cuando se llame a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-347">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="2fa64-348">Si no se establece el valor de clave, o bien si el tipo de entidad no usa claves generadas, se seguirá realizando el seguimiento de la entidad nueva como `Added` al igual que en las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="2fa64-348">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="2fa64-349">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-349">**Why**</span></span>

<span data-ttu-id="2fa64-350">Este cambio se ha realizado para que sea más sencillo y coherente trabajar con gráficos de entidades desconectadas mientras se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="2fa64-350">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="2fa64-351">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-351">**Mitigations**</span></span>

<span data-ttu-id="2fa64-352">Este cambio puede interrumpir una aplicación si se configura un tipo de entidad para usar claves generadas, pero se establecen de forma explícita valores de clave para las instancias nuevas.</span><span class="sxs-lookup"><span data-stu-id="2fa64-352">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="2fa64-353">La solución consiste en configurar de forma explícita las propiedades de clave para que no usen valores generados.</span><span class="sxs-lookup"><span data-stu-id="2fa64-353">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="2fa64-354">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="2fa64-354">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="2fa64-355">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="2fa64-355">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="2fa64-356">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="2fa64-356">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="2fa64-357">Problema de seguimiento n.º 10114</span><span class="sxs-lookup"><span data-stu-id="2fa64-357">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="2fa64-358">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-358">**Old behavior**</span></span>

<span data-ttu-id="2fa64-359">Antes de la versión 3.0, en EF Core no se aplicaban acciones en cascada (eliminación de entidades dependientes cuando se eliminaba una entidad de seguridad obligatoria o cuando se rompía la relación con una entidad de seguridad obligatoria) hasta que se llamaba a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="2fa64-359">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="2fa64-360">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-360">**New behavior**</span></span>

<span data-ttu-id="2fa64-361">A partir de 3.0, en EF Core las acciones en cascada se aplican en cuanto se detecta la condición desencadenadora.</span><span class="sxs-lookup"><span data-stu-id="2fa64-361">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="2fa64-362">Por ejemplo, como resultado de la llamada a `context.Remove()` para eliminar una entidad de seguridad, todos los dependientes obligatorios relacionados de los que se realiza el seguimiento también se establecen en `Deleted` inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="2fa64-362">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="2fa64-363">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-363">**Why**</span></span>

<span data-ttu-id="2fa64-364">Este cambio se ha realizado para mejorar la experiencia en escenarios de auditoría y enlace de datos donde es importante comprender qué entidades se van a eliminar _antes_ de llamar a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-364">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="2fa64-365">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-365">**Mitigations**</span></span>

<span data-ttu-id="2fa64-366">El comportamiento anterior se puede restaurar mediante opciones de `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-366">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="2fa64-367">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-367">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="2fa64-368">La carga diligente de entidades relacionadas ahora se realiza en una sola consulta</span><span class="sxs-lookup"><span data-stu-id="2fa64-368">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="2fa64-369">Problema de seguimiento n.º 18022</span><span class="sxs-lookup"><span data-stu-id="2fa64-369">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="2fa64-370">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-370">**Old behavior**</span></span>

<span data-ttu-id="2fa64-371">Antes de la versión 3.0, la carga diligente de navegaciones de colección a través de operadores `Include` provocaba la generación de varias consultas en la base de datos relacional, una para cada tipo de entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="2fa64-371">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="2fa64-372">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-372">**New behavior**</span></span>

<span data-ttu-id="2fa64-373">A partir de la versión 3.0, EF Core genera una sola consulta con operadores JOIN en las bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="2fa64-373">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="2fa64-374">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-374">**Why**</span></span>

<span data-ttu-id="2fa64-375">La emisión de varias consultas para implementar una única consulta LINQ provocaba numerosos problemas, incluido el rendimiento negativo, ya que se necesitaban varios recorridos de ida y vuelta a la base de datos, y problemas de coherencia de datos, ya que cada consulta podía observar un estado distinto de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2fa64-375">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="2fa64-376">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-376">**Mitigations**</span></span>

<span data-ttu-id="2fa64-377">Aunque técnicamente esto no es un cambio importante, podría tener un efecto considerable en el rendimiento de la aplicación cuando una sola consulta contiene un gran número de operadores `Include` en las navegaciones de la colección.</span><span class="sxs-lookup"><span data-stu-id="2fa64-377">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="2fa64-378">[Vea este comentario](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) para obtener más información y para volver a escribir las consultas de una manera más eficaz.</span><span class="sxs-lookup"><span data-stu-id="2fa64-378">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="2fa64-379">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="2fa64-379">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="2fa64-380">Problema de seguimiento n.º 12661</span><span class="sxs-lookup"><span data-stu-id="2fa64-380">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="2fa64-381">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-381">**Old behavior**</span></span>

<span data-ttu-id="2fa64-382">Antes de la versión 3.0, `DeleteBehavior.Restrict` creaba claves externas en la base de datos con la semántica `Restrict`, pero también realizaba una corrección interna de manera no evidente.</span><span class="sxs-lookup"><span data-stu-id="2fa64-382">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="2fa64-383">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-383">**New behavior**</span></span>

<span data-ttu-id="2fa64-384">A partir de la versión 3.0, `DeleteBehavior.Restrict` garantiza que las claves externas se crean con la semántica `Restrict`, es decir, sin cascadas y realizando una infracción de restricción, sin llevar a cabo correcciones internas de EF.</span><span class="sxs-lookup"><span data-stu-id="2fa64-384">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="2fa64-385">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-385">**Why**</span></span>

<span data-ttu-id="2fa64-386">Este cambio se realizó para mejorar la experiencia de uso de `DeleteBehavior` de manera intuitiva sin efectos secundarios inesperados.</span><span class="sxs-lookup"><span data-stu-id="2fa64-386">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="2fa64-387">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-387">**Mitigations**</span></span>

<span data-ttu-id="2fa64-388">El comportamiento anterior se puede restaurar con `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-388">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="2fa64-389">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="2fa64-389">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="2fa64-390">Problema de seguimiento n.º 14194</span><span class="sxs-lookup"><span data-stu-id="2fa64-390">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="2fa64-391">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-391">**Old behavior**</span></span>

<span data-ttu-id="2fa64-392">Antes de EF Core 3.0, los [tipos de consulta](xref:core/modeling/keyless-entity-types) eran un medio para consultar los datos que no definen una clave principal de una manera estructurada.</span><span class="sxs-lookup"><span data-stu-id="2fa64-392">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="2fa64-393">Es decir, un tipo de consulta se usaba para asignar tipos de entidad sin claves (más probablemente desde una vista, pero posiblemente desde una tabla), mientras que un tipo de entidad estándar se usaba cuando había una clave disponible (más probablemente desde una tabla, pero posiblemente desde una vista).</span><span class="sxs-lookup"><span data-stu-id="2fa64-393">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="2fa64-394">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-394">**New behavior**</span></span>

<span data-ttu-id="2fa64-395">Ahora un tipo de consulta se convierte en un tipo de entidad sin una clave principal.</span><span class="sxs-lookup"><span data-stu-id="2fa64-395">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="2fa64-396">Los tipos de entidad sin clave tienen la misma funcionalidad que los tipos de consulta de las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="2fa64-396">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="2fa64-397">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-397">**Why**</span></span>

<span data-ttu-id="2fa64-398">Este cambio se ha realizado para reducir la confusión en torno a la finalidad de los tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="2fa64-398">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="2fa64-399">En concreto, son tipos de entidad sin clave y, por ello, intrínsecamente son de solo lectura, pero no se deben usar solo porque un tipo de entidad tenga que ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="2fa64-399">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="2fa64-400">Del mismo modo, se suelen asignar a vistas, pero solo porque las vistas no suelen definir claves.</span><span class="sxs-lookup"><span data-stu-id="2fa64-400">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="2fa64-401">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-401">**Mitigations**</span></span>

<span data-ttu-id="2fa64-402">Los elementos siguientes de la API ahora están obsoletos:</span><span class="sxs-lookup"><span data-stu-id="2fa64-402">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="2fa64-403">**`ModelBuilder.Query<>()`** : en su lugar es necesario llamar a `ModelBuilder.Entity<>().HasNoKey()` para marcar un tipo de entidad como sin claves.</span><span class="sxs-lookup"><span data-stu-id="2fa64-403">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="2fa64-404">Esto todavía no se configurará por convención para evitar una configuración incorrecta cuando se espera una clave principal, pero no coincide con la convención.</span><span class="sxs-lookup"><span data-stu-id="2fa64-404">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="2fa64-405">**`DbQuery<>`** : en su lugar se debe usar `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-405">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="2fa64-406">**`DbContext.Query<>()`** : en su lugar se debe usar `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-406">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="2fa64-407">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="2fa64-407">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="2fa64-408">[Problema de seguimiento n.º 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema de seguimiento n.º 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema de seguimiento n.º 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="2fa64-408">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="2fa64-409">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-409">**Old behavior**</span></span>

<span data-ttu-id="2fa64-410">Antes de EF Core 3.0, la configuración de la relación de propiedad se realizaba directamente después de la llamada a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-410">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="2fa64-411">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-411">**New behavior**</span></span>

<span data-ttu-id="2fa64-412">A partir de EF Core 3.0, ahora hay una API fluida para configurar una propiedad de navegación para el propietario mediante `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-412">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="2fa64-413">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-413">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="2fa64-414">La configuración relacionada con la relación entre el propietario y lo que se posee ahora se debe encadenar después de `WithOwner()`, de forma similar a cómo se configuran otras relaciones.</span><span class="sxs-lookup"><span data-stu-id="2fa64-414">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="2fa64-415">Pero la configuración del propio tipo de propiedad se seguirá encadenando después de `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-415">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="2fa64-416">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-416">For example:</span></span>

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

<span data-ttu-id="2fa64-417">Además, la llamada a `Entity()`, `HasOne()` o `Set()` con un tipo de propiedad de destino ahora iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="2fa64-417">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="2fa64-418">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-418">**Why**</span></span>

<span data-ttu-id="2fa64-419">Este cambio se ha realizado para crear una separación más clara entre la configuración del propio tipo de propiedad y la _relación con_ el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="2fa64-419">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="2fa64-420">A su vez, esto elimina la ambigüedad y la confusión de métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-420">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="2fa64-421">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-421">**Mitigations**</span></span>

<span data-ttu-id="2fa64-422">Cambie la configuración de las relaciones de tipo de propiedad para usar la nueva superficie de API, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="2fa64-422">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="2fa64-423">Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales</span><span class="sxs-lookup"><span data-stu-id="2fa64-423">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="2fa64-424">Problema de seguimiento n.º 9005</span><span class="sxs-lookup"><span data-stu-id="2fa64-424">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="2fa64-425">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-425">**Old behavior**</span></span>

<span data-ttu-id="2fa64-426">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fa64-426">Consider the following model:</span></span>
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
<span data-ttu-id="2fa64-427">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, siempre era necesaria una instancia de `OrderDetails` al agregar un elemento `Order` nuevo.</span><span class="sxs-lookup"><span data-stu-id="2fa64-427">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="2fa64-428">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-428">**New behavior**</span></span>

<span data-ttu-id="2fa64-429">A partir de la versión 3.0, EF Core permite agregar `Order` sin `OrderDetails` y asigna todas las propiedades `OrderDetails` a excepción de la clave principal a columnas que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="2fa64-429">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="2fa64-430">Al realizar consultas, EF Core establece `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor o si no tiene propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-430">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="2fa64-431">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-431">**Mitigations**</span></span>

<span data-ttu-id="2fa64-432">Si el modelo tiene una tabla que comparte dependencias con todas las columnas opcionales, pero la navegación que apunta a ella no se espera que sea `null`, la aplicación debería modificarse para controlar los casos en los que la navegación sea `null`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-432">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="2fa64-433">Si esto no es posible, debería agregarse una propiedad necesaria al tipo de entidad o, al menos, una entidad debería tener un valor distinto a `null` asignado.</span><span class="sxs-lookup"><span data-stu-id="2fa64-433">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="2fa64-434">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="2fa64-434">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="2fa64-435">Problema de seguimiento n.º 14154</span><span class="sxs-lookup"><span data-stu-id="2fa64-435">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="2fa64-436">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-436">**Old behavior**</span></span>

<span data-ttu-id="2fa64-437">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fa64-437">Consider the following model:</span></span>
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
<span data-ttu-id="2fa64-438">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, si solo se actualizaba `OrderDetails`, no se actualizaba el valor `Version` en el cliente y se producía un error en la próxima actualización.</span><span class="sxs-lookup"><span data-stu-id="2fa64-438">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="2fa64-439">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-439">**New behavior**</span></span>

<span data-ttu-id="2fa64-440">A partir de la versión 3.0, EF Core propaga el nuevo valor `Version` en `Order` si posee `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-440">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="2fa64-441">En caso contrario, se produce una excepción durante la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="2fa64-441">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="2fa64-442">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-442">**Why**</span></span>

<span data-ttu-id="2fa64-443">Este cambio se realizó para evitar un valor de token de simultaneidad obsoleto cuando solo se actualiza una de las entidades asignadas a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="2fa64-443">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="2fa64-444">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-444">**Mitigations**</span></span>

<span data-ttu-id="2fa64-445">Todas las entidades que comparten la tabla deben incluir una propiedad que se asigna a la columna del token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="2fa64-445">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="2fa64-446">Es posible crear una en estado reemplazado:</span><span class="sxs-lookup"><span data-stu-id="2fa64-446">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="2fa64-447">Ahora, las propiedades heredadas de tipos sin asignar se asignan a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="2fa64-447">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="2fa64-448">Problema de seguimiento n.º 13998</span><span class="sxs-lookup"><span data-stu-id="2fa64-448">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="2fa64-449">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-449">**Old behavior**</span></span>

<span data-ttu-id="2fa64-450">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fa64-450">Consider the following model:</span></span>
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

<span data-ttu-id="2fa64-451">Antes de EF Core 3.0, la propiedad `ShippingAddress` se asignaba a columnas distintas para `BulkOrder` y `Order` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2fa64-451">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="2fa64-452">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-452">**New behavior**</span></span>

<span data-ttu-id="2fa64-453">A partir de la versión3.0, EF Core solo crea una columna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-453">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="2fa64-454">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-454">**Why**</span></span>

<span data-ttu-id="2fa64-455">El comportamiento anterior no era el esperado.</span><span class="sxs-lookup"><span data-stu-id="2fa64-455">The old behavoir was unexpected.</span></span>

<span data-ttu-id="2fa64-456">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-456">**Mitigations**</span></span>

<span data-ttu-id="2fa64-457">Todavía se puede asignar explícitamente la propiedad a columnas separadas en los tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="2fa64-457">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="2fa64-458">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="2fa64-458">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="2fa64-459">Problema de seguimiento n.º 13274</span><span class="sxs-lookup"><span data-stu-id="2fa64-459">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="2fa64-460">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-460">**Old behavior**</span></span>

<span data-ttu-id="2fa64-461">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fa64-461">Consider the following model:</span></span>
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
<span data-ttu-id="2fa64-462">Antes de EF Core 3.0, se podía usar la propiedad `CustomerId` para la clave externa por convención.</span><span class="sxs-lookup"><span data-stu-id="2fa64-462">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="2fa64-463">Pero si `Order` es un tipo de propiedad, entonces esto convertiría también a `CustomerId` en la clave principal, algo que no suele ser lo esperado.</span><span class="sxs-lookup"><span data-stu-id="2fa64-463">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="2fa64-464">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-464">**New behavior**</span></span>

<span data-ttu-id="2fa64-465">A partir de la versión 3.0, EF Core no intenta usar las propiedades de claves externas por convención si tienen el mismo nombre que la propiedad de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="2fa64-465">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="2fa64-466">Los patrones de nombre de tipo de entidad de seguridad concatenado con el nombre de propiedad de la entidad de seguridad y de nombre de navegación concatenado con el nombre de propiedad de la entidad de seguridad todavía se hacen coincidir.</span><span class="sxs-lookup"><span data-stu-id="2fa64-466">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="2fa64-467">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-467">For example:</span></span>

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

<span data-ttu-id="2fa64-468">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-468">**Why**</span></span>

<span data-ttu-id="2fa64-469">Este cambio se ha realizado para evitar definir erróneamente una propiedad de clave principal en el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="2fa64-469">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="2fa64-470">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-470">**Mitigations**</span></span>

<span data-ttu-id="2fa64-471">Si la propiedad se ha diseñado para ser la clave externa y, por tanto, parte de la clave principal, se debe configurar explícitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="2fa64-471">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="2fa64-472">Ahora, la conexión de base de datos se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="2fa64-472">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="2fa64-473">Problema de seguimiento n.º 14218</span><span class="sxs-lookup"><span data-stu-id="2fa64-473">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="2fa64-474">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-474">**Old behavior**</span></span>

<span data-ttu-id="2fa64-475">Antes de EF Core 3.0, si el contexto abría la conexión dentro de `TransactionScope`, la conexión permanecía abierta mientras el ámbito actual `TransactionScope` estuviese activo.</span><span class="sxs-lookup"><span data-stu-id="2fa64-475">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="2fa64-476">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-476">**New behavior**</span></span>

<span data-ttu-id="2fa64-477">A partir de la versión 3.0, EF Core cierra la conexión en cuanto se deja de usar.</span><span class="sxs-lookup"><span data-stu-id="2fa64-477">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="2fa64-478">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-478">**Why**</span></span>

<span data-ttu-id="2fa64-479">Este cambio permite usar varios contextos en el mismo ámbito `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-479">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="2fa64-480">El comportamiento nuevo también coincide con el de EF6.</span><span class="sxs-lookup"><span data-stu-id="2fa64-480">The new behavior also matches EF6.</span></span>

<span data-ttu-id="2fa64-481">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-481">**Mitigations**</span></span>

<span data-ttu-id="2fa64-482">Si la conexión debe permanecer abierta, una llamada explícita a `OpenConnection()` asegurará que EF Core no la cierre de forma prematura:</span><span class="sxs-lookup"><span data-stu-id="2fa64-482">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="2fa64-483">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="2fa64-483">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="2fa64-484">Problema de seguimiento n.º 6872</span><span class="sxs-lookup"><span data-stu-id="2fa64-484">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="2fa64-485">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-485">**Old behavior**</span></span>

<span data-ttu-id="2fa64-486">Antes de EF Core 3.0, se usaba un generador de valores compartidos para todas las propiedades de clave entera en memoria.</span><span class="sxs-lookup"><span data-stu-id="2fa64-486">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="2fa64-487">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-487">**New behavior**</span></span>

<span data-ttu-id="2fa64-488">A partir de EF Core 3.0, cada propiedad de clave entera obtiene su propio generador de valores cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="2fa64-488">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="2fa64-489">Además, si se elimina la base de datos, la generación de claves se restablece para todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="2fa64-489">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="2fa64-490">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-490">**Why**</span></span>

<span data-ttu-id="2fa64-491">Este cambio se ha realizado para alinear la generación de claves en memoria más estrechamente a la generación de claves de base de datos reales y para mejorar la capacidad para aislar las pruebas entre sí cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="2fa64-491">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="2fa64-492">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-492">**Mitigations**</span></span>

<span data-ttu-id="2fa64-493">Esto puede interrumpir una aplicación que se base en el establecimiento de valores de clave específicos en memoria.</span><span class="sxs-lookup"><span data-stu-id="2fa64-493">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="2fa64-494">En su lugar, considere la posibilidad de no depender de valores de clave específicos, o bien de actualizar para que coincida con el comportamiento nuevo.</span><span class="sxs-lookup"><span data-stu-id="2fa64-494">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="2fa64-495">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="2fa64-495">Backing fields are used by default</span></span>

[<span data-ttu-id="2fa64-496">Problema de seguimiento n.º 12430</span><span class="sxs-lookup"><span data-stu-id="2fa64-496">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="2fa64-497">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-497">**Old behavior**</span></span>

<span data-ttu-id="2fa64-498">Antes de la versión 3.0, incluso si se conocía el campo de respaldo de una propiedad, de forma predeterminada en EF Core se leía y escribía el valor de propiedad mediante los métodos captadores y establecedores de propiedades.</span><span class="sxs-lookup"><span data-stu-id="2fa64-498">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="2fa64-499">La excepción era la ejecución de consultas, donde el campo de respaldo se establecía directamente si se conocía.</span><span class="sxs-lookup"><span data-stu-id="2fa64-499">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="2fa64-500">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-500">**New behavior**</span></span>

<span data-ttu-id="2fa64-501">A partir de EF Core 3.0, si se conoce el campo de respaldo para una propiedad, EF Core siempre la leerá y escribirá mediante el campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="2fa64-501">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="2fa64-502">Esto podría provocar una interrupción de la aplicación si depende de un comportamiento adicional codificado en los métodos captadores o establecedores.</span><span class="sxs-lookup"><span data-stu-id="2fa64-502">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="2fa64-503">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-503">**Why**</span></span>

<span data-ttu-id="2fa64-504">Este cambio se ha realizado para evitar que EF Core desencadene erróneamente lógica de negocios de forma predeterminada al realizar operaciones de base de datos que implican entidades.</span><span class="sxs-lookup"><span data-stu-id="2fa64-504">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="2fa64-505">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-505">**Mitigations**</span></span>

<span data-ttu-id="2fa64-506">El comportamiento anterior a la versión 3.0 se puede restaurar mediante la configuración del modo de acceso de propiedad en `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-506">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="2fa64-507">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-507">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="2fa64-508">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="2fa64-508">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="2fa64-509">Problema de seguimiento n.º 12523</span><span class="sxs-lookup"><span data-stu-id="2fa64-509">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="2fa64-510">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-510">**Old behavior**</span></span>

<span data-ttu-id="2fa64-511">Antes de EF Core 3.0, si varios campos coincidían con las reglas para buscar el campo de respaldo de una propiedad, se elegía un campo según un orden de prioridad.</span><span class="sxs-lookup"><span data-stu-id="2fa64-511">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="2fa64-512">Esto podía producir que, en caso de ambigüedad, se usara el campo incorrecto.</span><span class="sxs-lookup"><span data-stu-id="2fa64-512">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="2fa64-513">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-513">**New behavior**</span></span>

<span data-ttu-id="2fa64-514">A partir de EF Core 3.0, si varios campos coinciden con la misma propiedad, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="2fa64-514">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="2fa64-515">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-515">**Why**</span></span>

<span data-ttu-id="2fa64-516">Este cambio se ha realizado para evitar de forma silenciosa el uso de un campo con respecto a otro cuando solo uno puede ser correcto.</span><span class="sxs-lookup"><span data-stu-id="2fa64-516">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="2fa64-517">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-517">**Mitigations**</span></span>

<span data-ttu-id="2fa64-518">En las propiedades con campos de respaldo ambiguos se debe especificar de forma explícita el campo que se va usar.</span><span class="sxs-lookup"><span data-stu-id="2fa64-518">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="2fa64-519">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="2fa64-519">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="2fa64-520">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="2fa64-520">Field-only property names should match the field name</span></span>

<span data-ttu-id="2fa64-521">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-521">**Old behavior**</span></span>

<span data-ttu-id="2fa64-522">Antes de EF Core 3.0, una propiedad podía especificarse con un valor de cadena y, si no había ninguna propiedad con ese nombre en el tipo .NET, EF Core intentaba hacerla coincidir con un campo mediante reglas de convención.</span><span class="sxs-lookup"><span data-stu-id="2fa64-522">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="2fa64-523">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-523">**New behavior**</span></span>

<span data-ttu-id="2fa64-524">A partir de EF Core 3.0, una propiedad de solo campo debe coincidir exactamente con el nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="2fa64-524">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="2fa64-525">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-525">**Why**</span></span>

<span data-ttu-id="2fa64-526">Este cambio se realizó para evitar el uso del mismo campo para dos propiedades con nombres similares. También hace que las reglas de coincidencia para propiedades solo de campo sean las mismas que para las propiedades asignadas a propiedades CLR.</span><span class="sxs-lookup"><span data-stu-id="2fa64-526">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="2fa64-527">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-527">**Mitigations**</span></span>

<span data-ttu-id="2fa64-528">Las propiedades solo de campo deberían tener el mismo nombre que el campo al que están asignadas.</span><span class="sxs-lookup"><span data-stu-id="2fa64-528">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="2fa64-529">En una próxima versión de EF Core 3.0 tenemos planeado volver a habilitar la configuración explícita de un nombre de campo distinto al nombre de la propiedad (vea el problema [n.° 15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="2fa64-529">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="2fa64-530">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="2fa64-530">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="2fa64-531">Problema de seguimiento n.° 14756</span><span class="sxs-lookup"><span data-stu-id="2fa64-531">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="2fa64-532">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-532">**Old behavior**</span></span>

<span data-ttu-id="2fa64-533">Antes de EF Core 3.0, llamar a `AddDbContext` o a `AddDbContextPool` también podría registrar los servicios de almacenamiento en caché y de registro con D.I. a través de llamadas a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) y a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2fa64-533">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="2fa64-534">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-534">**New behavior**</span></span>

<span data-ttu-id="2fa64-535">A partir de EF Core 3.0, `AddDbContext` y `AddDbContextPool` ya no registrarán estos servicios con inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="2fa64-535">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="2fa64-536">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-536">**Why**</span></span>

<span data-ttu-id="2fa64-537">EF Core 3.0 no requiere que estos servicios estén en el contenedor de inserción de dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-537">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="2fa64-538">Pero si `ILoggerFactory` se registra en el contenedor de DI de la aplicación, EF Core lo empezará a usar de todos modos.</span><span class="sxs-lookup"><span data-stu-id="2fa64-538">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="2fa64-539">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-539">**Mitigations**</span></span>

<span data-ttu-id="2fa64-540">Si la aplicación necesita estos servicios, regístrelos de manera explícita con el contenedor de DI mediante [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2fa64-540">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="2fa64-541">Ahora DbContext.Entry realiza una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="2fa64-541">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="2fa64-542">Problema de seguimiento n.º 13552</span><span class="sxs-lookup"><span data-stu-id="2fa64-542">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="2fa64-543">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-543">**Old behavior**</span></span>

<span data-ttu-id="2fa64-544">Antes de EF Core 3.0, la llamada a `DbContext.Entry` provocaba que se detectaran cambios para todas las entidades con seguimiento.</span><span class="sxs-lookup"><span data-stu-id="2fa64-544">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="2fa64-545">Esto garantizaba que el estado expuesto en `EntityEntry` estuviera actualizado.</span><span class="sxs-lookup"><span data-stu-id="2fa64-545">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="2fa64-546">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-546">**New behavior**</span></span>

<span data-ttu-id="2fa64-547">A partir de EF Core 3.0, ahora la llamada a `DbContext.Entry` solo intenta detectar cambios en la entidad dada y cualquier entidad de seguridad relacionada con ella de la que se haya realizado el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="2fa64-547">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="2fa64-548">Esto significa que es posible que la llamada a este método no haya detectado otros cambios, lo que podría tener implicaciones en el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-548">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="2fa64-549">Observe que si `ChangeTracker.AutoDetectChangesEnabled` se establece en `false` incluso esta detección de cambios local se deshabilitará.</span><span class="sxs-lookup"><span data-stu-id="2fa64-549">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="2fa64-550">Otros métodos que provocan la detección de cambios (como `ChangeTracker.Entries` y `SaveChanges`) siguen provocando una acción `DetectChanges` completa de todas las entidades de las que se realiza el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="2fa64-550">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="2fa64-551">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-551">**Why**</span></span>

<span data-ttu-id="2fa64-552">Este cambio se ha realizado para mejorar el rendimiento predeterminado del uso de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-552">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="2fa64-553">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-553">**Mitigations**</span></span>

<span data-ttu-id="2fa64-554">Llame a `ChgangeTracker.DetectChanges()` de forma explícita antes de llamar a `Entry` para garantizar el comportamiento anterior a la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="2fa64-554">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="2fa64-555">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="2fa64-555">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="2fa64-556">Problema de seguimiento n.º 14617</span><span class="sxs-lookup"><span data-stu-id="2fa64-556">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="2fa64-557">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-557">**Old behavior**</span></span>

<span data-ttu-id="2fa64-558">Antes de EF Core 3.0, se podían usar las propiedades de clave `string` y `byte[]` sin tener que establecer de forma explícita un valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="2fa64-558">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="2fa64-559">En ese caso, el valor de clave se generaba en el cliente como un GUID, que se serializaba en bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-559">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="2fa64-560">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-560">**New behavior**</span></span>

<span data-ttu-id="2fa64-561">A partir de EF Core 3.0, se iniciará una excepción en la que indica que no se ha establecido ningún valor de clave.</span><span class="sxs-lookup"><span data-stu-id="2fa64-561">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="2fa64-562">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-562">**Why**</span></span>

<span data-ttu-id="2fa64-563">Este cambio se ha realizado porque los valores `string`/`byte[]` generados por el cliente no suelen ser útiles, y el comportamiento predeterminado dificultaba razonar sobre los valores de clave generados de una forma habitual.</span><span class="sxs-lookup"><span data-stu-id="2fa64-563">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="2fa64-564">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-564">**Mitigations**</span></span>

<span data-ttu-id="2fa64-565">Se puede obtener el comportamiento anterior a la versión 3.0 si se especifica de forma explícita que las propiedades de clave deben usar los valores generados si no se establece ningún otro valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="2fa64-565">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="2fa64-566">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="2fa64-566">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="2fa64-567">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="2fa64-567">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="2fa64-568">Ahora ILoggerFactory es un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="2fa64-568">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="2fa64-569">Problema de seguimiento n.º 14698</span><span class="sxs-lookup"><span data-stu-id="2fa64-569">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="2fa64-570">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-570">**Old behavior**</span></span>

<span data-ttu-id="2fa64-571">Antes de EF Core 3.0, `ILoggerFactory` se registraba como un servicio de singleton.</span><span class="sxs-lookup"><span data-stu-id="2fa64-571">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="2fa64-572">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-572">**New behavior**</span></span>

<span data-ttu-id="2fa64-573">A partir de EF Core 3.0, `ILoggerFactory` ahora se registra como con ámbito.</span><span class="sxs-lookup"><span data-stu-id="2fa64-573">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="2fa64-574">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-574">**Why**</span></span>

<span data-ttu-id="2fa64-575">Este cambio se ha realizado para permitir la asociación de un registrador con una instancia de `DbContext`, lo que habilita otras funciones y quita algunos casos de comportamiento patológico como un aumento vertiginoso de los proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="2fa64-575">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="2fa64-576">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-576">**Mitigations**</span></span>

<span data-ttu-id="2fa64-577">Este cambio no debería afectar al código de la aplicación a menos que registre y use servicios personalizados en el proveedor de servicios internos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="2fa64-577">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="2fa64-578">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="2fa64-578">This isn't common.</span></span>
<span data-ttu-id="2fa64-579">En estos casos, la mayoría de los elementos seguirá funcionando, pero cualquier servicio de singleton que dependiera de `ILoggerFactory` tendrá que cambiarse para obtener la interfaz `ILoggerFactory` de otra forma.</span><span class="sxs-lookup"><span data-stu-id="2fa64-579">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="2fa64-580">Si experimenta situaciones como esta, registre un problema en el [rastreador de problemas de GitHub de EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para hacernos saber cómo usa `ILoggerFactory`, para que podamos comprender mejor cómo evitar esta interrupción en el futuro.</span><span class="sxs-lookup"><span data-stu-id="2fa64-580">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="2fa64-581">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="2fa64-581">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="2fa64-582">Problema de seguimiento n.º 12780</span><span class="sxs-lookup"><span data-stu-id="2fa64-582">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="2fa64-583">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-583">**Old behavior**</span></span>

<span data-ttu-id="2fa64-584">Antes de EF Core 3.0, una vez que se eliminaba `DbContext` no había ninguna forma de saber si una determinada propiedad de navegación de una entidad obtenida de ese contexto se había cargado completamente o no.</span><span class="sxs-lookup"><span data-stu-id="2fa64-584">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="2fa64-585">En su lugar, los proxies asumían que una navegación de referencia se cargaba si tenía un valor distinto de NULL, y que una navegación de colección se cargaba si no estaba vacía.</span><span class="sxs-lookup"><span data-stu-id="2fa64-585">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="2fa64-586">En estos casos, el intento de carga diferida era no operativo.</span><span class="sxs-lookup"><span data-stu-id="2fa64-586">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="2fa64-587">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-587">**New behavior**</span></span>

<span data-ttu-id="2fa64-588">A partir de EF Core 3.0, los proxies realizan el seguimiento de si una propiedad de navegación se carga o no.</span><span class="sxs-lookup"><span data-stu-id="2fa64-588">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="2fa64-589">Esto significa que el intento de acceder a una propiedad de navegación que se carga después de que se haya eliminado el contexto siempre será no operativo, incluso cuando la navegación cargada está vacía o es NULL.</span><span class="sxs-lookup"><span data-stu-id="2fa64-589">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="2fa64-590">Por el contrario, el intento de acceder a una propiedad de navegación que no está cargada iniciará una excepción si el contexto se ha eliminado, incluso si la propiedad de navegación es una colección no vacía.</span><span class="sxs-lookup"><span data-stu-id="2fa64-590">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="2fa64-591">Si se produce esta situación, significa que el código de aplicación está intentando usar la carga diferida en un momento no válido y que se debe cambiar la aplicación para que lo no haga.</span><span class="sxs-lookup"><span data-stu-id="2fa64-591">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="2fa64-592">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-592">**Why**</span></span>

<span data-ttu-id="2fa64-593">Este cambio se ha realizado para que el comportamiento sea coherente y correcto cuando se intenta la carga diferida de una instancia de `DbContext` eliminada.</span><span class="sxs-lookup"><span data-stu-id="2fa64-593">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="2fa64-594">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-594">**Mitigations**</span></span>

<span data-ttu-id="2fa64-595">Actualice el código de la aplicación para que no intente la carga diferida con un contexto eliminado, o bien configúrelo para que sea no operativo, como se describe en el mensaje de la excepción.</span><span class="sxs-lookup"><span data-stu-id="2fa64-595">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="2fa64-596">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="2fa64-596">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="2fa64-597">Problema de seguimiento n.º 10236</span><span class="sxs-lookup"><span data-stu-id="2fa64-597">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="2fa64-598">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-598">**Old behavior**</span></span>

<span data-ttu-id="2fa64-599">Antes de EF Core 3.0, se registraba una advertencia para una aplicación que creaba un número patológico de proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="2fa64-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="2fa64-600">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-600">**New behavior**</span></span>

<span data-ttu-id="2fa64-601">A partir de EF Core 3.0, ahora esta advertencia se considera un error y se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="2fa64-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="2fa64-602">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-602">**Why**</span></span>

<span data-ttu-id="2fa64-603">Este cambio se ha realizado para controlar mejor el código de la aplicación mediante la exposición de este caso patológico de una forma más explícita.</span><span class="sxs-lookup"><span data-stu-id="2fa64-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="2fa64-604">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-604">**Mitigations**</span></span>

<span data-ttu-id="2fa64-605">Cuando se produce este error, la acción más adecuada consiste en comprender la causa raíz y detener la creación de tantos proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="2fa64-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="2fa64-606">Pero el error se puede convertir en una advertencia (u omitirse) mediante configuración en `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="2fa64-607">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="2fa64-608">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="2fa64-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="2fa64-609">Problema de seguimiento n.° 9171</span><span class="sxs-lookup"><span data-stu-id="2fa64-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="2fa64-610">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-610">**Old behavior**</span></span>

<span data-ttu-id="2fa64-611">Antes de EF Core 3.0, el código para llamar a `HasOne` o `HasMany` con una cadena se interpretaba de manera confusa.</span><span class="sxs-lookup"><span data-stu-id="2fa64-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="2fa64-612">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="2fa64-613">El código parece relacionar `Samurai` con otro tipo de entidad mediante la propiedad de navegación `Entrance`, que puede ser privada.</span><span class="sxs-lookup"><span data-stu-id="2fa64-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="2fa64-614">En realidad, este código intenta crear una relación con algún tipo de entidad denominada `Entrance` sin ninguna propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="2fa64-615">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-615">**New behavior**</span></span>

<span data-ttu-id="2fa64-616">A partir de EF Core 3.0, el código anterior ahora hace lo que parecía que debía hacer antes.</span><span class="sxs-lookup"><span data-stu-id="2fa64-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="2fa64-617">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-617">**Why**</span></span>

<span data-ttu-id="2fa64-618">El comportamiento anterior era muy confuso, especialmente al leer el código de configuración y al buscar errores.</span><span class="sxs-lookup"><span data-stu-id="2fa64-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="2fa64-619">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-619">**Mitigations**</span></span>

<span data-ttu-id="2fa64-620">Esto solo interrumpirá las aplicaciones que configuran de manera explícita las relaciones con cadenas para nombres de tipos y sin especificar explícitamente la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="2fa64-621">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="2fa64-621">This is not common.</span></span>
<span data-ttu-id="2fa64-622">El comportamiento anterior se puede obtener al pasar de manera explícita `null` para el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="2fa64-623">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="2fa64-624">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="2fa64-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="2fa64-625">Problema de seguimiento n.º 15184</span><span class="sxs-lookup"><span data-stu-id="2fa64-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="2fa64-626">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-626">**Old behavior**</span></span>

<span data-ttu-id="2fa64-627">Antes, los siguientes métodos asincrónicos devolvían `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="2fa64-627">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="2fa64-628">`ValueGenerator.NextValueAsync()` (y las clases derivadas)</span><span class="sxs-lookup"><span data-stu-id="2fa64-628">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="2fa64-629">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-629">**New behavior**</span></span>

<span data-ttu-id="2fa64-630">Dichos métodos ahora devuelven `ValueTask<T>` durante el mismo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="2fa64-630">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="2fa64-631">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-631">**Why**</span></span>

<span data-ttu-id="2fa64-632">Este cambio reduce el número de asignaciones de montones que se producen al invocar estos métodos, lo que mejora el rendimiento general.</span><span class="sxs-lookup"><span data-stu-id="2fa64-632">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="2fa64-633">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-633">**Mitigations**</span></span>

<span data-ttu-id="2fa64-634">Las aplicaciones que simplemente esperen las API anteriores solo necesitan recompilarse, sin que sea necesario realizar cambios en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="2fa64-634">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="2fa64-635">Un uso más complejo (p. ej., pasar el valor `Task` devuelto a `Task.WhenAny()`) normalmente requiere que el valor `ValueTask<T>` devuelto se convierta en `Task<T>` mediante una llamada a `AsTask()` en él.</span><span class="sxs-lookup"><span data-stu-id="2fa64-635">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="2fa64-636">Tenga en cuenta que esto niega la reducción de asignación que implica este cambio.</span><span class="sxs-lookup"><span data-stu-id="2fa64-636">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="2fa64-637">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2fa64-637">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="2fa64-638">Problema de seguimiento n.º 9913</span><span class="sxs-lookup"><span data-stu-id="2fa64-638">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="2fa64-639">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-639">**Old behavior**</span></span>

<span data-ttu-id="2fa64-640">El nombre de anotación para las anotaciones de asignación de tipos era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="2fa64-640">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="2fa64-641">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-641">**New behavior**</span></span>

<span data-ttu-id="2fa64-642">Ahora, el nombre de anotación para las anotaciones de asignación de tipos es "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="2fa64-642">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="2fa64-643">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-643">**Why**</span></span>

<span data-ttu-id="2fa64-644">Ahora, las asignaciones de tipos se usan para algo más que solo para proveedores de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="2fa64-644">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="2fa64-645">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-645">**Mitigations**</span></span>

<span data-ttu-id="2fa64-646">Esto solo interrumpirá a las aplicaciones que acceden directamente a la asignación de tipos como una anotación, lo que no es habitual.</span><span class="sxs-lookup"><span data-stu-id="2fa64-646">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="2fa64-647">La acción más apropiada para corregir es usar la superficie de API para acceder a las asignaciones de tipos en lugar de usar directamente la anotación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-647">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="2fa64-648">ToTable en un tipo derivado inicia una excepción</span><span class="sxs-lookup"><span data-stu-id="2fa64-648">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="2fa64-649">Problema de seguimiento n.º 11811</span><span class="sxs-lookup"><span data-stu-id="2fa64-649">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="2fa64-650">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-650">**Old behavior**</span></span>

<span data-ttu-id="2fa64-651">Antes de EF Core 3.0, la llamada a `ToTable()` en un tipo derivado se omitía, ya que la única estrategia asignación de herencia era TPH, lo que no es válido.</span><span class="sxs-lookup"><span data-stu-id="2fa64-651">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="2fa64-652">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-652">**New behavior**</span></span>

<span data-ttu-id="2fa64-653">A partir de EF Core 3.0, y en preparación para agregar compatibilidad con TPT y TPC en una versión posterior, ahora la llamada a `ToTable()` en un tipo derivado iniciará una excepción para evitar un cambio de asignación inesperado en el futuro.</span><span class="sxs-lookup"><span data-stu-id="2fa64-653">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="2fa64-654">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-654">**Why**</span></span>

<span data-ttu-id="2fa64-655">En la actualidad no se considera válido asignar un tipo derivado a otra tabla.</span><span class="sxs-lookup"><span data-stu-id="2fa64-655">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="2fa64-656">Este cambio evita interrupciones en el futuro, cuando se convierta en una operación válida.</span><span class="sxs-lookup"><span data-stu-id="2fa64-656">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="2fa64-657">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-657">**Mitigations**</span></span>

<span data-ttu-id="2fa64-658">Quite todos los intentos de asignar tipos derivados a otras tablas.</span><span class="sxs-lookup"><span data-stu-id="2fa64-658">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="2fa64-659">ForSqlServerHasIndex se ha reemplazado por HasIndex</span><span class="sxs-lookup"><span data-stu-id="2fa64-659">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="2fa64-660">Problema de seguimiento n.º 12366</span><span class="sxs-lookup"><span data-stu-id="2fa64-660">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="2fa64-661">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-661">**Old behavior**</span></span>

<span data-ttu-id="2fa64-662">Antes de EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` proporcionaba una manera de configurar las columnas que se usaban con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-662">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="2fa64-663">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-663">**New behavior**</span></span>

<span data-ttu-id="2fa64-664">A partir de EF Core 3.0, ya se admite el uso de `Include` en un índice en el nivel relacional.</span><span class="sxs-lookup"><span data-stu-id="2fa64-664">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="2fa64-665">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-665">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="2fa64-666">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-666">**Why**</span></span>

<span data-ttu-id="2fa64-667">Este cambio se ha realizado para consolidar la API para índices con `Include` en un mismo lugar para todos los proveedores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="2fa64-667">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="2fa64-668">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-668">**Mitigations**</span></span>

<span data-ttu-id="2fa64-669">Use la API nueva, como se ha mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="2fa64-669">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="2fa64-670">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="2fa64-670">Metadata API changes</span></span>

[<span data-ttu-id="2fa64-671">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="2fa64-671">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2fa64-672">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-672">**New behavior**</span></span>

<span data-ttu-id="2fa64-673">Las siguientes propiedades se han convertido en métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="2fa64-673">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="2fa64-674">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-674">**Why**</span></span>

<span data-ttu-id="2fa64-675">Este cambio simplifica la implementación de las interfaces mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="2fa64-675">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="2fa64-676">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-676">**Mitigations**</span></span>

<span data-ttu-id="2fa64-677">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="2fa64-677">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="2fa64-678">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="2fa64-678">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="2fa64-679">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="2fa64-679">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2fa64-680">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-680">**New behavior**</span></span>

<span data-ttu-id="2fa64-681">Los métodos de extensión específicos del proveedor se simplificarán:</span><span class="sxs-lookup"><span data-stu-id="2fa64-681">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="2fa64-682">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-682">**Why**</span></span>

<span data-ttu-id="2fa64-683">Este cambio simplifica la implementación de los métodos de extensión mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="2fa64-683">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="2fa64-684">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-684">**Mitigations**</span></span>

<span data-ttu-id="2fa64-685">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="2fa64-685">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="2fa64-686">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="2fa64-686">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="2fa64-687">Problema de seguimiento n.º 12151</span><span class="sxs-lookup"><span data-stu-id="2fa64-687">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="2fa64-688">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-688">**Old behavior**</span></span>

<span data-ttu-id="2fa64-689">Antes de EF Core 3.0, EF Core enviaba `PRAGMA foreign_keys = 1` cuando se abría una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="2fa64-689">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2fa64-690">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-690">**New behavior**</span></span>

<span data-ttu-id="2fa64-691">A partir de EF Core 3.0, EF Core ya no envía `PRAGMA foreign_keys = 1` cuando se abre una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="2fa64-691">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2fa64-692">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-692">**Why**</span></span>

<span data-ttu-id="2fa64-693">Este cambio se ha realizado porque en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3` de forma predeterminada, lo que a su vez significa que el cumplimiento de CD está activado de forma predeterminada y no es necesario habilitarlo explícitamente cada vez que se abra una conexión.</span><span class="sxs-lookup"><span data-stu-id="2fa64-693">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="2fa64-694">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-694">**Mitigations**</span></span>

<span data-ttu-id="2fa64-695">Las claves externas se habilitan de forma predeterminada en SQLitePCLRaw.bundle_e_sqlite3, que en EF Core se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2fa64-695">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="2fa64-696">Para otros casos, las claves externas se pueden habilitar mediante la especificación de `Foreign Keys=True` en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="2fa64-696">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="2fa64-697">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="2fa64-697">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="2fa64-698">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-698">**Old behavior**</span></span>

<span data-ttu-id="2fa64-699">Antes de EF Core 3.0, en EF Core se usaba `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-699">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="2fa64-700">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-700">**New behavior**</span></span>

<span data-ttu-id="2fa64-701">A partir de EF Core 3.0, en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-701">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="2fa64-702">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-702">**Why**</span></span>

<span data-ttu-id="2fa64-703">Este cambio se ha realizado para que la versión de SQLite que se usa en iOS sea coherente con otras plataformas.</span><span class="sxs-lookup"><span data-stu-id="2fa64-703">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="2fa64-704">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-704">**Mitigations**</span></span>

<span data-ttu-id="2fa64-705">Para usar la versión nativa de SQLite en iOS, configure `Microsoft.Data.Sqlite` para usar otra agrupación `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-705">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2fa64-706">Almacenamiento de valores GUID como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="2fa64-706">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2fa64-707">Problema de seguimiento n.º 15078</span><span class="sxs-lookup"><span data-stu-id="2fa64-707">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="2fa64-708">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-708">**Old behavior**</span></span>

<span data-ttu-id="2fa64-709">Antes, los valores GUID se almacenaban como valores BLOB en SQLite.</span><span class="sxs-lookup"><span data-stu-id="2fa64-709">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="2fa64-710">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-710">**New behavior**</span></span>

<span data-ttu-id="2fa64-711">Ahora, los valores GUID se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="2fa64-711">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="2fa64-712">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-712">**Why**</span></span>

<span data-ttu-id="2fa64-713">El formato binario de los GUID no está normalizado.</span><span class="sxs-lookup"><span data-stu-id="2fa64-713">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="2fa64-714">El almacenamiento de los valores como TEXT mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="2fa64-714">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2fa64-715">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-715">**Mitigations**</span></span>

<span data-ttu-id="2fa64-716">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="2fa64-716">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="2fa64-717">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="2fa64-717">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="2fa64-718">Microsoft.Data.Sqlite sigue siendo capaz de leer valores GUID de ambas columnas BLOB y TEXT. Sin embargo, dado que el formato predeterminado de los parámetros y las constantes ha cambiado, seguramente deberá realizar alguna acción en la mayoría de casos que impliquen el uso de valores GUID.</span><span class="sxs-lookup"><span data-stu-id="2fa64-718">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2fa64-719">Ahora los valores char se almacenan como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="2fa64-719">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2fa64-720">Problema de seguimiento n.º 15020</span><span class="sxs-lookup"><span data-stu-id="2fa64-720">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="2fa64-721">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-721">**Old behavior**</span></span>

<span data-ttu-id="2fa64-722">Anteriormente los valores char se almacenaban como valores INTEGER en SQLite.</span><span class="sxs-lookup"><span data-stu-id="2fa64-722">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="2fa64-723">Por ejemplo, un valor char de *A* se almacenaba como el valor entero 65.</span><span class="sxs-lookup"><span data-stu-id="2fa64-723">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="2fa64-724">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-724">**New behavior**</span></span>

<span data-ttu-id="2fa64-725">Ahora, los valores char se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="2fa64-725">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="2fa64-726">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-726">**Why**</span></span>

<span data-ttu-id="2fa64-727">El almacenamiento de valores como TEXT es más natural y mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="2fa64-727">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2fa64-728">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-728">**Mitigations**</span></span>

<span data-ttu-id="2fa64-729">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="2fa64-729">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="2fa64-730">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="2fa64-730">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="2fa64-731">Microsoft.Data.Sqlite también puede leer valores de caracteres tanto de columnas INTEGER como de columnas TEXT, por lo que es posible que no deba hacer nada dependiendo de su caso.</span><span class="sxs-lookup"><span data-stu-id="2fa64-731">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="2fa64-732">Ahora los id. de migración se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="2fa64-732">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="2fa64-733">Problema de seguimiento n.º 12978</span><span class="sxs-lookup"><span data-stu-id="2fa64-733">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="2fa64-734">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-734">**Old behavior**</span></span>

<span data-ttu-id="2fa64-735">Los identificadores de migración se generaban de forma involuntaria con el calendario de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="2fa64-735">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="2fa64-736">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-736">**New behavior**</span></span>

<span data-ttu-id="2fa64-737">Ahora los id. de migración siempre se generan usando el calendario de la referencia cultural invariable (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="2fa64-737">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="2fa64-738">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-738">**Why**</span></span>

<span data-ttu-id="2fa64-739">El orden de las migraciones es importante al actualizar la base de datos o al solucionar conflictos de combinación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-739">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="2fa64-740">Al usar el calendario invariable, se evitan problemas de ordenación que pueden producirse si los miembros del equipo tienen distintos calendarios del sistema.</span><span class="sxs-lookup"><span data-stu-id="2fa64-740">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="2fa64-741">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-741">**Mitigations**</span></span>

<span data-ttu-id="2fa64-742">Esta cambio afecta a todas las personas que usan un calendario no gregoriano en el que el año sea superior al del calendario gregoriano (como el calendario budista tailandés).</span><span class="sxs-lookup"><span data-stu-id="2fa64-742">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="2fa64-743">Los id. de migración existentes deberán actualizarse para que las migraciones nuevas se ordenen después de las existentes.</span><span class="sxs-lookup"><span data-stu-id="2fa64-743">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="2fa64-744">Puede ver el id. de migración en el atributo Migration de los archivos de diseñador de la migración.</span><span class="sxs-lookup"><span data-stu-id="2fa64-744">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="2fa64-745">También debe actualizarse la tabla de historial de migraciones.</span><span class="sxs-lookup"><span data-stu-id="2fa64-745">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="2fa64-746">Se ha quitado el elemento UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="2fa64-746">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="2fa64-747">Problema de seguimiento n.º 16400</span><span class="sxs-lookup"><span data-stu-id="2fa64-747">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="2fa64-748">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-748">**Old behavior**</span></span>

<span data-ttu-id="2fa64-749">Antes de EF Core 3.0, `UseRowNumberForPaging` se podía usar para generar SQL para la paginación de forma que fuera compatible con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="2fa64-749">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="2fa64-750">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-750">**New behavior**</span></span>

<span data-ttu-id="2fa64-751">A partir de EF Core 3.0, EF solo genera SQL para la paginación que únicamente es compatible con las versiones posteriores de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2fa64-751">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="2fa64-752">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-752">**Why**</span></span>

<span data-ttu-id="2fa64-753">El motivo de este cambio es que [SQL Server 2008 ya no se admite](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/). Además, la actualización de esta característica para que funcionase con los cambios en las consultas implementados en EF Core 3.0 llevaría mucho trabajo.</span><span class="sxs-lookup"><span data-stu-id="2fa64-753">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="2fa64-754">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-754">**Mitigations**</span></span>

<span data-ttu-id="2fa64-755">Se recomienda actualizar a una versión más reciente de SQL Server, o bien utilizar un nivel de compatibilidad superior, de modo que el SQL que se genere se admita.</span><span class="sxs-lookup"><span data-stu-id="2fa64-755">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="2fa64-756">Dicho esto, si no puede hacerlo, [escriba un comentario en el problema de seguimiento](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con los detalles al respecto.</span><span class="sxs-lookup"><span data-stu-id="2fa64-756">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="2fa64-757">En función de los comentarios, es posible que volvamos a valorar esta decisión.</span><span class="sxs-lookup"><span data-stu-id="2fa64-757">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="2fa64-758">La información o metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="2fa64-758">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="2fa64-759">Problema de seguimiento n.º 16119</span><span class="sxs-lookup"><span data-stu-id="2fa64-759">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="2fa64-760">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-760">**Old behavior**</span></span>

<span data-ttu-id="2fa64-761">`IDbContextOptionsExtension` incluía métodos para proporcionar metadatos sobre la extensión.</span><span class="sxs-lookup"><span data-stu-id="2fa64-761">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="2fa64-762">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-762">**New behavior**</span></span>

<span data-ttu-id="2fa64-763">Estos métodos se han movido a una nueva clase base abstracta `DbContextOptionsExtensionInfo`, que se devuelve desde una nueva propiedad `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-763">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="2fa64-764">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-764">**Why**</span></span>

<span data-ttu-id="2fa64-765">Al lanzarse las versiones 2.0 y 3.0, tuvimos que agregar o cambiar estos métodos varias veces.</span><span class="sxs-lookup"><span data-stu-id="2fa64-765">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="2fa64-766">Su división en una nueva clase base abstracta facilitará la realización de este tipo de cambios sin interrumpir las extensiones existentes.</span><span class="sxs-lookup"><span data-stu-id="2fa64-766">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="2fa64-767">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-767">**Mitigations**</span></span>

<span data-ttu-id="2fa64-768">Actualice las extensiones para seguir el nuevo patrón.</span><span class="sxs-lookup"><span data-stu-id="2fa64-768">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="2fa64-769">Encontrará ejemplos en las muchas implementaciones de `IDbContextOptionsExtension` para los diferentes tipos de extensiones en el código fuente de EF Core.</span><span class="sxs-lookup"><span data-stu-id="2fa64-769">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="2fa64-770">Cambio de nombre de LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="2fa64-770">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="2fa64-771">Problema de seguimiento n.º 10985</span><span class="sxs-lookup"><span data-stu-id="2fa64-771">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="2fa64-772">**Cambio**</span><span class="sxs-lookup"><span data-stu-id="2fa64-772">**Change**</span></span>

<span data-ttu-id="2fa64-773">Se ha cambiado el nombre de `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="2fa64-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="2fa64-774">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-774">**Why**</span></span>

<span data-ttu-id="2fa64-775">Conviene alinear el nombre de este evento de advertencia con el del resto de eventos de advertencia.</span><span class="sxs-lookup"><span data-stu-id="2fa64-775">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="2fa64-776">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-776">**Mitigations**</span></span>

<span data-ttu-id="2fa64-777">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="2fa64-777">Use the new name.</span></span> <span data-ttu-id="2fa64-778">(Tenga en cuenta que el número de id. evento sigue siendo el mismo).</span><span class="sxs-lookup"><span data-stu-id="2fa64-778">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="2fa64-779">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="2fa64-779">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="2fa64-780">Problema de seguimiento n.º 10730</span><span class="sxs-lookup"><span data-stu-id="2fa64-780">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="2fa64-781">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-781">**Old behavior**</span></span>

<span data-ttu-id="2fa64-782">Antes de EF Core 3.0, se utilizaba simplemente el término "nombre" para hacer referencia a los nombres de las restricciones de claves externas.</span><span class="sxs-lookup"><span data-stu-id="2fa64-782">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="2fa64-783">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-783">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="2fa64-784">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-784">**New behavior**</span></span>

<span data-ttu-id="2fa64-785">A partir de EF Core 3.0, el término con el que se hace referencia a los nombres de las restricciones de claves externas es "nombre de la restricción".</span><span class="sxs-lookup"><span data-stu-id="2fa64-785">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="2fa64-786">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-786">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="2fa64-787">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-787">**Why**</span></span>

<span data-ttu-id="2fa64-788">Este cambio permite mejorar la coherencia relativa a la nomenclatura en este aspecto y aclarar que se trata del nombre de una restricción de clave externa, y no del de la columna o propiedad en la que está definida la clave externa.</span><span class="sxs-lookup"><span data-stu-id="2fa64-788">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="2fa64-789">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-789">**Mitigations**</span></span>

<span data-ttu-id="2fa64-790">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="2fa64-790">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="2fa64-791">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="2fa64-791">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="2fa64-792">Problema de seguimiento n.° 15997</span><span class="sxs-lookup"><span data-stu-id="2fa64-792">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="2fa64-793">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-793">**Old behavior**</span></span>

<span data-ttu-id="2fa64-794">Antes de EF Core 3.0, estos métodos estaban protegidos.</span><span class="sxs-lookup"><span data-stu-id="2fa64-794">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="2fa64-795">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-795">**New behavior**</span></span>

<span data-ttu-id="2fa64-796">Desde EF Core 3.0, estos métodos son públicos.</span><span class="sxs-lookup"><span data-stu-id="2fa64-796">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="2fa64-797">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-797">**Why**</span></span>

<span data-ttu-id="2fa64-798">EF usa estos métodos para determinar si se ha creado una base de datos, pero está vacía.</span><span class="sxs-lookup"><span data-stu-id="2fa64-798">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="2fa64-799">Esto también puede resultar útil fuera de EF al determinar si se deben aplicar migraciones o no.</span><span class="sxs-lookup"><span data-stu-id="2fa64-799">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="2fa64-800">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-800">**Mitigations**</span></span>

<span data-ttu-id="2fa64-801">Cambie la accesibilidad de cualquier invalidación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-801">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="2fa64-802">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="2fa64-802">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="2fa64-803">Problema de seguimiento n.° 11506</span><span class="sxs-lookup"><span data-stu-id="2fa64-803">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="2fa64-804">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-804">**Old behavior**</span></span>

<span data-ttu-id="2fa64-805">Antes de EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un paquete NuGet regular con un ensamblado al que podían hacer referencia los proyectos que dependían de él.</span><span class="sxs-lookup"><span data-stu-id="2fa64-805">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="2fa64-806">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-806">**New behavior**</span></span>

<span data-ttu-id="2fa64-807">Desde EF Core 3.0, es un paquete DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="2fa64-807">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="2fa64-808">Esto significa que la dependencia no fluirá de manera transitiva en otros proyectos y que ya no puede, de forma predeterminada, hacer referencia a su ensamblado.</span><span class="sxs-lookup"><span data-stu-id="2fa64-808">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="2fa64-809">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-809">**Why**</span></span>

<span data-ttu-id="2fa64-810">Este paquete solo está destinado a usarse en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="2fa64-810">This package is only intended to be used at design time.</span></span> <span data-ttu-id="2fa64-811">Las aplicaciones implementadas no deben hacer referencia al mismo.</span><span class="sxs-lookup"><span data-stu-id="2fa64-811">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="2fa64-812">Hacer que el paquete sea DevelopmentDependency refuerza esta recomendación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-812">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="2fa64-813">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-813">**Mitigations**</span></span>

<span data-ttu-id="2fa64-814">Si tiene que hacer referencia a este paquete para invalidar el comportamiento en tiempo de diseño de EF Core, puede actualizar los metadatos de elementos PackageReference del proyecto.</span><span class="sxs-lookup"><span data-stu-id="2fa64-814">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="2fa64-815">Si se hace referencia al paquete de manera transitiva a través de Microsoft.EntityFrameworkCore.Tools, tendrá que agregar una PackageReference explícita al paquete para cambiar sus metadatos.</span><span class="sxs-lookup"><span data-stu-id="2fa64-815">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="2fa64-816">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2fa64-816">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="2fa64-817">Problema de seguimiento n.° 14824</span><span class="sxs-lookup"><span data-stu-id="2fa64-817">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="2fa64-818">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-818">**Old behavior**</span></span>

<span data-ttu-id="2fa64-819">Microsoft.EntityFrameworkCore.Sqlite dependía anteriormente de la versión 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="2fa64-819">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="2fa64-820">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-820">**New behavior**</span></span>

<span data-ttu-id="2fa64-821">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2fa64-821">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2fa64-822">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-822">**Why**</span></span>

<span data-ttu-id="2fa64-823">La versión 2.0.0 de SQLitePCL.raw selecciona .NET Standard 2.0 como destino.</span><span class="sxs-lookup"><span data-stu-id="2fa64-823">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="2fa64-824">Anteriormente seleccionaba .NET Standard 1.1 como destino, que requería el cierre a gran escala de paquetes transitivos para su funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="2fa64-824">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="2fa64-825">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-825">**Mitigations**</span></span>

<span data-ttu-id="2fa64-826">En la versión 2.0.0 de SQLitePCL.raw se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="2fa64-826">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2fa64-827">Consulte las [notas de la versión](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="2fa64-827">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="2fa64-828">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2fa64-828">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="2fa64-829">Problema de seguimiento n.° 14825</span><span class="sxs-lookup"><span data-stu-id="2fa64-829">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="2fa64-830">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-830">**Old behavior**</span></span>

<span data-ttu-id="2fa64-831">Los paquetes espaciales anteriormente dependían de la versión 1.15.1 de NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="2fa64-831">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="2fa64-832">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-832">**New behavior**</span></span>

<span data-ttu-id="2fa64-833">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2fa64-833">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2fa64-834">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-834">**Why**</span></span>

<span data-ttu-id="2fa64-835">La versión 2.0.0 de NetTopologySuite pretende resolver varios problemas de usabilidad que encontraron los usuarios de EF Core.</span><span class="sxs-lookup"><span data-stu-id="2fa64-835">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="2fa64-836">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-836">**Mitigations**</span></span>

<span data-ttu-id="2fa64-837">En la versión 2.0.0 de NetTopologySuite se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="2fa64-837">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2fa64-838">Consulte las [notas de la versión](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="2fa64-838">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="2fa64-839">Se deben configurar varias relaciones de referencia automática ambiguas</span><span class="sxs-lookup"><span data-stu-id="2fa64-839">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="2fa64-840">Problema de seguimiento n.º 13573</span><span class="sxs-lookup"><span data-stu-id="2fa64-840">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="2fa64-841">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-841">**Old behavior**</span></span>

<span data-ttu-id="2fa64-842">Un tipo de entidad con varias propiedades de navegación unidireccional de referencia automática y claves externas coincidentes se configuró incorrectamente como una única relación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-842">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="2fa64-843">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-843">For example:</span></span>

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

<span data-ttu-id="2fa64-844">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-844">**New behavior**</span></span>

<span data-ttu-id="2fa64-845">Este escenario se detecta ahora en la generación del modelo y se produce una excepción que indica que el modelo es ambiguo.</span><span class="sxs-lookup"><span data-stu-id="2fa64-845">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="2fa64-846">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-846">**Why**</span></span>

<span data-ttu-id="2fa64-847">El modelo resultante era ambiguo, y lo más probable es que sea incorrecto en este caso.</span><span class="sxs-lookup"><span data-stu-id="2fa64-847">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="2fa64-848">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-848">**Mitigations**</span></span>

<span data-ttu-id="2fa64-849">Utilice la configuración completa de la relación.</span><span class="sxs-lookup"><span data-stu-id="2fa64-849">Use full configuration of the relationship.</span></span> <span data-ttu-id="2fa64-850">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2fa64-850">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="2fa64-851">DbFunction.Schema es NULL o la cadena vacía lo configura para estar en el esquema predeterminado del modelo</span><span class="sxs-lookup"><span data-stu-id="2fa64-851">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="2fa64-852">Problema de seguimiento n.º 12757</span><span class="sxs-lookup"><span data-stu-id="2fa64-852">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="2fa64-853">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="2fa64-853">**Old behavior**</span></span>

<span data-ttu-id="2fa64-854">Una función DbFunction configurada con el esquema como una cadena vacía se trataba como una función integrada sin un esquema.</span><span class="sxs-lookup"><span data-stu-id="2fa64-854">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="2fa64-855">Por ejemplo, el código siguiente asignará la función CLR `DatePart` a la función integrada `DATEPART` en SqlServer.</span><span class="sxs-lookup"><span data-stu-id="2fa64-855">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="2fa64-856">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="2fa64-856">**New behavior**</span></span>

<span data-ttu-id="2fa64-857">Todas las asignaciones de DbFunction se consideran asignadas a funciones definidas por el usuario.</span><span class="sxs-lookup"><span data-stu-id="2fa64-857">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="2fa64-858">Por lo tanto, el valor de cadena vacía colocaría la función dentro del esquema predeterminado del modelo,</span><span class="sxs-lookup"><span data-stu-id="2fa64-858">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="2fa64-859">que podría ser el esquema configurado de forma explícita mediante `modelBuilder.HasDefaultSchema()` de la API fluida o `dbo` en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="2fa64-859">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="2fa64-860">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="2fa64-860">**Why**</span></span>

<span data-ttu-id="2fa64-861">Anteriormente, el esquema vacío era una manera de indicar que la función estaba integrada, pero esa lógica solo es aplicable a SqlServer, donde las funciones integradas no pertenecen a ningún esquema.</span><span class="sxs-lookup"><span data-stu-id="2fa64-861">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="2fa64-862">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="2fa64-862">**Mitigations**</span></span>

<span data-ttu-id="2fa64-863">Configure la traslación de DbFunction manualmente para asignarla a una función integrada.</span><span class="sxs-lookup"><span data-stu-id="2fa64-863">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
