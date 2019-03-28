---
title: 'Cambios importantes en EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7ed55d4cae36f6b25059a5b218db4b0d5e2fb266
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419749"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="f0890-102">Cambios importantes incluidos en EF Core 3.0 (actualmente en versión preliminar)</span><span class="sxs-lookup"><span data-stu-id="f0890-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f0890-103">Tenga en cuenta que los conjuntos de características y las programaciones de las versiones futuras siempre están sujetos a cambios y, aunque intentamos mantener esta página actualizada, es posible que no refleje nuestros planes más recientes en todo momento.</span><span class="sxs-lookup"><span data-stu-id="f0890-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="f0890-104">Es posible que los siguientes cambios de API y comportamiento interrumpan las aplicaciones desarrolladas para EF Core 2.2. x cuando se actualicen a la versión 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="f0890-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="f0890-105">Los cambios que esperamos que solo afecten a proveedores de base de datos se documentan en [Cambios para proveedores](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="f0890-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="f0890-106">Aquí no se documentan los cambios en las características nuevas presentadas de una versión preliminar 3.0 a otra.</span><span class="sxs-lookup"><span data-stu-id="f0890-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="f0890-107">Las consultas LINQ ya no se evalúan en el cliente</span><span class="sxs-lookup"><span data-stu-id="f0890-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="f0890-108">[Problema de seguimiento n.° 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Consulte también el problema n.° 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="f0890-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="f0890-109">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-110">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-110">**Old behavior**</span></span>

<span data-ttu-id="f0890-111">Antes de 3.0, cuando en EF Core no se podía convertir una expresión que formaba parte de una consulta SQL o un parámetro, la expresión se evaluaba de forma automática en el cliente.</span><span class="sxs-lookup"><span data-stu-id="f0890-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="f0890-112">De forma predeterminada, la evaluación de cliente de expresiones potencialmente costosas solo desencadenaba una advertencia.</span><span class="sxs-lookup"><span data-stu-id="f0890-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="f0890-113">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-113">**New behavior**</span></span>

<span data-ttu-id="f0890-114">A partir de 3.0, en EF Core solo se permite que se evalúen en el cliente las expresiones en la proyección de nivel superior (la última llamada a `Select()` de la consulta).</span><span class="sxs-lookup"><span data-stu-id="f0890-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="f0890-115">Cuando las expresiones de otra parte de la consulta no se pueden convertir en SQL o un parámetro, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="f0890-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="f0890-116">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-116">**Why**</span></span>

<span data-ttu-id="f0890-117">La evaluación de cliente automática de las consultas permite que se ejecuten muchas consultas incluso si no se pueden convertir elementos importantes de ellas.</span><span class="sxs-lookup"><span data-stu-id="f0890-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="f0890-118">Esto puede provocar un comportamiento inesperado y potencialmente dañino que es posible que solo sea evidente en entornos de producción.</span><span class="sxs-lookup"><span data-stu-id="f0890-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="f0890-119">Por ejemplo, una condición en una llamada a `Where()` que no se puede convertir puede provocar que todas las filas de la tabla se transfieran desde el servidor de base de datos y que el filtro se aplique en el cliente.</span><span class="sxs-lookup"><span data-stu-id="f0890-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="f0890-120">Esta situación puede pasar desapercibida fácilmente si la tabla solo contiene algunas filas en la fase de desarrollo, pero ser más grave cuando la aplicación pase a producción, donde la tabla puede contener millones de filas.</span><span class="sxs-lookup"><span data-stu-id="f0890-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="f0890-121">Las advertencias de evaluación de cliente también se suelen pasar por alto durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f0890-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="f0890-122">Además de esto, la evaluación de cliente automática puede causar problemas en los que la mejora de la traducción de consultas para expresiones específicas provocaba cambios importantes no deseados entre versiones.</span><span class="sxs-lookup"><span data-stu-id="f0890-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="f0890-123">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-123">**Mitigations**</span></span>

<span data-ttu-id="f0890-124">Si una consulta no se puede traducir totalmente, vuelva a escribirla en un formato que se pueda traducir, o bien use `AsEnumerable()`, `ToList()` o una función similar para devolver los datos al cliente de forma explícita, donde después se puedan seguir procesando mediante LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="f0890-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="f0890-125">Entity Framework Core ya no forma parte del marco compartido ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0890-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="f0890-126">Anuncios del problema de seguimiento n.º 325</span><span class="sxs-lookup"><span data-stu-id="f0890-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="f0890-127">Este cambio se ha introducido en ASP.NET Core 3.0, versión preliminar 1.</span><span class="sxs-lookup"><span data-stu-id="f0890-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="f0890-128">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-128">**Old behavior**</span></span>

<span data-ttu-id="f0890-129">Antes de ASP.NET Core 3.0, cuando se agregaba una referencia de paquete a `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, se incluía EF Core y algunos de los proveedores de datos de EF Core, como el de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f0890-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="f0890-130">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-130">**New behavior**</span></span>

<span data-ttu-id="f0890-131">A partir de la versión 3.0, el marco compartido ASP.NET Core no incluye EF Core ni ningún proveedor de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="f0890-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="f0890-132">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-132">**Why**</span></span>

<span data-ttu-id="f0890-133">Antes de este cambio, para obtener EF Core se necesitaban varios pasos en función de si la aplicación se destinaba a ASP.NET Core y SQL Server o no.</span><span class="sxs-lookup"><span data-stu-id="f0890-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="f0890-134">Además, la actualización de ASP.NET Core forzaba la de EF Core y el proveedor de SQL Server, lo que no siempre es deseable.</span><span class="sxs-lookup"><span data-stu-id="f0890-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="f0890-135">Con este cambio, la experiencia de obtención de EF Core es la misma en todos los proveedores, implementaciones admitidas de .NET y tipos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0890-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="f0890-136">Ahora los desarrolladores también pueden controlar exactamente cuándo se actualizan EF Core y los proveedores de datos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="f0890-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="f0890-137">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-137">**Mitigations**</span></span>

<span data-ttu-id="f0890-138">Para usar EF Core en una aplicación ASP.NET Core 3.0 o cualquier otra aplicación compatible, debe agregar de forma explícita una referencia de paquete al proveedor de base de datos de EF Core que se va a usar en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0890-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="f0890-139">La ejecución de consultas se registra en el nivel de depuración</span><span class="sxs-lookup"><span data-stu-id="f0890-139">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="f0890-140">Problema de seguimiento n.º 14523</span><span class="sxs-lookup"><span data-stu-id="f0890-140">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="f0890-141">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-141">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-142">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-142">**Old behavior**</span></span>

<span data-ttu-id="f0890-143">Antes de EF Core 3.0, la ejecución de consultas y otros comandos se registraba en el nivel `Info`.</span><span class="sxs-lookup"><span data-stu-id="f0890-143">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="f0890-144">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-144">**New behavior**</span></span>

<span data-ttu-id="f0890-145">A partir de EF Core 3.0, el registro de ejecución de comandos y SQL se produce en el nivel `Debug`.</span><span class="sxs-lookup"><span data-stu-id="f0890-145">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="f0890-146">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-146">**Why**</span></span>

<span data-ttu-id="f0890-147">Este cambio se ha realizado para reducir el ruido en el nivel de registro `Info`.</span><span class="sxs-lookup"><span data-stu-id="f0890-147">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="f0890-148">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-148">**Mitigations**</span></span>

<span data-ttu-id="f0890-149">Este evento de registro se define mediante `RelationalEventId.CommandExecuting` con el id. de evento 20100.</span><span class="sxs-lookup"><span data-stu-id="f0890-149">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="f0890-150">Para volver a registrar SQL en el nivel `Info`, configure explícitamente el nivel en `OnConfiguring` o en `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="f0890-150">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="f0890-151">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0890-151">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="f0890-152">Los valores de clave temporal ya no se establecen en instancias de entidad</span><span class="sxs-lookup"><span data-stu-id="f0890-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="f0890-153">Problema de seguimiento n.º 12378</span><span class="sxs-lookup"><span data-stu-id="f0890-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="f0890-154">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="f0890-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="f0890-155">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-155">**Old behavior**</span></span>

<span data-ttu-id="f0890-156">Antes de EF Core 3.0, los valores temporales se asignaban a todas las propiedades de clave para las que posteriormente la base de datos generaba un valor real.</span><span class="sxs-lookup"><span data-stu-id="f0890-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="f0890-157">Normalmente, estos valores temporales eran números negativos grandes.</span><span class="sxs-lookup"><span data-stu-id="f0890-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="f0890-158">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-158">**New behavior**</span></span>

<span data-ttu-id="f0890-159">A partir de la versión 3.0, en EF Core se almacena el valor de clave temporal como parte de la información de seguimiento de la entidad y la propiedad clave en sí no se modifica.</span><span class="sxs-lookup"><span data-stu-id="f0890-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="f0890-160">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-160">**Why**</span></span>

<span data-ttu-id="f0890-161">Este cambio se ha realizado para evitar que los valores de clave temporales se conviertan erróneamente en permanentes cuando una entidad de la que previamente una instancia de `DbContext` ha realizado el seguimiento se mueve a otra instancia de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f0890-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="f0890-162">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-162">**Mitigations**</span></span>

<span data-ttu-id="f0890-163">Las aplicaciones que asignan valores de clave principal a claves externas para crear asociaciones entre entidades pueden depender del comportamiento anterior si las claves principales son generadas por el almacén y pertenecen a entidades en el estado `Added`.</span><span class="sxs-lookup"><span data-stu-id="f0890-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="f0890-164">Esto se puede evitar con las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="f0890-164">This can be avoided by:</span></span>
* <span data-ttu-id="f0890-165">No se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="f0890-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="f0890-166">Se establecen propiedades de navegación para crear relaciones en lugar de establecer valores de clave externa.</span><span class="sxs-lookup"><span data-stu-id="f0890-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="f0890-167">Se obtienen los valores de clave temporal reales de la información de seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="f0890-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="f0890-168">Por ejemplo, `context.Entry(blog).Property(e => e.Id).CurrentValue` devolverá el valor temporal aunque no se haya establecido `blog.Id`.</span><span class="sxs-lookup"><span data-stu-id="f0890-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="f0890-169">DetectChanges respeta los valores de clave generados por el almacén</span><span class="sxs-lookup"><span data-stu-id="f0890-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="f0890-170">Problema de seguimiento n.º 14616</span><span class="sxs-lookup"><span data-stu-id="f0890-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="f0890-171">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-172">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-172">**Old behavior**</span></span>

<span data-ttu-id="f0890-173">Antes de EF Core 3.0, se realizaba el seguimiento en el estado `Added` de las entidades sin seguimiento detectadas por `DetectChanges` y se insertaban como una fila nueva cuando se llamaba a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="f0890-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="f0890-174">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-174">**New behavior**</span></span>

<span data-ttu-id="f0890-175">A partir de EF Core 3.0, si una entidad usa valores de clave generados y se establece un valor de clave, se realizará el seguimiento de la entidad en el estado `Modified`.</span><span class="sxs-lookup"><span data-stu-id="f0890-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="f0890-176">Esto significa que se supone que existe una fila para la entidad y que se actualizará cuando se llame a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="f0890-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="f0890-177">Si no se establece el valor de clave, o bien si el tipo de entidad no usa claves generadas, se seguirá realizando el seguimiento de la entidad nueva como `Added` al igual que en las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="f0890-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="f0890-178">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-178">**Why**</span></span>

<span data-ttu-id="f0890-179">Este cambio se ha realizado para que sea más sencillo y coherente trabajar con gráficos de entidades desconectadas mientras se usan claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="f0890-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="f0890-180">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-180">**Mitigations**</span></span>

<span data-ttu-id="f0890-181">Este cambio puede interrumpir una aplicación si se configura un tipo de entidad para usar claves generadas, pero se establecen de forma explícita valores de clave para las instancias nuevas.</span><span class="sxs-lookup"><span data-stu-id="f0890-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="f0890-182">La solución consiste en configurar de forma explícita las propiedades de clave para que no usen valores generados.</span><span class="sxs-lookup"><span data-stu-id="f0890-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="f0890-183">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="f0890-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="f0890-184">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="f0890-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="f0890-185">Las eliminaciones en cascada ahora se realizan inmediatamente de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="f0890-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="f0890-186">Problema de seguimiento n.º 10114</span><span class="sxs-lookup"><span data-stu-id="f0890-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="f0890-187">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-188">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-188">**Old behavior**</span></span>

<span data-ttu-id="f0890-189">Antes de la versión 3.0, en EF Core no se aplicaban acciones en cascada (eliminación de entidades dependientes cuando se eliminaba una entidad de seguridad obligatoria o cuando se rompía la relación con una entidad de seguridad obligatoria) hasta que se llamaba a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="f0890-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="f0890-190">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-190">**New behavior**</span></span>

<span data-ttu-id="f0890-191">A partir de 3.0, en EF Core las acciones en cascada se aplican en cuanto se detecta la condición desencadenadora.</span><span class="sxs-lookup"><span data-stu-id="f0890-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="f0890-192">Por ejemplo, como resultado de la llamada a `context.Remove()` para eliminar una entidad de seguridad, todos los dependientes obligatorios relacionados de los que se realiza el seguimiento también se establecen en `Deleted` inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="f0890-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="f0890-193">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-193">**Why**</span></span>

<span data-ttu-id="f0890-194">Este cambio se ha realizado para mejorar la experiencia en escenarios de auditoría y enlace de datos donde es importante comprender qué entidades se van a eliminar _antes_ de llamar a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="f0890-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="f0890-195">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-195">**Mitigations**</span></span>

<span data-ttu-id="f0890-196">El comportamiento anterior se puede restaurar mediante opciones de `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="f0890-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="f0890-197">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0890-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="f0890-198">Los tipos de consulta se consolidan con tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="f0890-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="f0890-199">Problema de seguimiento n.º 14194</span><span class="sxs-lookup"><span data-stu-id="f0890-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="f0890-200">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-201">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-201">**Old behavior**</span></span>

<span data-ttu-id="f0890-202">Antes de EF Core 3.0, los [tipos de consulta](xref:core/modeling/query-types) eran un medio para consultar los datos que no definen una clave principal de una manera estructurada.</span><span class="sxs-lookup"><span data-stu-id="f0890-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="f0890-203">Es decir, un tipo de consulta se usaba para asignar tipos de entidad sin claves (más probablemente desde una vista, pero posiblemente desde una tabla), mientras que un tipo de entidad estándar se usaba cuando había una clave disponible (más probablemente desde una tabla, pero posiblemente desde una vista).</span><span class="sxs-lookup"><span data-stu-id="f0890-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="f0890-204">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-204">**New behavior**</span></span>

<span data-ttu-id="f0890-205">Ahora un tipo de consulta se convierte en un tipo de entidad sin una clave principal.</span><span class="sxs-lookup"><span data-stu-id="f0890-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="f0890-206">Los tipos de entidad sin clave tienen la misma funcionalidad que los tipos de consulta de las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="f0890-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="f0890-207">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-207">**Why**</span></span>

<span data-ttu-id="f0890-208">Este cambio se ha realizado para reducir la confusión en torno a la finalidad de los tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="f0890-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="f0890-209">En concreto, son tipos de entidad sin clave y, por ello, intrínsecamente son de solo lectura, pero no se deben usar solo porque un tipo de entidad tenga que ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="f0890-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="f0890-210">Del mismo modo, se suelen asignar a vistas, pero solo porque las vistas no suelen definir claves.</span><span class="sxs-lookup"><span data-stu-id="f0890-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="f0890-211">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-211">**Mitigations**</span></span>

<span data-ttu-id="f0890-212">Los elementos siguientes de la API ahora están obsoletos:</span><span class="sxs-lookup"><span data-stu-id="f0890-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="f0890-213">**`ModelBuilder.Query<>()`**: en su lugar es necesario llamar a `ModelBuilder.Entity<>().HasNoKey()` para marcar un tipo de entidad como sin claves.</span><span class="sxs-lookup"><span data-stu-id="f0890-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="f0890-214">Esto todavía no se configurará por convención para evitar una configuración incorrecta cuando se espera una clave principal, pero no coincide con la convención.</span><span class="sxs-lookup"><span data-stu-id="f0890-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="f0890-215">**`DbQuery<>`**: en su lugar se debe usar `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="f0890-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="f0890-216">**`DbContext.Query<>()`**: en su lugar se debe usar `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="f0890-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="f0890-217">La API de configuración para las relaciones de tipo de propiedad ha cambiado</span><span class="sxs-lookup"><span data-stu-id="f0890-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="f0890-218">[Problema de seguimiento n.º 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema de seguimiento n.º 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema de seguimiento n.º 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="f0890-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="f0890-219">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-220">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-220">**Old behavior**</span></span>

<span data-ttu-id="f0890-221">Antes de EF Core 3.0, la configuración de la relación de propiedad se realizaba directamente después de la llamada a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="f0890-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="f0890-222">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-222">**New behavior**</span></span>

<span data-ttu-id="f0890-223">A partir de EF Core 3.0, ahora hay una API fluida para configurar una propiedad de navegación para el propietario mediante `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="f0890-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="f0890-224">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0890-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="f0890-225">La configuración relacionada con la relación entre el propietario y lo que se posee ahora se debe encadenar después de `WithOwner()`, de forma similar a cómo se configuran otras relaciones.</span><span class="sxs-lookup"><span data-stu-id="f0890-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="f0890-226">Pero la configuración del propio tipo de propiedad se seguirá encadenando después de `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="f0890-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="f0890-227">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0890-227">For example:</span></span>

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

<span data-ttu-id="f0890-228">Además, la llamada a `Entity()`, `HasOne()` o `Set()` con un tipo de propiedad de destino ahora iniciará una excepción.</span><span class="sxs-lookup"><span data-stu-id="f0890-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="f0890-229">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-229">**Why**</span></span>

<span data-ttu-id="f0890-230">Este cambio se ha realizado para crear una separación más clara entre la configuración del propio tipo de propiedad y la _relación con_ el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="f0890-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="f0890-231">A su vez, esto elimina la ambigüedad y la confusión de métodos como `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="f0890-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="f0890-232">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-232">**Mitigations**</span></span>

<span data-ttu-id="f0890-233">Cambie la configuración de las relaciones de tipo de propiedad para usar la nueva superficie de API, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="f0890-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="f0890-234">La convención de propiedad de clave externa ya no coincide con el mismo nombre que la propiedad de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="f0890-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="f0890-235">Problema de seguimiento n.º 13274</span><span class="sxs-lookup"><span data-stu-id="f0890-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="f0890-236">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-237">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-237">**Old behavior**</span></span>

<span data-ttu-id="f0890-238">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f0890-238">Consider the following model:</span></span>
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
<span data-ttu-id="f0890-239">Antes de EF Core 3.0, se podía usar la propiedad `CustomerId` para la clave externa por convención.</span><span class="sxs-lookup"><span data-stu-id="f0890-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="f0890-240">Pero si `Order` es un tipo de propiedad, entonces esto convertiría también a `CustomerId` en la clave principal, algo que no suele ser lo esperado.</span><span class="sxs-lookup"><span data-stu-id="f0890-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="f0890-241">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-241">**New behavior**</span></span>

<span data-ttu-id="f0890-242">A partir de la versión 3.0, EF Core no intentará usar las propiedades de claves externas por convención si tienen el mismo nombre que la propiedad de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="f0890-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="f0890-243">Los patrones de nombre de tipo de entidad de seguridad concatenado con el nombre de propiedad de la entidad de seguridad y de nombre de navegación concatenado con el nombre de propiedad de la entidad de seguridad todavía se hacen coincidir.</span><span class="sxs-lookup"><span data-stu-id="f0890-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="f0890-244">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0890-244">For example:</span></span>

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

<span data-ttu-id="f0890-245">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-245">**Why**</span></span>

<span data-ttu-id="f0890-246">Este cambio se ha realizado para evitar definir erróneamente una propiedad de clave principal en el tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="f0890-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="f0890-247">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-247">**Mitigations**</span></span>

<span data-ttu-id="f0890-248">Si la propiedad se ha diseñado para ser la clave externa y, por tanto, parte de la clave principal, se debe configurar explícitamente como tal.</span><span class="sxs-lookup"><span data-stu-id="f0890-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="f0890-249">Cada propiedad usa la generación de claves enteras en memoria independiente</span><span class="sxs-lookup"><span data-stu-id="f0890-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="f0890-250">Problema de seguimiento n.º 6872</span><span class="sxs-lookup"><span data-stu-id="f0890-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="f0890-251">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-252">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-252">**Old behavior**</span></span>

<span data-ttu-id="f0890-253">Antes de EF Core 3.0, se usaba un generador de valores compartidos para todas las propiedades de clave entera en memoria.</span><span class="sxs-lookup"><span data-stu-id="f0890-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="f0890-254">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-254">**New behavior**</span></span>

<span data-ttu-id="f0890-255">A partir de EF Core 3.0, cada propiedad de clave entera obtiene su propio generador de valores cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="f0890-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="f0890-256">Además, si se elimina la base de datos, la generación de claves se restablece para todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="f0890-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="f0890-257">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-257">**Why**</span></span>

<span data-ttu-id="f0890-258">Este cambio se ha realizado para alinear la generación de claves en memoria más estrechamente a la generación de claves de base de datos reales y para mejorar la capacidad para aislar las pruebas entre sí cuando se usa la base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="f0890-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="f0890-259">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-259">**Mitigations**</span></span>

<span data-ttu-id="f0890-260">Esto puede interrumpir una aplicación que se base en el establecimiento de valores de clave específicos en memoria.</span><span class="sxs-lookup"><span data-stu-id="f0890-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="f0890-261">En su lugar, considere la posibilidad de no depender de valores de clave específicos, o bien de actualizar para que coincida con el comportamiento nuevo.</span><span class="sxs-lookup"><span data-stu-id="f0890-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="f0890-262">Los campos de respaldo se usan de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="f0890-262">Backing fields are used by default</span></span>

[<span data-ttu-id="f0890-263">Problema de seguimiento n.º 12430</span><span class="sxs-lookup"><span data-stu-id="f0890-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="f0890-264">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="f0890-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="f0890-265">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-265">**Old behavior**</span></span>

<span data-ttu-id="f0890-266">Antes de la versión 3.0, incluso si se conocía el campo de respaldo de una propiedad, de forma predeterminada en EF Core se leía y escribía el valor de propiedad mediante los métodos captadores y establecedores de propiedades.</span><span class="sxs-lookup"><span data-stu-id="f0890-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="f0890-267">La excepción era la ejecución de consultas, donde el campo de respaldo se establecía directamente si se conocía.</span><span class="sxs-lookup"><span data-stu-id="f0890-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="f0890-268">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-268">**New behavior**</span></span>

<span data-ttu-id="f0890-269">A partir de EF Core 3.0, si se conoce el campo de respaldo para una propiedad, esa propiedad siempre se leerá y escribirá mediante el campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="f0890-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="f0890-270">Esto podría provocar una interrupción de la aplicación si depende de un comportamiento adicional codificado en los métodos captadores o establecedores.</span><span class="sxs-lookup"><span data-stu-id="f0890-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="f0890-271">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-271">**Why**</span></span>

<span data-ttu-id="f0890-272">Este cambio se ha realizado para evitar que EF Core desencadene erróneamente lógica de negocios de forma predeterminada al realizar operaciones de base de datos que implican entidades.</span><span class="sxs-lookup"><span data-stu-id="f0890-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="f0890-273">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-273">**Mitigations**</span></span>

<span data-ttu-id="f0890-274">El comportamiento anterior a la versión 3.0 se puede restaurar mediante la configuración del modo de acceso de propiedad de la API fluida modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="f0890-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="f0890-275">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0890-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="f0890-276">Inicio de excepciones si se encuentran varios campos de respaldo compatibles</span><span class="sxs-lookup"><span data-stu-id="f0890-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="f0890-277">Problema de seguimiento n.º 12523</span><span class="sxs-lookup"><span data-stu-id="f0890-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="f0890-278">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-279">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-279">**Old behavior**</span></span>

<span data-ttu-id="f0890-280">Antes de EF Core 3.0, si varios campos coincidían con las reglas para buscar el campo de respaldo de una propiedad, se elegía un campo según un orden de prioridad.</span><span class="sxs-lookup"><span data-stu-id="f0890-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="f0890-281">Esto podía producir que, en caso de ambigüedad, se usara el campo incorrecto.</span><span class="sxs-lookup"><span data-stu-id="f0890-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="f0890-282">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-282">**New behavior**</span></span>

<span data-ttu-id="f0890-283">A partir de EF Core 3.0, si varios campos coinciden con la misma propiedad, se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="f0890-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="f0890-284">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-284">**Why**</span></span>

<span data-ttu-id="f0890-285">Este cambio se ha realizado para evitar de forma silenciosa el uso de un campo con respecto a otro cuando solo uno puede ser correcto.</span><span class="sxs-lookup"><span data-stu-id="f0890-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="f0890-286">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-286">**Mitigations**</span></span>

<span data-ttu-id="f0890-287">En las propiedades con campos de respaldo ambiguos se debe especificar de forma explícita el campo que se va usar.</span><span class="sxs-lookup"><span data-stu-id="f0890-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="f0890-288">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="f0890-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="f0890-289">AddDbContext/AddDbContextPool ya no llaman a AddLogging ni a AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="f0890-289">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="f0890-290">Problema de seguimiento n.° 14756</span><span class="sxs-lookup"><span data-stu-id="f0890-290">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="f0890-291">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-291">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-292">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-292">**Old behavior**</span></span>

<span data-ttu-id="f0890-293">Antes de EF Core 3.0, llamar a `AddDbContext` o a `AddDbContextPool` también podría registrar los servicios de almacenamiento en caché y de registro con D.I. a través de llamadas a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) y a [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="f0890-293">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="f0890-294">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-294">**New behavior**</span></span>

<span data-ttu-id="f0890-295">A partir de EF Core 3.0, `AddDbContext` y `AddDbContextPool` ya no registrarán estos servicios con inserción de dependencias (DI).</span><span class="sxs-lookup"><span data-stu-id="f0890-295">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="f0890-296">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-296">**Why**</span></span>

<span data-ttu-id="f0890-297">EF Core 3.0 no requiere que estos servicios estén en el contenedor de inserción de dependencias de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0890-297">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="f0890-298">Pero si `ILoggerFactory` se registra en el contenedor de DI de la aplicación, EF Core lo empezará a usar de todos modos.</span><span class="sxs-lookup"><span data-stu-id="f0890-298">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="f0890-299">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-299">**Mitigations**</span></span>

<span data-ttu-id="f0890-300">Si la aplicación necesita estos servicios, regístrelos de manera explícita con el contenedor de DI mediante [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="f0890-300">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="f0890-301">Ahora DbContext.Entry realiza una operación DetectChanges local</span><span class="sxs-lookup"><span data-stu-id="f0890-301">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="f0890-302">Problema de seguimiento n.º 13552</span><span class="sxs-lookup"><span data-stu-id="f0890-302">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="f0890-303">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-303">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-304">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-304">**Old behavior**</span></span>

<span data-ttu-id="f0890-305">Antes de EF Core 3.0, la llamada a `DbContext.Entry` provocaba que se detectaran cambios para todas las entidades con seguimiento.</span><span class="sxs-lookup"><span data-stu-id="f0890-305">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="f0890-306">Esto garantizaba que el estado expuesto en `EntityEntry` estuviera actualizado.</span><span class="sxs-lookup"><span data-stu-id="f0890-306">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="f0890-307">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-307">**New behavior**</span></span>

<span data-ttu-id="f0890-308">A partir de EF Core 3.0, ahora la llamada a `DbContext.Entry` solo intenta detectar cambios en la entidad dada y cualquier entidad de seguridad relacionada con ella de la que se haya realizado el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="f0890-308">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="f0890-309">Esto significa que es posible que la llamada a este método no haya detectado otros cambios, lo que podría tener implicaciones en el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0890-309">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="f0890-310">Observe que si `ChangeTracker.AutoDetectChangesEnabled` se establece en `false` incluso esta detección de cambios local se deshabilitará.</span><span class="sxs-lookup"><span data-stu-id="f0890-310">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="f0890-311">Otros métodos que provocan la detección de cambios (como `ChangeTracker.Entries` y `SaveChanges`) siguen provocando una acción `DetectChanges` completa de todas las entidades de las que se realiza el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="f0890-311">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="f0890-312">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-312">**Why**</span></span>

<span data-ttu-id="f0890-313">Este cambio se ha realizado para mejorar el rendimiento predeterminado del uso de `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="f0890-313">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="f0890-314">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-314">**Mitigations**</span></span>

<span data-ttu-id="f0890-315">Llame a `ChgangeTracker.DetectChanges()` de forma explícita antes de llamar a `Entry` para garantizar el comportamiento anterior a la versión 3.0.</span><span class="sxs-lookup"><span data-stu-id="f0890-315">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="f0890-316">El cliente no genera las claves de matriz de cadena y byte de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="f0890-316">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="f0890-317">Problema de seguimiento n.º 14617</span><span class="sxs-lookup"><span data-stu-id="f0890-317">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="f0890-318">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-318">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-319">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-319">**Old behavior**</span></span>

<span data-ttu-id="f0890-320">Antes de EF Core 3.0, se podían usar las propiedades de clave `string` y `byte[]` sin tener que establecer de forma explícita un valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="f0890-320">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="f0890-321">En ese caso, el valor de clave se generaba en el cliente como un GUID, que se serializaba en bytes para `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="f0890-321">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="f0890-322">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-322">**New behavior**</span></span>

<span data-ttu-id="f0890-323">A partir de EF Core 3.0, se iniciará una excepción en la que indica que no se ha establecido ningún valor de clave.</span><span class="sxs-lookup"><span data-stu-id="f0890-323">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="f0890-324">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-324">**Why**</span></span>

<span data-ttu-id="f0890-325">Este cambio se ha realizado porque los valores `string`/`byte[]` generados por el cliente no suelen ser útiles, y el comportamiento predeterminado dificultaba razonar sobre los valores de clave generados de una forma habitual.</span><span class="sxs-lookup"><span data-stu-id="f0890-325">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="f0890-326">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-326">**Mitigations**</span></span>

<span data-ttu-id="f0890-327">Se puede obtener el comportamiento anterior a la versión 3.0 si se especifica de forma explícita que las propiedades de clave deben usar los valores generados si no se establece ningún otro valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="f0890-327">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="f0890-328">Por ejemplo, con la API fluida:</span><span class="sxs-lookup"><span data-stu-id="f0890-328">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="f0890-329">O bien con anotaciones de datos:</span><span class="sxs-lookup"><span data-stu-id="f0890-329">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="f0890-330">Ahora ILoggerFactory es un servicio con ámbito</span><span class="sxs-lookup"><span data-stu-id="f0890-330">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="f0890-331">Problema de seguimiento n.º 14698</span><span class="sxs-lookup"><span data-stu-id="f0890-331">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="f0890-332">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-332">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-333">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-333">**Old behavior**</span></span>

<span data-ttu-id="f0890-334">Antes de EF Core 3.0, `ILoggerFactory` se registraba como un servicio de singleton.</span><span class="sxs-lookup"><span data-stu-id="f0890-334">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="f0890-335">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-335">**New behavior**</span></span>

<span data-ttu-id="f0890-336">A partir de EF Core 3.0, `ILoggerFactory` ahora se registra como con ámbito.</span><span class="sxs-lookup"><span data-stu-id="f0890-336">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="f0890-337">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-337">**Why**</span></span>

<span data-ttu-id="f0890-338">Este cambio se ha realizado para permitir la asociación de un registrador con una instancia de `DbContext`, lo que habilita otras funciones y quita algunos casos de comportamiento patológico como un aumento vertiginoso de los proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="f0890-338">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="f0890-339">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-339">**Mitigations**</span></span>

<span data-ttu-id="f0890-340">Este cambio no debería afectar al código de la aplicación a menos que registre y use servicios personalizados en el proveedor de servicios internos de EF Core.</span><span class="sxs-lookup"><span data-stu-id="f0890-340">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="f0890-341">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="f0890-341">This isn't common.</span></span>
<span data-ttu-id="f0890-342">En estos casos, la mayoría de los elementos seguirá funcionando, pero cualquier servicio de singleton que dependiera de `ILoggerFactory` tendrá que cambiarse para obtener la interfaz `ILoggerFactory` de otra forma.</span><span class="sxs-lookup"><span data-stu-id="f0890-342">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="f0890-343">Si experimenta situaciones como esta, registre un problema en el [rastreador de problemas de GitHub de EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) para hacernos saber cómo usa `ILoggerFactory`, para que podamos comprender mejor cómo evitar esta interrupción en el futuro.</span><span class="sxs-lookup"><span data-stu-id="f0890-343">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="f0890-344">IDbContextOptionsExtensionWithDebugInfo se ha fusionado en IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="f0890-344">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="f0890-345">Problema de seguimiento n.º 13552</span><span class="sxs-lookup"><span data-stu-id="f0890-345">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="f0890-346">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-346">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-347">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-347">**Old behavior**</span></span>

<span data-ttu-id="f0890-348">`IDbContextOptionsExtensionWithDebugInfo` era una interfaz opcional adicional ampliada de `IDbContextOptionsExtension` para evitar realizar cambios importantes en la interfaz durante el ciclo de versiones 2.x.</span><span class="sxs-lookup"><span data-stu-id="f0890-348">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="f0890-349">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-349">**New behavior**</span></span>

<span data-ttu-id="f0890-350">Ahora las interfaces se combinan en `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="f0890-350">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="f0890-351">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-351">**Why**</span></span>

<span data-ttu-id="f0890-352">Este cambio se ha realizado porque las interfaces son conceptualmente una.</span><span class="sxs-lookup"><span data-stu-id="f0890-352">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="f0890-353">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-353">**Mitigations**</span></span>

<span data-ttu-id="f0890-354">Cualquier implementación de `IDbContextOptionsExtension` tendrá que actualizarse para admitir el miembro nuevo.</span><span class="sxs-lookup"><span data-stu-id="f0890-354">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="f0890-355">En los proxies de carga diferida ya no se supone que las propiedades de navegación están totalmente cargadas</span><span class="sxs-lookup"><span data-stu-id="f0890-355">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="f0890-356">Problema de seguimiento n.º 12780</span><span class="sxs-lookup"><span data-stu-id="f0890-356">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="f0890-357">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-358">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-358">**Old behavior**</span></span>

<span data-ttu-id="f0890-359">Antes de EF Core 3.0, una vez que se eliminaba `DbContext` no había ninguna forma de saber si una determinada propiedad de navegación de una entidad obtenida de ese contexto se había cargado completamente o no.</span><span class="sxs-lookup"><span data-stu-id="f0890-359">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="f0890-360">En su lugar, los proxies asumían que una navegación de referencia se cargaba si tenía un valor distinto de NULL, y que una navegación de colección se cargaba si no estaba vacía.</span><span class="sxs-lookup"><span data-stu-id="f0890-360">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="f0890-361">En estos casos, el intento de carga diferida era no operativo.</span><span class="sxs-lookup"><span data-stu-id="f0890-361">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="f0890-362">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-362">**New behavior**</span></span>

<span data-ttu-id="f0890-363">A partir de EF Core 3.0, los proxies realizan el seguimiento de si una propiedad de navegación se carga o no.</span><span class="sxs-lookup"><span data-stu-id="f0890-363">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="f0890-364">Esto significa que el intento de acceder a una propiedad de navegación que se carga después de que se haya eliminado el contexto siempre será no operativo, incluso cuando la navegación cargada está vacía o es NULL.</span><span class="sxs-lookup"><span data-stu-id="f0890-364">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="f0890-365">Por el contrario, el intento de acceder a una propiedad de navegación que no está cargada iniciará una excepción si el contexto se ha eliminado, incluso si la propiedad de navegación es una colección no vacía.</span><span class="sxs-lookup"><span data-stu-id="f0890-365">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="f0890-366">Si se produce esta situación, significa que el código de aplicación está intentando usar la carga diferida en un momento no válido y que se debe cambiar la aplicación para que lo no haga.</span><span class="sxs-lookup"><span data-stu-id="f0890-366">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="f0890-367">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-367">**Why**</span></span>

<span data-ttu-id="f0890-368">Este cambio se ha realizado para que el comportamiento sea coherente y correcto cuando se intenta la carga diferida de una instancia de `DbContext` eliminada.</span><span class="sxs-lookup"><span data-stu-id="f0890-368">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="f0890-369">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-369">**Mitigations**</span></span>

<span data-ttu-id="f0890-370">Actualice el código de la aplicación para que no intente la carga diferida con un contexto eliminado, o bien configúrelo para que sea no operativo, como se describe en el mensaje de la excepción.</span><span class="sxs-lookup"><span data-stu-id="f0890-370">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="f0890-371">La creación excesiva de proveedores de servicios internos ahora es un error de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="f0890-371">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="f0890-372">Problema de seguimiento n.º 10236</span><span class="sxs-lookup"><span data-stu-id="f0890-372">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="f0890-373">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-373">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-374">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-374">**Old behavior**</span></span>

<span data-ttu-id="f0890-375">Antes de EF Core 3.0, se registraba una advertencia para una aplicación que creaba un número patológico de proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="f0890-375">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="f0890-376">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-376">**New behavior**</span></span>

<span data-ttu-id="f0890-377">A partir de EF Core 3.0, ahora esta advertencia se considera un error y se inicia una excepción.</span><span class="sxs-lookup"><span data-stu-id="f0890-377">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="f0890-378">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-378">**Why**</span></span>

<span data-ttu-id="f0890-379">Este cambio se ha realizado para controlar mejor el código de la aplicación mediante la exposición de este caso patológico de una forma más explícita.</span><span class="sxs-lookup"><span data-stu-id="f0890-379">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="f0890-380">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-380">**Mitigations**</span></span>

<span data-ttu-id="f0890-381">Cuando se produce este error, la acción más adecuada consiste en comprender la causa raíz y detener la creación de tantos proveedores de servicios internos.</span><span class="sxs-lookup"><span data-stu-id="f0890-381">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="f0890-382">Pero el error se puede convertir en una advertencia (u omitirse) mediante configuración en `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0890-382">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="f0890-383">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0890-383">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="f0890-384">Comportamiento nuevo de HasOne/HasMany llamado con una sola cadena</span><span class="sxs-lookup"><span data-stu-id="f0890-384">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="f0890-385">Problema de seguimiento n.° 9171</span><span class="sxs-lookup"><span data-stu-id="f0890-385">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="f0890-386">Este cambio se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-386">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-387">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-387">**Old behavior**</span></span>

<span data-ttu-id="f0890-388">Antes de EF Core 3.0, el código para llamar a `HasOne` o `HasMany` con una cadena se interpretaba de manera confusa.</span><span class="sxs-lookup"><span data-stu-id="f0890-388">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="f0890-389">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0890-389">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="f0890-390">El código parece relacionar `Samurai` con otro tipo de entidad mediante la propiedad de navegación `Entrance`, que puede ser privada.</span><span class="sxs-lookup"><span data-stu-id="f0890-390">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="f0890-391">En realidad, este código intenta crear una relación con algún tipo de entidad denominada `Entrance` sin ninguna propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="f0890-391">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="f0890-392">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-392">**New behavior**</span></span>

<span data-ttu-id="f0890-393">A partir de EF Core 3.0, el código anterior ahora hace lo que parecía que debía hacer antes.</span><span class="sxs-lookup"><span data-stu-id="f0890-393">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="f0890-394">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-394">**Why**</span></span>

<span data-ttu-id="f0890-395">El comportamiento anterior era muy confuso, especialmente al leer el código de configuración y al buscar errores.</span><span class="sxs-lookup"><span data-stu-id="f0890-395">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="f0890-396">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-396">**Mitigations**</span></span>

<span data-ttu-id="f0890-397">Esto solo interrumpirá las aplicaciones que configuran de manera explícita las relaciones con cadenas para nombres de tipos y sin especificar explícitamente la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="f0890-397">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="f0890-398">Esto no es habitual.</span><span class="sxs-lookup"><span data-stu-id="f0890-398">This is not common.</span></span>
<span data-ttu-id="f0890-399">El comportamiento anterior se puede obtener al pasar de manera explícita `null` para el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="f0890-399">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="f0890-400">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0890-400">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="f0890-401">La anotación Relational:TypeMapping ahora es simplemente TypeMapping</span><span class="sxs-lookup"><span data-stu-id="f0890-401">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="f0890-402">Problema de seguimiento n.º 9913</span><span class="sxs-lookup"><span data-stu-id="f0890-402">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="f0890-403">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 2).</span><span class="sxs-lookup"><span data-stu-id="f0890-403">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="f0890-404">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-404">**Old behavior**</span></span>

<span data-ttu-id="f0890-405">El nombre de anotación para las anotaciones de asignación de tipos era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="f0890-405">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="f0890-406">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-406">**New behavior**</span></span>

<span data-ttu-id="f0890-407">Ahora, el nombre de anotación para las anotaciones de asignación de tipos es "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="f0890-407">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="f0890-408">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-408">**Why**</span></span>

<span data-ttu-id="f0890-409">Ahora, las asignaciones de tipos se usan para algo más que solo para proveedores de bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="f0890-409">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="f0890-410">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-410">**Mitigations**</span></span>

<span data-ttu-id="f0890-411">Esto solo interrumpirá a las aplicaciones que acceden directamente a la asignación de tipos como una anotación, lo que no es habitual.</span><span class="sxs-lookup"><span data-stu-id="f0890-411">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="f0890-412">La acción más apropiada para corregir es usar la superficie de API para acceder a las asignaciones de tipos en lugar de usar directamente la anotación.</span><span class="sxs-lookup"><span data-stu-id="f0890-412">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="f0890-413">ToTable en un tipo derivado inicia una excepción</span><span class="sxs-lookup"><span data-stu-id="f0890-413">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="f0890-414">Problema de seguimiento n.º 11811</span><span class="sxs-lookup"><span data-stu-id="f0890-414">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="f0890-415">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-415">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-416">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-416">**Old behavior**</span></span>

<span data-ttu-id="f0890-417">Antes de EF Core 3.0, la llamada a `ToTable()` en un tipo derivado se omitía, ya que la única estrategia asignación de herencia era TPH, lo que no es válido.</span><span class="sxs-lookup"><span data-stu-id="f0890-417">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="f0890-418">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-418">**New behavior**</span></span>

<span data-ttu-id="f0890-419">A partir de EF Core 3.0, y en preparación para agregar compatibilidad con TPT y TPC en una versión posterior, ahora la llamada a `ToTable()` en un tipo derivado iniciará una excepción para evitar un cambio de asignación inesperado en el futuro.</span><span class="sxs-lookup"><span data-stu-id="f0890-419">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="f0890-420">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-420">**Why**</span></span>

<span data-ttu-id="f0890-421">En la actualidad no se considera válido asignar un tipo derivado a otra tabla.</span><span class="sxs-lookup"><span data-stu-id="f0890-421">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="f0890-422">Este cambio evita interrupciones en el futuro, cuando se convierta en una operación válida.</span><span class="sxs-lookup"><span data-stu-id="f0890-422">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="f0890-423">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-423">**Mitigations**</span></span>

<span data-ttu-id="f0890-424">Quite todos los intentos de asignar tipos derivados a otras tablas.</span><span class="sxs-lookup"><span data-stu-id="f0890-424">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="f0890-425">ForSqlServerHasIndex se ha reemplazado por HasIndex</span><span class="sxs-lookup"><span data-stu-id="f0890-425">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="f0890-426">Problema de seguimiento n.º 12366</span><span class="sxs-lookup"><span data-stu-id="f0890-426">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="f0890-427">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-427">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-428">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-428">**Old behavior**</span></span>

<span data-ttu-id="f0890-429">Antes de EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` proporcionaba una manera de configurar las columnas que se usaban con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="f0890-429">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="f0890-430">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-430">**New behavior**</span></span>

<span data-ttu-id="f0890-431">A partir de EF Core 3.0, ya se admite el uso de `Include` en un índice en el nivel relacional.</span><span class="sxs-lookup"><span data-stu-id="f0890-431">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="f0890-432">Use `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="f0890-432">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="f0890-433">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-433">**Why**</span></span>

<span data-ttu-id="f0890-434">Este cambio se ha realizado para consolidar la API para índices con `Includes` en un mismo lugar para todos los proveedores de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f0890-434">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="f0890-435">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-435">**Mitigations**</span></span>

<span data-ttu-id="f0890-436">Use la API nueva, como se ha mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f0890-436">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="f0890-437">EF Core ya no envía pragma para el cumplimiento de SQLite FK</span><span class="sxs-lookup"><span data-stu-id="f0890-437">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="f0890-438">Problema de seguimiento n.º 12151</span><span class="sxs-lookup"><span data-stu-id="f0890-438">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="f0890-439">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 3).</span><span class="sxs-lookup"><span data-stu-id="f0890-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="f0890-440">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-440">**Old behavior**</span></span>

<span data-ttu-id="f0890-441">Antes de EF Core 3.0, EF Core enviaba `PRAGMA foreign_keys = 1` cuando se abría una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="f0890-441">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="f0890-442">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-442">**New behavior**</span></span>

<span data-ttu-id="f0890-443">A partir de EF Core 3.0, EF Core ya no envía `PRAGMA foreign_keys = 1` cuando se abre una conexión con SQLite.</span><span class="sxs-lookup"><span data-stu-id="f0890-443">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="f0890-444">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-444">**Why**</span></span>

<span data-ttu-id="f0890-445">Este cambio se ha realizado porque en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3` de forma predeterminada, lo que a su vez significa que el cumplimiento de CD está activado de forma predeterminada y no es necesario habilitarlo explícitamente cada vez que se abra una conexión.</span><span class="sxs-lookup"><span data-stu-id="f0890-445">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="f0890-446">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-446">**Mitigations**</span></span>

<span data-ttu-id="f0890-447">Las claves externas se habilitan de forma predeterminada en SQLitePCLRaw.bundle_e_sqlite3, que en EF Core se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f0890-447">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="f0890-448">Para otros casos, las claves externas se pueden habilitar mediante la especificación de `Foreign Keys=True` en la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="f0890-448">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="f0890-449">Microsoft.EntityFrameworkCore.Sqlite ahora depende de SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="f0890-449">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="f0890-450">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-450">**Old behavior**</span></span>

<span data-ttu-id="f0890-451">Antes de EF Core 3.0, en EF Core se usaba `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="f0890-451">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="f0890-452">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-452">**New behavior**</span></span>

<span data-ttu-id="f0890-453">A partir de EF Core 3.0, en EF Core se usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="f0890-453">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="f0890-454">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-454">**Why**</span></span>

<span data-ttu-id="f0890-455">Este cambio se ha realizado para que la versión de SQLite que se usa en iOS sea coherente con otras plataformas.</span><span class="sxs-lookup"><span data-stu-id="f0890-455">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="f0890-456">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-456">**Mitigations**</span></span>

<span data-ttu-id="f0890-457">Para usar la versión nativa de SQLite en iOS, configure `Microsoft.Data.Sqlite` para usar otra agrupación `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="f0890-457">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="f0890-458">Almacenamiento de valores GUID como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="f0890-458">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="f0890-459">Problema de seguimiento n.º 15078</span><span class="sxs-lookup"><span data-stu-id="f0890-459">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="f0890-460">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-460">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-461">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-461">**Old behavior**</span></span>

<span data-ttu-id="f0890-462">Antes los valores GUID se almacenaban como valores BLOB en SQLite.</span><span class="sxs-lookup"><span data-stu-id="f0890-462">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="f0890-463">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-463">**New behavior**</span></span>

<span data-ttu-id="f0890-464">Ahora los valores GUID se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="f0890-464">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="f0890-465">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-465">**Why**</span></span>

<span data-ttu-id="f0890-466">El formato binario de los GUID no está normalizado.</span><span class="sxs-lookup"><span data-stu-id="f0890-466">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="f0890-467">El almacenamiento de los valores como TEXT mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="f0890-467">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="f0890-468">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-468">**Mitigations**</span></span>

<span data-ttu-id="f0890-469">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="f0890-469">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="f0890-470">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="f0890-470">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="f0890-471">Microsoft.Data.Sqlite sigue siendo capaz de leer valores GUID de ambas columnas BLOB y TEXT. Sin embargo, dado que el formato predeterminado de los parámetros y las constantes ha cambiado, seguramente deberá realizar alguna acción en la mayoría de casos que impliquen el uso de valores GUID.</span><span class="sxs-lookup"><span data-stu-id="f0890-471">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="f0890-472">Ahora los valores char se almacenan como TEXT en SQLite</span><span class="sxs-lookup"><span data-stu-id="f0890-472">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="f0890-473">Problema de seguimiento n.º 15020</span><span class="sxs-lookup"><span data-stu-id="f0890-473">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="f0890-474">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-474">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-475">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-475">**Old behavior**</span></span>

<span data-ttu-id="f0890-476">Anteriormente los valores char se almacenaban como valores INTEGER en SQLite.</span><span class="sxs-lookup"><span data-stu-id="f0890-476">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="f0890-477">Por ejemplo, un valor char de *A* se almacenaba como el valor entero 65.</span><span class="sxs-lookup"><span data-stu-id="f0890-477">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="f0890-478">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-478">**New behavior**</span></span>

<span data-ttu-id="f0890-479">Ahora, los valores char se almacenan como TEXT.</span><span class="sxs-lookup"><span data-stu-id="f0890-479">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="f0890-480">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-480">**Why**</span></span>

<span data-ttu-id="f0890-481">El almacenamiento de valores como TEXT es más natural y mejora la compatibilidad de la base de datos con otras tecnologías.</span><span class="sxs-lookup"><span data-stu-id="f0890-481">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="f0890-482">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-482">**Mitigations**</span></span>

<span data-ttu-id="f0890-483">Puede migrar las bases de datos existentes al nuevo formato ejecutando SQL de la siguiente forma.</span><span class="sxs-lookup"><span data-stu-id="f0890-483">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="f0890-484">En EF Core, también puede seguir usando el comportamiento anterior configurando un convertidor de valores en estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="f0890-484">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="f0890-485">Microsoft.Data.Sqlite también puede leer valores de caracteres tanto de columnas INTEGER como de columnas TEXT, por lo que es posible que no deba hacer nada dependiendo de su caso.</span><span class="sxs-lookup"><span data-stu-id="f0890-485">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="f0890-486">Ahora los id. de migración se generan usando el calendario de la referencia cultural invariable</span><span class="sxs-lookup"><span data-stu-id="f0890-486">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="f0890-487">Problema de seguimiento n.º 12978</span><span class="sxs-lookup"><span data-stu-id="f0890-487">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="f0890-488">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-488">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-489">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-489">**Old behavior**</span></span>

<span data-ttu-id="f0890-490">Los id. de migración se generaban de forma involuntaria con el calendario de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="f0890-490">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="f0890-491">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-491">**New behavior**</span></span>

<span data-ttu-id="f0890-492">Ahora los id. de migración siempre se generan usando el calendario de la referencia cultural invariable (gregoriano).</span><span class="sxs-lookup"><span data-stu-id="f0890-492">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="f0890-493">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-493">**Why**</span></span>

<span data-ttu-id="f0890-494">El orden de las migraciones es importante al actualizar la base de datos o al solucionar conflictos de combinación.</span><span class="sxs-lookup"><span data-stu-id="f0890-494">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="f0890-495">Al usar el calendario invariable, se evitan problemas de ordenación que pueden producirse si los miembros del equipo tienen distintos calendarios del sistema.</span><span class="sxs-lookup"><span data-stu-id="f0890-495">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="f0890-496">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-496">**Mitigations**</span></span>

<span data-ttu-id="f0890-497">Esta cambio afecta a todas las personas que usan un calendario no gregoriano en el que el año sea superior al del calendario gregoriano (como el calendario budista tailandés).</span><span class="sxs-lookup"><span data-stu-id="f0890-497">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="f0890-498">Los id. de migración existentes deberán actualizarse para que las migraciones nuevas se ordenen después de las existentes.</span><span class="sxs-lookup"><span data-stu-id="f0890-498">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="f0890-499">Puede ver el id. de migración en el atributo Migration de los archivos de diseñador de la migración.</span><span class="sxs-lookup"><span data-stu-id="f0890-499">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="f0890-500">También debe actualizarse la tabla de historial de migraciones.</span><span class="sxs-lookup"><span data-stu-id="f0890-500">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="f0890-501">Cambio de nombre de LogQueryPossibleExceptionWithAggregateOperator</span><span class="sxs-lookup"><span data-stu-id="f0890-501">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="f0890-502">Problema de seguimiento n.º 10985</span><span class="sxs-lookup"><span data-stu-id="f0890-502">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="f0890-503">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-503">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-504">**Cambio**</span><span class="sxs-lookup"><span data-stu-id="f0890-504">**Change**</span></span>

<span data-ttu-id="f0890-505">Se ha cambiado el nombre de `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` a `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="f0890-505">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="f0890-506">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-506">**Why**</span></span>

<span data-ttu-id="f0890-507">Conviene alinear el nombre de este evento de advertencia con el del resto de eventos de advertencia.</span><span class="sxs-lookup"><span data-stu-id="f0890-507">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="f0890-508">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-508">**Mitigations**</span></span>

<span data-ttu-id="f0890-509">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="f0890-509">Use the new name.</span></span> <span data-ttu-id="f0890-510">(Tenga en cuenta que el número de id. evento sigue siendo el mismo).</span><span class="sxs-lookup"><span data-stu-id="f0890-510">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="f0890-511">Clarificación de la API para nombres de restricciones de claves externas</span><span class="sxs-lookup"><span data-stu-id="f0890-511">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="f0890-512">Problema de seguimiento n.º 10730</span><span class="sxs-lookup"><span data-stu-id="f0890-512">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="f0890-513">Este cambio se ha introducido en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="f0890-513">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="f0890-514">**Comportamiento anterior**</span><span class="sxs-lookup"><span data-stu-id="f0890-514">**Old behavior**</span></span>

<span data-ttu-id="f0890-515">Antes de EF Core 3.0, se utilizaba simplemente el término "nombre" para hacer referencia a los nombres de las restricciones de claves externas.</span><span class="sxs-lookup"><span data-stu-id="f0890-515">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="f0890-516">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0890-516">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="f0890-517">**Comportamiento nuevo**</span><span class="sxs-lookup"><span data-stu-id="f0890-517">**New behavior**</span></span>

<span data-ttu-id="f0890-518">A partir de EF Core 3.0, el término con el que se hace referencia a los nombres de las restricciones de claves externas es "nombre de la restricción".</span><span class="sxs-lookup"><span data-stu-id="f0890-518">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="f0890-519">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0890-519">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="f0890-520">**Por qué**</span><span class="sxs-lookup"><span data-stu-id="f0890-520">**Why**</span></span>

<span data-ttu-id="f0890-521">Este cambio permite mejorar la coherencia relativa a la nomenclatura en este aspecto y aclarar que se trata del nombre de una restricción de clave externa, y no del de la columna o propiedad en la que está definida la clave externa.</span><span class="sxs-lookup"><span data-stu-id="f0890-521">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="f0890-522">**Mitigaciones**</span><span class="sxs-lookup"><span data-stu-id="f0890-522">**Mitigations**</span></span>

<span data-ttu-id="f0890-523">Use el nuevo nombre.</span><span class="sxs-lookup"><span data-stu-id="f0890-523">Use the new name.</span></span>
