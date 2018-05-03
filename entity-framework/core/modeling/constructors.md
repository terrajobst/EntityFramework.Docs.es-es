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
# <a name="entity-types-with-constructors"></a><span data-ttu-id="cf84d-102">Tipos de entidad con constructores</span><span class="sxs-lookup"><span data-stu-id="cf84d-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="cf84d-103">Esta característica es nueva en EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="cf84d-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="cf84d-104">A partir de EF Core 2.1, ahora es posible definir un constructor con parámetros y hacer EF Core llamar a este constructor cuando se crea una instancia de la entidad.</span><span class="sxs-lookup"><span data-stu-id="cf84d-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="cf84d-105">Los parámetros del constructor se pueden enlazar a propiedades asignadas, o a distintos tipos de servicios que facilitan comportamientos como carga diferida.</span><span class="sxs-lookup"><span data-stu-id="cf84d-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="cf84d-106">A partir de EF Core 2.1, todos los enlaces de constructor es por convención.</span><span class="sxs-lookup"><span data-stu-id="cf84d-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="cf84d-107">Configuración de constructores específicos para utilizar está planificado para una futura versión.</span><span class="sxs-lookup"><span data-stu-id="cf84d-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="cf84d-108">Enlazar a propiedades asignadas</span><span class="sxs-lookup"><span data-stu-id="cf84d-108">Binding to mapped properties</span></span>

<span data-ttu-id="cf84d-109">Considere la posibilidad de un modelo típico de o entrada de blog:</span><span class="sxs-lookup"><span data-stu-id="cf84d-109">Consider a typical Blog/Post model:</span></span>

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

<span data-ttu-id="cf84d-110">Cuando el núcleo de EF crea instancias de estos tipos, como los resultados de una consulta, se llamar primero el constructor predeterminado sin parámetros y establece cada propiedad en el valor de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="cf84d-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="cf84d-111">Sin embargo, si EF núcleo encuentra un constructor parametrizado con los nombres de parámetro y tipos que coincidan con las de asignan propiedades, se llamará en su lugar el constructor parametrizado con los valores de esas propiedades y no establece explícitamente cada propiedad.</span><span class="sxs-lookup"><span data-stu-id="cf84d-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="cf84d-112">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="cf84d-112">For example:</span></span>

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
<span data-ttu-id="cf84d-113">Tenga en cuenta algunas cosas:</span><span class="sxs-lookup"><span data-stu-id="cf84d-113">Some things to note:</span></span>
* <span data-ttu-id="cf84d-114">No todas las propiedades que necesite tener parámetros de constructor.</span><span class="sxs-lookup"><span data-stu-id="cf84d-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="cf84d-115">Por ejemplo, no se establece la propiedad Post.Content por ningún parámetro de constructor para que EF principales se establece después de llamar al constructor de la manera normal.</span><span class="sxs-lookup"><span data-stu-id="cf84d-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="cf84d-116">Los nombres y tipos de parámetros deben coincidir con los tipos de propiedad y los nombres, excepto en que las propiedades pueden ser mayúsculas y minúsculas Pascal: mientras que los parámetros son con grafía camel.</span><span class="sxs-lookup"><span data-stu-id="cf84d-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="cf84d-117">Núcleo EF no puede establecer las propiedades de navegación (como Blog o publicaciones anteriores) mediante un constructor.</span><span class="sxs-lookup"><span data-stu-id="cf84d-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="cf84d-118">El constructor puede ser public, private, o tiene cualquier otro tipo de accesibilidad.</span><span class="sxs-lookup"><span data-stu-id="cf84d-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="cf84d-119">Propiedades de sólo lectura</span><span class="sxs-lookup"><span data-stu-id="cf84d-119">Read-only properties</span></span>

