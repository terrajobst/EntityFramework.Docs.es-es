---
title: Guardar los datos - Core EF relacionados
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2018
---
# <a name="saving-related-data"></a><span data-ttu-id="3f723-102">Guardar datos relacionados</span><span class="sxs-lookup"><span data-stu-id="3f723-102">Saving Related Data</span></span>

<span data-ttu-id="3f723-103">Además de las entidades aisladas, también puede hacer que el uso de las relaciones definidas en el modelo.</span><span class="sxs-lookup"><span data-stu-id="3f723-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="3f723-104">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="3f723-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="3f723-105">Adición de un gráfico de nuevas entidades</span><span class="sxs-lookup"><span data-stu-id="3f723-105">Adding a graph of new entities</span></span>

<span data-ttu-id="3f723-106">Si crea varias nuevas entidades relacionadas, agregar uno de ellos en el contexto hará que los demás agregarse demasiado.</span><span class="sxs-lookup"><span data-stu-id="3f723-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="3f723-107">En el ejemplo siguiente, el blog y tres entradas relacionadas se todos insertan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f723-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="3f723-108">Se encontró y se agregan, porque son accesibles a través de las entradas de la `Blog.Posts` propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="3f723-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="3f723-109">Use la propiedad EntityEntry.State para establecer el estado de solo una única entidad.</span><span class="sxs-lookup"><span data-stu-id="3f723-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="3f723-110">Por ejemplo: `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="3f723-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="3f723-111">Agregar una entidad relacionada</span><span class="sxs-lookup"><span data-stu-id="3f723-111">Adding a related entity</span></span>

<span data-ttu-id="3f723-112">Si hace referencia a una nueva entidad de la propiedad de navegación de una entidad que ya está registrada por el contexto, la entidad se detectará y se insertan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f723-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="3f723-113">En el ejemplo siguiente, la `post` se inserta la entidad porque se agrega a la `Posts` propiedad de la `blog` entidad que se capturó desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f723-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="3f723-114">Cambiar las relaciones</span><span class="sxs-lookup"><span data-stu-id="3f723-114">Changing relationships</span></span>

<span data-ttu-id="3f723-115">Si cambia la propiedad de navegación de una entidad, se realizarán los cambios correspondientes a la columna de clave externa en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f723-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="3f723-116">En el ejemplo siguiente, la `post` entidad se actualiza para que pertenezcan a la nueva `blog` entidad porque su `Blog` se establece la propiedad de navegación para que apunte a `blog`.</span><span class="sxs-lookup"><span data-stu-id="3f723-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="3f723-117">Tenga en cuenta que `blog` también se pueden insertar en la base de datos es una nueva entidad que hace referencia la propiedad de navegación de una entidad que ya está registrada por el contexto (`post`).</span><span class="sxs-lookup"><span data-stu-id="3f723-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="3f723-118">Quitar relaciones</span><span class="sxs-lookup"><span data-stu-id="3f723-118">Removing relationships</span></span>

<span data-ttu-id="3f723-119">Puede quitar una relación estableciendo una navegación de referencia en `null`, o quitar la entidad relacionada de navegación de la colección.</span><span class="sxs-lookup"><span data-stu-id="3f723-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="3f723-120">Para eliminar una relación puede tener efectos secundarios en la entidad dependiente, según la cascada eliminar el comportamiento configurado en la relación.</span><span class="sxs-lookup"><span data-stu-id="3f723-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="3f723-121">De forma predeterminada, para las relaciones necesarias, se configura un comportamiento de eliminación en cascada y la entidad secundaria/dependientes se eliminarán de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f723-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="3f723-122">Para las relaciones opcionales, la eliminación en cascada no está configurada de forma predeterminada, pero se establecerá la propiedad de clave externa como null.</span><span class="sxs-lookup"><span data-stu-id="3f723-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="3f723-123">Vea [relaciones obligatorios y opcionales](../modeling/relationships.md#required-and-optional-relationships) para obtener información sobre cómo se puede configurar el requiredness de relaciones.</span><span class="sxs-lookup"><span data-stu-id="3f723-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="3f723-124">Vea [eliminación en cascada](cascade-delete.md) para obtener más detalles sobre la eliminación en cascada comportamientos funciona, cómo se puede configurar explícitamente y cómo se seleccionan por convención.</span><span class="sxs-lookup"><span data-stu-id="3f723-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="3f723-125">En el ejemplo siguiente, se configura una eliminación en cascada en la relación entre `Blog` y `Post`, por lo que el `post` entidad se elimina de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f723-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
