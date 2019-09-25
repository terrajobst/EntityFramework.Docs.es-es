---
title: Prueba de componentes mediante EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: a946387718546f14e1485b4093e6c8046188f62d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197495"
---
# <a name="testing-components-using-ef-core"></a>Prueba de componentes mediante EF Core

Es posible que quiera probar componentes con algo que se aproxime a la conexión a la base de datos real, sin la sobrecarga de las operaciones reales de E/S de base de datos.

Hay dos opciones principales para hacerlo:
 * El [modo en memoria de SQLite](sqlite.md) permite escribir pruebas eficaces en un proveedor que se comporte como una base de datos relacional.
 * El [proveedor de InMemory](in-memory.md) es un proveedor ligero que tiene dependencias mínimas, pero que no siempre se comporta como una base de datos relacional.
