---
title: 'Proveedor de Azure Cosmos DB: trabajar con datos no estructurados EF Core'
description: Cómo trabajar con Azure Cosmos DB datos no estructurados mediante Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 69f979d46174ff56310b334f28438ac271f45155
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888101"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Trabajar con datos no estructurados en EF Core proveedor de Azure Cosmos DB

EF Core se diseñó para facilitar el trabajo con datos que siguen un esquema definido en el modelo. Sin embargo, uno de los puntos fuertes de Azure Cosmos DB es la flexibilidad en la forma de los datos almacenados.

## <a name="accessing-the-raw-json"></a>Acceso a JSON sin formato

Es posible obtener acceso a las propiedades de las que no se realiza un seguimiento EF Core a través de una propiedad especial en el [Estado de sombra](../../modeling/shadow-properties.md) denominado `"__jObject"` que contiene un `JObject` que representa los datos recibidos del almacén y los datos que se almacenan:

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=23,24&name=Unmapped)]

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
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
> La propiedad `"__jObject"` forma parte de la infraestructura de EF Core y solo se debe utilizar como último recurso, ya que es probable que tenga un comportamiento diferente en versiones futuras.

> [!NOTE]
> Los cambios en la entidad invalidarán los valores almacenados en `"__jObject"` durante `SaveChanges`.

## <a name="using-cosmosclient"></a>Usar CosmosClient

Para desacoplar completamente de EF Core obtener el objeto [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) que forma [parte del SDK de Azure Cosmos dB](/azure/cosmos-db/sql-api-get-started) de `DbContext`:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Faltan valores de propiedad

En el ejemplo anterior, se ha quitado la propiedad `"TrackingNumber"` del pedido. Debido a cómo funciona la indexación en Cosmos DB, las consultas que hacen referencia a la propiedad que falta en otro lugar y en la proyección podrían devolver resultados inesperados. Por ejemplo:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

La consulta ordenada realmente no devuelve ningún resultado. Esto significa que debe tener cuidado de rellenar siempre las propiedades asignadas por EF Core al trabajar directamente con el almacén.

> [!NOTE]
> Este comportamiento puede cambiar en versiones futuras de Cosmos. Por ejemplo, actualmente, si la Directiva de indexación define el índice compuesto {ID/? ASC, TrackingNumber/? ASC)}, una consulta que tiene ' ORDER BY c.Id ASC, c. Discriminator ASC ' devolvería elementos a los __que les falta__ la propiedad `"TrackingNumber"`.
