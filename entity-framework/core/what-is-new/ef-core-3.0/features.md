---
title: 'Novedades de EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: d61fa884f4669daa220ffc96ae59dd63518e6d5a
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921672"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="7e7f3-102">Nuevas características incluidas en EF Core 3.0 (actualmente en versión preliminar)</span><span class="sxs-lookup"><span data-stu-id="7e7f3-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7e7f3-103">Tenga en cuenta que los conjuntos de características y las programaciones de las versiones futuras siempre están sujetos a cambios y, aunque intentamos mantener esta página actualizada, es posible que no refleje nuestros planes más recientes en todo momento.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="7e7f3-104">En la lista siguiente se incluyen las principales características nuevas planeadas para EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="7e7f3-105">La mayoría de estas características no están incluidas en la versión preliminar actual, pero estarán disponibles cuando avancemos hacia RTM.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="7e7f3-106">El motivo es que al principio de la versión, nos centraremos en la implementación de los [cambios importantes](xref:core/what-is-new/ef-core-3.0/breaking-changes) planeados.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="7e7f3-107">Muchos de estos cambios son, por sí solos, mejoras de EF Core.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="7e7f3-108">Otros muchos son necesarios para desbloquear más mejoras.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="7e7f3-109">Para obtener una lista completa de las correcciones de errores y las mejoras en curso, puede ver [esta consulta en nuestro rastreador de problemas](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="7e7f3-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="7e7f3-110">Mejoras de LINQ</span><span class="sxs-lookup"><span data-stu-id="7e7f3-110">LINQ improvements</span></span> 

