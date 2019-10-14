---
title: 'Consultas asincrónicas: EF Core'
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181820"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="e6fdf-102">Consultas asincrónicas</span><span class="sxs-lookup"><span data-stu-id="e6fdf-102">Asynchronous Queries</span></span>

<span data-ttu-id="e6fdf-103">Las consultas asincrónicas evitan bloquear un subproceso mientras la consulta se ejecuta en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e6fdf-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="e6fdf-104">Las consultas asincrónicas son importantes para mantener una interfaz de usuario dinámica en las aplicaciones cliente de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="e6fdf-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="e6fdf-105">También pueden aumentar el rendimiento de las aplicaciones web donde liberan el subproceso para atender otras solicitudes de las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="e6fdf-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="e6fdf-106">Para más información, consulte [Programación asincrónica en C#](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="e6fdf-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="e6fdf-107">EF Core no admite que varias operaciones en paralelo se ejecuten en la misma instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="e6fdf-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="e6fdf-108">Siempre debe esperar que se complete una operación antes de iniciar la siguiente.</span><span class="sxs-lookup"><span data-stu-id="e6fdf-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="e6fdf-109">Habitualmente, para esto se usa la palabra clave `await` en cada una de las operaciones asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="e6fdf-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="e6fdf-110">Entity Framework Core proporciona un conjunto de métodos de extensión asincrónicos similares a los de LINQ, que ejecutan una consulta y devuelven resultados.</span><span class="sxs-lookup"><span data-stu-id="e6fdf-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="e6fdf-111">Entre los ejemplos se incluyen `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span><span class="sxs-lookup"><span data-stu-id="e6fdf-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="e6fdf-112">No hay versiones asincrónicas de algunos operadores de LINQ como `Where(...)` o `OrderBy(...)` porque estos métodos solo generan el árbol de la expresión de LINQ y no hacen que la consulta se ejecute en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e6fdf-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="e6fdf-113">Los métodos de extensión asincrónicos de EF Core se define en el espacio de nombres `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="e6fdf-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="e6fdf-114">Es necesario importar este espacio de nombres para que los métodos estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="e6fdf-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
