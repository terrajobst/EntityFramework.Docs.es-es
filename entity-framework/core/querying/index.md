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
# <a name="querying-data"></a>Consulta de datos

Entity Framework Core usa Language Integrated Query (LINQ) para consultar datos de la base de datos. LINQ permite usar C# (o el lenguaje .NET que prefiera) para escribir consultas fuertemente tipadas basadas en el contexto derivado y las clases de entidad. Una representación de la consulta LINQ se pasa al proveedor de base de datos para que la convierta en lenguaje de consulta específico de la base de datos (por ejemplo, SQL para una base de datos relacional). Para más información sobre cómo se procesa una consulta, vea [Cómo funcionan las consultas](overview.md).