[<span data-ttu-id="7e7f3-111">Problema de seguimiento n.º 12795</span><span class="sxs-lookup"><span data-stu-id="7e7f3-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="7e7f3-112">Se ha iniciado el trabajo en esta característica, pero no se incluye en la versión preliminar actual.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="7e7f3-113">LINQ permite escribir consultas a la base de datos sin abandonar el idioma de elección, con lo que se aprovecha la información de tipo enriquecido para obtener la comprobación de IntelliSense y de tipos en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="7e7f3-114">Pero LINQ también le permite escribir un número ilimitado de consultas complicadas, y eso siempre ha sido un gran reto para los proveedores de LINQ.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="7e7f3-115">En las primeras versiones de EF Core, resolvimos esto en parte al calcular qué partes de una consulta se podían traducir a SQL, y luego al permitir que el resto de la consulta se ejecutara en memoria en el cliente.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="7e7f3-116">Esta ejecución de cliente puede ser deseable en algunas situaciones, pero en muchos otros casos puede dar como resultado consultas ineficientes que pueden no ser identificadas hasta que una aplicación se implemente en producción.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="7e7f3-117">En EF Core 3.0, tenemos previsto hacer cambios profundos en cómo funciona nuestra implementación de LINQ, y cómo la probamos.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="7e7f3-118">Los objetivos son hacerla más eficaz (por ejemplo, evitar interrumpir las consultas en las versiones de revisión), poder traducir más expresiones correctamente a SQL, generar consultas eficientes en más casos y evitar que las consultas ineficientes pasen desapercibidas.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="7e7f3-119">Compatibilidad con Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7e7f3-119">Cosmos DB support</span></span> 

[<span data-ttu-id="7e7f3-120">Problema de seguimiento n.º 8443</span><span class="sxs-lookup"><span data-stu-id="7e7f3-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="7e7f3-121">Esta característica está incluida en la versión preliminar actual, pero todavía no está completa.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="7e7f3-122">Estamos trabajando en un proveedor de Cosmos DB para EF Core, para que los desarrolladores familiarizados con el modelo de programación de EF puedan utilizar fácilmente Azure Cosmos DB como base de datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="7e7f3-123">El objetivo es hacer que algunas de las ventajas de Cosmos DB, como la distribución global, la disponibilidad "AlwaysOn", la escalabilidad elástica y la baja latencia, sean aún más accesibles para los desarrolladores de .NET.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="7e7f3-124">El proveedor habilitará la mayoría de las características de EF Core, como el seguimiento automático de cambios, LINQ y conversiones de valores, en comparación con SQL API de Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="7e7f3-125">Comenzamos este esfuerzo antes de EF Core 2.2 y [hemos puesto a disposición algunas versiones preliminares del proveedor](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="7e7f3-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="7e7f3-126">El nuevo plan es continuar desarrollando el proveedor junto con EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="7e7f3-127">Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales</span><span class="sxs-lookup"><span data-stu-id="7e7f3-127">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="7e7f3-128">Problema de seguimiento n.º 9005</span><span class="sxs-lookup"><span data-stu-id="7e7f3-128">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="7e7f3-129">Esta característica se introducirá en EF Core 3.0 (versión preliminar 4).</span><span class="sxs-lookup"><span data-stu-id="7e7f3-129">This feature will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="7e7f3-130">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7e7f3-130">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

<span data-ttu-id="7e7f3-131">A partir de la versión EF Core 3.0, si `OrderDetails` pertenece a `Order` o está asignado a la misma tabla explícitamente, será posible agregar `Order` sin `OrderDetails`; todas las propiedades `OrderDetails`, salvo la clave principal, se asignarán a columnas que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-131">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="7e7f3-132">Al realizar consultas, EF Core establecerá `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor, o bien no tiene las propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-132">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="7e7f3-133">Compatibilidad con C# 8.0</span><span class="sxs-lookup"><span data-stu-id="7e7f3-133">C# 8.0 support</span></span>

<span data-ttu-id="7e7f3-134">[Problema de seguimiento n.º 12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Problema de seguimiento n.º 10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="7e7f3-134">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="7e7f3-135">Se ha iniciado el trabajo en esta característica, pero no se incluye en la versión preliminar actual.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-135">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="7e7f3-136">Queremos que, mientras usan EF Core, nuestros clientes aprovechen algunas de las [nuevas características que se incluyen en C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/), como las secuencias de sincronización (incluida `await foreach`) y los tipos de referencia que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-136">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="7e7f3-137">Ingeniería inversa de vistas de base de datos</span><span class="sxs-lookup"><span data-stu-id="7e7f3-137">Reverse engineering of database views</span></span>

[<span data-ttu-id="7e7f3-138">Problema de seguimiento n.º 1679</span><span class="sxs-lookup"><span data-stu-id="7e7f3-138">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="7e7f3-139">Esta característica no está incluida en la versión preliminar actual.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-139">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="7e7f3-140">Los [tipos de consulta](xref:core/modeling/query-types), presentados en EF Core 2.1 y considerados tipos de entidad sin claves en EF Core 3.0, representan datos que se pueden leer desde la base de datos, pero que no se pueden actualizar.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-140">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="7e7f3-141">Esta característica los convierte en una elección excelente para las vistas de base de datos en la mayoría de los escenarios, por lo que tenemos previsto automatizar la creación de tipos de entidad sin claves al usar de técnicas de ingeniería inversa en vistas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-141">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="7e7f3-142">EF 6.3 en .NET Core</span><span class="sxs-lookup"><span data-stu-id="7e7f3-142">EF 6.3 on .NET Core</span></span>

[<span data-ttu-id="7e7f3-143">Problema de seguimiento n.º 271 de EF 6</span><span class="sxs-lookup"><span data-stu-id="7e7f3-143">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="7e7f3-144">Se ha iniciado el trabajo en esta característica, pero no se incluye en la versión preliminar actual.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-144">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="7e7f3-145">Entendemos que muchas aplicaciones existentes utilizan versiones anteriores de EF, y que portarlas a EF Core solo para aprovechar las ventajas de .NET Core puede requerir a veces un esfuerzo considerable.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-145">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="7e7f3-146">Por ese motivo, adaptaremos la próxima versión de EF 6 para se ejecute en .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-146">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="7e7f3-147">Hacemos esto para facilitar la portabilidad de las aplicaciones existentes con cambios mínimos.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-147">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="7e7f3-148">Habrá algunas limitaciones.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-148">There are going to be some limitations.</span></span> <span data-ttu-id="7e7f3-149">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7e7f3-149">For example:</span></span>
- <span data-ttu-id="7e7f3-150">Se necesitarán nuevos proveedores para trabajar con otras bases de datos, además de la compatibilidad de SQL Server incluida en .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-150">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="7e7f3-151">No se habilitará la compatibilidad espacial con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-151">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="7e7f3-152">Tenga en cuenta también que, en este momento, no hay características nuevas planeadas para EF 6.</span><span class="sxs-lookup"><span data-stu-id="7e7f3-152">Note also that there are no new features planned for EF 6 at this point.</span></span>
