---
title: Probar componentes con Entity Framework - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26048867"
---
# <a name="testing"></a><span data-ttu-id="3460d-102">Pruebas</span><span class="sxs-lookup"><span data-stu-id="3460d-102">Testing</span></span>

<span data-ttu-id="3460d-103">Es posible que quiera probar componentes con algo que se aproxime a la conexión a la base de datos real, sin la sobrecarga de las operaciones reales de E/S de base de datos.</span><span class="sxs-lookup"><span data-stu-id="3460d-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="3460d-104">Hay dos opciones principales para hacerlo:</span><span class="sxs-lookup"><span data-stu-id="3460d-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="3460d-105">El [modo en memoria de SQLite](sqlite.md) permite escribir pruebas eficaces en un proveedor que se comporte como una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="3460d-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="3460d-106">El [proveedor de InMemory](in-memory.md) es un proveedor ligero que tiene dependencias mínimas, pero que no siempre se comporta como una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="3460d-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
