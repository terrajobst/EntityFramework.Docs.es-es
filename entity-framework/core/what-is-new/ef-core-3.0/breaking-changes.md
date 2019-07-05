---
title: 'Cambios importantes en EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: dcbea1a2aab5baea35f81500bb7bb5482695d778
ms.sourcegitcommit: 812010a35afe902d8c4bb03a67d575f8e91b5ec0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67506265"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="1d4a9-102">Cambios importantes incluidos en EF Core 3.0 (actualmente en versión preliminar)</span><span class="sxs-lookup"><span data-stu-id="1d4a9-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1d4a9-103">Tenga en cuenta que los conjuntos de características y las programaciones de las versiones futuras siempre están sujetos a cambios y, aunque intentamos mantener esta página actualizada, es posible que no refleje nuestros planes más recientes en todo momento.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="1d4a9-104">Es posible que los siguientes cambios de API y comportamiento interrumpan las aplicaciones desarrolladas para EF Core 2.2. x cuando se actualicen a la versión 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="1d4a9-105">Los cambios que esperamos que solo afecten a proveedores de base de datos se documentan en [Cambios para proveedores](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="1d4a9-106">Aquí no se documentan los cambios en las características nuevas presentadas de una versión preliminar 3.0 a otra.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="1d4a9-107">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="1d4a9-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="1d4a9-108">[Problema de seguimiento n.° 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Consulte también el problema n.° 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="1d4a9-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="1d4a9-109">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-110">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-110">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-111">Antes de 3.0, cuando en EF Core no se podía convertir una expresión que formaba parte de una consulta SQL o un parámetro, la expresión se evaluaba de forma automática en el cliente.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="1d4a9-112">De forma predeterminada, la evaluación de cliente de expresiones potencialmente costosas solo desencadenaba una advertencia.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="1d4a9-113">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-113">**New behavior**</span></span>

<span data-ttu-id="1d4a9-114">A partir de 3.0, en EF Core solo se permite que se evalúen en el cliente las expresiones en la proyección de nivel superior (la última llamada a `Select()` de la consulta).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="1d4a9-115">Cuando las expresiones de otra parte de la consulta no se pueden convertir en SQL o un parámetro, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="1d4a9-116">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-116">**Why**</span></span>

<span data-ttu-id="1d4a9-117">La evaluación de cliente automática de las consultas permite que se ejecuten muchas consultas incluso si no se pueden convertir elementos importantes de ellas.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="1d4a9-118">Esto puede provocar un comportamiento inesperado y potencialmente dañino que es posible que solo sea evidente en entornos de producción.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="1d4a9-119">Por ejemplo, una condición en una llamada a `Where()` que no se puede convertir puede provocar que todas las filas de la tabla se transfieran desde el servidor de base de datos y que el filtro se aplique en el cliente.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="1d4a9-120">Esta situación puede pasar desapercibida fácilmente si la tabla solo contiene algunas filas en la fase de desarrollo, pero ser más grave cuando la aplicación pase a producción, donde la tabla puede contener millones de filas.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="1d4a9-121">Las advertencias de evaluación de cliente también se suelen pasar por alto durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="1d4a9-122">Además de esto, la evaluación de cliente automática puede causar problemas en los que la mejora de la traducción de consultas para expresiones específicas provocaba cambios importantes no deseados entre versiones.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="1d4a9-123">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-123">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-124">Si una consulta no se puede traducir totalmente, vuelva a escribirla en un formato que se pueda traducir, o bien use `AsEnumerable()`, `ToList()` o una función similar para devolver los datos al cliente de forma explícita, donde después se puedan seguir procesando mediante LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="1d4a9-125">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1d4a9-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="1d4a9-126">Anuncios del problema de seguimiento n.º 325</span><span class="sxs-lookup"><span data-stu-id="1d4a9-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="1d4a9-127">Este cambio se ha introducido en ASP.NET Core 3.0 (versión preliminar 1).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="1d4a9-128">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-128">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-129">Antes de ASP.NET Core 3.0, cuando se agregaba una referencia de paquete a `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, se incluía EF Core y algunos de los proveedores de datos de EF Core, como el de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="1d4a9-130">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-130">**New behavior**</span></span>

<span data-ttu-id="1d4a9-131">A partir de la versión 3.0, el marco compartido ASP.NET Core no incluye EF Core ni ningún proveedor de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="1d4a9-132">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-132">**Why**</span></span>

<span data-ttu-id="1d4a9-133">Antes de este cambio, para obtener EF Core se necesitaban varios pasos en función de si la aplicación se destinaba a ASP.NET Core y SQL Server o no.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="1d4a9-134">Además, la actualización de ASP.NET Core forzaba la de EF Core y el proveedor de SQL Server, lo que no siempre es deseable.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="1d4a9-135">Con este cambio, la experiencia de obtención de EF Core es la misma en todos los proveedores, implementaciones admitidas de .NET y tipos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="1d4a9-136">Ahora los desarrolladores también pueden controlar exactamente cuándo se actualizan EF Core y los proveedores de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="1d4a9-137">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-137">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-138">Para usar EF Core en una aplicación ASP.NET Core 3.0 o cualquier otra aplicación compatible, debe agregar de forma explícita una referencia de paquete al proveedor de base de datos de EF Core que se va a usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="1d4a9-139">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="1d4a9-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="1d4a9-140">Problema de seguimiento n.º 14016</span><span class="sxs-lookup"><span data-stu-id="1d4a9-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="1d4a9-141">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4) y la versión correspondiente del SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="1d4a9-142">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-142">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-143">Antes de 3.0, la herramienta `dotnet ef` se incluía en el SDK de .NET Core y estaba disponible para usarse desde la línea de comandos de cualquier proyecto sin necesidad de realizar pasos adicionales.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="1d4a9-144">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-144">**New behavior**</span></span>

<span data-ttu-id="1d4a9-145">A partir de la versión 3.0, el SDK de .NET no incluye la herramienta `dotnet ef`, por lo que antes de poder usarla tendrá que instalarla de forma explícita como una herramienta local o global.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="1d4a9-146">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-146">**Why**</span></span>