<span data-ttu-id="cf84d-120">Una vez que se establecen mediante el constructor puede tener sentido hacer algunas de ellas de sólo lectura.</span><span class="sxs-lookup"><span data-stu-id="cf84d-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="cf84d-121">EF Core es compatible con, pero hay algunas cosas que debe tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="cf84d-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="cf84d-122">No se asignan las propiedades sin establecedores por convención.</span><span class="sxs-lookup"><span data-stu-id="cf84d-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="cf84d-123">(Si lo hace, tiende a asignar propiedades que no se deben asignar, por ejemplo, las propiedades computadas.)</span><span class="sxs-lookup"><span data-stu-id="cf84d-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="cf84d-124">Utilizando los valores de clave generados automáticamente, requiere una propiedad clave que es de lectura y escritura, ya que el valor de la clave debe establecerse mediante el generador de claves al insertar nuevas entidades.</span><span class="sxs-lookup"><span data-stu-id="cf84d-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="cf84d-125">Una manera fácil de evitar estas situaciones es utilizar establecedores privados.</span><span class="sxs-lookup"><span data-stu-id="cf84d-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="cf84d-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="cf84d-126">For example:</span></span>
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
<span data-ttu-id="cf84d-127">Núcleo EF ve una propiedad con un establecedor privada como lectura y escritura, lo que significa que todas las propiedades se asignan como antes y todavía de la clave puede ser generado de almacén.</span><span class="sxs-lookup"><span data-stu-id="cf84d-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="cf84d-128">Es una alternativa al uso de establecedores privados que propiedades realmente de sólo lectura y agregar asignación más explícita en OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="cf84d-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="cf84d-129">Del mismo modo, algunas propiedades pueden quitarse completamente y se reemplazan con campos sólo.</span><span class="sxs-lookup"><span data-stu-id="cf84d-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="cf84d-130">Por ejemplo, tenga en cuenta estos tipos de entidades:</span><span class="sxs-lookup"><span data-stu-id="cf84d-130">For example, consider these entity types:</span></span>

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
<span data-ttu-id="cf84d-131">Y esta configuración en OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="cf84d-131">And this configuration in OnModelCreating:</span></span>
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
<span data-ttu-id="cf84d-132">Cosas que tenga en cuenta:</span><span class="sxs-lookup"><span data-stu-id="cf84d-132">Things to note:</span></span>
* <span data-ttu-id="cf84d-133">La "propiedad" de la clave ahora es un campo.</span><span class="sxs-lookup"><span data-stu-id="cf84d-133">The key "property" is now a field.</span></span> <span data-ttu-id="cf84d-134">No es un `readonly` de campo para que puedan utilizarse las claves generadas por el almacén.</span><span class="sxs-lookup"><span data-stu-id="cf84d-134">It is not a `readonly` field so that store-generated keys can be used.</span></span> 
* <span data-ttu-id="cf84d-135">Las demás propiedades son propiedades de sólo lectura sólo establecer en el constructor.</span><span class="sxs-lookup"><span data-stu-id="cf84d-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="cf84d-136">Si el valor de clave principal sólo se establece por EF o leer desde la base de datos, no hay ninguna necesidad para incluirlo en el constructor.</span><span class="sxs-lookup"><span data-stu-id="cf84d-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="cf84d-137">Esto deja la clave "propiedad" como un campo simple y deja claro que no debe establecerse explícitamente al crear nuevos blogs o mensajes.</span><span class="sxs-lookup"><span data-stu-id="cf84d-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="cf84d-138">Este código producirá '169' indicando que nunca se utiliza el campo de advertencia del compilador.</span><span class="sxs-lookup"><span data-stu-id="cf84d-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="cf84d-139">Pueden omitirse puesto que en realidad EF núcleo está utilizando el campo de manera extralingüístico.</span><span class="sxs-lookup"><span data-stu-id="cf84d-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="cf84d-140">Inyección de servicios</span><span class="sxs-lookup"><span data-stu-id="cf84d-140">Injecting services</span></span>

<span data-ttu-id="cf84d-141">Núcleo EF también puede inyectar "servicios" al constructor de un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="cf84d-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="cf84d-142">Por ejemplo, el siguiente puede ser inyectado:</span><span class="sxs-lookup"><span data-stu-id="cf84d-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="cf84d-143">`DbContext` -la instancia del contexto actual, que también se puede escribir como su tipo derivado de DbContext</span><span class="sxs-lookup"><span data-stu-id="cf84d-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="cf84d-144">`ILazyLoader` -el servicio carga diferida--consulte la [documentación de carga diferida](../querying/related-data.md) para obtener más detalles</span><span class="sxs-lookup"><span data-stu-id="cf84d-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="cf84d-145">`Action<object, string>` -un delegado carga diferida--consulte la [documentación de carga diferida](../querying/related-data.md) para obtener más detalles</span><span class="sxs-lookup"><span data-stu-id="cf84d-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="cf84d-146">`IEntityType` -los metadatos EF núcleo asociados con este tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="cf84d-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="cf84d-147">A partir de EF Core 2.1, sólo los servicios conocidos por núcleo EF pueden ser inyectados.</span><span class="sxs-lookup"><span data-stu-id="cf84d-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="cf84d-148">Soporte para insertar los servicios de aplicaciones está siendo considerado para una futura versión.</span><span class="sxs-lookup"><span data-stu-id="cf84d-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="cf84d-149">Por ejemplo, puede utilizarse un DbContext insertado para selectivamente tener acceso a la base de datos para obtener información acerca de las entidades relacionadas sin necesidad de cargar todos.</span><span class="sxs-lookup"><span data-stu-id="cf84d-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="cf84d-150">En el siguiente ejemplo esto se utiliza para obtener el número de publicaciones en un blog sin necesidad de cargar las publicaciones:</span><span class="sxs-lookup"><span data-stu-id="cf84d-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

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
<span data-ttu-id="cf84d-151">Algunas cosas a tener en cuenta acerca de esto:</span><span class="sxs-lookup"><span data-stu-id="cf84d-151">A few things to notice about this:</span></span>
* <span data-ttu-id="cf84d-152">El constructor es privado, ya que siempre se llama por núcleo de EF y no hay otro constructor público para uso general.</span><span class="sxs-lookup"><span data-stu-id="cf84d-152">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="cf84d-153">El código que utiliza el servicio insertado (es decir, el contexto) es defensivo contra él está `null` para controlar los casos donde Core EF no está creando la instancia.</span><span class="sxs-lookup"><span data-stu-id="cf84d-153">The code using the injected service (i.e. the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="cf84d-154">Porque el servicio se almacena en una propiedad de lectura y escritura se restablecerán cuando se asocia la entidad a una nueva instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="cf84d-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="cf84d-155">Inyectar el DbContext como esto a menudo se considera un antipatrón puesto que une los tipos de entidad directamente al núcleo de EF.</span><span class="sxs-lookup"><span data-stu-id="cf84d-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="cf84d-156">Considere detenidamente todas las opciones antes de utilizar la inyección de servicio como éste.</span><span class="sxs-lookup"><span data-stu-id="cf84d-156">Carefully consider all options before using service injection like this.</span></span>
