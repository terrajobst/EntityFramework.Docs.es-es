---
title: Propagación de datos - Core EF
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163205"
---
# <a name="data-seeding"></a><span data-ttu-id="5aba2-102">Propagación de datos</span><span class="sxs-lookup"><span data-stu-id="5aba2-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="5aba2-103">Esta característica es nueva en EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="5aba2-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="5aba2-104">La propagación de datos permite para proporcionar datos iniciales para rellenar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="5aba2-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="5aba2-105">A diferencia de en EF6, en el núcleo de EF, la propagación de datos está asociada a un tipo de entidad como parte de la configuración del modelo.</span><span class="sxs-lookup"><span data-stu-id="5aba2-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="5aba2-106">A continuación, EF Core [migraciones](xref:core/managing-schemas/migrations/index) automáticamente puede calcular qué insertar, actualizar o eliminar la necesidad de las operaciones se aplican al actualizar la base de datos a una nueva versión del modelo.</span><span class="sxs-lookup"><span data-stu-id="5aba2-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="5aba2-107">Por ejemplo, puede utilizarla para configurar los datos de inicialización para un `Blog` en `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="5aba2-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="5aba2-108">Para agregar las entidades que tienen una relación de los valores de clave externos debe especificarse.</span><span class="sxs-lookup"><span data-stu-id="5aba2-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="5aba2-109">Con frecuencia las propiedades de clave externas se encuentran en estado de instantáneas, por lo que poder establecer los valores de una clase anónima debe usarse:</span><span class="sxs-lookup"><span data-stu-id="5aba2-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="5aba2-110">Una vez que se han agregado las entidades, se recomienda usar [migraciones](xref:core/managing-schemas/migrations/index) para aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="5aba2-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="5aba2-111">Como alternativa, puede usar `context.Database.EnsureCreated()` para crear una nueva base de datos que contiene los datos de inicialización, por ejemplo para una base de datos de prueba o al usar el proveedor en memoria.</span><span class="sxs-lookup"><span data-stu-id="5aba2-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="5aba2-112">Tenga en cuenta que si ya existe la base de datos, `EnsureCreated()` ni actualizará el esquema ni los datos de inicialización de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5aba2-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
