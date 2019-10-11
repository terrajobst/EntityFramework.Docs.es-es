---
title: Prueba de componentes mediante EF Core - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 8de7df80ce91c4d94133a96d759dd552d0ba1884
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181308"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="4afe5-102">Prueba de componentes mediante EF Core</span><span class="sxs-lookup"><span data-stu-id="4afe5-102">Testing components using EF Core</span></span>

<span data-ttu-id="4afe5-103">Es posible que quiera probar componentes con algo que se aproxime a la conexión a la base de datos real, sin la sobrecarga de las operaciones reales de E/S de base de datos.</span><span class="sxs-lookup"><span data-stu-id="4afe5-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="4afe5-104">Hay dos opciones principales para hacerlo:</span><span class="sxs-lookup"><span data-stu-id="4afe5-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="4afe5-105">El [modo en memoria de SQLite](sqlite.md) permite escribir pruebas eficaces en un proveedor que se comporte como una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="4afe5-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="4afe5-106">El [proveedor de InMemory](in-memory.md) es un proveedor ligero que tiene dependencias mínimas, pero que no siempre se comporta como una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="4afe5-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
