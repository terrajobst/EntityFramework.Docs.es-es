---
title: Prueba de componentes mediante EF Core - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655785"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="e4746-102">Prueba de componentes mediante EF Core</span><span class="sxs-lookup"><span data-stu-id="e4746-102">Testing components using EF Core</span></span>

<span data-ttu-id="e4746-103">Es posible que quiera probar componentes con algo que se aproxime a la conexión a la base de datos real, sin la sobrecarga de las operaciones reales de E/S de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e4746-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="e4746-104">Hay dos opciones principales para hacerlo:</span><span class="sxs-lookup"><span data-stu-id="e4746-104">There are two main options for doing this:</span></span>

* <span data-ttu-id="e4746-105">El [modo en memoria de SQLite](sqlite.md) permite escribir pruebas eficaces en un proveedor que se comporte como una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="e4746-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
* <span data-ttu-id="e4746-106">El [proveedor de InMemory](in-memory.md) es un proveedor ligero que tiene dependencias mínimas, pero que no siempre se comporta como una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="e4746-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
