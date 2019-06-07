---
title: 'Cambios importantes en EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: faae0153e0f2bdd42d3b316582dfcab88d9ceb5b
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452299"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="046bc-102">Cambios importantes incluidos en EF Core 3.0 (actualmente en versión preliminar)</span><span class="sxs-lookup"><span data-stu-id="046bc-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="046bc-103">Tenga en cuenta que los conjuntos de características y las programaciones de las versiones futuras siempre están sujetos a cambios y, aunque intentamos mantener esta página actualizada, es posible que no refleje nuestros planes más recientes en todo momento.</span><span class="sxs-lookup"><span data-stu-id="046bc-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="046bc-104">Es posible que los siguientes cambios de API y comportamiento interrumpan las aplicaciones desarrolladas para EF Core 2.2. x cuando se actualicen a la versión 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="046bc-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="046bc-105">Los cambios que esperamos que solo afecten a proveedores de base de datos se documentan en [Cambios para proveedores](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="046bc-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="046bc-106">Aquí no se documentan los cambios en las características nuevas presentadas de una versión preliminar 3.0 a otra.</span><span class="sxs-lookup"><span data-stu-id="046bc-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="046bc-107">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="046bc-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="046bc-108">[Problema de seguimiento n.° 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Consulte también el problema n.° 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="046bc-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="046bc-109">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-110">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-110">**Old behavior**</span></span>

<span data-ttu-id="046bc-111">Antes de 3.0, cuando en EF Core no se podía convertir una expresión que formaba parte de una consulta SQL o un parámetro, la expresión se evaluaba de forma automática en el cliente.</span><span class="sxs-lookup"><span data-stu-id="046bc-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="046bc-112">De forma predeterminada, la evaluación de cliente de expresiones potencialmente costosas solo desencadenaba una advertencia.</span><span class="sxs-lookup"><span data-stu-id="046bc-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="046bc-113">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-113">**New behavior**</span></span>

<span data-ttu-id="046bc-114">A partir de 3.0, en EF Core solo se permite que se evalúen en el cliente las expresiones en la proyección de nivel superior (la última llamada a `Select()` de la consulta).</span><span class="sxs-lookup"><span data-stu-id="046bc-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="046bc-115">Cuando las expresiones de otra parte de la consulta no se pueden convertir en SQL o un parámetro, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="046bc-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="046bc-116">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-116">**Why**</span></span>

<span data-ttu-id="046bc-117">La evaluación de cliente automática de las consultas permite que se ejecuten muchas consultas incluso si no se pueden convertir elementos importantes de ellas.</span><span class="sxs-lookup"><span data-stu-id="046bc-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="046bc-118">Esto puede provocar un comportamiento inesperado y potencialmente dañino que es posible que solo sea evidente en entornos de producción.</span><span class="sxs-lookup"><span data-stu-id="046bc-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="046bc-119">Por ejemplo, una condición en una llamada a `Where()` que no se puede convertir puede provocar que todas las filas de la tabla se transfieran desde el servidor de base de datos y que el filtro se aplique en el cliente.</span><span class="sxs-lookup"><span data-stu-id="046bc-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="046bc-120">Esta situación puede pasar desapercibida fácilmente si la tabla solo contiene algunas filas en la fase de desarrollo, pero ser más grave cuando la aplicación pase a producción, donde la tabla puede contener millones de filas.</span><span class="sxs-lookup"><span data-stu-id="046bc-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="046bc-121">Las advertencias de evaluación de cliente también se suelen pasar por alto durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="046bc-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="046bc-122">Además de esto, la evaluación de cliente automática puede causar problemas en los que la mejora de la traducción de consultas para expresiones específicas provocaba cambios importantes no deseados entre versiones.</span><span class="sxs-lookup"><span data-stu-id="046bc-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="046bc-123">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-123">**Mitigations**</span></span>

<span data-ttu-id="046bc-124">Si una consulta no se puede traducir totalmente, vuelva a escribirla en un formato que se pueda traducir, o bien use `AsEnumerable()`, `ToList()` o una función similar para devolver los datos al cliente de forma explícita, donde después se puedan seguir procesando mediante LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="046bc-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="046bc-125">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="046bc-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="046bc-126">Anuncios del problema de seguimiento n.º 325</span><span class="sxs-lookup"><span data-stu-id="046bc-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="046bc-127">Este cambio se ha introducido en ASP.NET Core 3.0, versión preliminar 1.</span><span class="sxs-lookup"><span data-stu-id="046bc-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="046bc-128">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-128">**Old behavior**</span></span>

<span data-ttu-id="046bc-129">Antes de ASP.NET Core 3.0, cuando se agregaba una referencia de paquete a `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, se incluía EF Core y algunos de los proveedores de datos de EF Core, como el de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="046bc-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="046bc-130">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-130">**New behavior**</span></span>

<span data-ttu-id="046bc-131">A partir de la versión 3.0, el marco compartido ASP.NET Core no incluye EF Core ni ningún proveedor de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="046bc-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="046bc-132">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-132">**Why**</span></span>

<span data-ttu-id="046bc-133">Antes de este cambio, para obtener EF Core se necesitaban varios pasos en función de si la aplicación se destinaba a ASP.NET Core y SQL Server o no.</span><span class="sxs-lookup"><span data-stu-id="046bc-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="046bc-134">Además, la actualización de ASP.NET Core forzaba la de EF Core y el proveedor de SQL Server, lo que no siempre es deseable.</span><span class="sxs-lookup"><span data-stu-id="046bc-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="046bc-135">Con este cambio, la experiencia de obtención de EF Core es la misma en todos los proveedores, implementaciones admitidas de .NET y tipos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="046bc-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="046bc-136">Ahora los desarrolladores también pueden controlar exactamente cuándo se actualizan EF Core y los proveedores de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="046bc-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="046bc-137">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-137">**Mitigations**</span></span>

<span data-ttu-id="046bc-138">Para usar EF Core en una aplicación ASP.NET Core 3.0 o cualquier otra aplicación compatible, debe agregar de forma explícita una referencia de paquete al proveedor de base de datos de EF Core que se va a usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="046bc-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="046bc-139">La herramienta de línea de comandos de EF Core, dotnet ef, ya no forma parte del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="046bc-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="046bc-140">Problema de seguimiento n.º 14016</span><span class="sxs-lookup"><span data-stu-id="046bc-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="046bc-141">Este cambio se introdujo en EF Core 3.0-preview 4 y la versión correspondiente del SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="046bc-141">This change was introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="046bc-142">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-142">**Old behavior**</span></span>

<span data-ttu-id="046bc-143">Antes de 3.0, la herramienta `dotnet ef` se incluía en el SDK de .NET Core y estaba disponible para usarse desde la línea de comandos de cualquier proyecto sin necesidad de realizar pasos adicionales.</span><span class="sxs-lookup"><span data-stu-id="046bc-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="046bc-144">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-144">**New behavior**</span></span>

<span data-ttu-id="046bc-145">A partir de la versión 3.0, el SDK de .NET no incluye la herramienta `dotnet ef`, por lo que antes de poder usarla tendrá que instalarla de forma explícita como una herramienta local o global.</span><span class="sxs-lookup"><span data-stu-id="046bc-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="046bc-146">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-146">**Why**</span></span>

<span data-ttu-id="046bc-147">Este cambio nos permite distribuir y actualizar `dotnet ef` como una herramienta convencional de la CLI de .NET en NuGet, coherente con el hecho de que la versión 3.0 de EF Core también se distribuye siempre como un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="046bc-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="046bc-148">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-148">**Mitigations**</span></span>

<span data-ttu-id="046bc-149">Para poder administrar las migraciones o aplicar la técnica de scaffolding a `DbContext`, instale `dotnet-ef` con el comando `dotnet tool install`.</span><span class="sxs-lookup"><span data-stu-id="046bc-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="046bc-150">Por ejemplo, para instalarlo como una herramienta global, puede escribir este comando:</span><span class="sxs-lookup"><span data-stu-id="046bc-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="046bc-151">También se puede obtener una herramienta local cuando se restauran las dependencias de un proyecto que la declara como una dependencia de herramientas mediante un [archivo de manifiesto de herramientas](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="046bc-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="046bc-152">FromSql, ExecuteSql y ExecuteSqlAsync han cambiado de nombre</span><span class="sxs-lookup"><span data-stu-id="046bc-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="046bc-153">Problema de seguimiento n.º 10996</span><span class="sxs-lookup"><span data-stu-id="046bc-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="046bc-154">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-154">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-155">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-155">**Old behavior**</span></span>

<span data-ttu-id="046bc-156">Antes de EF Core 3.0, estos nombres de métodos se sobrecargaban para funcionar tanto con una cadena normal como con una cadena que se debería interpolar en SQL y parámetros.</span><span class="sxs-lookup"><span data-stu-id="046bc-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="046bc-157">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-157">**New behavior**</span></span>

<span data-ttu-id="046bc-158">A partir de la versión EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw` y `ExecuteSqlRawAsync` para crear una consulta con parámetros donde los parámetros se pasan por separado de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="046bc-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="046bc-159">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="046bc-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated` y `ExecuteSqlInterpolatedAsync` para crear una consulta con parámetros donde los parámetros se pasan como parte de una cadena de consulta interpolada.</span><span class="sxs-lookup"><span data-stu-id="046bc-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="046bc-161">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="046bc-162">Tenga en cuenta que las dos consultas anteriores producirán el mismo código SQL parametrizado con los mismos parámetros SQL.</span><span class="sxs-lookup"><span data-stu-id="046bc-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="046bc-163">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-163">**Why**</span></span>

<span data-ttu-id="046bc-164">Las sobrecargas del método como esta facilitan las llamadas accidentales al método de cadena sin procesar cuando la intención era llamar al método de cadena interpolada y viceversa.</span><span class="sxs-lookup"><span data-stu-id="046bc-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="046bc-165">Esto podría resultar en consultas que no se parametrizan cuando deberían.</span><span class="sxs-lookup"><span data-stu-id="046bc-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="046bc-166">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-166">**Mitigations**</span></span>

<span data-ttu-id="046bc-167">Haga el cambio para usar los nuevos nombres de métodos.</span><span class="sxs-lookup"><span data-stu-id="046bc-167">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="046bc-168">La ejecución de consultas se registra en el nivel de depuración</span><span class="sxs-lookup"><span data-stu-id="046bc-168">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="046bc-169">Problema de seguimiento n.º 14523</span><span class="sxs-lookup"><span data-stu-id="046bc-169">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="046bc-170">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-170">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-171">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-171">**Old behavior**</span></span>

<span data-ttu-id="046bc-172">Antes de EF Core 3.0, la ejecución de consultas y otros comandos se registraba en el nivel `Info`.</span><span class="sxs-lookup"><span data-stu-id="046bc-172">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="046bc-173">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-173">**New behavior**</span></span>

<span data-ttu-id="046bc-174">A partir de EF Core 3.0, el registro de ejecución de comandos y SQL se produce en el nivel `Debug`.</span><span class="sxs-lookup"><span data-stu-id="046bc-174">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="046bc-175">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-175">**Why**</span></span>

<span data-ttu-id="046bc-176">Este cambio se ha realizado para reducir el ruido en el nivel de registro `Info`.</span><span class="sxs-lookup"><span data-stu-id="046bc-176">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="046bc-177">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-177">**Mitigations**</span></span>

<span data-ttu-id="046bc-178">Este evento de registro se define mediante `RelationalEventId.CommandExecuting` con el id. de evento 20100.</span><span class="sxs-lookup"><span data-stu-id="046bc-178">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="046bc-179">Para volver a registrar SQL en el nivel `Info`, configure explícitamente el nivel en `OnConfiguring` o en `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="046bc-179">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="046bc-180">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-180">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="046bc-181">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="046bc-181">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="046bc-182">Problema de seguimiento n.º 12378</span><span class="sxs-lookup"><span data-stu-id="046bc-182">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="046bc-183">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="046bc-183">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="046bc-184">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-184">**Old behavior**</span></span>

<span data-ttu-id="046bc-185">Antes de EF Core 3.0, los valores temporales se asignaban a todas las propiedades de clave para las que posteriormente la base de datos generaba un valor real.</span><span class="sxs-lookup"><span data-stu-id="046bc-185">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="046bc-186">Normalmente, estos valores temporales eran números negativos grandes.</span><span class="sxs-lookup"><span data-stu-id="046bc-186">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="046bc-187">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-187">**New behavior**</span></span>

<span data-ttu-id="046bc-188">A partir de la versión 3.0, en EF Core se almacena el valor de clave temporal como parte de la información de seguimiento de la entidad y la propiedad clave en sí no se modifica.</span><span class="sxs-lookup"><span data-stu-id="046bc-188">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="046bc-189">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-189">**Why**</span></span>

<span data-ttu-id="046bc-190">Este cambio se ha realizado para evitar que los valores de clave temporales se conviertan erróneamente en permanentes cuando una entidad de la que previamente una instancia de `DbContext` ha realizado el seguimiento se mueve a otra instancia de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="046bc-190">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="046bc-191">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-191">**Mitigations**</span></span>

<span data-ttu-id="046bc-192">Las aplicaciones que asignan valores de clave principal a claves externas para crear asociaciones entre entidades pueden depender del comportamiento anterior si las claves principales son generadas por el almacén y pertenecen a entidades en el estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="046bc-192">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="046bc-193">Esto se puede evitar con las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="046bc-193">This can be avoided by:</span></span>
* <span data-ttu-id="046bc-194">No se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="046bc-194">Not using store-generated keys.</span></span>
* <span data-ttu-id="046bc-195">Se establecen propiedades de navegación para crear relaciones en lugar de establecer valores de clave externa.</span><span class="sxs-lookup"><span data-stu-id="046bc-195">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="046bc-196">Se obtienen los valores de clave temporal reales de la información de seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="046bc-196">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="046bc-197">Por ejemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` devolverá el valor temporal aunque no se haya establecido `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="046bc-197">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="046bc-198">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="046bc-198">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="046bc-199">Problema de seguimiento n.º 14616</span><span class="sxs-lookup"><span data-stu-id="046bc-199">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="046bc-200">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-201">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-201">**Old behavior**</span></span>

<span data-ttu-id="046bc-202">Antes de EF Core 3.0, se realizaba el seguimiento en el estado `Added` de las entidades sin seguimiento detectadas por `DetectChanges` y se insertaban como una fila nueva cuando se llamaba a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="046bc-202">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="046bc-203">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-203">**New behavior**</span></span>

<span data-ttu-id="046bc-204">A partir de EF Core 3.0, si una entidad usa valores de clave generados y se establece un valor de clave, se realizará el seguimiento de la entidad en el estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="046bc-204">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="046bc-205">Esto significa que se supone que existe una fila para la entidad y que se actualizará cuando se llame a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="046bc-205">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="046bc-206">Si no se establece el valor de clave, o bien si el tipo de entidad no usa claves generadas, se seguirá realizando el seguimiento de la entidad nueva como `Added` al igual que en las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="046bc-206">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="046bc-207">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-207">**Why**</span></span>

<span data-ttu-id="046bc-208">Este cambio se ha realizado para que sea más sencillo y coherente trabajar con gráficos de entidades desconectadas mientras se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="046bc-208">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="046bc-209">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-209">**Mitigations**</span></span>

<span data-ttu-id="046bc-210">Este cambio puede interrumpir una aplicación si se configura un tipo de entidad para usar claves generadas, pero se establecen de forma explícita valores de clave para las instancias nuevas.</span><span class="sxs-lookup"><span data-stu-id="046bc-210">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="046bc-211">La solución consiste en configurar de forma explícita las propiedades de clave para que no usen valores generados.</span><span class="sxs-lookup"><span data-stu-id="046bc-211">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="046bc-212">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="046bc-212">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="046bc-213">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="046bc-213">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="046bc-214">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="046bc-214">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="046bc-215">Problema de seguimiento n.º 10114</span><span class="sxs-lookup"><span data-stu-id="046bc-215">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="046bc-216">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-216">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-217">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-217">**Old behavior**</span></span>

<span data-ttu-id="046bc-218">Antes de la versión 3.0, en EF Core no se aplicaban acciones en cascada (eliminación de entidades dependientes cuando se eliminaba una entidad de seguridad obligatoria o cuando se rompía la relación con una entidad de seguridad obligatoria) hasta que se llamaba a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="046bc-218">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="046bc-219">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-219">**New behavior**</span></span>

<span data-ttu-id="046bc-220">A partir de 3.0, en EF Core las acciones en cascada se aplican en cuanto se detecta la condición desencadenadora.</span><span class="sxs-lookup"><span data-stu-id="046bc-220">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="046bc-221">Por ejemplo, como resultado de la llamada a `context.Remove()` para eliminar una entidad de seguridad, todos los dependientes obligatorios relacionados de los que se realiza el seguimiento también se establecen en `Deleted` inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="046bc-221">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="046bc-222">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-222">**Why**</span></span>

<span data-ttu-id="046bc-223">Este cambio se ha realizado para mejorar la experiencia en escenarios de auditoría y enlace de datos donde es importante comprender qué entidades se van a eliminar _antes_ de llamar a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="046bc-223">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="046bc-224">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-224">**Mitigations**</span></span>

<span data-ttu-id="046bc-225">El comportamiento anterior se puede restaurar mediante opciones de `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="046bc-225">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="046bc-226">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-226">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="046bc-227">DeleteBehavior.Restrict tiene una semántica más limpia</span><span class="sxs-lookup"><span data-stu-id="046bc-227">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="046bc-228">Problema de seguimiento n.º 12661</span><span class="sxs-lookup"><span data-stu-id="046bc-228">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="046bc-229">Este cambio se introducirá en EF Core 3.0 (versión preliminar 5).</span><span class="sxs-lookup"><span data-stu-id="046bc-229">This change will be introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="046bc-230">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-230">**Old behavior**</span></span>

<span data-ttu-id="046bc-231">Antes de la versión 3.0, `DeleteBehavior.Restrict` creaba claves externas en la base de datos con la semántica `Restrict`, pero también realizaba una corrección interna de manera no evidente.</span><span class="sxs-lookup"><span data-stu-id="046bc-231">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="046bc-232">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-232">**New behavior**</span></span>

<span data-ttu-id="046bc-233">A partir de la versión 3.0, `DeleteBehavior.Restrict` garantiza que las claves externas se crean con la semántica `Restrict`, es decir, sin cascadas y realizando una infracción de restricción, sin llevar a cabo correcciones internas de EF.</span><span class="sxs-lookup"><span data-stu-id="046bc-233">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="046bc-234">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-234">**Why**</span></span>

<span data-ttu-id="046bc-235">Este cambio se realizó para mejorar la experiencia de uso de `DeleteBehavior` de manera intuitiva sin efectos secundarios inesperados.</span><span class="sxs-lookup"><span data-stu-id="046bc-235">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="046bc-236">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-236">**Mitigations**</span></span>

<span data-ttu-id="046bc-237">El comportamiento anterior se puede restaurar con `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="046bc-237">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="046bc-238">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="046bc-238">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="046bc-239">Problema de seguimiento n.º 14194</span><span class="sxs-lookup"><span data-stu-id="046bc-239">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="046bc-240">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-240">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-241">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-241">**Old behavior**</span></span>

<span data-ttu-id="046bc-242">Antes de EF Core 3.0, los [tipos de consulta](xref:core/modeling/query-types) eran un medio para consultar los datos que no definen una clave principal de una manera estructurada.</span><span class="sxs-lookup"><span data-stu-id="046bc-242">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="046bc-243">Es decir, un tipo de consulta se usaba para asignar tipos de entidad sin claves (más probablemente desde una vista, pero posiblemente desde una tabla), mientras que un tipo de entidad estándar se usaba cuando había una clave disponible (más probablemente desde una tabla, pero posiblemente desde una vista).</span><span class="sxs-lookup"><span data-stu-id="046bc-243">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="046bc-244">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-244">**New behavior**</span></span>

<span data-ttu-id="046bc-245">Ahora un tipo de consulta se convierte en un tipo de entidad sin una clave principal.</span><span class="sxs-lookup"><span data-stu-id="046bc-245">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="046bc-246">Los tipos de entidad sin clave tienen la misma funcionalidad que los tipos de consulta de las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="046bc-246">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="046bc-247">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-247">**Why**</span></span>

<span data-ttu-id="046bc-248">Este cambio se ha realizado para reducir la confusión en torno a la finalidad de los tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="046bc-248">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="046bc-249">En concreto, son tipos de entidad sin clave y, por ello, intrínsecamente son de solo lectura, pero no se deben usar solo porque un tipo de entidad tenga que ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="046bc-249">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="046bc-250">Del mismo modo, se suelen asignar a vistas, pero solo porque las vistas no suelen definir claves.</span><span class="sxs-lookup"><span data-stu-id="046bc-250">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="046bc-251">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-251">**Mitigations**</span></span>

<span data-ttu-id="046bc-252">Los elementos siguientes de la API ahora están obsoletos:</span><span class="sxs-lookup"><span data-stu-id="046bc-252">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="046bc-253">**`ModelBuilder.Query<>()`** : en su lugar es necesario llamar a `ModelBuilder.Entity<>().HasNoKey()` para marcar un tipo de entidad como sin claves.</span><span class="sxs-lookup"><span data-stu-id="046bc-253">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="046bc-254">Esto todavía no se configurará por convención para evitar una configuración incorrecta cuando se espera una clave principal, pero no coincide con la convención.</span><span class="sxs-lookup"><span data-stu-id="046bc-254">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="046bc-255">**`DbQuery<>`** : en su lugar se debe usar `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="046bc-255">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="046bc-256">**`DbContext.Query<>()`** : en su lugar se debe usar `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="046bc-256">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="046bc-257">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="046bc-257">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="046bc-258">[Problema de seguimiento n.º 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema de seguimiento n.º 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema de seguimiento n.º 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="046bc-258">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="046bc-259">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-259">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-260">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-260">**Old behavior**</span></span>

<span data-ttu-id="046bc-261">Antes de EF Core 3.0, la configuración de la relación de propiedad se realizaba directamente después de la llamada a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="046bc-261">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="046bc-262">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-262">**New behavior**</span></span>

<span data-ttu-id="046bc-263">A partir de EF Core 3.0, ahora hay una API fluida para configurar una propiedad de navegación para el propietario mediante `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="046bc-263">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="046bc-264">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-264">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="046bc-265">La configuración relacionada con la relación entre el propietario y lo que se posee ahora se debe encadenar después de `WithOwner()`, de forma similar a cómo se configuran otras relaciones.</span><span class="sxs-lookup"><span data-stu-id="046bc-265">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="046bc-266">Pero la configuración del propio tipo de propiedad se seguirá encadenando después de `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="046bc-266">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="046bc-267">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-267">For example:</span></span>

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

<span data-ttu-id="046bc-268">Además, la llamada a `Entity()`, `HasOne()` o `Set()` con un tipo de propiedad de destino ahora iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="046bc-268">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="046bc-269">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-269">**Why**</span></span>

<span data-ttu-id="046bc-270">Este cambio se ha realizado para crear una separación más clara entre la configuración del propio tipo de propiedad y la _relación con_ el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="046bc-270">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="046bc-271">A su vez, esto elimina la ambigüedad y la confusión de métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="046bc-271">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="046bc-272">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-272">**Mitigations**</span></span>

<span data-ttu-id="046bc-273">Cambie la configuración de las relaciones de tipo de propiedad para usar la nueva superficie de API, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="046bc-273">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="046bc-274">Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales</span><span class="sxs-lookup"><span data-stu-id="046bc-274">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="046bc-275">Problema de seguimiento n.º 9005</span><span class="sxs-lookup"><span data-stu-id="046bc-275">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="046bc-276">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-276">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-277">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-277">**Old behavior**</span></span>

<span data-ttu-id="046bc-278">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="046bc-278">Consider the following model:</span></span>
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
<span data-ttu-id="046bc-279">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, siempre era necesaria una instancia de `OrderDetails` al agregar un elemento `Order` nuevo.</span><span class="sxs-lookup"><span data-stu-id="046bc-279">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="046bc-280">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-280">**New behavior**</span></span>

<span data-ttu-id="046bc-281">A partir de la versión 3.0, EF Core permite agregar `Order` sin `OrderDetails` y asigna todas las propiedades `OrderDetails` a excepción de la clave principal a columnas que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="046bc-281">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="046bc-282">Al realizar consultas, EF Core establece `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor o si no tiene propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.</span><span class="sxs-lookup"><span data-stu-id="046bc-282">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="046bc-283">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-283">**Mitigations**</span></span>

<span data-ttu-id="046bc-284">Si el modelo tiene una tabla que comparte dependencias con todas las columnas opcionales, pero la navegación que apunta a ella no se espera que sea `null`, la aplicación debería modificarse para controlar los casos en los que la navegación sea `null`.</span><span class="sxs-lookup"><span data-stu-id="046bc-284">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="046bc-285">Si esto no es posible, debería agregarse una propiedad necesaria al tipo de entidad o, al menos, una entidad debería tener un valor distinto a `null` asignado.</span><span class="sxs-lookup"><span data-stu-id="046bc-285">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="046bc-286">Todas las entidades que compartan una tabla con una columna de token de simultaneidad tienen que asignarla a una propiedad</span><span class="sxs-lookup"><span data-stu-id="046bc-286">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="046bc-287">Problema de seguimiento n.º 14154</span><span class="sxs-lookup"><span data-stu-id="046bc-287">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="046bc-288">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-288">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-289">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-289">**Old behavior**</span></span>

<span data-ttu-id="046bc-290">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="046bc-290">Consider the following model:</span></span>
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
<span data-ttu-id="046bc-291">Antes de EF Core 3.0, si `OrderDetails` era propiedad de `Order` o estaba asignado explícitamente a la misma tabla, si solo se actualizaba `OrderDetails`, no se actualizaba el valor `Version` en el cliente y se producía un error en la próxima actualización.</span><span class="sxs-lookup"><span data-stu-id="046bc-291">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="046bc-292">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-292">**New behavior**</span></span>

<span data-ttu-id="046bc-293">A partir de la versión 3.0, EF Core propaga el nuevo valor `Version` en `Order` si posee `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="046bc-293">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="046bc-294">En caso contrario, se produce una excepción durante la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="046bc-294">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="046bc-295">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-295">**Why**</span></span>

<span data-ttu-id="046bc-296">Este cambio se realizó para evitar un valor de token de simultaneidad obsoleto cuando solo se actualiza una de las entidades asignadas a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="046bc-296">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="046bc-297">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-297">**Mitigations**</span></span>

<span data-ttu-id="046bc-298">Todas las entidades que comparten la tabla deben incluir una propiedad que se asigna a la columna del token de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="046bc-298">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="046bc-299">Es posible crear una en estado reemplazado:</span><span class="sxs-lookup"><span data-stu-id="046bc-299">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="046bc-300">Ahora, las propiedades heredadas de tipos sin asignar se asignan a una única columna para todos los tipos derivados</span><span class="sxs-lookup"><span data-stu-id="046bc-300">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="046bc-301">Problema de seguimiento n.º 13998</span><span class="sxs-lookup"><span data-stu-id="046bc-301">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="046bc-302">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-302">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-303">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-303">**Old behavior**</span></span>

<span data-ttu-id="046bc-304">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="046bc-304">Consider the following model:</span></span>
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

<span data-ttu-id="046bc-305">Antes de EF Core 3.0, la propiedad `ShippingAddress` se asignaba a columnas distintas para `BulkOrder` y `Order` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="046bc-305">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="046bc-306">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-306">**New behavior**</span></span>

<span data-ttu-id="046bc-307">A partir de la versión3.0, EF Core solo crea una columna para `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="046bc-307">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="046bc-308">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-308">**Why**</span></span>

<span data-ttu-id="046bc-309">El comportamiento anterior no era el esperado.</span><span class="sxs-lookup"><span data-stu-id="046bc-309">The old behavoir was unexpected.</span></span>

<span data-ttu-id="046bc-310">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-310">**Mitigations**</span></span>

<span data-ttu-id="046bc-311">Todavía se puede asignar explícitamente la propiedad a columnas separadas en los tipos derivados:</span><span class="sxs-lookup"><span data-stu-id="046bc-311">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="046bc-312">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="046bc-312">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="046bc-313">Problema de seguimiento n.º 13274</span><span class="sxs-lookup"><span data-stu-id="046bc-313">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="046bc-314">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-314">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-315">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-315">**Old behavior**</span></span>

<span data-ttu-id="046bc-316">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="046bc-316">Consider the following model:</span></span>
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
<span data-ttu-id="046bc-317">Antes de EF Core 3.0, se podía usar la propiedad `CustomerId` para la clave externa por convención.</span><span class="sxs-lookup"><span data-stu-id="046bc-317">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="046bc-318">Pero si `Order` es un tipo de propiedad, entonces esto convertiría también a `CustomerId` en la clave principal, algo que no suele ser lo esperado.</span><span class="sxs-lookup"><span data-stu-id="046bc-318">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="046bc-319">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-319">**New behavior**</span></span>

<span data-ttu-id="046bc-320">A partir de la versión 3.0, EF Core no intenta usar las propiedades de claves externas por convención si tienen el mismo nombre que la propiedad de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="046bc-320">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="046bc-321">Los patrones de nombre de tipo de entidad de seguridad concatenado con el nombre de propiedad de la entidad de seguridad y de nombre de navegación concatenado con el nombre de propiedad de la entidad de seguridad todavía se hacen coincidir.</span><span class="sxs-lookup"><span data-stu-id="046bc-321">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="046bc-322">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-322">For example:</span></span>

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

<span data-ttu-id="046bc-323">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-323">**Why**</span></span>

<span data-ttu-id="046bc-324">Este cambio se ha realizado para evitar definir erróneamente una propiedad de clave principal en el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="046bc-324">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="046bc-325">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-325">**Mitigations**</span></span>

<span data-ttu-id="046bc-326">Si la propiedad se ha diseñado para ser la clave externa y, por tanto, parte de la clave principal, se debe configurar explícitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="046bc-326">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="046bc-327">Ahora, la conexión de base de datos se cierra si ya no se usa antes de que se complete TransactionScope</span><span class="sxs-lookup"><span data-stu-id="046bc-327">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="046bc-328">Problema de seguimiento n.º 14218</span><span class="sxs-lookup"><span data-stu-id="046bc-328">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="046bc-329">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-329">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-330">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-330">**Old behavior**</span></span>

<span data-ttu-id="046bc-331">Antes de EF Core 3.0, si el contexto abría la conexión dentro de `TransactionScope`, la conexión permanecía abierta mientras el ámbito actual `TransactionScope` estuviese activo.</span><span class="sxs-lookup"><span data-stu-id="046bc-331">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="046bc-332">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-332">**New behavior**</span></span>

<span data-ttu-id="046bc-333">A partir de la versión 3.0, EF Core cierra la conexión en cuanto se deja de usar.</span><span class="sxs-lookup"><span data-stu-id="046bc-333">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="046bc-334">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-334">**Why**</span></span>

<span data-ttu-id="046bc-335">Este cambio permite usar varios contextos en el mismo ámbito `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="046bc-335">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="046bc-336">El comportamiento nuevo también coincide con el de EF6.</span><span class="sxs-lookup"><span data-stu-id="046bc-336">The new behavior also matches EF6.</span></span>

<span data-ttu-id="046bc-337">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-337">**Mitigations**</span></span>

<span data-ttu-id="046bc-338">Si la conexión debe permanecer abierta, una llamada explícita a `OpenConnection()` asegurará que EF Core no la cierre de forma prematura:</span><span class="sxs-lookup"><span data-stu-id="046bc-338">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="046bc-339">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="046bc-339">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="046bc-340">Problema de seguimiento n.º 6872</span><span class="sxs-lookup"><span data-stu-id="046bc-340">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="046bc-341">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-341">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-342">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-342">**Old behavior**</span></span>

<span data-ttu-id="046bc-343">Antes de EF Core 3.0, se usaba un generador de valores compartidos para todas las propiedades de clave entera en memoria.</span><span class="sxs-lookup"><span data-stu-id="046bc-343">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="046bc-344">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-344">**New behavior**</span></span>

<span data-ttu-id="046bc-345">A partir de EF Core 3.0, cada propiedad de clave entera obtiene su propio generador de valores cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="046bc-345">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="046bc-346">Además, si se elimina la base de datos, la generación de claves se restablece para todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="046bc-346">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="046bc-347">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-347">**Why**</span></span>

<span data-ttu-id="046bc-348">Este cambio se ha realizado para alinear la generación de claves en memoria más estrechamente a la generación de claves de base de datos reales y para mejorar la capacidad para aislar las pruebas entre sí cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="046bc-348">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="046bc-349">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-349">**Mitigations**</span></span>

<span data-ttu-id="046bc-350">Esto puede interrumpir una aplicación que se base en el establecimiento de valores de clave específicos en memoria.</span><span class="sxs-lookup"><span data-stu-id="046bc-350">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="046bc-351">En su lugar, considere la posibilidad de no depender de valores de clave específicos, o bien de actualizar para que coincida con el comportamiento nuevo.</span><span class="sxs-lookup"><span data-stu-id="046bc-351">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="046bc-352">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="046bc-352">Backing fields are used by default</span></span>

[<span data-ttu-id="046bc-353">Problema de seguimiento n.º 12430</span><span class="sxs-lookup"><span data-stu-id="046bc-353">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="046bc-354">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="046bc-354">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="046bc-355">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-355">**Old behavior**</span></span>

<span data-ttu-id="046bc-356">Antes de la versión 3.0, incluso si se conocía el campo de respaldo de una propiedad, de forma predeterminada en EF Core se leía y escribía el valor de propiedad mediante los métodos captadores y establecedores de propiedades.</span><span class="sxs-lookup"><span data-stu-id="046bc-356">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="046bc-357">La excepción era la ejecución de consultas, donde el campo de respaldo se establecía directamente si se conocía.</span><span class="sxs-lookup"><span data-stu-id="046bc-357">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="046bc-358">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-358">**New behavior**</span></span>

<span data-ttu-id="046bc-359">A partir de EF Core 3.0, si se conoce el campo de respaldo para una propiedad, EF Core siempre la leerá y escribirá mediante el campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="046bc-359">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="046bc-360">Esto podría provocar una interrupción de la aplicación si depende de un comportamiento adicional codificado en los métodos captadores o establecedores.</span><span class="sxs-lookup"><span data-stu-id="046bc-360">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="046bc-361">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-361">**Why**</span></span>

<span data-ttu-id="046bc-362">Este cambio se ha realizado para evitar que EF Core desencadene erróneamente lógica de negocios de forma predeterminada al realizar operaciones de base de datos que implican entidades.</span><span class="sxs-lookup"><span data-stu-id="046bc-362">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="046bc-363">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-363">**Mitigations**</span></span>

<span data-ttu-id="046bc-364">El comportamiento anterior a la versión 3.0 se puede restaurar mediante la configuración del modo de acceso de propiedad de la API fluida modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="046bc-364">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="046bc-365">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-365">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="046bc-366">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="046bc-366">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="046bc-367">Problema de seguimiento n.º 12523</span><span class="sxs-lookup"><span data-stu-id="046bc-367">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="046bc-368">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-368">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-369">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-369">**Old behavior**</span></span>

<span data-ttu-id="046bc-370">Antes de EF Core 3.0, si varios campos coincidían con las reglas para buscar el campo de respaldo de una propiedad, se elegía un campo según un orden de prioridad.</span><span class="sxs-lookup"><span data-stu-id="046bc-370">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="046bc-371">Esto podía producir que, en caso de ambigüedad, se usara el campo incorrecto.</span><span class="sxs-lookup"><span data-stu-id="046bc-371">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="046bc-372">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-372">**New behavior**</span></span>

<span data-ttu-id="046bc-373">A partir de EF Core 3.0, si varios campos coinciden con la misma propiedad, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="046bc-373">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="046bc-374">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-374">**Why**</span></span>

<span data-ttu-id="046bc-375">Este cambio se ha realizado para evitar de forma silenciosa el uso de un campo con respecto a otro cuando solo uno puede ser correcto.</span><span class="sxs-lookup"><span data-stu-id="046bc-375">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="046bc-376">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-376">**Mitigations**</span></span>

<span data-ttu-id="046bc-377">En las propiedades con campos de respaldo ambiguos se debe especificar de forma explícita el campo que se va usar.</span><span class="sxs-lookup"><span data-stu-id="046bc-377">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="046bc-378">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="046bc-378">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="046bc-379">Los nombres de propiedades de solo campo deben coincidir con el nombre del campo</span><span class="sxs-lookup"><span data-stu-id="046bc-379">Field-only property names should match the field name</span></span>

<span data-ttu-id="046bc-380">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-380">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-381">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-381">**Old behavior**</span></span>

<span data-ttu-id="046bc-382">Antes de EF Core 3.0, una propiedad podía especificarse con un valor de cadena y, si no había ninguna propiedad con ese nombre en el tipo CLR, EF Core intentaba hacerla coincidir con un campo mediante reglas de convención.</span><span class="sxs-lookup"><span data-stu-id="046bc-382">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="046bc-383">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-383">**New behavior**</span></span>

<span data-ttu-id="046bc-384">A partir de EF Core 3.0, una propiedad de solo campo debe coincidir exactamente con el nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="046bc-384">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="046bc-385">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-385">**Why**</span></span>

<span data-ttu-id="046bc-386">Este cambio se realizó para evitar el uso del mismo campo para dos propiedades con nombres similares. También hace que las reglas de coincidencia para propiedades solo de campo sean las mismas que para las propiedades asignadas a propiedades CLR.</span><span class="sxs-lookup"><span data-stu-id="046bc-386">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="046bc-387">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-387">**Mitigations**</span></span>

<span data-ttu-id="046bc-388">Las propiedades solo de campo deberían tener el mismo nombre que el campo al que están asignadas.</span><span class="sxs-lookup"><span data-stu-id="046bc-388">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="046bc-389">En una próxima versión preliminar de EF Core 3.0 tenemos planeado volver a habilitar la configuración explícita de un nombre de campo distinto al nombre de la propiedad:</span><span class="sxs-lookup"><span data-stu-id="046bc-389">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="046bc-390">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="046bc-390">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="046bc-391">Problema de seguimiento n.° 14756</span><span class="sxs-lookup"><span data-stu-id="046bc-391">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="046bc-392">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-392">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-393">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-393">**Old behavior**</span></span>

<span data-ttu-id="046bc-394">Antes de EF Core 3.0, llamar a `AddDbContext` o a `AddDbContextPool` también podría registrar los servicios de almacenamiento en caché y de registro con D.I. a través de llamadas a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) y a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="046bc-394">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="046bc-395">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-395">**New behavior**</span></span>

<span data-ttu-id="046bc-396">A partir de EF Core 3.0, `AddDbContext` y `AddDbContextPool` ya no registrarán estos servicios con inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="046bc-396">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="046bc-397">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-397">**Why**</span></span>

<span data-ttu-id="046bc-398">EF Core 3.0 no requiere que estos servicios estén en el contenedor de inserción de dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="046bc-398">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="046bc-399">Pero si `ILoggerFactory` se registra en el contenedor de DI de la aplicación, EF Core lo empezará a usar de todos modos.</span><span class="sxs-lookup"><span data-stu-id="046bc-399">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="046bc-400">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-400">**Mitigations**</span></span>

<span data-ttu-id="046bc-401">Si la aplicación necesita estos servicios, regístrelos de manera explícita con el contenedor de DI mediante [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="046bc-401">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="046bc-402">Ahora DbContext.Entry realiza una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="046bc-402">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="046bc-403">Problema de seguimiento n.º 13552</span><span class="sxs-lookup"><span data-stu-id="046bc-403">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="046bc-404">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-404">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-405">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-405">**Old behavior**</span></span>

<span data-ttu-id="046bc-406">Antes de EF Core 3.0, la llamada a `DbContext.Entry` provocaba que se detectaran cambios para todas las entidades con seguimiento.</span><span class="sxs-lookup"><span data-stu-id="046bc-406">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="046bc-407">Esto garantizaba que el estado expuesto en `EntityEntry` estuviera actualizado.</span><span class="sxs-lookup"><span data-stu-id="046bc-407">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="046bc-408">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-408">**New behavior**</span></span>

<span data-ttu-id="046bc-409">A partir de EF Core 3.0, ahora la llamada a `DbContext.Entry` solo intenta detectar cambios en la entidad dada y cualquier entidad de seguridad relacionada con ella de la que se haya realizado el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="046bc-409">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="046bc-410">Esto significa que es posible que la llamada a este método no haya detectado otros cambios, lo que podría tener implicaciones en el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="046bc-410">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="046bc-411">Observe que si `ChangeTracker.AutoDetectChangesEnabled` se establece en `false` incluso esta detección de cambios local se deshabilitará.</span><span class="sxs-lookup"><span data-stu-id="046bc-411">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="046bc-412">Otros métodos que provocan la detección de cambios (como `ChangeTracker.Entries` y `SaveChanges`) siguen provocando una acción `DetectChanges` completa de todas las entidades de las que se realiza el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="046bc-412">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="046bc-413">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-413">**Why**</span></span>

<span data-ttu-id="046bc-414">Este cambio se ha realizado para mejorar el rendimiento predeterminado del uso de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="046bc-414">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="046bc-415">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-415">**Mitigations**</span></span>

<span data-ttu-id="046bc-416">Llame a `ChgangeTracker.DetectChanges()` de forma explícita antes de llamar a `Entry` para garantizar el comportamiento anterior a la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="046bc-416">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="046bc-417">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="046bc-417">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="046bc-418">Problema de seguimiento n.º 14617</span><span class="sxs-lookup"><span data-stu-id="046bc-418">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="046bc-419">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-419">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-420">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-420">**Old behavior**</span></span>

<span data-ttu-id="046bc-421">Antes de EF Core 3.0, se podían usar las propiedades de clave `string` y `byte[]` sin tener que establecer de forma explícita un valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="046bc-421">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="046bc-422">En ese caso, el valor de clave se generaba en el cliente como un GUID, que se serializaba en bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="046bc-422">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="046bc-423">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-423">**New behavior**</span></span>

<span data-ttu-id="046bc-424">A partir de EF Core 3.0, se iniciará una excepción en la que indica que no se ha establecido ningún valor de clave.</span><span class="sxs-lookup"><span data-stu-id="046bc-424">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="046bc-425">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-425">**Why**</span></span>

<span data-ttu-id="046bc-426">Este cambio se ha realizado porque los valores `string`/`byte[]` generados por el cliente no suelen ser útiles, y el comportamiento predeterminado dificultaba razonar sobre los valores de clave generados de una forma habitual.</span><span class="sxs-lookup"><span data-stu-id="046bc-426">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="046bc-427">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-427">**Mitigations**</span></span>

<span data-ttu-id="046bc-428">Se puede obtener el comportamiento anterior a la versión 3.0 si se especifica de forma explícita que las propiedades de clave deben usar los valores generados si no se establece ningún otro valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="046bc-428">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="046bc-429">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="046bc-429">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="046bc-430">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="046bc-430">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="046bc-431">Ahora ILoggerFactory es un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="046bc-431">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="046bc-432">Problema de seguimiento n.º 14698</span><span class="sxs-lookup"><span data-stu-id="046bc-432">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="046bc-433">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-433">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-434">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-434">**Old behavior**</span></span>

<span data-ttu-id="046bc-435">Antes de EF Core 3.0, `ILoggerFactory` se registraba como un servicio de singleton.</span><span class="sxs-lookup"><span data-stu-id="046bc-435">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="046bc-436">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-436">**New behavior**</span></span>

<span data-ttu-id="046bc-437">A partir de EF Core 3.0, `ILoggerFactory` ahora se registra como con ámbito.</span><span class="sxs-lookup"><span data-stu-id="046bc-437">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="046bc-438">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-438">**Why**</span></span>

<span data-ttu-id="046bc-439">Este cambio se ha realizado para permitir la asociación de un registrador con una instancia de `DbContext`, lo que habilita otras funciones y quita algunos casos de comportamiento patológico como un aumento vertiginoso de los proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="046bc-439">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="046bc-440">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-440">**Mitigations**</span></span>

<span data-ttu-id="046bc-441">Este cambio no debería afectar al código de la aplicación a menos que registre y use servicios personalizados en el proveedor de servicios internos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="046bc-441">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="046bc-442">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="046bc-442">This isn't common.</span></span>
<span data-ttu-id="046bc-443">En estos casos, la mayoría de los elementos seguirá funcionando, pero cualquier servicio de singleton que dependiera de `ILoggerFactory` tendrá que cambiarse para obtener la interfaz `ILoggerFactory` de otra forma.</span><span class="sxs-lookup"><span data-stu-id="046bc-443">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="046bc-444">Si experimenta situaciones como esta, registre un problema en el [rastreador de problemas de GitHub de EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para hacernos saber cómo usa `ILoggerFactory`, para que podamos comprender mejor cómo evitar esta interrupción en el futuro.</span><span class="sxs-lookup"><span data-stu-id="046bc-444">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="046bc-445">IDbContextOptionsExtensionWithDebugInfo se ha fusionado en IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="046bc-445">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="046bc-446">Problema de seguimiento n.º 13552</span><span class="sxs-lookup"><span data-stu-id="046bc-446">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="046bc-447">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-447">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-448">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-448">**Old behavior**</span></span>

<span data-ttu-id="046bc-449">`IDbContextOptionsExtensionWithDebugInfo` era una interfaz opcional adicional ampliada de `IDbContextOptionsExtension` para evitar realizar cambios importantes en la interfaz durante el ciclo de versiones 2.x.</span><span class="sxs-lookup"><span data-stu-id="046bc-449">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="046bc-450">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-450">**New behavior**</span></span>

<span data-ttu-id="046bc-451">Ahora las interfaces se combinan en `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="046bc-451">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="046bc-452">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-452">**Why**</span></span>

<span data-ttu-id="046bc-453">Este cambio se ha realizado porque las interfaces son conceptualmente una.</span><span class="sxs-lookup"><span data-stu-id="046bc-453">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="046bc-454">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-454">**Mitigations**</span></span>

<span data-ttu-id="046bc-455">Cualquier implementación de `IDbContextOptionsExtension` tendrá que actualizarse para admitir el miembro nuevo.</span><span class="sxs-lookup"><span data-stu-id="046bc-455">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="046bc-456">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="046bc-456">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="046bc-457">Problema de seguimiento n.º 12780</span><span class="sxs-lookup"><span data-stu-id="046bc-457">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="046bc-458">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-458">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-459">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-459">**Old behavior**</span></span>

<span data-ttu-id="046bc-460">Antes de EF Core 3.0, una vez que se eliminaba `DbContext` no había ninguna forma de saber si una determinada propiedad de navegación de una entidad obtenida de ese contexto se había cargado completamente o no.</span><span class="sxs-lookup"><span data-stu-id="046bc-460">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="046bc-461">En su lugar, los proxies asumían que una navegación de referencia se cargaba si tenía un valor distinto de NULL, y que una navegación de colección se cargaba si no estaba vacía.</span><span class="sxs-lookup"><span data-stu-id="046bc-461">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="046bc-462">En estos casos, el intento de carga diferida era no operativo.</span><span class="sxs-lookup"><span data-stu-id="046bc-462">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="046bc-463">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-463">**New behavior**</span></span>

<span data-ttu-id="046bc-464">A partir de EF Core 3.0, los proxies realizan el seguimiento de si una propiedad de navegación se carga o no.</span><span class="sxs-lookup"><span data-stu-id="046bc-464">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="046bc-465">Esto significa que el intento de acceder a una propiedad de navegación que se carga después de que se haya eliminado el contexto siempre será no operativo, incluso cuando la navegación cargada está vacía o es NULL.</span><span class="sxs-lookup"><span data-stu-id="046bc-465">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="046bc-466">Por el contrario, el intento de acceder a una propiedad de navegación que no está cargada iniciará una excepción si el contexto se ha eliminado, incluso si la propiedad de navegación es una colección no vacía.</span><span class="sxs-lookup"><span data-stu-id="046bc-466">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="046bc-467">Si se produce esta situación, significa que el código de aplicación está intentando usar la carga diferida en un momento no válido y que se debe cambiar la aplicación para que lo no haga.</span><span class="sxs-lookup"><span data-stu-id="046bc-467">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="046bc-468">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-468">**Why**</span></span>

<span data-ttu-id="046bc-469">Este cambio se ha realizado para que el comportamiento sea coherente y correcto cuando se intenta la carga diferida de una instancia de `DbContext` eliminada.</span><span class="sxs-lookup"><span data-stu-id="046bc-469">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="046bc-470">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-470">**Mitigations**</span></span>

<span data-ttu-id="046bc-471">Actualice el código de la aplicación para que no intente la carga diferida con un contexto eliminado, o bien configúrelo para que sea no operativo, como se describe en el mensaje de la excepción.</span><span class="sxs-lookup"><span data-stu-id="046bc-471">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="046bc-472">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="046bc-472">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="046bc-473">Problema de seguimiento n.º 10236</span><span class="sxs-lookup"><span data-stu-id="046bc-473">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="046bc-474">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-474">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-475">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-475">**Old behavior**</span></span>

<span data-ttu-id="046bc-476">Antes de EF Core 3.0, se registraba una advertencia para una aplicación que creaba un número patológico de proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="046bc-476">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="046bc-477">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-477">**New behavior**</span></span>

<span data-ttu-id="046bc-478">A partir de EF Core 3.0, ahora esta advertencia se considera un error y se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="046bc-478">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="046bc-479">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-479">**Why**</span></span>

<span data-ttu-id="046bc-480">Este cambio se ha realizado para controlar mejor el código de la aplicación mediante la exposición de este caso patológico de una forma más explícita.</span><span class="sxs-lookup"><span data-stu-id="046bc-480">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="046bc-481">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-481">**Mitigations**</span></span>

<span data-ttu-id="046bc-482">Cuando se produce este error, la acción más adecuada consiste en comprender la causa raíz y detener la creación de tantos proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="046bc-482">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="046bc-483">Pero el error se puede convertir en una advertencia (u omitirse) mediante configuración en `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="046bc-483">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="046bc-484">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-484">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="046bc-485">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="046bc-485">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="046bc-486">Problema de seguimiento n.° 9171</span><span class="sxs-lookup"><span data-stu-id="046bc-486">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="046bc-487">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-487">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-488">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-488">**Old behavior**</span></span>

<span data-ttu-id="046bc-489">Antes de EF Core 3.0, el código para llamar a `HasOne` o `HasMany` con una cadena se interpretaba de manera confusa.</span><span class="sxs-lookup"><span data-stu-id="046bc-489">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="046bc-490">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-490">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="046bc-491">El código parece relacionar `Samurai` con otro tipo de entidad mediante la propiedad de navegación `Entrance`, que puede ser privada.</span><span class="sxs-lookup"><span data-stu-id="046bc-491">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="046bc-492">En realidad, este código intenta crear una relación con algún tipo de entidad denominada `Entrance` sin ninguna propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="046bc-492">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="046bc-493">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-493">**New behavior**</span></span>

<span data-ttu-id="046bc-494">A partir de EF Core 3.0, el código anterior ahora hace lo que parecía que debía hacer antes.</span><span class="sxs-lookup"><span data-stu-id="046bc-494">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="046bc-495">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-495">**Why**</span></span>

<span data-ttu-id="046bc-496">El comportamiento anterior era muy confuso, especialmente al leer el código de configuración y al buscar errores.</span><span class="sxs-lookup"><span data-stu-id="046bc-496">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="046bc-497">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-497">**Mitigations**</span></span>

<span data-ttu-id="046bc-498">Esto solo interrumpirá las aplicaciones que configuran de manera explícita las relaciones con cadenas para nombres de tipos y sin especificar explícitamente la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="046bc-498">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="046bc-499">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="046bc-499">This is not common.</span></span>
<span data-ttu-id="046bc-500">El comportamiento anterior se puede obtener al pasar de manera explícita `null` para el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="046bc-500">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="046bc-501">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-501">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="046bc-502">El tipo de valor devuelto para varios métodos asincrónicos se ha cambiado de Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="046bc-502">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="046bc-503">Problema de seguimiento n.º 15184</span><span class="sxs-lookup"><span data-stu-id="046bc-503">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="046bc-504">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-504">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-505">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-505">**Old behavior**</span></span>

<span data-ttu-id="046bc-506">Antes, los siguientes métodos asincrónicos devolvían `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="046bc-506">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="046bc-507">`ValueGenerator.NextValueAsync()` (y las clases derivadas)</span><span class="sxs-lookup"><span data-stu-id="046bc-507">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="046bc-508">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-508">**New behavior**</span></span>

<span data-ttu-id="046bc-509">Dichos métodos ahora devuelven `ValueTask<T>` durante el mismo `T` que antes.</span><span class="sxs-lookup"><span data-stu-id="046bc-509">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="046bc-510">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-510">**Why**</span></span>

<span data-ttu-id="046bc-511">Este cambio reduce el número de asignaciones de montones que se producen al invocar estos métodos, lo que mejora el rendimiento general.</span><span class="sxs-lookup"><span data-stu-id="046bc-511">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="046bc-512">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-512">**Mitigations**</span></span>

<span data-ttu-id="046bc-513">Las aplicaciones que simplemente esperen las API anteriores solo necesitan recompilarse, sin que sea necesario realizar cambios en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="046bc-513">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="046bc-514">Un uso más complejo (p. ej., pasar el valor `Task` devuelto a `Task.WhenAny()`) normalmente requiere que el valor `ValueTask<T>` devuelto se convierta en `Task<T>` mediante una llamada a `AsTask()` en él.</span><span class="sxs-lookup"><span data-stu-id="046bc-514">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="046bc-515">Tenga en cuenta que esto niega la reducción de asignación que implica este cambio.</span><span class="sxs-lookup"><span data-stu-id="046bc-515">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="046bc-516">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="046bc-516">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="046bc-517">Problema de seguimiento n.º 9913</span><span class="sxs-lookup"><span data-stu-id="046bc-517">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="046bc-518">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="046bc-518">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="046bc-519">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-519">**Old behavior**</span></span>

<span data-ttu-id="046bc-520">El nombre de anotación para las anotaciones de asignación de tipos era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="046bc-520">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="046bc-521">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-521">**New behavior**</span></span>

<span data-ttu-id="046bc-522">Ahora, el nombre de anotación para las anotaciones de asignación de tipos es "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="046bc-522">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="046bc-523">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-523">**Why**</span></span>

<span data-ttu-id="046bc-524">Ahora, las asignaciones de tipos se usan para algo más que solo para proveedores de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="046bc-524">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="046bc-525">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-525">**Mitigations**</span></span>

<span data-ttu-id="046bc-526">Esto solo interrumpirá a las aplicaciones que acceden directamente a la asignación de tipos como una anotación, lo que no es habitual.</span><span class="sxs-lookup"><span data-stu-id="046bc-526">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="046bc-527">La acción más apropiada para corregir es usar la superficie de API para acceder a las asignaciones de tipos en lugar de usar directamente la anotación.</span><span class="sxs-lookup"><span data-stu-id="046bc-527">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="046bc-528">ToTable en un tipo derivado inicia una excepción</span><span class="sxs-lookup"><span data-stu-id="046bc-528">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="046bc-529">Problema de seguimiento n.º 11811</span><span class="sxs-lookup"><span data-stu-id="046bc-529">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="046bc-530">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-530">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-531">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-531">**Old behavior**</span></span>

<span data-ttu-id="046bc-532">Antes de EF Core 3.0, la llamada a `ToTable()` en un tipo derivado se omitía, ya que la única estrategia asignación de herencia era TPH, lo que no es válido.</span><span class="sxs-lookup"><span data-stu-id="046bc-532">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="046bc-533">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-533">**New behavior**</span></span>

<span data-ttu-id="046bc-534">A partir de EF Core 3.0, y en preparación para agregar compatibilidad con TPT y TPC en una versión posterior, ahora la llamada a `ToTable()` en un tipo derivado iniciará una excepción para evitar un cambio de asignación inesperado en el futuro.</span><span class="sxs-lookup"><span data-stu-id="046bc-534">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="046bc-535">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-535">**Why**</span></span>

<span data-ttu-id="046bc-536">En la actualidad no se considera válido asignar un tipo derivado a otra tabla.</span><span class="sxs-lookup"><span data-stu-id="046bc-536">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="046bc-537">Este cambio evita interrupciones en el futuro, cuando se convierta en una operación válida.</span><span class="sxs-lookup"><span data-stu-id="046bc-537">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="046bc-538">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-538">**Mitigations**</span></span>

<span data-ttu-id="046bc-539">Quite todos los intentos de asignar tipos derivados a otras tablas.</span><span class="sxs-lookup"><span data-stu-id="046bc-539">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="046bc-540">ForSqlServerHasIndex se ha reemplazado por HasIndex</span><span class="sxs-lookup"><span data-stu-id="046bc-540">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="046bc-541">Problema de seguimiento n.º 12366</span><span class="sxs-lookup"><span data-stu-id="046bc-541">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="046bc-542">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-542">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-543">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-543">**Old behavior**</span></span>

<span data-ttu-id="046bc-544">Antes de EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` proporcionaba una manera de configurar las columnas que se usaban con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="046bc-544">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="046bc-545">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-545">**New behavior**</span></span>

<span data-ttu-id="046bc-546">A partir de EF Core 3.0, ya se admite el uso de `Include` en un índice en el nivel relacional.</span><span class="sxs-lookup"><span data-stu-id="046bc-546">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="046bc-547">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="046bc-547">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="046bc-548">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-548">**Why**</span></span>

<span data-ttu-id="046bc-549">Este cambio se ha realizado para consolidar la API para índices con `Include` en un mismo lugar para todos los proveedores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="046bc-549">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="046bc-550">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-550">**Mitigations**</span></span>

<span data-ttu-id="046bc-551">Use la API nueva, como se ha mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="046bc-551">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="046bc-552">Cambios en la API de metadatos</span><span class="sxs-lookup"><span data-stu-id="046bc-552">Metadata API changes</span></span>

[<span data-ttu-id="046bc-553">Problema de seguimiento n.º 214</span><span class="sxs-lookup"><span data-stu-id="046bc-553">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="046bc-554">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-554">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-555">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-555">**New behavior**</span></span>

<span data-ttu-id="046bc-556">Las siguientes propiedades se han convertido en métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="046bc-556">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="046bc-557">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-557">**Why**</span></span>

<span data-ttu-id="046bc-558">Este cambio simplifica la implementación de las interfaces mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="046bc-558">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="046bc-559">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-559">**Mitigations**</span></span>

<span data-ttu-id="046bc-560">Use los nuevos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="046bc-560">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="046bc-561">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="046bc-561">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="046bc-562">Problema de seguimiento n.º 12151</span><span class="sxs-lookup"><span data-stu-id="046bc-562">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="046bc-563">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="046bc-563">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="046bc-564">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-564">**Old behavior**</span></span>

<span data-ttu-id="046bc-565">Antes de EF Core 3.0, EF Core enviaba `PRAGMA foreign_keys = 1` cuando se abría una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="046bc-565">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="046bc-566">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-566">**New behavior**</span></span>

<span data-ttu-id="046bc-567">A partir de EF Core 3.0, EF Core ya no envía `PRAGMA foreign_keys = 1` cuando se abre una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="046bc-567">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="046bc-568">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-568">**Why**</span></span>

<span data-ttu-id="046bc-569">Este cambio se ha realizado porque en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3` de forma predeterminada, lo que a su vez significa que el cumplimiento de CD está activado de forma predeterminada y no es necesario habilitarlo explícitamente cada vez que se abra una conexión.</span><span class="sxs-lookup"><span data-stu-id="046bc-569">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="046bc-570">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-570">**Mitigations**</span></span>

<span data-ttu-id="046bc-571">Las claves externas se habilitan de forma predeterminada en SQLitePCLRaw.bundle_e_sqlite3, que en EF Core se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="046bc-571">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="046bc-572">Para otros casos, las claves externas se pueden habilitar mediante la especificación de `Foreign Keys=True` en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="046bc-572">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="046bc-573">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="046bc-573">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="046bc-574">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-574">**Old behavior**</span></span>

<span data-ttu-id="046bc-575">Antes de EF Core 3.0, en EF Core se usaba `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="046bc-575">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="046bc-576">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-576">**New behavior**</span></span>

<span data-ttu-id="046bc-577">A partir de EF Core 3.0, en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="046bc-577">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="046bc-578">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-578">**Why**</span></span>

<span data-ttu-id="046bc-579">Este cambio se ha realizado para que la versión de SQLite que se usa en iOS sea coherente con otras plataformas.</span><span class="sxs-lookup"><span data-stu-id="046bc-579">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="046bc-580">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-580">**Mitigations**</span></span>

<span data-ttu-id="046bc-581">Para usar la versión nativa de SQLite en iOS, configure `Microsoft.Data.Sqlite` para usar otra agrupación `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="046bc-581">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="046bc-582">Almacenamiento de valores GUID como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="046bc-582">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="046bc-583">Problema de seguimiento n.º 15078</span><span class="sxs-lookup"><span data-stu-id="046bc-583">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="046bc-584">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-584">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-585">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-585">**Old behavior**</span></span>

<span data-ttu-id="046bc-586">Antes los valores GUID se almacenaban como valores BLOB en SQLite.</span><span class="sxs-lookup"><span data-stu-id="046bc-586">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="046bc-587">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-587">**New behavior**</span></span>

<span data-ttu-id="046bc-588">Ahora los valores GUID se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="046bc-588">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="046bc-589">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-589">**Why**</span></span>

<span data-ttu-id="046bc-590">El formato binario de los GUID no está normalizado.</span><span class="sxs-lookup"><span data-stu-id="046bc-590">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="046bc-591">El almacenamiento de los valores como TEXT mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="046bc-591">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="046bc-592">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-592">**Mitigations**</span></span>

<span data-ttu-id="046bc-593">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="046bc-593">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="046bc-594">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="046bc-594">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="046bc-595">Microsoft.Data.Sqlite sigue siendo capaz de leer valores GUID de ambas columnas BLOB y TEXT. Sin embargo, dado que el formato predeterminado de los parámetros y las constantes ha cambiado, seguramente deberá realizar alguna acción en la mayoría de casos que impliquen el uso de valores GUID.</span><span class="sxs-lookup"><span data-stu-id="046bc-595">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="046bc-596">Ahora los valores char se almacenan como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="046bc-596">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="046bc-597">Problema de seguimiento n.º 15020</span><span class="sxs-lookup"><span data-stu-id="046bc-597">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="046bc-598">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-598">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-599">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-599">**Old behavior**</span></span>

<span data-ttu-id="046bc-600">Anteriormente los valores char se almacenaban como valores INTEGER en SQLite.</span><span class="sxs-lookup"><span data-stu-id="046bc-600">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="046bc-601">Por ejemplo, un valor char de *A* se almacenaba como el valor entero 65.</span><span class="sxs-lookup"><span data-stu-id="046bc-601">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="046bc-602">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-602">**New behavior**</span></span>

<span data-ttu-id="046bc-603">Ahora, los valores char se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="046bc-603">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="046bc-604">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-604">**Why**</span></span>

<span data-ttu-id="046bc-605">El almacenamiento de valores como TEXT es más natural y mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="046bc-605">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="046bc-606">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-606">**Mitigations**</span></span>

<span data-ttu-id="046bc-607">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="046bc-607">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="046bc-608">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="046bc-608">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="046bc-609">Microsoft.Data.Sqlite también puede leer valores de caracteres tanto de columnas INTEGER como de columnas TEXT, por lo que es posible que no deba hacer nada dependiendo de su caso.</span><span class="sxs-lookup"><span data-stu-id="046bc-609">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="046bc-610">Ahora los id. de migración se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="046bc-610">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="046bc-611">Problema de seguimiento n.º 12978</span><span class="sxs-lookup"><span data-stu-id="046bc-611">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="046bc-612">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-612">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-613">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-613">**Old behavior**</span></span>

<span data-ttu-id="046bc-614">Los identificadores de migración se generaban de forma involuntaria con el calendario de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="046bc-614">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="046bc-615">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-615">**New behavior**</span></span>

<span data-ttu-id="046bc-616">Ahora los id. de migración siempre se generan usando el calendario de la referencia cultural invariable (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="046bc-616">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="046bc-617">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-617">**Why**</span></span>

<span data-ttu-id="046bc-618">El orden de las migraciones es importante al actualizar la base de datos o al solucionar conflictos de combinación.</span><span class="sxs-lookup"><span data-stu-id="046bc-618">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="046bc-619">Al usar el calendario invariable, se evitan problemas de ordenación que pueden producirse si los miembros del equipo tienen distintos calendarios del sistema.</span><span class="sxs-lookup"><span data-stu-id="046bc-619">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="046bc-620">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-620">**Mitigations**</span></span>

<span data-ttu-id="046bc-621">Esta cambio afecta a todas las personas que usan un calendario no gregoriano en el que el año sea superior al del calendario gregoriano (como el calendario budista tailandés).</span><span class="sxs-lookup"><span data-stu-id="046bc-621">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="046bc-622">Los id. de migración existentes deberán actualizarse para que las migraciones nuevas se ordenen después de las existentes.</span><span class="sxs-lookup"><span data-stu-id="046bc-622">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="046bc-623">Puede ver el id. de migración en el atributo Migration de los archivos de diseñador de la migración.</span><span class="sxs-lookup"><span data-stu-id="046bc-623">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="046bc-624">También debe actualizarse la tabla de historial de migraciones.</span><span class="sxs-lookup"><span data-stu-id="046bc-624">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="046bc-625">Cambio de nombre de LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="046bc-625">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="046bc-626">Problema de seguimiento n.º 10985</span><span class="sxs-lookup"><span data-stu-id="046bc-626">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="046bc-627">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-627">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-628">**Cambio**</span><span class="sxs-lookup"><span data-stu-id="046bc-628">**Change**</span></span>

<span data-ttu-id="046bc-629">Se ha cambiado el nombre de `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="046bc-629">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="046bc-630">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-630">**Why**</span></span>

<span data-ttu-id="046bc-631">Conviene alinear el nombre de este evento de advertencia con el del resto de eventos de advertencia.</span><span class="sxs-lookup"><span data-stu-id="046bc-631">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="046bc-632">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-632">**Mitigations**</span></span>

<span data-ttu-id="046bc-633">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="046bc-633">Use the new name.</span></span> <span data-ttu-id="046bc-634">(Tenga en cuenta que el número de id. evento sigue siendo el mismo).</span><span class="sxs-lookup"><span data-stu-id="046bc-634">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="046bc-635">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="046bc-635">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="046bc-636">Problema de seguimiento n.º 10730</span><span class="sxs-lookup"><span data-stu-id="046bc-636">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="046bc-637">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="046bc-637">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="046bc-638">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="046bc-638">**Old behavior**</span></span>

<span data-ttu-id="046bc-639">Antes de EF Core 3.0, se utilizaba simplemente el término "nombre" para hacer referencia a los nombres de las restricciones de claves externas.</span><span class="sxs-lookup"><span data-stu-id="046bc-639">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="046bc-640">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-640">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="046bc-641">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="046bc-641">**New behavior**</span></span>

<span data-ttu-id="046bc-642">A partir de EF Core 3.0, el término con el que se hace referencia a los nombres de las restricciones de claves externas es "nombre de la restricción".</span><span class="sxs-lookup"><span data-stu-id="046bc-642">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="046bc-643">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="046bc-643">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="046bc-644">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="046bc-644">**Why**</span></span>

<span data-ttu-id="046bc-645">Este cambio permite mejorar la coherencia relativa a la nomenclatura en este aspecto y aclarar que se trata del nombre de una restricción de clave externa, y no del de la columna o propiedad en la que está definida la clave externa.</span><span class="sxs-lookup"><span data-stu-id="046bc-645">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="046bc-646">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="046bc-646">**Mitigations**</span></span>

<span data-ttu-id="046bc-647">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="046bc-647">Use the new name.</span></span>
