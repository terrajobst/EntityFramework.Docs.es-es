---
title: Probar componentes con Entity Framework - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997842"
---
# <a name="testing"></a><span data-ttu-id="6aebb-102">Pruebas</span><span class="sxs-lookup"><span data-stu-id="6aebb-102">Testing</span></span>

<span data-ttu-id="6aebb-103">Es posible que quiera probar componentes con algo que se aproxime a la conexión a la base de datos real, sin la sobrecarga de las operaciones reales de E/S de base de datos.</span><span class="sxs-lookup"><span data-stu-id="6aebb-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="6aebb-104">Hay dos opciones principales para hacerlo:</span><span class="sxs-lookup"><span data-stu-id="6aebb-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="6aebb-105">El [modo en memoria de SQLite](sqlite.md) permite escribir pruebas eficaces en un proveedor que se comporte como una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="6aebb-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="6aebb-106">El [proveedor de InMemory](in-memory.md) es un proveedor ligero que tiene dependencias mínimas, pero que no siempre se comporta como una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="6aebb-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
