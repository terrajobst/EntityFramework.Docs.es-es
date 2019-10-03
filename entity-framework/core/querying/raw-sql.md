---
title: 'Consultas SQL sin formato: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: d8f52edfdf4bd7776ab8d81185c867cbfd7bcf44
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813599"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="eae50-102">Consultas SQL sin formato</span><span class="sxs-lookup"><span data-stu-id="eae50-102">Raw SQL Queries</span></span>

<span data-ttu-id="eae50-103">Entity Framework Core le permite descender hasta las consultas SQL sin formato cuando trabaja con una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="eae50-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="eae50-104">Esto puede resultar útil si la consulta que quiere hacer no se puede expresar con LINQ o si usar una consulta LINQ hará que se envíe una consulta SQL ineficaz.</span><span class="sxs-lookup"><span data-stu-id="eae50-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="eae50-105">Las consultas SQL sin formato pueden devolver tipos de entidad normales o [tipos de entidad sin clave](xref:core/modeling/keyless-entity-types) que forman parte del modelo.</span><span class="sxs-lookup"><span data-stu-id="eae50-105">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="eae50-106">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="eae50-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="eae50-107">Consultas SQL básicas sin formato</span><span class="sxs-lookup"><span data-stu-id="eae50-107">Basic raw SQL queries</span></span>

<span data-ttu-id="eae50-108">Puede usar el método de extensión `FromSqlRaw` para empezar una consulta LINQ basada en una consulta SQL sin formato.</span><span class="sxs-lookup"><span data-stu-id="eae50-108">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="eae50-109">Las consultas SQL sin formato se pueden usar para ejecutar un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="eae50-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="eae50-110">Pasar parámetros</span><span class="sxs-lookup"><span data-stu-id="eae50-110">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="eae50-111">**Use siempre la parametrización para las consultas SQL sin formato**</span><span class="sxs-lookup"><span data-stu-id="eae50-111">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="eae50-112">Al indicar cualquier valor proporcionado por el usuario en una consulta SQL sin formato, debe tener cuidado para evitar ataques por inyección de código SQL.</span><span class="sxs-lookup"><span data-stu-id="eae50-112">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="eae50-113">Además de validar que dichos valores no contienen caracteres no válidos, use siempre la parametrización que envía los valores separados del texto SQL.</span><span class="sxs-lookup"><span data-stu-id="eae50-113">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="eae50-114">En concreto, no pase nunca una cadena concatenada o interpolada (`$""`) con valores proporcionados por el usuario sin validar en `FromSqlRaw` o `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="eae50-114">In particular, never pass a concatenated or interpolated string (`$""`) with unvalidated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="eae50-115">Los métodos `FromSqlInterpolated` y `ExecuteSqlInterpolated` permiten usar la sintaxis de interpolación de cadenas de manera que se protege frente a los ataques por inyección de código SQL.</span><span class="sxs-lookup"><span data-stu-id="eae50-115">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="eae50-116">En el ejemplo siguiente se pasa un parámetro único a un procedimiento almacenado; para ello, se incluye un marcador de posición de parámetro en la cadena de consulta SQL y se proporciona un argumento adicional.</span><span class="sxs-lookup"><span data-stu-id="eae50-116">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="eae50-117">Si bien esto se puede ver como la sintaxis `String.Format`, el valor suministrado se encapsula en un `DbParameter` y el nombre del parámetro generado se inserta donde se ha especificado el marcador de posición `{0}`.</span><span class="sxs-lookup"><span data-stu-id="eae50-117">While this may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="eae50-118">Como alternativa a `FromSqlRaw`, puede usar `FromSqlInterpolated`, que permite el uso seguro de la interpolación de cadenas.</span><span class="sxs-lookup"><span data-stu-id="eae50-118">As an alternative to `FromSqlRaw`, you can use `FromSqlInterpolated` which allows the safe use of string interpolation.</span></span> <span data-ttu-id="eae50-119">Como en el ejemplo anterior, el valor se convierte en `DbParameter` y, por lo tanto, no es vulnerable a la inyección de código SQL:</span><span class="sxs-lookup"><span data-stu-id="eae50-119">As with the previous example, the value is converted to a `DbParameter` and is therefore not vulnerable to SQL injection:</span></span>

