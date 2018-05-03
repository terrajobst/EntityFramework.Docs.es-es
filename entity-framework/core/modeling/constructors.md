---
title: Tipos de entidad con constructores - Core EF
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 8cea624c295f99ef54cb8b4758642eade03c235e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2018
---
# <a name="entity-types-with-constructors"></a>Tipos de entidad con constructores

> [!NOTE]  
> Esta característica es nueva en EF Core 2.1.

A partir de EF Core 2.1, ahora es posible definir un constructor con parámetros y hacer EF Core llamar a este constructor cuando se crea una instancia de la entidad. Los parámetros del constructor se pueden enlazar a propiedades asignadas, o a distintos tipos de servicios que facilitan comportamientos como carga diferida.

> [!NOTE]  
> A partir de EF Core 2.1, todos los enlaces de constructor es por convención. Configuración de constructores específicos para utilizar está planificado para una futura versión.

## <a name="binding-to-mapped-properties"></a>Enlazar a propiedades asignadas

Considere la posibilidad de un modelo típico de o entrada de blog:

```Csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

Cuando el núcleo de EF crea instancias de estos tipos, como los resultados de una consulta, se llamar primero el constructor predeterminado sin parámetros y establece cada propiedad en el valor de la base de datos. Sin embargo, si EF núcleo encuentra un constructor parametrizado con los nombres de parámetro y tipos que coincidan con las de asignan propiedades, se llamará en su lugar el constructor parametrizado con los valores de esas propiedades y no establece explícitamente cada propiedad. Por ejemplo:

```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
Tenga en cuenta algunas cosas:
* No todas las propiedades que necesite tener parámetros de constructor. Por ejemplo, no se establece la propiedad Post.Content por ningún parámetro de constructor para que EF principales se establece después de llamar al constructor de la manera normal.
* Los nombres y tipos de parámetros deben coincidir con los tipos de propiedad y los nombres, excepto en que las propiedades pueden ser mayúsculas y minúsculas Pascal: mientras que los parámetros son con grafía camel.
* Núcleo EF no puede establecer las propiedades de navegación (como Blog o publicaciones anteriores) mediante un constructor.
* El constructor puede ser public, private, o tiene cualquier otro tipo de accesibilidad.

### <a name="read-only-properties"></a>Propiedades de sólo lectura

Una vez que se establecen mediante el constructor puede tener sentido hacer algunas de ellas de sólo lectura. EF Core es compatible con, pero hay algunas cosas que debe tener en cuenta:
* No se asignan las propiedades sin establecedores por convención. (Si lo hace, tiende a asignar propiedades que no se deben asignar, por ejemplo, las propiedades computadas.)
* Utilizando los valores de clave generados automáticamente, requiere una propiedad clave que es de lectura y escritura, ya que el valor de la clave debe establecerse mediante el generador de claves al insertar nuevas entidades.

Una manera fácil de evitar estas situaciones es utilizar establecedores privados. Por ejemplo:
```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}
```
Núcleo EF ve una propiedad con un establecedor privada como lectura y escritura, lo que significa que todas las propiedades se asignan como antes y todavía de la clave puede ser generado de almacén.

Es una alternativa al uso de establecedores privados que propiedades realmente de sólo lectura y agregar asignación más explícita en OnModelCreating. Del mismo modo, algunas propiedades pueden quitarse completamente y se reemplazan con campos sólo. Por ejemplo, tenga en cuenta estos tipos de entidades:

```Csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```
Y esta configuración en OnModelCreating:
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```
Cosas que tenga en cuenta:
* La "propiedad" de la clave ahora es un campo. No es un `readonly` de campo para que puedan utilizarse las claves generadas por el almacén. 
* Las demás propiedades son propiedades de sólo lectura sólo establecer en el constructor.
* Si el valor de clave principal sólo se establece por EF o leer desde la base de datos, no hay ninguna necesidad para incluirlo en el constructor. Esto deja la clave "propiedad" como un campo simple y deja claro que no debe establecerse explícitamente al crear nuevos blogs o mensajes.

> [!NOTE]  
> Este código producirá '169' indicando que nunca se utiliza el campo de advertencia del compilador. Pueden omitirse puesto que en realidad EF núcleo está utilizando el campo de manera extralingüístico.

## <a name="injecting-services"></a>Inyección de servicios

Núcleo EF también puede inyectar "servicios" al constructor de un tipo de entidad. Por ejemplo, el siguiente puede ser inyectado:
* `DbContext` -la instancia del contexto actual, que también se puede escribir como su tipo derivado de DbContext
* `ILazyLoader` -el servicio carga diferida--consulte la [documentación de carga diferida](../querying/related-data.md) para obtener más detalles
* `Action<object, string>` -un delegado carga diferida--consulte la [documentación de carga diferida](../querying/related-data.md) para obtener más detalles
* `IEntityType` -los metadatos EF núcleo asociados con este tipo de entidad

> [!NOTE]  
> A partir de EF Core 2.1, sólo los servicios conocidos por núcleo EF pueden ser inyectados. Soporte para insertar los servicios de aplicaciones está siendo considerado para una futura versión.

Por ejemplo, puede utilizarse un DbContext insertado para selectivamente tener acceso a la base de datos para obtener información acerca de las entidades relacionadas sin necesidad de cargar todos. En el siguiente ejemplo esto se utiliza para obtener el número de publicaciones en un blog sin necesidad de cargar las publicaciones:

```Csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
Algunas cosas a tener en cuenta acerca de esto:
* El constructor es privado, ya que siempre se llama por núcleo de EF y no hay otro constructor público para uso general.
* El código que utiliza el servicio insertado (es decir, el contexto) es defensivo contra él está `null` para controlar los casos donde Core EF no está creando la instancia.
* Porque el servicio se almacena en una propiedad de lectura y escritura se restablecerán cuando se asocia la entidad a una nueva instancia de contexto.

> [!WARNING]  
> Inyectar el DbContext como esto a menudo se considera un antipatrón puesto que une los tipos de entidad directamente al núcleo de EF. Considere detenidamente todas las opciones antes de utilizar la inyección de servicio como éste.
