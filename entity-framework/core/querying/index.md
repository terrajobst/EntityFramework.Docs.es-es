---
title: Consulta de datos - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26048887"
---
# <a name="querying-data"></a>Consulta de datos

Entity Framework Core usa Language Integrated Query (LINQ) para consultar los datos de la base de datos. LINQ permite usar C# (o el lenguaje .NET que prefiera) para escribir consultas fuertemente tipadas basadas en el contexto derivado y las clases de entidad. Una representación de la consulta LINQ se pasa al proveedor de base de datos para que la convierta en lenguaje de consulta específico de la base de datos (por ejemplo, SQL para una base de datos relacional). Para más información sobre cómo se procesa una consulta, vea [Cómo funcionan las consultas](overview.md).
