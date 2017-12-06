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
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="f1270-102">Cliente frente a. Evaluación de servidor</span><span class="sxs-lookup"><span data-stu-id="f1270-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="f1270-103">Entity Framework Core es compatible con partes de la consulta se evalúa en el cliente y partes del mismo que se va a insertar en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f1270-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="f1270-104">Es responsabilidad del proveedor de base de datos para determinar qué partes de la consulta se evaluará en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f1270-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="f1270-105">Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="f1270-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="f1270-106">Evaluación de cliente</span><span class="sxs-lookup"><span data-stu-id="f1270-106">Client evaluation</span></span>

<span data-ttu-id="f1270-107">En el ejemplo siguiente se usa un método auxiliar para estandarizar las direcciones URL para los blogs que se devuelven desde una base de datos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f1270-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="f1270-108">Dado que el proveedor de SQL Server no tiene ninguna información sobre cómo se implementa este método, no es posible traducir en SQL.</span><span class="sxs-lookup"><span data-stu-id="f1270-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="f1270-109">Se evalúan todos los demás aspectos de la consulta en la base de datos, pero pasar el valor devuelto `URL` a través de este método se realiza en el cliente.</span><span class="sxs-lookup"><span data-stu-id="f1270-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="disabling-client-evaluation"></a><span data-ttu-id="f1270-110">Deshabilitar la evaluación de cliente</span><span class="sxs-lookup"><span data-stu-id="f1270-110">Disabling client evaluation</span></span>

<span data-ttu-id="f1270-111">Evaluación de cliente puede ser muy útil, en algunos casos puede causar un bajo rendimiento.</span><span class="sxs-lookup"><span data-stu-id="f1270-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="f1270-112">Considere la siguiente consulta, donde ahora se utiliza el método auxiliar en un filtro.</span><span class="sxs-lookup"><span data-stu-id="f1270-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="f1270-113">Dado que esto no se puede realizar en la base de datos, todos los datos se extraen en memoria y, a continuación, el filtro se aplica en el cliente.</span><span class="sxs-lookup"><span data-stu-id="f1270-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="f1270-114">Según la cantidad de datos y la cantidad de datos que se filtra, esto podría causar un bajo rendimiento.</span><span class="sxs-lookup"><span data-stu-id="f1270-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="f1270-115">De forma predeterminada, el núcleo de EF registrará una advertencia cuando se realiza la evaluación de cliente.</span><span class="sxs-lookup"><span data-stu-id="f1270-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="f1270-116">Vea [registro](../miscellaneous/logging.md) para obtener más información acerca de cómo ver los resultados del registro.</span><span class="sxs-lookup"><span data-stu-id="f1270-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="f1270-117">Puede cambiar el comportamiento cuando la evaluación de cliente se produce a producir o no hacer nada.</span><span class="sxs-lookup"><span data-stu-id="f1270-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="f1270-118">Esto se realiza al configurar las opciones para el contexto - normalmente en `DbContext.OnConfiguring`, o bien en `Startup.cs` si utiliza ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1270-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
