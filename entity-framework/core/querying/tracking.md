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
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="c6e02-102">Frente a seguimiento. Consultas de seguimiento no</span><span class="sxs-lookup"><span data-stu-id="c6e02-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="c6e02-103">Controles de comportamiento de seguimiento o no Entity Framework Core guardará la información acerca de una instancia de entidad en su herramienta de seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="c6e02-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="c6e02-104">Si se realiza el seguimiento de una entidad, se conservará cualquier cambio detectado en la entidad en la base de datos durante la `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="c6e02-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="c6e02-105">Propiedades de navegación también revisión de seguridad de entidad Framework Core will entre entidades que se obtienen de una consulta de seguimiento y las entidades que se cargaron previamente en la instancia de DbContext.</span><span class="sxs-lookup"><span data-stu-id="c6e02-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="c6e02-106">Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="c6e02-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="c6e02-107">Consultas de seguimiento</span><span class="sxs-lookup"><span data-stu-id="c6e02-107">Tracking queries</span></span>

<span data-ttu-id="c6e02-108">De forma predeterminada, las consultas que devuelven tipos de entidad están realizando el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c6e02-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="c6e02-109">Esto significa que puede realizar cambios a las instancias de entidad y han guardado los cambios por `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="c6e02-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="c6e02-110">En el ejemplo siguiente, se detectarán y guardado en la base de datos durante el cambio a la clasificación de blogs `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="c6e02-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="c6e02-111">Consultas de seguimiento no</span><span class="sxs-lookup"><span data-stu-id="c6e02-111">No-tracking queries</span></span>

<span data-ttu-id="c6e02-112">No hay consultas de seguimiento son útiles cuando se usan los resultados en un escenario de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="c6e02-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="c6e02-113">Son más rápidas ejecutar porque no hay ninguna necesidad de información de seguimiento de cambios de configuración.</span><span class="sxs-lookup"><span data-stu-id="c6e02-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="c6e02-114">Puede intercambiar una consulta individual a seguimiento de no ser:</span><span class="sxs-lookup"><span data-stu-id="c6e02-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="c6e02-115">También puede cambiar el comportamiento en el nivel de instancia de contexto de seguimiento predeterminado:</span><span class="sxs-lookup"><span data-stu-id="c6e02-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="c6e02-116">No hay consultas de seguimiento seguir realizan la resolución de identidad en la consulta de ejecutar.</span><span class="sxs-lookup"><span data-stu-id="c6e02-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="c6e02-117">Si el conjunto de resultados contiene la misma entidad varias veces, se devolverá la misma instancia de la clase de entidad para cada repetición del conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="c6e02-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="c6e02-118">Sin embargo, las referencias débiles sirven para realizar un seguimiento de las entidades que ya se han devuelto.</span><span class="sxs-lookup"><span data-stu-id="c6e02-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="c6e02-119">Si un resultado anterior con la misma identidad queda fuera del ámbito y se ejecuta la recolección de elementos no utilizados, obtendrá una nueva instancia de entidad.</span><span class="sxs-lookup"><span data-stu-id="c6e02-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="c6e02-120">Para obtener más información, consulte [funcionamiento de la consulta](overview.md).</span><span class="sxs-lookup"><span data-stu-id="c6e02-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="c6e02-121">Seguimiento y proyecciones</span><span class="sxs-lookup"><span data-stu-id="c6e02-121">Tracking and projections</span></span>

<span data-ttu-id="c6e02-122">Incluso si el tipo de resultado de la consulta no es un tipo de entidad, si el resultado contiene tipos de entidad sigue realizará el seguimiento de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c6e02-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="c6e02-123">En la siguiente consulta, que devuelve un tipo anónimo, las instancias de `Blog` en el resultado de conjunto se efectuará un seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c6e02-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="c6e02-124">Si el conjunto de resultados no contiene ningún tipo de entidad, no se realiza ningún seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c6e02-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="c6e02-125">En la siguiente consulta, que devuelve un tipo anónimo con algunos de los valores de la entidad (pero ninguna instancia del tipo de entidad real), no hay ningún seguimiento realizado.</span><span class="sxs-lookup"><span data-stu-id="c6e02-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
