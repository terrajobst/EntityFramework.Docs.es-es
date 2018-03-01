---
title: "Propagación de datos - Core EF"
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a><span data-ttu-id="04e88-102">Propagación de datos</span><span class="sxs-lookup"><span data-stu-id="04e88-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="04e88-103">Esta característica es nueva en EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="04e88-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="04e88-104">La propagación de datos permite para proporcionar datos iniciales para rellenar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="04e88-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="04e88-105">A diferencia de en EF6, en el núcleo de EF, la propagación de datos está asociada a un tipo de entidad como parte de la configuración del modelo.</span><span class="sxs-lookup"><span data-stu-id="04e88-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="04e88-106">A continuación, migraciones de EF Core automáticamente pueden calcular qué insertar, actualizar o eliminar la necesidad de las operaciones se aplican al actualizar la base de datos a una nueva versión del modelo.</span><span class="sxs-lookup"><span data-stu-id="04e88-106">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="04e88-107">Por ejemplo, puede utilizarla para configurar los datos de inicialización para un `Blog` en `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="04e88-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="04e88-108">Para agregar las entidades que tienen una relación de los valores de clave externos debe especificarse.</span><span class="sxs-lookup"><span data-stu-id="04e88-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="04e88-109">Con frecuencia las propiedades de clave externas se encuentran en estado de instantáneas, por lo que poder establecer los valores de una clase anónima debe usarse:</span><span class="sxs-lookup"><span data-stu-id="04e88-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
