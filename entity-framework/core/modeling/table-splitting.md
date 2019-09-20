---
title: 'División de tablas: EF Core'
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 684fcfbb66debfd1b89e23c8aaf0a32909378c6b
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149183"
---
# <a name="table-splitting"></a><span data-ttu-id="cc8e8-102">División de tabla</span><span class="sxs-lookup"><span data-stu-id="cc8e8-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="cc8e8-103">Esta característica es nueva en EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="cc8e8-104">EF Core permite asignar dos o más entidades a una sola fila.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="cc8e8-105">Esto se denomina _División de tablas_ o _uso compartido de tablas_.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="cc8e8-106">Configuración</span><span class="sxs-lookup"><span data-stu-id="cc8e8-106">Configuration</span></span>

<span data-ttu-id="cc8e8-107">Para usar la división de tablas, los tipos de entidad deben asignarse a la misma tabla, tener las claves principales asignadas a las mismas columnas y al menos una relación configurada entre la clave principal de un tipo de entidad y otra en la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="cc8e8-108">Un escenario común para la división de tablas es usar solo un subconjunto de las columnas de la tabla para un mayor rendimiento o encapsulación.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="cc8e8-109">En este ejemplo `Order` representa un subconjunto `DetailedOrder`de.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="cc8e8-110">Además de la configuración necesaria, `Property(o => o.Status).HasColumnName("Status")` llamamos a para asignarla `DetailedOrder.Status` a la misma columna `Order.Status`que.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="cc8e8-111">Vea el [proyecto de ejemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) para obtener más contexto.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="cc8e8-112">Uso</span><span class="sxs-lookup"><span data-stu-id="cc8e8-112">Usage</span></span>

<span data-ttu-id="cc8e8-113">Guardar y consultar entidades mediante la división de tablas se realiza de la misma manera que otras entidades.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-113">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="cc8e8-114">Y, a partir de EF Core 3,0, la referencia de `null`entidad dependiente puede ser.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-114">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="cc8e8-115">Si todas las columnas utilizadas por la entidad dependiente son `NULL` la base de datos, no se creará ninguna instancia para ella cuando se realice la consulta.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-115">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="cc8e8-116">Esto también ocurre cuando todas las propiedades son opcionales y se establecen en `null`, lo que podría no ser el esperado.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-116">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="cc8e8-117">Tokens de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="cc8e8-117">Concurrency tokens</span></span>

<span data-ttu-id="cc8e8-118">Si alguno de los tipos de entidad que comparten una tabla tiene un token de simultaneidad, debe estar incluido en todos los demás tipos de entidad para evitar un valor de token de simultaneidad obsoleto cuando solo se actualiza una de las entidades asignadas a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-118">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="cc8e8-119">Para evitar que se exponga al código de consumo, es posible crear uno en el estado de sombra.</span><span class="sxs-lookup"><span data-stu-id="cc8e8-119">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]