---
title: 'Datos locales: EF6'
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
caps.latest.revision: 3
ms.openlocfilehash: 79f0d2175199780d41b43088832bab808ab2fff0
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122303"
---
# <a name="local-data"></a>Datos locales
Ejecuta una consulta LINQ directamente en una clase DbSet siempre enviará una consulta a la base de datos, pero puede tener acceso a los datos que están actualmente en memoria mediante la propiedad DbSet.Local. También puede tener acceso a la EF está realizando el seguimiento de información adicional acerca de las entidades mediante los métodos DbContext.Entry y DbContext.ChangeTracker.Entries. Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.  

## <a name="using-local-to-look-at-local-data"></a>Para ver los datos locales mediante Local  

La propiedad Local de DbSet ofrece acceso sencillo a las entidades del conjunto que se realiza un seguimiento actualmente por el contexto y no se han marcado como eliminado. Acceso a la propiedad Local nunca hace que una consulta para enviarse a la base de datos. Esto significa que se utiliza normalmente después de que ya se ha realizado una consulta. El método de extensión de carga puede usarse para ejecutar una consulta para que el contexto realiza un seguimiento de los resultados. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

Si tuviéramos dos blogs en la base de datos - 'ADO.NET Blog' con un BlogId 1 - y "Blog de Visual Studio' con un BlogId 2 podríamos esperamos el siguiente resultado:  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Esto muestra tres puntos:  

- El nuevo blog de 'Mi nuevo Blog' se incluye en la colección Local, aunque lo aún no se ha guardado en la base de datos. Esta entrada de blog tiene una clave principal de cero porque la base de datos no ha generado todavía una clave de real para la entidad.  
- El Blog del ADO.NET no se incluye en la colección local, aunque todavía se está realizando un seguimiento por el contexto. Esto es porque se ha quitado de la clase DbSet, por tanto, lo marca como eliminada.  
- Cuando se utiliza DbSet para realizar una consulta el blog de marcado para su eliminación (Blog de ADO.NET) se incluye en los resultados y el nuevo blog (Blog de mi nuevo) que aún no se ha guardado en la base de datos no se incluye en los resultados. Esto es porque DbSet está realizando una consulta en la base de datos y los resultados devueltos siempre reflejan lo que está en la base de datos.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Uso a Local para agregar y quitar las entidades en el contexto  

Devuelve la propiedad Local en DbSet un [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) con eventos que se enlazó tal que se mantiene sincronizada con el contenido del contexto. Esto significa que las entidades se pueden agregar o quitar de la colección Local o la clase DbSet. También significa que las consultas que introducir nuevas entidades en el contexto dará como resultado de la colección Local que se va a actualizar con esas entidades. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

Supongamos que contamos con unas pocas entradas etiquetadas con 'entity framework' y 'asp.net' la salida podría tener un aspecto similar al siguiente:  

