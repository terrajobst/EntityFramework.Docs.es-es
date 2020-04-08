---
title: Guardado de datos - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413100"
---
# <a name="saving-data"></a>Guardado de datos

Cada instancia de contexto tiene un elemento `ChangeTracker` que es responsable de realizar el seguimiento de los cambios que deben escribirse en la base de datos. Al realizar cambios en instancias de las clases de entidad, estos cambios se registran en `ChangeTracker` y luego se escriben en la base de datos cuando se llama a `SaveChanges`. El proveedor de base de datos es responsable de convertir los cambios en operaciones específicas de la base de datos (por ejemplo, los comandos `INSERT`, `UPDATE` y `DELETE` de una base de datos relacional).
