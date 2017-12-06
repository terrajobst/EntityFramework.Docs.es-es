---
title: "Cliente frente a. Evaluación del servidor - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
ms.technology: entity-framework-core
uid: core/querying/client-eval
ms.openlocfilehash: e1852b780041e9e92fb4d25129175346e3a601a3
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="client-vs-server-evaluation"></a>Cliente frente a. Evaluación de servidor

Entity Framework Core es compatible con partes de la consulta se evalúa en el cliente y partes del mismo que se va a insertar en la base de datos. Es responsabilidad del proveedor de base de datos para determinar qué partes de la consulta se evaluará en la base de datos.

> [!TIP]  
> Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) en GitHub.

## <a name="client-evaluation"></a>Evaluación de cliente

En el ejemplo siguiente se usa un método auxiliar para estandarizar las direcciones URL para los blogs que se devuelven desde una base de datos de SQL Server. Dado que el proveedor de SQL Server no tiene ninguna información sobre cómo se implementa este método, no es posible traducir en SQL. Se evalúan todos los demás aspectos de la consulta en la base de datos, pero pasar el valor devuelto `URL` a través de este método se realiza en el cliente.

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

## <a name="disabling-client-evaluation"></a>Deshabilitar la evaluación de cliente

Evaluación de cliente puede ser muy útil, en algunos casos puede causar un bajo rendimiento. Considere la siguiente consulta, donde ahora se utiliza el método auxiliar en un filtro. Dado que esto no se puede realizar en la base de datos, todos los datos se extraen en memoria y, a continuación, el filtro se aplica en el cliente. Según la cantidad de datos y la cantidad de datos que se filtra, esto podría causar un bajo rendimiento.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

De forma predeterminada, el núcleo de EF registrará una advertencia cuando se realiza la evaluación de cliente. Vea [registro](../miscellaneous/logging.md) para obtener más información acerca de cómo ver los resultados del registro. Puede cambiar el comportamiento cuando la evaluación de cliente se produce a producir o no hacer nada. Esto se realiza al configurar las opciones para el contexto - normalmente en `DbContext.OnConfiguring`, o bien en `Startup.cs` si utiliza ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
