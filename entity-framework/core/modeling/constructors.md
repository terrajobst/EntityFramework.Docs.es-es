---
title: Tipos de entidad con constructores - EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 1b36197465fb9a6571a306d36eb1e9d885a5399e
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152470"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="46149-102">Tipos de entidad con constructores</span><span class="sxs-lookup"><span data-stu-id="46149-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="46149-103">Esta característica es nueva en EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="46149-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="46149-104">A partir de EF Core 2.1, ahora es posible definir un constructor con parámetros y que EF Core llamar a este constructor al crear una instancia de la entidad.</span><span class="sxs-lookup"><span data-stu-id="46149-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="46149-105">Los parámetros del constructor se pueden enlazar a propiedades asignadas o para distintos tipos de servicios que facilitan los comportamientos como la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="46149-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="46149-106">A partir de EF Core 2.1, todo el enlace de constructor es por convención.</span><span class="sxs-lookup"><span data-stu-id="46149-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="46149-107">Configuración de constructores específicos para usar está prevista para una versión futura.</span><span class="sxs-lookup"><span data-stu-id="46149-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="46149-108">Enlazar a propiedades asignadas</span><span class="sxs-lookup"><span data-stu-id="46149-108">Binding to mapped properties</span></span>

<span data-ttu-id="46149-109">Considere la posibilidad de un modelo de entrada de Blog/típico:</span><span class="sxs-lookup"><span data-stu-id="46149-109">Consider a typical Blog/Post model:</span></span>

``` csharp
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

<span data-ttu-id="46149-110">Cuando EF Core crea instancias de estos tipos, como los resultados de una consulta, se llamar primero al constructor sin parámetros predeterminado y, a continuación, establece cada propiedad en el valor de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="46149-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="46149-111">Sin embargo, si EF Core busca un constructor con parámetros con los nombres de parámetro y tipos que coinciden con los de asignan propiedades y después en su lugar, llamará al constructor con parámetros con valores de esas propiedades y no establece explícitamente cada propiedad.</span><span class="sxs-lookup"><span data-stu-id="46149-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="46149-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="46149-112">For example:</span></span>

``` csharp
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
<span data-ttu-id="46149-113">Algunos aspectos a tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="46149-113">Some things to note:</span></span>
* <span data-ttu-id="46149-114">No todas las propiedades deben tener los parámetros del constructor.</span><span class="sxs-lookup"><span data-stu-id="46149-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="46149-115">Por ejemplo, no se establece la propiedad Post.Content por cualquier parámetro de constructor, por lo que EF Core establecerá después de llamar al constructor de la manera normal.</span><span class="sxs-lookup"><span data-stu-id="46149-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="46149-116">Los tipos de parámetro y nombres deben coincidir con los tipos de propiedad y los nombres, excepto en que las propiedades pueden ser con grafía Pascal mientras que los parámetros son mayúsculas y minúsculas camel.</span><span class="sxs-lookup"><span data-stu-id="46149-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="46149-117">EF Core no puede establecer las propiedades de navegación (por ejemplo, Blog o publicaciones anteriores) mediante un constructor.</span><span class="sxs-lookup"><span data-stu-id="46149-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="46149-118">El constructor puede ser público, privado, o tiene cualquier otro tipo de accesibilidad.</span><span class="sxs-lookup"><span data-stu-id="46149-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="46149-119">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="46149-119">Read-only properties</span></span>

