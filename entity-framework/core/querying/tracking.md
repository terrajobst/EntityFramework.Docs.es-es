---
title: 'Consultas de seguimiento frente a consultas de no seguimiento: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: d93be5c2b727d8fbaddd103f8f367c699ae80a7c
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921655"
---
# <a name="tracking-vs-no-tracking-queries"></a>Consultas de seguimiento frente a consultas de no seguimiento

El comportamiento de seguimiento controla si Entity Framework Core guardará o no información sobre una instancia de entidad en la herramienta de seguimiento de cambios. Si se hace seguimiento de una entidad, cualquier cambio detectado en ella persistirá hasta la base de datos durante `SaveChanges()`. Entity Framework Core también corregirá las propiedades de navegación entre las entidades que se obtienen de una consulta de seguimiento y las entidades que se cargaron previamente en la instancia de DbContext.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.

## <a name="tracking-queries"></a>Consultas de seguimiento

De manera predeterminada, las consultas que devuelven tipos de entidad son consultas de seguimiento. Esto significa que puede hacer cambios en esas instancias de entidad y que esos cambios persisten por `SaveChanges()`.

En el ejemplo siguiente, se detectará el cambio en la clasificación de los blogs y persistirá hasta la base de datos durante `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Consultas de no seguimiento

Las consultas de no seguimiento son útiles cuando los resultados se usan en un escenario de solo lectura. Su ejecución es más rápida porque no es necesario configurar información de seguimiento de cambios.

Puede cambiar una consulta individual para que sea una consulta de no seguimiento:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

También puede cambiar el comportamiento de seguimiento predeterminado en el nivel de instancia de contexto:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Las consultas de no seguimiento siguen realizando la resolución de identidad dentro de la consulta en ejecución. Si el conjunto de resultados contiene varias veces la misma entidad, se devolverá la misma instancia de la clase de entidad para cada repetición en el conjunto de resultados. Sin embargo, se usan referencias parciales para llevar un seguimiento de las entidades que ya se devolvieron. Si un resultado anterior con la misma identidad queda fuera del ámbito y se ejecuta la recolección de elementos no utilizados, puede obtener una instancia de entidad nueva. Para más información, consulte el artículo sobre el [funcionamiento de una consulta ](overview.md).

## <a name="tracking-and-projections"></a>Seguimientos y proyecciones

Incluso si el tipo de resultado de la consulta no es un tipo de entidad, si el resultado contiene tipos de entidad, de todos modos se hará seguimiento de estos de manera predeterminada. En la consulta siguiente, que devuelve un tipo anónimo, se hará seguimiento de las instancias de `Blog` en el conjunto de resultados.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=7)] -->
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

Si el conjunto de resultados no contiene ningún tipo de entidad, no se realiza ningún seguimiento. En la consulta siguiente, que devuelve un tipo anónimo con algunos de los valores de la entidad (pero no instancias del tipo de entidad real), no se realiza ningún seguimiento.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
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
