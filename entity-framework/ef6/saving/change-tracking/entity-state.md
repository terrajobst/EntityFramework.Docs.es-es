---
title: Trabajar con estados de entidad - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: e5f9ca4aa41e64141fa63a1e5fcf4d4775d67d24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899657"
---
# <a name="working-with-entity-states"></a>Trabajar con estados de entidad
Este tema tratará cómo agregar y asociar entidades a un contexto y cómo Entity Framework procesa estos durante SaveChanges.
Entity Framework se encarga de debe pertenecer el estado de entidades mientras están conectados a un contexto, pero en escenarios sin conexión o de N niveles, puede informar a EF qué estado de las entidades de seguimiento.
Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.  

## <a name="entity-states-and-savechanges"></a>Estados de entidad y SaveChanges

Una entidad puede estar en uno de los cinco estados tal como se define por la enumeración EntityState. Estos estados son:  

- Se ha agregado: la entidad realiza el seguimiento del contexto, pero aún no existe en la base de datos  
- Sin cambios: la entidad que se realiza el seguimiento del contexto y existe en la base de datos y no han cambiado sus valores de propiedad de los valores de la base de datos  
- Modificado: la entidad que se realiza el seguimiento del contexto y existe en la base de datos, y se han modificado algunos o todos sus valores de propiedad  
- Eliminado: la entidad está realizando el contexto y existe en la base de datos, pero se ha marcado para su eliminación de la base de datos la próxima vez que se llama a SaveChanges  
- Desasociado: la entidad no se realiza el seguimiento del contexto  

SaveChanges hace cosas diferentes para las entidades en estados diferentes:  

- Las entidades sin cambios no se vinculan a SaveChanges. Las actualizaciones no se envían a la base de datos para las entidades en estado Unchanged.  
- Agrega las entidades se insertan en la base de datos y, a continuación, se convierten en no se modifican cuando SaveChanges se devuelve.  
- Entidades modificadas se actualizan en la base de datos y, a continuación, se convierten en no se modifican cuando SaveChanges se devuelve.  
- Entidades eliminadas se eliminan de la base de datos y, a continuación, se desasocian del contexto.  

Los ejemplos siguientes muestran formas en que se puede cambiar el estado de una entidad o un gráfico de la entidad.  

## <a name="adding-a-new-entity-to-the-context"></a>Agregar una nueva entidad al contexto  

Una nueva entidad se puede agregar al contexto llamando al método Add en DbSet.
Esto coloca la entidad en el estado Added, lo que significa que se va a insertar en la base de datos la próxima vez que se llama a SaveChanges.
Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

Otra manera de agregar una nueva entidad al contexto es cambiar su estado en Added. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Por último, puede agregar una nueva entidad al contexto enlazando hasta otra entidad que ya se está realizando un seguimiento.
Esto podría ser agregando la nueva entidad a la propiedad de navegación de colección de otra entidad o estableciendo una propiedad de navegación de referencia de otra entidad para que apunte a la nueva entidad. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

Tenga en cuenta que para todos estos ejemplos si la entidad que se va a agregar tiene referencias a otras entidades que no están todavía realiza un seguimiento, a continuación, estas nuevas entidades también se agregará al contexto y se insertará en la base de datos la próxima vez que se llama a SaveChanges.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Asociar una entidad existente en el contexto  

Si tiene una entidad que ya sabe que existe en la base de datos pero que no actualmente que se realiza el seguimiento del contexto, a continuación, puede indicar el contexto para realizar el seguimiento de la entidad mediante el método Attach en DbSet. La entidad estará en el estado sin modificar en el contexto. Por ejemplo:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

Tenga en cuenta que no se realizará ningún cambio a la base de datos si se llama a SaveChanges sin tener que realizar cualquier otra manipulación de la entidad adjunta. Esto es porque la entidad está en estado Unchanged.  

Otra manera de asociar una entidad existente en el contexto es cambiar su estado en Unchanged. Por ejemplo:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Tenga en cuenta que para ambos ejemplos si la entidad que se va a adjuntar tiene referencias a otras entidades que no se hace seguimiento, a continuación, estas nuevas entidades también adjuntará al contexto en estado Unchanged.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Asociar una existente, pero la entidad modificada en el contexto  

Si tiene una entidad que ya sabe que existe en la base de datos, pero que es posible que se realizaron cambios, a continuación, puede indicar el contexto para asociar la entidad y establecer su estado en Modified.
Por ejemplo:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

Cuando se cambia el estado en Modified todas las propiedades de la entidad se marcarán como modificados y todos los valores de propiedad se enviará a la base de datos cuando se llama a SaveChanges.  

Tenga en cuenta que si la entidad que se va a adjuntar tiene referencias a otras entidades que no se hace seguimiento, a continuación, estas nuevas entidades adjuntará al contexto en estado Unchanged, no estará automáticamente Modified.
Si tiene varias entidades que deben marcarse como Modified debe establecer el estado individualmente para cada una de estas entidades.  

## <a name="changing-the-state-of-a-tracked-entity"></a>Cambiar el estado de una entidad con seguimiento  

Puede cambiar el estado de una entidad que se estuvieran rastreando estableciendo la propiedad de estado en su entrada. Por ejemplo:  

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

Tenga en cuenta que una llamada a Add o Attach para una entidad que ya se realiza el seguimiento también sirve para cambiar el estado de la entidad. Por ejemplo, una llamada a adjuntar para una entidad que está actualmente en estado Added cambiará su estado en Unchanged.  

## <a name="insert-or-update-pattern"></a>Insertar o actualizar el modelo  

Es un patrón común para algunas aplicaciones agregar una entidad nueva (resultante en una inserción de una base de datos) o asociar una entidad como existente y marcarlo como modificado (lo que conlleva una actualización de la base de datos) según el valor de la clave principal.
Por ejemplo, cuando se usa la base de datos genera las claves principales de entero es común para tratar una entidad con una cero clave como nueva y una entidad con una clave distinta de cero como existente.
Este patrón puede lograrse estableciendo el estado de la entidad basándose en una comprobación del valor de clave principal. Por ejemplo:  

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

Tenga en cuenta que al cambiar el estado en Modified todas las propiedades de la entidad se marcarán como modificados y todos los valores de propiedad se enviará a la base de datos cuando se llama a SaveChanges.  
