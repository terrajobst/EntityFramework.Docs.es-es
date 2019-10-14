---
title: 'Operadores de consulta complejos: EF Core'
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 350a7fa6a3ee1de16bad4b63e10842f9356a1b60
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72186237"
---
# <a name="complex-query-operators"></a><span data-ttu-id="6f050-102">Operadores de consulta complejos</span><span class="sxs-lookup"><span data-stu-id="6f050-102">Complex Query Operators</span></span>

<span data-ttu-id="6f050-103">Language Integrated Query (LINQ) contiene muchos operadores complejos, que combinan varios orígenes de datos o realizan procesamientos complejos.</span><span class="sxs-lookup"><span data-stu-id="6f050-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="6f050-104">No todos los operadores de LINQ tienen traducciones adecuadas en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="6f050-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="6f050-105">En ocasiones, una consulta en un formato se traduce en el servidor, pero si se escribe en otro formato, no se traduce aunque el resultado sea el mismo.</span><span class="sxs-lookup"><span data-stu-id="6f050-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="6f050-106">En esta página se describen algunos de los operadores complejos y sus variaciones admitidas.</span><span class="sxs-lookup"><span data-stu-id="6f050-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="6f050-107">En futuras versiones, es posible que se reconozcan más patrones y se agreguen sus correspondientes traducciones.</span><span class="sxs-lookup"><span data-stu-id="6f050-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="6f050-108">También es importante tener en cuenta que la compatibilidad con la traducción varía entre proveedores.</span><span class="sxs-lookup"><span data-stu-id="6f050-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="6f050-109">Es posible que una consulta determinada, que se traduzca en SqlServer, no funcione para bases de datos SQLite.</span><span class="sxs-lookup"><span data-stu-id="6f050-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="6f050-110">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="6f050-110">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="6f050-111">Join</span><span class="sxs-lookup"><span data-stu-id="6f050-111">Join</span></span>

<span data-ttu-id="6f050-112">El operador Join de LINQ permite conectar dos orígenes de datos en función del selector de claves de cada origen, lo que genera una tupla de valores cuando la clave coincide.</span><span class="sxs-lookup"><span data-stu-id="6f050-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="6f050-113">Se traduce de forma natural a `INNER JOIN` en las bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="6f050-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="6f050-114">Aunque Join de LINQ tiene selectores de clave externa e interna, la base de datos requiere una única condición de combinación.</span><span class="sxs-lookup"><span data-stu-id="6f050-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="6f050-115">Por tanto, EF Core genera una condición de combinación que compara el selector de clave externa con el selector de clave interna para determinar si son iguales.</span><span class="sxs-lookup"><span data-stu-id="6f050-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="6f050-116">Además, si los selectores de clave son tipos anónimos, EF Core genera una condición de combinación para comparar los componentes de igualdad.</span><span class="sxs-lookup"><span data-stu-id="6f050-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="6f050-117">GroupJoin</span><span class="sxs-lookup"><span data-stu-id="6f050-117">GroupJoin</span></span>

<span data-ttu-id="6f050-118">El operador GroupJoin de LINQ permite conectar dos orígenes de datos de forma similar a Join, pero crea un grupo de valores internos para los elementos externos coincidentes.</span><span class="sxs-lookup"><span data-stu-id="6f050-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="6f050-119">Al ejecutar una consulta como la del ejemplo siguiente se genera un resultado de `Blog` & `IEnumerable<Post>`.</span><span class="sxs-lookup"><span data-stu-id="6f050-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="6f050-120">Como las bases de datos (especialmente las relacionales) no tienen una manera de representar una colección de objetos del lado cliente, en muchos casos GroupJoin no se traduce en el servidor.</span><span class="sxs-lookup"><span data-stu-id="6f050-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="6f050-121">Requiere que se obtengan todos los datos del servidor para ejecutar GroupJoin sin un selector especial (la primera de las consultas siguientes).</span><span class="sxs-lookup"><span data-stu-id="6f050-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="6f050-122">Pero si el selector limita los datos que se seleccionan, la captura de todos los datos del servidor puede causar problemas de rendimiento (la segunda de las consultas siguientes).</span><span class="sxs-lookup"><span data-stu-id="6f050-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="6f050-123">Por eso EF Core no traduce GroupJoin.</span><span class="sxs-lookup"><span data-stu-id="6f050-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="6f050-124">SelectMany</span><span class="sxs-lookup"><span data-stu-id="6f050-124">SelectMany</span></span>

