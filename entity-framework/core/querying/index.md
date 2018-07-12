---
title: Consulta de datos - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: 447f48b780bc48b7a79153d17dcc1b8ef0cc508c
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949280"
---
# <a name="querying-data"></a><span data-ttu-id="aa249-102">Consulta de datos</span><span class="sxs-lookup"><span data-stu-id="aa249-102">Querying Data</span></span>

<span data-ttu-id="aa249-103">Entity Framework Core usa Language Integrated Query (LINQ) para consultar datos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="aa249-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="aa249-104">LINQ permite usar C# (o el lenguaje .NET que prefiera) para escribir consultas fuertemente tipadas basadas en el contexto derivado y las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="aa249-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="aa249-105">Una representación de la consulta LINQ se pasa al proveedor de base de datos para que la convierta en lenguaje de consulta específico de la base de datos (por ejemplo, SQL para una base de datos relacional).</span><span class="sxs-lookup"><span data-stu-id="aa249-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="aa249-106">Para más información sobre cómo se procesa una consulta, vea [Cómo funcionan las consultas](overview.md).</span><span class="sxs-lookup"><span data-stu-id="aa249-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
