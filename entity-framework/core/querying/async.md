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
# <a name="asynchronous-queries"></a>Consultas asincrónicas

Consultas asincrónicas evitar bloquear un subproceso mientras se ejecuta la consulta en la base de datos. Esto puede ser útil para evitar la inmovilización de la interfaz de usuario de una aplicación cliente grueso. Operaciones asincrónicas también pueden aumentar el rendimiento en una aplicación web, donde el subproceso se puede liberar para atender a otras solicitudes mientras se completa la operación de base de datos. Para obtener más información, consulte [programación asincrónica en C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Núcleo EF no admite varias operaciones paralelas que se va a ejecutar en la misma instancia de contexto. Siempre debe esperar a que finalice antes de comenzar la siguiente operación de una operación. Esto normalmente se realiza mediante el `await` palabra clave en cada operación asincrónica.

Entity Framework Core proporciona un conjunto de métodos de extensión asincrónica que puede utilizarse como alternativa a los métodos LINQ que hacen que se ejecuta una consulta y los resultados devueltos. Algunos ejemplos son `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etcetera. No existen versiones asincrónicas de los operadores LINQ como `Where(...)`, `OrderBy(...)`, etc. porque estos métodos sólo generación el árbol de expresión LINQ y hacen que la consulta que se ejecutará en la base de datos.

> [!IMPORTANT]  
> Los métodos de extensión de núcleo de EF async se definen en el `Microsoft.EntityFrameworkCore` espacio de nombres. Este espacio de nombres se debe importar para que los métodos que se va a estar disponible.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
