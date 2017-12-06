---
title: Frente a seguimiento. Consultas de seguimiento no - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a>Frente a seguimiento. Consultas de seguimiento no

Controles de comportamiento de seguimiento o no Entity Framework Core guardará la información acerca de una instancia de entidad en su herramienta de seguimiento de cambios. Si se realiza el seguimiento de una entidad, se conservará cualquier cambio detectado en la entidad en la base de datos durante la `SaveChanges()`. Propiedades de navegación también revisión de seguridad de entidad Framework Core will entre entidades que se obtienen de una consulta de seguimiento y las entidades que se cargaron previamente en la instancia de DbContext.

> [!TIP]  
> Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) en GitHub.

## <a name="tracking-queries"></a>Consultas de seguimiento

De forma predeterminada, las consultas que devuelven tipos de entidad están realizando el seguimiento. Esto significa que puede realizar cambios a las instancias de entidad y han guardado los cambios por `SaveChanges()`.

En el ejemplo siguiente, se detectarán y guardado en la base de datos durante el cambio a la clasificación de blogs `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Consultas de seguimiento no

No hay consultas de seguimiento son útiles cuando se usan los resultados en un escenario de solo lectura. Son más rápidas ejecutar porque no hay ninguna necesidad de información de seguimiento de cambios de configuración.

Puede intercambiar una consulta individual a seguimiento de no ser:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

También puede cambiar el comportamiento en el nivel de instancia de contexto de seguimiento predeterminado:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> No hay consultas de seguimiento seguir realizan la resolución de identidad en la consulta de ejecutar. Si el conjunto de resultados contiene la misma entidad varias veces, se devolverá la misma instancia de la clase de entidad para cada repetición del conjunto de resultados. Sin embargo, las referencias débiles sirven para realizar un seguimiento de las entidades que ya se han devuelto. Si un resultado anterior con la misma identidad queda fuera del ámbito y se ejecuta la recolección de elementos no utilizados, obtendrá una nueva instancia de entidad. Para obtener más información, consulte [funcionamiento de la consulta](overview.md).

## <a name="tracking-and-projections"></a>Seguimiento y proyecciones

Incluso si el tipo de resultado de la consulta no es un tipo de entidad, si el resultado contiene tipos de entidad sigue realizará el seguimiento de forma predeterminada. En la siguiente consulta, que devuelve un tipo anónimo, las instancias de `Blog` en el resultado de conjunto se efectuará un seguimiento.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

Si el conjunto de resultados no contiene ningún tipo de entidad, no se realiza ningún seguimiento. En la siguiente consulta, que devuelve un tipo anónimo con algunos de los valores de la entidad (pero ninguna instancia del tipo de entidad real), no hay ningún seguimiento realizado.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
