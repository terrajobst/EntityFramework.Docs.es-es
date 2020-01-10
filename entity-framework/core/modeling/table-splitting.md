---
title: 'División de tablas: EF Core'
description: Cómo configurar la división de tablas mediante Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: c38d3ee0efa82f84a1051017ae40c9f3fdd57f1f
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781175"
---
# <a name="table-splitting"></a><span data-ttu-id="08b18-103">División de tablas</span><span class="sxs-lookup"><span data-stu-id="08b18-103">Table Splitting</span></span>

<span data-ttu-id="08b18-104">EF Core permite asignar dos o más entidades a una sola fila.</span><span class="sxs-lookup"><span data-stu-id="08b18-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="08b18-105">Esto se denomina _División de tablas_ o _uso compartido de tablas_.</span><span class="sxs-lookup"><span data-stu-id="08b18-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="08b18-106">Configuración de</span><span class="sxs-lookup"><span data-stu-id="08b18-106">Configuration</span></span>

<span data-ttu-id="08b18-107">Para usar la división de tablas, los tipos de entidad deben asignarse a la misma tabla, tener las claves principales asignadas a las mismas columnas y al menos una relación configurada entre la clave principal de un tipo de entidad y otra en la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="08b18-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="08b18-108">Un escenario común para la división de tablas es usar solo un subconjunto de las columnas de la tabla para un mayor rendimiento o encapsulación.</span><span class="sxs-lookup"><span data-stu-id="08b18-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="08b18-109">En este ejemplo `Order` representa un subconjunto de `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="08b18-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="08b18-110">Además de la configuración necesaria, llamamos `Property(o => o.Status).HasColumnName("Status")` para asignar `DetailedOrder.Status` a la misma columna que `Order.Status`.</span><span class="sxs-lookup"><span data-stu-id="08b18-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> <span data-ttu-id="08b18-111">Vea el [proyecto de ejemplo completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) para obtener más contexto.</span><span class="sxs-lookup"><span data-stu-id="08b18-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="08b18-112">Usage</span><span class="sxs-lookup"><span data-stu-id="08b18-112">Usage</span></span>

<span data-ttu-id="08b18-113">Guardar y consultar entidades mediante la división de tablas se realiza de la misma manera que otras entidades:</span><span class="sxs-lookup"><span data-stu-id="08b18-113">Saving and querying entities using table splitting is done in the same way as other entities:</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a><span data-ttu-id="08b18-114">Entidad dependiente opcional</span><span class="sxs-lookup"><span data-stu-id="08b18-114">Optional dependent entity</span></span>

> [!NOTE]
> <span data-ttu-id="08b18-115">Esta característica se presentó en EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="08b18-115">This feature was introduced in EF Core 3.0.</span></span>

<span data-ttu-id="08b18-116">Si todas las columnas utilizadas por una entidad dependiente están `NULL` en la base de datos, no se creará ninguna instancia para ella cuando se realice la consulta.</span><span class="sxs-lookup"><span data-stu-id="08b18-116">If all of the columns used by a dependent entity are `NULL` in the database, then no instance for it will be created when queried.</span></span> <span data-ttu-id="08b18-117">Esto permite el modelado de una entidad dependiente opcional, donde la propiedad Relationship de la entidad de seguridad sería null.</span><span class="sxs-lookup"><span data-stu-id="08b18-117">This allows modeling an optional dependent entity, where the relationship property on the principal would be null.</span></span> <span data-ttu-id="08b18-118">Tenga en cuenta que esto también ocurrirá si todas las propiedades del dependiente son opcionales y se establecen en `null`, lo que podría no ser el esperado.</span><span class="sxs-lookup"><span data-stu-id="08b18-118">Note that This would also happen all of the dependent's properties are optional and set to `null`, which might not be expected.</span></span>

## <a name="concurrency-tokens"></a><span data-ttu-id="08b18-119">Tokens de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="08b18-119">Concurrency tokens</span></span>

<span data-ttu-id="08b18-120">Si alguno de los tipos de entidad que comparten una tabla tiene un token de simultaneidad, también debe incluirse en todos los demás tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="08b18-120">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types as well.</span></span> <span data-ttu-id="08b18-121">Esto es necesario para evitar un valor de token de simultaneidad obsoleto cuando solo se actualiza una de las entidades asignadas a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="08b18-121">This is necessary in order to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="08b18-122">Para evitar exponer el token de simultaneidad al código utilizado, es posible crear uno como una [propiedad de sombra](xref:core/modeling/shadow-properties):</span><span class="sxs-lookup"><span data-stu-id="08b18-122">To avoid exposing the concurrency token to the consuming code, it's possible the create one as a [shadow property](xref:core/modeling/shadow-properties):</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
