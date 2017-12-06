---
title: Consultas SQL sin formato - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: ddf3a841800684688d50dcf9323f4d83c851222f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="raw-sql-queries"></a><span data-ttu-id="18bf3-102">Consultas SQL sin formato</span><span class="sxs-lookup"><span data-stu-id="18bf3-102">Raw SQL Queries</span></span>

<span data-ttu-id="18bf3-103">Entity Framework Core permite desplegable a consultas SQL sin formato cuando se trabaja con una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="18bf3-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="18bf3-104">Esto puede ser útil si la consulta que desea realizar no se puede expresar utilizando LINQ, o si ineficaz SQL que se envían a la base de datos mediante una consulta LINQ es lo que permite.</span><span class="sxs-lookup"><span data-stu-id="18bf3-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="18bf3-105">Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="18bf3-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="18bf3-106">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="18bf3-106">Limitations</span></span>

<span data-ttu-id="18bf3-107">Hay un par de limitaciones a tener en cuenta al utilizar sin procesar consultas SQL:</span><span class="sxs-lookup"><span data-stu-id="18bf3-107">There are a couple of limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="18bf3-108">Las consultas SQL solo se utilizan para devolver tipos de entidades que forman parte del modelo.</span><span class="sxs-lookup"><span data-stu-id="18bf3-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="18bf3-109">Hay una mejora en nuestro trabajo pendiente a [permiten devolver tipos de ad-hoc desde consultas SQL sin formato](https://github.com/aspnet/EntityFramework/issues/1862).</span><span class="sxs-lookup"><span data-stu-id="18bf3-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="18bf3-110">La consulta SQL debe devolver datos para todas las propiedades del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="18bf3-110">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="18bf3-111">Los nombres de columna del conjunto de resultados deben coincidir con los nombres de columna que se asignan a las propiedades.</span><span class="sxs-lookup"><span data-stu-id="18bf3-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="18bf3-112">Tenga en cuenta que esto es diferente de EF6 donde se omitió la asignación de propiedad o columna para las consultas SQL sin formato y nombres tenían que coinciden con los nombres de propiedad de columna del conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="18bf3-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="18bf3-113">La consulta SQL no puede contener datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="18bf3-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="18bf3-114">Sin embargo, en muchos casos se puede componer encima de la consulta con la `Include` operador que se va a devolver datos relacionados (consulte [incluidos los datos relacionados](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="18bf3-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="18bf3-115">Consultas básicas de SQL sin formato</span><span class="sxs-lookup"><span data-stu-id="18bf3-115">Basic raw SQL queries</span></span>

<span data-ttu-id="18bf3-116">Puede usar el *FromSql* método de extensión para iniciar una consulta LINQ en función de una consulta SQL sin formato.</span><span class="sxs-lookup"><span data-stu-id="18bf3-116">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="18bf3-117">Sin procesar consultas SQL pueden utilizarse para ejecutar un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="18bf3-117">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="18bf3-118">Pasar parámetros</span><span class="sxs-lookup"><span data-stu-id="18bf3-118">Passing parameters</span></span>

<span data-ttu-id="18bf3-119">Al igual que con cualquier API que acepta SQL, es importante parametrizar cualquier entrada del usuario para protegerse contra un ataque de inyección de SQL.</span><span class="sxs-lookup"><span data-stu-id="18bf3-119">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="18bf3-120">Puede incluir marcadores de posición de parámetro en la cadena de consulta SQL y, a continuación, proporcionar valores de parámetros como argumentos adicionales.</span><span class="sxs-lookup"><span data-stu-id="18bf3-120">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="18bf3-121">Los valores de parámetro se proporciona automáticamente se convertirá en un `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="18bf3-121">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="18bf3-122">En el ejemplo siguiente se pasa un solo parámetro para un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="18bf3-122">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="18bf3-123">Aunque esto puede parecer como `String.Format` sintaxis, el valor proporcionado se ajusta en un parámetro y el nombre de parámetro generado insertan dónde el `{0}` se especificó el marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="18bf3-123">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="18bf3-124">Esta es la misma consulta, pero con la sintaxis de interpolación de cadena, que es compatible en EF Core 2.0 y versiones posteriores:</span><span class="sxs-lookup"><span data-stu-id="18bf3-124">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="18bf3-125">También puede construir un DbParameter y suministrarlo como un valor de parámetro.</span><span class="sxs-lookup"><span data-stu-id="18bf3-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="18bf3-126">Esto le permite usar parámetros con nombre en la cadena de consulta SQL</span><span class="sxs-lookup"><span data-stu-id="18bf3-126">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="18bf3-127">Componer con LINQ</span><span class="sxs-lookup"><span data-stu-id="18bf3-127">Composing with LINQ</span></span>

<span data-ttu-id="18bf3-128">Si la consulta SQL se puede formular en la base de datos, puede crear encima de la consulta SQL sin formato inicial mediante operadores de LINQ.</span><span class="sxs-lookup"><span data-stu-id="18bf3-128">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="18bf3-129">Las consultas SQL que se pueden formular en que se va a con la `SELECT` palabra clave.</span><span class="sxs-lookup"><span data-stu-id="18bf3-129">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="18bf3-130">En el ejemplo siguiente se utiliza una consulta SQL sin formato que selecciona de una función con valores de tabla (TVF) y, a continuación, se crea en el mismo uso de LINQ para realizar el filtrado y ordenación.</span><span class="sxs-lookup"><span data-stu-id="18bf3-130">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="18bf3-131">Incluidos los datos relacionados</span><span class="sxs-lookup"><span data-stu-id="18bf3-131">Including related data</span></span>

<span data-ttu-id="18bf3-132">Crear operadores de LINQ puede utilizarse para incluir datos relacionados en la consulta.</span><span class="sxs-lookup"><span data-stu-id="18bf3-132">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="18bf3-133">**Siempre utiliza la parametrización de consultas SQL sin formato:** API que aceptan una instancia de SQL sin formato de cadena como `FromSql` y `ExecuteSqlCommand` permiten valores fácilmente pasarse como parámetros.</span><span class="sxs-lookup"><span data-stu-id="18bf3-133">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="18bf3-134">Además de validar proporcionados por el usuario, utilice siempre la parametrización para los valores utilizados en un consulta o comando SQL sin formato.</span><span class="sxs-lookup"><span data-stu-id="18bf3-134">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="18bf3-135">Si está utilizando concatenación de cadenas para generar dinámicamente cualquier parte de la cadena de consulta, tú eres responsable de validar ninguna entrada para protegerse frente a ataques de inyección de SQL.</span><span class="sxs-lookup"><span data-stu-id="18bf3-135">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