<span data-ttu-id="46149-120">Una vez que se establecen las propiedades mediante el constructor puede sentido hacer algunos de ellos de sólo lectura.</span><span class="sxs-lookup"><span data-stu-id="46149-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="46149-121">EF Core es compatible con esto, pero hay algunas cosas a tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="46149-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="46149-122">No se asignan las propiedades sin establecedores por convención.</span><span class="sxs-lookup"><span data-stu-id="46149-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="46149-123">(Si lo hace, suele asignar propiedades que no deberían asignarse, por ejemplo, las propiedades calculadas).</span><span class="sxs-lookup"><span data-stu-id="46149-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="46149-124">Utilizando los valores de clave generados automáticamente, requiere una propiedad de clave es de lectura y escritura, ya que el valor de clave debe establecerse mediante el generador de claves cuando se insertan nuevas entidades.</span><span class="sxs-lookup"><span data-stu-id="46149-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="46149-125">Una manera fácil de evitar estas situaciones es usar establecedores privados.</span><span class="sxs-lookup"><span data-stu-id="46149-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="46149-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="46149-126">For example:</span></span>
``` csharp
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
<span data-ttu-id="46149-127">EF Core, ve una propiedad con un establecedor privado de lectura y escritura, lo que significa que todas las propiedades se asignan como antes y la clave puede ser generado por el almacén.</span><span class="sxs-lookup"><span data-stu-id="46149-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="46149-128">Una alternativa al uso de establecedores privados es poner propiedades realmente de solo lectura y agregar una asignación más explícita en OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="46149-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="46149-129">Del mismo modo, algunas propiedades se pueden quitar completamente y reemplaza con solo los campos.</span><span class="sxs-lookup"><span data-stu-id="46149-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="46149-130">Por ejemplo, considere la posibilidad de estos tipos de entidad:</span><span class="sxs-lookup"><span data-stu-id="46149-130">For example, consider these entity types:</span></span>

``` csharp
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
<span data-ttu-id="46149-131">Y esta configuración en OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="46149-131">And this configuration in OnModelCreating:</span></span>
``` csharp
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
<span data-ttu-id="46149-132">Cosas a tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="46149-132">Things to note:</span></span>
* <span data-ttu-id="46149-133">La clave "property" es ahora un campo.</span><span class="sxs-lookup"><span data-stu-id="46149-133">The key "property" is now a field.</span></span> <span data-ttu-id="46149-134">No es un `readonly` campo para que se pueden usar claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="46149-134">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="46149-135">Las demás propiedades son propiedades de solo lectura que se establece solo en el constructor.</span><span class="sxs-lookup"><span data-stu-id="46149-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="46149-136">Si el valor de clave principal siempre se establece por EF o leer desde la base de datos, no hay ninguna necesidad de incluirlo en el constructor.</span><span class="sxs-lookup"><span data-stu-id="46149-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="46149-137">Esto deja la clave "property" como un campo sencillo y deja claro que no se debe establecer explícitamente al crear nuevos blogs o entradas.</span><span class="sxs-lookup"><span data-stu-id="46149-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="46149-138">Este código dará como resultado '169' indicando que nunca se usa el campo de advertencia del compilador.</span><span class="sxs-lookup"><span data-stu-id="46149-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="46149-139">Puede omitirse dado que en realidad, EF Core está usando el campo de una manera extralinguistic.</span><span class="sxs-lookup"><span data-stu-id="46149-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="46149-140">Inyección de servicios</span><span class="sxs-lookup"><span data-stu-id="46149-140">Injecting services</span></span>

<span data-ttu-id="46149-141">EF Core también puede insertar "servicios" en el constructor de un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="46149-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="46149-142">Por ejemplo, el siguiente se puede insertar:</span><span class="sxs-lookup"><span data-stu-id="46149-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="46149-143">`DbContext` -la instancia de contexto actual, que también se puede escribir como su tipo derivado de DbContext</span><span class="sxs-lookup"><span data-stu-id="46149-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="46149-144">`ILazyLoader` -el servicio de la carga diferida, vea el [documentación de la carga diferida](../querying/related-data.md) para obtener más detalles</span><span class="sxs-lookup"><span data-stu-id="46149-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="46149-145">`Action<object, string>` -un delegado de la carga diferida, vea el [documentación de la carga diferida](../querying/related-data.md) para obtener más detalles</span><span class="sxs-lookup"><span data-stu-id="46149-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="46149-146">`IEntityType` -los metadatos de EF Core asociados con este tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="46149-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="46149-147">A partir de EF Core 2.1, solo los servicios conocidos con EF Core se pueden insertar.</span><span class="sxs-lookup"><span data-stu-id="46149-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="46149-148">Soporte técnico para insertar los servicios de aplicación se está considerando para una versión futura.</span><span class="sxs-lookup"><span data-stu-id="46149-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="46149-149">Por ejemplo, puede utilizarse un DbContext insertado selectivamente acceder a la base de datos para obtener información sobre las entidades relacionadas sin tener que cargar todos ellos.</span><span class="sxs-lookup"><span data-stu-id="46149-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="46149-150">En el ejemplo siguiente se utiliza para obtener el número de publicaciones en un blog sin tener que cargar las entradas de esto:</span><span class="sxs-lookup"><span data-stu-id="46149-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

``` csharp
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
<span data-ttu-id="46149-151">Algunas cosas a tener en cuenta sobre esto:</span><span class="sxs-lookup"><span data-stu-id="46149-151">A few things to notice about this:</span></span>
* <span data-ttu-id="46149-152">El constructor es privado, ya que solo se llama mediante EF Core y no hay otro constructor público para uso general.</span><span class="sxs-lookup"><span data-stu-id="46149-152">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="46149-153">El código que usa el servicio insertado (es decir, el contexto) es estable en el mismo que se va a `null` para controlar los casos donde EF Core no está creando la instancia.</span><span class="sxs-lookup"><span data-stu-id="46149-153">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="46149-154">Porque el servicio se almacena en una propiedad de lectura/escritura se restablecerá cuando la entidad está asociada a una nueva instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="46149-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="46149-155">Inyección de DbContext como esta, a menudo se considera un antipatrón puesto que asocia los tipos de entidad directamente a EF Core.</span><span class="sxs-lookup"><span data-stu-id="46149-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="46149-156">Considere detenidamente todas las opciones antes de usar la inserción de un servicio similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="46149-156">Carefully consider all options before using service injection like this.</span></span>
