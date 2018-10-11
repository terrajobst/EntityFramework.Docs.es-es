---
title: 'Evaluación de cliente frente a servidor: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: 47e22be274d02b5221c638d07151d9607aa7e24f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250808"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="9a4aa-102">Evaluación de cliente frente a servidor</span><span class="sxs-lookup"><span data-stu-id="9a4aa-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="9a4aa-103">Entity Framework Core admite partes de la consulta que se evalúan en el cliente y partes de ella que se insertan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="9a4aa-104">Determinar qué partes de la consulta se evaluarán en la base de datos depende del proveedor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="9a4aa-105">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="9a4aa-106">Evaluación de cliente</span><span class="sxs-lookup"><span data-stu-id="9a4aa-106">Client evaluation</span></span>

<span data-ttu-id="9a4aa-107">En el ejemplo siguiente, se usa un método auxiliar para estandarizar las direcciones URL para los blogs que se devuelven desde una base de datos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="9a4aa-108">Como el proveedor de SQL Server no tiene información de cómo se implementa este método, no es posible traducirlo a código SQL.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="9a4aa-109">Todos los demás aspectos de la consulta se evalúan en la base de datos, pero es el cliente quien pasa la `URL` devuelta a través de este método.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="client-evaluation-performance-issues"></a><span data-ttu-id="9a4aa-110">Problemas de rendimiento de evaluación de cliente</span><span class="sxs-lookup"><span data-stu-id="9a4aa-110">Client evaluation performance issues</span></span>

<span data-ttu-id="9a4aa-111">Si bien la evaluación de cliente puede resultar muy útil, en algunas instancias puede generar un rendimiento deficiente.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="9a4aa-112">Considere la consulta siguiente, en la que el método auxiliar ahora se usa en un filtro.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="9a4aa-113">Como no se puede realizar en la base de datos, todos los datos se incorporan a la memoria y, luego, el filtro se aplica en el cliente.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="9a4aa-114">En función de la cantidad de datos y de cuántos de esos datos se filtran, esta acción podría generar un rendimiento deficiente.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a><span data-ttu-id="9a4aa-115">Registros de evaluación de cliente</span><span class="sxs-lookup"><span data-stu-id="9a4aa-115">Client evaluation logging</span></span>

<span data-ttu-id="9a4aa-116">De manera predeterminada, EF Core registrará una advertencia cuando se realice la evaluación de cliente.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-116">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="9a4aa-117">Consulte [Logging](../miscellaneous/logging.md) (Registro) para más información sobre cómo ver la salida de registro.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-117">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a><span data-ttu-id="9a4aa-118">Comportamiento opcional: iniciar una excepción para la evaluación de cliente</span><span class="sxs-lookup"><span data-stu-id="9a4aa-118">Optional behavior: throw an exception for client evaluation</span></span>

<span data-ttu-id="9a4aa-119">Puede cambiar el comportamiento cuando se produzca la evaluación de cliente, ya sea para que se genere una excepción o para que no haga nada.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-119">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="9a4aa-120">Esto se hace al configurar las opciones del contexto, por lo general en `DbContext.OnConfiguring` o en `Startup.cs` si usa ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9a4aa-120">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
