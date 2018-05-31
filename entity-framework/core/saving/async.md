---
title: 'Guardado asincrónico: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052625"
---
# <a name="asynchronous-saving"></a>Guardado asincrónico

El guardado asincrónico evita bloquear un subproceso mientras se escriben los cambios en la base de datos. Esto puede resultar útil para evitar la inmovilización de la interfaz de usuario de una aplicación cliente pesada. Las operaciones asincrónicas también pueden aumentar el rendimiento de una aplicación web, donde se puede liberar el subproceso para atender otras solicitudes mientras se completa la operación de la base de datos. Para más información, consulte [Programación asincrónica en C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core no admite que varias operaciones en paralelo se ejecuten en la misma instancia de contexto. Siempre debe esperar que se complete una operación antes de iniciar la siguiente. Habitualmente, para esto se usa la palabra clave `await` en cada una de las operaciones asincrónicas.

Entity Framework Core proporciona `DbContext.SaveChangesAsync()` como una alternativa asincrónica para `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
