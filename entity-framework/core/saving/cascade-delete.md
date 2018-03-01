---
title: "Eliminación - Core EF en cascada"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 1ab9d114e27aac0bec972df631a426c8ce87a518
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="cascade-delete"></a>Eliminación en cascada

La eliminación en cascada se utiliza normalmente en la terminología de base de datos para describir una característica que permite la eliminación de una fila para desencadenar automáticamente la eliminación de filas relacionadas. Un concepto estrechamente relacionado también cubierto por comportamientos de eliminación de EF principal es la eliminación automática de una entidad secundaria cuando se trata de relación en un elemento primario se ha roto--este i que se conoce como "eliminar huérfanos".

Núcleo EF implementa varios comportamientos de eliminación diferente y permite la configuración de los comportamientos de eliminación de relaciones individuales. Núcleo EF también implementa convenciones que configurar automáticamente los comportamientos de eliminación de predeterminados útiles para cada relación según la [requiredness de la relación] (../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Eliminar comportamientos
Eliminar comportamientos se definen en el *DeleteBehavior* enumerador tipo y puede pasarse a la *OnDelete* API fluida para controlar si la eliminación de una entidad principal/primaria o el desactivar de la relación con las entidades dependientes/secundarias debe tener un efecto secundario en las entidades dependientes/secundario.

Hay tres acciones que EF pueda realizar cuando se elimina una entidad principal/primaria o se rompe la relación para el elemento secundario:
* Se puede eliminar el elemento secundario/dependiente
* Valores de clave externa del elemento secundario pueden establecerse en null
* No cambie el elemento secundario

> [!NOTE]  
> El comportamiento de eliminación configurado en el modelo de EF Core solo se aplica cuando la entidad de seguridad se elimina mediante EF principales y las entidades dependientes se cargan en memoria (es decir, para dependientes del seguimiento). Un comportamiento en cascada correspondiente debe estar configurado en la base de datos para garantizar que los datos que no realiza el seguimiento por el contexto tiene la acción es necesaria que se aplica. Si usas EF básica para crear la base de datos, este comportamiento en cascada se configurará automáticamente.

La segunda acción, establecer un valor de clave externa como null no es válido si la clave externa no acepta valores NULL. (Una clave externa que no aceptan valores NULL equivale a una relación de obligatorio). En estos casos, el núcleo de EF realiza un seguimiento que la propiedad de clave externa se ha marcado como null hasta que se llama a SaveChanges, momento en el que se produce una excepción porque no se puede guardar el cambio a la base de datos. Esto es similar a obtener una infracción de restricción de la base de datos.

Hay cuatro eliminar comportamientos, como se muestra en las tablas siguientes. Para las relaciones opcionales (clave externa que aceptan valores NULL), _es_ pueden guardar un null valor de clave externa, lo que resulta en los siguientes efectos:

| Nombre de comportamiento               | Efecto en dependientes/elemento secundario en la memoria    | Efecto en dependientes/elemento secundario en la base de datos  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Cascade**                 | Se eliminan las entidades                   | Se eliminan las entidades                   |
| **ClientSetNull** (predeterminado) | Propiedades de clave externa se establecen en null | Ninguna                                   |
| **SetNull**                 | Propiedades de clave externa se establecen en null | Propiedades de clave externa se establecen en null |
| **Restrict**                | Ninguna                                   | Ninguna                                   |

Para las relaciones necesarias (clave externa no acepta valores NULL) es _no_ pueden guardar un null valor de clave externa, lo que resulta en los siguientes efectos:

| Nombre de comportamiento         | Efecto en dependientes/elemento secundario en la memoria | Efecto en dependientes/elemento secundario en la base de datos |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Cascada** (predeterminado) | Se eliminan las entidades                | Se eliminan las entidades                  |
| **ClientSetNull**     | SaveChanges produce                  | Ninguna                                  |
| **SetNull**           | SaveChanges produce                  | SaveChanges produce                    |
| **Restrict**          | Ninguna                                | Ninguna                                  |

En las tablas anteriores, *ninguno* puede dar lugar a una infracción de restricción. Por ejemplo, si se elimina una entidad principal o secundaria, pero no se realiza ninguna acción para cambiar la clave externa de un elemento dependiente/secundario, a continuación, la base de datos probablemente producirá en SaveChanges debido a una infracción de restricción externa.

En un nivel superior:
* Si tiene entidades que no pueden existir sin un elemento primario, y desea EF tener en cuenta para eliminar automáticamente los elementos secundarios y luego use *Cascade*.
  * Las entidades que no pueden existir sin un elemento primario normalmente se realizan uso de relaciones necesarias, para el que *Cascade* es el valor predeterminado.
* Si tiene entidades que pueden o no tener un elemento primario, y que desea EF encargarse de anular la clave externa para usted, luego utilice *ClientSetNull*
  * Entidades que pueden existir sin normalmente se realizan en un elemento primario usa de relaciones opcionales, para que *ClientSetNull* es el valor predeterminado.
  * Si desea que la base de datos para intentar propagar los valores null para las claves externas secundarios incluso cuando la entidad secundaria no está cargada, a continuación, use *SetNull*. Sin embargo, tenga en cuenta que la base de datos debe admitir esto, y configurar la base de datos similar al siguiente puede dar lugar a otras restricciones, lo que en la práctica a menudo hace poco práctico si esta opción. Se trata de por qué *SetNull* no es el valor predeterminado.
* Si no desea que EF Core para eliminar una entidad automáticamente o null automáticamente a la clave externa, use alguna vez *restringir*. Tenga en cuenta que esto requiere que el código mantener sus valores de clave externas y las entidades secundarias sincronizadas manualmente en caso contrario, las excepciones de restricción se producirá.

> [!NOTE]
> En el núcleo de EF, a diferencia de EF6, efectos en cascada no realizan inmediatamente, pero en su lugar sólo cuando se llama a SaveChanges.

> [!NOTE]  
> **Cambios en EF Core 2.0:** en versiones anteriores, *Restrict* haría que las propiedades de clave externa opcionales marcas entidades dependientes se establece en null y era el valor predeterminado comportamiento de eliminación para relaciones opcionales. En EF Core 2.0, el *ClientSetNull* se introdujo para representar ese comportamiento y se ha convertido el valor predeterminado para las relaciones opcionales. El comportamiento de *restringir* se ajustó para nunca tienen efectos secundarios en entidades dependientes.

## <a name="entity-deletion-examples"></a>Ejemplos de eliminación de entidad

El código siguiente forma parte de un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que puede descargarse y ejecutarse. El ejemplo muestra lo que sucede para cada comportamiento de eliminación de relaciones obligatorios y opcionales cuando se elimina una entidad primaria.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Analicemos cada variación para comprender lo que está sucediendo.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade con relación de obligatorio u opcional

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

* Blog se marca como eliminado
* Entradas inicialmente permanecen sin cambios desde que se ponen en cascada no se realizan hasta SaveChanges
* SaveChanges envía las eliminaciones para dependientes/hijos (o publica) y, a continuación, la entidad de seguridad/primario (blog)
* Después de guardar, todas las entidades se desasocian ya que ahora se han eliminado de la base de datos

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación necesaria

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

* Blog se marca como eliminado
* Entradas inicialmente permanecen sin cambios desde que se ponen en cascada no se realizan hasta SaveChanges
* SaveChanges intenta establecer la entrada de clave externa como null, pero se produce un error como la clave externa no aceptan valores null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación opcional

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

* Blog se marca como eliminado
* Entradas inicialmente permanecen sin cambios desde que se ponen en cascada no se realizan hasta SaveChanges
* Intentos de SaveChanges establece la clave externa de dependientes y elementos secundarios (o publica) en null antes de eliminar la entidad de seguridad/primario (blog)
* Después de guardar, se elimina la entidad de seguridad/primario (blog), pero todavía se realiza un seguimiento de los elementos dependientes/hijos (o publica)
* El seguimiento dependientes/hijos (o publica) ahora tienen valores null de clave externa y se ha quitado su referencia a la entidad de seguridad/primario eliminado (blog)

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict con relación de obligatorio u opcional

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

* Blog se marca como eliminado
* Entradas inicialmente permanecen sin cambios desde que se ponen en cascada no se realizan hasta SaveChanges
* Puesto que *restringir* indica EF para establecer automáticamente la clave externa como null, permanece no null y SaveChanges produce sin guardar los cambios

## <a name="delete-orphans-examples"></a>Ejemplos de huérfanos de eliminación

El código siguiente forma parte de un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) que se puede descargar una ejecución. El ejemplo muestra lo que sucede para cada comportamiento de eliminación de relaciones obligatorios y opcionales cuando se rompe la relación entre una entidad de principal y sus elementos secundarios o elementos dependientes. En este ejemplo, se rompe la relación mediante la eliminación de los elementos dependientes/hijos (o publica) de la propiedad de navegación de colección en la entidad de seguridad/primario (blog). Sin embargo, el comportamiento es el mismo si en su lugar, se anula la referencia de dependiente/secundario a principal/primario.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Analicemos cada variación para comprender lo que está sucediendo.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade con relación de obligatorio u opcional

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