<span data-ttu-id="6f050-125">El operador SelectMany de LINQ permite enumerar un selector de colecciones para cada elemento externo y generar tuplas de valores de cada origen de datos.</span><span class="sxs-lookup"><span data-stu-id="6f050-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="6f050-126">En cierto modo es una combinación, pero sin ninguna condición, por lo que todos los elementos externos se conectan con un elemento del origen de la colección.</span><span class="sxs-lookup"><span data-stu-id="6f050-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="6f050-127">En función de cómo se relacione el selector de colecciones con el origen de datos externo, SelectMany puede traducirse en varias consultas diferentes en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="6f050-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="6f050-128">El selector de colecciones no hace referencia al elemento externo</span><span class="sxs-lookup"><span data-stu-id="6f050-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="6f050-129">Cuando el selector de colecciones no hace referencia a nada del origen externo, el resultado es un producto cartesiano de ambos orígenes de datos.</span><span class="sxs-lookup"><span data-stu-id="6f050-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="6f050-130">Se traduce a `CROSS JOIN` en las bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="6f050-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="6f050-131">El selector de colecciones hace referencia al elemento externo en una cláusula WHERE</span><span class="sxs-lookup"><span data-stu-id="6f050-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="6f050-132">Cuando el selector de colecciones tiene una cláusula WHERE, que hace referencia al elemento exterior, EF Core lo traduce a una combinación de base de datos y usa el predicado como condición de combinación.</span><span class="sxs-lookup"><span data-stu-id="6f050-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="6f050-133">Normalmente, este caso se produce cuando se usa la navegación de colección en el elemento exterior como selector de colecciones.</span><span class="sxs-lookup"><span data-stu-id="6f050-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="6f050-134">Si la colección está vacía para un elemento externo, no se generarán resultados para ese elemento externo.</span><span class="sxs-lookup"><span data-stu-id="6f050-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="6f050-135">Pero si se aplica `DefaultIfEmpty` en el selector de colecciones, el elemento exterior se conectará con un valor predeterminado del elemento interno.</span><span class="sxs-lookup"><span data-stu-id="6f050-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="6f050-136">Debido a esta distinción, este tipo de consultas se traduce a `INNER JOIN` en ausencia de `DefaultIfEmpty` y `LEFT JOIN` cuando se aplica `DefaultIfEmpty`.</span><span class="sxs-lookup"><span data-stu-id="6f050-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="6f050-137">El selector de colecciones hace referencia al elemento externo en una cláusula distinta de WHERE</span><span class="sxs-lookup"><span data-stu-id="6f050-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="6f050-138">Cuando el selector de colecciones hace referencia al elemento exterior, que no está en una cláusula WHERE (como en el caso anterior), no se traduce a una combinación de base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f050-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="6f050-139">Por este motivo es necesario evaluar el selector de colecciones para cada elemento exterior.</span><span class="sxs-lookup"><span data-stu-id="6f050-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="6f050-140">Se traduce a operaciones `APPLY` en muchas bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="6f050-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="6f050-141">Si la colección está vacía para un elemento externo, no se generarán resultados para ese elemento externo.</span><span class="sxs-lookup"><span data-stu-id="6f050-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="6f050-142">Pero si se aplica `DefaultIfEmpty` en el selector de colecciones, el elemento exterior se conectará con un valor predeterminado del elemento interno.</span><span class="sxs-lookup"><span data-stu-id="6f050-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="6f050-143">Debido a esta distinción, este tipo de consultas se traduce a `CROSS APPLY` en ausencia de `DefaultIfEmpty` y `OUTER APPLY` cuando se aplica `DefaultIfEmpty`.</span><span class="sxs-lookup"><span data-stu-id="6f050-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="6f050-144">Algunas bases de datos como SQLite no admiten los operadores `APPLY`, por lo que este tipo de consulta no se puede traducir.</span><span class="sxs-lookup"><span data-stu-id="6f050-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="6f050-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="6f050-145">GroupBy</span></span>

