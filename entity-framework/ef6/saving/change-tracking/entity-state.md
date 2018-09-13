---
title: Trabajar con estados de entidad - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: c1dde7810d1dfa8a73e6bd2cf091b24be6b865d8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490677"
---
# <a name="working-with-entity-states"></a><span data-ttu-id="22274-102">Trabajar con estados de entidad</span><span class="sxs-lookup"><span data-stu-id="22274-102">Working with entity states</span></span>
<span data-ttu-id="22274-103">Este tema tratará cómo agregar y asociar entidades a un contexto y cómo Entity Framework procesa estos durante SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="22274-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="22274-104">Entity Framework se encarga de debe pertenecer el estado de entidades mientras están conectados a un contexto, pero en escenarios sin conexión o de N niveles, puede informar a EF qué estado de las entidades de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="22274-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="22274-105">Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.</span><span class="sxs-lookup"><span data-stu-id="22274-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="22274-106">Estados de entidad y SaveChanges</span><span class="sxs-lookup"><span data-stu-id="22274-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="22274-107">Una entidad puede estar en uno de los cinco estados tal como se define por la enumeración EntityState.</span><span class="sxs-lookup"><span data-stu-id="22274-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="22274-108">Estos estados son:</span><span class="sxs-lookup"><span data-stu-id="22274-108">These states are:</span></span>  

- <span data-ttu-id="22274-109">Se ha agregado: la entidad realiza el seguimiento del contexto, pero aún no existe en la base de datos</span><span class="sxs-lookup"><span data-stu-id="22274-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="22274-110">Sin cambios: la entidad que se realiza el seguimiento del contexto y existe en la base de datos y no han cambiado sus valores de propiedad de los valores de la base de datos</span><span class="sxs-lookup"><span data-stu-id="22274-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="22274-111">Modificado: la entidad que se realiza el seguimiento del contexto y existe en la base de datos, y se han modificado algunos o todos sus valores de propiedad</span><span class="sxs-lookup"><span data-stu-id="22274-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="22274-112">Eliminado: la entidad está realizando el contexto y existe en la base de datos, pero se ha marcado para su eliminación de la base de datos la próxima vez que se llama a SaveChanges</span><span class="sxs-lookup"><span data-stu-id="22274-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="22274-113">Desasociado: la entidad no se realiza el seguimiento del contexto</span><span class="sxs-lookup"><span data-stu-id="22274-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="22274-114">SaveChanges hace cosas diferentes para las entidades en estados diferentes:</span><span class="sxs-lookup"><span data-stu-id="22274-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="22274-115">Las entidades sin cambios no se vinculan a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="22274-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="22274-116">Las actualizaciones no se envían a la base de datos para las entidades en estado Unchanged.</span><span class="sxs-lookup"><span data-stu-id="22274-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="22274-117">Agrega las entidades se insertan en la base de datos y, a continuación, se convierten en no se modifican cuando SaveChanges se devuelve.</span><span class="sxs-lookup"><span data-stu-id="22274-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="22274-118">Entidades modificadas se actualizan en la base de datos y, a continuación, se convierten en no se modifican cuando SaveChanges se devuelve.</span><span class="sxs-lookup"><span data-stu-id="22274-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="22274-119">Entidades eliminadas se eliminan de la base de datos y, a continuación, se desasocian del contexto.</span><span class="sxs-lookup"><span data-stu-id="22274-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="22274-120">Los ejemplos siguientes muestran formas en que se puede cambiar el estado de una entidad o un gráfico de la entidad.</span><span class="sxs-lookup"><span data-stu-id="22274-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="22274-121">Agregar una nueva entidad al contexto</span><span class="sxs-lookup"><span data-stu-id="22274-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="22274-122">Una nueva entidad se puede agregar al contexto llamando al método Add en DbSet.</span><span class="sxs-lookup"><span data-stu-id="22274-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="22274-123">Esto coloca la entidad en el estado Added, lo que significa que se va a insertar en la base de datos la próxima vez que se llama a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="22274-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="22274-124">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22274-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="22274-125">Otra manera de agregar una nueva entidad al contexto es cambiar su estado en Added.</span><span class="sxs-lookup"><span data-stu-id="22274-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="22274-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22274-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="22274-127">Por último, puede agregar una nueva entidad al contexto enlazando hasta otra entidad que ya se está realizando un seguimiento.</span><span class="sxs-lookup"><span data-stu-id="22274-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="22274-128">Esto podría ser agregando la nueva entidad a la propiedad de navegación de colección de otra entidad o estableciendo una propiedad de navegación de referencia de otra entidad para que apunte a la nueva entidad.</span><span class="sxs-lookup"><span data-stu-id="22274-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="22274-129">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22274-129">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

