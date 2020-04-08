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
# <a name="saving-data"></a><span data-ttu-id="71d89-102">Guardado de datos</span><span class="sxs-lookup"><span data-stu-id="71d89-102">Saving Data</span></span>

<span data-ttu-id="71d89-103">Cada instancia de contexto tiene un elemento `ChangeTracker` que es responsable de realizar el seguimiento de los cambios que deben escribirse en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="71d89-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="71d89-104">Al realizar cambios en instancias de las clases de entidad, estos cambios se registran en `ChangeTracker` y luego se escriben en la base de datos cuando se llama a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="71d89-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="71d89-105">El proveedor de base de datos es responsable de convertir los cambios en operaciones específicas de la base de datos (por ejemplo, los comandos `INSERT`, `UPDATE` y `DELETE` de una base de datos relacional).</span><span class="sxs-lookup"><span data-stu-id="71d89-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
