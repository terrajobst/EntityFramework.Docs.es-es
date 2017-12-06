---
title: Guardar los datos - Core EF relacionados
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: 078879163002cb66e0f0f439415789963181ec15
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="saving-related-data"></a>Guardar datos relacionados

Además de las entidades aisladas, también puede hacer que el uso de las relaciones definidas en el modelo.

> [!TIP]  
> Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) en GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Adición de un gráfico de nuevas entidades

Si crea varias nuevas entidades relacionadas, agregar uno de ellos en el contexto hará que los demás agregarse demasiado.

En el ejemplo siguiente, el blog y tres entradas relacionadas se todos insertan en la base de datos. Se encontró y se agregan, porque son accesibles a través de las entradas de la `Blog.Posts` propiedad de navegación.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

## <a name="adding-a-related-entity"></a>Agregar una entidad relacionada

Si hace referencia a una nueva entidad de la propiedad de navegación de una entidad que ya está registrada por el contexto, la entidad se detectará y se insertan en la base de datos.

En el ejemplo siguiente, la `post` se inserta la entidad porque se agrega a la `Posts` propiedad de la `blog` entidad que se capturó desde la base de datos.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Cambiar las relaciones

Si cambia la propiedad de navegación de una entidad, se realizarán los cambios correspondientes a la columna de clave externa en la base de datos.

En el ejemplo siguiente, la `post` entidad se actualiza para que pertenezcan a la nueva `blog` entidad porque su `Blog` se establece la propiedad de navegación para que apunte a `blog`. Tenga en cuenta que `blog` también se pueden insertar en la base de datos es una nueva entidad que hace referencia la propiedad de navegación de una entidad que ya está registrada por el contexto (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Quitar relaciones

Puede quitar una relación estableciendo una navegación de referencia en `null`, o quitar la entidad relacionada de navegación de la colección.

Para eliminar una relación puede tener efectos secundarios en la entidad dependiente, según la cascada eliminar el comportamiento configurado en la relación.

De forma predeterminada, para las relaciones necesarias, se configura un comportamiento de eliminación en cascada y la entidad secundaria/dependientes se eliminarán de la base de datos. Para las relaciones opcionales, la eliminación en cascada no está configurada de forma predeterminada, pero se establecerá la propiedad de clave externa como null.

Vea [relaciones obligatorios y opcionales](../modeling/relationships.md#required-and-optional-relationships) para obtener información sobre cómo se puede configurar el requiredness de relaciones.

Vea [eliminación en cascada](cascade-delete.md) para obtener más detalles sobre la eliminación en cascada comportamientos funciona, cómo se puede configurar explícitamente y cómo se seleccionan por convención.

En el ejemplo siguiente, se configura una eliminación en cascada en la relación entre `Blog` y `Post`, por lo que el `post` entidad se elimina de la base de datos.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