> [!NOTE]
> <span data-ttu-id="eae50-120">Antes de la versión 3.0, `FromSqlRaw` y `FromSqlInterpolated` eran dos sobrecargas denominadas `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="eae50-120">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="eae50-121">Consulte la [sección de versiones anteriores](#previous-versions) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="eae50-121">See the [previous versions section](#previous-versions) for more details.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="eae50-122">También puede construir un elemento DbParameter y suministrarlo como un valor de parámetro.</span><span class="sxs-lookup"><span data-stu-id="eae50-122">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="eae50-123">Dado que se usa un marcador de posición de parámetro SQL normal, en lugar de un marcador de posición de cadena, `FromSqlRaw` se puede usar de forma segura:</span><span class="sxs-lookup"><span data-stu-id="eae50-123">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="eae50-124">Esto permite usar los parámetros con nombre en la cadena de consulta SQL, lo que resulta útil cuando un procedimiento almacenado tiene parámetros opcionales:</span><span class="sxs-lookup"><span data-stu-id="eae50-124">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="eae50-125">Redacción con LINQ</span><span class="sxs-lookup"><span data-stu-id="eae50-125">Composing with LINQ</span></span>

<span data-ttu-id="eae50-126">Si la consulta SQL admite redacción en la base de datos, puede redactarla sobre la consulta SQL sin formato inicial mediante operadores LINQ.</span><span class="sxs-lookup"><span data-stu-id="eae50-126">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="eae50-127">Las consultas SQL que se pueden redactar empiezan con la palabra clave `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="eae50-127">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="eae50-128">En el ejemplo siguiente, se usa una consulta SQL sin formato que se selecciona de una función con valores de tabla (TVF) y luego se redacta en ella con LINQ para realizar filtrado y ordenación.</span><span class="sxs-lookup"><span data-stu-id="eae50-128">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

<span data-ttu-id="eae50-129">Se generará la siguiente consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="eae50-129">This will produce the following SQL query:</span></span>

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a><span data-ttu-id="eae50-130">Seguimiento de cambios</span><span class="sxs-lookup"><span data-stu-id="eae50-130">Change Tracking</span></span>