<span data-ttu-id="22274-130">Tenga en cuenta que para todos estos ejemplos si la entidad que se va a agregar tiene referencias a otras entidades que no están todavía realiza un seguimiento, a continuación, estas nuevas entidades también se agregará al contexto y se insertará en la base de datos la próxima vez que se llama a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="22274-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="22274-131">Asociar una entidad existente en el contexto</span><span class="sxs-lookup"><span data-stu-id="22274-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="22274-132">Si tiene una entidad que ya sabe que existe en la base de datos pero que no actualmente que se realiza el seguimiento del contexto, a continuación, puede indicar el contexto para realizar el seguimiento de la entidad mediante el método Attach en DbSet.</span><span class="sxs-lookup"><span data-stu-id="22274-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="22274-133">La entidad estará en el estado sin modificar en el contexto.</span><span class="sxs-lookup"><span data-stu-id="22274-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="22274-134">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22274-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="22274-135">Tenga en cuenta que no se realizará ningún cambio a la base de datos si se llama a SaveChanges sin tener que realizar cualquier otra manipulación de la entidad adjunta.</span><span class="sxs-lookup"><span data-stu-id="22274-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="22274-136">Esto es porque la entidad está en estado Unchanged.</span><span class="sxs-lookup"><span data-stu-id="22274-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="22274-137">Otra manera de asociar una entidad existente en el contexto es cambiar su estado en Unchanged.</span><span class="sxs-lookup"><span data-stu-id="22274-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="22274-138">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22274-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="22274-139">Tenga en cuenta que para ambos ejemplos si la entidad que se va a adjuntar tiene referencias a otras entidades que no se hace seguimiento, a continuación, estas nuevas entidades también adjuntará al contexto en estado Unchanged.</span><span class="sxs-lookup"><span data-stu-id="22274-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="22274-140">Asociar una existente, pero la entidad modificada en el contexto</span><span class="sxs-lookup"><span data-stu-id="22274-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="22274-141">Si tiene una entidad que ya sabe que existe en la base de datos, pero que es posible que se realizaron cambios, a continuación, puede indicar el contexto para asociar la entidad y establecer su estado en Modified.</span><span class="sxs-lookup"><span data-stu-id="22274-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="22274-142">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22274-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="22274-143">Cuando se cambia el estado en Modified todas las propiedades de la entidad se marcarán como modificados y todos los valores de propiedad se enviará a la base de datos cuando se llama a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="22274-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="22274-144">Tenga en cuenta que si la entidad que se va a adjuntar tiene referencias a otras entidades que no se hace seguimiento, a continuación, estas nuevas entidades adjuntará al contexto en estado Unchanged, no estará automáticamente Modified.</span><span class="sxs-lookup"><span data-stu-id="22274-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="22274-145">Si tiene varias entidades que deben marcarse como Modified debe establecer el estado individualmente para cada una de estas entidades.</span><span class="sxs-lookup"><span data-stu-id="22274-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="22274-146">Cambiar el estado de una entidad con seguimiento</span><span class="sxs-lookup"><span data-stu-id="22274-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="22274-147">Puede cambiar el estado de una entidad que se estuvieran rastreando estableciendo la propiedad de estado en su entrada.</span><span class="sxs-lookup"><span data-stu-id="22274-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="22274-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22274-148">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="22274-149">Tenga en cuenta que una llamada a Add o Attach para una entidad que ya se realiza el seguimiento también sirve para cambiar el estado de la entidad.</span><span class="sxs-lookup"><span data-stu-id="22274-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="22274-150">Por ejemplo, una llamada a adjuntar para una entidad que está actualmente en estado Added cambiará su estado en Unchanged.</span><span class="sxs-lookup"><span data-stu-id="22274-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="22274-151">Insertar o actualizar el modelo</span><span class="sxs-lookup"><span data-stu-id="22274-151">Insert or update pattern</span></span>  

<span data-ttu-id="22274-152">Es un patrón común para algunas aplicaciones agregar una entidad nueva (resultante en una inserción de una base de datos) o asociar una entidad como existente y marcarlo como modificado (lo que conlleva una actualización de la base de datos) según el valor de la clave principal.</span><span class="sxs-lookup"><span data-stu-id="22274-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="22274-153">Por ejemplo, cuando se usa la base de datos genera las claves principales de entero es común para tratar una entidad con una cero clave como nueva y una entidad con una clave distinta de cero como existente.</span><span class="sxs-lookup"><span data-stu-id="22274-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="22274-154">Este patrón puede lograrse estableciendo el estado de la entidad basándose en una comprobación del valor de clave principal.</span><span class="sxs-lookup"><span data-stu-id="22274-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="22274-155">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22274-155">For example:</span></span>  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

<span data-ttu-id="22274-156">Tenga en cuenta que al cambiar el estado en Modified todas las propiedades de la entidad se marcarán como modificados y todos los valores de propiedad se enviará a la base de datos cuando se llama a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="22274-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