<span data-ttu-id="1d4a9-147">Este cambio nos permite distribuir y actualizar `dotnet ef` como una herramienta convencional de la CLI de .NET en NuGet, coherente con el hecho de que la versión 3.0 de EF Core también se distribuye siempre como un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="1d4a9-148">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-148">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-149">Para poder administrar las migraciones o aplicar la técnica de scaffolding a `DbContext`, instale `dotnet-ef` con el comando `dotnet tool install`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="1d4a9-150">Por ejemplo, para instalarlo como una herramienta global, puede escribir este comando:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="1d4a9-151">También se puede obtener una herramienta local cuando se restauran las dependencias de un proyecto que la declara como una dependencia de herramientas mediante un [archivo de manifiesto de herramientas](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="1d4a9-152">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="1d4a9-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="1d4a9-153">Problema de seguimiento n.º 10996</span><span class="sxs-lookup"><span data-stu-id="1d4a9-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="1d4a9-154">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-154">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-155">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-155">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-156">Antes de EF Core 3.0, estos nombres de métodos se sobrecargaban para funcionar tanto con una cadena normal como con una cadena que se debería interpolar en SQL y parámetros.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="1d4a9-157">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-157">**New behavior**</span></span>

<span data-ttu-id="1d4a9-158">A partir de la versión EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw` y `ExecuteSqlRawAsync` para crear una consulta con parámetros donde los parámetros se pasan por separado de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="1d4a9-159">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="1d4a9-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated` y `ExecuteSqlInterpolatedAsync` para crear una consulta con parámetros donde los parámetros se pasan como parte de una cadena de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="1d4a9-161">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="1d4a9-162">Tenga en cuenta que las dos consultas anteriores producirán el mismo código SQL parametrizado con los mismos parámetros SQL.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="1d4a9-163">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-163">**Why**</span></span>

<span data-ttu-id="1d4a9-164">Las sobrecargas del método como esta facilitan las llamadas accidentales al método de cadena sin procesar cuando la intención era llamar al método de cadena interpolada y viceversa.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="1d4a9-165">Esto podría resultar en consultas que no se parametrizan cuando deberían.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="1d4a9-166">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-166">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-167">Haga el cambio para usar los nuevos nombres de métodos.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-167">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="1d4a9-168">Solo se pueden especificar métodos de FromSql en raíces de consulta.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-168">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="1d4a9-169">Problema de seguimiento n.° 15704</span><span class="sxs-lookup"><span data-stu-id="1d4a9-169">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="1d4a9-170">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-170">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1d4a9-171">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-171">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-172">Antes de EF Core 3.0, el método `FromSql` podía especificarse en cualquier lugar en la consulta.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-172">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="1d4a9-173">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-173">**New behavior**</span></span>

<span data-ttu-id="1d4a9-174">A partir de EF Core 3.0, los nuevos métodos `FromSqlRaw` y `FromSqlInterpolated` (que reemplazan a `FromSql`) solo pueden especificarse en las raíces de la consulta, es decir, directamente en `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-174">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="1d4a9-175">Si intenta especificarlos en cualquier otro lugar se producirá un error de compilación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-175">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="1d4a9-176">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-176">**Why**</span></span>

<span data-ttu-id="1d4a9-177">La especificación de `FromSql` en cualquier otro lugar diferente de `DbSet` no tenía un significado o valor agregado, y podría causar ambigüedad en ciertos escenarios.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-177">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="1d4a9-178">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-178">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-179">Las invocaciones de `FromSql` se deben mover para que estén directamente en el `DbSet` al que se aplican.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-179">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="1d4a9-180">~~La ejecución de consultas se registra en el nivel de depuración~~ Revertido</span><span class="sxs-lookup"><span data-stu-id="1d4a9-180">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="1d4a9-181">Problema de seguimiento n.º 14523</span><span class="sxs-lookup"><span data-stu-id="1d4a9-181">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="1d4a9-182">Este cambio se revirtió en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-182">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1d4a9-183">Revertimos este cambio porque la nueva configuración de EF Core 3.0 permite a la aplicación especificar el nivel de registro para cualquier evento.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-183">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="1d4a9-184">Por ejemplo, para cambiar el registro de SQL a `Debug`, configure el nivel de forma explícita en `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-184">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="1d4a9-185">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="1d4a9-185">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="1d4a9-186">Problema de seguimiento n.º 12378</span><span class="sxs-lookup"><span data-stu-id="1d4a9-186">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="1d4a9-187">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-187">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1d4a9-188">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-188">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-189">Antes de EF Core 3.0, los valores temporales se asignaban a todas las propiedades de clave para las que posteriormente la base de datos generaba un valor real.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-189">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="1d4a9-190">Normalmente, estos valores temporales eran números negativos grandes.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-190">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="1d4a9-191">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-191">**New behavior**</span></span>

<span data-ttu-id="1d4a9-192">A partir de la versión 3.0, en EF Core se almacena el valor de clave temporal como parte de la información de seguimiento de la entidad y la propiedad clave en sí no se modifica.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-192">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="1d4a9-193">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-193">**Why**</span></span>

<span data-ttu-id="1d4a9-194">Este cambio se ha realizado para evitar que los valores de clave temporales se conviertan erróneamente en permanentes cuando una entidad de la que previamente una instancia de `DbContext` ha realizado el seguimiento se mueve a otra instancia de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-194">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="1d4a9-195">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-195">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-196">Las aplicaciones que asignan valores de clave principal a claves externas para crear asociaciones entre entidades pueden depender del comportamiento anterior si las claves principales son generadas por el almacén y pertenecen a entidades en el estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-196">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="1d4a9-197">Esto se puede evitar con las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-197">This can be avoided by:</span></span>
* <span data-ttu-id="1d4a9-198">No se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-198">Not using store-generated keys.</span></span>
* <span data-ttu-id="1d4a9-199">Se establecen propiedades de navegación para crear relaciones en lugar de establecer valores de clave externa.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-199">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="1d4a9-200">Se obtienen los valores de clave temporal reales de la información de seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-200">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="1d4a9-201">Por ejemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` devolverá el valor temporal aunque no se haya establecido `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-201">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="1d4a9-202">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="1d4a9-202">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="1d4a9-203">Problema de seguimiento n.º 14616</span><span class="sxs-lookup"><span data-stu-id="1d4a9-203">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="1d4a9-204">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-204">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d4a9-205">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-205">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-206">Antes de EF Core 3.0, se realizaba el seguimiento en el estado `Added` de las entidades sin seguimiento detectadas por `DetectChanges` y se insertaban como una fila nueva cuando se llamaba a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-206">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="1d4a9-207">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-207">**New behavior**</span></span>

<span data-ttu-id="1d4a9-208">A partir de EF Core 3.0, si una entidad usa valores de clave generados y se establece un valor de clave, se realizará el seguimiento de la entidad en el estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-208">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="1d4a9-209">Esto significa que se supone que existe una fila para la entidad y que se actualizará cuando se llame a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-209">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="1d4a9-210">Si no se establece el valor de clave, o bien si el tipo de entidad no usa claves generadas, se seguirá realizando el seguimiento de la entidad nueva como `Added` al igual que en las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-210">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="1d4a9-211">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-211">**Why**</span></span>

<span data-ttu-id="1d4a9-212">Este cambio se ha realizado para que sea más sencillo y coherente trabajar con gráficos de entidades desconectadas mientras se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-212">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="1d4a9-213">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-213">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-214">Este cambio puede interrumpir una aplicación si se configura un tipo de entidad para usar claves generadas, pero se establecen de forma explícita valores de clave para las instancias nuevas.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-214">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="1d4a9-215">La solución consiste en configurar de forma explícita las propiedades de clave para que no usen valores generados.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-215">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="1d4a9-216">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-216">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="1d4a9-217">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-217">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="1d4a9-218">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="1d4a9-218">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="1d4a9-219">Problema de seguimiento n.º 10114</span><span class="sxs-lookup"><span data-stu-id="1d4a9-219">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="1d4a9-220">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-220">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d4a9-221">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-221">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-222">Antes de la versión 3.0, en EF Core no se aplicaban acciones en cascada (eliminación de entidades dependientes cuando se eliminaba una entidad de seguridad obligatoria o cuando se rompía la relación con una entidad de seguridad obligatoria) hasta que se llamaba a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-222">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="1d4a9-223">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-223">**New behavior**</span></span>

<span data-ttu-id="1d4a9-224">A partir de 3.0, en EF Core las acciones en cascada se aplican en cuanto se detecta la condición desencadenadora.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-224">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="1d4a9-225">Por ejemplo, como resultado de la llamada a `context.Remove()` para eliminar una entidad de seguridad, todos los dependientes obligatorios relacionados de los que se realiza el seguimiento también se establecen en `Deleted` inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-225">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="1d4a9-226">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-226">**Why**</span></span>

<span data-ttu-id="1d4a9-227">Este cambio se ha realizado para mejorar la experiencia en escenarios de auditoría y enlace de datos donde es importante comprender qué entidades se van a eliminar _antes_ de llamar a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-227">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="1d4a9-228">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-228">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-229">El comportamiento anterior se puede restaurar mediante opciones de `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-229">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="1d4a9-230">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-230">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="1d4a9-231">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="1d4a9-231">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="1d4a9-232">Problema de seguimiento n.º 12661</span><span class="sxs-lookup"><span data-stu-id="1d4a9-232">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="1d4a9-233">Este cambio se introdujo en EF Core 3.0 (versión preliminar 5).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-233">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="1d4a9-234">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-234">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-235">Antes de la versión 3.0, `DeleteBehavior.Restrict` creaba claves externas en la base de datos con la semántica `Restrict`, pero también realizaba una corrección interna de manera no evidente.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-235">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="1d4a9-236">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-236">**New behavior**</span></span>

<span data-ttu-id="1d4a9-237">A partir de la versión 3.0, `DeleteBehavior.Restrict` garantiza que las claves externas se crean con la semántica `Restrict`, es decir, sin cascadas y realizando una infracción de restricción, sin llevar a cabo correcciones internas de EF.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-237">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="1d4a9-238">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-238">**Why**</span></span>

<span data-ttu-id="1d4a9-239">Este cambio se realizó para mejorar la experiencia de uso de `DeleteBehavior` de manera intuitiva sin efectos secundarios inesperados.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-239">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="1d4a9-240">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-240">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-241">El comportamiento anterior se puede restaurar con `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-241">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="1d4a9-242">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="1d4a9-242">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="1d4a9-243">Problema de seguimiento n.º 14194</span><span class="sxs-lookup"><span data-stu-id="1d4a9-243">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="1d4a9-244">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-244">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d4a9-245">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-245">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-246">Antes de EF Core 3.0, los [tipos de consulta](xref:core/modeling/query-types) eran un medio para consultar los datos que no definen una clave principal de una manera estructurada.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-246">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="1d4a9-247">Es decir, un tipo de consulta se usaba para asignar tipos de entidad sin claves (más probablemente desde una vista, pero posiblemente desde una tabla), mientras que un tipo de entidad estándar se usaba cuando había una clave disponible (más probablemente desde una tabla, pero posiblemente desde una vista).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-247">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="1d4a9-248">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-248">**New behavior**</span></span>

<span data-ttu-id="1d4a9-249">Ahora un tipo de consulta se convierte en un tipo de entidad sin una clave principal.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-249">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="1d4a9-250">Los tipos de entidad sin clave tienen la misma funcionalidad que los tipos de consulta de las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-250">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="1d4a9-251">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-251">**Why**</span></span>

<span data-ttu-id="1d4a9-252">Este cambio se ha realizado para reducir la confusión en torno a la finalidad de los tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-252">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="1d4a9-253">En concreto, son tipos de entidad sin clave y, por ello, intrínsecamente son de solo lectura, pero no se deben usar solo porque un tipo de entidad tenga que ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-253">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="1d4a9-254">Del mismo modo, se suelen asignar a vistas, pero solo porque las vistas no suelen definir claves.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-254">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="1d4a9-255">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-255">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-256">Los elementos siguientes de la API ahora están obsoletos:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-256">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="1d4a9-257">**`ModelBuilder.Query<>()`** : en su lugar es necesario llamar a `ModelBuilder.Entity<>().HasNoKey()` para marcar un tipo de entidad como sin claves.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-257">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="1d4a9-258">Esto todavía no se configurará por convención para evitar una configuración incorrecta cuando se espera una clave principal, pero no coincide con la convención.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-258">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="1d4a9-259">**`DbQuery<>`** : en su lugar se debe usar `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-259">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="1d4a9-260">**`DbContext.Query<>()`** : en su lugar se debe usar `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-260">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="1d4a9-261">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="1d4a9-261">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="1d4a9-262">[Problema de seguimiento n.º 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema de seguimiento n.º 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema de seguimiento n.º 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="1d4a9-262">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="1d4a9-263">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-263">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d4a9-264">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-264">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-265">Antes de EF Core 3.0, la configuración de la relación de propiedad se realizaba directamente después de la llamada a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-265">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="1d4a9-266">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-266">**New behavior**</span></span>

<span data-ttu-id="1d4a9-267">A partir de EF Core 3.0, ahora hay una API fluida para configurar una propiedad de navegación para el propietario mediante `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-267">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="1d4a9-268">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-268">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="1d4a9-269">La configuración relacionada con la relación entre el propietario y lo que se posee ahora se debe encadenar después de `WithOwner()`, de forma similar a cómo se configuran otras relaciones.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-269">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="1d4a9-270">Pero la configuración del propio tipo de propiedad se seguirá encadenando después de `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-270">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="1d4a9-271">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-271">For example:</span></span>

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

<span data-ttu-id="1d4a9-272">Además, la llamada a `Entity()`, `HasOne()` o `Set()` con un tipo de propiedad de destino ahora iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-272">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="1d4a9-273">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-273">**Why**</span></span>

<span data-ttu-id="1d4a9-274">Este cambio se ha realizado para crear una separación más clara entre la configuración del propio tipo de propiedad y la _relación con_ el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-274">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="1d4a9-275">A su vez, esto elimina la ambigüedad y la confusión de métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-275">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="1d4a9-276">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-276">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-277">Cambie la configuración de las relaciones de tipo de propiedad para usar la nueva superficie de API, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-277">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="1d4a9-278">Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales</span><span class="sxs-lookup"><span data-stu-id="1d4a9-278">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="1d4a9-279">Problema de seguimiento n.º 9005</span><span class="sxs-lookup"><span data-stu-id="1d4a9-279">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="1d4a9-280">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-280">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-281">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-281">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-282">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-282">Consider the following model:</span></span>
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
<span data-ttu-id="1d4a9-283">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, siempre era necesaria una instancia de `OrderDetails` al agregar un elemento `Order` nuevo.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-283">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="1d4a9-284">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-284">**New behavior**</span></span>

<span data-ttu-id="1d4a9-285">A partir de la versión 3.0, EF Core permite agregar `Order` sin `OrderDetails` y asigna todas las propiedades `OrderDetails` a excepción de la clave principal a columnas que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-285">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="1d4a9-286">Al realizar consultas, EF Core establece `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor o si no tiene propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-286">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="1d4a9-287">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-287">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-288">Si el modelo tiene una tabla que comparte dependencias con todas las columnas opcionales, pero la navegación que apunta a ella no se espera que sea `null`, la aplicación debería modificarse para controlar los casos en los que la navegación sea `null`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-288">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="1d4a9-289">Si esto no es posible, debería agregarse una propiedad necesaria al tipo de entidad o, al menos, una entidad debería tener un valor distinto a `null` asignado.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-289">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="1d4a9-290">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="1d4a9-290">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="1d4a9-291">Problema de seguimiento n.º 14154</span><span class="sxs-lookup"><span data-stu-id="1d4a9-291">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="1d4a9-292">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-292">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-293">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-293">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-294">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-294">Consider the following model:</span></span>
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
<span data-ttu-id="1d4a9-295">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, si solo se actualizaba `OrderDetails`, no se actualizaba el valor `Version` en el cliente y se producía un error en la próxima actualización.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-295">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="1d4a9-296">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-296">**New behavior**</span></span>

<span data-ttu-id="1d4a9-297">A partir de la versión 3.0, EF Core propaga el nuevo valor `Version` en `Order` si posee `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-297">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="1d4a9-298">En caso contrario, se produce una excepción durante la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-298">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="1d4a9-299">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-299">**Why**</span></span>

<span data-ttu-id="1d4a9-300">Este cambio se realizó para evitar un valor de token de simultaneidad obsoleto cuando solo se actualiza una de las entidades asignadas a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-300">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="1d4a9-301">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-301">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-302">Todas las entidades que comparten la tabla deben incluir una propiedad que se asigna a la columna del token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-302">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="1d4a9-303">Es posible crear una en estado reemplazado:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-303">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="1d4a9-304">Ahora, las propiedades heredadas de tipos sin asignar se asignan a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="1d4a9-304">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="1d4a9-305">Problema de seguimiento n.º 13998</span><span class="sxs-lookup"><span data-stu-id="1d4a9-305">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="1d4a9-306">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-306">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-307">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-307">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-308">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-308">Consider the following model:</span></span>
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

<span data-ttu-id="1d4a9-309">Antes de EF Core 3.0, la propiedad `ShippingAddress` se asignaba a columnas distintas para `BulkOrder` y `Order` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-309">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="1d4a9-310">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-310">**New behavior**</span></span>

<span data-ttu-id="1d4a9-311">A partir de la versión3.0, EF Core solo crea una columna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-311">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="1d4a9-312">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-312">**Why**</span></span>

<span data-ttu-id="1d4a9-313">El comportamiento anterior no era el esperado.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-313">The old behavoir was unexpected.</span></span>

<span data-ttu-id="1d4a9-314">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-314">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-315">Todavía se puede asignar explícitamente la propiedad a columnas separadas en los tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-315">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="1d4a9-316">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="1d4a9-316">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="1d4a9-317">Problema de seguimiento n.º 13274</span><span class="sxs-lookup"><span data-stu-id="1d4a9-317">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="1d4a9-318">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-318">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d4a9-319">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-319">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-320">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-320">Consider the following model:</span></span>
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
<span data-ttu-id="1d4a9-321">Antes de EF Core 3.0, se podía usar la propiedad `CustomerId` para la clave externa por convención.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-321">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="1d4a9-322">Pero si `Order` es un tipo de propiedad, entonces esto convertiría también a `CustomerId` en la clave principal, algo que no suele ser lo esperado.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-322">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="1d4a9-323">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-323">**New behavior**</span></span>

<span data-ttu-id="1d4a9-324">A partir de la versión 3.0, EF Core no intenta usar las propiedades de claves externas por convención si tienen el mismo nombre que la propiedad de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-324">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="1d4a9-325">Los patrones de nombre de tipo de entidad de seguridad concatenado con el nombre de propiedad de la entidad de seguridad y de nombre de navegación concatenado con el nombre de propiedad de la entidad de seguridad todavía se hacen coincidir.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-325">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="1d4a9-326">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-326">For example:</span></span>

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

<span data-ttu-id="1d4a9-327">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-327">**Why**</span></span>

<span data-ttu-id="1d4a9-328">Este cambio se ha realizado para evitar definir erróneamente una propiedad de clave principal en el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-328">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="1d4a9-329">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-329">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-330">Si la propiedad se ha diseñado para ser la clave externa y, por tanto, parte de la clave principal, se debe configurar explícitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-330">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="1d4a9-331">Ahora, la conexión de base de datos se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="1d4a9-331">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="1d4a9-332">Problema de seguimiento n.º 14218</span><span class="sxs-lookup"><span data-stu-id="1d4a9-332">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="1d4a9-333">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-333">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-334">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-334">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-335">Antes de EF Core 3.0, si el contexto abría la conexión dentro de `TransactionScope`, la conexión permanecía abierta mientras el ámbito actual `TransactionScope` estuviese activo.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-335">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="1d4a9-336">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-336">**New behavior**</span></span>

<span data-ttu-id="1d4a9-337">A partir de la versión 3.0, EF Core cierra la conexión en cuanto se deja de usar.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-337">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="1d4a9-338">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-338">**Why**</span></span>

<span data-ttu-id="1d4a9-339">Este cambio permite usar varios contextos en el mismo ámbito `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-339">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="1d4a9-340">El comportamiento nuevo también coincide con el de EF6.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-340">The new behavior also matches EF6.</span></span>

<span data-ttu-id="1d4a9-341">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-341">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-342">Si la conexión debe permanecer abierta, una llamada explícita a `OpenConnection()` asegurará que EF Core no la cierre de forma prematura:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-342">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="1d4a9-343">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="1d4a9-343">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="1d4a9-344">Problema de seguimiento n.º 6872</span><span class="sxs-lookup"><span data-stu-id="1d4a9-344">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="1d4a9-345">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-345">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-346">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-346">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-347">Antes de EF Core 3.0, se usaba un generador de valores compartidos para todas las propiedades de clave entera en memoria.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-347">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="1d4a9-348">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-348">**New behavior**</span></span>

<span data-ttu-id="1d4a9-349">A partir de EF Core 3.0, cada propiedad de clave entera obtiene su propio generador de valores cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-349">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="1d4a9-350">Además, si se elimina la base de datos, la generación de claves se restablece para todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-350">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="1d4a9-351">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-351">**Why**</span></span>

<span data-ttu-id="1d4a9-352">Este cambio se ha realizado para alinear la generación de claves en memoria más estrechamente a la generación de claves de base de datos reales y para mejorar la capacidad para aislar las pruebas entre sí cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-352">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="1d4a9-353">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-353">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-354">Esto puede interrumpir una aplicación que se base en el establecimiento de valores de clave específicos en memoria.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-354">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="1d4a9-355">En su lugar, considere la posibilidad de no depender de valores de clave específicos, o bien de actualizar para que coincida con el comportamiento nuevo.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-355">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="1d4a9-356">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="1d4a9-356">Backing fields are used by default</span></span>

[<span data-ttu-id="1d4a9-357">Problema de seguimiento n.º 12430</span><span class="sxs-lookup"><span data-stu-id="1d4a9-357">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="1d4a9-358">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-358">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1d4a9-359">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-359">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-360">Antes de la versión 3.0, incluso si se conocía el campo de respaldo de una propiedad, de forma predeterminada en EF Core se leía y escribía el valor de propiedad mediante los métodos captadores y establecedores de propiedades.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-360">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="1d4a9-361">La excepción era la ejecución de consultas, donde el campo de respaldo se establecía directamente si se conocía.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-361">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="1d4a9-362">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-362">**New behavior**</span></span>

<span data-ttu-id="1d4a9-363">A partir de EF Core 3.0, si se conoce el campo de respaldo para una propiedad, EF Core siempre la leerá y escribirá mediante el campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-363">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="1d4a9-364">Esto podría provocar una interrupción de la aplicación si depende de un comportamiento adicional codificado en los métodos captadores o establecedores.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-364">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="1d4a9-365">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-365">**Why**</span></span>

<span data-ttu-id="1d4a9-366">Este cambio se ha realizado para evitar que EF Core desencadene erróneamente lógica de negocios de forma predeterminada al realizar operaciones de base de datos que implican entidades.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-366">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="1d4a9-367">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-367">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-368">El comportamiento anterior a la versión 3.0 se puede restaurar mediante la configuración del modo de acceso de propiedad en `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-368">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="1d4a9-369">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-369">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="1d4a9-370">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="1d4a9-370">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="1d4a9-371">Problema de seguimiento n.º 12523</span><span class="sxs-lookup"><span data-stu-id="1d4a9-371">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="1d4a9-372">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-372">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-373">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-373">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-374">Antes de EF Core 3.0, si varios campos coincidían con las reglas para buscar el campo de respaldo de una propiedad, se elegía un campo según un orden de prioridad.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-374">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="1d4a9-375">Esto podía producir que, en caso de ambigüedad, se usara el campo incorrecto.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-375">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="1d4a9-376">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-376">**New behavior**</span></span>

<span data-ttu-id="1d4a9-377">A partir de EF Core 3.0, si varios campos coinciden con la misma propiedad, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-377">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="1d4a9-378">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-378">**Why**</span></span>

<span data-ttu-id="1d4a9-379">Este cambio se ha realizado para evitar de forma silenciosa el uso de un campo con respecto a otro cuando solo uno puede ser correcto.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-379">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="1d4a9-380">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-380">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-381">En las propiedades con campos de respaldo ambiguos se debe especificar de forma explícita el campo que se va usar.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-381">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="1d4a9-382">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-382">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="1d4a9-383">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="1d4a9-383">Field-only property names should match the field name</span></span>

<span data-ttu-id="1d4a9-384">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-384">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-385">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-385">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-386">Antes de EF Core 3.0, una propiedad podía especificarse con un valor de cadena y, si no había ninguna propiedad con ese nombre en el tipo CLR, EF Core intentaba hacerla coincidir con un campo mediante reglas de convención.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-386">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="1d4a9-387">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-387">**New behavior**</span></span>

<span data-ttu-id="1d4a9-388">A partir de EF Core 3.0, una propiedad de solo campo debe coincidir exactamente con el nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-388">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="1d4a9-389">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-389">**Why**</span></span>

<span data-ttu-id="1d4a9-390">Este cambio se realizó para evitar el uso del mismo campo para dos propiedades con nombres similares. También hace que las reglas de coincidencia para propiedades solo de campo sean las mismas que para las propiedades asignadas a propiedades CLR.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-390">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="1d4a9-391">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-391">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-392">Las propiedades solo de campo deberían tener el mismo nombre que el campo al que están asignadas.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-392">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="1d4a9-393">En una próxima versión preliminar de EF Core 3.0 tenemos planeado volver a habilitar la configuración explícita de un nombre de campo distinto al nombre de la propiedad:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-393">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="1d4a9-394">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="1d4a9-394">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="1d4a9-395">Problema de seguimiento n.° 14756</span><span class="sxs-lookup"><span data-stu-id="1d4a9-395">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="1d4a9-396">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-396">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-397">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-397">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-398">Antes de EF Core 3.0, llamar a `AddDbContext` o a `AddDbContextPool` también podría registrar los servicios de almacenamiento en caché y de registro con D.I. a través de llamadas a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) y a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-398">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="1d4a9-399">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-399">**New behavior**</span></span>

<span data-ttu-id="1d4a9-400">A partir de EF Core 3.0, `AddDbContext` y `AddDbContextPool` ya no registrarán estos servicios con inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-400">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="1d4a9-401">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-401">**Why**</span></span>

<span data-ttu-id="1d4a9-402">EF Core 3.0 no requiere que estos servicios estén en el contenedor de inserción de dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-402">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="1d4a9-403">Pero si `ILoggerFactory` se registra en el contenedor de DI de la aplicación, EF Core lo empezará a usar de todos modos.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-403">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="1d4a9-404">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-404">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-405">Si la aplicación necesita estos servicios, regístrelos de manera explícita con el contenedor de DI mediante [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-405">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="1d4a9-406">Ahora DbContext.Entry realiza una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="1d4a9-406">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="1d4a9-407">Problema de seguimiento n.º 13552</span><span class="sxs-lookup"><span data-stu-id="1d4a9-407">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1d4a9-408">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-408">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d4a9-409">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-409">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-410">Antes de EF Core 3.0, la llamada a `DbContext.Entry` provocaba que se detectaran cambios para todas las entidades con seguimiento.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-410">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="1d4a9-411">Esto garantizaba que el estado expuesto en `EntityEntry` estuviera actualizado.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-411">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="1d4a9-412">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-412">**New behavior**</span></span>

<span data-ttu-id="1d4a9-413">A partir de EF Core 3.0, ahora la llamada a `DbContext.Entry` solo intenta detectar cambios en la entidad dada y cualquier entidad de seguridad relacionada con ella de la que se haya realizado el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-413">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="1d4a9-414">Esto significa que es posible que la llamada a este método no haya detectado otros cambios, lo que podría tener implicaciones en el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-414">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="1d4a9-415">Observe que si `ChangeTracker.AutoDetectChangesEnabled` se establece en `false` incluso esta detección de cambios local se deshabilitará.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-415">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="1d4a9-416">Otros métodos que provocan la detección de cambios (como `ChangeTracker.Entries` y `SaveChanges`) siguen provocando una acción `DetectChanges` completa de todas las entidades de las que se realiza el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-416">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="1d4a9-417">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-417">**Why**</span></span>

<span data-ttu-id="1d4a9-418">Este cambio se ha realizado para mejorar el rendimiento predeterminado del uso de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-418">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="1d4a9-419">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-419">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-420">Llame a `ChgangeTracker.DetectChanges()` de forma explícita antes de llamar a `Entry` para garantizar el comportamiento anterior a la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-420">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="1d4a9-421">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="1d4a9-421">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="1d4a9-422">Problema de seguimiento n.º 14617</span><span class="sxs-lookup"><span data-stu-id="1d4a9-422">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="1d4a9-423">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-423">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-424">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-424">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-425">Antes de EF Core 3.0, se podían usar las propiedades de clave `string` y `byte[]` sin tener que establecer de forma explícita un valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-425">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="1d4a9-426">En ese caso, el valor de clave se generaba en el cliente como un GUID, que se serializaba en bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-426">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="1d4a9-427">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-427">**New behavior**</span></span>

<span data-ttu-id="1d4a9-428">A partir de EF Core 3.0, se iniciará una excepción en la que indica que no se ha establecido ningún valor de clave.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-428">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="1d4a9-429">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-429">**Why**</span></span>

<span data-ttu-id="1d4a9-430">Este cambio se ha realizado porque los valores `string`/`byte[]` generados por el cliente no suelen ser útiles, y el comportamiento predeterminado dificultaba razonar sobre los valores de clave generados de una forma habitual.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-430">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="1d4a9-431">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-431">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-432">Se puede obtener el comportamiento anterior a la versión 3.0 si se especifica de forma explícita que las propiedades de clave deben usar los valores generados si no se establece ningún otro valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-432">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="1d4a9-433">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-433">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="1d4a9-434">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-434">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="1d4a9-435">Ahora ILoggerFactory es un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="1d4a9-435">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="1d4a9-436">Problema de seguimiento n.º 14698</span><span class="sxs-lookup"><span data-stu-id="1d4a9-436">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="1d4a9-437">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-437">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d4a9-438">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-438">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-439">Antes de EF Core 3.0, `ILoggerFactory` se registraba como un servicio de singleton.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-439">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="1d4a9-440">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-440">**New behavior**</span></span>

<span data-ttu-id="1d4a9-441">A partir de EF Core 3.0, `ILoggerFactory` ahora se registra como con ámbito.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-441">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="1d4a9-442">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-442">**Why**</span></span>

<span data-ttu-id="1d4a9-443">Este cambio se ha realizado para permitir la asociación de un registrador con una instancia de `DbContext`, lo que habilita otras funciones y quita algunos casos de comportamiento patológico como un aumento vertiginoso de los proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-443">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="1d4a9-444">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-444">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-445">Este cambio no debería afectar al código de la aplicación a menos que registre y use servicios personalizados en el proveedor de servicios internos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-445">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="1d4a9-446">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-446">This isn't common.</span></span>
<span data-ttu-id="1d4a9-447">En estos casos, la mayoría de los elementos seguirá funcionando, pero cualquier servicio de singleton que dependiera de `ILoggerFactory` tendrá que cambiarse para obtener la interfaz `ILoggerFactory` de otra forma.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-447">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="1d4a9-448">Si experimenta situaciones como esta, registre un problema en el [rastreador de problemas de GitHub de EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para hacernos saber cómo usa `ILoggerFactory`, para que podamos comprender mejor cómo evitar esta interrupción en el futuro.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-448">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="1d4a9-449">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="1d4a9-449">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="1d4a9-450">Problema de seguimiento n.º 12780</span><span class="sxs-lookup"><span data-stu-id="1d4a9-450">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="1d4a9-451">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-451">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-452">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-452">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-453">Antes de EF Core 3.0, una vez que se eliminaba `DbContext` no había ninguna forma de saber si una determinada propiedad de navegación de una entidad obtenida de ese contexto se había cargado completamente o no.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-453">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="1d4a9-454">En su lugar, los proxies asumían que una navegación de referencia se cargaba si tenía un valor distinto de NULL, y que una navegación de colección se cargaba si no estaba vacía.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-454">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="1d4a9-455">En estos casos, el intento de carga diferida era no operativo.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-455">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="1d4a9-456">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-456">**New behavior**</span></span>

<span data-ttu-id="1d4a9-457">A partir de EF Core 3.0, los proxies realizan el seguimiento de si una propiedad de navegación se carga o no.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-457">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="1d4a9-458">Esto significa que el intento de acceder a una propiedad de navegación que se carga después de que se haya eliminado el contexto siempre será no operativo, incluso cuando la navegación cargada está vacía o es NULL.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-458">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="1d4a9-459">Por el contrario, el intento de acceder a una propiedad de navegación que no está cargada iniciará una excepción si el contexto se ha eliminado, incluso si la propiedad de navegación es una colección no vacía.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-459">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="1d4a9-460">Si se produce esta situación, significa que el código de aplicación está intentando usar la carga diferida en un momento no válido y que se debe cambiar la aplicación para que lo no haga.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-460">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="1d4a9-461">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-461">**Why**</span></span>

<span data-ttu-id="1d4a9-462">Este cambio se ha realizado para que el comportamiento sea coherente y correcto cuando se intenta la carga diferida de una instancia de `DbContext` eliminada.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-462">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="1d4a9-463">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-463">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-464">Actualice el código de la aplicación para que no intente la carga diferida con un contexto eliminado, o bien configúrelo para que sea no operativo, como se describe en el mensaje de la excepción.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-464">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="1d4a9-465">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="1d4a9-465">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="1d4a9-466">Problema de seguimiento n.º 10236</span><span class="sxs-lookup"><span data-stu-id="1d4a9-466">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="1d4a9-467">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-467">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d4a9-468">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-468">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-469">Antes de EF Core 3.0, se registraba una advertencia para una aplicación que creaba un número patológico de proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-469">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="1d4a9-470">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-470">**New behavior**</span></span>

<span data-ttu-id="1d4a9-471">A partir de EF Core 3.0, ahora esta advertencia se considera un error y se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-471">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="1d4a9-472">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-472">**Why**</span></span>

<span data-ttu-id="1d4a9-473">Este cambio se ha realizado para controlar mejor el código de la aplicación mediante la exposición de este caso patológico de una forma más explícita.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-473">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="1d4a9-474">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-474">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-475">Cuando se produce este error, la acción más adecuada consiste en comprender la causa raíz y detener la creación de tantos proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-475">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="1d4a9-476">Pero el error se puede convertir en una advertencia (u omitirse) mediante configuración en `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-476">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="1d4a9-477">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-477">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="1d4a9-478">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="1d4a9-478">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="1d4a9-479">Problema de seguimiento n.° 9171</span><span class="sxs-lookup"><span data-stu-id="1d4a9-479">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="1d4a9-480">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-481">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-481">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-482">Antes de EF Core 3.0, el código para llamar a `HasOne` o `HasMany` con una cadena se interpretaba de manera confusa.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-482">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="1d4a9-483">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-483">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="1d4a9-484">El código parece relacionar `Samurai` con otro tipo de entidad mediante la propiedad de navegación `Entrance`, que puede ser privada.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-484">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="1d4a9-485">En realidad, este código intenta crear una relación con algún tipo de entidad denominada `Entrance` sin ninguna propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-485">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="1d4a9-486">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-486">**New behavior**</span></span>

<span data-ttu-id="1d4a9-487">A partir de EF Core 3.0, el código anterior ahora hace lo que parecía que debía hacer antes.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-487">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="1d4a9-488">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-488">**Why**</span></span>

<span data-ttu-id="1d4a9-489">El comportamiento anterior era muy confuso, especialmente al leer el código de configuración y al buscar errores.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-489">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="1d4a9-490">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-490">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-491">Esto solo interrumpirá las aplicaciones que configuran de manera explícita las relaciones con cadenas para nombres de tipos y sin especificar explícitamente la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-491">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="1d4a9-492">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-492">This is not common.</span></span>
<span data-ttu-id="1d4a9-493">El comportamiento anterior se puede obtener al pasar de manera explícita `null` para el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-493">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="1d4a9-494">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-494">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="1d4a9-495">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="1d4a9-495">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="1d4a9-496">Problema de seguimiento n.º 15184</span><span class="sxs-lookup"><span data-stu-id="1d4a9-496">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="1d4a9-497">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-497">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-498">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-498">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-499">Antes, los siguientes métodos asincrónicos devolvían `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-499">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="1d4a9-500">`ValueGenerator.NextValueAsync()` (y las clases derivadas)</span><span class="sxs-lookup"><span data-stu-id="1d4a9-500">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="1d4a9-501">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-501">**New behavior**</span></span>

<span data-ttu-id="1d4a9-502">Dichos métodos ahora devuelven `ValueTask<T>` durante el mismo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-502">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="1d4a9-503">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-503">**Why**</span></span>

<span data-ttu-id="1d4a9-504">Este cambio reduce el número de asignaciones de montones que se producen al invocar estos métodos, lo que mejora el rendimiento general.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-504">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="1d4a9-505">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-505">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-506">Las aplicaciones que simplemente esperen las API anteriores solo necesitan recompilarse, sin que sea necesario realizar cambios en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-506">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="1d4a9-507">Un uso más complejo (p. ej., pasar el valor `Task` devuelto a `Task.WhenAny()`) normalmente requiere que el valor `ValueTask<T>` devuelto se convierta en `Task<T>` mediante una llamada a `AsTask()` en él.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-507">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="1d4a9-508">Tenga en cuenta que esto niega la reducción de asignación que implica este cambio.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-508">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="1d4a9-509">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="1d4a9-509">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="1d4a9-510">Problema de seguimiento n.º 9913</span><span class="sxs-lookup"><span data-stu-id="1d4a9-510">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="1d4a9-511">Este cambio se introdujo en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-511">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1d4a9-512">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-512">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-513">El nombre de anotación para las anotaciones de asignación de tipos era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="1d4a9-513">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="1d4a9-514">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-514">**New behavior**</span></span>

<span data-ttu-id="1d4a9-515">Ahora, el nombre de anotación para las anotaciones de asignación de tipos es "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="1d4a9-515">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="1d4a9-516">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-516">**Why**</span></span>

<span data-ttu-id="1d4a9-517">Ahora, las asignaciones de tipos se usan para algo más que solo para proveedores de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-517">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="1d4a9-518">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-518">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-519">Esto solo interrumpirá a las aplicaciones que acceden directamente a la asignación de tipos como una anotación, lo que no es habitual.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-519">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="1d4a9-520">La acción más apropiada para corregir es usar la superficie de API para acceder a las asignaciones de tipos en lugar de usar directamente la anotación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-520">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="1d4a9-521">ToTable en un tipo derivado inicia una excepción</span><span class="sxs-lookup"><span data-stu-id="1d4a9-521">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="1d4a9-522">Problema de seguimiento n.º 11811</span><span class="sxs-lookup"><span data-stu-id="1d4a9-522">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="1d4a9-523">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-523">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d4a9-524">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-524">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-525">Antes de EF Core 3.0, la llamada a `ToTable()` en un tipo derivado se omitía, ya que la única estrategia asignación de herencia era TPH, lo que no es válido.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-525">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="1d4a9-526">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-526">**New behavior**</span></span>

<span data-ttu-id="1d4a9-527">A partir de EF Core 3.0, y en preparación para agregar compatibilidad con TPT y TPC en una versión posterior, ahora la llamada a `ToTable()` en un tipo derivado iniciará una excepción para evitar un cambio de asignación inesperado en el futuro.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-527">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="1d4a9-528">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-528">**Why**</span></span>

<span data-ttu-id="1d4a9-529">En la actualidad no se considera válido asignar un tipo derivado a otra tabla.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-529">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="1d4a9-530">Este cambio evita interrupciones en el futuro, cuando se convierta en una operación válida.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-530">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="1d4a9-531">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-531">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-532">Quite todos los intentos de asignar tipos derivados a otras tablas.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-532">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="1d4a9-533">ForSqlServerHasIndex se ha reemplazado por HasIndex</span><span class="sxs-lookup"><span data-stu-id="1d4a9-533">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="1d4a9-534">Problema de seguimiento n.º 12366</span><span class="sxs-lookup"><span data-stu-id="1d4a9-534">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="1d4a9-535">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-535">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d4a9-536">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-536">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-537">Antes de EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` proporcionaba una manera de configurar las columnas que se usaban con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-537">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="1d4a9-538">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-538">**New behavior**</span></span>

<span data-ttu-id="1d4a9-539">A partir de EF Core 3.0, ya se admite el uso de `Include` en un índice en el nivel relacional.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-539">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="1d4a9-540">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-540">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="1d4a9-541">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-541">**Why**</span></span>

<span data-ttu-id="1d4a9-542">Este cambio se ha realizado para consolidar la API para índices con `Include` en un mismo lugar para todos los proveedores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-542">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="1d4a9-543">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-543">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-544">Use la API nueva, como se ha mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-544">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="1d4a9-545">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="1d4a9-545">Metadata API changes</span></span>

[<span data-ttu-id="1d4a9-546">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="1d4a9-546">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="1d4a9-547">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-548">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-548">**New behavior**</span></span>

<span data-ttu-id="1d4a9-549">Las siguientes propiedades se han convertido en métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-549">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="1d4a9-550">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-550">**Why**</span></span>

<span data-ttu-id="1d4a9-551">Este cambio simplifica la implementación de las interfaces mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-551">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="1d4a9-552">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-552">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-553">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-553">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="1d4a9-554">Cambios en la API de metadatos específicos del proveedor</span><span class="sxs-lookup"><span data-stu-id="1d4a9-554">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="1d4a9-555">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="1d4a9-555">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="1d4a9-556">Este cambio se introdujo en EF Core 3.0 (versión preliminar 6).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-556">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="1d4a9-557">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-557">**New behavior**</span></span>

<span data-ttu-id="1d4a9-558">Los métodos de extensión específicos del proveedor se simplificarán:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-558">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="1d4a9-559">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-559">**Why**</span></span>

<span data-ttu-id="1d4a9-560">Este cambio simplifica la implementación de los métodos de extensión mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-560">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="1d4a9-561">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-561">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-562">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-562">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="1d4a9-563">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="1d4a9-563">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="1d4a9-564">Problema de seguimiento n.º 12151</span><span class="sxs-lookup"><span data-stu-id="1d4a9-564">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="1d4a9-565">Este cambio se introdujo en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-565">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1d4a9-566">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-566">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-567">Antes de EF Core 3.0, EF Core enviaba `PRAGMA foreign_keys = 1` cuando se abría una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-567">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1d4a9-568">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-568">**New behavior**</span></span>

<span data-ttu-id="1d4a9-569">A partir de EF Core 3.0, EF Core ya no envía `PRAGMA foreign_keys = 1` cuando se abre una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-569">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1d4a9-570">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-570">**Why**</span></span>

<span data-ttu-id="1d4a9-571">Este cambio se ha realizado porque en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3` de forma predeterminada, lo que a su vez significa que el cumplimiento de CD está activado de forma predeterminada y no es necesario habilitarlo explícitamente cada vez que se abra una conexión.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-571">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="1d4a9-572">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-572">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-573">Las claves externas se habilitan de forma predeterminada en SQLitePCLRaw.bundle_e_sqlite3, que en EF Core se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-573">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="1d4a9-574">Para otros casos, las claves externas se pueden habilitar mediante la especificación de `Foreign Keys=True` en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-574">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="1d4a9-575">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="1d4a9-575">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="1d4a9-576">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-576">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-577">Antes de EF Core 3.0, en EF Core se usaba `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-577">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="1d4a9-578">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-578">**New behavior**</span></span>

<span data-ttu-id="1d4a9-579">A partir de EF Core 3.0, en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-579">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="1d4a9-580">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-580">**Why**</span></span>

<span data-ttu-id="1d4a9-581">Este cambio se ha realizado para que la versión de SQLite que se usa en iOS sea coherente con otras plataformas.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-581">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="1d4a9-582">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-582">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-583">Para usar la versión nativa de SQLite en iOS, configure `Microsoft.Data.Sqlite` para usar otra agrupación `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-583">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1d4a9-584">Almacenamiento de valores GUID como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="1d4a9-584">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1d4a9-585">Problema de seguimiento n.º 15078</span><span class="sxs-lookup"><span data-stu-id="1d4a9-585">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="1d4a9-586">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-586">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-587">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-587">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-588">Antes los valores GUID se almacenaban como valores BLOB en SQLite.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-588">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="1d4a9-589">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-589">**New behavior**</span></span>

<span data-ttu-id="1d4a9-590">Ahora, los valores GUID se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-590">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="1d4a9-591">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-591">**Why**</span></span>

<span data-ttu-id="1d4a9-592">El formato binario de los GUID no está normalizado.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-592">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="1d4a9-593">El almacenamiento de los valores como TEXT mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-593">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1d4a9-594">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-594">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-595">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-595">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="1d4a9-596">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-596">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="1d4a9-597">Microsoft.Data.Sqlite sigue siendo capaz de leer valores GUID de ambas columnas BLOB y TEXT. Sin embargo, dado que el formato predeterminado de los parámetros y las constantes ha cambiado, seguramente deberá realizar alguna acción en la mayoría de casos que impliquen el uso de valores GUID.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-597">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1d4a9-598">Ahora los valores char se almacenan como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="1d4a9-598">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1d4a9-599">Problema de seguimiento n.º 15020</span><span class="sxs-lookup"><span data-stu-id="1d4a9-599">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="1d4a9-600">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-600">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-601">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-601">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-602">Anteriormente los valores char se almacenaban como valores INTEGER en SQLite.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-602">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="1d4a9-603">Por ejemplo, un valor char de *A* se almacenaba como el valor entero 65.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-603">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="1d4a9-604">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-604">**New behavior**</span></span>

<span data-ttu-id="1d4a9-605">Ahora, los valores char se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-605">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="1d4a9-606">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-606">**Why**</span></span>

<span data-ttu-id="1d4a9-607">El almacenamiento de valores como TEXT es más natural y mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-607">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1d4a9-608">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-608">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-609">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-609">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="1d4a9-610">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-610">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="1d4a9-611">Microsoft.Data.Sqlite también puede leer valores de caracteres tanto de columnas INTEGER como de columnas TEXT, por lo que es posible que no deba hacer nada dependiendo de su caso.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-611">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="1d4a9-612">Ahora los id. de migración se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="1d4a9-612">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="1d4a9-613">Problema de seguimiento n.º 12978</span><span class="sxs-lookup"><span data-stu-id="1d4a9-613">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="1d4a9-614">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-614">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-615">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-615">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-616">Los identificadores de migración se generaban de forma involuntaria con el calendario de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-616">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="1d4a9-617">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-617">**New behavior**</span></span>

<span data-ttu-id="1d4a9-618">Ahora los id. de migración siempre se generan usando el calendario de la referencia cultural invariable (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-618">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="1d4a9-619">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-619">**Why**</span></span>

<span data-ttu-id="1d4a9-620">El orden de las migraciones es importante al actualizar la base de datos o al solucionar conflictos de combinación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-620">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="1d4a9-621">Al usar el calendario invariable, se evitan problemas de ordenación que pueden producirse si los miembros del equipo tienen distintos calendarios del sistema.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-621">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="1d4a9-622">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-622">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-623">Esta cambio afecta a todas las personas que usan un calendario no gregoriano en el que el año sea superior al del calendario gregoriano (como el calendario budista tailandés).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-623">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="1d4a9-624">Los id. de migración existentes deberán actualizarse para que las migraciones nuevas se ordenen después de las existentes.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-624">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="1d4a9-625">Puede ver el id. de migración en el atributo Migration de los archivos de diseñador de la migración.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-625">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="1d4a9-626">También debe actualizarse la tabla de historial de migraciones.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-626">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="1d4a9-627">La información o metadatos de la extensión se han quitado de IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="1d4a9-627">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="1d4a9-628">Problema de seguimiento n.º 16119</span><span class="sxs-lookup"><span data-stu-id="1d4a9-628">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="1d4a9-629">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-629">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1d4a9-630">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-630">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-631">`IDbContextOptionsExtension` incluía métodos para proporcionar metadatos sobre la extensión.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-631">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="1d4a9-632">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-632">**New behavior**</span></span>

<span data-ttu-id="1d4a9-633">Estos métodos se han movido a una nueva clase base abstracta `DbContextOptionsExtensionInfo`, que se devuelve desde una nueva propiedad `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-633">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="1d4a9-634">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-634">**Why**</span></span>

<span data-ttu-id="1d4a9-635">Al lanzarse las versiones 2.0 y 3.0, tuvimos que agregar o cambiar estos métodos varias veces.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-635">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="1d4a9-636">Su división en una nueva clase base abstracta facilitará la realización de este tipo de cambios sin interrumpir las extensiones existentes.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-636">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="1d4a9-637">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-637">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-638">Actualice las extensiones para seguir el nuevo patrón.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-638">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="1d4a9-639">Encontrará ejemplos en las muchas implementaciones de `IDbContextOptionsExtension` para los diferentes tipos de extensiones en el código fuente de EF Core.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-639">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="1d4a9-640">Cambio de nombre de LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="1d4a9-640">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="1d4a9-641">Problema de seguimiento n.º 10985</span><span class="sxs-lookup"><span data-stu-id="1d4a9-641">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="1d4a9-642">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-642">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-643">**Cambio**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-643">**Change**</span></span>

<span data-ttu-id="1d4a9-644">Se ha cambiado el nombre de `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="1d4a9-645">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-645">**Why**</span></span>

<span data-ttu-id="1d4a9-646">Conviene alinear el nombre de este evento de advertencia con el del resto de eventos de advertencia.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-646">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="1d4a9-647">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-647">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-648">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-648">Use the new name.</span></span> <span data-ttu-id="1d4a9-649">(Tenga en cuenta que el número de id. evento sigue siendo el mismo).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-649">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="1d4a9-650">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="1d4a9-650">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="1d4a9-651">Problema de seguimiento n.º 10730</span><span class="sxs-lookup"><span data-stu-id="1d4a9-651">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="1d4a9-652">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-652">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-653">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-653">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-654">Antes de EF Core 3.0, se utilizaba simplemente el término "nombre" para hacer referencia a los nombres de las restricciones de claves externas.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-654">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="1d4a9-655">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-655">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="1d4a9-656">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-656">**New behavior**</span></span>

<span data-ttu-id="1d4a9-657">A partir de EF Core 3.0, el término con el que se hace referencia a los nombres de las restricciones de claves externas es "nombre de la restricción".</span><span class="sxs-lookup"><span data-stu-id="1d4a9-657">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="1d4a9-658">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1d4a9-658">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="1d4a9-659">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-659">**Why**</span></span>

<span data-ttu-id="1d4a9-660">Este cambio permite mejorar la coherencia relativa a la nomenclatura en este aspecto y aclarar que se trata del nombre de una restricción de clave externa, y no del de la columna o propiedad en la que está definida la clave externa.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-660">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="1d4a9-661">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-661">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-662">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-662">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="1d4a9-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync se han hecho públicos</span><span class="sxs-lookup"><span data-stu-id="1d4a9-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="1d4a9-664">Problema de seguimiento n.° 15997</span><span class="sxs-lookup"><span data-stu-id="1d4a9-664">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="1d4a9-665">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-665">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1d4a9-666">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-666">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-667">Antes de EF Core 3.0, estos métodos estaban protegidos.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-667">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="1d4a9-668">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-668">**New behavior**</span></span>

<span data-ttu-id="1d4a9-669">Desde EF Core 3.0, estos métodos son públicos.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-669">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="1d4a9-670">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-670">**Why**</span></span>

<span data-ttu-id="1d4a9-671">EF usa estos métodos para determinar si se ha creado una base de datos, pero está vacía.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-671">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="1d4a9-672">Esto también puede resultar útil fuera de EF al determinar si se deben aplicar migraciones o no.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-672">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="1d4a9-673">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-673">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-674">Cambie la accesibilidad de cualquier invalidación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-674">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="1d4a9-675">Microsoft.EntityFrameworkCore.Design es ahora un paquete DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="1d4a9-675">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="1d4a9-676">Problema de seguimiento n.° 11506</span><span class="sxs-lookup"><span data-stu-id="1d4a9-676">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="1d4a9-677">Este cambio se introdujo en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1d4a9-678">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-678">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-679">Antes de EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un paquete NuGet regular con un ensamblado al que podían hacer referencia los proyectos que dependían de él.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-679">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="1d4a9-680">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-680">**New behavior**</span></span>

<span data-ttu-id="1d4a9-681">Desde EF Core 3.0, es un paquete DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-681">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="1d4a9-682">Esto significa que la dependencia no fluirá de manera transitiva en otros proyectos y que ya no puede, de forma predeterminada, hacer referencia a su ensamblado.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-682">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="1d4a9-683">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-683">**Why**</span></span>

<span data-ttu-id="1d4a9-684">Este paquete solo está destinado a usarse en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-684">This package is only intended to be used at design time.</span></span> <span data-ttu-id="1d4a9-685">Las aplicaciones implementadas no deben hacer referencia al mismo.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-685">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="1d4a9-686">Hacer que el paquete sea DevelopmentDependency refuerza esta recomendación.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-686">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="1d4a9-687">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-687">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-688">Si tiene que hacer referencia a este paquete para invalidar el comportamiento en tiempo de diseño de EF Core, puede actualizar los metadatos de elementos PackageReference del proyecto.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-688">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="1d4a9-689">Si se hace referencia al paquete de manera transitiva a través de Microsoft.EntityFrameworkCore.Tools, tendrá que agregar una PackageReference explícita al paquete para cambiar sus metadatos.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-689">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="1d4a9-690">SQLitePCL.raw se ha actualizado a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="1d4a9-690">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="1d4a9-691">Problema de seguimiento n.° 14824</span><span class="sxs-lookup"><span data-stu-id="1d4a9-691">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="1d4a9-692">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-692">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1d4a9-693">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-693">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-694">Microsoft.EntityFrameworkCore.Sqlite dependía anteriormente de la versión 1.1.12 de SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-694">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="1d4a9-695">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-695">**New behavior**</span></span>

<span data-ttu-id="1d4a9-696">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-696">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="1d4a9-697">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-697">**Why**</span></span>

<span data-ttu-id="1d4a9-698">La versión 2.0.0 de SQLitePCL.raw selecciona .NET Standard 2.0 como destino.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-698">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="1d4a9-699">Anteriormente seleccionaba .NET Standard 1.1 como destino, que requería el cierre a gran escala de paquetes transitivos para su funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-699">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="1d4a9-700">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-700">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-701">En la versión 2.0.0 de SQLitePCL.raw se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-701">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="1d4a9-702">Consulte las [notas de la versión](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-702">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>


## <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="1d4a9-703">NetTopologySuite se actualizó a la versión 2.0.0</span><span class="sxs-lookup"><span data-stu-id="1d4a9-703">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="1d4a9-704">Problema de seguimiento n.° 14825</span><span class="sxs-lookup"><span data-stu-id="1d4a9-704">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="1d4a9-705">Este cambio se introdujo en EF Core 3.0 (versión preliminar 7).</span><span class="sxs-lookup"><span data-stu-id="1d4a9-705">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="1d4a9-706">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-706">**Old behavior**</span></span>

<span data-ttu-id="1d4a9-707">Los paquetes espaciales anteriormente dependían de la versión 1.15.1 de NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-707">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="1d4a9-708">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-708">**New behavior**</span></span>

<span data-ttu-id="1d4a9-709">Hemos actualizado nuestro paquete para depender de la versión 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-709">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="1d4a9-710">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-710">**Why**</span></span>

<span data-ttu-id="1d4a9-711">La versión 2.0.0 de NetTopologySuite pretende resolver varios problemas de usabilidad que encontraron los usuarios de EF Core.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-711">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="1d4a9-712">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="1d4a9-712">**Mitigations**</span></span>

<span data-ttu-id="1d4a9-713">En la versión 2.0.0 de NetTopologySuite se incluyen algunos cambios importantes.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-713">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="1d4a9-714">Consulte las [notas de la versión](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) para obtener detalles.</span><span class="sxs-lookup"><span data-stu-id="1d4a9-714">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>
