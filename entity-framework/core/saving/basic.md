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
# <a name="basic-save"></a>Guardar básica

Obtenga información acerca de cómo agregar, modificar y eliminar datos mediante las clases de contexto y la entidad.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) de este artículo en GitHub.

## <a name="adding-data"></a>Agregar datos

Use la *DbSet.Add* método para agregar nuevas instancias de las clases de entidad. Los datos se insertará en la base de datos cuando se llama a *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Los métodos Add, adjuntar y actualizar todo el trabajo en el gráfico completo de entidades que se les pasan, tal y como se describe en el [datos relacionados](related-data.md) sección. Como alternativa, la propiedad EntityEntry.State puede utilizarse para establecer el estado de solo una única entidad. Por ejemplo: `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Actualizar datos

EF detectará automáticamente los cambios realizados en una entidad existente que se realiza un seguimiento por el contexto. Esto incluye las entidades que carga o consulta de la base de datos y entidades que se han agregado previamente y se guardan en la base de datos.

Basta con modificar los valores asignados a las propiedades y, a continuación, llame a *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Eliminar datos

Use la *DbSet.Remove* método para eliminar instancias de que las clases de entidad.

Si la entidad ya existe en la base de datos, se eliminarán durante la *SaveChanges*. Si la entidad aún no se han guardado en la base de datos (es decir, se realiza un seguimiento a medida que agregan) a continuación, se quitará el contexto y ya no estará cuando inserta *SaveChanges* se llama.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Varias operaciones en una sola SaveChanges

Puede combinar varias operaciones de agregar, actualizar o quitar en una sola llamada a *SaveChanges*.

> [!NOTE]  
> La mayoría de los proveedores de base de datos, *SaveChanges* es transaccional. Esto significa que todas las operaciones se ejecutan correcta o incorrectamente y las operaciones no se nunca izquierda parcialmente aplicará.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
