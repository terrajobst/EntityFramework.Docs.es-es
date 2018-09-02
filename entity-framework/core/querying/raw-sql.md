---
title: 'Consultas SQL sin formato: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 21cb688d6775039def3b0be12768da71b5d96531
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997149"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="a82a5-102">Consultas SQL sin formato</span><span class="sxs-lookup"><span data-stu-id="a82a5-102">Raw SQL Queries</span></span>

<span data-ttu-id="a82a5-103">Entity Framework Core le permite descender hasta las consultas SQL sin formato cuando trabaja con una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="a82a5-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="a82a5-104">Esto puede resultar útil si la consulta que desea hacer no se puede expresar con LINQ o si usar una consulta LINQ hará que se envíe código SQL ineficaz a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a82a5-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span> <span data-ttu-id="a82a5-105">Las consultas SQL sin formato pueden devolver tipos de entidad o, a partir de EF Core 2.1, [tipos de consulta](xref:core/modeling/query-types) que son parte del modelo.</span><span class="sxs-lookup"><span data-stu-id="a82a5-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="a82a5-106">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="a82a5-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="a82a5-107">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="a82a5-107">Limitations</span></span>

<span data-ttu-id="a82a5-108">Existen algunas limitaciones que debe considerar al usar las consultas SQL sin formato:</span><span class="sxs-lookup"><span data-stu-id="a82a5-108">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="a82a5-109">La consulta SQL debe devolver datos para todas las propiedades del tipo de entidad o de consulta.</span><span class="sxs-lookup"><span data-stu-id="a82a5-109">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="a82a5-110">Los nombres de las columnas del conjunto de resultados deben coincidir con los nombres de las columnas a los que se asignan las propiedades.</span><span class="sxs-lookup"><span data-stu-id="a82a5-110">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="a82a5-111">Observe que esto es diferente de lo que ocurre en EF6, donde las consultas SQL omitían la asignación de propiedades y columnas, y los nombres de las columnas del conjunto de resultados tenían que coincidir con los nombres de las propiedades.</span><span class="sxs-lookup"><span data-stu-id="a82a5-111">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="a82a5-112">La consulta SQL no puede incluir datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="a82a5-112">The SQL query cannot contain related data.</span></span> <span data-ttu-id="a82a5-113">Sin embargo, en muchos casos puede redactar sobre la consulta si usa el operador `Include` para devolver datos relacionados (consulte [Inclusión de datos relacionados](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="a82a5-113">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="a82a5-114">Las instrucciones `SELECT` que se pasan a este método generalmente admitirán composición. Si EF Core debe evaluar operadores de consulta adicionales en el servidor (por ejemplo, para trasladar los operadores LINQ aplicados después de `FromSql`), el código SQL suministrado se considerará como una subconsulta.</span><span class="sxs-lookup"><span data-stu-id="a82a5-114">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="a82a5-115">Esto significa que el código SQL que se pasa no contendrá ningún carácter ni opción que no sea válido en una subconsulta, como:</span><span class="sxs-lookup"><span data-stu-id="a82a5-115">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="a82a5-116">un punto y coma final</span><span class="sxs-lookup"><span data-stu-id="a82a5-116">a trailing semicolon</span></span>
  * <span data-ttu-id="a82a5-117">En SQL Server, una sugerencia en el nivel de consulta final (por ejemplo, `OPTION (HASH JOIN)`)</span><span class="sxs-lookup"><span data-stu-id="a82a5-117">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="a82a5-118">en SQL Server, una cláusula `ORDER BY` que no va acompañada de `TOP 100 PERCENT` en la cláusula `SELECT`</span><span class="sxs-lookup"><span data-stu-id="a82a5-118">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="a82a5-119">Las instrucciones SQL que no son `SELECT` se reconocen de manera automáticamente como instrucciones que no admiten composición.</span><span class="sxs-lookup"><span data-stu-id="a82a5-119">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="a82a5-120">Como consecuencia, los resultados completos de los procedimientos almacenados siempre se devuelven al cliente y cualquier operador LINQ aplicado después de `FromSql` se evalúa en la memoria.</span><span class="sxs-lookup"><span data-stu-id="a82a5-120">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="a82a5-121">Consultas SQL básicas sin formato</span><span class="sxs-lookup"><span data-stu-id="a82a5-121">Basic raw SQL queries</span></span>

<span data-ttu-id="a82a5-122">Puede usar el método de extensión *FromSql* para empezar una consulta LINQ basada en una consulta SQL sin formato.</span><span class="sxs-lookup"><span data-stu-id="a82a5-122">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="a82a5-123">Las consultas SQL sin formato se pueden usar para ejecutar un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="a82a5-123">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="a82a5-124">Pasar parámetros</span><span class="sxs-lookup"><span data-stu-id="a82a5-124">Passing parameters</span></span>

<span data-ttu-id="a82a5-125">Al igual que con cualquier API que acepta código SQL, es importante parametrizar cualquier entrada de usuario como protección contra un ataque por inyección de código SQL.</span><span class="sxs-lookup"><span data-stu-id="a82a5-125">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="a82a5-126">Puede incluir marcadores de posición de parámetro en la cadena de la consulta y luego suministrar valores de parámetro como argumentos adicionales.</span><span class="sxs-lookup"><span data-stu-id="a82a5-126">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="a82a5-127">Cualquier valor de parámetro que suministre se convertirá automáticamente en `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="a82a5-127">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="a82a5-128">En el ejemplo siguiente, se pasa un parámetro único a un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="a82a5-128">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="a82a5-129">Si bien esto se puede ver como la sintaxis `String.Format`, el valor suministrado se encapsula en un parámetro y el nombre del parámetro generado se inserta donde se especificó el marcador de posición `{0}`.</span><span class="sxs-lookup"><span data-stu-id="a82a5-129">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="a82a5-130">Se trata de la misma consulta, pero usando la sintaxis de interpolación de cadena, que se admite en EF Core 2.0 y versiones superiores:</span><span class="sxs-lookup"><span data-stu-id="a82a5-130">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="a82a5-131">También puede construir un elemento DbParameter y suministrarlo como un valor de parámetro.</span><span class="sxs-lookup"><span data-stu-id="a82a5-131">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="a82a5-132">Esto le permite usar parámetros con nombre en la cadena de consulta SQL</span><span class="sxs-lookup"><span data-stu-id="a82a5-132">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="a82a5-133">Redacción con LINQ</span><span class="sxs-lookup"><span data-stu-id="a82a5-133">Composing with LINQ</span></span>

<span data-ttu-id="a82a5-134">Si la consulta SQL admite redacción en la base de datos, puede redactarla sobre la consulta SQL sin formato inicial mediante operadores LINQ.</span><span class="sxs-lookup"><span data-stu-id="a82a5-134">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="a82a5-135">Las consultas SQL se pueden redactar con la palabra clave `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="a82a5-135">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="a82a5-136">En el ejemplo siguiente, se usa una consulta SQL sin formato que se selecciona de una función con valores de tabla (TVF) y luego se redacta en ella con LINQ para realizar filtrado y ordenación.</span><span class="sxs-lookup"><span data-stu-id="a82a5-136">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="a82a5-137">Inclusión de datos relacionados</span><span class="sxs-lookup"><span data-stu-id="a82a5-137">Including related data</span></span>

<span data-ttu-id="a82a5-138">Se puede usar la redacción con operadores LINQ para incluir datos relacionados en la consulta.</span><span class="sxs-lookup"><span data-stu-id="a82a5-138">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="a82a5-139">**Siempre use la parametrización para las consultas SQL sin formato:** las API que aceptan una cadena SQL sin formato como `FromSql` y `ExecuteSqlCommand` permiten que los valores se pasen de manera sencilla como parámetros.</span><span class="sxs-lookup"><span data-stu-id="a82a5-139">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="a82a5-140">Además de validar la entrada del usuario, siempre use la parametrización para cualquier valor usado en un comando o consulta SQL sin formato.</span><span class="sxs-lookup"><span data-stu-id="a82a5-140">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="a82a5-141">Si usa la concatenación de cadenas para compilar de manera dinámica cualquier parte de la cadena de consulta, es su responsabilidad validar cualquier entrada como protección contra los ataques por inyección de código SQL.</span><span class="sxs-lookup"><span data-stu-id="a82a5-141">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
