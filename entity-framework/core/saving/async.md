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
# <a name="asynchronous-saving"></a>Guardar asincrónica

Guardar asincrónica evita bloqueando un subproceso mientras se escriben los cambios en la base de datos. Esto puede ser útil para evitar la inmovilización de la interfaz de usuario de una aplicación cliente grueso. Operaciones asincrónicas también pueden aumentar el rendimiento en una aplicación web, donde el subproceso se puede liberar para atender a otras solicitudes mientras se completa la operación de base de datos. Para obtener más información, consulte [programación asincrónica en C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Núcleo EF no admite varias operaciones paralelas que se va a ejecutar en la misma instancia de contexto. Siempre debe esperar a que finalice antes de comenzar la siguiente operación de una operación. Esto normalmente se realiza mediante el `await` palabra clave en cada operación asincrónica.

Entity Framework Core proporciona `DbContext.SaveChangesAsync()` como una alternativa asincrónica a `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
