---
title: "Administración de esquemas de base de datos - EF Core"
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="managing-database-schemas"></a><span data-ttu-id="335e1-102">Administración de esquemas de base de datos</span><span class="sxs-lookup"><span data-stu-id="335e1-102">Managing Database Schemas</span></span>
<span data-ttu-id="335e1-103">EF Core proporciona dos métodos principales para mantener sincronizados el esquema de la base de datos y el modelo de EF Core. Para elegir entre los dos, decida si es el modelo de EF Core o el esquema de la base de datos el origen verdadero.</span><span class="sxs-lookup"><span data-stu-id="335e1-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="335e1-104">Si quiere que el modelo de EF Core sea el origen verdadero, use [Migraciones][1].</span><span class="sxs-lookup"><span data-stu-id="335e1-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="335e1-105">Al realizar cambios en el modelo de EF Core, este método aplica de forma incremental los cambios de esquema correspondientes a la base de datos para que siga siendo compatible con el modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="335e1-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="335e1-106">Use [Ingeniería inversa][2] si quiere que el esquema de la base de datos sea el origen verdadero.</span><span class="sxs-lookup"><span data-stu-id="335e1-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="335e1-107">Este método permite aplicar la técnica de scaffolding a un elemento DbContext y a las clases de tipo de entidad mediante la aplicación de ingeniería inversa al esquema de la base de datos de un modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="335e1-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="335e1-108">Las [API de creación y eliminación][3] también pueden crear el esquema de la base de datos a partir del modelo de EF Core.</span><span class="sxs-lookup"><span data-stu-id="335e1-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="335e1-109">Pero son principalmente para pruebas, creación de prototipos y otros escenarios donde la eliminación de la base de datos es aceptable.</span><span class="sxs-lookup"><span data-stu-id="335e1-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
