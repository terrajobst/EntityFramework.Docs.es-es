---
title: Novedades en EF Core 5.0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: e858379cc46abbef999fd32a3685e1d522524889
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052021"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="38317-102">Novedades en EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="38317-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="38317-103">EF Core 5.0 está actualmente en desarrollo.</span><span class="sxs-lookup"><span data-stu-id="38317-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="38317-104">Esta página contendrá información general sobre los cambios interesantes introducidos en cada versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="38317-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>
<span data-ttu-id="38317-105">La primera versión preliminar de EF Core 5.0 se espera provisionalmente para el primer trimestre de 2020.</span><span class="sxs-lookup"><span data-stu-id="38317-105">The first preview of EF Core 5.0 is tentatively expected in in the first quarter of 2020.</span></span>

<span data-ttu-id="38317-106">Esta página no duplica el [plan para EF Core 5.0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="38317-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="38317-107">En el plan se describen los temas generales relativos a EF Core 5.0, incluido todo lo que estamos planeando incluir antes de publicar la versión final.</span><span class="sxs-lookup"><span data-stu-id="38317-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="38317-108">A medida que se publique el contenido, se agregarán vínculos que redirigirán de esta página a la documentación oficial.</span><span class="sxs-lookup"><span data-stu-id="38317-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1-not-yet-shipped"></a><span data-ttu-id="38317-109">Versión preliminar 1 (no publicada todavía)</span><span class="sxs-lookup"><span data-stu-id="38317-109">Preview 1 (Not yet shipped)</span></span>

### <a name="simple-logging"></a><span data-ttu-id="38317-110">Registro sencillo</span><span class="sxs-lookup"><span data-stu-id="38317-110">Simple logging</span></span>

<span data-ttu-id="38317-111">Esta característica agrega funcionalidad similar a `Database.Log` en EF6.</span><span class="sxs-lookup"><span data-stu-id="38317-111">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="38317-112">Es decir, proporciona una manera sencilla de obtener registros de EF Core sin necesidad de configurar ningún tipo de plataforma de registro externa.</span><span class="sxs-lookup"><span data-stu-id="38317-112">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="38317-113">La documentación preliminar se incluye en el [estado semanal de EF del 5 de diciembre de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="38317-113">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="38317-114">En el problema [n.º 2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085) se realiza el seguimiento de la documentación adicional.</span><span class="sxs-lookup"><span data-stu-id="38317-114">Additional documentation is tracked by issue [#2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="38317-115">Forma sencilla de generar contenido SQL</span><span class="sxs-lookup"><span data-stu-id="38317-115">Simple way to get generated SQL</span></span>

<span data-ttu-id="38317-116">EF Core 5.0 presenta el método de extensión `ToQueryString` que devolverá el contenido SQL que EF Core generará al ejecutar una consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="38317-116">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="38317-117">La documentación preliminar se incluye en el [estado semanal de EF del 9 de enero de 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="38317-117">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="38317-118">En el problema [n.º 1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331) se realiza el seguimiento de la documentación adicional.</span><span class="sxs-lookup"><span data-stu-id="38317-118">Additional documentation is tracked by issue [#1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="38317-119">Vistas de depuración mejoradas</span><span class="sxs-lookup"><span data-stu-id="38317-119">Enhanced debug views</span></span>

<span data-ttu-id="38317-120">Las vistas de depuración son una forma fácil de consultar los aspectos internos de EF Core al depurar problemas.</span><span class="sxs-lookup"><span data-stu-id="38317-120">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="38317-121">Hace algún tiempo ya se implementó una vista de depuración para el modelo.</span><span class="sxs-lookup"><span data-stu-id="38317-121">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="38317-122">En el caso de EF Core 5.0, hemos facilitado la lectura de la vista de modelo y hemos agregado una nueva vista de depuración para las entidades de las que se ha realizado un seguimiento en el administrador de estado.</span><span class="sxs-lookup"><span data-stu-id="38317-122">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="38317-123">La documentación preliminar se incluye en el [estado semanal de EF del 12 de diciembre de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="38317-123">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="38317-124">En el problema [n.º 2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086) se realiza el seguimiento de la documentación adicional.</span><span class="sxs-lookup"><span data-stu-id="38317-124">Additional documentation is tracked by issue [#2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="38317-125">Posibilidad de cambiar la conexión o la cadena de conexión en una instancia inicializada de DbContext</span><span class="sxs-lookup"><span data-stu-id="38317-125">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="38317-126">Ahora es más fácil crear una instancia de DbContext sin ninguna conexión o cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="38317-126">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="38317-127">Además, la conexión o la cadena de conexión ahora también se pueden mutar en la instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="38317-127">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="38317-128">Esto permite que la misma instancia de contexto se conecte dinámicamente a diferentes bases de datos.</span><span class="sxs-lookup"><span data-stu-id="38317-128">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="38317-129">En el problema [n.º 2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075) se realiza el seguimiento de la documentación.</span><span class="sxs-lookup"><span data-stu-id="38317-129">Documentation is tracked by issue [#2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="38317-130">Proxies de seguimiento de cambios</span><span class="sxs-lookup"><span data-stu-id="38317-130">Change-tracking proxies</span></span>

<span data-ttu-id="38317-131">Ahora EF Core puede generar proxies del entorno de ejecución que implementen automáticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) y [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="38317-131">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="38317-132">A continuación, los cambios de valor en las propiedades de las entidades se notifican directamente a EF Core, lo cual evita la necesidad de buscar los cambios.</span><span class="sxs-lookup"><span data-stu-id="38317-132">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="38317-133">Sin embargo, los proxies vienen con su propio conjunto de limitaciones, por lo que no son para todo el mundo.</span><span class="sxs-lookup"><span data-stu-id="38317-133">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="38317-134">En el problema [n.º 2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076) se realiza el seguimiento de la documentación.</span><span class="sxs-lookup"><span data-stu-id="38317-134">Documentation is tracked by issue [#2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="38317-135">Control mejorado de la semántica de valores NULL de base de datos</span><span class="sxs-lookup"><span data-stu-id="38317-135">Improved handling of database null semantics</span></span>

<span data-ttu-id="38317-136">Normalmente, las bases de datos relacionales tratan NULL como valores desconocidos y, por lo tanto, no son iguales a otros valores NULL.</span><span class="sxs-lookup"><span data-stu-id="38317-136">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="38317-137">C#, por otro lado, trata los valores NULL como valores definidos y los compara igual que con cualquier otro valor NULL.</span><span class="sxs-lookup"><span data-stu-id="38317-137">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="38317-138">De forma predeterminada, EF Core traduce las consultas para que usen la semántica de valores NULL de C#.</span><span class="sxs-lookup"><span data-stu-id="38317-138">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="38317-139">EF Core 5.0 mejora en gran medida la eficacia de dichas traducciones.</span><span class="sxs-lookup"><span data-stu-id="38317-139">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="38317-140">En el problema [n.º 1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612) se realiza el seguimiento de la documentación.</span><span class="sxs-lookup"><span data-stu-id="38317-140">Documentation is tracked by issue [#1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="38317-141">Propiedades del indizador</span><span class="sxs-lookup"><span data-stu-id="38317-141">Indexer properties</span></span>

<span data-ttu-id="38317-142">EF Core 5.0 admite la asignación de propiedades de indizador de C#.</span><span class="sxs-lookup"><span data-stu-id="38317-142">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="38317-143">Esto permite a las entidades actuar como bolsas de propiedades en las que las columnas se asignan a las propiedades con nombre en la bolsa.</span><span class="sxs-lookup"><span data-stu-id="38317-143">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="38317-144">En el problema [n.º 2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018) se realiza el seguimiento de la documentación.</span><span class="sxs-lookup"><span data-stu-id="38317-144">Documentation is tracked by issue [#2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="38317-145">Generación de restricciones CHECK para las asignaciones de enumeración</span><span class="sxs-lookup"><span data-stu-id="38317-145">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="38317-146">Las migraciones de EF Core 5.0 ahora pueden generar restricciones CHECK para las asignaciones de propiedades de enumeración.</span><span class="sxs-lookup"><span data-stu-id="38317-146">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="38317-147">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="38317-147">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="38317-148">En el problema [n.º 2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082) se realiza el seguimiento de la documentación.</span><span class="sxs-lookup"><span data-stu-id="38317-148">Documentation is tracked by issue [#2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="38317-149">Traducciones de consultas para más construcciones DateTime</span><span class="sxs-lookup"><span data-stu-id="38317-149">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="38317-150">Ahora las consultas que contienen la nueva construcción DataTime se traducen.</span><span class="sxs-lookup"><span data-stu-id="38317-150">Queries containing new DataTime construction are now translated.</span></span>
<span data-ttu-id="38317-151">Además, la función de SQL Server DateDiffWeek ahora se puede asignar.</span><span class="sxs-lookup"><span data-stu-id="38317-151">Also, the SQL Server function DateDiffWeek is now mapped.</span></span>

<span data-ttu-id="38317-152">En el problema [n.º 2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079) realiza se el seguimiento de la documentación.</span><span class="sxs-lookup"><span data-stu-id="38317-152">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="38317-153">Traducciones de consultas para más construcciones de matriz de bytes</span><span class="sxs-lookup"><span data-stu-id="38317-153">Query translations for more byte array constructs</span></span>

<span data-ttu-id="38317-154">Las consultas que usan Contains, Length, SequenceEqual, etc. en las propiedades byte[] ahora se traducen a SQL.</span><span class="sxs-lookup"><span data-stu-id="38317-154">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="38317-155">La documentación preliminar se incluye en el [estado semanal de EF del 5 de diciembre de 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="38317-155">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="38317-156">En el problema [n.º 2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079) se realiza el seguimiento de la documentación adicional.</span><span class="sxs-lookup"><span data-stu-id="38317-156">Additional documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="38317-157">Traducción de consultas para Reverse</span><span class="sxs-lookup"><span data-stu-id="38317-157">Query translation for Reverse</span></span>

<span data-ttu-id="38317-158">Las consultas que usan `Reverse` ahora se traducen.</span><span class="sxs-lookup"><span data-stu-id="38317-158">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="38317-159">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="38317-159">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="38317-160">En el problema [n.º 2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079) realiza se el seguimiento de la documentación.</span><span class="sxs-lookup"><span data-stu-id="38317-160">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="38317-161">Traducción de consultas para operadores bit a bit</span><span class="sxs-lookup"><span data-stu-id="38317-161">Query translation for bitwise operators</span></span>

<span data-ttu-id="38317-162">Las consultas que usan operadores bit a bit ahora se traducen en más casos, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="38317-162">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="38317-163">En el problema [n.º 2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079) realiza se el seguimiento de la documentación.</span><span class="sxs-lookup"><span data-stu-id="38317-163">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="38317-164">Traducción de consultas para cadenas en Cosmos</span><span class="sxs-lookup"><span data-stu-id="38317-164">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="38317-165">Al emplear el proveedor Azure Cosmos DB, las consultas que usan los métodos de cadena Contains, StartsWith y EndsWith ahora se traducen.</span><span class="sxs-lookup"><span data-stu-id="38317-165">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="38317-166">En el problema [n.º 2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079) realiza se el seguimiento de la documentación.</span><span class="sxs-lookup"><span data-stu-id="38317-166">Documentation is tracked by issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).</span></span>