<span data-ttu-id="eae50-131">Las consultas que usan los métodos `FromSql` siguen las mismas reglas de seguimiento de cambios que las demás consultas LINQ en EF Core.</span><span class="sxs-lookup"><span data-stu-id="eae50-131">Queries that use the `FromSql` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="eae50-132">Por ejemplo, si la consulta proyecta tipos de entidad, se realizará un seguimiento de los resultados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="eae50-132">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="eae50-133">En el ejemplo siguiente se usa una consulta SQL sin formato que realiza una selección en una función con valores de tabla (TVF) y después deshabilita el seguimiento de cambios con la llamada a `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="eae50-133">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="eae50-134">Inclusión de datos relacionados</span><span class="sxs-lookup"><span data-stu-id="eae50-134">Including related data</span></span>

<span data-ttu-id="eae50-135">El método `Include` puede usarse para incluir datos relacionados, igual que cualquier otra consulta LINQ:</span><span class="sxs-lookup"><span data-stu-id="eae50-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

<span data-ttu-id="eae50-136">Tenga en cuenta que esto requiere que la consulta SQL sin formato admita composición; en particular, no funcionará con llamadas a procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="eae50-136">Note that this requires your raw SQL query to be composable; it will notably not work with stored procedure calls.</span></span> <span data-ttu-id="eae50-137">Vea las notas sobre la admisión de la composición en [Limitaciones](#limitations).</span><span class="sxs-lookup"><span data-stu-id="eae50-137">See notes on composability under [Limitations](#limitations)).</span></span>

## <a name="limitations"></a><span data-ttu-id="eae50-138">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="eae50-138">Limitations</span></span>

<span data-ttu-id="eae50-139">Existen algunas limitaciones que debe considerar al usar las consultas SQL sin formato:</span><span class="sxs-lookup"><span data-stu-id="eae50-139">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="eae50-140">La consulta SQL debe devolver datos para todas las propiedades del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="eae50-140">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="eae50-141">Los nombres de las columnas del conjunto de resultados deben coincidir con los nombres de las columnas a los que se asignan las propiedades.</span><span class="sxs-lookup"><span data-stu-id="eae50-141">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="eae50-142">Observe que esto es diferente de lo que ocurre en EF6, donde las consultas SQL omitían la asignación de propiedades y columnas, y los nombres de las columnas del conjunto de resultados tenían que coincidir con los nombres de las propiedades.</span><span class="sxs-lookup"><span data-stu-id="eae50-142">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="eae50-143">La consulta SQL no puede incluir datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="eae50-143">The SQL query cannot contain related data.</span></span> <span data-ttu-id="eae50-144">Sin embargo, en muchos casos puede redactar sobre la consulta si usa el operador `Include` para devolver datos relacionados (consulte [Inclusión de datos relacionados](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="eae50-144">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="eae50-145">Las instrucciones `SELECT` que se pasan a este método por lo general admiten composición: Si EF Core debe evaluar operadores de consulta adicionales en el servidor (por ejemplo, para trasladar los operadores LINQ aplicados después de los métodos `FromSql`), el código SQL suministrado se considerará una subconsulta.</span><span class="sxs-lookup"><span data-stu-id="eae50-145">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql` methods), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="eae50-146">Esto significa que el código SQL que se pasa no contendrá ningún carácter ni opción que no sea válido en una subconsulta, como:</span><span class="sxs-lookup"><span data-stu-id="eae50-146">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="eae50-147">Un punto y coma final</span><span class="sxs-lookup"><span data-stu-id="eae50-147">A trailing semicolon</span></span>
  * <span data-ttu-id="eae50-148">En SQL Server, una sugerencia en el nivel de consulta final (por ejemplo, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="eae50-148">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="eae50-149">En SQL Server, cláusula `ORDER BY` que no va acompañada por `OFFSET 0` O BIEN `TOP 100 PERCENT` en la cláusula `SELECT`</span><span class="sxs-lookup"><span data-stu-id="eae50-149">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="eae50-150">Tenga en cuenta que SQL Server no permite la composición en llamadas a procedimientos almacenados, por lo que cualquier intento de aplicar operadores de consulta adicionales a dicha llamada producirá un código SQL no válido.</span><span class="sxs-lookup"><span data-stu-id="eae50-150">Note that SQL Server does not allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="eae50-151">Los operadores de consulta se pueden indicar después de `AsEnumerable()` para la evaluación del cliente.</span><span class="sxs-lookup"><span data-stu-id="eae50-151">Query operators may be introduced after `AsEnumerable()` for client evaluation.</span></span>

## <a name="previous-versions"></a><span data-ttu-id="eae50-152">Versiones anteriores</span><span class="sxs-lookup"><span data-stu-id="eae50-152">Previous versions</span></span>

<span data-ttu-id="eae50-153">EF Core 2.2 y las versiones anteriores tenían dos sobrecargas denominadas `FromSql`, que se comportaban de la misma manera que las sobrecargas `FromSqlRaw` y `FromSqlInterpolated` nuevas.</span><span class="sxs-lookup"><span data-stu-id="eae50-153">EF Core version 2.2 and earlier had two overloads named `FromSql` which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="eae50-154">Esto facilitaba las llamadas accidentales al método de cadena sin formato cuando la intención era llamar al método de cadena interpolada y viceversa.</span><span class="sxs-lookup"><span data-stu-id="eae50-154">This made it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="eae50-155">Esto podría resultar en consultas que no se parametrizan cuando deberían.</span><span class="sxs-lookup"><span data-stu-id="eae50-155">This could result in queries not being parameterized when they should have been.</span></span>