* Entradas se marcan como modificadas porque desactivar la relación provocó la clave externa se marquen como null
  * Si la clave externa no acepta valores NULL, a continuación, el valor real no cambiará incluso aunque esté marcado como null
* SaveChanges envía las eliminaciones de elementos dependientes/los elementos secundarios (o publica)
* Después de guardar, se desasocian dependientes/hijos (o publica) ya que ahora se han eliminado de la base de datos

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación necesaria

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

* Entradas se marcan como modificadas porque desactivar la relación provocó la clave externa se marquen como null
  * Si la clave externa no acepta valores NULL, a continuación, el valor real no cambiará incluso aunque esté marcado como null
* SaveChanges intenta establecer la entrada de clave externa como null, pero se produce un error como la clave externa no aceptan valores null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relación opcional

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

* Entradas se marcan como modificadas porque desactivar la relación provocó la clave externa se marquen como null
  * Si la clave externa no acepta valores NULL, a continuación, el valor real no cambiará incluso aunque esté marcado como null
* SaveChanges establece la clave externa de dependientes y elementos secundarios (o publica) en null
* Después de guardar, dependientes/hijos (o publica) ahora tienen valores null de clave externa y se ha quitado su referencia a la entidad de seguridad/primario eliminado (blog)

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict con relación de obligatorio u opcional

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

