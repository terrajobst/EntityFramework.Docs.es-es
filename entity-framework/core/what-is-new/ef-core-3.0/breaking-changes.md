---
title: 'Cambios importantes en EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7cc0bd3946be2e63d9fb46a023bf6abe750ae0e3
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286494"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="eb674-102">Cambios importantes incluidos en EF Core 3.0 (actualmente en versión preliminar)</span><span class="sxs-lookup"><span data-stu-id="eb674-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eb674-103">Tenga en cuenta que los conjuntos de características y las programaciones de las versiones futuras siempre están sujetos a cambios y, aunque intentamos mantener esta página actualizada, es posible que no refleje nuestros planes más recientes en todo momento.</span><span class="sxs-lookup"><span data-stu-id="eb674-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="eb674-104">Es posible que los siguientes cambios de API y comportamiento interrumpan las aplicaciones desarrolladas para EF Core 2.2. x cuando se actualicen a la versión 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="eb674-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="eb674-105">Los cambios que esperamos que solo afecten a proveedores de base de datos se documentan en [Cambios para proveedores](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="eb674-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="eb674-106">Aquí no se documentan los cambios en las características nuevas presentadas de una versión preliminar 3.0 a otra.</span><span class="sxs-lookup"><span data-stu-id="eb674-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="eb674-107">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="eb674-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="eb674-108">[Problema de seguimiento n.° 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Consulte también el problema n.° 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="eb674-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="eb674-109">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-110">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-110">**Old behavior**</span></span>

<span data-ttu-id="eb674-111">Antes de 3.0, cuando en EF Core no se podía convertir una expresión que formaba parte de una consulta SQL o un parámetro, la expresión se evaluaba de forma automática en el cliente.</span><span class="sxs-lookup"><span data-stu-id="eb674-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="eb674-112">De forma predeterminada, la evaluación de cliente de expresiones potencialmente costosas solo desencadenaba una advertencia.</span><span class="sxs-lookup"><span data-stu-id="eb674-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="eb674-113">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-113">**New behavior**</span></span>

<span data-ttu-id="eb674-114">A partir de 3.0, en EF Core solo se permite que se evalúen en el cliente las expresiones en la proyección de nivel superior (la última llamada a `Select()` de la consulta).</span><span class="sxs-lookup"><span data-stu-id="eb674-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="eb674-115">Cuando las expresiones de otra parte de la consulta no se pueden convertir en SQL o un parámetro, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="eb674-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="eb674-116">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-116">**Why**</span></span>

<span data-ttu-id="eb674-117">La evaluación de cliente automática de las consultas permite que se ejecuten muchas consultas incluso si no se pueden convertir elementos importantes de ellas.</span><span class="sxs-lookup"><span data-stu-id="eb674-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="eb674-118">Esto puede provocar un comportamiento inesperado y potencialmente dañino que es posible que solo sea evidente en entornos de producción.</span><span class="sxs-lookup"><span data-stu-id="eb674-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="eb674-119">Por ejemplo, una condición en una llamada a `Where()` que no se puede convertir puede provocar que todas las filas de la tabla se transfieran desde el servidor de base de datos y que el filtro se aplique en el cliente.</span><span class="sxs-lookup"><span data-stu-id="eb674-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="eb674-120">Esta situación puede pasar desapercibida fácilmente si la tabla solo contiene algunas filas en la fase de desarrollo, pero ser más grave cuando la aplicación pase a producción, donde la tabla puede contener millones de filas.</span><span class="sxs-lookup"><span data-stu-id="eb674-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="eb674-121">Las advertencias de evaluación de cliente también se suelen pasar por alto durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="eb674-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="eb674-122">Además de esto, la evaluación de cliente automática puede causar problemas en los que la mejora de la traducción de consultas para expresiones específicas provocaba cambios importantes no deseados entre versiones.</span><span class="sxs-lookup"><span data-stu-id="eb674-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="eb674-123">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-123">**Mitigations**</span></span>

<span data-ttu-id="eb674-124">Si una consulta no se puede traducir totalmente, vuelva a escribirla en un formato que se pueda traducir, o bien use `AsEnumerable()`, `ToList()` o una función similar para devolver los datos al cliente de forma explícita, donde después se puedan seguir procesando mediante LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="eb674-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="eb674-125">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eb674-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="eb674-126">Anuncios del problema de seguimiento n.º 325</span><span class="sxs-lookup"><span data-stu-id="eb674-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="eb674-127">Este cambio se ha introducido en ASP.NET Core 3.0 (versión preliminar 1).</span><span class="sxs-lookup"><span data-stu-id="eb674-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="eb674-128">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-128">**Old behavior**</span></span>

<span data-ttu-id="eb674-129">Antes de ASP.NET Core 3.0, cuando se agregaba una referencia de paquete a `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, se incluía EF Core y algunos de los proveedores de datos de EF Core, como el de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="eb674-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="eb674-130">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-130">**New behavior**</span></span>

<span data-ttu-id="eb674-131">A partir de la versión 3.0, el marco compartido ASP.NET Core no incluye EF Core ni ningún proveedor de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="eb674-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="eb674-132">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-132">**Why**</span></span>

<span data-ttu-id="eb674-133">Antes de este cambio, para obtener EF Core se necesitaban varios pasos en función de si la aplicación se destinaba a ASP.NET Core y SQL Server o no.</span><span class="sxs-lookup"><span data-stu-id="eb674-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="eb674-134">Además, la actualización de ASP.NET Core forzaba la de EF Core y el proveedor de SQL Server, lo que no siempre es deseable.</span><span class="sxs-lookup"><span data-stu-id="eb674-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="eb674-135">Con este cambio, la experiencia de obtención de EF Core es la misma en todos los proveedores, implementaciones admitidas de .NET y tipos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb674-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="eb674-136">Ahora los desarrolladores también pueden controlar exactamente cuándo se actualizan EF Core y los proveedores de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="eb674-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="eb674-137">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-137">**Mitigations**</span></span>

<span data-ttu-id="eb674-138">Para usar EF Core en una aplicación ASP.NET Core 3.0 o cualquier otra aplicación compatible, debe agregar de forma explícita una referencia de paquete al proveedor de base de datos de EF Core que se va a usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb674-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="eb674-139">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="eb674-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="eb674-140">Problema de seguimiento n.º 14016</span><span class="sxs-lookup"><span data-stu-id="eb674-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="eb674-141">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4) y la versión correspondiente del SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb674-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="eb674-142">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-142">**Old behavior**</span></span>

<span data-ttu-id="eb674-143">Antes de 3.0, la herramienta `dotnet ef` se incluía en el SDK de .NET Core y estaba disponible para usarse desde la línea de comandos de cualquier proyecto sin necesidad de realizar pasos adicionales.</span><span class="sxs-lookup"><span data-stu-id="eb674-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="eb674-144">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-144">**New behavior**</span></span>

<span data-ttu-id="eb674-145">A partir de la versión 3.0, el SDK de .NET no incluye la herramienta `dotnet ef`, por lo que antes de poder usarla tendrá que instalarla de forma explícita como una herramienta local o global.</span><span class="sxs-lookup"><span data-stu-id="eb674-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="eb674-146">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-146">**Why**</span></span>

<span data-ttu-id="eb674-147">Este cambio nos permite distribuir y actualizar `dotnet ef` como una herramienta convencional de la CLI de .NET en NuGet, coherente con el hecho de que la versión 3.0 de EF Core también se distribuye siempre como un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="eb674-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="eb674-148">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-148">**Mitigations**</span></span>

<span data-ttu-id="eb674-149">Para poder administrar las migraciones o aplicar scaffolding a `DbContext`, instale `dotnet-ef` como herramienta global:</span><span class="sxs-lookup"><span data-stu-id="eb674-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="eb674-150">También se puede obtener una herramienta local cuando se restauran las dependencias de un proyecto que la declara como una dependencia de herramientas mediante un [archivo de manifiesto de herramientas](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="eb674-150">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="eb674-151">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="eb674-151">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="eb674-152">Problema de seguimiento n.º 10996</span><span class="sxs-lookup"><span data-stu-id="eb674-152">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="eb674-153">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-153">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-154">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-154">**Old behavior**</span></span>

<span data-ttu-id="eb674-155">Antes de EF Core 3.0, estos nombres de métodos se sobrecargaban para funcionar tanto con una cadena normal como con una cadena que se debería interpolar en SQL y parámetros.</span><span class="sxs-lookup"><span data-stu-id="eb674-155">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="eb674-156">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-156">**New behavior**</span></span>

<span data-ttu-id="eb674-157">A partir de la versión EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw` y `ExecuteSqlRawAsync` para crear una consulta con parámetros donde los parámetros se pasan por separado de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="eb674-157">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="eb674-158">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-158">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="eb674-159">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated` y `ExecuteSqlInterpolatedAsync` para crear una consulta con parámetros donde los parámetros se pasan como parte de una cadena de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="eb674-159">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="eb674-160">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-160">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="eb674-161">Tenga en cuenta que las dos consultas anteriores producirán el mismo código SQL parametrizado con los mismos parámetros SQL.</span><span class="sxs-lookup"><span data-stu-id="eb674-161">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="eb674-162">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-162">**Why**</span></span>

<span data-ttu-id="eb674-163">Las sobrecargas del método como esta facilitan las llamadas accidentales al método de cadena sin procesar cuando la intención era llamar al método de cadena interpolada y viceversa.</span><span class="sxs-lookup"><span data-stu-id="eb674-163">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="eb674-164">Esto podría resultar en consultas que no se parametrizan cuando deberían.</span><span class="sxs-lookup"><span data-stu-id="eb674-164">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="eb674-165">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-165">**Mitigations**</span></span>

<span data-ttu-id="eb674-166">Haga el cambio para usar los nuevos nombres de métodos.</span><span class="sxs-lookup"><span data-stu-id="eb674-166">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="eb674-167">Solo se pueden especificar métodos de FromSql en raíces de consulta.</span><span class="sxs-lookup"><span data-stu-id="eb674-167">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="eb674-168">Problema de seguimiento n.° 15704</span><span class="sxs-lookup"><span data-stu-id="eb674-168">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="eb674-169">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="eb674-169">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="eb674-170">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-170">**Old behavior**</span></span>

<span data-ttu-id="eb674-171">Antes de EF Core 3.0, el método `FromSql` podía especificarse en cualquier lugar en la consulta.</span><span class="sxs-lookup"><span data-stu-id="eb674-171">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="eb674-172">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-172">**New behavior**</span></span>

<span data-ttu-id="eb674-173">A partir de EF Core 3.0, los nuevos métodos `FromSqlRaw` y `FromSqlInterpolated` (que reemplazan a `FromSql`) solo pueden especificarse en las raíces de la consulta, es decir, directamente en `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="eb674-173">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="eb674-174">Si intenta especificarlos en cualquier otro lugar se producirá un error de compilación.</span><span class="sxs-lookup"><span data-stu-id="eb674-174">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="eb674-175">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-175">**Why**</span></span>

<span data-ttu-id="eb674-176">La especificación de `FromSql` en cualquier otro lugar diferente de `DbSet` no tenía un significado o valor agregado, y podría causar ambigüedad en ciertos escenarios.</span><span class="sxs-lookup"><span data-stu-id="eb674-176">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="eb674-177">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-177">**Mitigations**</span></span>

<span data-ttu-id="eb674-178">Las invocaciones de `FromSql` se deben mover para que estén directamente en el `DbSet` al que se aplican.</span><span class="sxs-lookup"><span data-stu-id="eb674-178">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="eb674-179">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="eb674-179">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="eb674-180">Problema de seguimiento n.º 14523</span><span class="sxs-lookup"><span data-stu-id="eb674-180">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="eb674-181">Este cambio se revirtió en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="eb674-181">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="eb674-182">Revertimos este cambio porque la nueva configuración de EF Core 3.0 permite a la aplicación especificar el nivel de registro para cualquier evento.</span><span class="sxs-lookup"><span data-stu-id="eb674-182">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="eb674-183">Por ejemplo, para cambiar el registro de SQL a `Debug`, configure el nivel de forma explícita en `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="eb674-183">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="eb674-184">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="eb674-184">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="eb674-185">Problema de seguimiento n.º 12378</span><span class="sxs-lookup"><span data-stu-id="eb674-185">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="eb674-186">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="eb674-186">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="eb674-187">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-187">**Old behavior**</span></span>

<span data-ttu-id="eb674-188">Antes de EF Core 3.0, los valores temporales se asignaban a todas las propiedades de clave para las que posteriormente la base de datos generaba un valor real.</span><span class="sxs-lookup"><span data-stu-id="eb674-188">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="eb674-189">Normalmente, estos valores temporales eran números negativos grandes.</span><span class="sxs-lookup"><span data-stu-id="eb674-189">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="eb674-190">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-190">**New behavior**</span></span>

<span data-ttu-id="eb674-191">A partir de la versión 3.0, en EF Core se almacena el valor de clave temporal como parte de la información de seguimiento de la entidad y la propiedad clave en sí no se modifica.</span><span class="sxs-lookup"><span data-stu-id="eb674-191">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="eb674-192">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-192">**Why**</span></span>

<span data-ttu-id="eb674-193">Este cambio se ha realizado para evitar que los valores de clave temporales se conviertan erróneamente en permanentes cuando una entidad de la que previamente una instancia de `DbContext` ha realizado el seguimiento se mueve a otra instancia de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="eb674-193">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="eb674-194">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-194">**Mitigations**</span></span>

<span data-ttu-id="eb674-195">Las aplicaciones que asignan valores de clave principal a claves externas para crear asociaciones entre entidades pueden depender del comportamiento anterior si las claves principales son generadas por el almacén y pertenecen a entidades en el estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="eb674-195">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="eb674-196">Esto se puede evitar con las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="eb674-196">This can be avoided by:</span></span>
* <span data-ttu-id="eb674-197">No se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="eb674-197">Not using store-generated keys.</span></span>
* <span data-ttu-id="eb674-198">Se establecen propiedades de navegación para crear relaciones en lugar de establecer valores de clave externa.</span><span class="sxs-lookup"><span data-stu-id="eb674-198">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="eb674-199">Se obtienen los valores de clave temporal reales de la información de seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="eb674-199">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="eb674-200">Por ejemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` devolverá el valor temporal aunque no se haya establecido `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="eb674-200">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="eb674-201">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="eb674-201">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="eb674-202">Problema de seguimiento n.º 14616</span><span class="sxs-lookup"><span data-stu-id="eb674-202">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="eb674-203">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="eb674-203">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="eb674-204">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-204">**Old behavior**</span></span>

<span data-ttu-id="eb674-205">Antes de EF Core 3.0, se realizaba el seguimiento en el estado `Added` de las entidades sin seguimiento detectadas por `DetectChanges` y se insertaban como una fila nueva cuando se llamaba a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="eb674-205">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="eb674-206">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-206">**New behavior**</span></span>

<span data-ttu-id="eb674-207">A partir de EF Core 3.0, si una entidad usa valores de clave generados y se establece un valor de clave, se realizará el seguimiento de la entidad en el estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="eb674-207">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="eb674-208">Esto significa que se supone que existe una fila para la entidad y que se actualizará cuando se llame a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="eb674-208">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="eb674-209">Si no se establece el valor de clave, o bien si el tipo de entidad no usa claves generadas, se seguirá realizando el seguimiento de la entidad nueva como `Added` al igual que en las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="eb674-209">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="eb674-210">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-210">**Why**</span></span>

<span data-ttu-id="eb674-211">Este cambio se ha realizado para que sea más sencillo y coherente trabajar con gráficos de entidades desconectadas mientras se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="eb674-211">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="eb674-212">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-212">**Mitigations**</span></span>

<span data-ttu-id="eb674-213">Este cambio puede interrumpir una aplicación si se configura un tipo de entidad para usar claves generadas, pero se establecen de forma explícita valores de clave para las instancias nuevas.</span><span class="sxs-lookup"><span data-stu-id="eb674-213">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="eb674-214">La solución consiste en configurar de forma explícita las propiedades de clave para que no usen valores generados.</span><span class="sxs-lookup"><span data-stu-id="eb674-214">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="eb674-215">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="eb674-215">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="eb674-216">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="eb674-216">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="eb674-217">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="eb674-217">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="eb674-218">Problema de seguimiento n.º 10114</span><span class="sxs-lookup"><span data-stu-id="eb674-218">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="eb674-219">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="eb674-219">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="eb674-220">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-220">**Old behavior**</span></span>

<span data-ttu-id="eb674-221">Antes de la versión 3.0, en EF Core no se aplicaban acciones en cascada (eliminación de entidades dependientes cuando se eliminaba una entidad de seguridad obligatoria o cuando se rompía la relación con una entidad de seguridad obligatoria) hasta que se llamaba a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="eb674-221">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="eb674-222">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-222">**New behavior**</span></span>

<span data-ttu-id="eb674-223">A partir de 3.0, en EF Core las acciones en cascada se aplican en cuanto se detecta la condición desencadenadora.</span><span class="sxs-lookup"><span data-stu-id="eb674-223">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="eb674-224">Por ejemplo, como resultado de la llamada a `context.Remove()` para eliminar una entidad de seguridad, todos los dependientes obligatorios relacionados de los que se realiza el seguimiento también se establecen en `Deleted` inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="eb674-224">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="eb674-225">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-225">**Why**</span></span>

<span data-ttu-id="eb674-226">Este cambio se ha realizado para mejorar la experiencia en escenarios de auditoría y enlace de datos donde es importante comprender qué entidades se van a eliminar _antes_ de llamar a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="eb674-226">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="eb674-227">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-227">**Mitigations**</span></span>

<span data-ttu-id="eb674-228">El comportamiento anterior se puede restaurar mediante opciones de `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="eb674-228">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="eb674-229">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-229">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="eb674-230">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="eb674-230">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="eb674-231">Problema de seguimiento n.º 12661</span><span class="sxs-lookup"><span data-stu-id="eb674-231">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="eb674-232">Este cambio se introdujo en EF Core 3.0 (versión preliminar 5).</span><span class="sxs-lookup"><span data-stu-id="eb674-232">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="eb674-233">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-233">**Old behavior**</span></span>

<span data-ttu-id="eb674-234">Antes de la versión 3.0, `DeleteBehavior.Restrict` creaba claves externas en la base de datos con la semántica `Restrict`, pero también realizaba una corrección interna de manera no evidente.</span><span class="sxs-lookup"><span data-stu-id="eb674-234">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="eb674-235">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-235">**New behavior**</span></span>

<span data-ttu-id="eb674-236">A partir de la versión 3.0, `DeleteBehavior.Restrict` garantiza que las claves externas se crean con la semántica `Restrict`, es decir, sin cascadas y realizando una infracción de restricción, sin llevar a cabo correcciones internas de EF.</span><span class="sxs-lookup"><span data-stu-id="eb674-236">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="eb674-237">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-237">**Why**</span></span>

<span data-ttu-id="eb674-238">Este cambio se realizó para mejorar la experiencia de uso de `DeleteBehavior` de manera intuitiva sin efectos secundarios inesperados.</span><span class="sxs-lookup"><span data-stu-id="eb674-238">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="eb674-239">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-239">**Mitigations**</span></span>

<span data-ttu-id="eb674-240">El comportamiento anterior se puede restaurar con `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="eb674-240">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="eb674-241">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="eb674-241">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="eb674-242">Problema de seguimiento n.º 14194</span><span class="sxs-lookup"><span data-stu-id="eb674-242">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="eb674-243">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="eb674-243">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="eb674-244">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-244">**Old behavior**</span></span>

<span data-ttu-id="eb674-245">Antes de EF Core 3.0, los [tipos de consulta](xref:core/modeling/query-types) eran un medio para consultar los datos que no definen una clave principal de una manera estructurada.</span><span class="sxs-lookup"><span data-stu-id="eb674-245">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="eb674-246">Es decir, un tipo de consulta se usaba para asignar tipos de entidad sin claves (más probablemente desde una vista, pero posiblemente desde una tabla), mientras que un tipo de entidad estándar se usaba cuando había una clave disponible (más probablemente desde una tabla, pero posiblemente desde una vista).</span><span class="sxs-lookup"><span data-stu-id="eb674-246">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="eb674-247">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-247">**New behavior**</span></span>

<span data-ttu-id="eb674-248">Ahora un tipo de consulta se convierte en un tipo de entidad sin una clave principal.</span><span class="sxs-lookup"><span data-stu-id="eb674-248">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="eb674-249">Los tipos de entidad sin clave tienen la misma funcionalidad que los tipos de consulta de las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="eb674-249">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="eb674-250">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-250">**Why**</span></span>

<span data-ttu-id="eb674-251">Este cambio se ha realizado para reducir la confusión en torno a la finalidad de los tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="eb674-251">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="eb674-252">En concreto, son tipos de entidad sin clave y, por ello, intrínsecamente son de solo lectura, pero no se deben usar solo porque un tipo de entidad tenga que ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="eb674-252">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="eb674-253">Del mismo modo, se suelen asignar a vistas, pero solo porque las vistas no suelen definir claves.</span><span class="sxs-lookup"><span data-stu-id="eb674-253">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="eb674-254">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-254">**Mitigations**</span></span>

<span data-ttu-id="eb674-255">Los elementos siguientes de la API ahora están obsoletos:</span><span class="sxs-lookup"><span data-stu-id="eb674-255">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="eb674-256">**`ModelBuilder.Query<>()`** : en su lugar es necesario llamar a `ModelBuilder.Entity<>().HasNoKey()` para marcar un tipo de entidad como sin claves.</span><span class="sxs-lookup"><span data-stu-id="eb674-256">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="eb674-257">Esto todavía no se configurará por convención para evitar una configuración incorrecta cuando se espera una clave principal, pero no coincide con la convención.</span><span class="sxs-lookup"><span data-stu-id="eb674-257">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="eb674-258">**`DbQuery<>`** : en su lugar se debe usar `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="eb674-258">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="eb674-259">**`DbContext.Query<>()`** : en su lugar se debe usar `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="eb674-259">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="eb674-260">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="eb674-260">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="eb674-261">[Problema de seguimiento n.º 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema de seguimiento n.º 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema de seguimiento n.º 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="eb674-261">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="eb674-262">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="eb674-262">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="eb674-263">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-263">**Old behavior**</span></span>

<span data-ttu-id="eb674-264">Antes de EF Core 3.0, la configuración de la relación de propiedad se realizaba directamente después de la llamada a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="eb674-264">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="eb674-265">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-265">**New behavior**</span></span>

<span data-ttu-id="eb674-266">A partir de EF Core 3.0, ahora hay una API fluida para configurar una propiedad de navegación para el propietario mediante `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="eb674-266">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="eb674-267">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-267">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="eb674-268">La configuración relacionada con la relación entre el propietario y lo que se posee ahora se debe encadenar después de `WithOwner()`, de forma similar a cómo se configuran otras relaciones.</span><span class="sxs-lookup"><span data-stu-id="eb674-268">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="eb674-269">Pero la configuración del propio tipo de propiedad se seguirá encadenando después de `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="eb674-269">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="eb674-270">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-270">For example:</span></span>

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

<span data-ttu-id="eb674-271">Además, la llamada a `Entity()`, `HasOne()` o `Set()` con un tipo de propiedad de destino ahora iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="eb674-271">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="eb674-272">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-272">**Why**</span></span>

<span data-ttu-id="eb674-273">Este cambio se ha realizado para crear una separación más clara entre la configuración del propio tipo de propiedad y la _relación con_ el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="eb674-273">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="eb674-274">A su vez, esto elimina la ambigüedad y la confusión de métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="eb674-274">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="eb674-275">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-275">**Mitigations**</span></span>

<span data-ttu-id="eb674-276">Cambie la configuración de las relaciones de tipo de propiedad para usar la nueva superficie de API, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="eb674-276">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="eb674-277">Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales</span><span class="sxs-lookup"><span data-stu-id="eb674-277">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="eb674-278">Problema de seguimiento n.º 9005</span><span class="sxs-lookup"><span data-stu-id="eb674-278">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="eb674-279">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-279">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-280">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-280">**Old behavior**</span></span>

<span data-ttu-id="eb674-281">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb674-281">Consider the following model:</span></span>
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
<span data-ttu-id="eb674-282">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, siempre era necesaria una instancia de `OrderDetails` al agregar un elemento `Order` nuevo.</span><span class="sxs-lookup"><span data-stu-id="eb674-282">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="eb674-283">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-283">**New behavior**</span></span>

<span data-ttu-id="eb674-284">A partir de la versión 3.0, EF Core permite agregar `Order` sin `OrderDetails` y asigna todas las propiedades `OrderDetails` a excepción de la clave principal a columnas que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="eb674-284">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="eb674-285">Al realizar consultas, EF Core establece `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor o si no tiene propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.</span><span class="sxs-lookup"><span data-stu-id="eb674-285">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="eb674-286">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-286">**Mitigations**</span></span>

<span data-ttu-id="eb674-287">Si el modelo tiene una tabla que comparte dependencias con todas las columnas opcionales, pero la navegación que apunta a ella no se espera que sea `null`, la aplicación debería modificarse para controlar los casos en los que la navegación sea `null`.</span><span class="sxs-lookup"><span data-stu-id="eb674-287">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="eb674-288">Si esto no es posible, debería agregarse una propiedad necesaria al tipo de entidad o, al menos, una entidad debería tener un valor distinto a `null` asignado.</span><span class="sxs-lookup"><span data-stu-id="eb674-288">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="eb674-289">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="eb674-289">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="eb674-290">Problema de seguimiento n.º 14154</span><span class="sxs-lookup"><span data-stu-id="eb674-290">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="eb674-291">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-291">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-292">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-292">**Old behavior**</span></span>

<span data-ttu-id="eb674-293">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb674-293">Consider the following model:</span></span>
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
<span data-ttu-id="eb674-294">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, si solo se actualizaba `OrderDetails`, no se actualizaba el valor `Version` en el cliente y se producía un error en la próxima actualización.</span><span class="sxs-lookup"><span data-stu-id="eb674-294">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="eb674-295">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-295">**New behavior**</span></span>

<span data-ttu-id="eb674-296">A partir de la versión 3.0, EF Core propaga el nuevo valor `Version` en `Order` si posee `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="eb674-296">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="eb674-297">En caso contrario, se produce una excepción durante la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="eb674-297">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="eb674-298">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-298">**Why**</span></span>

<span data-ttu-id="eb674-299">Este cambio se realizó para evitar un valor de token de simultaneidad obsoleto cuando solo se actualiza una de las entidades asignadas a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="eb674-299">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="eb674-300">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-300">**Mitigations**</span></span>

<span data-ttu-id="eb674-301">Todas las entidades que comparten la tabla deben incluir una propiedad que se asigna a la columna del token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="eb674-301">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="eb674-302">Es posible crear una en estado reemplazado:</span><span class="sxs-lookup"><span data-stu-id="eb674-302">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="eb674-303">Ahora, las propiedades heredadas de tipos sin asignar se asignan a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="eb674-303">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="eb674-304">Problema de seguimiento n.º 13998</span><span class="sxs-lookup"><span data-stu-id="eb674-304">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="eb674-305">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-305">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-306">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-306">**Old behavior**</span></span>

<span data-ttu-id="eb674-307">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb674-307">Consider the following model:</span></span>
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

<span data-ttu-id="eb674-308">Antes de EF Core 3.0, la propiedad `ShippingAddress` se asignaba a columnas distintas para `BulkOrder` y `Order` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="eb674-308">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="eb674-309">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-309">**New behavior**</span></span>

<span data-ttu-id="eb674-310">A partir de la versión3.0, EF Core solo crea una columna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="eb674-310">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="eb674-311">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-311">**Why**</span></span>

<span data-ttu-id="eb674-312">El comportamiento anterior no era el esperado.</span><span class="sxs-lookup"><span data-stu-id="eb674-312">The old behavoir was unexpected.</span></span>

<span data-ttu-id="eb674-313">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-313">**Mitigations**</span></span>

<span data-ttu-id="eb674-314">Todavía se puede asignar explícitamente la propiedad a columnas separadas en los tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="eb674-314">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="eb674-315">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="eb674-315">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="eb674-316">Problema de seguimiento n.º 13274</span><span class="sxs-lookup"><span data-stu-id="eb674-316">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="eb674-317">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="eb674-317">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="eb674-318">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-318">**Old behavior**</span></span>

<span data-ttu-id="eb674-319">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="eb674-319">Consider the following model:</span></span>
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
<span data-ttu-id="eb674-320">Antes de EF Core 3.0, se podía usar la propiedad `CustomerId` para la clave externa por convención.</span><span class="sxs-lookup"><span data-stu-id="eb674-320">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="eb674-321">Pero si `Order` es un tipo de propiedad, entonces esto convertiría también a `CustomerId` en la clave principal, algo que no suele ser lo esperado.</span><span class="sxs-lookup"><span data-stu-id="eb674-321">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="eb674-322">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-322">**New behavior**</span></span>

<span data-ttu-id="eb674-323">A partir de la versión 3.0, EF Core no intenta usar las propiedades de claves externas por convención si tienen el mismo nombre que la propiedad de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="eb674-323">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="eb674-324">Los patrones de nombre de tipo de entidad de seguridad concatenado con el nombre de propiedad de la entidad de seguridad y de nombre de navegación concatenado con el nombre de propiedad de la entidad de seguridad todavía se hacen coincidir.</span><span class="sxs-lookup"><span data-stu-id="eb674-324">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="eb674-325">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-325">For example:</span></span>

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

<span data-ttu-id="eb674-326">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-326">**Why**</span></span>

<span data-ttu-id="eb674-327">Este cambio se ha realizado para evitar definir erróneamente una propiedad de clave principal en el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="eb674-327">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="eb674-328">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-328">**Mitigations**</span></span>

<span data-ttu-id="eb674-329">Si la propiedad se ha diseñado para ser la clave externa y, por tanto, parte de la clave principal, se debe configurar explícitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="eb674-329">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="eb674-330">Ahora, la conexión de base de datos se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="eb674-330">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="eb674-331">Problema de seguimiento n.º 14218</span><span class="sxs-lookup"><span data-stu-id="eb674-331">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="eb674-332">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-332">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-333">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-333">**Old behavior**</span></span>

<span data-ttu-id="eb674-334">Antes de EF Core 3.0, si el contexto abría la conexión dentro de `TransactionScope`, la conexión permanecía abierta mientras el ámbito actual `TransactionScope` estuviese activo.</span><span class="sxs-lookup"><span data-stu-id="eb674-334">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="eb674-335">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-335">**New behavior**</span></span>

<span data-ttu-id="eb674-336">A partir de la versión 3.0, EF Core cierra la conexión en cuanto se deja de usar.</span><span class="sxs-lookup"><span data-stu-id="eb674-336">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="eb674-337">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-337">**Why**</span></span>

<span data-ttu-id="eb674-338">Este cambio permite usar varios contextos en el mismo ámbito `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="eb674-338">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="eb674-339">El comportamiento nuevo también coincide con el de EF6.</span><span class="sxs-lookup"><span data-stu-id="eb674-339">The new behavior also matches EF6.</span></span>

<span data-ttu-id="eb674-340">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-340">**Mitigations**</span></span>

<span data-ttu-id="eb674-341">Si la conexión debe permanecer abierta, una llamada explícita a `OpenConnection()` asegurará que EF Core no la cierre de forma prematura:</span><span class="sxs-lookup"><span data-stu-id="eb674-341">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="eb674-342">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="eb674-342">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="eb674-343">Problema de seguimiento n.º 6872</span><span class="sxs-lookup"><span data-stu-id="eb674-343">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="eb674-344">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-344">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-345">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-345">**Old behavior**</span></span>

<span data-ttu-id="eb674-346">Antes de EF Core 3.0, se usaba un generador de valores compartidos para todas las propiedades de clave entera en memoria.</span><span class="sxs-lookup"><span data-stu-id="eb674-346">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="eb674-347">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-347">**New behavior**</span></span>

<span data-ttu-id="eb674-348">A partir de EF Core 3.0, cada propiedad de clave entera obtiene su propio generador de valores cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="eb674-348">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="eb674-349">Además, si se elimina la base de datos, la generación de claves se restablece para todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="eb674-349">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="eb674-350">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-350">**Why**</span></span>

<span data-ttu-id="eb674-351">Este cambio se ha realizado para alinear la generación de claves en memoria más estrechamente a la generación de claves de base de datos reales y para mejorar la capacidad para aislar las pruebas entre sí cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="eb674-351">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="eb674-352">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-352">**Mitigations**</span></span>

<span data-ttu-id="eb674-353">Esto puede interrumpir una aplicación que se base en el establecimiento de valores de clave específicos en memoria.</span><span class="sxs-lookup"><span data-stu-id="eb674-353">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="eb674-354">En su lugar, considere la posibilidad de no depender de valores de clave específicos, o bien de actualizar para que coincida con el comportamiento nuevo.</span><span class="sxs-lookup"><span data-stu-id="eb674-354">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="eb674-355">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="eb674-355">Backing fields are used by default</span></span>

[<span data-ttu-id="eb674-356">Problema de seguimiento n.º 12430</span><span class="sxs-lookup"><span data-stu-id="eb674-356">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="eb674-357">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="eb674-357">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="eb674-358">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-358">**Old behavior**</span></span>

<span data-ttu-id="eb674-359">Antes de la versión 3.0, incluso si se conocía el campo de respaldo de una propiedad, de forma predeterminada en EF Core se leía y escribía el valor de propiedad mediante los métodos captadores y establecedores de propiedades.</span><span class="sxs-lookup"><span data-stu-id="eb674-359">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="eb674-360">La excepción era la ejecución de consultas, donde el campo de respaldo se establecía directamente si se conocía.</span><span class="sxs-lookup"><span data-stu-id="eb674-360">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="eb674-361">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-361">**New behavior**</span></span>

<span data-ttu-id="eb674-362">A partir de EF Core 3.0, si se conoce el campo de respaldo para una propiedad, EF Core siempre la leerá y escribirá mediante el campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="eb674-362">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="eb674-363">Esto podría provocar una interrupción de la aplicación si depende de un comportamiento adicional codificado en los métodos captadores o establecedores.</span><span class="sxs-lookup"><span data-stu-id="eb674-363">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="eb674-364">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-364">**Why**</span></span>

<span data-ttu-id="eb674-365">Este cambio se ha realizado para evitar que EF Core desencadene erróneamente lógica de negocios de forma predeterminada al realizar operaciones de base de datos que implican entidades.</span><span class="sxs-lookup"><span data-stu-id="eb674-365">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="eb674-366">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-366">**Mitigations**</span></span>

<span data-ttu-id="eb674-367">El comportamiento anterior a la versión 3.0 se puede restaurar mediante la configuración del modo de acceso de propiedad en `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="eb674-367">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="eb674-368">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-368">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="eb674-369">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="eb674-369">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="eb674-370">Problema de seguimiento n.º 12523</span><span class="sxs-lookup"><span data-stu-id="eb674-370">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="eb674-371">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-371">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-372">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-372">**Old behavior**</span></span>

<span data-ttu-id="eb674-373">Antes de EF Core 3.0, si varios campos coincidían con las reglas para buscar el campo de respaldo de una propiedad, se elegía un campo según un orden de prioridad.</span><span class="sxs-lookup"><span data-stu-id="eb674-373">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="eb674-374">Esto podía producir que, en caso de ambigüedad, se usara el campo incorrecto.</span><span class="sxs-lookup"><span data-stu-id="eb674-374">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="eb674-375">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-375">**New behavior**</span></span>

<span data-ttu-id="eb674-376">A partir de EF Core 3.0, si varios campos coinciden con la misma propiedad, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="eb674-376">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="eb674-377">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-377">**Why**</span></span>

<span data-ttu-id="eb674-378">Este cambio se ha realizado para evitar de forma silenciosa el uso de un campo con respecto a otro cuando solo uno puede ser correcto.</span><span class="sxs-lookup"><span data-stu-id="eb674-378">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="eb674-379">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-379">**Mitigations**</span></span>

<span data-ttu-id="eb674-380">En las propiedades con campos de respaldo ambiguos se debe especificar de forma explícita el campo que se va usar.</span><span class="sxs-lookup"><span data-stu-id="eb674-380">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="eb674-381">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="eb674-381">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="eb674-382">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="eb674-382">Field-only property names should match the field name</span></span>

<span data-ttu-id="eb674-383">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-383">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-384">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-384">**Old behavior**</span></span>

<span data-ttu-id="eb674-385">Antes de EF Core 3.0, una propiedad podía especificarse con un valor de cadena y, si no había ninguna propiedad con ese nombre en el tipo CLR, EF Core intentaba hacerla coincidir con un campo mediante reglas de convención.</span><span class="sxs-lookup"><span data-stu-id="eb674-385">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="eb674-386">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-386">**New behavior**</span></span>

<span data-ttu-id="eb674-387">A partir de EF Core 3.0, una propiedad de solo campo debe coincidir exactamente con el nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="eb674-387">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="eb674-388">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-388">**Why**</span></span>

<span data-ttu-id="eb674-389">Este cambio se realizó para evitar el uso del mismo campo para dos propiedades con nombres similares. También hace que las reglas de coincidencia para propiedades solo de campo sean las mismas que para las propiedades asignadas a propiedades CLR.</span><span class="sxs-lookup"><span data-stu-id="eb674-389">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="eb674-390">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-390">**Mitigations**</span></span>

<span data-ttu-id="eb674-391">Las propiedades solo de campo deberían tener el mismo nombre que el campo al que están asignadas.</span><span class="sxs-lookup"><span data-stu-id="eb674-391">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="eb674-392">En una próxima versión preliminar de EF Core 3.0 tenemos planeado volver a habilitar la configuración explícita de un nombre de campo distinto al nombre de la propiedad:</span><span class="sxs-lookup"><span data-stu-id="eb674-392">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="eb674-393">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="eb674-393">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="eb674-394">Problema de seguimiento n.° 14756</span><span class="sxs-lookup"><span data-stu-id="eb674-394">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="eb674-395">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-395">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-396">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-396">**Old behavior**</span></span>

<span data-ttu-id="eb674-397">Antes de EF Core 3.0, llamar a `AddDbContext` o a `AddDbContextPool` también podría registrar los servicios de almacenamiento en caché y de registro con D.I. a través de llamadas a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) y a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="eb674-397">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="eb674-398">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-398">**New behavior**</span></span>

<span data-ttu-id="eb674-399">A partir de EF Core 3.0, `AddDbContext` y `AddDbContextPool` ya no registrarán estos servicios con inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="eb674-399">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="eb674-400">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-400">**Why**</span></span>

<span data-ttu-id="eb674-401">EF Core 3.0 no requiere que estos servicios estén en el contenedor de inserción de dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb674-401">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="eb674-402">Pero si `ILoggerFactory` se registra en el contenedor de DI de la aplicación, EF Core lo empezará a usar de todos modos.</span><span class="sxs-lookup"><span data-stu-id="eb674-402">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="eb674-403">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-403">**Mitigations**</span></span>

<span data-ttu-id="eb674-404">Si la aplicación necesita estos servicios, regístrelos de manera explícita con el contenedor de DI mediante [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="eb674-404">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="eb674-405">Ahora DbContext.Entry realiza una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="eb674-405">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="eb674-406">Problema de seguimiento n.º 13552</span><span class="sxs-lookup"><span data-stu-id="eb674-406">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="eb674-407">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="eb674-407">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="eb674-408">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-408">**Old behavior**</span></span>

<span data-ttu-id="eb674-409">Antes de EF Core 3.0, la llamada a `DbContext.Entry` provocaba que se detectaran cambios para todas las entidades con seguimiento.</span><span class="sxs-lookup"><span data-stu-id="eb674-409">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="eb674-410">Esto garantizaba que el estado expuesto en `EntityEntry` estuviera actualizado.</span><span class="sxs-lookup"><span data-stu-id="eb674-410">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="eb674-411">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-411">**New behavior**</span></span>

<span data-ttu-id="eb674-412">A partir de EF Core 3.0, ahora la llamada a `DbContext.Entry` solo intenta detectar cambios en la entidad dada y cualquier entidad de seguridad relacionada con ella de la que se haya realizado el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="eb674-412">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="eb674-413">Esto significa que es posible que la llamada a este método no haya detectado otros cambios, lo que podría tener implicaciones en el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb674-413">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="eb674-414">Observe que si `ChangeTracker.AutoDetectChangesEnabled` se establece en `false` incluso esta detección de cambios local se deshabilitará.</span><span class="sxs-lookup"><span data-stu-id="eb674-414">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="eb674-415">Otros métodos que provocan la detección de cambios (como `ChangeTracker.Entries` y `SaveChanges`) siguen provocando una acción `DetectChanges` completa de todas las entidades de las que se realiza el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="eb674-415">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="eb674-416">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-416">**Why**</span></span>

<span data-ttu-id="eb674-417">Este cambio se ha realizado para mejorar el rendimiento predeterminado del uso de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="eb674-417">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="eb674-418">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-418">**Mitigations**</span></span>

<span data-ttu-id="eb674-419">Llame a `ChgangeTracker.DetectChanges()` de forma explícita antes de llamar a `Entry` para garantizar el comportamiento anterior a la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="eb674-419">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="eb674-420">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="eb674-420">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="eb674-421">Problema de seguimiento n.º 14617</span><span class="sxs-lookup"><span data-stu-id="eb674-421">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="eb674-422">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-423">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-423">**Old behavior**</span></span>

<span data-ttu-id="eb674-424">Antes de EF Core 3.0, se podían usar las propiedades de clave `string` y `byte[]` sin tener que establecer de forma explícita un valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="eb674-424">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="eb674-425">En ese caso, el valor de clave se generaba en el cliente como un GUID, que se serializaba en bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="eb674-425">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="eb674-426">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-426">**New behavior**</span></span>

<span data-ttu-id="eb674-427">A partir de EF Core 3.0, se iniciará una excepción en la que indica que no se ha establecido ningún valor de clave.</span><span class="sxs-lookup"><span data-stu-id="eb674-427">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="eb674-428">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-428">**Why**</span></span>

<span data-ttu-id="eb674-429">Este cambio se ha realizado porque los valores `string`/`byte[]` generados por el cliente no suelen ser útiles, y el comportamiento predeterminado dificultaba razonar sobre los valores de clave generados de una forma habitual.</span><span class="sxs-lookup"><span data-stu-id="eb674-429">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="eb674-430">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-430">**Mitigations**</span></span>

<span data-ttu-id="eb674-431">Se puede obtener el comportamiento anterior a la versión 3.0 si se especifica de forma explícita que las propiedades de clave deben usar los valores generados si no se establece ningún otro valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="eb674-431">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="eb674-432">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="eb674-432">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="eb674-433">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="eb674-433">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="eb674-434">Ahora ILoggerFactory es un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="eb674-434">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="eb674-435">Problema de seguimiento n.º 14698</span><span class="sxs-lookup"><span data-stu-id="eb674-435">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="eb674-436">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="eb674-436">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="eb674-437">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-437">**Old behavior**</span></span>

<span data-ttu-id="eb674-438">Antes de EF Core 3.0, `ILoggerFactory` se registraba como un servicio de singleton.</span><span class="sxs-lookup"><span data-stu-id="eb674-438">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="eb674-439">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-439">**New behavior**</span></span>

<span data-ttu-id="eb674-440">A partir de EF Core 3.0, `ILoggerFactory` ahora se registra como con ámbito.</span><span class="sxs-lookup"><span data-stu-id="eb674-440">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="eb674-441">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-441">**Why**</span></span>

<span data-ttu-id="eb674-442">Este cambio se ha realizado para permitir la asociación de un registrador con una instancia de `DbContext`, lo que habilita otras funciones y quita algunos casos de comportamiento patológico como un aumento vertiginoso de los proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="eb674-442">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="eb674-443">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-443">**Mitigations**</span></span>

<span data-ttu-id="eb674-444">Este cambio no debería afectar al código de la aplicación a menos que registre y use servicios personalizados en el proveedor de servicios internos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="eb674-444">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="eb674-445">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="eb674-445">This isn't common.</span></span>
<span data-ttu-id="eb674-446">En estos casos, la mayoría de los elementos seguirá funcionando, pero cualquier servicio de singleton que dependiera de `ILoggerFactory` tendrá que cambiarse para obtener la interfaz `ILoggerFactory` de otra forma.</span><span class="sxs-lookup"><span data-stu-id="eb674-446">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="eb674-447">Si experimenta situaciones como esta, registre un problema en el [rastreador de problemas de GitHub de EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para hacernos saber cómo usa `ILoggerFactory`, para que podamos comprender mejor cómo evitar esta interrupción en el futuro.</span><span class="sxs-lookup"><span data-stu-id="eb674-447">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="eb674-448">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="eb674-448">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="eb674-449">Problema de seguimiento n.º 12780</span><span class="sxs-lookup"><span data-stu-id="eb674-449">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="eb674-450">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-450">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-451">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-451">**Old behavior**</span></span>

<span data-ttu-id="eb674-452">Antes de EF Core 3.0, una vez que se eliminaba `DbContext` no había ninguna forma de saber si una determinada propiedad de navegación de una entidad obtenida de ese contexto se había cargado completamente o no.</span><span class="sxs-lookup"><span data-stu-id="eb674-452">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="eb674-453">En su lugar, los proxies asumían que una navegación de referencia se cargaba si tenía un valor distinto de NULL, y que una navegación de colección se cargaba si no estaba vacía.</span><span class="sxs-lookup"><span data-stu-id="eb674-453">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="eb674-454">En estos casos, el intento de carga diferida era no operativo.</span><span class="sxs-lookup"><span data-stu-id="eb674-454">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="eb674-455">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-455">**New behavior**</span></span>

<span data-ttu-id="eb674-456">A partir de EF Core 3.0, los proxies realizan el seguimiento de si una propiedad de navegación se carga o no.</span><span class="sxs-lookup"><span data-stu-id="eb674-456">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="eb674-457">Esto significa que el intento de acceder a una propiedad de navegación que se carga después de que se haya eliminado el contexto siempre será no operativo, incluso cuando la navegación cargada está vacía o es NULL.</span><span class="sxs-lookup"><span data-stu-id="eb674-457">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="eb674-458">Por el contrario, el intento de acceder a una propiedad de navegación que no está cargada iniciará una excepción si el contexto se ha eliminado, incluso si la propiedad de navegación es una colección no vacía.</span><span class="sxs-lookup"><span data-stu-id="eb674-458">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="eb674-459">Si se produce esta situación, significa que el código de aplicación está intentando usar la carga diferida en un momento no válido y que se debe cambiar la aplicación para que lo no haga.</span><span class="sxs-lookup"><span data-stu-id="eb674-459">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="eb674-460">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-460">**Why**</span></span>

<span data-ttu-id="eb674-461">Este cambio se ha realizado para que el comportamiento sea coherente y correcto cuando se intenta la carga diferida de una instancia de `DbContext` eliminada.</span><span class="sxs-lookup"><span data-stu-id="eb674-461">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="eb674-462">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-462">**Mitigations**</span></span>

<span data-ttu-id="eb674-463">Actualice el código de la aplicación para que no intente la carga diferida con un contexto eliminado, o bien configúrelo para que sea no operativo, como se describe en el mensaje de la excepción.</span><span class="sxs-lookup"><span data-stu-id="eb674-463">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="eb674-464">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="eb674-464">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="eb674-465">Problema de seguimiento n.º 10236</span><span class="sxs-lookup"><span data-stu-id="eb674-465">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="eb674-466">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="eb674-466">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="eb674-467">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-467">**Old behavior**</span></span>

<span data-ttu-id="eb674-468">Antes de EF Core 3.0, se registraba una advertencia para una aplicación que creaba un número patológico de proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="eb674-468">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="eb674-469">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-469">**New behavior**</span></span>

<span data-ttu-id="eb674-470">A partir de EF Core 3.0, ahora esta advertencia se considera un error y se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="eb674-470">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="eb674-471">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-471">**Why**</span></span>

<span data-ttu-id="eb674-472">Este cambio se ha realizado para controlar mejor el código de la aplicación mediante la exposición de este caso patológico de una forma más explícita.</span><span class="sxs-lookup"><span data-stu-id="eb674-472">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="eb674-473">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-473">**Mitigations**</span></span>

<span data-ttu-id="eb674-474">Cuando se produce este error, la acción más adecuada consiste en comprender la causa raíz y detener la creación de tantos proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="eb674-474">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="eb674-475">Pero el error se puede convertir en una advertencia (u omitirse) mediante configuración en `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="eb674-475">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="eb674-476">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-476">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="eb674-477">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="eb674-477">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="eb674-478">Problema de seguimiento n.° 9171</span><span class="sxs-lookup"><span data-stu-id="eb674-478">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="eb674-479">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-479">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-480">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-480">**Old behavior**</span></span>

<span data-ttu-id="eb674-481">Antes de EF Core 3.0, el código para llamar a `HasOne` o `HasMany` con una cadena se interpretaba de manera confusa.</span><span class="sxs-lookup"><span data-stu-id="eb674-481">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="eb674-482">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-482">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="eb674-483">El código parece relacionar `Samurai` con otro tipo de entidad mediante la propiedad de navegación `Entrance`, que puede ser privada.</span><span class="sxs-lookup"><span data-stu-id="eb674-483">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="eb674-484">En realidad, este código intenta crear una relación con algún tipo de entidad denominada `Entrance` sin ninguna propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="eb674-484">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="eb674-485">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-485">**New behavior**</span></span>

<span data-ttu-id="eb674-486">A partir de EF Core 3.0, el código anterior ahora hace lo que parecía que debía hacer antes.</span><span class="sxs-lookup"><span data-stu-id="eb674-486">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="eb674-487">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-487">**Why**</span></span>

<span data-ttu-id="eb674-488">El comportamiento anterior era muy confuso, especialmente al leer el código de configuración y al buscar errores.</span><span class="sxs-lookup"><span data-stu-id="eb674-488">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="eb674-489">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-489">**Mitigations**</span></span>

<span data-ttu-id="eb674-490">Esto solo interrumpirá las aplicaciones que configuran de manera explícita las relaciones con cadenas para nombres de tipos y sin especificar explícitamente la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="eb674-490">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="eb674-491">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="eb674-491">This is not common.</span></span>
<span data-ttu-id="eb674-492">El comportamiento anterior se puede obtener al pasar de manera explícita `null` para el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="eb674-492">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="eb674-493">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-493">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="eb674-494">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="eb674-494">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="eb674-495">Problema de seguimiento n.º 15184</span><span class="sxs-lookup"><span data-stu-id="eb674-495">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="eb674-496">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-496">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-497">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-497">**Old behavior**</span></span>

<span data-ttu-id="eb674-498">Antes, los siguientes métodos asincrónicos devolvían `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="eb674-498">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="eb674-499">`ValueGenerator.NextValueAsync()` (y las clases derivadas)</span><span class="sxs-lookup"><span data-stu-id="eb674-499">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="eb674-500">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-500">**New behavior**</span></span>

<span data-ttu-id="eb674-501">Dichos métodos ahora devuelven `ValueTask<T>` durante el mismo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="eb674-501">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="eb674-502">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-502">**Why**</span></span>

<span data-ttu-id="eb674-503">Este cambio reduce el número de asignaciones de montones que se producen al invocar estos métodos, lo que mejora el rendimiento general.</span><span class="sxs-lookup"><span data-stu-id="eb674-503">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="eb674-504">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-504">**Mitigations**</span></span>

<span data-ttu-id="eb674-505">Las aplicaciones que simplemente esperen las API anteriores solo necesitan recompilarse, sin que sea necesario realizar cambios en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="eb674-505">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="eb674-506">Un uso más complejo (p. ej., pasar el valor `Task` devuelto a `Task.WhenAny()`) normalmente requiere que el valor `ValueTask<T>` devuelto se convierta en `Task<T>` mediante una llamada a `AsTask()` en él.</span><span class="sxs-lookup"><span data-stu-id="eb674-506">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="eb674-507">Tenga en cuenta que esto niega la reducción de asignación que implica este cambio.</span><span class="sxs-lookup"><span data-stu-id="eb674-507">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="eb674-508">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="eb674-508">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="eb674-509">Problema de seguimiento n.º 9913</span><span class="sxs-lookup"><span data-stu-id="eb674-509">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="eb674-510">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="eb674-510">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="eb674-511">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-511">**Old behavior**</span></span>

<span data-ttu-id="eb674-512">El nombre de anotación para las anotaciones de asignación de tipos era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="eb674-512">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="eb674-513">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-513">**New behavior**</span></span>

<span data-ttu-id="eb674-514">Ahora, el nombre de anotación para las anotaciones de asignación de tipos es "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="eb674-514">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="eb674-515">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-515">**Why**</span></span>

<span data-ttu-id="eb674-516">Ahora, las asignaciones de tipos se usan para algo más que solo para proveedores de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="eb674-516">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="eb674-517">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-517">**Mitigations**</span></span>

<span data-ttu-id="eb674-518">Esto solo interrumpirá a las aplicaciones que acceden directamente a la asignación de tipos como una anotación, lo que no es habitual.</span><span class="sxs-lookup"><span data-stu-id="eb674-518">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="eb674-519">La acción más apropiada para corregir es usar la superficie de API para acceder a las asignaciones de tipos en lugar de usar directamente la anotación.</span><span class="sxs-lookup"><span data-stu-id="eb674-519">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="eb674-520">ToTable en un tipo derivado inicia una excepción</span><span class="sxs-lookup"><span data-stu-id="eb674-520">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="eb674-521">Problema de seguimiento n.º 11811</span><span class="sxs-lookup"><span data-stu-id="eb674-521">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="eb674-522">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="eb674-522">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="eb674-523">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-523">**Old behavior**</span></span>

<span data-ttu-id="eb674-524">Antes de EF Core 3.0, la llamada a `ToTable()` en un tipo derivado se omitía, ya que la única estrategia asignación de herencia era TPH, lo que no es válido.</span><span class="sxs-lookup"><span data-stu-id="eb674-524">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="eb674-525">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-525">**New behavior**</span></span>

<span data-ttu-id="eb674-526">A partir de EF Core 3.0, y en preparación para agregar compatibilidad con TPT y TPC en una versión posterior, ahora la llamada a `ToTable()` en un tipo derivado iniciará una excepción para evitar un cambio de asignación inesperado en el futuro.</span><span class="sxs-lookup"><span data-stu-id="eb674-526">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="eb674-527">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-527">**Why**</span></span>

<span data-ttu-id="eb674-528">En la actualidad no se considera válido asignar un tipo derivado a otra tabla.</span><span class="sxs-lookup"><span data-stu-id="eb674-528">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="eb674-529">Este cambio evita interrupciones en el futuro, cuando se convierta en una operación válida.</span><span class="sxs-lookup"><span data-stu-id="eb674-529">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="eb674-530">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-530">**Mitigations**</span></span>

<span data-ttu-id="eb674-531">Quite todos los intentos de asignar tipos derivados a otras tablas.</span><span class="sxs-lookup"><span data-stu-id="eb674-531">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="eb674-532">ForSqlServerHasIndex se ha reemplazado por HasIndex</span><span class="sxs-lookup"><span data-stu-id="eb674-532">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="eb674-533">Problema de seguimiento n.º 12366</span><span class="sxs-lookup"><span data-stu-id="eb674-533">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="eb674-534">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="eb674-534">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="eb674-535">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-535">**Old behavior**</span></span>

<span data-ttu-id="eb674-536">Antes de EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` proporcionaba una manera de configurar las columnas que se usaban con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="eb674-536">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="eb674-537">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-537">**New behavior**</span></span>

<span data-ttu-id="eb674-538">A partir de EF Core 3.0, ya se admite el uso de `Include` en un índice en el nivel relacional.</span><span class="sxs-lookup"><span data-stu-id="eb674-538">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="eb674-539">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="eb674-539">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="eb674-540">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-540">**Why**</span></span>

<span data-ttu-id="eb674-541">Este cambio se ha realizado para consolidar la API para índices con `Include` en un mismo lugar para todos los proveedores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="eb674-541">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="eb674-542">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-542">**Mitigations**</span></span>

<span data-ttu-id="eb674-543">Use la API nueva, como se ha mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="eb674-543">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="eb674-544">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="eb674-544">Metadata API changes</span></span>

[<span data-ttu-id="eb674-545">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="eb674-545">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="eb674-546">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-546">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-547">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-547">**New behavior**</span></span>

<span data-ttu-id="eb674-548">Las siguientes propiedades se han convertido en métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="eb674-548">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="eb674-549">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-549">**Why**</span></span>

<span data-ttu-id="eb674-550">Este cambio simplifica la implementación de las interfaces mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="eb674-550">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="eb674-551">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-551">**Mitigations**</span></span>

<span data-ttu-id="eb674-552">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="eb674-552">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="eb674-553">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="eb674-553">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="eb674-554">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="eb674-554">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="eb674-555">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="eb674-555">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="eb674-556">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-556">**New behavior**</span></span>

<span data-ttu-id="eb674-557">Los métodos de extensión específicos del proveedor se simplificarán:</span><span class="sxs-lookup"><span data-stu-id="eb674-557">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="eb674-558">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-558">**Why**</span></span>

<span data-ttu-id="eb674-559">Este cambio simplifica la implementación de los métodos de extensión mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="eb674-559">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="eb674-560">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-560">**Mitigations**</span></span>

<span data-ttu-id="eb674-561">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="eb674-561">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="eb674-562">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="eb674-562">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="eb674-563">Problema de seguimiento n.º 12151</span><span class="sxs-lookup"><span data-stu-id="eb674-563">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="eb674-564">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="eb674-564">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="eb674-565">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-565">**Old behavior**</span></span>

<span data-ttu-id="eb674-566">Antes de EF Core 3.0, EF Core enviaba `PRAGMA foreign_keys = 1` cuando se abría una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="eb674-566">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="eb674-567">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-567">**New behavior**</span></span>

<span data-ttu-id="eb674-568">A partir de EF Core 3.0, EF Core ya no envía `PRAGMA foreign_keys = 1` cuando se abre una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="eb674-568">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="eb674-569">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-569">**Why**</span></span>

<span data-ttu-id="eb674-570">Este cambio se ha realizado porque en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3` de forma predeterminada, lo que a su vez significa que el cumplimiento de CD está activado de forma predeterminada y no es necesario habilitarlo explícitamente cada vez que se abra una conexión.</span><span class="sxs-lookup"><span data-stu-id="eb674-570">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="eb674-571">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-571">**Mitigations**</span></span>

<span data-ttu-id="eb674-572">Las claves externas se habilitan de forma predeterminada en SQLitePCLRaw.bundle_e_sqlite3, que en EF Core se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="eb674-572">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="eb674-573">Para otros casos, las claves externas se pueden habilitar mediante la especificación de `Foreign Keys=True` en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="eb674-573">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="eb674-574">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="eb674-574">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="eb674-575">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-575">**Old behavior**</span></span>

<span data-ttu-id="eb674-576">Antes de EF Core 3.0, en EF Core se usaba `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="eb674-576">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="eb674-577">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-577">**New behavior**</span></span>

<span data-ttu-id="eb674-578">A partir de EF Core 3.0, en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="eb674-578">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="eb674-579">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-579">**Why**</span></span>

<span data-ttu-id="eb674-580">Este cambio se ha realizado para que la versión de SQLite que se usa en iOS sea coherente con otras plataformas.</span><span class="sxs-lookup"><span data-stu-id="eb674-580">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="eb674-581">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-581">**Mitigations**</span></span>

<span data-ttu-id="eb674-582">Para usar la versión nativa de SQLite en iOS, configure `Microsoft.Data.Sqlite` para usar otra agrupación `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="eb674-582">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="eb674-583">Almacenamiento de valores GUID como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="eb674-583">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="eb674-584">Problema de seguimiento n.º 15078</span><span class="sxs-lookup"><span data-stu-id="eb674-584">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="eb674-585">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-585">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-586">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-586">**Old behavior**</span></span>

<span data-ttu-id="eb674-587">Antes los valores GUID se almacenaban como valores BLOB en SQLite.</span><span class="sxs-lookup"><span data-stu-id="eb674-587">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="eb674-588">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-588">**New behavior**</span></span>

<span data-ttu-id="eb674-589">Ahora, los valores GUID se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="eb674-589">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="eb674-590">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-590">**Why**</span></span>

<span data-ttu-id="eb674-591">El formato binario de los GUID no está normalizado.</span><span class="sxs-lookup"><span data-stu-id="eb674-591">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="eb674-592">El almacenamiento de los valores como TEXT mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="eb674-592">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="eb674-593">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-593">**Mitigations**</span></span>

<span data-ttu-id="eb674-594">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="eb674-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="eb674-595">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="eb674-595">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="eb674-596">Microsoft.Data.Sqlite sigue siendo capaz de leer valores GUID de ambas columnas BLOB y TEXT. Sin embargo, dado que el formato predeterminado de los parámetros y las constantes ha cambiado, seguramente deberá realizar alguna acción en la mayoría de casos que impliquen el uso de valores GUID.</span><span class="sxs-lookup"><span data-stu-id="eb674-596">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="eb674-597">Ahora los valores char se almacenan como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="eb674-597">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="eb674-598">Problema de seguimiento n.º 15020</span><span class="sxs-lookup"><span data-stu-id="eb674-598">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="eb674-599">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-599">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-600">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-600">**Old behavior**</span></span>

<span data-ttu-id="eb674-601">Anteriormente los valores char se almacenaban como valores INTEGER en SQLite.</span><span class="sxs-lookup"><span data-stu-id="eb674-601">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="eb674-602">Por ejemplo, un valor char de *A* se almacenaba como el valor entero 65.</span><span class="sxs-lookup"><span data-stu-id="eb674-602">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="eb674-603">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-603">**New behavior**</span></span>

<span data-ttu-id="eb674-604">Ahora, los valores char se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="eb674-604">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="eb674-605">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-605">**Why**</span></span>

<span data-ttu-id="eb674-606">El almacenamiento de valores como TEXT es más natural y mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="eb674-606">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="eb674-607">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-607">**Mitigations**</span></span>

<span data-ttu-id="eb674-608">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="eb674-608">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="eb674-609">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="eb674-609">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="eb674-610">Microsoft.Data.Sqlite también puede leer valores de caracteres tanto de columnas INTEGER como de columnas TEXT, por lo que es posible que no deba hacer nada dependiendo de su caso.</span><span class="sxs-lookup"><span data-stu-id="eb674-610">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="eb674-611">Ahora los id. de migración se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="eb674-611">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="eb674-612">Problema de seguimiento n.º 12978</span><span class="sxs-lookup"><span data-stu-id="eb674-612">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="eb674-613">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-613">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-614">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-614">**Old behavior**</span></span>

<span data-ttu-id="eb674-615">Los identificadores de migración se generaban de forma involuntaria con el calendario de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="eb674-615">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="eb674-616">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-616">**New behavior**</span></span>

<span data-ttu-id="eb674-617">Ahora los id. de migración siempre se generan usando el calendario de la referencia cultural invariable (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="eb674-617">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="eb674-618">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-618">**Why**</span></span>

<span data-ttu-id="eb674-619">El orden de las migraciones es importante al actualizar la base de datos o al solucionar conflictos de combinación.</span><span class="sxs-lookup"><span data-stu-id="eb674-619">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="eb674-620">Al usar el calendario invariable, se evitan problemas de ordenación que pueden producirse si los miembros del equipo tienen distintos calendarios del sistema.</span><span class="sxs-lookup"><span data-stu-id="eb674-620">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="eb674-621">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-621">**Mitigations**</span></span>

<span data-ttu-id="eb674-622">Esta cambio afecta a todas las personas que usan un calendario no gregoriano en el que el año sea superior al del calendario gregoriano (como el calendario budista tailandés).</span><span class="sxs-lookup"><span data-stu-id="eb674-622">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="eb674-623">Los id. de migración existentes deberán actualizarse para que las migraciones nuevas se ordenen después de las existentes.</span><span class="sxs-lookup"><span data-stu-id="eb674-623">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="eb674-624">Puede ver el id. de migración en el atributo Migration de los archivos de diseñador de la migración.</span><span class="sxs-lookup"><span data-stu-id="eb674-624">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="eb674-625">También debe actualizarse la tabla de historial de migraciones.</span><span class="sxs-lookup"><span data-stu-id="eb674-625">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="eb674-626">Se ha quitado el elemento UseRowNumberForPaging</span><span class="sxs-lookup"><span data-stu-id="eb674-626">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="eb674-627">Problema de seguimiento n.º 16400</span><span class="sxs-lookup"><span data-stu-id="eb674-627">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="eb674-628">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="eb674-628">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="eb674-629">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-629">**Old behavior**</span></span>

<span data-ttu-id="eb674-630">Antes de EF Core 3.0, `UseRowNumberForPaging` se podía usar para generar SQL para la paginación de forma que fuera compatible con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="eb674-630">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="eb674-631">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-631">**New behavior**</span></span>

<span data-ttu-id="eb674-632">A partir de EF Core 3.0, EF solo genera SQL para la paginación que únicamente es compatible con las versiones posteriores de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="eb674-632">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="eb674-633">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-633">**Why**</span></span>

<span data-ttu-id="eb674-634">El motivo de este cambio es que [SQL Server 2008 ya no se admite](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/). Además, la actualización de esta característica para que funcionase con los cambios en las consultas implementados en EF Core 3.0 llevaría mucho trabajo.</span><span class="sxs-lookup"><span data-stu-id="eb674-634">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="eb674-635">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-635">**Mitigations**</span></span>

<span data-ttu-id="eb674-636">Se recomienda actualizar a una versión más reciente de SQL Server, o bien utilizar un nivel de compatibilidad superior, de modo que el SQL que se genere se admita.</span><span class="sxs-lookup"><span data-stu-id="eb674-636">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="eb674-637">Dicho esto, si no puede hacerlo, [escriba un comentario en el problema de seguimiento](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con los detalles al respecto.</span><span class="sxs-lookup"><span data-stu-id="eb674-637">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="eb674-638">En función de los comentarios, es posible que volvamos a valorar esta decisión.</span><span class="sxs-lookup"><span data-stu-id="eb674-638">We may revisit this decision based on feedback.</span></span>

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="eb674-639">La información o metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="eb674-639">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="eb674-640">Problema de seguimiento n.º 16119</span><span class="sxs-lookup"><span data-stu-id="eb674-640">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="eb674-641">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="eb674-641">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="eb674-642">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-642">**Old behavior**</span></span>

<span data-ttu-id="eb674-643">`IDbContextOptionsExtension` incluía métodos para proporcionar metadatos sobre la extensión.</span><span class="sxs-lookup"><span data-stu-id="eb674-643">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="eb674-644">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-644">**New behavior**</span></span>

<span data-ttu-id="eb674-645">Estos métodos se han movido a una nueva clase base abstracta `DbContextOptionsExtensionInfo`, que se devuelve desde una nueva propiedad `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="eb674-645">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="eb674-646">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-646">**Why**</span></span>

<span data-ttu-id="eb674-647">Al lanzarse las versiones 2.0 y 3.0, tuvimos que agregar o cambiar estos métodos varias veces.</span><span class="sxs-lookup"><span data-stu-id="eb674-647">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="eb674-648">Su división en una nueva clase base abstracta facilitará la realización de este tipo de cambios sin interrumpir las extensiones existentes.</span><span class="sxs-lookup"><span data-stu-id="eb674-648">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="eb674-649">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-649">**Mitigations**</span></span>

<span data-ttu-id="eb674-650">Actualice las extensiones para seguir el nuevo patrón.</span><span class="sxs-lookup"><span data-stu-id="eb674-650">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="eb674-651">Encontrará ejemplos en las muchas implementaciones de `IDbContextOptionsExtension` para los diferentes tipos de extensiones en el código fuente de EF Core.</span><span class="sxs-lookup"><span data-stu-id="eb674-651">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="eb674-652">Cambio de nombre de LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="eb674-652">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="eb674-653">Problema de seguimiento n.º 10985</span><span class="sxs-lookup"><span data-stu-id="eb674-653">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="eb674-654">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-654">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-655">**Cambio**</span><span class="sxs-lookup"><span data-stu-id="eb674-655">**Change**</span></span>

<span data-ttu-id="eb674-656">Se ha cambiado el nombre de `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="eb674-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="eb674-657">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-657">**Why**</span></span>

<span data-ttu-id="eb674-658">Conviene alinear el nombre de este evento de advertencia con el del resto de eventos de advertencia.</span><span class="sxs-lookup"><span data-stu-id="eb674-658">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="eb674-659">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-659">**Mitigations**</span></span>

<span data-ttu-id="eb674-660">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="eb674-660">Use the new name.</span></span> <span data-ttu-id="eb674-661">(Tenga en cuenta que el número de id. evento sigue siendo el mismo).</span><span class="sxs-lookup"><span data-stu-id="eb674-661">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="eb674-662">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="eb674-662">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="eb674-663">Problema de seguimiento n.º 10730</span><span class="sxs-lookup"><span data-stu-id="eb674-663">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="eb674-664">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-664">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-665">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-665">**Old behavior**</span></span>

<span data-ttu-id="eb674-666">Antes de EF Core 3.0, se utilizaba simplemente el término "nombre" para hacer referencia a los nombres de las restricciones de claves externas.</span><span class="sxs-lookup"><span data-stu-id="eb674-666">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="eb674-667">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-667">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="eb674-668">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-668">**New behavior**</span></span>

<span data-ttu-id="eb674-669">A partir de EF Core 3.0, el término con el que se hace referencia a los nombres de las restricciones de claves externas es "nombre de la restricción".</span><span class="sxs-lookup"><span data-stu-id="eb674-669">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="eb674-670">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="eb674-670">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="eb674-671">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-671">**Why**</span></span>

<span data-ttu-id="eb674-672">Este cambio permite mejorar la coherencia relativa a la nomenclatura en este aspecto y aclarar que se trata del nombre de una restricción de clave externa, y no del de la columna o propiedad en la que está definida la clave externa.</span><span class="sxs-lookup"><span data-stu-id="eb674-672">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="eb674-673">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-673">**Mitigations**</span></span>

<span data-ttu-id="eb674-674">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="eb674-674">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="eb674-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="eb674-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="eb674-676">Problema de seguimiento n.° 15997</span><span class="sxs-lookup"><span data-stu-id="eb674-676">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="eb674-677">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="eb674-677">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="eb674-678">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-678">**Old behavior**</span></span>

<span data-ttu-id="eb674-679">Antes de EF Core 3.0, estos métodos estaban protegidos.</span><span class="sxs-lookup"><span data-stu-id="eb674-679">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="eb674-680">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-680">**New behavior**</span></span>

<span data-ttu-id="eb674-681">Desde EF Core 3.0, estos métodos son públicos.</span><span class="sxs-lookup"><span data-stu-id="eb674-681">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="eb674-682">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-682">**Why**</span></span>

<span data-ttu-id="eb674-683">EF usa estos métodos para determinar si se ha creado una base de datos, pero está vacía.</span><span class="sxs-lookup"><span data-stu-id="eb674-683">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="eb674-684">Esto también puede resultar útil fuera de EF al determinar si se deben aplicar migraciones o no.</span><span class="sxs-lookup"><span data-stu-id="eb674-684">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="eb674-685">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-685">**Mitigations**</span></span>

<span data-ttu-id="eb674-686">Cambie la accesibilidad de cualquier invalidación.</span><span class="sxs-lookup"><span data-stu-id="eb674-686">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="eb674-687">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="eb674-687">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="eb674-688">Problema de seguimiento n.° 11506</span><span class="sxs-lookup"><span data-stu-id="eb674-688">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="eb674-689">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="eb674-689">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="eb674-690">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-690">**Old behavior**</span></span>

<span data-ttu-id="eb674-691">Antes de EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un paquete NuGet regular con un ensamblado al que podían hacer referencia los proyectos que dependían de él.</span><span class="sxs-lookup"><span data-stu-id="eb674-691">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="eb674-692">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-692">**New behavior**</span></span>

<span data-ttu-id="eb674-693">Desde EF Core 3.0, es un paquete DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="eb674-693">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="eb674-694">Esto significa que la dependencia no fluirá de manera transitiva en otros proyectos y que ya no puede, de forma predeterminada, hacer referencia a su ensamblado.</span><span class="sxs-lookup"><span data-stu-id="eb674-694">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="eb674-695">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-695">**Why**</span></span>

<span data-ttu-id="eb674-696">Este paquete solo está destinado a usarse en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="eb674-696">This package is only intended to be used at design time.</span></span> <span data-ttu-id="eb674-697">Las aplicaciones implementadas no deben hacer referencia al mismo.</span><span class="sxs-lookup"><span data-stu-id="eb674-697">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="eb674-698">Hacer que el paquete sea DevelopmentDependency refuerza esta recomendación.</span><span class="sxs-lookup"><span data-stu-id="eb674-698">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="eb674-699">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-699">**Mitigations**</span></span>

<span data-ttu-id="eb674-700">Si tiene que hacer referencia a este paquete para invalidar el comportamiento en tiempo de diseño de EF Core, puede actualizar los metadatos de elementos PackageReference del proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb674-700">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="eb674-701">Si se hace referencia al paquete de manera transitiva a través de Microsoft.EntityFrameworkCore.Tools, tendrá que agregar una PackageReference explícita al paquete para cambiar sus metadatos.</span><span class="sxs-lookup"><span data-stu-id="eb674-701">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="eb674-702">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="eb674-702">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="eb674-703">Problema de seguimiento n.° 14824</span><span class="sxs-lookup"><span data-stu-id="eb674-703">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="eb674-704">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="eb674-704">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="eb674-705">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-705">**Old behavior**</span></span>

<span data-ttu-id="eb674-706">Microsoft.EntityFrameworkCore.Sqlite dependía anteriormente de la versión 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="eb674-706">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="eb674-707">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-707">**New behavior**</span></span>

<span data-ttu-id="eb674-708">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="eb674-708">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="eb674-709">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-709">**Why**</span></span>

<span data-ttu-id="eb674-710">La versión 2.0.0 de SQLitePCL.raw selecciona .NET Standard 2.0 como destino.</span><span class="sxs-lookup"><span data-stu-id="eb674-710">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="eb674-711">Anteriormente seleccionaba .NET Standard 1.1 como destino, que requería el cierre a gran escala de paquetes transitivos para su funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="eb674-711">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="eb674-712">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-712">**Mitigations**</span></span>

<span data-ttu-id="eb674-713">En la versión 2.0.0 de SQLitePCL.raw se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="eb674-713">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="eb674-714">Consulte las [notas de la versión](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="eb674-714">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>


## <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="eb674-715">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="eb674-715">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="eb674-716">Problema de seguimiento n.° 14825</span><span class="sxs-lookup"><span data-stu-id="eb674-716">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="eb674-717">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="eb674-717">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="eb674-718">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="eb674-718">**Old behavior**</span></span>

<span data-ttu-id="eb674-719">Los paquetes espaciales anteriormente dependían de la versión 1.15.1 de NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="eb674-719">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="eb674-720">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="eb674-720">**New behavior**</span></span>

<span data-ttu-id="eb674-721">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="eb674-721">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="eb674-722">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="eb674-722">**Why**</span></span>

<span data-ttu-id="eb674-723">La versión 2.0.0 de NetTopologySuite pretende resolver varios problemas de usabilidad que encontraron los usuarios de EF Core.</span><span class="sxs-lookup"><span data-stu-id="eb674-723">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="eb674-724">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="eb674-724">**Mitigations**</span></span>

<span data-ttu-id="eb674-725">En la versión 2.0.0 de NetTopologySuite se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="eb674-725">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="eb674-726">Consulte las [notas de la versión](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="eb674-726">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>
