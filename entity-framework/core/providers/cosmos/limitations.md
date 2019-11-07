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
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="7fe8e-103">Limitaciones del proveedor de Azure Cosmos DB de EF Core</span><span class="sxs-lookup"><span data-stu-id="7fe8e-103">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="7fe8e-104">El proveedor de Cosmos tiene una serie de limitaciones.</span><span class="sxs-lookup"><span data-stu-id="7fe8e-104">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="7fe8e-105">Muchas de estas limitaciones son consecuencia de las limitaciones del motor de base de datos de Cosmos subyacente y no son específicas de EF.</span><span class="sxs-lookup"><span data-stu-id="7fe8e-105">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="7fe8e-106">Pero la mayoría [no se ha implementado todavía](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="7fe8e-106">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="7fe8e-107">Limitaciones temporales</span><span class="sxs-lookup"><span data-stu-id="7fe8e-107">Temporary limitations</span></span>

- <span data-ttu-id="7fe8e-108">Incluso si solo hay un tipo de entidad sin herencia asignada a un contenedor, todavía tiene una propiedad discriminadora.</span><span class="sxs-lookup"><span data-stu-id="7fe8e-108">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="7fe8e-109">Los tipos de entidad con claves de partición no funcionan correctamente en algunos escenarios</span><span class="sxs-lookup"><span data-stu-id="7fe8e-109">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="7fe8e-110">no se admiten llamadas `Include`</span><span class="sxs-lookup"><span data-stu-id="7fe8e-110">`Include` calls are not supported</span></span>
- <span data-ttu-id="7fe8e-111">no se admiten llamadas `Join`</span><span class="sxs-lookup"><span data-stu-id="7fe8e-111">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="7fe8e-112">Limitaciones del SDK de Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7fe8e-112">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="7fe8e-113">Solo se proporcionan métodos asincrónicos</span><span class="sxs-lookup"><span data-stu-id="7fe8e-113">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="7fe8e-114">Dado que no hay ninguna versión de sincronización de los métodos de nivel bajo EF Core se basa en, la funcionalidad correspondiente se implementa actualmente llamando a `.Wait()` en el `Task`devuelto.</span><span class="sxs-lookup"><span data-stu-id="7fe8e-114">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="7fe8e-115">Esto significa que el uso de métodos como `SaveChanges`, o `ToList` en lugar de sus homólogos asincrónicos podría provocar un interbloqueo en la aplicación</span><span class="sxs-lookup"><span data-stu-id="7fe8e-115">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="7fe8e-116">Limitaciones de Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7fe8e-116">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="7fe8e-117">Puede ver la información general completa de [Azure Cosmos dB características admitidas](/azure/cosmos-db/modeling-data), estas son las diferencias más importantes en comparación con una base de datos relacional:</span><span class="sxs-lookup"><span data-stu-id="7fe8e-117">You can see the full overview of [Azure Cosmos DB supported features](/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="7fe8e-118">No se admiten las transacciones iniciadas por el cliente</span><span class="sxs-lookup"><span data-stu-id="7fe8e-118">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="7fe8e-119">Algunas consultas entre particiones no se admiten o son mucho más lentas en función de los operadores implicados</span><span class="sxs-lookup"><span data-stu-id="7fe8e-119">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>
