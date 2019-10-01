---
title: 'Consultas SQL sin formato: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: ebec5775770c0f1e297eaaf35bf644c605a69afc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197771"
---
# <a name="raw-sql-queries"></a>Consultas SQL sin formato

Entity Framework Core le permite descender hasta las consultas SQL sin formato cuando trabaja con una base de datos relacional. Esto puede resultar útil si la consulta que quiere hacer no se puede expresar con LINQ o si usar una consulta LINQ hará que se envíe una consulta SQL ineficaz. Las consultas SQL sin formato pueden devolver tipos de entidad normales o [tipos de entidad sin clave](xref:core/modeling/keyless-entity-types) que forman parte del modelo.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) de este artículo en GitHub.

## <a name="basic-raw-sql-queries"></a>Consultas SQL básicas sin formato

Puede usar el método de extensión `FromSqlRaw` para empezar una consulta LINQ basada en una consulta SQL sin formato.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

Las consultas SQL sin formato se pueden usar para ejecutar un procedimiento almacenado.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Pasar parámetros

> [!WARNING]
> **Use siempre la parametrización para las consultas SQL sin formato**
>
> Al indicar cualquier valor proporcionado por el usuario en una consulta SQL sin formato, debe tener cuidado para evitar ataques por inyección de código SQL. Además de validar que dichos valores no contienen caracteres no válidos, use siempre la parametrización que envía los valores separados del texto SQL.
>
> En concreto, no pase nunca una cadena concatenada o interpolada (`$""`) con valores proporcionados por el usuario sin validar en `FromSqlRaw` o `ExecuteSqlRaw`. Los métodos `FromSqlInterpolated` y `ExecuteSqlInterpolated` permiten usar la sintaxis de interpolación de cadenas de manera que se protege frente a los ataques por inyección de código SQL.

En el ejemplo siguiente se pasa un parámetro único a un procedimiento almacenado; para ello, se incluye un marcador de posición de parámetro en la cadena de consulta SQL y se proporciona un argumento adicional. Si bien esto se puede ver como la sintaxis `String.Format`, el valor suministrado se encapsula en un `DbParameter` y el nombre del parámetro generado se inserta donde se ha especificado el marcador de posición `{0}`.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Como alternativa a `FromSqlRaw`, puede usar `FromSqlInterpolated`, que permite el uso seguro de la interpolación de cadenas. Como en el ejemplo anterior, el valor se convierte en `DbParameter` y, por lo tanto, no es vulnerable a la inyección de código SQL:

> [!NOTE]
> Antes de la versión 3.0, `FromSqlRaw` y `FromSqlInterpolated` eran dos sobrecargas denominadas `FromSql`. Consulte la [sección de versiones anteriores](#previous-versions) para obtener más detalles.


<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

También puede construir un elemento DbParameter y suministrarlo como un valor de parámetro. Dado que se usa un marcador de posición de parámetro SQL normal, en lugar de un marcador de posición de cadena, `FromSqlRaw` se puede usar de forma segura:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

Esto permite usar los parámetros con nombre en la cadena de consulta SQL, lo que resulta útil cuando un procedimiento almacenado tiene parámetros opcionales:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Redacción con LINQ

Si la consulta SQL admite redacción en la base de datos, puede redactarla sobre la consulta SQL sin formato inicial mediante operadores LINQ. Las consultas SQL que se pueden redactar empiezan con la palabra clave `SELECT`.

En el ejemplo siguiente, se usa una consulta SQL sin formato que se selecciona de una función con valores de tabla (TVF) y luego se redacta en ella con LINQ para realizar filtrado y ordenación.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

Se generará la siguiente consulta SQL:

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a>Seguimiento de cambios

Las consultas que usan los métodos `FromSql` siguen las mismas reglas de seguimiento de cambios que las demás consultas LINQ en EF Core. Por ejemplo, si la consulta proyecta tipos de entidad, se realizará un seguimiento de los resultados de forma predeterminada.

En el ejemplo siguiente se usa una consulta SQL sin formato que realiza una selección en una función con valores de tabla (TVF) y después deshabilita el seguimiento de cambios con la llamada a `AsNoTracking`:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Inclusión de datos relacionados

El método `Include` puede usarse para incluir datos relacionados, igual que cualquier otra consulta LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

Tenga en cuenta que esto requiere que la consulta SQL sin formato admita composición; en particular, no funcionará con llamadas a procedimientos almacenados. Vea las notas sobre la admisión de la composición en [Limitaciones](#limitations).

## <a name="limitations"></a>Limitaciones

Existen algunas limitaciones que debe considerar al usar las consultas SQL sin formato:

* La consulta SQL debe devolver datos para todas las propiedades del tipo de entidad.

* Los nombres de las columnas del conjunto de resultados deben coincidir con los nombres de las columnas a los que se asignan las propiedades. Observe que esto es diferente de lo que ocurre en EF6, donde las consultas SQL omitían la asignación de propiedades y columnas, y los nombres de las columnas del conjunto de resultados tenían que coincidir con los nombres de las propiedades.

* La consulta SQL no puede incluir datos relacionados. Sin embargo, en muchos casos puede redactar sobre la consulta si usa el operador `Include` para devolver datos relacionados (consulte [Inclusión de datos relacionados](#including-related-data)).

* Las instrucciones `SELECT` que se pasan a este método por lo general admiten composición: Si EF Core debe evaluar operadores de consulta adicionales en el servidor (por ejemplo, para trasladar los operadores LINQ aplicados después de los métodos `FromSql`), el código SQL suministrado se considerará una subconsulta. Esto significa que el código SQL que se pasa no contendrá ningún carácter ni opción que no sea válido en una subconsulta, como:
  * Un punto y coma final
  * En SQL Server, una sugerencia en el nivel de consulta final (por ejemplo, `OPTION (HASH JOIN)`)
  * En SQL Server, cláusula `ORDER BY` que no va acompañada por `OFFSET 0` O BIEN `TOP 100 PERCENT` en la cláusula `SELECT`

* Tenga en cuenta que SQL Server no permite la composición en llamadas a procedimientos almacenados, por lo que cualquier intento de aplicar operadores de consulta adicionales a dicha llamada producirá un código SQL no válido. Los operadores de consulta se pueden indicar después de `AsEnumerable()` para la evaluación del cliente.

# <a name="previous-versions"></a>Versiones anteriores

EF Core 2.2 y las versiones anteriores tenían dos sobrecargas denominadas `FromSql`, que se comportaban de la misma manera que las sobrecargas `FromSqlRaw` y `FromSqlInterpolated` nuevas. Esto facilitaba las llamadas accidentales al método de cadena sin formato cuando la intención era llamar al método de cadena interpolada y viceversa. Esto podría resultar en consultas que no se parametrizan cuando deberían.
