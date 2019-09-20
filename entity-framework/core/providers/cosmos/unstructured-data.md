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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Trabajar con datos no estructurados en EF Core proveedor de Azure Cosmos DB

EF Core se diseñó para facilitar el trabajo con datos que siguen un esquema definido en el modelo. Sin embargo, uno de los puntos fuertes de Azure Cosmos DB es la flexibilidad en la forma de los datos almacenados.

## <a name="accessing-the-raw-json"></a>Acceso a JSON sin formato

Es posible obtener acceso a las propiedades de las que no realiza un seguimiento EF Core a través de una propiedad especial en el `"__jObject"` [Estado de sombra](../../modeling/shadow-properties.md) denominado que contiene un `JObject` que representa los datos recibidos del almacén y los datos que se van a almacenar:

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
> La `"__jObject"` propiedad forma parte de la infraestructura de EF Core y solo se debe utilizar como último recurso, ya que es probable que tenga un comportamiento diferente en futuras versiones.

> [!NOTE]
> Los cambios en la entidad invalidarán los valores `"__jObject"` almacenados `SaveChanges`en durante.

## <a name="using-cosmosclient"></a>Usar CosmosClient

Para desacoplar completamente de EF Core obtener `CosmosClient` el objeto que forma [parte del SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) de Azure Cosmos dB `DbContext`de:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Faltan valores de propiedad

En el ejemplo anterior, se ha `"TrackingNumber"` quitado la propiedad del pedido. Debido a cómo funciona la indexación en Cosmos DB, las consultas que hacen referencia a la propiedad que falta en otro lugar y en la proyección podrían devolver resultados inesperados. Por ejemplo:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

La consulta ordenada realmente no devuelve ningún resultado. Esto significa que debe tener cuidado de rellenar siempre las propiedades asignadas por EF Core al trabajar directamente con el almacén.

> [!NOTE]
> Este comportamiento puede cambiar en versiones futuras de Cosmos. Por ejemplo, actualmente, si la Directiva de indexación define el índice compuesto {ID/? ASC, TrackingNumber/? ASC)}, una consulta que tiene ' order by c.ID ASC, c. Discriminator ASC ' devolvería elementos a los __que les falta__ la `"TrackingNumber"` propiedad.