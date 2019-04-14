---
title: Tabla dividir - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562595"
---
# <a name="table-splitting"></a><span data-ttu-id="c80d8-102">División de tablas</span><span class="sxs-lookup"><span data-stu-id="c80d8-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="c80d8-103">Esta característica es nueva en EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c80d8-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="c80d8-104">EF Core permite asignar dos o más entidades a una sola fila.</span><span class="sxs-lookup"><span data-stu-id="c80d8-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="c80d8-105">Esto se denomina _división de tabla_ o _uso compartido de la tabla_.</span><span class="sxs-lookup"><span data-stu-id="c80d8-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="c80d8-106">Configuración</span><span class="sxs-lookup"><span data-stu-id="c80d8-106">Configuration</span></span>

<span data-ttu-id="c80d8-107">Para usar los tipos de entidad deben asignarse a la misma tabla de la división de tablas, tiene las claves principales que se asignan a las mismas columnas y al menos una relación configurado entre la clave principal de un tipo de entidad y otra en la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="c80d8-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="c80d8-108">Un escenario común para la división de tablas está utilizando solo un subconjunto de las columnas de la tabla para mayor rendimiento o encapsulación.</span><span class="sxs-lookup"><span data-stu-id="c80d8-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="c80d8-109">En este ejemplo `Order` representa un subconjunto de `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="c80d8-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="c80d8-110">Además de la configuración necesaria que llamamos `HasBaseType((string)null)` para evitar la asignación `DetailedOrder` en la misma jerarquía que `Order`.</span><span class="sxs-lookup"><span data-stu-id="c80d8-110">In addition to the required configuration we call `HasBaseType((string)null)` to avoid mapping `DetailedOrder` in the same hierarchy as `Order`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

<span data-ttu-id="c80d8-111">Consulte la [proyecto de ejemplo completa](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) para obtener más contexto.</span><span class="sxs-lookup"><span data-stu-id="c80d8-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="c80d8-112">Uso</span><span class="sxs-lookup"><span data-stu-id="c80d8-112">Usage</span></span>

<span data-ttu-id="c80d8-113">Guardar y consultar entidades mediante la división de tablas se realiza en la misma manera que otras entidades, la única diferencia es que se deben realizar un seguimiento de todas las entidades de uso compartido de una fila para la inserción.</span><span class="sxs-lookup"><span data-stu-id="c80d8-113">Saving and querying entities using table splitting is done in the same way as other entities, the only difference is that all entities sharing a row must be tracked for the insert.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="c80d8-114">Tokens de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="c80d8-114">Concurrency tokens</span></span>

<span data-ttu-id="c80d8-115">Si alguno de los tipos de entidad uso compartido de una tabla tiene un token de simultaneidad, a continuación, se debe incluir en todos los demás tipos de entidad para evitar un valor de token de simultaneidad obsoletos cuando se actualiza solo una de las entidades que se asigna a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="c80d8-115">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="c80d8-116">Para evitar la exposición a que el código utilizado es posible crear uno en estado de la sombra.</span><span class="sxs-lookup"><span data-stu-id="c80d8-116">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]