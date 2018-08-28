---
title: Carga relacionados entidades - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 091493f60c732573ca63adb8c65bc28606453d34
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995930"
---
# <a name="loading-related-entities"></a>Cargar entidades relacionadas
Entity Framework admite tres formas de cargar datos relacionados: carga diligente, la carga diferida y la carga explícita. Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.  

## <a name="eagerly-loading"></a>Cargar concienzudamente  

La carga diligente es el proceso mediante el cual una consulta para un tipo de entidad también carga las entidades relacionadas como parte de la consulta. La carga diligente se logra mediante el uso del método Include. Por ejemplo, las consultas siguientes cargará blogs y todas las entradas relacionadas con cada blog.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                          .Include(b => b.Posts)
                          .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include(b => b.Posts)
                        .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                          .Include("Posts")
                          .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include("Posts")
                        .FirstOrDefault();
}
```  

Tenga en cuenta que incluir un método de extensión en el espacio de nombres System.Data.Entity así que asegúrese de que está utilizando ese espacio de nombres.  

### <a name="eagerly-loading-multiple-levels"></a>Cargar concienzudamente varios niveles  

También es posible cargar concienzudamente varios niveles de entidades relacionadas. Las consultas siguientes muestran ejemplos de cómo hacer esto para la recopilación y las propiedades de navegación de referencia.  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

Tenga en cuenta que no es actualmente posible filtrar qué entidades relacionadas se cargan. Incluir will siempre aportar en todos ellos relacionados con entidades.  

## <a name="lazy-loading"></a>Carga diferida  

La carga diferida es el proceso mediante el cual una entidad o colección de entidades se carga automáticamente desde la base de datos la primera vez que se accede a una propiedad que hace referencia a la entidad o entidades. Al utilizar tipos de entidad POCO, la carga diferida se logra mediante la creación de instancias de tipos proxy derivado y, a continuación, reemplazar propiedades virtuales para agregar el enlace de la carga. Por ejemplo, cuando se usa la clase de entidad Blog definida a continuación, las entradas relacionadas se cargará la primera vez que se tiene acceso a la propiedad de navegación de publicaciones:  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public virtual ICollection<Post> Posts { get; set; }  
}
```  

### <a name="turn-lazy-loading-off-for-serialization"></a>Activar la carga desactivado para la serialización diferida  

Serialización y la carga diferida no mezclan bien y, si no tiene cuidado puede acabar consultar toda la base de datos simplemente porque está habilitada la carga diferida. La mayoría de los serializadores funcionan mediante el acceso a cada propiedad en una instancia de un tipo. Acceso a la propiedad desencadena la carga diferida, por lo que se serializan más entidades. En esas entidades se tiene acceso a las propiedades y se cargan entidades aún más. Es una buena práctica para activar carga desactivado antes de serializar una entidad diferida. Las secciones siguientes muestran cómo hacerlo.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Al desactivar la carga diferida para las propiedades de navegación específicos  

Se puede desactivar la carga diferida de la colección de entradas mediante la realización de la propiedad de publicaciones no virtual:  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public ICollection<Post> Posts { get; set; }  
}
```  

La carga de las entradas de colección todavía se puede lograr mediante la carga diligente (consulte *cargar concienzudamente* anteriormente) o el método Load (consulte *cargar explícitamente* a continuación).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Desactivar la carga diferida para todas las entidades  

La carga diferida se puede desactivar para todas las entidades en el contexto estableciendo un indicador en la propiedad de configuración. Por ejemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

Todavía se puede lograr la carga de las entidades relacionadas mediante la carga diligente (consulte *cargar concienzudamente* anteriormente) o el método Load (consulte *cargar explícitamente* a continuación).  

## <a name="explicitly-loading"></a>Carga explícita  

Incluso con la carga diferida deshabilitada es posible cargar entidades relacionadas de forma diferida, pero debe hacerse con una llamada explícita. Para ello, utilice el método Load en la entrada de la entidad relacionada. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

Tenga en cuenta que el método de referencia debe usarse cuando una entidad tiene una propiedad de navegación a otra entidad única. Por otro lado, el método de colección debe usarse cuando una entidad tiene una propiedad de navegación a una colección de otras entidades.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Aplicar filtros al cargar explícitamente entidades relacionadas  

El método de consulta proporciona acceso a la consulta subyacente que se va a usar Entity Framework al cargar entidades relacionadas. A continuación, puede usar LINQ para aplicar filtros a la consulta antes de ejecutarlo con una llamada a un método de extensión LINQ como ToList, carga, etcetera. El método de consulta se puede usar con las propiedades de navegación de referencia y de colección, pero es muy útil para las colecciones donde se puede usar para cargar solo una parte de la colección. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

Cuando se usa el método de consulta normalmente es mejor desactivar la carga diferida para la propiedad de navegación. Esto es porque en caso contrario, toda la colección es posible que se cargan automáticamente mediante el mecanismo de carga diferida antes o después de haber ejecutado la consulta filtrada.  

Tenga en cuenta que mientras que la relación se puede especificar como una cadena en lugar de una expresión lambda, IQueryable devuelta no es genérico cuando se usa una cadena, por lo que el método de conversión de tipos suele ser necesario antes de nada útil se puede hacer con él.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>Uso de consultas para contar las entidades relacionadas sin cargarlos  

A veces resulta útil saber el número de entidades relacionadas con otra entidad en la base de datos sin incurrir en realidad en el costo de la carga de todas las entidades. El método de consulta con el método de recuento de LINQ puede usarse para hacer esto. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                          .Collection(b => b.Posts)
                          .Query()
                          .Count();
}
```  
