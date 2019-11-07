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
# <a name="testing-components-using-ef-core"></a>Prueba de componentes mediante EF Core

Es posible que quiera probar componentes con algo que se aproxime a la conexión a la base de datos real, sin la sobrecarga de las operaciones reales de E/S de base de datos.

Hay dos opciones principales para hacerlo:

* El [modo en memoria de SQLite](sqlite.md) permite escribir pruebas eficaces en un proveedor que se comporte como una base de datos relacional.
* El [proveedor de InMemory](in-memory.md) es un proveedor ligero que tiene dependencias mínimas, pero que no siempre se comporta como una base de datos relacional.
