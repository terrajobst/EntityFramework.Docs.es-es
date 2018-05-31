---
title: 'Evaluación de cliente frente a servidor: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
ms.technology: entity-framework-core
uid: core/querying/client-eval
ms.openlocfilehash: e1852b780041e9e92fb4d25129175346e3a601a3
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052655"
---
# <a name="client-vs-server-evaluation"></a>Evaluación de cliente frente a servidor

Entity Framework Core admite partes de la consulta que se evalúan en el cliente y partes de ella que se insertan en la base de datos. Determinar qué partes de la consulta se evaluarán en la base de datos depende del proveedor de base de datos.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.

## <a name="client-evaluation"></a>Evaluación de cliente

En el ejemplo siguiente, se usa un método auxiliar para estandarizar las direcciones URL para los blogs que se devuelven desde una base de datos de SQL Server. Como el proveedor de SQL Server no tiene información de cómo se implementa este método, no es posible traducirlo a código SQL. Todos los demás aspectos de la consulta se evalúan en la base de datos, pero es el cliente quien pasa la `URL` devuelta a través de este método.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="disabling-client-evaluation"></a>Deshabilitación de la evaluación de cliente

Si bien la evaluación de cliente puede resultar muy útil, en algunas instancias puede generar un rendimiento deficiente. Considere la consulta siguiente, en la que el método auxiliar ahora se usa en un filtro. Como no se puede realizar en la base de datos, todos los datos se incorporan a la memoria y, luego, el filtro se aplica en el cliente. En función de la cantidad de datos y de cuántos de esos datos se filtran, esta acción podría generar un rendimiento deficiente.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

De manera predeterminada, EF Core registrará una advertencia cuando se realice la evaluación de cliente. Consulte [Logging](../miscellaneous/logging.md) (Registro) para más información sobre cómo ver la salida de registro. Puede cambiar el comportamiento cuando se produzca la evaluación de cliente, ya sea para que se genere una excepción o para que no haga nada. Esto se hace al configurar las opciones del contexto, por lo general en `DbContext.OnConfiguring` o en `Startup.cs` si usa ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
