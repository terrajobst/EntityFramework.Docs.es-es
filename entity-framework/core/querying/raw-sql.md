---
title: Consultas SQL sin formato - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 29b7e20e875bf791a88a92636c1df4bc4e31656b
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2018
---
# <a name="raw-sql-queries"></a>Consultas SQL sin formato

Entity Framework Core permite desplegable a consultas SQL sin formato cuando se trabaja con una base de datos relacional. Esto puede ser útil si la consulta que desea realizar no se puede expresar utilizando LINQ, o si ineficaz SQL que se envían a la base de datos mediante una consulta LINQ es lo que permite.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.

## <a name="limitations"></a>Limitaciones

Existen algunas limitaciones a tener en cuenta cuando se usan sin procesar consultas SQL:
* Las consultas SQL solo se utilizan para devolver tipos de entidades que forman parte del modelo. Hay una mejora en nuestro trabajo pendiente a [permiten devolver tipos de ad-hoc desde consultas SQL sin formato](https://github.com/aspnet/EntityFramework/issues/1862).

* La consulta SQL debe devolver datos para todas las propiedades del tipo de entidad o consulta.

* Los nombres de columna del conjunto de resultados deben coincidir con los nombres de columna que se asignan a las propiedades. Tenga en cuenta que esto es diferente de EF6 donde se omitió la asignación de propiedad o columna para las consultas SQL sin formato y nombres tenían que coinciden con los nombres de propiedad de columna del conjunto de resultados.

* La consulta SQL no puede contener datos relacionados. Sin embargo, en muchos casos se puede componer encima de la consulta con la `Include` operador que se va a devolver datos relacionados (consulte [incluidos los datos relacionados](#including-related-data)).

* `SELECT` instrucciones pasadas a este método deben estar admite composición: Core EF si es necesario evaluar los operadores de consulta adicionales en el servidor (por ejemplo, para traducir los operadores LINQ que se aplican después de `FromSql`), SQL proporcionado se tratará como una subconsulta. Esto significa que la instrucción SQL que se pasó no debe contener los caracteres o las opciones que no son válidos en una subconsulta, como:
  * un punto y coma final
  * En SQL Server, un nivel de consulta finales sugerencia, p. ej. `OPTION (HASH JOIN)`
  * En SQL Server, un `ORDER BY` cláusula que no va acompañada de `TOP 100 PERCENT` en el `SELECT` cláusula

* Instrucciones SQL distinto `SELECT` se reconocen automáticamente como no admite composición. En consecuencia, los resultados completos de los procedimientos almacenados siempre se devuelven al cliente y los operadores LINQ se aplican después `FromSql` evaluada en memoria. 

## <a name="basic-raw-sql-queries"></a>Consultas básicas de SQL sin formato

Puede usar el *FromSql* método de extensión para iniciar una consulta LINQ en función de una consulta SQL sin formato.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Sin procesar consultas SQL pueden utilizarse para ejecutar un procedimiento almacenado.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Pasar parámetros

Al igual que con cualquier API que acepta SQL, es importante parametrizar cualquier entrada del usuario para protegerse contra un ataque de inyección de SQL. Puede incluir marcadores de posición de parámetro en la cadena de consulta SQL y, a continuación, proporcionar valores de parámetros como argumentos adicionales. Los valores de parámetro se proporciona automáticamente se convertirá en un `DbParameter`.

En el ejemplo siguiente se pasa un solo parámetro para un procedimiento almacenado. Aunque esto puede parecer como `String.Format` sintaxis, el valor proporcionado se ajusta en un parámetro y el nombre de parámetro generado insertan dónde el `{0}` se especificó el marcador de posición.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Esta es la misma consulta, pero con la sintaxis de interpolación de cadena, que es compatible en EF Core 2.0 y versiones posteriores:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

También puede construir un DbParameter y suministrarlo como un valor de parámetro. Esto le permite usar parámetros con nombre en la cadena de consulta SQL

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Componer con LINQ

Si la consulta SQL se puede formular en la base de datos, puede crear encima de la consulta SQL sin formato inicial mediante operadores de LINQ. Las consultas SQL que se pueden formular en que se va a con la `SELECT` palabra clave.

En el ejemplo siguiente se utiliza una consulta SQL sin formato que selecciona de una función con valores de tabla (TVF) y, a continuación, se crea en el mismo uso de LINQ para realizar el filtrado y ordenación.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>Incluidos los datos relacionados

Crear operadores de LINQ puede utilizarse para incluir datos relacionados en la consulta.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Siempre utiliza la parametrización de consultas SQL sin formato:** API que aceptan una instancia de SQL sin formato de cadena como `FromSql` y `ExecuteSqlCommand` permiten valores fácilmente pasarse como parámetros. Además de validar proporcionados por el usuario, utilice siempre la parametrización para los valores utilizados en un consulta o comando SQL sin formato. Si está utilizando concatenación de cadenas para generar dinámicamente cualquier parte de la cadena de consulta, tú eres responsable de validar ninguna entrada para protegerse frente a ataques de inyección de SQL.