```  
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

Esto muestra tres puntos:  

- La nueva publicación de 'Novedades de EF' que se agregó local colección realiza un seguimiento el contexto en estado Added. Por lo tanto, se insertará en la base de datos cuando se llama a SaveChanges.  
- La entrada que se ha quitado de la colección Local (Guía básica de EF) ahora se marca como eliminada en el contexto. Por lo tanto, se eliminará de la base de datos cuando se llama a SaveChanges.  
- La entrada adicional (Guía básica de ASP.NET) cargada en el contexto con la segunda consulta se agrega automáticamente a la colección Local.  

Una última cosa a tener en cuenta sobre Local es porque es que un ObservableCollection de rendimiento no es excelente para un gran número de entidades. Por lo tanto, si está trabajando con miles de entidades en el contexto puede no ser aconsejable usar a Local.  

## <a name="using-local-for-wpf-data-binding"></a>Uso a Local para el enlace de datos WPF  

La propiedad Local en DbSet puede utilizarse directamente para el enlace de datos en una aplicación de WPF porque es una instancia de ObservableCollection. Como se describe en las secciones anteriores, de que esto significa que automáticamente permanecerá sincronizada con el contenido del contexto y configurará automáticamente y el contenido del contexto estar sincronizados con él. Tenga en cuenta que es necesario rellenar previamente la colección Local con los datos para que haya algo para enlazar a, ya que Local nunca produce una consulta de base de datos.  

Esto no es un lugar adecuado para obtener un ejemplo de enlace de datos WPF completo, pero los elementos clave son:  

- Programa de instalación de un origen de enlace  
- Enlazarla a la propiedad Local del conjunto de  
- Rellenar a Local mediante una consulta a la base de datos.  

## <a name="wpf-binding-to-navigation-properties"></a>Enlace de WPF a las propiedades de navegación  

Si está realizando enlaces de datos principal-detalle es posible que desee enlazar la vista de detalle para una propiedad de navegación de una de las entidades. Una manera fácil para solucionar este problema es usar una colección ObservableCollection para la propiedad de navegación. Por ejemplo:  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>Uso a Local para limpiar las entidades de SaveChanges  

En la mayoría de los casos las entidades que se quita de una propiedad de navegación no automáticamente marcará como eliminados en el contexto. Por ejemplo, si quita un objeto de entrada de la colección Blog.Posts registrar no se eliminan automáticamente cuando se llama a SaveChanges. Si debe eliminarse, a continuación, deberá buscar estas entidades pendientes y los marca como eliminado antes de llamar a SaveChanges o como parte de un SaveChanges invalidado. Por ejemplo:  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

El código anterior usa la colección Local para buscar todas las publicaciones y las marcas de cualquier usuario que no tiene una referencia blog como eliminado. La llamada ToList es necesaria porque de lo contrario, se modificará la colección mediante la eliminación llamar mientras se está enumerando. En la mayoría de los otra casos puede consultar directamente en la propiedad Local sin usar primero ToList.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Uso de enlace de datos Local y ToBindingList for Windows Forms  

Formularios de Windows no admite el enlace de datos con plena fidelidad con ObservableCollection directamente. Sin embargo, todavía puede usar la propiedad DbSet Local para el enlace de datos para obtener todas las ventajas descritas en las secciones anteriores. Esto se logra mediante el método de extensión ToBindingList que crea un [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementación respaldada por el objeto ObservableCollection Local.  

Esto no es un lugar adecuado para obtener un ejemplo de enlace de datos de Windows Forms completo, pero los elementos clave son:  

- Configurar un origen de enlace de objetos  
- Enlazarla a la propiedad Local de su conjunto mediante Local.ToBindingList()  
- Rellenar a Local mediante una consulta a la base de datos  

## <a name="getting-detailed-information-about-tracked-entities"></a>Obtener información detallada acerca de las entidades sometidas a seguimiento  

Muchos de los ejemplos en esta serie de usan el método de entrada para devolver una instancia de DbEntityEntry para una entidad. Este objeto de entrada, a continuación, actúa como punto de partida para recopilar información acerca de la entidad, como su estado actual, así como para realizar operaciones en la entidad como la carga explícita de una entidad relacionada.  

Los métodos de las entradas devuelven objetos DbEntityEntry para muchas o todas las entidades que realiza un seguimiento del contexto. Esto le permite recopilar información o realizar operaciones en varias entidades en lugar de simplemente una sola entrada. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

Observará que presentamos una clase de autor y el lector en el ejemplo: ambas clases implementan la interfaz de IPerson.  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

Supongamos que tenemos los datos siguientes en la base de datos:

Blog de BlogId = 1 y nombre = 'Blog de ADO.NET'  
Blog de BlogId = 2 y el nombre = "Blog de Visual Studio"  
Blog de BlogId = 3 y el nombre = 'Blog de .NET Framework'  
Autor con AuthorId = 1 y nombre = 'Joe Calvo'  
Lector con ReaderId = 1 y nombre = "John Doe"  

El resultado de ejecutar el código sería:  

```  
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

Estos ejemplos muestran varios puntos:  

- Los métodos de entradas devuelven las entradas para las entidades en todos los Estados, incluida Deleted. Esta opción para comparar Local, que excluye elimina entidades.  
- Cuando se usa el método de entradas no genérica, se devuelven las entradas para todos los tipos de entidad. Cuando se usa el método genérico entradas sólo se devuelven las entradas para las entidades que son instancias del tipo genérico. Se usó anteriormente para obtener las entradas para todos los blogs. También se usó para obtener las entradas de todas las entidades que implementan IPerson. Esto demuestra que el tipo genérico no tiene un tipo de entidad real.  
- Para filtrar los resultados devueltos se puede usar LINQ para objetos. Se usó anteriormente para buscar las entidades de cualquier tipo, siempre y cuando se modifican.  

Tenga en cuenta que las instancias de DbEntityEntry siempre contienen una entidad que no sea null. Entradas de código auxiliar y las entradas de relación no se representan como instancias de DbEntityEntry por lo que no es necesario para filtrar estos.
