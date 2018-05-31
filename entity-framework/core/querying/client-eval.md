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
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="d777e-102">Evaluación de cliente frente a servidor</span><span class="sxs-lookup"><span data-stu-id="d777e-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="d777e-103">Entity Framework Core admite partes de la consulta que se evalúan en el cliente y partes de ella que se insertan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d777e-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="d777e-104">Determinar qué partes de la consulta se evaluarán en la base de datos depende del proveedor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="d777e-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="d777e-105">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="d777e-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="d777e-106">Evaluación de cliente</span><span class="sxs-lookup"><span data-stu-id="d777e-106">Client evaluation</span></span>

<span data-ttu-id="d777e-107">En el ejemplo siguiente, se usa un método auxiliar para estandarizar las direcciones URL para los blogs que se devuelven desde una base de datos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d777e-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="d777e-108">Como el proveedor de SQL Server no tiene información de cómo se implementa este método, no es posible traducirlo a código SQL.</span><span class="sxs-lookup"><span data-stu-id="d777e-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="d777e-109">Todos los demás aspectos de la consulta se evalúan en la base de datos, pero es el cliente quien pasa la `URL` devuelta a través de este método.</span><span class="sxs-lookup"><span data-stu-id="d777e-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="disabling-client-evaluation"></a><span data-ttu-id="d777e-110">Deshabilitación de la evaluación de cliente</span><span class="sxs-lookup"><span data-stu-id="d777e-110">Disabling client evaluation</span></span>

<span data-ttu-id="d777e-111">Si bien la evaluación de cliente puede resultar muy útil, en algunas instancias puede generar un rendimiento deficiente.</span><span class="sxs-lookup"><span data-stu-id="d777e-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="d777e-112">Considere la consulta siguiente, en la que el método auxiliar ahora se usa en un filtro.</span><span class="sxs-lookup"><span data-stu-id="d777e-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="d777e-113">Como no se puede realizar en la base de datos, todos los datos se incorporan a la memoria y, luego, el filtro se aplica en el cliente.</span><span class="sxs-lookup"><span data-stu-id="d777e-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="d777e-114">En función de la cantidad de datos y de cuántos de esos datos se filtran, esta acción podría generar un rendimiento deficiente.</span><span class="sxs-lookup"><span data-stu-id="d777e-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="d777e-115">De manera predeterminada, EF Core registrará una advertencia cuando se realice la evaluación de cliente.</span><span class="sxs-lookup"><span data-stu-id="d777e-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="d777e-116">Consulte [Logging](../miscellaneous/logging.md) (Registro) para más información sobre cómo ver la salida de registro.</span><span class="sxs-lookup"><span data-stu-id="d777e-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="d777e-117">Puede cambiar el comportamiento cuando se produzca la evaluación de cliente, ya sea para que se genere una excepción o para que no haga nada.</span><span class="sxs-lookup"><span data-stu-id="d777e-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="d777e-118">Esto se hace al configurar las opciones del contexto, por lo general en `DbContext.OnConfiguring` o en `Startup.cs` si usa ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d777e-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
