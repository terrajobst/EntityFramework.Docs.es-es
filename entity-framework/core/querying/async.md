---
title: 'Consultas asincrónicas: EF Core'
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: de00e25279e29355a4eb3e55597a8578ceccecb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993570"
---
# <a name="asynchronous-queries"></a>Consultas asincrónicas

Las consultas asincrónicas evitan bloquear un subproceso mientras la consulta se ejecuta en la base de datos. Esto puede resultar útil para evitar la inmovilización de la interfaz de usuario de una aplicación cliente pesada. Las operaciones asincrónicas también pueden aumentar el rendimiento de una aplicación web, donde se puede liberar el subproceso para atender otras solicitudes mientras se completa la operación de la base de datos. Para más información, consulte [Programación asincrónica en C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core no admite que varias operaciones en paralelo se ejecuten en la misma instancia de contexto. Siempre debe esperar que se complete una operación antes de iniciar la siguiente. Habitualmente, para esto se usa la palabra clave `await` en cada una de las operaciones asincrónicas.

Entity Framework Core proporciona un conjunto de métodos de extensión asincrónicos que se pueden usar como alternativa a los métodos LINQ que hacen que se ejecute una consulta y se devuelvan los resultados. Los ejemplos incluyen `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. No hay versiones asincrónicas de operadores LINQ como `Where(...)`, `OrderBy(...)`, etc., porque estos métodos solo generan el árbol de la expresión LINQ y no hacen que la consulta se ejecute en la base de datos.

> [!IMPORTANT]  
> Los métodos de extensión asincrónicos de EF Core se define en el espacio de nombres `Microsoft.EntityFrameworkCore`. Es necesario importar este espacio de nombres para que los métodos estén disponibles.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
