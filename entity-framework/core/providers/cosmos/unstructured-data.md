---
title: 'Proveedor de Azure Cosmos DB: trabajar con datos no estructurados EF Core'
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: b47d41b6-984f-419a-ab10-2ed3b95e3919
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 86bb0f7915c8a2561e7d5cd5dffc27474218a112
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150774"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="d3f1d-102">Trabajar con datos no estructurados en EF Core proveedor de Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="d3f1d-102">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="d3f1d-103">EF Core se diseñó para facilitar el trabajo con datos que siguen un esquema definido en el modelo.</span><span class="sxs-lookup"><span data-stu-id="d3f1d-103">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="d3f1d-104">Sin embargo, uno de los puntos fuertes de Azure Cosmos DB es la flexibilidad en la forma de los datos almacenados.</span><span class="sxs-lookup"><span data-stu-id="d3f1d-104">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="d3f1d-105">Acceso a JSON sin formato</span><span class="sxs-lookup"><span data-stu-id="d3f1d-105">Accessing the raw JSON</span></span>

<span data-ttu-id="d3f1d-106">Es posible obtener acceso a las propiedades de las que no realiza un seguimiento EF Core a través de una propiedad especial en el `"__jObject"` [Estado de sombra](../../modeling/shadow-properties.md) denominado que contiene un `JObject` que representa los datos recibidos del almacén y los datos que se van a almacenar:</span><span class="sxs-lookup"><span data-stu-id="d3f1d-106">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=21-23&name=Unmapped)]

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "eLMaAK8TzkIBAAAAAAAAAA==",
    "_self": "dbs/eLMaAA==/colls/eLMaAK8TzkI=/docs/eLMaAK8TzkIBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683e-0a12bf8d01d5\"",
    "_attachments": "attachments/",
    "BillingAddress": "Clarence House",
    "_ts": 1568164374
}
```

> [!WARNING]
> <span data-ttu-id="d3f1d-107">La `"__jObject"` propiedad forma parte de la infraestructura de EF Core y solo se debe utilizar como último recurso, ya que es probable que tenga un comportamiento diferente en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="d3f1d-107">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="d3f1d-108">Los cambios en la entidad invalidarán los valores `"__jObject"` almacenados `SaveChanges`en durante.</span><span class="sxs-lookup"><span data-stu-id="d3f1d-108">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="d3f1d-109">Usar CosmosClient</span><span class="sxs-lookup"><span data-stu-id="d3f1d-109">Using CosmosClient</span></span>

<span data-ttu-id="d3f1d-110">Para desacoplar completamente de EF Core obtener `CosmosClient` el objeto que forma [parte del SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) de Azure Cosmos dB `DbContext`de:</span><span class="sxs-lookup"><span data-stu-id="d3f1d-110">To decouple completely from EF Core get the `CosmosClient` object that is [part of the Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="d3f1d-111">Faltan valores de propiedad</span><span class="sxs-lookup"><span data-stu-id="d3f1d-111">Missing property values</span></span>

<span data-ttu-id="d3f1d-112">En el ejemplo anterior, se ha `"TrackingNumber"` quitado la propiedad del pedido.</span><span class="sxs-lookup"><span data-stu-id="d3f1d-112">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="d3f1d-113">Debido a cómo funciona la indexación en Cosmos DB, las consultas que hacen referencia a la propiedad que falta en otro lugar y en la proyección podrían devolver resultados inesperados.</span><span class="sxs-lookup"><span data-stu-id="d3f1d-113">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="d3f1d-114">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d3f1d-114">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="d3f1d-115">La consulta ordenada realmente no devuelve ningún resultado.</span><span class="sxs-lookup"><span data-stu-id="d3f1d-115">The sorted query actually returns no results.</span></span> <span data-ttu-id="d3f1d-116">Esto significa que debe tener cuidado de rellenar siempre las propiedades asignadas por EF Core al trabajar directamente con el almacén.</span><span class="sxs-lookup"><span data-stu-id="d3f1d-116">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="d3f1d-117">Este comportamiento puede cambiar en versiones futuras de Cosmos.</span><span class="sxs-lookup"><span data-stu-id="d3f1d-117">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="d3f1d-118">Por ejemplo, actualmente, si la Directiva de indexación define el índice compuesto {ID/?</span><span class="sxs-lookup"><span data-stu-id="d3f1d-118">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="d3f1d-119">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="d3f1d-119">ASC, TrackingNumber/?</span></span> <span data-ttu-id="d3f1d-120">ASC)}, una consulta que tiene ' order by c.ID ASC, c. Discriminator ASC ' devolvería elementos a los __que les falta__ la `"TrackingNumber"` propiedad.</span><span class="sxs-lookup"><span data-stu-id="d3f1d-120">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>