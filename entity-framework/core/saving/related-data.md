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
# <a name="saving-related-data"></a><span data-ttu-id="1ab77-102">Guardar datos relacionados</span><span class="sxs-lookup"><span data-stu-id="1ab77-102">Saving Related Data</span></span>

<span data-ttu-id="1ab77-103">Además de las entidades aisladas, también puede hacer que el uso de las relaciones definidas en el modelo.</span><span class="sxs-lookup"><span data-stu-id="1ab77-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="1ab77-104">Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="1ab77-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="1ab77-105">Adición de un gráfico de nuevas entidades</span><span class="sxs-lookup"><span data-stu-id="1ab77-105">Adding a graph of new entities</span></span>

<span data-ttu-id="1ab77-106">Si crea varias nuevas entidades relacionadas, agregar uno de ellos en el contexto hará que los demás agregarse demasiado.</span><span class="sxs-lookup"><span data-stu-id="1ab77-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="1ab77-107">En el ejemplo siguiente, el blog y tres entradas relacionadas se todos insertan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ab77-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="1ab77-108">Se encontró y se agregan, porque son accesibles a través de las entradas de la `Blog.Posts` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="1ab77-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

## <a name="adding-a-related-entity"></a><span data-ttu-id="1ab77-109">Agregar una entidad relacionada</span><span class="sxs-lookup"><span data-stu-id="1ab77-109">Adding a related entity</span></span>

<span data-ttu-id="1ab77-110">Si hace referencia a una nueva entidad de la propiedad de navegación de una entidad que ya está registrada por el contexto, la entidad se detectará y se insertan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ab77-110">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="1ab77-111">En el ejemplo siguiente, la `post` se inserta la entidad porque se agrega a la `Posts` propiedad de la `blog` entidad que se capturó desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ab77-111">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="1ab77-112">Cambiar las relaciones</span><span class="sxs-lookup"><span data-stu-id="1ab77-112">Changing relationships</span></span>

<span data-ttu-id="1ab77-113">Si cambia la propiedad de navegación de una entidad, se realizarán los cambios correspondientes a la columna de clave externa en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ab77-113">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="1ab77-114">En el ejemplo siguiente, la `post` entidad se actualiza para que pertenezcan a la nueva `blog` entidad porque su `Blog` se establece la propiedad de navegación para que apunte a `blog`.</span><span class="sxs-lookup"><span data-stu-id="1ab77-114">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="1ab77-115">Tenga en cuenta que `blog` también se pueden insertar en la base de datos es una nueva entidad que hace referencia la propiedad de navegación de una entidad que ya está registrada por el contexto (`post`).</span><span class="sxs-lookup"><span data-stu-id="1ab77-115">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="1ab77-116">Quitar relaciones</span><span class="sxs-lookup"><span data-stu-id="1ab77-116">Removing relationships</span></span>

<span data-ttu-id="1ab77-117">Puede quitar una relación estableciendo una navegación de referencia en `null`, o quitar la entidad relacionada de navegación de la colección.</span><span class="sxs-lookup"><span data-stu-id="1ab77-117">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="1ab77-118">Para eliminar una relación puede tener efectos secundarios en la entidad dependiente, según la cascada eliminar el comportamiento configurado en la relación.</span><span class="sxs-lookup"><span data-stu-id="1ab77-118">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="1ab77-119">De forma predeterminada, para las relaciones necesarias, se configura un comportamiento de eliminación en cascada y la entidad secundaria/dependientes se eliminarán de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ab77-119">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="1ab77-120">Para las relaciones opcionales, la eliminación en cascada no está configurada de forma predeterminada, pero se establecerá la propiedad de clave externa como null.</span><span class="sxs-lookup"><span data-stu-id="1ab77-120">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="1ab77-121">Vea [relaciones obligatorios y opcionales](../modeling/relationships.md#required-and-optional-relationships) para obtener información sobre cómo se puede configurar el requiredness de relaciones.</span><span class="sxs-lookup"><span data-stu-id="1ab77-121">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="1ab77-122">Vea [eliminación en cascada](cascade-delete.md) para obtener más detalles sobre la eliminación en cascada comportamientos funciona, cómo se puede configurar explícitamente y cómo se seleccionan por convención.</span><span class="sxs-lookup"><span data-stu-id="1ab77-122">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="1ab77-123">En el ejemplo siguiente, se configura una eliminación en cascada en la relación entre `Blog` y `Post`, por lo que el `post` entidad se elimina de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1ab77-123">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
