---
title: 'Guardado de datos relacionados: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2018
ms.locfileid: "31006655"
---
# <a name="saving-related-data"></a><span data-ttu-id="acbf0-102">Guardado de datos relacionados</span><span class="sxs-lookup"><span data-stu-id="acbf0-102">Saving Related Data</span></span>

<span data-ttu-id="acbf0-103">Además de las entidades aisladas, también puede usar las relaciones definidas en el modelo.</span><span class="sxs-lookup"><span data-stu-id="acbf0-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="acbf0-104">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="acbf0-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="acbf0-105">Incorporación de un grafo de entidades nuevas</span><span class="sxs-lookup"><span data-stu-id="acbf0-105">Adding a graph of new entities</span></span>

<span data-ttu-id="acbf0-106">Si crea varias entidades relacionadas, agregar una de ellas al contexto hará que las otras también se agreguen.</span><span class="sxs-lookup"><span data-stu-id="acbf0-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="acbf0-107">En el ejemplo siguiente, el blog y tres entradas relacionadas se insertan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="acbf0-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="acbf0-108">Las entradas se buscan y agregan, porque son accesibles a través de la propiedad de navegación `Blog.Posts`.</span><span class="sxs-lookup"><span data-stu-id="acbf0-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="acbf0-109">Use la propiedad EntityEntry.State para establecer el estado de una sola unidad.</span><span class="sxs-lookup"><span data-stu-id="acbf0-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="acbf0-110">Por ejemplo: `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="acbf0-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="acbf0-111">Incorporación de una entidad relacionada</span><span class="sxs-lookup"><span data-stu-id="acbf0-111">Adding a related entity</span></span>

<span data-ttu-id="acbf0-112">Si hace referencia a una entidad nueva desde la propiedad de navegación de una entidad a la que el contexto ya hace seguimiento, se detectará la entidad y se insertará en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="acbf0-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="acbf0-113">En el ejemplo siguiente, la entidad `post` se inserta porque se agrega a la propiedad `Posts` de la entidad `blog` que se capturó de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="acbf0-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="acbf0-114">Cambio de las relaciones</span><span class="sxs-lookup"><span data-stu-id="acbf0-114">Changing relationships</span></span>

<span data-ttu-id="acbf0-115">Si cambia la propiedad de navegación de una entidad, los cambios correspondientes se harán en la columna de clave externa de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="acbf0-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="acbf0-116">En el ejemplo siguiente, la entidad `post` se actualiza para que pertenezca a la entidad `blog` nueva, porque su propiedad de navegación `Blog` está establecida para que apunte a `blog`.</span><span class="sxs-lookup"><span data-stu-id="acbf0-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="acbf0-117">Tenga en cuenta que `blog` también se insertará en la base de datos porque se trata de una entidad nueva a la que hace referencia la propiedad de navegación de una entidad de la que el contexto (`post`) ya hace seguimiento.</span><span class="sxs-lookup"><span data-stu-id="acbf0-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="acbf0-118">Eliminación de relaciones</span><span class="sxs-lookup"><span data-stu-id="acbf0-118">Removing relationships</span></span>

<span data-ttu-id="acbf0-119">Para quitar una relación, establezca una navegación de referencia en `null` o quite la entidad relacionada de una navegación de colección.</span><span class="sxs-lookup"><span data-stu-id="acbf0-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="acbf0-120">Quitar una relación puede tener efectos secundarios en la entidad dependiente, según el comportamiento de eliminación en cascada que esté configurado en la relación.</span><span class="sxs-lookup"><span data-stu-id="acbf0-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="acbf0-121">De manera predeterminada, en el caso de las relaciones obligatorias, hay configurado un comportamiento de eliminación en cascada y la entidad secundaria o dependiente se eliminará de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="acbf0-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="acbf0-122">En el caso de las relaciones opcionales, no hay configurada una eliminación en cascada de manera predeterminada, pero la propiedad de clave externa se establecerá en NULL.</span><span class="sxs-lookup"><span data-stu-id="acbf0-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="acbf0-123">Consulte la sección sobre las [relaciones obligatorias y opcionales](../modeling/relationships.md#required-and-optional-relationships) para más información sobre cómo se puede configurar la obligatoriedad de las relaciones.</span><span class="sxs-lookup"><span data-stu-id="acbf0-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="acbf0-124">Consulte el artículo sobre la [eliminación en cascada](cascade-delete.md) para más detalles sobre el funcionamiento de los comportamientos de eliminación en cascada, cómo se pueden configurar de manera explícita y cómo se seleccionan por convención.</span><span class="sxs-lookup"><span data-stu-id="acbf0-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="acbf0-125">En el ejemplo siguiente, se configura una eliminación en cascada en la relación entre `Blog` y `Post`, por lo que la entidad `post` se elimina de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="acbf0-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
