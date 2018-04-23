---
title: Guardar básica - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a><span data-ttu-id="e4764-102">Guardar básica</span><span class="sxs-lookup"><span data-stu-id="e4764-102">Basic Save</span></span>

<span data-ttu-id="e4764-103">Obtenga información acerca de cómo agregar, modificar y eliminar datos mediante las clases de contexto y la entidad.</span><span class="sxs-lookup"><span data-stu-id="e4764-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="e4764-104">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="e4764-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="e4764-105">Agregar datos</span><span class="sxs-lookup"><span data-stu-id="e4764-105">Adding Data</span></span>

<span data-ttu-id="e4764-106">Use la *DbSet.Add* método para agregar nuevas instancias de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="e4764-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="e4764-107">Los datos se insertará en la base de datos cuando se llama a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="e4764-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="e4764-108">Los métodos Add, adjuntar y actualizar todo el trabajo en el gráfico completo de entidades que se les pasan, tal y como se describe en el [datos relacionados](related-data.md) sección.</span><span class="sxs-lookup"><span data-stu-id="e4764-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="e4764-109">Como alternativa, la propiedad EntityEntry.State puede utilizarse para establecer el estado de solo una única entidad.</span><span class="sxs-lookup"><span data-stu-id="e4764-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="e4764-110">Por ejemplo: `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="e4764-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="e4764-111">Actualizar datos</span><span class="sxs-lookup"><span data-stu-id="e4764-111">Updating Data</span></span>

<span data-ttu-id="e4764-112">EF detectará automáticamente los cambios realizados en una entidad existente que se realiza un seguimiento por el contexto.</span><span class="sxs-lookup"><span data-stu-id="e4764-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="e4764-113">Esto incluye las entidades que carga o consulta de la base de datos y entidades que se han agregado previamente y se guardan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e4764-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="e4764-114">Basta con modificar los valores asignados a las propiedades y, a continuación, llame a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="e4764-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="e4764-115">Eliminar datos</span><span class="sxs-lookup"><span data-stu-id="e4764-115">Deleting Data</span></span>

<span data-ttu-id="e4764-116">Use la *DbSet.Remove* método para eliminar instancias de que las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="e4764-116">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="e4764-117">Si la entidad ya existe en la base de datos, se eliminarán durante la *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="e4764-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="e4764-118">Si la entidad aún no se han guardado en la base de datos (es decir, se realiza un seguimiento a medida que agregan) a continuación, se quitará el contexto y ya no estará cuando inserta *SaveChanges* se llama.</span><span class="sxs-lookup"><span data-stu-id="e4764-118">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="e4764-119">Varias operaciones en una sola SaveChanges</span><span class="sxs-lookup"><span data-stu-id="e4764-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="e4764-120">Puede combinar varias operaciones de agregar, actualizar o quitar en una sola llamada a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="e4764-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="e4764-121">La mayoría de los proveedores de base de datos, *SaveChanges* es transaccional.</span><span class="sxs-lookup"><span data-stu-id="e4764-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="e4764-122">Esto significa que todas las operaciones se ejecutan correcta o incorrectamente y las operaciones no se nunca izquierda parcialmente aplicará.</span><span class="sxs-lookup"><span data-stu-id="e4764-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
