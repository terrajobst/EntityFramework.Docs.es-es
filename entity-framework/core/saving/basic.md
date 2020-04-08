---
title: 'Guardado básico: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413688"
---
# <a name="basic-save"></a><span data-ttu-id="aaca1-102">Guardado básico</span><span class="sxs-lookup"><span data-stu-id="aaca1-102">Basic Save</span></span>

<span data-ttu-id="aaca1-103">Obtenga información sobre cómo agregar, modificar y quitar datos mediante las clases de entidad y contexto.</span><span class="sxs-lookup"><span data-stu-id="aaca1-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="aaca1-104">Puede ver un [ejemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="aaca1-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="aaca1-105">Agregar datos</span><span class="sxs-lookup"><span data-stu-id="aaca1-105">Adding Data</span></span>

<span data-ttu-id="aaca1-106">Use el método *DbSet.Add* para agregar instancias nuevas de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="aaca1-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="aaca1-107">Los datos se insertarán en la base de datos cuando llame a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="aaca1-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="aaca1-108">Los métodos Add, Attach y Update funcionan en todo el grafo de entidades que se pasaron a ellos, tal como se describe en la sección de [datos relacionados](related-data.md).</span><span class="sxs-lookup"><span data-stu-id="aaca1-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="aaca1-109">También puede usar la propiedad EntityEntry.State para establecer el estado de una sola unidad.</span><span class="sxs-lookup"><span data-stu-id="aaca1-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="aaca1-110">Por ejemplo, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="aaca1-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="aaca1-111">Actualización de datos</span><span class="sxs-lookup"><span data-stu-id="aaca1-111">Updating Data</span></span>

<span data-ttu-id="aaca1-112">EF detectará automáticamente los cambios hechos en una entidad existente de la que hace seguimiento el contexto.</span><span class="sxs-lookup"><span data-stu-id="aaca1-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="aaca1-113">Esto incluye entidades que se cargan o consultan desde la base de datos y las entidades que se agregaron y guardaron anteriormente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="aaca1-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="aaca1-114">Solo debe modificar los valores asignados a las propiedades y llamar a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="aaca1-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="aaca1-115">Eliminar datos</span><span class="sxs-lookup"><span data-stu-id="aaca1-115">Deleting Data</span></span>

<span data-ttu-id="aaca1-116">Use el método *DbSet.Remove* para eliminar las instancias de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="aaca1-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="aaca1-117">Si la entidad ya existe en la base de datos, se eliminará durante *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="aaca1-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="aaca1-118">Si la entidad todavía no se guarda en la base de datos (es decir, si se hace seguimiento cuando se agrega), se quitará del contexto y ya no se insertará cuando se llame a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="aaca1-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="aaca1-119">Varias operaciones en una única llamada a SaveChanges</span><span class="sxs-lookup"><span data-stu-id="aaca1-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="aaca1-120">Puede combinar varias operaciones Add, Update y Remove en una sola llamada a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="aaca1-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="aaca1-121">Para la mayoría de los proveedores de base de datos, *SaveChanges* es transaccional.</span><span class="sxs-lookup"><span data-stu-id="aaca1-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="aaca1-122">Esto significa que todas las operaciones se realizarán correctamente o presentarán un error y que nunca se aplicarán de manera parcial.</span><span class="sxs-lookup"><span data-stu-id="aaca1-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
