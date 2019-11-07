---
title: Proveedor de Azure Cosmos DB-limitaciones-EF Core
description: Limitaciones del proveedor de Azure Cosmos DB de Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655982"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Limitaciones del proveedor de Azure Cosmos DB de EF Core

El proveedor de Cosmos tiene una serie de limitaciones. Muchas de estas limitaciones son consecuencia de las limitaciones del motor de base de datos de Cosmos subyacente y no son específicas de EF. Pero la mayoría [no se ha implementado todavía](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Limitaciones temporales

- Incluso si solo hay un tipo de entidad sin herencia asignada a un contenedor, todavía tiene una propiedad discriminadora.
- Los tipos de entidad con claves de partición no funcionan correctamente en algunos escenarios
- no se admiten llamadas `Include`
- no se admiten llamadas `Join`

## <a name="azure-cosmos-db-sdk-limitations"></a>Limitaciones del SDK de Azure Cosmos DB

- Solo se proporcionan métodos asincrónicos

> [!WARNING]
> Dado que no hay ninguna versión de sincronización de los métodos de nivel bajo EF Core se basa en, la funcionalidad correspondiente se implementa actualmente llamando a `.Wait()` en el `Task`devuelto. Esto significa que el uso de métodos como `SaveChanges`, o `ToList` en lugar de sus homólogos asincrónicos podría provocar un interbloqueo en la aplicación

## <a name="azure-cosmos-db-limitations"></a>Limitaciones de Azure Cosmos DB

Puede ver la información general completa de [Azure Cosmos dB características admitidas](/azure/cosmos-db/modeling-data), estas son las diferencias más importantes en comparación con una base de datos relacional:

- No se admiten las transacciones iniciadas por el cliente
- Algunas consultas entre particiones no se admiten o son mucho más lentas en función de los operadores implicados
