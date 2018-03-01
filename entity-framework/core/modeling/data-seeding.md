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
# <a name="data-seeding"></a>Propagación de datos

> [!NOTE]  
> Esta característica es nueva en EF Core 2.1.

La propagación de datos permite para proporcionar datos iniciales para rellenar una base de datos. A diferencia de en EF6, en el núcleo de EF, la propagación de datos está asociada a un tipo de entidad como parte de la configuración del modelo. A continuación, migraciones de EF Core automáticamente pueden calcular qué insertar, actualizar o eliminar la necesidad de las operaciones se aplican al actualizar la base de datos a una nueva versión del modelo.

Por ejemplo, puede utilizarla para configurar los datos de inicialización para un `Blog` en `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Para agregar las entidades que tienen una relación de los valores de clave externos debe especificarse. Con frecuencia las propiedades de clave externas se encuentran en estado de instantáneas, por lo que poder establecer los valores de una clase anónima debe usarse:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
