---
title: 'Consultas asincrónicas: EF Core'
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413784"
---
# <a name="asynchronous-queries"></a>Consultas asincrónicas

Las consultas asincrónicas evitan bloquear un subproceso mientras la consulta se ejecuta en la base de datos. Las consultas asincrónicas son importantes para mantener una interfaz de usuario dinámica en las aplicaciones cliente de gran tamaño. También pueden aumentar el rendimiento de las aplicaciones web donde liberan el subproceso para atender otras solicitudes de las aplicaciones web. Para más información, consulte [Programación asincrónica en C#](/dotnet/csharp/async).

> [!WARNING]  
> EF Core no admite que varias operaciones en paralelo se ejecuten en la misma instancia de contexto. Siempre debe esperar que se complete una operación antes de iniciar la siguiente. Habitualmente, para esto se usa la palabra clave `await` en cada una de las operaciones asincrónicas.

Entity Framework Core proporciona un conjunto de métodos de extensión asincrónicos similares a los de LINQ, que ejecutan una consulta y devuelven resultados. Entre los ejemplos se incluyen `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`. No hay versiones asincrónicas de algunos operadores de LINQ como `Where(...)` o `OrderBy(...)` porque estos métodos solo generan el árbol de la expresión de LINQ y no hacen que la consulta se ejecute en la base de datos.

> [!IMPORTANT]  
> Los métodos de extensión asincrónicos de EF Core se define en el espacio de nombres `Microsoft.EntityFrameworkCore`. Es necesario importar este espacio de nombres para que los métodos estén disponibles.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
