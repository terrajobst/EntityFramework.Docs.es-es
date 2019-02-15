---
title: 'Consultas de seguimiento frente a consultas de no seguimiento: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 6c5d516fcb3950ae168860029660e1b1061546b8
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668783"
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="07409-102">Consultas de seguimiento frente a consultas de no seguimiento</span><span class="sxs-lookup"><span data-stu-id="07409-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="07409-103">El comportamiento de seguimiento controla si Entity Framework Core guardará o no información sobre una instancia de entidad en la herramienta de seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="07409-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="07409-104">Si se hace seguimiento de una entidad, cualquier cambio detectado en ella persistirá hasta la base de datos durante `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="07409-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="07409-105">Entity Framework Core también corregirá las propiedades de navegación entre las entidades que se obtienen de una consulta de seguimiento y las entidades que se cargaron previamente en la instancia de DbContext.</span><span class="sxs-lookup"><span data-stu-id="07409-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="07409-106">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="07409-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="07409-107">Consultas de seguimiento</span><span class="sxs-lookup"><span data-stu-id="07409-107">Tracking queries</span></span>

<span data-ttu-id="07409-108">De manera predeterminada, las consultas que devuelven tipos de entidad son consultas de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="07409-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="07409-109">Esto significa que puede hacer cambios en esas instancias de entidad y que esos cambios persisten por `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="07409-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="07409-110">En el ejemplo siguiente, se detectará el cambio en la clasificación de los blogs y persistirá hasta la base de datos durante `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="07409-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="07409-111">Consultas de no seguimiento</span><span class="sxs-lookup"><span data-stu-id="07409-111">No-tracking queries</span></span>

<span data-ttu-id="07409-112">Las consultas de no seguimiento son útiles cuando los resultados se usan en un escenario de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="07409-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="07409-113">Su ejecución es más rápida porque no es necesario configurar información de seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="07409-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="07409-114">Puede cambiar una consulta individual para que sea una consulta de no seguimiento:</span><span class="sxs-lookup"><span data-stu-id="07409-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="07409-115">También puede cambiar el comportamiento de seguimiento predeterminado en el nivel de instancia de contexto:</span><span class="sxs-lookup"><span data-stu-id="07409-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="07409-116">Las consultas de no seguimiento siguen realizando la resolución de identidad dentro de la consulta en ejecución.</span><span class="sxs-lookup"><span data-stu-id="07409-116">No tracking queries still perform identity resolution within the executing query.</span></span> <span data-ttu-id="07409-117">Si el conjunto de resultados contiene varias veces la misma entidad, se devolverá la misma instancia de la clase de entidad para cada repetición en el conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="07409-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="07409-118">Sin embargo, se usan referencias parciales para llevar un seguimiento de las entidades que ya se devolvieron.</span><span class="sxs-lookup"><span data-stu-id="07409-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="07409-119">Si un resultado anterior con la misma identidad queda fuera del ámbito y se ejecuta la recolección de elementos no utilizados, puede obtener una instancia de entidad nueva.</span><span class="sxs-lookup"><span data-stu-id="07409-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="07409-120">Para más información, consulte el artículo sobre el [funcionamiento de una consulta ](overview.md).</span><span class="sxs-lookup"><span data-stu-id="07409-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="07409-121">Seguimientos y proyecciones</span><span class="sxs-lookup"><span data-stu-id="07409-121">Tracking and projections</span></span>

<span data-ttu-id="07409-122">Incluso si el tipo de resultado de la consulta no es un tipo de entidad, si el resultado contiene tipos de entidad, de todos modos se hará seguimiento de estos de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="07409-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="07409-123">En la consulta siguiente, que devuelve un tipo anónimo, se hará seguimiento de las instancias de `Blog` en el conjunto de resultados.</span><span class="sxs-lookup"><span data-stu-id="07409-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="07409-124">Si el conjunto de resultados no contiene ningún tipo de entidad, no se realiza ningún seguimiento.</span><span class="sxs-lookup"><span data-stu-id="07409-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="07409-125">En la consulta siguiente, que devuelve un tipo anónimo con algunos de los valores de la entidad (pero no instancias del tipo de entidad real), no se realiza ningún seguimiento.</span><span class="sxs-lookup"><span data-stu-id="07409-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
