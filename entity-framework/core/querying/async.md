---
title: "Consultas asincrónicas - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a><span data-ttu-id="c754d-102">Consultas asincrónicas</span><span class="sxs-lookup"><span data-stu-id="c754d-102">Asynchronous Queries</span></span>

<span data-ttu-id="c754d-103">Consultas asincrónicas evitar bloquear un subproceso mientras se ejecuta la consulta en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c754d-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="c754d-104">Esto puede ser útil para evitar la inmovilización de la interfaz de usuario de una aplicación cliente grueso.</span><span class="sxs-lookup"><span data-stu-id="c754d-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="c754d-105">Operaciones asincrónicas también pueden aumentar el rendimiento en una aplicación web, donde el subproceso se puede liberar para atender a otras solicitudes mientras se completa la operación de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c754d-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="c754d-106">Para obtener más información, consulte [programación asincrónica en C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="c754d-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="c754d-107">Núcleo EF no admite varias operaciones paralelas que se va a ejecutar en la misma instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="c754d-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="c754d-108">Siempre debe esperar a que finalice antes de comenzar la siguiente operación de una operación.</span><span class="sxs-lookup"><span data-stu-id="c754d-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="c754d-109">Esto normalmente se realiza mediante el `await` palabra clave en cada operación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="c754d-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="c754d-110">Entity Framework Core proporciona un conjunto de métodos de extensión asincrónica que puede utilizarse como alternativa a los métodos LINQ que hacen que se ejecuta una consulta y los resultados devueltos.</span><span class="sxs-lookup"><span data-stu-id="c754d-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="c754d-111">Algunos ejemplos son `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etcetera. No existen versiones asincrónicas de los operadores LINQ como `Where(...)`, `OrderBy(...)`, etc. porque estos métodos sólo generación el árbol de expresión LINQ y hacen que la consulta que se ejecutará en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c754d-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="c754d-112">Los métodos de extensión de núcleo de EF async se definen en el `Microsoft.EntityFrameworkCore` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="c754d-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="c754d-113">Este espacio de nombres se debe importar para que los métodos que se va a estar disponible.</span><span class="sxs-lookup"><span data-stu-id="c754d-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
