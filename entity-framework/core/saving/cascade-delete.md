---
title: 'Eliminación en cascada: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 0fc8929c56d4c657b7fb1e3c8e4b1a71659220c9
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812682"
---
# <a name="cascade-delete"></a><span data-ttu-id="c4dc1-102">Eliminación en cascada</span><span class="sxs-lookup"><span data-stu-id="c4dc1-102">Cascade Delete</span></span>

<span data-ttu-id="c4dc1-103">En la terminología de las bases de datos, la eliminación en cascada se usa habitualmente para describir una característica que permite eliminar una fila para desencadenar de manera automática la eliminación de las filas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="c4dc1-104">Un concepto estrechamente relacionado que también abarcan los comportamientos de eliminación de EF Core es la eliminación automática de una entidad secundaria cuando se interrumpe su relación con una entidad primaria. Esto normalmente se conoce como la "eliminación de entidades huérfanas".</span><span class="sxs-lookup"><span data-stu-id="c4dc1-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="c4dc1-105">EF Core implementa varios comportamientos de eliminación distintos y permite configurar estos comportamientos de las relaciones individuales.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="c4dc1-106">EF Core también implementa convenciones que configuran automáticamente útiles comportamientos de eliminación predeterminados para cada relación en función de la [obligatoriedad de la relación](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="c4dc1-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="c4dc1-107">Comportamientos de eliminación</span><span class="sxs-lookup"><span data-stu-id="c4dc1-107">Delete behaviors</span></span>
<span data-ttu-id="c4dc1-108">Los comportamientos de eliminación se define en el tipo de enumerador *DeleteBehavior* y se pueden pasar a la API fluida *OnDelete* para controlar si la eliminación de una entidad principal o primaria o la interrupción de la relación con entidades dependientes o secundarias debería tener un efecto secundario en estas últimas.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="c4dc1-109">Hay tres acciones que EF puede llevar a cabo cuando se elimina una entidad principal o primaria o cuando se interrumpe la relación con una entidad secundaria:</span><span class="sxs-lookup"><span data-stu-id="c4dc1-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="c4dc1-110">Se puede eliminar la entidad secundaria o dependiente.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="c4dc1-111">Los valores de clave externa de la entidad secundaria se pueden establecer en NULL.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="c4dc1-112">La entidad secundaria se mantiene sin cambios.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="c4dc1-113">El comportamiento de eliminación configurado en el modelo EF Core solo se aplica cuando la entidad principal se elimina mediante EF Core y las entidades dependientes se cargan en la memoria (es decir, en el caso de las entidades dependientes con seguimiento).</span><span class="sxs-lookup"><span data-stu-id="c4dc1-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="c4dc1-114">Es necesario configurar un comportamiento en cascada correspondiente en la base de datos para garantizar que la acción necesaria se aplique a los datos a los que el contexto no hace seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="c4dc1-115">Si usa EF Core para crear la base de datos, este comportamiento en cascada se configurará automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="c4dc1-116">La segunda acción mencionada, establecer un valor de clave externa en NULL, no es válida si la clave externa no admite un valor NULL.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="c4dc1-117">(Una clave externa que no admite un valor NULL equivale a una relación obligatoria). En estos casos, EF Core hace seguimiento de que la propiedad de la clave externa se haya marcado como NULL hasta que se llama a SaveChanges, momento en que se genera una excepción porque el cambio no puede durar hasta la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="c4dc1-118">Esto es similar a obtener una infracción de restricción de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="c4dc1-119">Existen cuatro comportamientos de eliminación, los que se indican en las tablas siguientes.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="c4dc1-120">En el caso de las relaciones opcionales (clave externa que admite un valor NULL) _es_ posible guardar un valor de clave externa NULL, lo que tiene como resultado los efectos siguientes:</span><span class="sxs-lookup"><span data-stu-id="c4dc1-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="c4dc1-121">Nombre del comportamiento</span><span class="sxs-lookup"><span data-stu-id="c4dc1-121">Behavior Name</span></span>               | <span data-ttu-id="c4dc1-122">Efecto en la entidad dependiente o secundaria en la memoria</span><span class="sxs-lookup"><span data-stu-id="c4dc1-122">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="c4dc1-123">Efecto en la entidad dependiente o secundaria en la base de datos</span><span class="sxs-lookup"><span data-stu-id="c4dc1-123">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="c4dc1-124">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="c4dc1-124">**Cascade**</span></span>                 | <span data-ttu-id="c4dc1-125">Las entidades se eliminan</span><span class="sxs-lookup"><span data-stu-id="c4dc1-125">Entities are deleted</span></span>                   | <span data-ttu-id="c4dc1-126">Las entidades se eliminan</span><span class="sxs-lookup"><span data-stu-id="c4dc1-126">Entities are deleted</span></span>                   |
| <span data-ttu-id="c4dc1-127">**ClientSetNull** (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="c4dc1-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="c4dc1-128">Las propiedades de clave externa se establecen en NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="c4dc1-129">Ninguna</span><span class="sxs-lookup"><span data-stu-id="c4dc1-129">None</span></span>                                   |
| <span data-ttu-id="c4dc1-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="c4dc1-130">**SetNull**</span></span>                 | <span data-ttu-id="c4dc1-131">Las propiedades de clave externa se establecen en NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="c4dc1-132">Las propiedades de clave externa se establecen en NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-132">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="c4dc1-133">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="c4dc1-133">**Restrict**</span></span>                | <span data-ttu-id="c4dc1-134">Ninguna</span><span class="sxs-lookup"><span data-stu-id="c4dc1-134">None</span></span>                                   | <span data-ttu-id="c4dc1-135">Ninguna</span><span class="sxs-lookup"><span data-stu-id="c4dc1-135">None</span></span>                                   |

<span data-ttu-id="c4dc1-136">En el caso de las relaciones obligatorias (clave externa que no admite un valor NULL) _no_ es posible guardar un valor de clave externa NULL, lo que tiene como resultado los efectos siguientes:</span><span class="sxs-lookup"><span data-stu-id="c4dc1-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="c4dc1-137">Nombre del comportamiento</span><span class="sxs-lookup"><span data-stu-id="c4dc1-137">Behavior Name</span></span>         | <span data-ttu-id="c4dc1-138">Efecto en la entidad dependiente o secundaria en la memoria</span><span class="sxs-lookup"><span data-stu-id="c4dc1-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="c4dc1-139">Efecto en la entidad dependiente o secundaria en la base de datos</span><span class="sxs-lookup"><span data-stu-id="c4dc1-139">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="c4dc1-140">**Cascade** (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="c4dc1-140">**Cascade** (Default)</span></span> | <span data-ttu-id="c4dc1-141">Las entidades se eliminan</span><span class="sxs-lookup"><span data-stu-id="c4dc1-141">Entities are deleted</span></span>                | <span data-ttu-id="c4dc1-142">Las entidades se eliminan</span><span class="sxs-lookup"><span data-stu-id="c4dc1-142">Entities are deleted</span></span>                  |
| <span data-ttu-id="c4dc1-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="c4dc1-143">**ClientSetNull**</span></span>     | <span data-ttu-id="c4dc1-144">SaveChanges genera una excepción</span><span class="sxs-lookup"><span data-stu-id="c4dc1-144">SaveChanges throws</span></span>                  | <span data-ttu-id="c4dc1-145">Ninguna</span><span class="sxs-lookup"><span data-stu-id="c4dc1-145">None</span></span>                                  |
| <span data-ttu-id="c4dc1-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="c4dc1-146">**SetNull**</span></span>           | <span data-ttu-id="c4dc1-147">SaveChanges genera una excepción</span><span class="sxs-lookup"><span data-stu-id="c4dc1-147">SaveChanges throws</span></span>                  | <span data-ttu-id="c4dc1-148">SaveChanges genera una excepción</span><span class="sxs-lookup"><span data-stu-id="c4dc1-148">SaveChanges throws</span></span>                    |
| <span data-ttu-id="c4dc1-149">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="c4dc1-149">**Restrict**</span></span>          | <span data-ttu-id="c4dc1-150">Ninguna</span><span class="sxs-lookup"><span data-stu-id="c4dc1-150">None</span></span>                                | <span data-ttu-id="c4dc1-151">Ninguna</span><span class="sxs-lookup"><span data-stu-id="c4dc1-151">None</span></span>                                  |

<span data-ttu-id="c4dc1-152">En las tablas anteriores, *None* puede dar lugar a una infracción de restricción.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="c4dc1-153">Por ejemplo, si se elimina una entidad principal o secundaria pero no se hace ninguna acción para cambiar la clave externa de una entidad dependiente o secundaria, es probable que la base de datos genere una excepción en SaveChanges debido a una infracción de restricción externa.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="c4dc1-154">En un nivel superior:</span><span class="sxs-lookup"><span data-stu-id="c4dc1-154">At a high level:</span></span>
* <span data-ttu-id="c4dc1-155">Si tiene entidades que no pueden existir sin una entidad primaria y quiere que EF se encargue de eliminar automáticamente las entidades secundarias, use *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="c4dc1-156">Habitualmente, las entidades que no pueden existir sin una entidad primaria usan las relaciones obligatorias, en las que el valor predeterminado es *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="c4dc1-157">Si tiene entidades que pueden tener o no una entidad primaria y quiere que EF se encargue de anular automáticamente la clave externa, use *ClientSetNull*.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="c4dc1-158">Habitualmente, las entidades que pueden existir sin una entidad primaria usan las relaciones opcionales, en las que el valor predeterminado es *ClientSetNull*.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="c4dc1-159">Si quiere que la base de datos también intente propagar los valores NULL a las claves externas secundarias incluso si no se cargó la entidad secundaria, use *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="c4dc1-160">Sin embargo, tenga en cuenta que la base de datos debe admitir esta opción y que configurar de este modo la base de datos puede dar lugar a otras restricciones, por lo que, en la práctica, a menudo esta opción no es factible.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="c4dc1-161">Es por este motivo que *SetNull* no es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="c4dc1-162">Si no quiere que EF Core elimine una entidad o anule la clave externa automáticamente, use *Restrict*.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="c4dc1-163">Tenga en cuenta que esta acción requiere que el código mantenga las entidades secundarias y sus valores de clave externa sincronizados de manera manual. Si no es así, se generarán excepciones de restricción.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="c4dc1-164">En EF Core, a diferencia de lo que ocurre en EF6, los efectos en cascada no se producen de inmediato, sino que solo cuando se llama a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="c4dc1-165">**Cambios en EF Core 2.0:** en versiones anteriores, *Restrict* supondría que las propiedades de claves externas opcionales de las entidades dependientes con seguimiento se establezcan en NULL y que este fuese el comportamiento de eliminación predeterminado de las relaciones opcionales.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="c4dc1-166">En EF Core 2.0, se introdujo *ClientSetNull* para representar ese comportamiento y se transformó en el valor predeterminado de las relaciones opcionales.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="c4dc1-167">El comportamiento de *Restrict* se ajustó para que nunca haya efectos secundarios en las entidades dependientes.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="c4dc1-168">Ejemplos de eliminación de entidades</span><span class="sxs-lookup"><span data-stu-id="c4dc1-168">Entity deletion examples</span></span>

<span data-ttu-id="c4dc1-169">El código siguiente forma parte de un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que se puede descargar y ejecutar.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="c4dc1-170">El ejemplo muestra qué pasa con cada comportamiento de eliminación, tanto en las relaciones opcionales como en las obligatorias, cuando se elimina una entidad primaria.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="c4dc1-171">Analicemos cada variación para comprender lo que sucede.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="c4dc1-172">DeleteBehavior.Cascade con relación obligatoria u opcional</span><span class="sxs-lookup"><span data-stu-id="c4dc1-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="c4dc1-173">El blog se marca como eliminado</span><span class="sxs-lookup"><span data-stu-id="c4dc1-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="c4dc1-174">Inicialmente, las entradas no se modifican porque las cascadas no se producen hasta que se genera SaveChanges</span><span class="sxs-lookup"><span data-stu-id="c4dc1-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="c4dc1-175">SaveChanges envía eliminaciones para las entidades dependientes o secundarias (entradas) y para la entidad principal o primaria (blog)</span><span class="sxs-lookup"><span data-stu-id="c4dc1-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="c4dc1-176">Después de guardar, todas las entidades se desasocian porque se eliminaron de la base de datos</span><span class="sxs-lookup"><span data-stu-id="c4dc1-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="c4dc1-177">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación obligatoria</span><span class="sxs-lookup"><span data-stu-id="c4dc1-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="c4dc1-178">El blog se marca como eliminado</span><span class="sxs-lookup"><span data-stu-id="c4dc1-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="c4dc1-179">Inicialmente, las entradas no se modifican porque las cascadas no se producen hasta que se genera SaveChanges</span><span class="sxs-lookup"><span data-stu-id="c4dc1-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="c4dc1-180">SaveChanges intenta establecer la clave externa de la entrada en NULL, pero se produce un error porque la clave externa no admite un valor NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="c4dc1-181">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación opcional</span><span class="sxs-lookup"><span data-stu-id="c4dc1-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="c4dc1-182">El blog se marca como eliminado</span><span class="sxs-lookup"><span data-stu-id="c4dc1-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="c4dc1-183">Inicialmente, las entradas no se modifican porque las cascadas no se producen hasta que se genera SaveChanges</span><span class="sxs-lookup"><span data-stu-id="c4dc1-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="c4dc1-184">SaveChanges intenta establecer la clave externa de las entidades dependientes o secundarias (entradas) en NULL antes de eliminar la entidad principal o primaria (blog)</span><span class="sxs-lookup"><span data-stu-id="c4dc1-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="c4dc1-185">Después de guardar, se elimina la entidad principal o primaria (blog), pero todavía se hace seguimiento de las entidades dependientes o secundarias (entradas)</span><span class="sxs-lookup"><span data-stu-id="c4dc1-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="c4dc1-186">Las entidades dependientes o secundarias (entradas) con seguimiento ahora tienen valores de clave externa NULL y se quitó la referencia a la entidad principal o primaria (blog) que se eliminó</span><span class="sxs-lookup"><span data-stu-id="c4dc1-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="c4dc1-187">DeleteBehavior.Restrict con relación obligatoria u opcional</span><span class="sxs-lookup"><span data-stu-id="c4dc1-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="c4dc1-188">El blog se marca como eliminado</span><span class="sxs-lookup"><span data-stu-id="c4dc1-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="c4dc1-189">Inicialmente, las entradas no se modifican porque las cascadas no se producen hasta que se genera SaveChanges</span><span class="sxs-lookup"><span data-stu-id="c4dc1-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="c4dc1-190">Como *Restrict* le indica a EF que no establezca automáticamente la clave externa en NULL, no se anula y SaveChanges genera una excepción sin guardar</span><span class="sxs-lookup"><span data-stu-id="c4dc1-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="c4dc1-191">Ejemplos de eliminación de entidades huérfanas</span><span class="sxs-lookup"><span data-stu-id="c4dc1-191">Delete orphans examples</span></span>

<span data-ttu-id="c4dc1-192">El código siguiente forma parte de un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que se puede descargar y ejecutar.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="c4dc1-193">El ejemplo muestra qué sucede en cada comportamiento de eliminación, tanto para las relaciones opcionales como para las obligatorias, cuando se interrumpe la relación entre una entidad primaria o principal y sus entidades secundarias o dependientes.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="c4dc1-194">En este ejemplo, la relación se interrumpe al quitar las entidades dependientes o secundarias (entradas) de la propiedad de navegación de la colección en la entidad principal o primaria (blog).</span><span class="sxs-lookup"><span data-stu-id="c4dc1-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="c4dc1-195">Sin embargo, el comportamiento es el mismo si se anula la referencia de la entidad secundaria o dependiente respecto de la entidad principal o primaria.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="c4dc1-196">Analicemos cada variación para comprender lo que sucede.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="c4dc1-197">DeleteBehavior.Cascade con relación obligatoria u opcional</span><span class="sxs-lookup"><span data-stu-id="c4dc1-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="c4dc1-198">Las entradas se marcan como modificadas porque la interrupción de la relación hizo que la clave externa se marcara como NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="c4dc1-199">Si la clave externa no admite un valor NULL, el valor actual no se modificará incluso si se marca como NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="c4dc1-200">SaveChanges envía eliminaciones para las entidades dependientes o secundarias (entradas)</span><span class="sxs-lookup"><span data-stu-id="c4dc1-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="c4dc1-201">Después de guardar, las entidades dependientes o secundarias (entradas) se desasocian porque se eliminaron de la base de datos</span><span class="sxs-lookup"><span data-stu-id="c4dc1-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="c4dc1-202">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación obligatoria</span><span class="sxs-lookup"><span data-stu-id="c4dc1-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="c4dc1-203">Las entradas se marcan como modificadas porque la interrupción de la relación hizo que la clave externa se marcara como NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="c4dc1-204">Si la clave externa no admite un valor NULL, el valor actual no se modificará incluso si se marca como NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="c4dc1-205">SaveChanges intenta establecer la clave externa de la entrada en NULL, pero se produce un error porque la clave externa no admite un valor NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="c4dc1-206">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación opcional</span><span class="sxs-lookup"><span data-stu-id="c4dc1-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="c4dc1-207">Las entradas se marcan como modificadas porque la interrupción de la relación hizo que la clave externa se marcara como NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="c4dc1-208">Si la clave externa no admite un valor NULL, el valor actual no se modificará incluso si se marca como NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="c4dc1-209">SaveChanges establece la clave externa de las entidades dependientes o secundarias (entradas) en NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="c4dc1-210">Después de guardar, las entidades dependientes o secundarias (entradas) ahora tienen valores NULL de clave externa y se quitó la referencia a la entidad principal o primaria (blog) que se eliminó</span><span class="sxs-lookup"><span data-stu-id="c4dc1-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="c4dc1-211">DeleteBehavior.Restrict con relación obligatoria u opcional</span><span class="sxs-lookup"><span data-stu-id="c4dc1-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="c4dc1-212">Las entradas se marcan como modificadas porque la interrupción de la relación hizo que la clave externa se marcara como NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="c4dc1-213">Si la clave externa no admite un valor NULL, el valor actual no se modificará incluso si se marca como NULL</span><span class="sxs-lookup"><span data-stu-id="c4dc1-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="c4dc1-214">Como *Restrict* le indica a EF que no establezca automáticamente la clave externa en NULL, no se anula y SaveChanges genera una excepción sin guardar</span><span class="sxs-lookup"><span data-stu-id="c4dc1-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="c4dc1-215">Aplicación en cascada a las entidades sin seguimiento</span><span class="sxs-lookup"><span data-stu-id="c4dc1-215">Cascading to untracked entities</span></span>

<span data-ttu-id="c4dc1-216">Cuando llama a *SaveChanges*, se aplicarán las reglas de la aplicación en cascada a cualquier entidad de la que el contexto hace el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="c4dc1-217">Esto es lo que ocurre en todos los ejemplos anteriores, que es la razón por la cual se generó código SQL para eliminar tanto la entidad principal o primaria (blog) como todas las entidades dependientes o secundarias (entradas):</span><span class="sxs-lookup"><span data-stu-id="c4dc1-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="c4dc1-218">Si solo se carga la entidad principal (por ejemplo, cuando se hace una solicitud para un blog sin `Include(b => b.Posts)` para que también incluya las entradas), SaveChanges solo generará código SQL para eliminar la entidad principal o primaria:</span><span class="sxs-lookup"><span data-stu-id="c4dc1-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="c4dc1-219">Las entidades dependientes o secundarias (entradas) solo se eliminarán si la base de datos tiene configurado un comportamiento en cascada correspondiente.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="c4dc1-220">Si usa EF para crear la base de datos, este comportamiento en cascada se configurará automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c4dc1-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
