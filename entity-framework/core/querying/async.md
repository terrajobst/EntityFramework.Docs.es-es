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
# <a name="asynchronous-queries"></a>Consultas asincrónicas

Las consultas asincrónicas evitan bloquear un subproceso mientras la consulta se ejecuta en la base de datos. Esto puede resultar útil para evitar la inmovilización de la interfaz de usuario de una aplicación cliente pesada. Las operaciones asincrónicas también pueden aumentar el rendimiento de una aplicación web, donde se puede liberar el subproceso para atender otras solicitudes mientras se completa la operación de la base de datos. Para más información, consulte [Programación asincrónica en C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core no admite que varias operaciones en paralelo se ejecuten en la misma instancia de contexto. Siempre debe esperar que se complete una operación antes de iniciar la siguiente. Habitualmente, para esto se usa la palabra clave `await` en cada una de las operaciones asincrónicas.

Entity Framework Core proporciona un conjunto de métodos de extensión asincrónicos que se pueden usar como alternativa a los métodos LINQ que hacen que se ejecute una consulta y se devuelvan los resultados. Los ejemplos incluyen `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. No hay versiones asincrónicas de operadores LINQ como `Where(...)`, `OrderBy(...)`, etc., porque estos métodos solo generan el árbol de la expresión LINQ y no hacen que la consulta se ejecute en la base de datos.

> [!IMPORTANT]  
> Los métodos de extensión asincrónicos de EF Core se define en el espacio de nombres `Microsoft.EntityFrameworkCore`. Es necesario importar este espacio de nombres para que los métodos estén disponibles.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#Sample)]