<span data-ttu-id="6f050-146">Los operadores GroupBy de LINQ crean un resultado de tipo `IGrouping<TKey, TElement>`, donde `TKey` y `TElement` podrían ser cualquier tipo arbitrario.</span><span class="sxs-lookup"><span data-stu-id="6f050-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="6f050-147">Además, `IGrouping` implementa `IEnumerable<TElement>`, lo que significa que se puede redactar sobre este elemento con cualquier operador de LINQ después de la agrupación.</span><span class="sxs-lookup"><span data-stu-id="6f050-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="6f050-148">Como ninguna estructura de base de datos puede representar una instancia de `IGrouping`, en la mayoría de los casos los operadores GroupBy no tienen ninguna traducción.</span><span class="sxs-lookup"><span data-stu-id="6f050-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="6f050-149">Cuando se aplica un operador de agregado a cada grupo, lo que devuelve un valor escalar, se puede traducir a `GROUP BY` de SQL en las bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="6f050-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="6f050-150">`GROUP BY` de SQL también es restrictivo.</span><span class="sxs-lookup"><span data-stu-id="6f050-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="6f050-151">Requiere que se agrupe solo por valores escalares.</span><span class="sxs-lookup"><span data-stu-id="6f050-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="6f050-152">La proyección solo puede contener columnas de clave de agrupación o cualquier agregado aplicado en una columna.</span><span class="sxs-lookup"><span data-stu-id="6f050-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="6f050-153">EF Core identifica este patrón y lo traduce al servidor, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6f050-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="6f050-154">EF Core también traduce las consultas en las que un operador de agregado en la agrupación aparece en un operador Where o OrderBy (u otro orden) de LINQ.</span><span class="sxs-lookup"><span data-stu-id="6f050-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="6f050-155">Usa la cláusula `HAVING` en SQL para la cláusula WHERE.</span><span class="sxs-lookup"><span data-stu-id="6f050-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="6f050-156">La parte de la consulta antes de aplicar el operador GroupBy puede ser cualquier consulta compleja, siempre que se pueda traducir al servidor.</span><span class="sxs-lookup"><span data-stu-id="6f050-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="6f050-157">Además, una vez que se aplican operadores de agregado en una consulta de agrupación para quitar agrupaciones del origen resultante, se puede redactar sobre ella como cualquier otra consulta.</span><span class="sxs-lookup"><span data-stu-id="6f050-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="6f050-158">Los operadores de agregado que admite EF Core son los siguientes</span><span class="sxs-lookup"><span data-stu-id="6f050-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="6f050-159">Average</span><span class="sxs-lookup"><span data-stu-id="6f050-159">Average</span></span>
- <span data-ttu-id="6f050-160">Recuento</span><span class="sxs-lookup"><span data-stu-id="6f050-160">Count</span></span>
- <span data-ttu-id="6f050-161">LongCount</span><span class="sxs-lookup"><span data-stu-id="6f050-161">LongCount</span></span>
- <span data-ttu-id="6f050-162">Máx.</span><span class="sxs-lookup"><span data-stu-id="6f050-162">Max</span></span>
- <span data-ttu-id="6f050-163">Mín.</span><span class="sxs-lookup"><span data-stu-id="6f050-163">Min</span></span>
- <span data-ttu-id="6f050-164">Sum</span><span class="sxs-lookup"><span data-stu-id="6f050-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="6f050-165">Left Join</span><span class="sxs-lookup"><span data-stu-id="6f050-165">Left Join</span></span>

<span data-ttu-id="6f050-166">Aunque Left Join no es un operador de LINQ, las bases de datos relacionales tienen el concepto de combinación izquierda que se usa con frecuencia en las consultas.</span><span class="sxs-lookup"><span data-stu-id="6f050-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="6f050-167">Un patrón determinado en las consultas LINQ proporciona el mismo resultado que `LEFT JOIN` en el servidor.</span><span class="sxs-lookup"><span data-stu-id="6f050-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="6f050-168">EF Core identifica estos patrones y genera la operación `LEFT JOIN` equivalente en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="6f050-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="6f050-169">El patrón implica la creación de GroupJoin entre los dos orígenes de datos y, después, la reducción de la agrupación mediante el operador SelectMany con DefaultIfEmpty en el origen de agrupación para que coincida con NULL cuando el elemento interior no tiene un elemento relacionado.</span><span class="sxs-lookup"><span data-stu-id="6f050-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="6f050-170">En el ejemplo siguiente se muestra el aspecto de este patrón y lo que genera.</span><span class="sxs-lookup"><span data-stu-id="6f050-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="6f050-171">En el patrón anterior se crea una estructura compleja en el árbol de expresión.</span><span class="sxs-lookup"><span data-stu-id="6f050-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="6f050-172">Por eso, EF Core requiere que se reduzcan los resultados de agrupación del operador GroupJoin en un paso inmediatamente después del operador.</span><span class="sxs-lookup"><span data-stu-id="6f050-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="6f050-173">Aunque se use GroupJoin-DefaultIfEmpty-SelectMany, pero en otro patrón, es posible que no se identifique como una combinación izquierda.</span><span class="sxs-lookup"><span data-stu-id="6f050-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
