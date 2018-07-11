---
title: Tipos de entidad con constructores - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 80c7ee04d3bb0dd45b66ec7d6fec5ee7e3343026
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949210"
---
# <a name="entity-types-with-constructors"></a>Tipos de entidad con constructores

> [!NOTE]  
> Esta característica es nueva en EF Core 2.1.

A partir de EF Core 2.1, ahora es posible definir un constructor con parámetros y que EF Core llamar a este constructor al crear una instancia de la entidad. Los parámetros del constructor se pueden enlazar a propiedades asignadas o para distintos tipos de servicios que facilitan los comportamientos como la carga diferida.

> [!NOTE]  
> A partir de EF Core 2.1, todo el enlace de constructor es por convención. Configuración de constructores específicos para usar está prevista para una versión futura.

## <a name="binding-to-mapped-properties"></a>Enlazar a propiedades asignadas

Considere la posibilidad de un modelo de entrada de Blog/típico:

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

Cuando EF Core crea instancias de estos tipos, como los resultados de una consulta, se llamar primero al constructor sin parámetros predeterminado y, a continuación, establece cada propiedad en el valor de la base de datos. Sin embargo, si EF Core busca un constructor con parámetros con los nombres de parámetro y tipos que coinciden con los de asignan propiedades y después en su lugar, llamará al constructor con parámetros con valores de esas propiedades y no establece explícitamente cada propiedad. Por ejemplo:

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
Algunos aspectos a tener en cuenta:
* No todas las propiedades deben tener los parámetros del constructor. Por ejemplo, no se establece la propiedad Post.Content por cualquier parámetro de constructor, por lo que EF Core establecerá después de llamar al constructor de la manera normal.
* Los tipos de parámetro y nombres deben coincidir con los tipos de propiedad y los nombres, excepto en que las propiedades pueden ser con grafía Pascal mientras que los parámetros son mayúsculas y minúsculas camel.
* EF Core no puede establecer las propiedades de navegación (por ejemplo, Blog o publicaciones anteriores) mediante un constructor.
* El constructor puede ser público, privado, o tiene cualquier otro tipo de accesibilidad.

### <a name="read-only-properties"></a>Propiedades de solo lectura

Una vez que se establecen las propiedades mediante el constructor puede sentido hacer algunos de ellos de sólo lectura. EF Core es compatible con esto, pero hay algunas cosas a tener en cuenta:
* No se asignan las propiedades sin establecedores por convención. (Si lo hace, suele asignar propiedades que no deberían asignarse, por ejemplo, las propiedades calculadas).
* Utilizando los valores de clave generados automáticamente, requiere una propiedad de clave es de lectura y escritura, ya que el valor de clave debe establecerse mediante el generador de claves cuando se insertan nuevas entidades.

Una manera fácil de evitar estas situaciones es usar establecedores privados. Por ejemplo:
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
EF Core, ve una propiedad con un establecedor privado de lectura y escritura, lo que significa que todas las propiedades se asignan como antes y la clave puede ser generado por el almacén.

Una alternativa al uso de establecedores privados es poner propiedades realmente de solo lectura y agregar una asignación más explícita en OnModelCreating. Del mismo modo, algunas propiedades se pueden quitar completamente y reemplaza con solo los campos. Por ejemplo, considere la posibilidad de estos tipos de entidad:

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
Cosas a tener en cuenta:
* La clave "property" es ahora un campo. No es un `readonly` campo para que se pueden usar claves generadas por el almacén.
* Las demás propiedades son propiedades de solo lectura que se establece solo en el constructor.
* Si el valor de clave principal siempre se establece por EF o leer desde la base de datos, no hay ninguna necesidad de incluirlo en el constructor. Esto deja la clave "property" como un campo sencillo y deja claro que no se debe establecer explícitamente al crear nuevos blogs o entradas.

> [!NOTE]  
> Este código dará como resultado '169' indicando que nunca se usa el campo de advertencia del compilador. Puede omitirse dado que en realidad, EF Core está usando el campo de una manera extralinguistic.

## <a name="injecting-services"></a>Inyección de servicios

EF Core también puede insertar "servicios" en el constructor de un tipo de entidad. Por ejemplo, el siguiente se puede insertar:
* `DbContext` -la instancia de contexto actual, que también se puede escribir como su tipo derivado de DbContext
* `ILazyLoader` -el servicio de la carga diferida, vea el [documentación de la carga diferida](../querying/related-data.md) para obtener más detalles
* `Action<object, string>` -un delegado de la carga diferida, vea el [documentación de la carga diferida](../querying/related-data.md) para obtener más detalles
* `IEntityType` -los metadatos de EF Core asociados con este tipo de entidad

> [!NOTE]  
> A partir de EF Core 2.1, solo los servicios conocidos con EF Core se pueden insertar. Soporte técnico para insertar los servicios de aplicación se está considerando para una versión futura.

Por ejemplo, puede utilizarse un DbContext insertado selectivamente acceder a la base de datos para obtener información sobre las entidades relacionadas sin tener que cargar todos ellos. En el ejemplo siguiente se utiliza para obtener el número de publicaciones en un blog sin tener que cargar las entradas de esto:

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
Algunas cosas a tener en cuenta sobre esto:
* El constructor es privado, ya que solo se llama mediante EF Core y no hay otro constructor público para uso general.
* El código que usa el servicio insertado (es decir, el contexto) es estable en el mismo que se va a `null` para controlar los casos donde EF Core no está creando la instancia.
* Porque el servicio se almacena en una propiedad de lectura/escritura se restablecerá cuando la entidad está asociada a una nueva instancia de contexto.

> [!WARNING]  
> Inyección de DbContext como esta, a menudo se considera un antipatrón puesto que asocia los tipos de entidad directamente a EF Core. Considere detenidamente todas las opciones antes de usar la inserción de un servicio similar al siguiente.
