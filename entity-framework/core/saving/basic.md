---
title: "Guardar básica - Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: e99d755b8f7c42b15a73cfdb6a2f8999a405a739
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="basic-save"></a><span data-ttu-id="980a4-102">Guardar básica</span><span class="sxs-lookup"><span data-stu-id="980a4-102">Basic Save</span></span>

<span data-ttu-id="980a4-103">Obtenga información acerca de cómo agregar, modificar y eliminar datos mediante las clases de contexto y la entidad.</span><span class="sxs-lookup"><span data-stu-id="980a4-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="980a4-104">Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="980a4-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="980a4-105">Agregar datos</span><span class="sxs-lookup"><span data-stu-id="980a4-105">Adding Data</span></span>

<span data-ttu-id="980a4-106">Use la *DbSet.Add* método para agregar nuevas instancias de las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="980a4-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="980a4-107">Los datos se insertará en la base de datos cuando se llama a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="980a4-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

## <a name="updating-data"></a><span data-ttu-id="980a4-108">Actualizar datos</span><span class="sxs-lookup"><span data-stu-id="980a4-108">Updating Data</span></span>

<span data-ttu-id="980a4-109">EF detectará automáticamente los cambios realizados en una entidad existente que se realiza un seguimiento por el contexto.</span><span class="sxs-lookup"><span data-stu-id="980a4-109">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="980a4-110">Esto incluye las entidades que carga o consulta de la base de datos y entidades que se han agregado previamente y se guardan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="980a4-110">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="980a4-111">Basta con modificar los valores asignados a las propiedades y, a continuación, llame a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="980a4-111">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="980a4-112">Eliminar datos</span><span class="sxs-lookup"><span data-stu-id="980a4-112">Deleting Data</span></span>

<span data-ttu-id="980a4-113">Use la *DbSet.Remove* método para eliminar instancias de que las clases de entidad.</span><span class="sxs-lookup"><span data-stu-id="980a4-113">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="980a4-114">Si la entidad ya existe en la base de datos, se eliminarán durante la *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="980a4-114">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="980a4-115">Si la entidad aún no se han guardado en la base de datos (es decir, se realiza un seguimiento a medida que agregan) a continuación, se quitará el contexto y ya no estará cuando inserta *SaveChanges* se llama.</span><span class="sxs-lookup"><span data-stu-id="980a4-115">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="980a4-116">Varias operaciones en una sola SaveChanges</span><span class="sxs-lookup"><span data-stu-id="980a4-116">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="980a4-117">Puede combinar varias operaciones de agregar, actualizar o quitar en una sola llamada a *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="980a4-117">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="980a4-118">La mayoría de los proveedores de base de datos, *SaveChanges* es transaccional.</span><span class="sxs-lookup"><span data-stu-id="980a4-118">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="980a4-119">Esto significa que todas las operaciones se ejecutan correcta o incorrectamente y las operaciones no se nunca izquierda parcialmente aplicará.</span><span class="sxs-lookup"><span data-stu-id="980a4-119">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
