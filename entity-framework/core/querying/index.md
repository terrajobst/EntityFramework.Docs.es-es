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
# <a name="querying-data"></a><span data-ttu-id="aeb49-102">Consulta de datos</span><span class="sxs-lookup"><span data-stu-id="aeb49-102">Querying Data</span></span>

<span data-ttu-id="aeb49-103">Entity Framework Core usa Language Integrated Query (LINQ) para consultar datos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="aeb49-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="aeb49-104">LINQ permite usar C# (o el lenguaje .NET que prefiera) para escribir consultas fuertemente tipadas.</span><span class="sxs-lookup"><span data-stu-id="aeb49-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="aeb49-105">Usa el contexto derivado y las clases de entidad para hacer referencia a los objetos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="aeb49-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="aeb49-106">EF Core pasa una representación de la consulta LINQ al proveedor de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="aeb49-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="aeb49-107">A su vez, los proveedores de la base de datos la traducen al lenguaje de la consulta específico para la base de datos (por ejemplo, SQL para una base de datos relacional).</span><span class="sxs-lookup"><span data-stu-id="aeb49-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="aeb49-108">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="aeb49-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="aeb49-109">Los fragmentos de código siguientes muestran algunos ejemplos de cómo realizar tareas comunes con Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="aeb49-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="aeb49-110">Carga de todos los datos</span><span class="sxs-lookup"><span data-stu-id="aeb49-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="aeb49-111">Carga de una sola entidad</span><span class="sxs-lookup"><span data-stu-id="aeb49-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="aeb49-112">Filtrado</span><span class="sxs-lookup"><span data-stu-id="aeb49-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="aeb49-113">Lecturas adicionales</span><span class="sxs-lookup"><span data-stu-id="aeb49-113">Further readings</span></span>

- <span data-ttu-id="aeb49-114">Obtenga más información sobre las [expresiones de consulta LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).</span><span class="sxs-lookup"><span data-stu-id="aeb49-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="aeb49-115">Para más información sobre cómo se procesa una consulta en EF Core, vea [Cómo funcionan las consultas](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="aeb49-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
