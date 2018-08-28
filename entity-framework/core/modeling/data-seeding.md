---
title: Propagación de datos - EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994484"
---
# <a name="data-seeding"></a>Propagación de datos

> [!NOTE]  
> Esta característica es nueva en EF Core 2.1.

Propagación de datos permite para proporcionar datos iniciales para rellenar una base de datos. A diferencia de en EF6 en EF Core, la propagación de datos está asociada con un tipo de entidad como parte de la configuración del modelo. A continuación, EF Core [migraciones](xref:core/managing-schemas/migrations/index) puede calcular automáticamente qué insertar, actualizar o eliminar la necesidad de las operaciones que se aplicará al actualizar la base de datos a una nueva versión del modelo.

Por ejemplo, puede utilizarlo para configurar datos de inicialización para un `Blog` en `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Para agregar las entidades que tienen una relación de los valores de clave externos debe especificarse. Con frecuencia las propiedades de clave externas están en estado de la sombra, por lo que poder establecer los valores de una clase anónima debe usarse:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Una vez que se han agregado las entidades, se recomienda usar [migraciones](xref:core/managing-schemas/migrations/index) para aplicar los cambios. 

Como alternativa, puede usar `context.Database.EnsureCreated()` para crear una nueva base de datos que contiene los datos de inicialización, por ejemplo para una base de datos de prueba o al usar el proveedor en memoria. Observe que si ya existe la base de datos, `EnsureCreated()` ni actualizará el esquema ni los datos de inicialización en la base de datos.
