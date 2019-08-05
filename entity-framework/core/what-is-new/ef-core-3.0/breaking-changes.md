---
title: 'Cambios importantes en EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: c73663412efcd93c04892f193d4f5a2485724e22
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497530"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="226ce-102">Cambios importantes incluidos en EF Core 3.0 (actualmente en versión preliminar)</span><span class="sxs-lookup"><span data-stu-id="226ce-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="226ce-103">Tenga en cuenta que los conjuntos de características y las programaciones de las versiones futuras siempre están sujetos a cambios y, aunque intentamos mantener esta página actualizada, es posible que no refleje nuestros planes más recientes en todo momento.</span><span class="sxs-lookup"><span data-stu-id="226ce-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="226ce-104">Es posible que los siguientes cambios de API y comportamiento interrumpan las aplicaciones desarrolladas para EF Core 2.2. x cuando se actualicen a la versión 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="226ce-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="226ce-105">Los cambios que esperamos que solo afecten a proveedores de base de datos se documentan en [Cambios para proveedores](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="226ce-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="226ce-106">Aquí no se documentan los cambios en las características nuevas presentadas de una versión preliminar 3.0 a otra.</span><span class="sxs-lookup"><span data-stu-id="226ce-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="226ce-107">Resumen</span><span class="sxs-lookup"><span data-stu-id="226ce-107">Summary</span></span>

| <span data-ttu-id="226ce-108">**Cambio importante**</span><span class="sxs-lookup"><span data-stu-id="226ce-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="226ce-109">**Impacto**</span><span class="sxs-lookup"><span data-stu-id="226ce-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="226ce-110">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="226ce-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="226ce-111">Alto</span><span class="sxs-lookup"><span data-stu-id="226ce-111">High</span></span>       |
| [<span data-ttu-id="226ce-112">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="226ce-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="226ce-113">Alto</span><span class="sxs-lookup"><span data-stu-id="226ce-113">High</span></span>      |
| [<span data-ttu-id="226ce-114">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="226ce-114">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="226ce-115">Alto</span><span class="sxs-lookup"><span data-stu-id="226ce-115">High</span></span>      |
| [<span data-ttu-id="226ce-116">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="226ce-116">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="226ce-117">Alto</span><span class="sxs-lookup"><span data-stu-id="226ce-117">High</span></span>      |
| [<span data-ttu-id="226ce-118">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="226ce-118">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="226ce-119">Medium</span><span class="sxs-lookup"><span data-stu-id="226ce-119">Medium</span></span>      |
| [<span data-ttu-id="226ce-120">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="226ce-120">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="226ce-121">Medium</span><span class="sxs-lookup"><span data-stu-id="226ce-121">Medium</span></span>      |
| [<span data-ttu-id="226ce-122">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="226ce-122">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="226ce-123">Medium</span><span class="sxs-lookup"><span data-stu-id="226ce-123">Medium</span></span>      |
| [<span data-ttu-id="226ce-124">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="226ce-124">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="226ce-125">Medium</span><span class="sxs-lookup"><span data-stu-id="226ce-125">Medium</span></span>      |
| [<span data-ttu-id="226ce-126">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="226ce-126">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="226ce-127">Medium</span><span class="sxs-lookup"><span data-stu-id="226ce-127">Medium</span></span>      |
| [<span data-ttu-id="226ce-128">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="226ce-128">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="226ce-129">Medium</span><span class="sxs-lookup"><span data-stu-id="226ce-129">Medium</span></span>      |
| [<span data-ttu-id="226ce-130">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="226ce-130">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="226ce-131">Medium</span><span class="sxs-lookup"><span data-stu-id="226ce-131">Medium</span></span>      |
| [<span data-ttu-id="226ce-132">Se ha quitado el elemento UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="226ce-132">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="226ce-133">Medium</span><span class="sxs-lookup"><span data-stu-id="226ce-133">Medium</span></span>      |
| [<span data-ttu-id="226ce-134">Solo se pueden especificar métodos de FromSql en raíces de consulta</span><span class="sxs-lookup"><span data-stu-id="226ce-134">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="226ce-135">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-135">Low</span></span>      |
| [<span data-ttu-id="226ce-136">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="226ce-136">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="226ce-137">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-137">Low</span></span>      |
| [<span data-ttu-id="226ce-138">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="226ce-138">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="226ce-139">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-139">Low</span></span>      |
| [<span data-ttu-id="226ce-140">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="226ce-140">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="226ce-141">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-141">Low</span></span>      |
| [<span data-ttu-id="226ce-142">Las entidades dependientes que comparten la tabla con la entidad de seguridad son ahora opcionales</span><span class="sxs-lookup"><span data-stu-id="226ce-142">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="226ce-143">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-143">Low</span></span>      |
| [<span data-ttu-id="226ce-144">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="226ce-144">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="226ce-145">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-145">Low</span></span>      |
| [<span data-ttu-id="226ce-146">Las propiedades heredadas de tipos sin asignar se asignan ahora a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="226ce-146">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="226ce-147">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-147">Low</span></span>      |
| [<span data-ttu-id="226ce-148">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="226ce-148">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="226ce-149">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-149">Low</span></span>      |
| [<span data-ttu-id="226ce-150">La conexión de base de datos ahora se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="226ce-150">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="226ce-151">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-151">Low</span></span>      |
| [<span data-ttu-id="226ce-152">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="226ce-152">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="226ce-153">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-153">Low</span></span>      |
| [<span data-ttu-id="226ce-154">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="226ce-154">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="226ce-155">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-155">Low</span></span>      |
| [<span data-ttu-id="226ce-156">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="226ce-156">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="226ce-157">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-157">Low</span></span>      |
| [<span data-ttu-id="226ce-158">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="226ce-158">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="226ce-159">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-159">Low</span></span>      |
| [<span data-ttu-id="226ce-160">DbContext.Entry realiza ahora una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="226ce-160">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="226ce-161">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-161">Low</span></span>      |
| [<span data-ttu-id="226ce-162">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="226ce-162">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="226ce-163">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-163">Low</span></span>      |
| [<span data-ttu-id="226ce-164">ILoggerFactory es ahora un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="226ce-164">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="226ce-165">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-165">Low</span></span>      |
| [<span data-ttu-id="226ce-166">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="226ce-166">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="226ce-167">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-167">Low</span></span>      |
| [<span data-ttu-id="226ce-168">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="226ce-168">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="226ce-169">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-169">Low</span></span>      |
| [<span data-ttu-id="226ce-170">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="226ce-170">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="226ce-171">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-171">Low</span></span>      |
| [<span data-ttu-id="226ce-172">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="226ce-172">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="226ce-173">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-173">Low</span></span>      |
| [<span data-ttu-id="226ce-174">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="226ce-174">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="226ce-175">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-175">Low</span></span>      |
| [<span data-ttu-id="226ce-176">ToTable en un tipo derivado produce una excepción</span><span class="sxs-lookup"><span data-stu-id="226ce-176">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="226ce-177">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-177">Low</span></span>      |
| [<span data-ttu-id="226ce-178">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="226ce-178">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="226ce-179">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-179">Low</span></span>      |
| [<span data-ttu-id="226ce-180">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="226ce-180">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="226ce-181">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-181">Low</span></span>      |
| [<span data-ttu-id="226ce-182">Los valores GUID se almacenan ahora como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="226ce-182">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="226ce-183">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-183">Low</span></span>      |
| [<span data-ttu-id="226ce-184">Los valores char se almacenan ahora como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="226ce-184">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="226ce-185">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-185">Low</span></span>      |
| [<span data-ttu-id="226ce-186">Los id. de migración ahora se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="226ce-186">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="226ce-187">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-187">Low</span></span>      |
| [<span data-ttu-id="226ce-188">La información o los metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="226ce-188">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="226ce-189">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-189">Low</span></span>      |
| [<span data-ttu-id="226ce-190">LogQueryPossibleExceptionWithAggregateOperator ha cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="226ce-190">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="226ce-191">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-191">Low</span></span>      |
| [<span data-ttu-id="226ce-192">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="226ce-192">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="226ce-193">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-193">Low</span></span>      |
| [<span data-ttu-id="226ce-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="226ce-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="226ce-195">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-195">Low</span></span>      |
| [<span data-ttu-id="226ce-196">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="226ce-196">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="226ce-197">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-197">Low</span></span>      |
| [<span data-ttu-id="226ce-198">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="226ce-198">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="226ce-199">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-199">Low</span></span>      |
| [<span data-ttu-id="226ce-200">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="226ce-200">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="226ce-201">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-201">Low</span></span>      |
| [<span data-ttu-id="226ce-202">Se deben configurar varias relaciones de referencia automática ambiguas</span><span class="sxs-lookup"><span data-stu-id="226ce-202">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="226ce-203">Bajo</span><span class="sxs-lookup"><span data-stu-id="226ce-203">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="226ce-204">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="226ce-204">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="226ce-205">[Problema de seguimiento n.° 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Consulte también el problema n.° 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="226ce-205">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="226ce-206">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-206">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-207">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-207">**Old behavior**</span></span>

<span data-ttu-id="226ce-208">Antes de 3.0, cuando en EF Core no se podía convertir una expresión que formaba parte de una consulta SQL o un parámetro, la expresión se evaluaba de forma automática en el cliente.</span><span class="sxs-lookup"><span data-stu-id="226ce-208">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="226ce-209">De forma predeterminada, la evaluación de cliente de expresiones potencialmente costosas solo desencadenaba una advertencia.</span><span class="sxs-lookup"><span data-stu-id="226ce-209">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="226ce-210">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-210">**New behavior**</span></span>

<span data-ttu-id="226ce-211">A partir de 3.0, en EF Core solo se permite que se evalúen en el cliente las expresiones en la proyección de nivel superior (la última llamada a `Select()` de la consulta).</span><span class="sxs-lookup"><span data-stu-id="226ce-211">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="226ce-212">Cuando las expresiones de otra parte de la consulta no se pueden convertir en SQL o un parámetro, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="226ce-212">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="226ce-213">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-213">**Why**</span></span>

<span data-ttu-id="226ce-214">La evaluación de cliente automática de las consultas permite que se ejecuten muchas consultas incluso si no se pueden convertir elementos importantes de ellas.</span><span class="sxs-lookup"><span data-stu-id="226ce-214">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="226ce-215">Esto puede provocar un comportamiento inesperado y potencialmente dañino que es posible que solo sea evidente en entornos de producción.</span><span class="sxs-lookup"><span data-stu-id="226ce-215">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="226ce-216">Por ejemplo, una condición en una llamada a `Where()` que no se puede convertir puede provocar que todas las filas de la tabla se transfieran desde el servidor de base de datos y que el filtro se aplique en el cliente.</span><span class="sxs-lookup"><span data-stu-id="226ce-216">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="226ce-217">Esta situación puede pasar desapercibida fácilmente si la tabla solo contiene algunas filas en la fase de desarrollo, pero ser más grave cuando la aplicación pase a producción, donde la tabla puede contener millones de filas.</span><span class="sxs-lookup"><span data-stu-id="226ce-217">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="226ce-218">Las advertencias de evaluación de cliente también se suelen pasar por alto durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="226ce-218">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="226ce-219">Además de esto, la evaluación de cliente automática puede causar problemas en los que la mejora de la traducción de consultas para expresiones específicas provocaba cambios importantes no deseados entre versiones.</span><span class="sxs-lookup"><span data-stu-id="226ce-219">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="226ce-220">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-220">**Mitigations**</span></span>

<span data-ttu-id="226ce-221">Si una consulta no se puede traducir totalmente, vuelva a escribirla en un formato que se pueda traducir, o bien use `AsEnumerable()`, `ToList()` o una función similar para devolver los datos al cliente de forma explícita, donde después se puedan seguir procesando mediante LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="226ce-221">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="226ce-222">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="226ce-222">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="226ce-223">Anuncios del problema de seguimiento n.º 325</span><span class="sxs-lookup"><span data-stu-id="226ce-223">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="226ce-224">Este cambio se ha introducido en ASP.NET Core 3.0 (versión preliminar 1).</span><span class="sxs-lookup"><span data-stu-id="226ce-224">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="226ce-225">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-225">**Old behavior**</span></span>

<span data-ttu-id="226ce-226">Antes de ASP.NET Core 3.0, cuando se agregaba una referencia de paquete a `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, se incluía EF Core y algunos de los proveedores de datos de EF Core, como el de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="226ce-226">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="226ce-227">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-227">**New behavior**</span></span>

<span data-ttu-id="226ce-228">A partir de la versión 3.0, el marco compartido ASP.NET Core no incluye EF Core ni ningún proveedor de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="226ce-228">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="226ce-229">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-229">**Why**</span></span>

<span data-ttu-id="226ce-230">Antes de este cambio, para obtener EF Core se necesitaban varios pasos en función de si la aplicación se destinaba a ASP.NET Core y SQL Server o no.</span><span class="sxs-lookup"><span data-stu-id="226ce-230">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="226ce-231">Además, la actualización de ASP.NET Core forzaba la de EF Core y el proveedor de SQL Server, lo que no siempre es deseable.</span><span class="sxs-lookup"><span data-stu-id="226ce-231">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="226ce-232">Con este cambio, la experiencia de obtención de EF Core es la misma en todos los proveedores, implementaciones admitidas de .NET y tipos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="226ce-232">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="226ce-233">Ahora los desarrolladores también pueden controlar exactamente cuándo se actualizan EF Core y los proveedores de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="226ce-233">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="226ce-234">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-234">**Mitigations**</span></span>

<span data-ttu-id="226ce-235">Para usar EF Core en una aplicación ASP.NET Core 3.0 o cualquier otra aplicación compatible, debe agregar de forma explícita una referencia de paquete al proveedor de base de datos de EF Core que se va a usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="226ce-235">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="226ce-236">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="226ce-236">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="226ce-237">Problema de seguimiento n.º 14016</span><span class="sxs-lookup"><span data-stu-id="226ce-237">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="226ce-238">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4) y la versión correspondiente del SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="226ce-238">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="226ce-239">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-239">**Old behavior**</span></span>

<span data-ttu-id="226ce-240">Antes de 3.0, la herramienta `dotnet ef` se incluía en el SDK de .NET Core y estaba disponible para usarse desde la línea de comandos de cualquier proyecto sin necesidad de realizar pasos adicionales.</span><span class="sxs-lookup"><span data-stu-id="226ce-240">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="226ce-241">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-241">**New behavior**</span></span>

<span data-ttu-id="226ce-242">A partir de la versión 3.0, el SDK de .NET no incluye la herramienta `dotnet ef`, por lo que antes de poder usarla tendrá que instalarla de forma explícita como una herramienta local o global.</span><span class="sxs-lookup"><span data-stu-id="226ce-242">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="226ce-243">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-243">**Why**</span></span>

<span data-ttu-id="226ce-244">Este cambio nos permite distribuir y actualizar `dotnet ef` como una herramienta convencional de la CLI de .NET en NuGet, coherente con el hecho de que la versión 3.0 de EF Core también se distribuye siempre como un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="226ce-244">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="226ce-245">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-245">**Mitigations**</span></span>

<span data-ttu-id="226ce-246">Para poder administrar las migraciones o aplicar scaffolding a `DbContext`, instale `dotnet-ef` como herramienta global:</span><span class="sxs-lookup"><span data-stu-id="226ce-246">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="226ce-247">También se puede obtener una herramienta local cuando se restauran las dependencias de un proyecto que la declara como una dependencia de herramientas mediante un [archivo de manifiesto de herramientas](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="226ce-247">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="226ce-248">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="226ce-248">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="226ce-249">Problema de seguimiento n.º 10996</span><span class="sxs-lookup"><span data-stu-id="226ce-249">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="226ce-250">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-250">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-251">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-251">**Old behavior**</span></span>

<span data-ttu-id="226ce-252">Antes de EF Core 3.0, estos nombres de métodos se sobrecargaban para funcionar tanto con una cadena normal como con una cadena que se debería interpolar en SQL y parámetros.</span><span class="sxs-lookup"><span data-stu-id="226ce-252">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="226ce-253">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-253">**New behavior**</span></span>

<span data-ttu-id="226ce-254">A partir de la versión EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw` y `ExecuteSqlRawAsync` para crear una consulta con parámetros donde los parámetros se pasan por separado de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="226ce-254">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="226ce-255">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-255">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="226ce-256">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated` y `ExecuteSqlInterpolatedAsync` para crear una consulta con parámetros donde los parámetros se pasan como parte de una cadena de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="226ce-256">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="226ce-257">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-257">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="226ce-258">Tenga en cuenta que las dos consultas anteriores producirán el mismo código SQL parametrizado con los mismos parámetros SQL.</span><span class="sxs-lookup"><span data-stu-id="226ce-258">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="226ce-259">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-259">**Why**</span></span>

<span data-ttu-id="226ce-260">Las sobrecargas del método como esta facilitan las llamadas accidentales al método de cadena sin procesar cuando la intención era llamar al método de cadena interpolada y viceversa.</span><span class="sxs-lookup"><span data-stu-id="226ce-260">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="226ce-261">Esto podría resultar en consultas que no se parametrizan cuando deberían.</span><span class="sxs-lookup"><span data-stu-id="226ce-261">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="226ce-262">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-262">**Mitigations**</span></span>

<span data-ttu-id="226ce-263">Haga el cambio para usar los nuevos nombres de métodos.</span><span class="sxs-lookup"><span data-stu-id="226ce-263">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="226ce-264">Solo se pueden especificar métodos de FromSql en raíces de consulta.</span><span class="sxs-lookup"><span data-stu-id="226ce-264">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="226ce-265">Problema de seguimiento n.° 15704</span><span class="sxs-lookup"><span data-stu-id="226ce-265">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="226ce-266">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="226ce-266">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="226ce-267">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-267">**Old behavior**</span></span>

<span data-ttu-id="226ce-268">Antes de EF Core 3.0, el método `FromSql` podía especificarse en cualquier lugar en la consulta.</span><span class="sxs-lookup"><span data-stu-id="226ce-268">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="226ce-269">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-269">**New behavior**</span></span>

<span data-ttu-id="226ce-270">A partir de EF Core 3.0, los nuevos métodos `FromSqlRaw` y `FromSqlInterpolated` (que reemplazan a `FromSql`) solo pueden especificarse en las raíces de la consulta, es decir, directamente en `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="226ce-270">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="226ce-271">Si intenta especificarlos en cualquier otro lugar se producirá un error de compilación.</span><span class="sxs-lookup"><span data-stu-id="226ce-271">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="226ce-272">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-272">**Why**</span></span>

<span data-ttu-id="226ce-273">La especificación de `FromSql` en cualquier otro lugar diferente de `DbSet` no tenía un significado o valor agregado, y podría causar ambigüedad en ciertos escenarios.</span><span class="sxs-lookup"><span data-stu-id="226ce-273">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="226ce-274">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-274">**Mitigations**</span></span>

<span data-ttu-id="226ce-275">Las invocaciones de `FromSql` se deben mover para que estén directamente en el `DbSet` al que se aplican.</span><span class="sxs-lookup"><span data-stu-id="226ce-275">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="226ce-276">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="226ce-276">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="226ce-277">Problema de seguimiento n.º 14523</span><span class="sxs-lookup"><span data-stu-id="226ce-277">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="226ce-278">Este cambio se revirtió en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="226ce-278">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="226ce-279">Revertimos este cambio porque la nueva configuración de EF Core 3.0 permite a la aplicación especificar el nivel de registro para cualquier evento.</span><span class="sxs-lookup"><span data-stu-id="226ce-279">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="226ce-280">Por ejemplo, para cambiar el registro de SQL a `Debug`, configure el nivel de forma explícita en `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="226ce-280">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="226ce-281">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="226ce-281">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="226ce-282">Problema de seguimiento n.º 12378</span><span class="sxs-lookup"><span data-stu-id="226ce-282">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="226ce-283">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="226ce-283">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="226ce-284">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-284">**Old behavior**</span></span>

<span data-ttu-id="226ce-285">Antes de EF Core 3.0, los valores temporales se asignaban a todas las propiedades de clave para las que posteriormente la base de datos generaba un valor real.</span><span class="sxs-lookup"><span data-stu-id="226ce-285">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="226ce-286">Normalmente, estos valores temporales eran números negativos grandes.</span><span class="sxs-lookup"><span data-stu-id="226ce-286">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="226ce-287">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-287">**New behavior**</span></span>

<span data-ttu-id="226ce-288">A partir de la versión 3.0, en EF Core se almacena el valor de clave temporal como parte de la información de seguimiento de la entidad y la propiedad clave en sí no se modifica.</span><span class="sxs-lookup"><span data-stu-id="226ce-288">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="226ce-289">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-289">**Why**</span></span>

<span data-ttu-id="226ce-290">Este cambio se ha realizado para evitar que los valores de clave temporales se conviertan erróneamente en permanentes cuando una entidad de la que previamente una instancia de `DbContext` ha realizado el seguimiento se mueve a otra instancia de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="226ce-290">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="226ce-291">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-291">**Mitigations**</span></span>

<span data-ttu-id="226ce-292">Las aplicaciones que asignan valores de clave principal a claves externas para crear asociaciones entre entidades pueden depender del comportamiento anterior si las claves principales son generadas por el almacén y pertenecen a entidades en el estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="226ce-292">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="226ce-293">Esto se puede evitar con las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="226ce-293">This can be avoided by:</span></span>
* <span data-ttu-id="226ce-294">No se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="226ce-294">Not using store-generated keys.</span></span>
* <span data-ttu-id="226ce-295">Se establecen propiedades de navegación para crear relaciones en lugar de establecer valores de clave externa.</span><span class="sxs-lookup"><span data-stu-id="226ce-295">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="226ce-296">Se obtienen los valores de clave temporal reales de la información de seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="226ce-296">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="226ce-297">Por ejemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` devolverá el valor temporal aunque no se haya establecido `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="226ce-297">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="226ce-298">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="226ce-298">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="226ce-299">Problema de seguimiento n.º 14616</span><span class="sxs-lookup"><span data-stu-id="226ce-299">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="226ce-300">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="226ce-300">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="226ce-301">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-301">**Old behavior**</span></span>

<span data-ttu-id="226ce-302">Antes de EF Core 3.0, se realizaba el seguimiento en el estado `Added` de las entidades sin seguimiento detectadas por `DetectChanges` y se insertaban como una fila nueva cuando se llamaba a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="226ce-302">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="226ce-303">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-303">**New behavior**</span></span>

<span data-ttu-id="226ce-304">A partir de EF Core 3.0, si una entidad usa valores de clave generados y se establece un valor de clave, se realizará el seguimiento de la entidad en el estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="226ce-304">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="226ce-305">Esto significa que se supone que existe una fila para la entidad y que se actualizará cuando se llame a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="226ce-305">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="226ce-306">Si no se establece el valor de clave, o bien si el tipo de entidad no usa claves generadas, se seguirá realizando el seguimiento de la entidad nueva como `Added` al igual que en las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="226ce-306">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="226ce-307">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-307">**Why**</span></span>

<span data-ttu-id="226ce-308">Este cambio se ha realizado para que sea más sencillo y coherente trabajar con gráficos de entidades desconectadas mientras se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="226ce-308">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="226ce-309">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-309">**Mitigations**</span></span>

<span data-ttu-id="226ce-310">Este cambio puede interrumpir una aplicación si se configura un tipo de entidad para usar claves generadas, pero se establecen de forma explícita valores de clave para las instancias nuevas.</span><span class="sxs-lookup"><span data-stu-id="226ce-310">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="226ce-311">La solución consiste en configurar de forma explícita las propiedades de clave para que no usen valores generados.</span><span class="sxs-lookup"><span data-stu-id="226ce-311">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="226ce-312">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="226ce-312">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="226ce-313">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="226ce-313">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="226ce-314">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="226ce-314">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="226ce-315">Problema de seguimiento n.º 10114</span><span class="sxs-lookup"><span data-stu-id="226ce-315">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="226ce-316">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="226ce-316">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="226ce-317">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-317">**Old behavior**</span></span>

<span data-ttu-id="226ce-318">Antes de la versión 3.0, en EF Core no se aplicaban acciones en cascada (eliminación de entidades dependientes cuando se eliminaba una entidad de seguridad obligatoria o cuando se rompía la relación con una entidad de seguridad obligatoria) hasta que se llamaba a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="226ce-318">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="226ce-319">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-319">**New behavior**</span></span>

<span data-ttu-id="226ce-320">A partir de 3.0, en EF Core las acciones en cascada se aplican en cuanto se detecta la condición desencadenadora.</span><span class="sxs-lookup"><span data-stu-id="226ce-320">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="226ce-321">Por ejemplo, como resultado de la llamada a `context.Remove()` para eliminar una entidad de seguridad, todos los dependientes obligatorios relacionados de los que se realiza el seguimiento también se establecen en `Deleted` inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="226ce-321">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="226ce-322">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-322">**Why**</span></span>

<span data-ttu-id="226ce-323">Este cambio se ha realizado para mejorar la experiencia en escenarios de auditoría y enlace de datos donde es importante comprender qué entidades se van a eliminar _antes_ de llamar a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="226ce-323">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="226ce-324">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-324">**Mitigations**</span></span>

<span data-ttu-id="226ce-325">El comportamiento anterior se puede restaurar mediante opciones de `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="226ce-325">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="226ce-326">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-326">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="226ce-327">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="226ce-327">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="226ce-328">Problema de seguimiento n.º 12661</span><span class="sxs-lookup"><span data-stu-id="226ce-328">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="226ce-329">Este cambio se introdujo en EF Core 3.0 (versión preliminar 5).</span><span class="sxs-lookup"><span data-stu-id="226ce-329">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="226ce-330">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-330">**Old behavior**</span></span>

<span data-ttu-id="226ce-331">Antes de la versión 3.0, `DeleteBehavior.Restrict` creaba claves externas en la base de datos con la semántica `Restrict`, pero también realizaba una corrección interna de manera no evidente.</span><span class="sxs-lookup"><span data-stu-id="226ce-331">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="226ce-332">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-332">**New behavior**</span></span>

<span data-ttu-id="226ce-333">A partir de la versión 3.0, `DeleteBehavior.Restrict` garantiza que las claves externas se crean con la semántica `Restrict`, es decir, sin cascadas y realizando una infracción de restricción, sin llevar a cabo correcciones internas de EF.</span><span class="sxs-lookup"><span data-stu-id="226ce-333">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="226ce-334">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-334">**Why**</span></span>

<span data-ttu-id="226ce-335">Este cambio se realizó para mejorar la experiencia de uso de `DeleteBehavior` de manera intuitiva sin efectos secundarios inesperados.</span><span class="sxs-lookup"><span data-stu-id="226ce-335">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="226ce-336">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-336">**Mitigations**</span></span>

<span data-ttu-id="226ce-337">El comportamiento anterior se puede restaurar con `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="226ce-337">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="226ce-338">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="226ce-338">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="226ce-339">Problema de seguimiento n.º 14194</span><span class="sxs-lookup"><span data-stu-id="226ce-339">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="226ce-340">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="226ce-340">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="226ce-341">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-341">**Old behavior**</span></span>

<span data-ttu-id="226ce-342">Antes de EF Core 3.0, los [tipos de consulta](xref:core/modeling/query-types) eran un medio para consultar los datos que no definen una clave principal de una manera estructurada.</span><span class="sxs-lookup"><span data-stu-id="226ce-342">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="226ce-343">Es decir, un tipo de consulta se usaba para asignar tipos de entidad sin claves (más probablemente desde una vista, pero posiblemente desde una tabla), mientras que un tipo de entidad estándar se usaba cuando había una clave disponible (más probablemente desde una tabla, pero posiblemente desde una vista).</span><span class="sxs-lookup"><span data-stu-id="226ce-343">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="226ce-344">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-344">**New behavior**</span></span>

<span data-ttu-id="226ce-345">Ahora un tipo de consulta se convierte en un tipo de entidad sin una clave principal.</span><span class="sxs-lookup"><span data-stu-id="226ce-345">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="226ce-346">Los tipos de entidad sin clave tienen la misma funcionalidad que los tipos de consulta de las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="226ce-346">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="226ce-347">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-347">**Why**</span></span>

<span data-ttu-id="226ce-348">Este cambio se ha realizado para reducir la confusión en torno a la finalidad de los tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="226ce-348">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="226ce-349">En concreto, son tipos de entidad sin clave y, por ello, intrínsecamente son de solo lectura, pero no se deben usar solo porque un tipo de entidad tenga que ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="226ce-349">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="226ce-350">Del mismo modo, se suelen asignar a vistas, pero solo porque las vistas no suelen definir claves.</span><span class="sxs-lookup"><span data-stu-id="226ce-350">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="226ce-351">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-351">**Mitigations**</span></span>

<span data-ttu-id="226ce-352">Los elementos siguientes de la API ahora están obsoletos:</span><span class="sxs-lookup"><span data-stu-id="226ce-352">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="226ce-353">**`ModelBuilder.Query<>()`** : en su lugar es necesario llamar a `ModelBuilder.Entity<>().HasNoKey()` para marcar un tipo de entidad como sin claves.</span><span class="sxs-lookup"><span data-stu-id="226ce-353">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="226ce-354">Esto todavía no se configurará por convención para evitar una configuración incorrecta cuando se espera una clave principal, pero no coincide con la convención.</span><span class="sxs-lookup"><span data-stu-id="226ce-354">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="226ce-355">**`DbQuery<>`** : en su lugar se debe usar `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="226ce-355">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="226ce-356">**`DbContext.Query<>()`** : en su lugar se debe usar `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="226ce-356">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="226ce-357">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="226ce-357">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="226ce-358">[Problema de seguimiento n.º 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema de seguimiento n.º 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema de seguimiento n.º 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="226ce-358">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="226ce-359">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="226ce-359">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="226ce-360">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-360">**Old behavior**</span></span>

<span data-ttu-id="226ce-361">Antes de EF Core 3.0, la configuración de la relación de propiedad se realizaba directamente después de la llamada a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="226ce-361">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="226ce-362">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-362">**New behavior**</span></span>

<span data-ttu-id="226ce-363">A partir de EF Core 3.0, ahora hay una API fluida para configurar una propiedad de navegación para el propietario mediante `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="226ce-363">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="226ce-364">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-364">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="226ce-365">La configuración relacionada con la relación entre el propietario y lo que se posee ahora se debe encadenar después de `WithOwner()`, de forma similar a cómo se configuran otras relaciones.</span><span class="sxs-lookup"><span data-stu-id="226ce-365">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="226ce-366">Pero la configuración del propio tipo de propiedad se seguirá encadenando después de `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="226ce-366">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="226ce-367">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-367">For example:</span></span>

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

<span data-ttu-id="226ce-368">Además, la llamada a `Entity()`, `HasOne()` o `Set()` con un tipo de propiedad de destino ahora iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="226ce-368">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="226ce-369">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-369">**Why**</span></span>

<span data-ttu-id="226ce-370">Este cambio se ha realizado para crear una separación más clara entre la configuración del propio tipo de propiedad y la _relación con_ el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="226ce-370">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="226ce-371">A su vez, esto elimina la ambigüedad y la confusión de métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="226ce-371">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="226ce-372">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-372">**Mitigations**</span></span>

<span data-ttu-id="226ce-373">Cambie la configuración de las relaciones de tipo de propiedad para usar la nueva superficie de API, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="226ce-373">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="226ce-374">Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales</span><span class="sxs-lookup"><span data-stu-id="226ce-374">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="226ce-375">Problema de seguimiento n.º 9005</span><span class="sxs-lookup"><span data-stu-id="226ce-375">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="226ce-376">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-376">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-377">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-377">**Old behavior**</span></span>

<span data-ttu-id="226ce-378">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="226ce-378">Consider the following model:</span></span>
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
<span data-ttu-id="226ce-379">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, siempre era necesaria una instancia de `OrderDetails` al agregar un elemento `Order` nuevo.</span><span class="sxs-lookup"><span data-stu-id="226ce-379">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="226ce-380">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-380">**New behavior**</span></span>

<span data-ttu-id="226ce-381">A partir de la versión 3.0, EF Core permite agregar `Order` sin `OrderDetails` y asigna todas las propiedades `OrderDetails` a excepción de la clave principal a columnas que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="226ce-381">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="226ce-382">Al realizar consultas, EF Core establece `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor o si no tiene propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.</span><span class="sxs-lookup"><span data-stu-id="226ce-382">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="226ce-383">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-383">**Mitigations**</span></span>

<span data-ttu-id="226ce-384">Si el modelo tiene una tabla que comparte dependencias con todas las columnas opcionales, pero la navegación que apunta a ella no se espera que sea `null`, la aplicación debería modificarse para controlar los casos en los que la navegación sea `null`.</span><span class="sxs-lookup"><span data-stu-id="226ce-384">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="226ce-385">Si esto no es posible, debería agregarse una propiedad necesaria al tipo de entidad o, al menos, una entidad debería tener un valor distinto a `null` asignado.</span><span class="sxs-lookup"><span data-stu-id="226ce-385">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="226ce-386">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="226ce-386">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="226ce-387">Problema de seguimiento n.º 14154</span><span class="sxs-lookup"><span data-stu-id="226ce-387">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="226ce-388">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-388">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-389">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-389">**Old behavior**</span></span>

<span data-ttu-id="226ce-390">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="226ce-390">Consider the following model:</span></span>
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
<span data-ttu-id="226ce-391">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, si solo se actualizaba `OrderDetails`, no se actualizaba el valor `Version` en el cliente y se producía un error en la próxima actualización.</span><span class="sxs-lookup"><span data-stu-id="226ce-391">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="226ce-392">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-392">**New behavior**</span></span>

<span data-ttu-id="226ce-393">A partir de la versión 3.0, EF Core propaga el nuevo valor `Version` en `Order` si posee `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="226ce-393">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="226ce-394">En caso contrario, se produce una excepción durante la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="226ce-394">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="226ce-395">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-395">**Why**</span></span>

<span data-ttu-id="226ce-396">Este cambio se realizó para evitar un valor de token de simultaneidad obsoleto cuando solo se actualiza una de las entidades asignadas a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="226ce-396">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="226ce-397">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-397">**Mitigations**</span></span>

<span data-ttu-id="226ce-398">Todas las entidades que comparten la tabla deben incluir una propiedad que se asigna a la columna del token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="226ce-398">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="226ce-399">Es posible crear una en estado reemplazado:</span><span class="sxs-lookup"><span data-stu-id="226ce-399">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="226ce-400">Ahora, las propiedades heredadas de tipos sin asignar se asignan a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="226ce-400">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="226ce-401">Problema de seguimiento n.º 13998</span><span class="sxs-lookup"><span data-stu-id="226ce-401">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="226ce-402">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-402">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-403">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-403">**Old behavior**</span></span>

<span data-ttu-id="226ce-404">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="226ce-404">Consider the following model:</span></span>
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

<span data-ttu-id="226ce-405">Antes de EF Core 3.0, la propiedad `ShippingAddress` se asignaba a columnas distintas para `BulkOrder` y `Order` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="226ce-405">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="226ce-406">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-406">**New behavior**</span></span>

<span data-ttu-id="226ce-407">A partir de la versión3.0, EF Core solo crea una columna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="226ce-407">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="226ce-408">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-408">**Why**</span></span>

<span data-ttu-id="226ce-409">El comportamiento anterior no era el esperado.</span><span class="sxs-lookup"><span data-stu-id="226ce-409">The old behavoir was unexpected.</span></span>

<span data-ttu-id="226ce-410">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-410">**Mitigations**</span></span>

<span data-ttu-id="226ce-411">Todavía se puede asignar explícitamente la propiedad a columnas separadas en los tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="226ce-411">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="226ce-412">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="226ce-412">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="226ce-413">Problema de seguimiento n.º 13274</span><span class="sxs-lookup"><span data-stu-id="226ce-413">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="226ce-414">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="226ce-414">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="226ce-415">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-415">**Old behavior**</span></span>

<span data-ttu-id="226ce-416">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="226ce-416">Consider the following model:</span></span>
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
<span data-ttu-id="226ce-417">Antes de EF Core 3.0, se podía usar la propiedad `CustomerId` para la clave externa por convención.</span><span class="sxs-lookup"><span data-stu-id="226ce-417">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="226ce-418">Pero si `Order` es un tipo de propiedad, entonces esto convertiría también a `CustomerId` en la clave principal, algo que no suele ser lo esperado.</span><span class="sxs-lookup"><span data-stu-id="226ce-418">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="226ce-419">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-419">**New behavior**</span></span>

<span data-ttu-id="226ce-420">A partir de la versión 3.0, EF Core no intenta usar las propiedades de claves externas por convención si tienen el mismo nombre que la propiedad de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="226ce-420">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="226ce-421">Los patrones de nombre de tipo de entidad de seguridad concatenado con el nombre de propiedad de la entidad de seguridad y de nombre de navegación concatenado con el nombre de propiedad de la entidad de seguridad todavía se hacen coincidir.</span><span class="sxs-lookup"><span data-stu-id="226ce-421">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="226ce-422">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-422">For example:</span></span>

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

<span data-ttu-id="226ce-423">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-423">**Why**</span></span>

<span data-ttu-id="226ce-424">Este cambio se ha realizado para evitar definir erróneamente una propiedad de clave principal en el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="226ce-424">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="226ce-425">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-425">**Mitigations**</span></span>

<span data-ttu-id="226ce-426">Si la propiedad se ha diseñado para ser la clave externa y, por tanto, parte de la clave principal, se debe configurar explícitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="226ce-426">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="226ce-427">Ahora, la conexión de base de datos se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="226ce-427">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="226ce-428">Problema de seguimiento n.º 14218</span><span class="sxs-lookup"><span data-stu-id="226ce-428">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="226ce-429">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-429">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-430">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-430">**Old behavior**</span></span>

<span data-ttu-id="226ce-431">Antes de EF Core 3.0, si el contexto abría la conexión dentro de `TransactionScope`, la conexión permanecía abierta mientras el ámbito actual `TransactionScope` estuviese activo.</span><span class="sxs-lookup"><span data-stu-id="226ce-431">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="226ce-432">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-432">**New behavior**</span></span>

<span data-ttu-id="226ce-433">A partir de la versión 3.0, EF Core cierra la conexión en cuanto se deja de usar.</span><span class="sxs-lookup"><span data-stu-id="226ce-433">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="226ce-434">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-434">**Why**</span></span>

<span data-ttu-id="226ce-435">Este cambio permite usar varios contextos en el mismo ámbito `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="226ce-435">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="226ce-436">El comportamiento nuevo también coincide con el de EF6.</span><span class="sxs-lookup"><span data-stu-id="226ce-436">The new behavior also matches EF6.</span></span>

<span data-ttu-id="226ce-437">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-437">**Mitigations**</span></span>

<span data-ttu-id="226ce-438">Si la conexión debe permanecer abierta, una llamada explícita a `OpenConnection()` asegurará que EF Core no la cierre de forma prematura:</span><span class="sxs-lookup"><span data-stu-id="226ce-438">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="226ce-439">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="226ce-439">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="226ce-440">Problema de seguimiento n.º 6872</span><span class="sxs-lookup"><span data-stu-id="226ce-440">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="226ce-441">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-441">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-442">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-442">**Old behavior**</span></span>

<span data-ttu-id="226ce-443">Antes de EF Core 3.0, se usaba un generador de valores compartidos para todas las propiedades de clave entera en memoria.</span><span class="sxs-lookup"><span data-stu-id="226ce-443">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="226ce-444">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-444">**New behavior**</span></span>

<span data-ttu-id="226ce-445">A partir de EF Core 3.0, cada propiedad de clave entera obtiene su propio generador de valores cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="226ce-445">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="226ce-446">Además, si se elimina la base de datos, la generación de claves se restablece para todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="226ce-446">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="226ce-447">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-447">**Why**</span></span>

<span data-ttu-id="226ce-448">Este cambio se ha realizado para alinear la generación de claves en memoria más estrechamente a la generación de claves de base de datos reales y para mejorar la capacidad para aislar las pruebas entre sí cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="226ce-448">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="226ce-449">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-449">**Mitigations**</span></span>

<span data-ttu-id="226ce-450">Esto puede interrumpir una aplicación que se base en el establecimiento de valores de clave específicos en memoria.</span><span class="sxs-lookup"><span data-stu-id="226ce-450">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="226ce-451">En su lugar, considere la posibilidad de no depender de valores de clave específicos, o bien de actualizar para que coincida con el comportamiento nuevo.</span><span class="sxs-lookup"><span data-stu-id="226ce-451">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="226ce-452">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="226ce-452">Backing fields are used by default</span></span>

[<span data-ttu-id="226ce-453">Problema de seguimiento n.º 12430</span><span class="sxs-lookup"><span data-stu-id="226ce-453">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="226ce-454">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="226ce-454">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="226ce-455">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-455">**Old behavior**</span></span>

<span data-ttu-id="226ce-456">Antes de la versión 3.0, incluso si se conocía el campo de respaldo de una propiedad, de forma predeterminada en EF Core se leía y escribía el valor de propiedad mediante los métodos captadores y establecedores de propiedades.</span><span class="sxs-lookup"><span data-stu-id="226ce-456">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="226ce-457">La excepción era la ejecución de consultas, donde el campo de respaldo se establecía directamente si se conocía.</span><span class="sxs-lookup"><span data-stu-id="226ce-457">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="226ce-458">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-458">**New behavior**</span></span>

<span data-ttu-id="226ce-459">A partir de EF Core 3.0, si se conoce el campo de respaldo para una propiedad, EF Core siempre la leerá y escribirá mediante el campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="226ce-459">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="226ce-460">Esto podría provocar una interrupción de la aplicación si depende de un comportamiento adicional codificado en los métodos captadores o establecedores.</span><span class="sxs-lookup"><span data-stu-id="226ce-460">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="226ce-461">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-461">**Why**</span></span>

<span data-ttu-id="226ce-462">Este cambio se ha realizado para evitar que EF Core desencadene erróneamente lógica de negocios de forma predeterminada al realizar operaciones de base de datos que implican entidades.</span><span class="sxs-lookup"><span data-stu-id="226ce-462">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="226ce-463">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-463">**Mitigations**</span></span>

<span data-ttu-id="226ce-464">El comportamiento anterior a la versión 3.0 se puede restaurar mediante la configuración del modo de acceso de propiedad en `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="226ce-464">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="226ce-465">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-465">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="226ce-466">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="226ce-466">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="226ce-467">Problema de seguimiento n.º 12523</span><span class="sxs-lookup"><span data-stu-id="226ce-467">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="226ce-468">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-468">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-469">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-469">**Old behavior**</span></span>

<span data-ttu-id="226ce-470">Antes de EF Core 3.0, si varios campos coincidían con las reglas para buscar el campo de respaldo de una propiedad, se elegía un campo según un orden de prioridad.</span><span class="sxs-lookup"><span data-stu-id="226ce-470">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="226ce-471">Esto podía producir que, en caso de ambigüedad, se usara el campo incorrecto.</span><span class="sxs-lookup"><span data-stu-id="226ce-471">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="226ce-472">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-472">**New behavior**</span></span>

<span data-ttu-id="226ce-473">A partir de EF Core 3.0, si varios campos coinciden con la misma propiedad, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="226ce-473">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="226ce-474">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-474">**Why**</span></span>

<span data-ttu-id="226ce-475">Este cambio se ha realizado para evitar de forma silenciosa el uso de un campo con respecto a otro cuando solo uno puede ser correcto.</span><span class="sxs-lookup"><span data-stu-id="226ce-475">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="226ce-476">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-476">**Mitigations**</span></span>

<span data-ttu-id="226ce-477">En las propiedades con campos de respaldo ambiguos se debe especificar de forma explícita el campo que se va usar.</span><span class="sxs-lookup"><span data-stu-id="226ce-477">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="226ce-478">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="226ce-478">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="226ce-479">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="226ce-479">Field-only property names should match the field name</span></span>

<span data-ttu-id="226ce-480">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-481">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-481">**Old behavior**</span></span>

<span data-ttu-id="226ce-482">Antes de EF Core 3.0, una propiedad podía especificarse con un valor de cadena y, si no había ninguna propiedad con ese nombre en el tipo CLR, EF Core intentaba hacerla coincidir con un campo mediante reglas de convención.</span><span class="sxs-lookup"><span data-stu-id="226ce-482">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="226ce-483">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-483">**New behavior**</span></span>

<span data-ttu-id="226ce-484">A partir de EF Core 3.0, una propiedad de solo campo debe coincidir exactamente con el nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="226ce-484">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="226ce-485">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-485">**Why**</span></span>

<span data-ttu-id="226ce-486">Este cambio se realizó para evitar el uso del mismo campo para dos propiedades con nombres similares. También hace que las reglas de coincidencia para propiedades solo de campo sean las mismas que para las propiedades asignadas a propiedades CLR.</span><span class="sxs-lookup"><span data-stu-id="226ce-486">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="226ce-487">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-487">**Mitigations**</span></span>

<span data-ttu-id="226ce-488">Las propiedades solo de campo deberían tener el mismo nombre que el campo al que están asignadas.</span><span class="sxs-lookup"><span data-stu-id="226ce-488">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="226ce-489">En una próxima versión preliminar de EF Core 3.0 tenemos planeado volver a habilitar la configuración explícita de un nombre de campo distinto al nombre de la propiedad:</span><span class="sxs-lookup"><span data-stu-id="226ce-489">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="226ce-490">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="226ce-490">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="226ce-491">Problema de seguimiento n.° 14756</span><span class="sxs-lookup"><span data-stu-id="226ce-491">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="226ce-492">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-492">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-493">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-493">**Old behavior**</span></span>

<span data-ttu-id="226ce-494">Antes de EF Core 3.0, llamar a `AddDbContext` o a `AddDbContextPool` también podría registrar los servicios de almacenamiento en caché y de registro con D.I. a través de llamadas a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) y a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="226ce-494">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="226ce-495">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-495">**New behavior**</span></span>

<span data-ttu-id="226ce-496">A partir de EF Core 3.0, `AddDbContext` y `AddDbContextPool` ya no registrarán estos servicios con inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="226ce-496">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="226ce-497">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-497">**Why**</span></span>

<span data-ttu-id="226ce-498">EF Core 3.0 no requiere que estos servicios estén en el contenedor de inserción de dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="226ce-498">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="226ce-499">Pero si `ILoggerFactory` se registra en el contenedor de DI de la aplicación, EF Core lo empezará a usar de todos modos.</span><span class="sxs-lookup"><span data-stu-id="226ce-499">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="226ce-500">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-500">**Mitigations**</span></span>

<span data-ttu-id="226ce-501">Si la aplicación necesita estos servicios, regístrelos de manera explícita con el contenedor de DI mediante [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="226ce-501">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="226ce-502">Ahora DbContext.Entry realiza una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="226ce-502">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="226ce-503">Problema de seguimiento n.º 13552</span><span class="sxs-lookup"><span data-stu-id="226ce-503">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="226ce-504">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="226ce-504">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="226ce-505">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-505">**Old behavior**</span></span>

<span data-ttu-id="226ce-506">Antes de EF Core 3.0, la llamada a `DbContext.Entry` provocaba que se detectaran cambios para todas las entidades con seguimiento.</span><span class="sxs-lookup"><span data-stu-id="226ce-506">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="226ce-507">Esto garantizaba que el estado expuesto en `EntityEntry` estuviera actualizado.</span><span class="sxs-lookup"><span data-stu-id="226ce-507">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="226ce-508">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-508">**New behavior**</span></span>

<span data-ttu-id="226ce-509">A partir de EF Core 3.0, ahora la llamada a `DbContext.Entry` solo intenta detectar cambios en la entidad dada y cualquier entidad de seguridad relacionada con ella de la que se haya realizado el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="226ce-509">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="226ce-510">Esto significa que es posible que la llamada a este método no haya detectado otros cambios, lo que podría tener implicaciones en el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="226ce-510">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="226ce-511">Observe que si `ChangeTracker.AutoDetectChangesEnabled` se establece en `false` incluso esta detección de cambios local se deshabilitará.</span><span class="sxs-lookup"><span data-stu-id="226ce-511">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="226ce-512">Otros métodos que provocan la detección de cambios (como `ChangeTracker.Entries` y `SaveChanges`) siguen provocando una acción `DetectChanges` completa de todas las entidades de las que se realiza el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="226ce-512">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="226ce-513">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-513">**Why**</span></span>

<span data-ttu-id="226ce-514">Este cambio se ha realizado para mejorar el rendimiento predeterminado del uso de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="226ce-514">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="226ce-515">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-515">**Mitigations**</span></span>

<span data-ttu-id="226ce-516">Llame a `ChgangeTracker.DetectChanges()` de forma explícita antes de llamar a `Entry` para garantizar el comportamiento anterior a la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="226ce-516">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="226ce-517">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="226ce-517">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="226ce-518">Problema de seguimiento n.º 14617</span><span class="sxs-lookup"><span data-stu-id="226ce-518">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="226ce-519">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-519">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-520">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-520">**Old behavior**</span></span>

<span data-ttu-id="226ce-521">Antes de EF Core 3.0, se podían usar las propiedades de clave `string` y `byte[]` sin tener que establecer de forma explícita un valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="226ce-521">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="226ce-522">En ese caso, el valor de clave se generaba en el cliente como un GUID, que se serializaba en bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="226ce-522">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="226ce-523">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-523">**New behavior**</span></span>

<span data-ttu-id="226ce-524">A partir de EF Core 3.0, se iniciará una excepción en la que indica que no se ha establecido ningún valor de clave.</span><span class="sxs-lookup"><span data-stu-id="226ce-524">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="226ce-525">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-525">**Why**</span></span>

<span data-ttu-id="226ce-526">Este cambio se ha realizado porque los valores `string`/`byte[]` generados por el cliente no suelen ser útiles, y el comportamiento predeterminado dificultaba razonar sobre los valores de clave generados de una forma habitual.</span><span class="sxs-lookup"><span data-stu-id="226ce-526">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="226ce-527">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-527">**Mitigations**</span></span>

<span data-ttu-id="226ce-528">Se puede obtener el comportamiento anterior a la versión 3.0 si se especifica de forma explícita que las propiedades de clave deben usar los valores generados si no se establece ningún otro valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="226ce-528">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="226ce-529">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="226ce-529">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="226ce-530">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="226ce-530">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="226ce-531">Ahora ILoggerFactory es un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="226ce-531">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="226ce-532">Problema de seguimiento n.º 14698</span><span class="sxs-lookup"><span data-stu-id="226ce-532">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="226ce-533">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="226ce-533">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="226ce-534">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-534">**Old behavior**</span></span>

<span data-ttu-id="226ce-535">Antes de EF Core 3.0, `ILoggerFactory` se registraba como un servicio de singleton.</span><span class="sxs-lookup"><span data-stu-id="226ce-535">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="226ce-536">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-536">**New behavior**</span></span>

<span data-ttu-id="226ce-537">A partir de EF Core 3.0, `ILoggerFactory` ahora se registra como con ámbito.</span><span class="sxs-lookup"><span data-stu-id="226ce-537">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="226ce-538">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-538">**Why**</span></span>

<span data-ttu-id="226ce-539">Este cambio se ha realizado para permitir la asociación de un registrador con una instancia de `DbContext`, lo que habilita otras funciones y quita algunos casos de comportamiento patológico como un aumento vertiginoso de los proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="226ce-539">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="226ce-540">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-540">**Mitigations**</span></span>

<span data-ttu-id="226ce-541">Este cambio no debería afectar al código de la aplicación a menos que registre y use servicios personalizados en el proveedor de servicios internos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="226ce-541">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="226ce-542">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="226ce-542">This isn't common.</span></span>
<span data-ttu-id="226ce-543">En estos casos, la mayoría de los elementos seguirá funcionando, pero cualquier servicio de singleton que dependiera de `ILoggerFactory` tendrá que cambiarse para obtener la interfaz `ILoggerFactory` de otra forma.</span><span class="sxs-lookup"><span data-stu-id="226ce-543">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="226ce-544">Si experimenta situaciones como esta, registre un problema en el [rastreador de problemas de GitHub de EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para hacernos saber cómo usa `ILoggerFactory`, para que podamos comprender mejor cómo evitar esta interrupción en el futuro.</span><span class="sxs-lookup"><span data-stu-id="226ce-544">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="226ce-545">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="226ce-545">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="226ce-546">Problema de seguimiento n.º 12780</span><span class="sxs-lookup"><span data-stu-id="226ce-546">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="226ce-547">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-548">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-548">**Old behavior**</span></span>

<span data-ttu-id="226ce-549">Antes de EF Core 3.0, una vez que se eliminaba `DbContext` no había ninguna forma de saber si una determinada propiedad de navegación de una entidad obtenida de ese contexto se había cargado completamente o no.</span><span class="sxs-lookup"><span data-stu-id="226ce-549">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="226ce-550">En su lugar, los proxies asumían que una navegación de referencia se cargaba si tenía un valor distinto de NULL, y que una navegación de colección se cargaba si no estaba vacía.</span><span class="sxs-lookup"><span data-stu-id="226ce-550">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="226ce-551">En estos casos, el intento de carga diferida era no operativo.</span><span class="sxs-lookup"><span data-stu-id="226ce-551">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="226ce-552">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-552">**New behavior**</span></span>

<span data-ttu-id="226ce-553">A partir de EF Core 3.0, los proxies realizan el seguimiento de si una propiedad de navegación se carga o no.</span><span class="sxs-lookup"><span data-stu-id="226ce-553">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="226ce-554">Esto significa que el intento de acceder a una propiedad de navegación que se carga después de que se haya eliminado el contexto siempre será no operativo, incluso cuando la navegación cargada está vacía o es NULL.</span><span class="sxs-lookup"><span data-stu-id="226ce-554">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="226ce-555">Por el contrario, el intento de acceder a una propiedad de navegación que no está cargada iniciará una excepción si el contexto se ha eliminado, incluso si la propiedad de navegación es una colección no vacía.</span><span class="sxs-lookup"><span data-stu-id="226ce-555">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="226ce-556">Si se produce esta situación, significa que el código de aplicación está intentando usar la carga diferida en un momento no válido y que se debe cambiar la aplicación para que lo no haga.</span><span class="sxs-lookup"><span data-stu-id="226ce-556">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="226ce-557">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-557">**Why**</span></span>

<span data-ttu-id="226ce-558">Este cambio se ha realizado para que el comportamiento sea coherente y correcto cuando se intenta la carga diferida de una instancia de `DbContext` eliminada.</span><span class="sxs-lookup"><span data-stu-id="226ce-558">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="226ce-559">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-559">**Mitigations**</span></span>

<span data-ttu-id="226ce-560">Actualice el código de la aplicación para que no intente la carga diferida con un contexto eliminado, o bien configúrelo para que sea no operativo, como se describe en el mensaje de la excepción.</span><span class="sxs-lookup"><span data-stu-id="226ce-560">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="226ce-561">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="226ce-561">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="226ce-562">Problema de seguimiento n.º 10236</span><span class="sxs-lookup"><span data-stu-id="226ce-562">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="226ce-563">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="226ce-563">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="226ce-564">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-564">**Old behavior**</span></span>

<span data-ttu-id="226ce-565">Antes de EF Core 3.0, se registraba una advertencia para una aplicación que creaba un número patológico de proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="226ce-565">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="226ce-566">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-566">**New behavior**</span></span>

<span data-ttu-id="226ce-567">A partir de EF Core 3.0, ahora esta advertencia se considera un error y se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="226ce-567">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="226ce-568">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-568">**Why**</span></span>

<span data-ttu-id="226ce-569">Este cambio se ha realizado para controlar mejor el código de la aplicación mediante la exposición de este caso patológico de una forma más explícita.</span><span class="sxs-lookup"><span data-stu-id="226ce-569">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="226ce-570">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-570">**Mitigations**</span></span>

<span data-ttu-id="226ce-571">Cuando se produce este error, la acción más adecuada consiste en comprender la causa raíz y detener la creación de tantos proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="226ce-571">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="226ce-572">Pero el error se puede convertir en una advertencia (u omitirse) mediante configuración en `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="226ce-572">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="226ce-573">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-573">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="226ce-574">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="226ce-574">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="226ce-575">Problema de seguimiento n.° 9171</span><span class="sxs-lookup"><span data-stu-id="226ce-575">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="226ce-576">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-576">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-577">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-577">**Old behavior**</span></span>

<span data-ttu-id="226ce-578">Antes de EF Core 3.0, el código para llamar a `HasOne` o `HasMany` con una cadena se interpretaba de manera confusa.</span><span class="sxs-lookup"><span data-stu-id="226ce-578">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="226ce-579">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-579">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="226ce-580">El código parece relacionar `Samurai` con otro tipo de entidad mediante la propiedad de navegación `Entrance`, que puede ser privada.</span><span class="sxs-lookup"><span data-stu-id="226ce-580">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="226ce-581">En realidad, este código intenta crear una relación con algún tipo de entidad denominada `Entrance` sin ninguna propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="226ce-581">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="226ce-582">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-582">**New behavior**</span></span>

<span data-ttu-id="226ce-583">A partir de EF Core 3.0, el código anterior ahora hace lo que parecía que debía hacer antes.</span><span class="sxs-lookup"><span data-stu-id="226ce-583">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="226ce-584">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-584">**Why**</span></span>

<span data-ttu-id="226ce-585">El comportamiento anterior era muy confuso, especialmente al leer el código de configuración y al buscar errores.</span><span class="sxs-lookup"><span data-stu-id="226ce-585">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="226ce-586">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-586">**Mitigations**</span></span>

<span data-ttu-id="226ce-587">Esto solo interrumpirá las aplicaciones que configuran de manera explícita las relaciones con cadenas para nombres de tipos y sin especificar explícitamente la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="226ce-587">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="226ce-588">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="226ce-588">This is not common.</span></span>
<span data-ttu-id="226ce-589">El comportamiento anterior se puede obtener al pasar de manera explícita `null` para el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="226ce-589">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="226ce-590">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-590">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="226ce-591">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="226ce-591">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="226ce-592">Problema de seguimiento n.º 15184</span><span class="sxs-lookup"><span data-stu-id="226ce-592">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="226ce-593">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-593">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-594">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-594">**Old behavior**</span></span>

<span data-ttu-id="226ce-595">Antes, los siguientes métodos asincrónicos devolvían `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="226ce-595">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="226ce-596">`ValueGenerator.NextValueAsync()` (y las clases derivadas)</span><span class="sxs-lookup"><span data-stu-id="226ce-596">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="226ce-597">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-597">**New behavior**</span></span>

<span data-ttu-id="226ce-598">Dichos métodos ahora devuelven `ValueTask<T>` durante el mismo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="226ce-598">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="226ce-599">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-599">**Why**</span></span>

<span data-ttu-id="226ce-600">Este cambio reduce el número de asignaciones de montones que se producen al invocar estos métodos, lo que mejora el rendimiento general.</span><span class="sxs-lookup"><span data-stu-id="226ce-600">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="226ce-601">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-601">**Mitigations**</span></span>

<span data-ttu-id="226ce-602">Las aplicaciones que simplemente esperen las API anteriores solo necesitan recompilarse, sin que sea necesario realizar cambios en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="226ce-602">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="226ce-603">Un uso más complejo (p. ej., pasar el valor `Task` devuelto a `Task.WhenAny()`) normalmente requiere que el valor `ValueTask<T>` devuelto se convierta en `Task<T>` mediante una llamada a `AsTask()` en él.</span><span class="sxs-lookup"><span data-stu-id="226ce-603">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="226ce-604">Tenga en cuenta que esto niega la reducción de asignación que implica este cambio.</span><span class="sxs-lookup"><span data-stu-id="226ce-604">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="226ce-605">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="226ce-605">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="226ce-606">Problema de seguimiento n.º 9913</span><span class="sxs-lookup"><span data-stu-id="226ce-606">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="226ce-607">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="226ce-607">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="226ce-608">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-608">**Old behavior**</span></span>

<span data-ttu-id="226ce-609">El nombre de anotación para las anotaciones de asignación de tipos era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="226ce-609">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="226ce-610">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-610">**New behavior**</span></span>

<span data-ttu-id="226ce-611">Ahora, el nombre de anotación para las anotaciones de asignación de tipos es "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="226ce-611">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="226ce-612">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-612">**Why**</span></span>

<span data-ttu-id="226ce-613">Ahora, las asignaciones de tipos se usan para algo más que solo para proveedores de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="226ce-613">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="226ce-614">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-614">**Mitigations**</span></span>

<span data-ttu-id="226ce-615">Esto solo interrumpirá a las aplicaciones que acceden directamente a la asignación de tipos como una anotación, lo que no es habitual.</span><span class="sxs-lookup"><span data-stu-id="226ce-615">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="226ce-616">La acción más apropiada para corregir es usar la superficie de API para acceder a las asignaciones de tipos en lugar de usar directamente la anotación.</span><span class="sxs-lookup"><span data-stu-id="226ce-616">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="226ce-617">ToTable en un tipo derivado inicia una excepción</span><span class="sxs-lookup"><span data-stu-id="226ce-617">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="226ce-618">Problema de seguimiento n.º 11811</span><span class="sxs-lookup"><span data-stu-id="226ce-618">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="226ce-619">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="226ce-619">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="226ce-620">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-620">**Old behavior**</span></span>

<span data-ttu-id="226ce-621">Antes de EF Core 3.0, la llamada a `ToTable()` en un tipo derivado se omitía, ya que la única estrategia asignación de herencia era TPH, lo que no es válido.</span><span class="sxs-lookup"><span data-stu-id="226ce-621">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="226ce-622">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-622">**New behavior**</span></span>

<span data-ttu-id="226ce-623">A partir de EF Core 3.0, y en preparación para agregar compatibilidad con TPT y TPC en una versión posterior, ahora la llamada a `ToTable()` en un tipo derivado iniciará una excepción para evitar un cambio de asignación inesperado en el futuro.</span><span class="sxs-lookup"><span data-stu-id="226ce-623">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="226ce-624">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-624">**Why**</span></span>

<span data-ttu-id="226ce-625">En la actualidad no se considera válido asignar un tipo derivado a otra tabla.</span><span class="sxs-lookup"><span data-stu-id="226ce-625">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="226ce-626">Este cambio evita interrupciones en el futuro, cuando se convierta en una operación válida.</span><span class="sxs-lookup"><span data-stu-id="226ce-626">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="226ce-627">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-627">**Mitigations**</span></span>

<span data-ttu-id="226ce-628">Quite todos los intentos de asignar tipos derivados a otras tablas.</span><span class="sxs-lookup"><span data-stu-id="226ce-628">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="226ce-629">ForSqlServerHasIndex se ha reemplazado por HasIndex</span><span class="sxs-lookup"><span data-stu-id="226ce-629">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="226ce-630">Problema de seguimiento n.º 12366</span><span class="sxs-lookup"><span data-stu-id="226ce-630">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="226ce-631">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="226ce-631">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="226ce-632">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-632">**Old behavior**</span></span>

<span data-ttu-id="226ce-633">Antes de EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` proporcionaba una manera de configurar las columnas que se usaban con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="226ce-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="226ce-634">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-634">**New behavior**</span></span>

<span data-ttu-id="226ce-635">A partir de EF Core 3.0, ya se admite el uso de `Include` en un índice en el nivel relacional.</span><span class="sxs-lookup"><span data-stu-id="226ce-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="226ce-636">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="226ce-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="226ce-637">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-637">**Why**</span></span>

<span data-ttu-id="226ce-638">Este cambio se ha realizado para consolidar la API para índices con `Include` en un mismo lugar para todos los proveedores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="226ce-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="226ce-639">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-639">**Mitigations**</span></span>

<span data-ttu-id="226ce-640">Use la API nueva, como se ha mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="226ce-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="226ce-641">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="226ce-641">Metadata API changes</span></span>

[<span data-ttu-id="226ce-642">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="226ce-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="226ce-643">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-643">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-644">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-644">**New behavior**</span></span>

<span data-ttu-id="226ce-645">Las siguientes propiedades se han convertido en métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="226ce-645">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="226ce-646">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-646">**Why**</span></span>

<span data-ttu-id="226ce-647">Este cambio simplifica la implementación de las interfaces mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="226ce-647">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="226ce-648">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-648">**Mitigations**</span></span>

<span data-ttu-id="226ce-649">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="226ce-649">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="226ce-650">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="226ce-650">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="226ce-651">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="226ce-651">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="226ce-652">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="226ce-652">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="226ce-653">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-653">**New behavior**</span></span>

<span data-ttu-id="226ce-654">Los métodos de extensión específicos del proveedor se simplificarán:</span><span class="sxs-lookup"><span data-stu-id="226ce-654">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="226ce-655">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-655">**Why**</span></span>

<span data-ttu-id="226ce-656">Este cambio simplifica la implementación de los métodos de extensión mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="226ce-656">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="226ce-657">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-657">**Mitigations**</span></span>

<span data-ttu-id="226ce-658">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="226ce-658">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="226ce-659">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="226ce-659">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="226ce-660">Problema de seguimiento n.º 12151</span><span class="sxs-lookup"><span data-stu-id="226ce-660">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="226ce-661">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="226ce-661">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="226ce-662">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-662">**Old behavior**</span></span>

<span data-ttu-id="226ce-663">Antes de EF Core 3.0, EF Core enviaba `PRAGMA foreign_keys = 1` cuando se abría una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="226ce-663">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="226ce-664">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-664">**New behavior**</span></span>

<span data-ttu-id="226ce-665">A partir de EF Core 3.0, EF Core ya no envía `PRAGMA foreign_keys = 1` cuando se abre una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="226ce-665">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="226ce-666">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-666">**Why**</span></span>

<span data-ttu-id="226ce-667">Este cambio se ha realizado porque en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3` de forma predeterminada, lo que a su vez significa que el cumplimiento de CD está activado de forma predeterminada y no es necesario habilitarlo explícitamente cada vez que se abra una conexión.</span><span class="sxs-lookup"><span data-stu-id="226ce-667">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="226ce-668">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-668">**Mitigations**</span></span>

<span data-ttu-id="226ce-669">Las claves externas se habilitan de forma predeterminada en SQLitePCLRaw.bundle_e_sqlite3, que en EF Core se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="226ce-669">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="226ce-670">Para otros casos, las claves externas se pueden habilitar mediante la especificación de `Foreign Keys=True` en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="226ce-670">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="226ce-671">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="226ce-671">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="226ce-672">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-672">**Old behavior**</span></span>

<span data-ttu-id="226ce-673">Antes de EF Core 3.0, en EF Core se usaba `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="226ce-673">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="226ce-674">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-674">**New behavior**</span></span>

<span data-ttu-id="226ce-675">A partir de EF Core 3.0, en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="226ce-675">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="226ce-676">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-676">**Why**</span></span>

<span data-ttu-id="226ce-677">Este cambio se ha realizado para que la versión de SQLite que se usa en iOS sea coherente con otras plataformas.</span><span class="sxs-lookup"><span data-stu-id="226ce-677">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="226ce-678">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-678">**Mitigations**</span></span>

<span data-ttu-id="226ce-679">Para usar la versión nativa de SQLite en iOS, configure `Microsoft.Data.Sqlite` para usar otra agrupación `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="226ce-679">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="226ce-680">Almacenamiento de valores GUID como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="226ce-680">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="226ce-681">Problema de seguimiento n.º 15078</span><span class="sxs-lookup"><span data-stu-id="226ce-681">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="226ce-682">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-682">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-683">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-683">**Old behavior**</span></span>

<span data-ttu-id="226ce-684">Antes, los valores GUID se almacenaban como valores BLOB en SQLite.</span><span class="sxs-lookup"><span data-stu-id="226ce-684">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="226ce-685">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-685">**New behavior**</span></span>

<span data-ttu-id="226ce-686">Ahora, los valores GUID se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="226ce-686">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="226ce-687">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-687">**Why**</span></span>

<span data-ttu-id="226ce-688">El formato binario de los GUID no está normalizado.</span><span class="sxs-lookup"><span data-stu-id="226ce-688">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="226ce-689">El almacenamiento de los valores como TEXT mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="226ce-689">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="226ce-690">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-690">**Mitigations**</span></span>

<span data-ttu-id="226ce-691">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="226ce-691">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="226ce-692">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="226ce-692">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="226ce-693">Microsoft.Data.Sqlite sigue siendo capaz de leer valores GUID de ambas columnas BLOB y TEXT. Sin embargo, dado que el formato predeterminado de los parámetros y las constantes ha cambiado, seguramente deberá realizar alguna acción en la mayoría de casos que impliquen el uso de valores GUID.</span><span class="sxs-lookup"><span data-stu-id="226ce-693">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="226ce-694">Ahora los valores char se almacenan como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="226ce-694">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="226ce-695">Problema de seguimiento n.º 15020</span><span class="sxs-lookup"><span data-stu-id="226ce-695">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="226ce-696">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-696">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-697">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-697">**Old behavior**</span></span>

<span data-ttu-id="226ce-698">Anteriormente los valores char se almacenaban como valores INTEGER en SQLite.</span><span class="sxs-lookup"><span data-stu-id="226ce-698">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="226ce-699">Por ejemplo, un valor char de *A* se almacenaba como el valor entero 65.</span><span class="sxs-lookup"><span data-stu-id="226ce-699">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="226ce-700">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-700">**New behavior**</span></span>

<span data-ttu-id="226ce-701">Ahora, los valores char se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="226ce-701">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="226ce-702">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-702">**Why**</span></span>

<span data-ttu-id="226ce-703">El almacenamiento de valores como TEXT es más natural y mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="226ce-703">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="226ce-704">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-704">**Mitigations**</span></span>

<span data-ttu-id="226ce-705">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="226ce-705">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="226ce-706">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="226ce-706">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="226ce-707">Microsoft.Data.Sqlite también puede leer valores de caracteres tanto de columnas INTEGER como de columnas TEXT, por lo que es posible que no deba hacer nada dependiendo de su caso.</span><span class="sxs-lookup"><span data-stu-id="226ce-707">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="226ce-708">Ahora los id. de migración se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="226ce-708">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="226ce-709">Problema de seguimiento n.º 12978</span><span class="sxs-lookup"><span data-stu-id="226ce-709">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="226ce-710">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-710">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-711">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-711">**Old behavior**</span></span>

<span data-ttu-id="226ce-712">Los identificadores de migración se generaban de forma involuntaria con el calendario de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="226ce-712">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="226ce-713">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-713">**New behavior**</span></span>

<span data-ttu-id="226ce-714">Ahora los id. de migración siempre se generan usando el calendario de la referencia cultural invariable (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="226ce-714">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="226ce-715">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-715">**Why**</span></span>

<span data-ttu-id="226ce-716">El orden de las migraciones es importante al actualizar la base de datos o al solucionar conflictos de combinación.</span><span class="sxs-lookup"><span data-stu-id="226ce-716">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="226ce-717">Al usar el calendario invariable, se evitan problemas de ordenación que pueden producirse si los miembros del equipo tienen distintos calendarios del sistema.</span><span class="sxs-lookup"><span data-stu-id="226ce-717">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="226ce-718">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-718">**Mitigations**</span></span>

<span data-ttu-id="226ce-719">Esta cambio afecta a todas las personas que usan un calendario no gregoriano en el que el año sea superior al del calendario gregoriano (como el calendario budista tailandés).</span><span class="sxs-lookup"><span data-stu-id="226ce-719">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="226ce-720">Los id. de migración existentes deberán actualizarse para que las migraciones nuevas se ordenen después de las existentes.</span><span class="sxs-lookup"><span data-stu-id="226ce-720">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="226ce-721">Puede ver el id. de migración en el atributo Migration de los archivos de diseñador de la migración.</span><span class="sxs-lookup"><span data-stu-id="226ce-721">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="226ce-722">También debe actualizarse la tabla de historial de migraciones.</span><span class="sxs-lookup"><span data-stu-id="226ce-722">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="226ce-723">Se ha quitado el elemento UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="226ce-723">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="226ce-724">Problema de seguimiento n.º 16400</span><span class="sxs-lookup"><span data-stu-id="226ce-724">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="226ce-725">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="226ce-725">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="226ce-726">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-726">**Old behavior**</span></span>

<span data-ttu-id="226ce-727">Antes de EF Core 3.0, `UseRowNumberForPaging` se podía usar para generar SQL para la paginación de forma que fuera compatible con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="226ce-727">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="226ce-728">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-728">**New behavior**</span></span>

<span data-ttu-id="226ce-729">A partir de EF Core 3.0, EF solo genera SQL para la paginación que únicamente es compatible con las versiones posteriores de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="226ce-729">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="226ce-730">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-730">**Why**</span></span>

<span data-ttu-id="226ce-731">El motivo de este cambio es que [SQL Server 2008 ya no se admite](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/). Además, la actualización de esta característica para que funcionase con los cambios en las consultas implementados en EF Core 3.0 llevaría mucho trabajo.</span><span class="sxs-lookup"><span data-stu-id="226ce-731">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="226ce-732">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-732">**Mitigations**</span></span>

<span data-ttu-id="226ce-733">Se recomienda actualizar a una versión más reciente de SQL Server, o bien utilizar un nivel de compatibilidad superior, de modo que el SQL que se genere se admita.</span><span class="sxs-lookup"><span data-stu-id="226ce-733">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="226ce-734">Dicho esto, si no puede hacerlo, [escriba un comentario en el problema de seguimiento](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con los detalles al respecto.</span><span class="sxs-lookup"><span data-stu-id="226ce-734">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="226ce-735">En función de los comentarios, es posible que volvamos a valorar esta decisión.</span><span class="sxs-lookup"><span data-stu-id="226ce-735">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="226ce-736">La información o metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="226ce-736">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="226ce-737">Problema de seguimiento n.º 16119</span><span class="sxs-lookup"><span data-stu-id="226ce-737">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="226ce-738">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="226ce-738">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="226ce-739">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-739">**Old behavior**</span></span>

<span data-ttu-id="226ce-740">`IDbContextOptionsExtension` incluía métodos para proporcionar metadatos sobre la extensión.</span><span class="sxs-lookup"><span data-stu-id="226ce-740">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="226ce-741">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-741">**New behavior**</span></span>

<span data-ttu-id="226ce-742">Estos métodos se han movido a una nueva clase base abstracta `DbContextOptionsExtensionInfo`, que se devuelve desde una nueva propiedad `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="226ce-742">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="226ce-743">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-743">**Why**</span></span>

<span data-ttu-id="226ce-744">Al lanzarse las versiones 2.0 y 3.0, tuvimos que agregar o cambiar estos métodos varias veces.</span><span class="sxs-lookup"><span data-stu-id="226ce-744">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="226ce-745">Su división en una nueva clase base abstracta facilitará la realización de este tipo de cambios sin interrumpir las extensiones existentes.</span><span class="sxs-lookup"><span data-stu-id="226ce-745">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="226ce-746">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-746">**Mitigations**</span></span>

<span data-ttu-id="226ce-747">Actualice las extensiones para seguir el nuevo patrón.</span><span class="sxs-lookup"><span data-stu-id="226ce-747">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="226ce-748">Encontrará ejemplos en las muchas implementaciones de `IDbContextOptionsExtension` para los diferentes tipos de extensiones en el código fuente de EF Core.</span><span class="sxs-lookup"><span data-stu-id="226ce-748">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="226ce-749">Cambio de nombre de LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="226ce-749">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="226ce-750">Problema de seguimiento n.º 10985</span><span class="sxs-lookup"><span data-stu-id="226ce-750">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="226ce-751">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-751">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-752">**Cambio**</span><span class="sxs-lookup"><span data-stu-id="226ce-752">**Change**</span></span>

<span data-ttu-id="226ce-753">Se ha cambiado el nombre de `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="226ce-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="226ce-754">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-754">**Why**</span></span>

<span data-ttu-id="226ce-755">Conviene alinear el nombre de este evento de advertencia con el del resto de eventos de advertencia.</span><span class="sxs-lookup"><span data-stu-id="226ce-755">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="226ce-756">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-756">**Mitigations**</span></span>

<span data-ttu-id="226ce-757">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="226ce-757">Use the new name.</span></span> <span data-ttu-id="226ce-758">(Tenga en cuenta que el número de id. evento sigue siendo el mismo).</span><span class="sxs-lookup"><span data-stu-id="226ce-758">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="226ce-759">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="226ce-759">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="226ce-760">Problema de seguimiento n.º 10730</span><span class="sxs-lookup"><span data-stu-id="226ce-760">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="226ce-761">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-761">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-762">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-762">**Old behavior**</span></span>

<span data-ttu-id="226ce-763">Antes de EF Core 3.0, se utilizaba simplemente el término "nombre" para hacer referencia a los nombres de las restricciones de claves externas.</span><span class="sxs-lookup"><span data-stu-id="226ce-763">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="226ce-764">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-764">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="226ce-765">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-765">**New behavior**</span></span>

<span data-ttu-id="226ce-766">A partir de EF Core 3.0, el término con el que se hace referencia a los nombres de las restricciones de claves externas es "nombre de la restricción".</span><span class="sxs-lookup"><span data-stu-id="226ce-766">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="226ce-767">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-767">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="226ce-768">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-768">**Why**</span></span>

<span data-ttu-id="226ce-769">Este cambio permite mejorar la coherencia relativa a la nomenclatura en este aspecto y aclarar que se trata del nombre de una restricción de clave externa, y no del de la columna o propiedad en la que está definida la clave externa.</span><span class="sxs-lookup"><span data-stu-id="226ce-769">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="226ce-770">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-770">**Mitigations**</span></span>

<span data-ttu-id="226ce-771">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="226ce-771">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="226ce-772">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="226ce-772">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="226ce-773">Problema de seguimiento n.° 15997</span><span class="sxs-lookup"><span data-stu-id="226ce-773">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="226ce-774">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="226ce-774">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="226ce-775">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-775">**Old behavior**</span></span>

<span data-ttu-id="226ce-776">Antes de EF Core 3.0, estos métodos estaban protegidos.</span><span class="sxs-lookup"><span data-stu-id="226ce-776">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="226ce-777">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-777">**New behavior**</span></span>

<span data-ttu-id="226ce-778">Desde EF Core 3.0, estos métodos son públicos.</span><span class="sxs-lookup"><span data-stu-id="226ce-778">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="226ce-779">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-779">**Why**</span></span>

<span data-ttu-id="226ce-780">EF usa estos métodos para determinar si se ha creado una base de datos, pero está vacía.</span><span class="sxs-lookup"><span data-stu-id="226ce-780">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="226ce-781">Esto también puede resultar útil fuera de EF al determinar si se deben aplicar migraciones o no.</span><span class="sxs-lookup"><span data-stu-id="226ce-781">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="226ce-782">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-782">**Mitigations**</span></span>

<span data-ttu-id="226ce-783">Cambie la accesibilidad de cualquier invalidación.</span><span class="sxs-lookup"><span data-stu-id="226ce-783">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="226ce-784">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="226ce-784">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="226ce-785">Problema de seguimiento n.° 11506</span><span class="sxs-lookup"><span data-stu-id="226ce-785">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="226ce-786">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="226ce-786">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="226ce-787">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-787">**Old behavior**</span></span>

<span data-ttu-id="226ce-788">Antes de EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un paquete NuGet regular con un ensamblado al que podían hacer referencia los proyectos que dependían de él.</span><span class="sxs-lookup"><span data-stu-id="226ce-788">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="226ce-789">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-789">**New behavior**</span></span>

<span data-ttu-id="226ce-790">Desde EF Core 3.0, es un paquete DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="226ce-790">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="226ce-791">Esto significa que la dependencia no fluirá de manera transitiva en otros proyectos y que ya no puede, de forma predeterminada, hacer referencia a su ensamblado.</span><span class="sxs-lookup"><span data-stu-id="226ce-791">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="226ce-792">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-792">**Why**</span></span>

<span data-ttu-id="226ce-793">Este paquete solo está destinado a usarse en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="226ce-793">This package is only intended to be used at design time.</span></span> <span data-ttu-id="226ce-794">Las aplicaciones implementadas no deben hacer referencia al mismo.</span><span class="sxs-lookup"><span data-stu-id="226ce-794">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="226ce-795">Hacer que el paquete sea DevelopmentDependency refuerza esta recomendación.</span><span class="sxs-lookup"><span data-stu-id="226ce-795">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="226ce-796">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-796">**Mitigations**</span></span>

<span data-ttu-id="226ce-797">Si tiene que hacer referencia a este paquete para invalidar el comportamiento en tiempo de diseño de EF Core, puede actualizar los metadatos de elementos PackageReference del proyecto.</span><span class="sxs-lookup"><span data-stu-id="226ce-797">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="226ce-798">Si se hace referencia al paquete de manera transitiva a través de Microsoft.EntityFrameworkCore.Tools, tendrá que agregar una PackageReference explícita al paquete para cambiar sus metadatos.</span><span class="sxs-lookup"><span data-stu-id="226ce-798">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="226ce-799">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="226ce-799">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="226ce-800">Problema de seguimiento n.° 14824</span><span class="sxs-lookup"><span data-stu-id="226ce-800">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="226ce-801">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="226ce-801">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="226ce-802">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-802">**Old behavior**</span></span>

<span data-ttu-id="226ce-803">Microsoft.EntityFrameworkCore.Sqlite dependía anteriormente de la versión 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="226ce-803">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="226ce-804">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-804">**New behavior**</span></span>

<span data-ttu-id="226ce-805">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="226ce-805">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="226ce-806">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-806">**Why**</span></span>

<span data-ttu-id="226ce-807">La versión 2.0.0 de SQLitePCL.raw selecciona .NET Standard 2.0 como destino.</span><span class="sxs-lookup"><span data-stu-id="226ce-807">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="226ce-808">Anteriormente seleccionaba .NET Standard 1.1 como destino, que requería el cierre a gran escala de paquetes transitivos para su funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="226ce-808">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="226ce-809">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-809">**Mitigations**</span></span>

<span data-ttu-id="226ce-810">En la versión 2.0.0 de SQLitePCL.raw se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="226ce-810">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="226ce-811">Consulte las [notas de la versión](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="226ce-811">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="226ce-812">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="226ce-812">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="226ce-813">Problema de seguimiento n.° 14825</span><span class="sxs-lookup"><span data-stu-id="226ce-813">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="226ce-814">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="226ce-814">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="226ce-815">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-815">**Old behavior**</span></span>

<span data-ttu-id="226ce-816">Los paquetes espaciales anteriormente dependían de la versión 1.15.1 de NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="226ce-816">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="226ce-817">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-817">**New behavior**</span></span>

<span data-ttu-id="226ce-818">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="226ce-818">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="226ce-819">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-819">**Why**</span></span>

<span data-ttu-id="226ce-820">La versión 2.0.0 de NetTopologySuite pretende resolver varios problemas de usabilidad que encontraron los usuarios de EF Core.</span><span class="sxs-lookup"><span data-stu-id="226ce-820">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="226ce-821">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-821">**Mitigations**</span></span>

<span data-ttu-id="226ce-822">En la versión 2.0.0 de NetTopologySuite se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="226ce-822">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="226ce-823">Consulte las [notas de la versión](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="226ce-823">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="226ce-824">Se deben configurar varias relaciones de referencia automática ambiguas</span><span class="sxs-lookup"><span data-stu-id="226ce-824">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="226ce-825">Problema de seguimiento n.º 13573</span><span class="sxs-lookup"><span data-stu-id="226ce-825">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="226ce-826">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="226ce-826">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="226ce-827">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="226ce-827">**Old behavior**</span></span>

<span data-ttu-id="226ce-828">Un tipo de entidad con varias propiedades de navegación unidireccional de referencia automática y claves externas coincidentes se configuró incorrectamente como una única relación.</span><span class="sxs-lookup"><span data-stu-id="226ce-828">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="226ce-829">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-829">For example:</span></span>

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

<span data-ttu-id="226ce-830">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="226ce-830">**New behavior**</span></span>

<span data-ttu-id="226ce-831">Este escenario se detecta ahora en la generación del modelo y se produce una excepción que indica que el modelo es ambiguo.</span><span class="sxs-lookup"><span data-stu-id="226ce-831">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="226ce-832">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="226ce-832">**Why**</span></span>

<span data-ttu-id="226ce-833">El modelo resultante era ambiguo, y lo más probable es que sea incorrecto en este caso.</span><span class="sxs-lookup"><span data-stu-id="226ce-833">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="226ce-834">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="226ce-834">**Mitigations**</span></span>

<span data-ttu-id="226ce-835">Utilice la configuración completa de la relación.</span><span class="sxs-lookup"><span data-stu-id="226ce-835">Use full configuration of the relationship.</span></span> <span data-ttu-id="226ce-836">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="226ce-836">For example:</span></span>

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
