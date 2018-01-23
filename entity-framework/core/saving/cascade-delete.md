---
title: "Eliminación - Core EF en cascada"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: a9481fe851cc264ab3eaecad052c2e683ae57a44
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2017
---
# <a name="cascade-delete"></a><span data-ttu-id="7be99-102">Eliminación en cascada</span><span class="sxs-lookup"><span data-stu-id="7be99-102">Cascade Delete</span></span>

<span data-ttu-id="7be99-103">La eliminación en cascada se utiliza normalmente en la terminología de base de datos para describir una característica que permite la eliminación de una fila para desencadenar automáticamente la eliminación de filas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="7be99-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="7be99-104">Un concepto estrechamente relacionado también cubierto por comportamientos de eliminación de EF principal es la eliminación automática de una entidad secundaria cuando se trata de relación en un elemento primario se ha roto--este i que se conoce como "eliminar huérfanos".</span><span class="sxs-lookup"><span data-stu-id="7be99-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this i commonly known as "deleting orphans".</span></span>

<span data-ttu-id="7be99-105">Núcleo EF implementa varios comportamientos de eliminación diferente y permite la configuración de los comportamientos de eliminación de relaciones individuales.</span><span class="sxs-lookup"><span data-stu-id="7be99-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="7be99-106">Núcleo EF también implementa convenciones que configurar automáticamente los comportamientos de eliminación de predeterminados útiles para cada relación según la [requiredness de la relación] (../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="7be99-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship] (../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="7be99-107">Eliminar comportamientos</span><span class="sxs-lookup"><span data-stu-id="7be99-107">Delete behaviors</span></span>
<span data-ttu-id="7be99-108">Eliminar comportamientos se definen en el *DeleteBehavior* enumerador tipo y puede pasarse a la *OnDelete* API fluida para controlar si la eliminación de una entidad principal/primaria o el desactivar de la relación con las entidades dependientes/secundarias debe tener un efecto secundario en las entidades dependientes/secundario.</span><span class="sxs-lookup"><span data-stu-id="7be99-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="7be99-109">Hay tres acciones que EF pueda realizar cuando se elimina una entidad principal/primaria o se rompe la relación para el elemento secundario:</span><span class="sxs-lookup"><span data-stu-id="7be99-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="7be99-110">Se puede eliminar el elemento secundario/dependiente</span><span class="sxs-lookup"><span data-stu-id="7be99-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="7be99-111">Valores de clave externa del elemento secundario pueden establecerse en null</span><span class="sxs-lookup"><span data-stu-id="7be99-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="7be99-112">No cambie el elemento secundario</span><span class="sxs-lookup"><span data-stu-id="7be99-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="7be99-113">El comportamiento de eliminación configurado en el modelo de EF Core solo se aplica cuando la entidad de seguridad se elimina mediante EF principales y las entidades dependientes se cargan en memoria (es decir, para dependientes del seguimiento).</span><span class="sxs-lookup"><span data-stu-id="7be99-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="7be99-114">Un comportamiento en cascada correspondiente debe estar configurado en la base de datos para garantizar que los datos que no realiza el seguimiento por el contexto tiene la acción es necesaria que se aplica.</span><span class="sxs-lookup"><span data-stu-id="7be99-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="7be99-115">Si usas EF básica para crear la base de datos, este comportamiento en cascada se configurará automáticamente.</span><span class="sxs-lookup"><span data-stu-id="7be99-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="7be99-116">La segunda acción, establecer un valor de clave externa como null no es válido si la clave externa no acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="7be99-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="7be99-117">(Una clave externa que no aceptan valores NULL equivale a una relación de obligatorio). En estos casos, el núcleo de EF realiza un seguimiento que la propiedad de clave externa se ha marcado como null hasta que se llama a SaveChanges, momento en el que se produce una excepción porque no se puede guardar el cambio a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7be99-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="7be99-118">Esto es similar a obtener una infracción de restricción de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7be99-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="7be99-119">Hay cuatro eliminar comportamientos, como se muestra en las tablas siguientes.</span><span class="sxs-lookup"><span data-stu-id="7be99-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="7be99-120">Para las relaciones opcionales (clave externa que aceptan valores NULL), _es_ pueden guardar un null valor de clave externa, lo que resulta en los siguientes efectos:</span><span class="sxs-lookup"><span data-stu-id="7be99-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="7be99-121">Nombre de comportamiento</span><span class="sxs-lookup"><span data-stu-id="7be99-121">Behavior Name</span></span> | <span data-ttu-id="7be99-122">Efecto en dependientes/elemento secundario en la memoria</span><span class="sxs-lookup"><span data-stu-id="7be99-122">Effect on dependent/child in memory</span></span> | <span data-ttu-id="7be99-123">Efecto en dependientes/elemento secundario en la base de datos</span><span class="sxs-lookup"><span data-stu-id="7be99-123">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="7be99-124">**En cascada**</span><span class="sxs-lookup"><span data-stu-id="7be99-124">**Cascade**</span></span> | <span data-ttu-id="7be99-125">Se eliminan las entidades</span><span class="sxs-lookup"><span data-stu-id="7be99-125">Entities are deleted</span></span> | <span data-ttu-id="7be99-126">Se eliminan las entidades</span><span class="sxs-lookup"><span data-stu-id="7be99-126">Entities are deleted</span></span>
| <span data-ttu-id="7be99-127">**ClientSetNull** (predeterminado)</span><span class="sxs-lookup"><span data-stu-id="7be99-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="7be99-128">Propiedades de clave externa se establecen en null</span><span class="sxs-lookup"><span data-stu-id="7be99-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="7be99-129">Ninguna</span><span class="sxs-lookup"><span data-stu-id="7be99-129">None</span></span>
| <span data-ttu-id="7be99-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="7be99-130">**SetNull**</span></span> | <span data-ttu-id="7be99-131">Propiedades de clave externa se establecen en null</span><span class="sxs-lookup"><span data-stu-id="7be99-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="7be99-132">Propiedades de clave externa se establecen en null</span><span class="sxs-lookup"><span data-stu-id="7be99-132">Foreign key properties are set to null</span></span>
| <span data-ttu-id="7be99-133">**Restringir**</span><span class="sxs-lookup"><span data-stu-id="7be99-133">**Restrict**</span></span> | <span data-ttu-id="7be99-134">Ninguna</span><span class="sxs-lookup"><span data-stu-id="7be99-134">None</span></span> | <span data-ttu-id="7be99-135">Ninguna</span><span class="sxs-lookup"><span data-stu-id="7be99-135">None</span></span>

<span data-ttu-id="7be99-136">Para las relaciones necesarias (clave externa no acepta valores NULL) es _no_ pueden guardar un null valor de clave externa, lo que resulta en los siguientes efectos:</span><span class="sxs-lookup"><span data-stu-id="7be99-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="7be99-137">Nombre de comportamiento</span><span class="sxs-lookup"><span data-stu-id="7be99-137">Behavior Name</span></span> | <span data-ttu-id="7be99-138">Efecto en dependientes/elemento secundario en la memoria</span><span class="sxs-lookup"><span data-stu-id="7be99-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="7be99-139">Efecto en dependientes/elemento secundario en la base de datos</span><span class="sxs-lookup"><span data-stu-id="7be99-139">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="7be99-140">**CASCADE** (predeterminado)</span><span class="sxs-lookup"><span data-stu-id="7be99-140">**Cascade** (Default)</span></span> | <span data-ttu-id="7be99-141">Se eliminan las entidades</span><span class="sxs-lookup"><span data-stu-id="7be99-141">Entities are deleted</span></span> | <span data-ttu-id="7be99-142">Se eliminan las entidades</span><span class="sxs-lookup"><span data-stu-id="7be99-142">Entities are deleted</span></span>
| <span data-ttu-id="7be99-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="7be99-143">**ClientSetNull**</span></span> | <span data-ttu-id="7be99-144">SaveChanges produce</span><span class="sxs-lookup"><span data-stu-id="7be99-144">SaveChanges throws</span></span> | <span data-ttu-id="7be99-145">Ninguna</span><span class="sxs-lookup"><span data-stu-id="7be99-145">None</span></span>
| <span data-ttu-id="7be99-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="7be99-146">**SetNull**</span></span> | <span data-ttu-id="7be99-147">SaveChanges produce</span><span class="sxs-lookup"><span data-stu-id="7be99-147">SaveChanges throws</span></span> | <span data-ttu-id="7be99-148">SaveChanges produce</span><span class="sxs-lookup"><span data-stu-id="7be99-148">SaveChanges throws</span></span>
| <span data-ttu-id="7be99-149">**Restringir**</span><span class="sxs-lookup"><span data-stu-id="7be99-149">**Restrict**</span></span> | <span data-ttu-id="7be99-150">Ninguna</span><span class="sxs-lookup"><span data-stu-id="7be99-150">None</span></span> | <span data-ttu-id="7be99-151">Ninguna</span><span class="sxs-lookup"><span data-stu-id="7be99-151">None</span></span>

<span data-ttu-id="7be99-152">En las tablas anteriores, *ninguno* puede dar lugar a una infracción de restricción.</span><span class="sxs-lookup"><span data-stu-id="7be99-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="7be99-153">Por ejemplo, si se elimina una entidad principal o secundaria, pero no se realiza ninguna acción para cambiar la clave externa de un elemento dependiente/secundario, a continuación, la base de datos probablemente producirá en SaveChanges debido a una infracción de restricción externa.</span><span class="sxs-lookup"><span data-stu-id="7be99-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="7be99-154">En un nivel superior:</span><span class="sxs-lookup"><span data-stu-id="7be99-154">At a high level:</span></span>
* <span data-ttu-id="7be99-155">Si tiene entidades que no pueden existir sin un elemento primario, y desea EF tener en cuenta para eliminar automáticamente los elementos secundarios y luego use *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="7be99-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="7be99-156">Las entidades que no pueden existir sin un elemento primario normalmente se realizan uso de relaciones necesarias, para el que *Cascade* es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7be99-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="7be99-157">Si tiene entidades que pueden tienen o no un elemento primario, y desea EF a cargo de anular automáticamente la clave externa y luego use *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="7be99-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="7be99-158">Entidades que pueden existir sin normalmente se realizan en un elemento primario usa de relaciones opcionales, para que *ClientSetNull* es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7be99-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="7be99-159">Si desea que la base de datos para intentar propagar los valores null para las claves externas secundarios incluso cuando la entidad secundaria no está cargada, a continuación, use *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="7be99-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="7be99-160">Sin embargo, tenga en cuenta que la base de datos debe admitir esto, y configurar la base de datos similar al siguiente puede dar lugar a otras restricciones, lo que en la práctica a menudo hace poco práctico si esta opción.</span><span class="sxs-lookup"><span data-stu-id="7be99-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="7be99-161">Se trata de por qué *SetNull* no es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7be99-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="7be99-162">Si no desea que EF Core para eliminar una entidad automáticamente o null automáticamente a la clave externa, use alguna vez *restringir*.</span><span class="sxs-lookup"><span data-stu-id="7be99-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="7be99-163">Tenga en cuenta que esto requiere que el código mantener sus valores de clave externas y las entidades secundarias sincronizadas manualmente en caso contrario, las excepciones de restricción se producirá.</span><span class="sxs-lookup"><span data-stu-id="7be99-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="7be99-164">En el núcleo de EF, a diferencia de EF6, efectos en cascada no realizan inmediatamente, pero en su lugar sólo cuando se llama a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="7be99-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="7be99-165">**Cambios en EF Core 2.0:** en versiones anteriores, *restringir* causaría opcionales propiedades de clave externa en entidades dependientes sometidas a seguimiento se establezca en null, mientras que fue el comportamiento para las relaciones opcionales al eliminar el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7be99-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="7be99-166">En EF Core 2.0, el *ClientSetNull* se introdujo para representar ese comportamiento y se ha convertido el valor predeterminado para las relaciones opcionales.</span><span class="sxs-lookup"><span data-stu-id="7be99-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="7be99-167">El comportamiento de *restringir* se ajustó para nunca tienen efectos secundarios en entidades dependientes.</span><span class="sxs-lookup"><span data-stu-id="7be99-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="7be99-168">Ejemplos de eliminación de entidad</span><span class="sxs-lookup"><span data-stu-id="7be99-168">Entity deletion examples</span></span>

<span data-ttu-id="7be99-169">El código siguiente forma parte de un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que se puede descargar una ejecución.</span><span class="sxs-lookup"><span data-stu-id="7be99-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="7be99-170">El ejemplo muestra lo que sucede para cada comportamiento de eliminación de relaciones obligatorios y opcionales cuando se elimina una entidad primaria.</span><span class="sxs-lookup"><span data-stu-id="7be99-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="7be99-171">Analicemos cada variación para comprender lo que está sucediendo.</span><span class="sxs-lookup"><span data-stu-id="7be99-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="7be99-172">DeleteBehavior.Cascade con relación de obligatorio u opcional</span><span class="sxs-lookup"><span data-stu-id="7be99-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="7be99-173">Blog se marca como eliminado</span><span class="sxs-lookup"><span data-stu-id="7be99-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="7be99-174">Entradas inicialmente permanecen sin cambios desde que se ponen en cascada no se realizan hasta SaveChanges</span><span class="sxs-lookup"><span data-stu-id="7be99-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="7be99-175">SaveChanges envía las eliminaciones para dependientes/hijos (o publica) y, a continuación, la entidad de seguridad/primario (blog)</span><span class="sxs-lookup"><span data-stu-id="7be99-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="7be99-176">Después de guardar, todas las entidades se desasocian ya que ahora se han eliminado de la base de datos</span><span class="sxs-lookup"><span data-stu-id="7be99-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="7be99-177">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación necesaria</span><span class="sxs-lookup"><span data-stu-id="7be99-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="7be99-178">Blog se marca como eliminado</span><span class="sxs-lookup"><span data-stu-id="7be99-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="7be99-179">Entradas inicialmente permanecen sin cambios desde que se ponen en cascada no se realizan hasta SaveChanges</span><span class="sxs-lookup"><span data-stu-id="7be99-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="7be99-180">SaveChanges intenta establecer la entrada de clave externa como null, pero se produce un error como la clave externa no aceptan valores null</span><span class="sxs-lookup"><span data-stu-id="7be99-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="7be99-181">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación opcional</span><span class="sxs-lookup"><span data-stu-id="7be99-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="7be99-182">Blog se marca como eliminado</span><span class="sxs-lookup"><span data-stu-id="7be99-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="7be99-183">Entradas inicialmente permanecen sin cambios desde que se ponen en cascada no se realizan hasta SaveChanges</span><span class="sxs-lookup"><span data-stu-id="7be99-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="7be99-184">Intentos de SaveChanges establece la clave externa de dependientes y elementos secundarios (o publica) en null antes de eliminar la entidad de seguridad/primario (blog)</span><span class="sxs-lookup"><span data-stu-id="7be99-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="7be99-185">Después de guardar, se elimina la entidad de seguridad/primario (blog), pero todavía se realiza un seguimiento de los elementos dependientes/hijos (o publica)</span><span class="sxs-lookup"><span data-stu-id="7be99-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="7be99-186">El seguimiento dependientes/hijos (o publica) ahora tienen valores null de clave externa y se ha quitado su referencia a la entidad de seguridad/primario eliminado (blog)</span><span class="sxs-lookup"><span data-stu-id="7be99-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="7be99-187">DeleteBehavior.Restrict con relación de obligatorio u opcional</span><span class="sxs-lookup"><span data-stu-id="7be99-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="7be99-188">Blog se marca como eliminado</span><span class="sxs-lookup"><span data-stu-id="7be99-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="7be99-189">Entradas inicialmente permanecen sin cambios desde que se ponen en cascada no se realizan hasta SaveChanges</span><span class="sxs-lookup"><span data-stu-id="7be99-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="7be99-190">Puesto que *restringir* indica EF para establecer automáticamente la clave externa como null, permanece no null y SaveChanges produce sin guardar los cambios</span><span class="sxs-lookup"><span data-stu-id="7be99-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="7be99-191">Ejemplos de huérfanos de eliminación</span><span class="sxs-lookup"><span data-stu-id="7be99-191">Delete orphans examples</span></span>

<span data-ttu-id="7be99-192">El código siguiente forma parte de un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que se puede descargar una ejecución.</span><span class="sxs-lookup"><span data-stu-id="7be99-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="7be99-193">El ejemplo muestra lo que sucede para cada comportamiento de eliminación de relaciones obligatorios y opcionales cuando se rompe la relación entre una entidad de principal y sus elementos secundarios o elementos dependientes.</span><span class="sxs-lookup"><span data-stu-id="7be99-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="7be99-194">En este ejemplo, se rompe la relación mediante la eliminación de los elementos dependientes/hijos (o publica) de la propiedad de navegación de colección en la entidad de seguridad/primario (blog).</span><span class="sxs-lookup"><span data-stu-id="7be99-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="7be99-195">Sin embargo, el comportamiento es el mismo si en su lugar, se anula la referencia de dependiente/secundario a principal/primario.</span><span class="sxs-lookup"><span data-stu-id="7be99-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="7be99-196">Analicemos cada variación para comprender lo que está sucediendo.</span><span class="sxs-lookup"><span data-stu-id="7be99-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="7be99-197">DeleteBehavior.Cascade con relación de obligatorio u opcional</span><span class="sxs-lookup"><span data-stu-id="7be99-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="7be99-198">Entradas se marcan como modificadas porque desactivar la relación provocó la clave externa se marquen como null</span><span class="sxs-lookup"><span data-stu-id="7be99-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="7be99-199">Si la clave externa no acepta valores NULL, a continuación, el valor real no cambiará incluso aunque esté marcado como null</span><span class="sxs-lookup"><span data-stu-id="7be99-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="7be99-200">SaveChanges envía las eliminaciones de elementos dependientes/los elementos secundarios (o publica)</span><span class="sxs-lookup"><span data-stu-id="7be99-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="7be99-201">Después de guardar, se desasocian dependientes/hijos (o publica) ya que ahora se han eliminado de la base de datos</span><span class="sxs-lookup"><span data-stu-id="7be99-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="7be99-202">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación necesaria</span><span class="sxs-lookup"><span data-stu-id="7be99-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="7be99-203">Entradas se marcan como modificadas porque desactivar la relación provocó la clave externa se marquen como null</span><span class="sxs-lookup"><span data-stu-id="7be99-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="7be99-204">Si la clave externa no acepta valores NULL, a continuación, el valor real no cambiará incluso aunque esté marcado como null</span><span class="sxs-lookup"><span data-stu-id="7be99-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="7be99-205">SaveChanges intenta establecer la entrada de clave externa como null, pero se produce un error como la clave externa no aceptan valores null</span><span class="sxs-lookup"><span data-stu-id="7be99-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="7be99-206">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación opcional</span><span class="sxs-lookup"><span data-stu-id="7be99-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="7be99-207">Entradas se marcan como modificadas porque desactivar la relación provocó la clave externa se marquen como null</span><span class="sxs-lookup"><span data-stu-id="7be99-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="7be99-208">Si la clave externa no acepta valores NULL, a continuación, el valor real no cambiará incluso aunque esté marcado como null</span><span class="sxs-lookup"><span data-stu-id="7be99-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="7be99-209">SaveChanges establece la clave externa de dependientes y elementos secundarios (o publica) en null</span><span class="sxs-lookup"><span data-stu-id="7be99-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="7be99-210">Después de guardar, dependientes/hijos (o publica) ahora tienen valores null de clave externa y se ha quitado su referencia a la entidad de seguridad/primario eliminado (blog)</span><span class="sxs-lookup"><span data-stu-id="7be99-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="7be99-211">DeleteBehavior.Restrict con relación de obligatorio u opcional</span><span class="sxs-lookup"><span data-stu-id="7be99-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="7be99-212">Entradas se marcan como modificadas porque desactivar la relación provocó la clave externa se marquen como null</span><span class="sxs-lookup"><span data-stu-id="7be99-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="7be99-213">Si la clave externa no acepta valores NULL, a continuación, el valor real no cambiará incluso aunque esté marcado como null</span><span class="sxs-lookup"><span data-stu-id="7be99-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="7be99-214">Puesto que *restringir* indica EF para establecer automáticamente la clave externa como null, permanece no null y SaveChanges produce sin guardar los cambios</span><span class="sxs-lookup"><span data-stu-id="7be99-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="7be99-215">En cascada a entidades sin seguimiento</span><span class="sxs-lookup"><span data-stu-id="7be99-215">Cascading to untracked entities</span></span>

<span data-ttu-id="7be99-216">Cuando se llama a *SaveChanges*, las reglas de eliminación en cascada se aplicarán a todas las entidades que se realiza el seguimiento por el contexto.</span><span class="sxs-lookup"><span data-stu-id="7be99-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="7be99-217">Ésta es la situación en todos los ejemplos mostrados anteriormente, que es la razón por SQL se generó para eliminar la entidad de seguridad/primario (blog) y todos los elementos dependientes/nodos secundarios (o publica):</span><span class="sxs-lookup"><span data-stu-id="7be99-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="7be99-218">Si solo la entidad de seguridad se carga, por ejemplo, cuando se realiza una consulta para un blog sin un `Include(b => b.Posts)` incluir también entradas--, SaveChanges solo generará SQL para eliminar la entidad de seguridad o elemento primario:</span><span class="sxs-lookup"><span data-stu-id="7be99-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="7be99-219">Solo se eliminarán los elementos dependientes/hijos (o publica) si la base de datos tiene un comportamiento en cascada correspondiente configurado.</span><span class="sxs-lookup"><span data-stu-id="7be99-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="7be99-220">Si usas EF para crear la base de datos, este comportamiento en cascada se configurará automáticamente.</span><span class="sxs-lookup"><span data-stu-id="7be99-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
