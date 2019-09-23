---
title: 'Novedades de EF Core 3.0: EF Core'
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005557"
---
# <a name="new-features-included-in-ef-core-30"></a><span data-ttu-id="33b3d-102">Nuevas características incluidas en EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="33b3d-102">New features included in EF Core 3.0</span></span>

<span data-ttu-id="33b3d-103">En la lista siguiente se incluyen las principales características nuevas planeadas para EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="33b3d-103">The following list includes the major new features planned for EF Core 3.0.</span></span>

<span data-ttu-id="33b3d-104">EF Core 3.0 es una versión principal que también presenta numerosos [cambios importantes](xref:core/what-is-new/ef-core-3.0/breaking-changes). Se trata de mejoras en la API que podrían afectar negativamente a las aplicaciones existentes.</span><span class="sxs-lookup"><span data-stu-id="33b3d-104">EF Core 3.0 is a major release and also contains numerous [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-improvements"></a><span data-ttu-id="33b3d-105">Mejoras de LINQ</span><span class="sxs-lookup"><span data-stu-id="33b3d-105">LINQ improvements</span></span> 

<span data-ttu-id="33b3d-106">LINQ permite escribir consultas a la base de datos sin abandonar el idioma de elección, con lo que se aprovecha la información de tipo enriquecido para ofrecer la comprobación de IntelliSense y de tipos en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="33b3d-106">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="33b3d-107">Pero LINQ también permite escribir un número ilimitado de consultas complicadas que contienen expresiones arbitrarias (llamadas a métodos u operaciones).</span><span class="sxs-lookup"><span data-stu-id="33b3d-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="33b3d-108">Controlar todas esas combinaciones siempre ha sido un desafío importante para los proveedores LINQ.</span><span class="sxs-lookup"><span data-stu-id="33b3d-108">Handling all those combinations has always been a significant challenge for LINQ providers.</span></span>
<span data-ttu-id="33b3d-109">En EF Core 3.0, hemos vuelto a escribir nuestra implementación de LINQ para habilitar la traducción de más expresiones en SQL, generar consultas eficaces en más casos, evitar que las consultas ineficaces no se detecten y facilitar la introducción gradual de las mejoras de las capacidades y el rendimiento de la nueva consulta, sin interrumpir las aplicaciones y los proveedores de datos existentes.</span><span class="sxs-lookup"><span data-stu-id="33b3d-109">In EF Core 3.0, we've rewritten our LINQ implementation to enable translating more expressions into SQL, to generate efficient queries in more cases, to prevent inefficient queries from going undetected, and to make it easier for us to gradually introduce new query capabilities and performance improvementswithout breaking existing applications and data providers.</span></span>

### <a name="client-evaluation"></a><span data-ttu-id="33b3d-110">Evaluación de cliente</span><span class="sxs-lookup"><span data-stu-id="33b3d-110">Client evaluation</span></span>

<span data-ttu-id="33b3d-111">El cambio de diseño principal en EF Core 3.0 está relacionado con cómo controla las expresiones LINQ que no se pueden traducir a SQL o parámetros:</span><span class="sxs-lookup"><span data-stu-id="33b3d-111">The main design change in EF Core 3.0 has to do with how it handles LINQ expressions that it cannot translate to SQL or parameters:</span></span>

<span data-ttu-id="33b3d-112">En las primeras versiones, EF Core simplemente averiguaba qué partes de una consulta se pueden traducir a SQL y ejecutaba el resto de la consulta en el cliente.</span><span class="sxs-lookup"><span data-stu-id="33b3d-112">In the first few versions, EF Core simply figured out what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="33b3d-113">Este tipo de ejecución en el lado cliente puede ser una opción interesante en algunas situaciones, pero en muchos otros casos puede dar lugar a consultas ineficaces.</span><span class="sxs-lookup"><span data-stu-id="33b3d-113">This type of client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>
<span data-ttu-id="33b3d-114">Por ejemplo, si EF Core 2.2 no podía traducir un predicado en una llamada a `Where()`, ejecutaba una instrucción SQL sin filtro, leía todas las filas de la base de datos y luego las filtraba en memoria.</span><span class="sxs-lookup"><span data-stu-id="33b3d-114">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed a SQL statement without a filter, read all all the rows from the database, and then filtered them in-memory.</span></span>
<span data-ttu-id="33b3d-115">Esta operación puede ser aceptable si la base de datos contiene pocas filas, pero puede dar lugar a problemas de rendimiento considerables o incluso errores en la aplicación si la base de datos contiene muchas filas.</span><span class="sxs-lookup"><span data-stu-id="33b3d-115">That may be acceptable if the database contains a small number of rows, but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>
<span data-ttu-id="33b3d-116">En EF Core 3.0 hemos restringido la evaluación de cliente para que solo suceda en la proyección de nivel superior (la última llamada a `Select()`).</span><span class="sxs-lookup"><span data-stu-id="33b3d-116">In EF Core 3.0 we have restricted client evaluation to only happen on the top-level projection (the last call to `Select()`).</span></span>
<span data-ttu-id="33b3d-117">Cuando EF Core 3.0 detecta expresiones que no se pueden traducir en ningún otro lugar de la consulta, produce una excepción en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="33b3d-117">When EF Core 3.0 detects expressions that cannot be translated anywhere else in the query, it throws a runtime exception.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="33b3d-118">Compatibilidad con Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="33b3d-118">Cosmos DB support</span></span> 

<span data-ttu-id="33b3d-119">Con el proveedor de Cosmos DB para EF Core, los desarrolladores que están familiarizados con el modelo de programación de EF puedan usar fácilmente Azure Cosmos DB como base de datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="33b3d-119">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="33b3d-120">El objetivo es hacer que algunas de las ventajas de Cosmos DB, como la distribución global, la disponibilidad "AlwaysOn", la escalabilidad elástica y la baja latencia, sean aún más accesibles para los desarrolladores de .NET.</span><span class="sxs-lookup"><span data-stu-id="33b3d-120">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="33b3d-121">El proveedor habilitará la mayoría de las características de EF Core, como el seguimiento automático de cambios, LINQ y conversiones de valores, en comparación con SQL API de Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="33b3d-121">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="33b3d-122">Compatibilidad con C# 8.0</span><span class="sxs-lookup"><span data-stu-id="33b3d-122">C# 8.0 support</span></span>

<span data-ttu-id="33b3d-123">EF Core 3.0 aprovecha algunas de las nuevas características de C# 8.0:</span><span class="sxs-lookup"><span data-stu-id="33b3d-123">EF Core 3.0 takes advantage of some of the new features in C# 8.0:</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="33b3d-124">Secuencias asincrónicas</span><span class="sxs-lookup"><span data-stu-id="33b3d-124">Asynchronous streams</span></span>

<span data-ttu-id="33b3d-125">Los resultados de la consulta asincrónica se exponen ahora mediante la nueva interfaz de `await foreach` estándar y se pueden usar con `IAsyncEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="33b3d-125">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a><span data-ttu-id="33b3d-126">Tipos de referencia que aceptan valores NULL</span><span class="sxs-lookup"><span data-stu-id="33b3d-126">Nullable reference types</span></span> 

<span data-ttu-id="33b3d-127">Cuando esta nueva característica está habilitada en el código, EF Core puede tener en cuenta la nulabilidad de las propiedades de los tipos de referencia (ya sea de tipos primitivos, como las propiedades de navegación o de cadena) para decidir la nulabilidad de las columnas y las relaciones de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="33b3d-127">When this new feature is enabled in your code, EF Core can reason about the nullability of properties of refrence types (either of primitive types like string or navigation properties) to decide the nullability of columns and relationships in the database.</span></span>

## <a name="interception"></a><span data-ttu-id="33b3d-128">Interception</span><span class="sxs-lookup"><span data-stu-id="33b3d-128">Interception</span></span>

<span data-ttu-id="33b3d-129">La nueva API de interceptación en EF Core 3.0 permite observar y modificar mediante programación el resultado de las operaciones detalladas de la base de datos que se producen como parte del funcionamiento normal de EF Core, como abrir conexiones, iniciar transacciones y ejecutar comandos.</span><span class="sxs-lookup"><span data-stu-id="33b3d-129">The new interception API in EF Core 3.0 allows programatically observing and modifying the outcome of low-level database operations that occur as part of the normal operation of EF Core, such as opening connections, initating transactions, and executing commands.</span></span> 

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="33b3d-130">Ingeniería inversa de vistas de base de datos</span><span class="sxs-lookup"><span data-stu-id="33b3d-130">Reverse engineering of database views</span></span>

<span data-ttu-id="33b3d-131">Los tipos de entidad sin claves (denominados anteriormente [tipos de consulta](xref:core/modeling/query-types)) representan los datos que se pueden leer de la base de datos, pero que no se pueden actualizar.</span><span class="sxs-lookup"><span data-stu-id="33b3d-131">Entity types without keys (previously known as [query types](xref:core/modeling/query-types)) represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="33b3d-132">Esta característica los convierte en una elección excelente para asignar vistas de base de datos en la mayoría de los escenarios, por lo que hemos automatizado la creación de tipos de entidad sin claves al usar técnicas de ingeniería inversa en vistas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="33b3d-132">This characteristic makes them an excellent fit for mapping database views in most scenarios, so we automated the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="33b3d-133">Ahora, las entidades dependientes que comparten la tabla con la entidad de seguridad son opcionales</span><span class="sxs-lookup"><span data-stu-id="33b3d-133">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="33b3d-134">A partir de la versión EF Core 3.0, si `OrderDetails` pertenece a `Order` o está asignado a la misma tabla explícitamente, será posible agregar `Order` sin `OrderDetails`; todas las propiedades `OrderDetails`, salvo la clave principal, se asignarán a columnas que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="33b3d-134">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="33b3d-135">Al realizar consultas, EF Core establecerá `OrderDetails` en `null` si ninguna de las propiedades necesarias tiene un valor, o bien no tiene las propiedades necesarias más allá de la clave principal y todas las propiedades son `null`.</span><span class="sxs-lookup"><span data-stu-id="33b3d-135">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

``` csharp
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

## <a name="ef-63-on-net-core"></a><span data-ttu-id="33b3d-136">EF 6.3 en .NET Core</span><span class="sxs-lookup"><span data-stu-id="33b3d-136">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="33b3d-137">Entendemos que muchas aplicaciones existentes utilizan versiones anteriores de EF, y que portarlas a EF Core solo para aprovechar las ventajas de .NET Core puede requerir a veces un esfuerzo considerable.</span><span class="sxs-lookup"><span data-stu-id="33b3d-137">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="33b3d-138">Por ese motivo, hemos habilitado la versión más reciente de EF 6 para que se ejecute en .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="33b3d-138">For that reason, we have enabled the newewst version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="33b3d-139">Existen algunas limitaciones, como por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="33b3d-139">There are some limitations, for example:</span></span>
- <span data-ttu-id="33b3d-140">Se necesitan nuevos proveedores para trabajar en .NET Core</span><span class="sxs-lookup"><span data-stu-id="33b3d-140">New providers are required to work on .NET Core</span></span>
- <span data-ttu-id="33b3d-141">No se habilitará la compatibilidad espacial con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="33b3d-141">Spatial support with SQL Server won't be enabled</span></span>

## <a name="postponed-features"></a><span data-ttu-id="33b3d-142">Características pospuestas</span><span class="sxs-lookup"><span data-stu-id="33b3d-142">Postponed features</span></span>

<span data-ttu-id="33b3d-143">Algunas características planeadas originalmente para EF Core 3.0 se pospusieron para versiones futuras:</span><span class="sxs-lookup"><span data-stu-id="33b3d-143">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span> 

- <span data-ttu-id="33b3d-144">Capacidad de omitir partes de un modelo en migraciones, con seguimiento realizado a través del problema [n.° 2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span><span class="sxs-lookup"><span data-stu-id="33b3d-144">Ability to ingore parts of a model in migrations, tracked by [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="33b3d-145">Entidades contenedoras de propiedades, de las que se realiza un seguimiento a través de dos problemas independientes: [n.° 9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) sobre las entidades de tipo compartido y [n.° 13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) sobre la compatibilidad con la asignación de propiedades indizadas.</span><span class="sxs-lookup"><span data-stu-id="33b3d-145">Property bag entities, tracked by two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
