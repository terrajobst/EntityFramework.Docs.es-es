---
title: 'Consultas asincrónicas: EF Core'
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: 415c57df599f1cb1a255f01d1072890fedfd6d2b
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921691"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="d3626-102">Consultas asincrónicas</span><span class="sxs-lookup"><span data-stu-id="d3626-102">Asynchronous Queries</span></span>

<span data-ttu-id="d3626-103">Las consultas asincrónicas evitan bloquear un subproceso mientras la consulta se ejecuta en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d3626-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="d3626-104">Esto puede resultar útil para evitar la inmovilización de la interfaz de usuario de una aplicación cliente pesada.</span><span class="sxs-lookup"><span data-stu-id="d3626-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="d3626-105">Las operaciones asincrónicas también pueden aumentar el rendimiento de una aplicación web, donde se puede liberar el subproceso para atender otras solicitudes mientras se completa la operación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d3626-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="d3626-106">Para más información, consulte [Programación asincrónica en C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="d3626-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="d3626-107">EF Core no admite que varias operaciones en paralelo se ejecuten en la misma instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="d3626-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="d3626-108">Siempre debe esperar que se complete una operación antes de iniciar la siguiente.</span><span class="sxs-lookup"><span data-stu-id="d3626-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="d3626-109">Habitualmente, para esto se usa la palabra clave `await` en cada una de las operaciones asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="d3626-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="d3626-110">Entity Framework Core proporciona un conjunto de métodos de extensión asincrónicos que se pueden usar como alternativa a los métodos LINQ que hacen que se ejecute una consulta y se devuelvan los resultados.</span><span class="sxs-lookup"><span data-stu-id="d3626-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="d3626-111">Los ejemplos incluyen `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. No hay versiones asincrónicas de operadores LINQ como `Where(...)`, `OrderBy(...)`, etc., porque estos métodos solo generan el árbol de la expresión LINQ y no hacen que la consulta se ejecute en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d3626-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="d3626-112">Los métodos de extensión asincrónicos de EF Core se define en el espacio de nombres `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="d3626-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="d3626-113">Es necesario importar este espacio de nombres para que los métodos estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="d3626-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#Sample)]