* Entradas se marcan como modificadas porque desactivar la relación provocó la clave externa se marquen como null
  * Si la clave externa no acepta valores NULL, a continuación, el valor real no cambiará incluso aunque esté marcado como null
* Puesto que *restringir* indica EF para establecer automáticamente la clave externa como null, permanece no null y SaveChanges produce sin guardar los cambios

## <a name="cascading-to-untracked-entities"></a>En cascada a entidades sin seguimiento

Cuando se llama a *SaveChanges*, las reglas de eliminación en cascada se aplicarán a todas las entidades que se realiza el seguimiento por el contexto. Ésta es la situación en todos los ejemplos mostrados anteriormente, que es la razón por SQL se generó para eliminar la entidad de seguridad/primario (blog) y todos los elementos dependientes/nodos secundarios (o publica):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Si solo la entidad de seguridad se carga, por ejemplo, cuando se realiza una consulta para un blog sin un `Include(b => b.Posts)` incluir también entradas--, SaveChanges solo generará SQL para eliminar la entidad de seguridad o elemento primario:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Solo se eliminarán los elementos dependientes/hijos (o publica) si la base de datos tiene un comportamiento en cascada correspondiente configurado. Si usas EF para crear la base de datos, este comportamiento en cascada se configurará automáticamente.
