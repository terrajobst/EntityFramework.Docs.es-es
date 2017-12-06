---
title: "Guardar asincrónica - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-saving"></a><span data-ttu-id="bda63-102">Guardar asincrónica</span><span class="sxs-lookup"><span data-stu-id="bda63-102">Asynchronous Saving</span></span>

<span data-ttu-id="bda63-103">Guardar asincrónica evita bloqueando un subproceso mientras se escriben los cambios en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bda63-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="bda63-104">Esto puede ser útil para evitar la inmovilización de la interfaz de usuario de una aplicación cliente grueso.</span><span class="sxs-lookup"><span data-stu-id="bda63-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="bda63-105">Operaciones asincrónicas también pueden aumentar el rendimiento en una aplicación web, donde el subproceso se puede liberar para atender a otras solicitudes mientras se completa la operación de base de datos.</span><span class="sxs-lookup"><span data-stu-id="bda63-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="bda63-106">Para obtener más información, consulte [programación asincrónica en C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="bda63-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="bda63-107">Núcleo EF no admite varias operaciones paralelas que se va a ejecutar en la misma instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="bda63-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="bda63-108">Siempre debe esperar a que finalice antes de comenzar la siguiente operación de una operación.</span><span class="sxs-lookup"><span data-stu-id="bda63-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="bda63-109">Esto normalmente se realiza mediante el `await` palabra clave en cada operación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="bda63-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="bda63-110">Entity Framework Core proporciona `DbContext.SaveChangesAsync()` como una alternativa asincrónica a `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="bda63-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
