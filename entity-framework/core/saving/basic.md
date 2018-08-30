---
title: 'Guardado básico: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: ecf8f344a5baae37a5e7255a4affb1085f1b3ff3
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "42447814"
---
# <a name="basic-save"></a><span data-ttu-id="e3df3-102">Guardado básico</span><span class="sxs-lookup"><span data-stu-id="e3df3-102">Basic Save</span></span>

<span data-ttu-id="e3df3-103">Obtenga información sobre cómo agregar, modificar y quitar datos mediante las clases de entidad y contexto.</span><span class="sxs-lookup"><span data-stu-id="e3df3-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="e3df3-104">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="e3df3-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="e3df3-105">Agregar datos</span><span class="sxs-lookup"><span data-stu-id="e3df3-105">Adding Data</span></span>

<span data-ttu-id="e3df3-106">Use el método *DbSet.Add* para agregar instancias nuevas de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="e3df3-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="e3df3-107">Los datos se insertarán en la base de datos cuando llame a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="e3df3-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="e3df3-108">Los métodos Add, Attach y Update funcionan en todo el grafo de entidades que se pasaron a ellos, tal como se describe en la sección de [datos relacionados](related-data.md).</span><span class="sxs-lookup"><span data-stu-id="e3df3-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="e3df3-109">También puede usar la propiedad EntityEntry.State para establecer el estado de una sola unidad.</span><span class="sxs-lookup"><span data-stu-id="e3df3-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="e3df3-110">Por ejemplo: `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="e3df3-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="e3df3-111">Actualizar datos</span><span class="sxs-lookup"><span data-stu-id="e3df3-111">Updating Data</span></span>

<span data-ttu-id="e3df3-112">EF detectará automáticamente los cambios hechos en una entidad existente de la que hace seguimiento el contexto.</span><span class="sxs-lookup"><span data-stu-id="e3df3-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="e3df3-113">Esto incluye entidades que se cargan o consultan desde la base de datos y las entidades que se agregaron y guardaron anteriormente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e3df3-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="e3df3-114">Solo debe modificar los valores asignados a las propiedades y llamar a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="e3df3-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="e3df3-115">Eliminar datos</span><span class="sxs-lookup"><span data-stu-id="e3df3-115">Deleting Data</span></span>

<span data-ttu-id="e3df3-116">Use el método *DbSet.Remove* para eliminar las instancias de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="e3df3-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="e3df3-117">Si la entidad ya existe en la base de datos, se eliminará durante *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="e3df3-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="e3df3-118">Si la entidad todavía no se guarda en la base de datos (es decir, si se hace seguimiento cuando se agrega), se quitará del contexto y ya no se insertará cuando se llame a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="e3df3-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="e3df3-119">Varias operaciones en una única llamada a SaveChanges</span><span class="sxs-lookup"><span data-stu-id="e3df3-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="e3df3-120">Puede combinar varias operaciones Add, Update y Remove en una sola llamada a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="e3df3-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="e3df3-121">Para la mayoría de los proveedores de base de datos, *SaveChanges* es transaccional.</span><span class="sxs-lookup"><span data-stu-id="e3df3-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="e3df3-122">Esto significa que todas las operaciones se realizarán correctamente o presentarán un error y que nunca se aplicarán de manera parcial.</span><span class="sxs-lookup"><span data-stu-id="e3df3-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
