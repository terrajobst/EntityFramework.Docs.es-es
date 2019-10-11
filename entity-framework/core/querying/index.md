---
title: Consulta de datos - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 009235c3673a414e06d1a64f9877b60e7cde97b0
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181915"
---
# <a name="querying-data"></a>Consulta de datos

Entity Framework Core usa Language Integrated Query (LINQ) para consultar datos de la base de datos. LINQ permite usar C# (o el lenguaje .NET que prefiera) para escribir consultas fuertemente tipadas. Usa el contexto derivado y las clases de entidad para hacer referencia a los objetos de base de datos. EF Core pasa una representación de la consulta LINQ al proveedor de la base de datos. A su vez, los proveedores de la base de datos la traducen al lenguaje de la consulta específico para la base de datos (por ejemplo, SQL para una base de datos relacional).

> [!TIP]
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.

Los fragmentos de código siguientes muestran algunos ejemplos de cómo realizar tareas comunes con Entity Framework Core.

## <a name="loading-all-data"></a>Carga de todos los datos

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>Carga de una sola entidad

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Filtrado

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a>Lecturas adicionales

- Obtenga más información sobre las [expresiones de consulta LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).
- Para más información sobre cómo se procesa una consulta en EF Core, vea [Cómo funcionan las consultas](xref:core/querying/how-query-works).
