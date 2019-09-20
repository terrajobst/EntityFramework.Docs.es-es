---
title: Proveedor de Azure Cosmos DB-limitaciones-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150768"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="eccbd-102">Limitaciones del proveedor de Azure Cosmos DB de EF Core</span><span class="sxs-lookup"><span data-stu-id="eccbd-102">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="eccbd-103">El proveedor de Cosmos tiene una serie de limitaciones.</span><span class="sxs-lookup"><span data-stu-id="eccbd-103">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="eccbd-104">Muchas de estas limitaciones son consecuencia de las limitaciones del motor de base de datos de Cosmos subyacente y no son específicas de EF.</span><span class="sxs-lookup"><span data-stu-id="eccbd-104">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="eccbd-105">Pero la mayoría [no se ha implementado todavía](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="eccbd-105">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="eccbd-106">Limitaciones temporales</span><span class="sxs-lookup"><span data-stu-id="eccbd-106">Temporary limitations</span></span>

- <span data-ttu-id="eccbd-107">Incluso si solo hay un tipo de entidad sin herencia asignada a un contenedor, todavía tiene una propiedad discriminadora.</span><span class="sxs-lookup"><span data-stu-id="eccbd-107">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="eccbd-108">Los tipos de entidad con claves de partición no funcionan correctamente en algunos escenarios</span><span class="sxs-lookup"><span data-stu-id="eccbd-108">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="eccbd-109">`Include`no se admiten llamadas</span><span class="sxs-lookup"><span data-stu-id="eccbd-109">`Include` calls are not supported</span></span>
- <span data-ttu-id="eccbd-110">`Join`no se admiten llamadas</span><span class="sxs-lookup"><span data-stu-id="eccbd-110">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="eccbd-111">Limitaciones del SDK de Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="eccbd-111">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="eccbd-112">Solo se proporcionan métodos asincrónicos</span><span class="sxs-lookup"><span data-stu-id="eccbd-112">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="eccbd-113">Dado que no hay ninguna versión de sincronización de los métodos de nivel bajo EF Core se basa en, la funcionalidad correspondiente se implementa `.Wait()` actualmente mediante una `Task`llamada a en el devuelto.</span><span class="sxs-lookup"><span data-stu-id="eccbd-113">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="eccbd-114">Esto significa que el uso de `SaveChanges`métodos como `ToList` o en lugar de sus homólogos asincrónicos podría provocar un interbloqueo en la aplicación</span><span class="sxs-lookup"><span data-stu-id="eccbd-114">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="eccbd-115">Limitaciones de Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="eccbd-115">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="eccbd-116">Puede ver la información general completa de [Azure Cosmos dB características admitidas](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), estas son las diferencias más importantes en comparación con una base de datos relacional:</span><span class="sxs-lookup"><span data-stu-id="eccbd-116">You can see the full overview of [Azure Cosmos DB supported features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="eccbd-117">No se admiten las transacciones iniciadas por el cliente</span><span class="sxs-lookup"><span data-stu-id="eccbd-117">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="eccbd-118">Algunas consultas entre particiones no se admiten o son mucho más lentas en función de los operadores implicados</span><span class="sxs-lookup"><span data-stu-id="eccbd-118">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>