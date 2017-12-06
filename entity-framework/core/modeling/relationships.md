---
title: Relaciones - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="relationships"></a>Relaciones

Una relación define cómo dos entidades se relacionan entre sí. En una base de datos relacional, se representa mediante una restricción foreign key.

> [!NOTE]  
> La mayoría de los ejemplos de este artículo utiliza una relación uno a varios para demostrar conceptos. Para obtener ejemplos de las relaciones de uno a uno y varios a varios, vea el [otros patrones de relación](#other-relationship-patterns) sección al final del artículo.

## <a name="definition-of-terms"></a>Definición de términos

Hay una serie de términos que se usan para describir las relaciones

* **Entidad dependiente:** se trata de la entidad que contiene las propiedades de clave externas. A veces se denomina 'child' de la relación.

* **Entidad de seguridad:** se trata de la entidad que contiene las propiedades de clave principal y alternativa. A veces se denomina 'parent' de la relación.

* **Clave externa:** las propiedades de la entidad dependiente que se utiliza para almacenar los valores de la propiedad de clave principal que está relacionado con la entidad.

* **Clave principal:** las propiedades que identifica de forma única la entidad de seguridad. Esto puede ser la clave principal o una clave alternativa.

* **Propiedad de navegación:** una propiedad definida en la entidad principal o dependiente que contiene las referencias a las entidades compradoras relacionados.

  * **Propiedad de navegación de colección:** una propiedad de navegación que contiene referencias a muchas de las entidades relacionadas.

  * **Referencia de propiedad de navegación:** una propiedad de navegación que contiene una referencia a una única entidad relacionada.

  * **Propiedad de navegación inversa:** al hablar de una propiedad de navegación determinado, este término hace referencia a la propiedad de navegación en el otro extremo de la relación.

La lista de código siguiente muestra una relación de uno a varios entre `Blog` y`Post`

* `Post`es la entidad dependiente

* `Blog`es la entidad de seguridad

* `Post.BlogId`es la clave externa

* `Blog.BlogId`es la clave principal (en este caso es una clave principal en lugar de una clave alternativa)

* `Post.Blog`es una propiedad de navegación de referencia

* `Blog.Posts`es una propiedad de navegación de colección

* `Post.Blog`es la propiedad de navegación inversa de `Blog.Posts` (y viceversa)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Convenciones

Por convención, se creará una relación cuando hay una propiedad de navegación que se detecten en un tipo. Una propiedad se considera una propiedad de navegación, si el tipo a que apunta no se puede asignar como un tipo escalar por el proveedor de base de datos actual.

> [!NOTE]  
> Las relaciones que se detectan por convención siempre tendrán como destino la clave principal de la entidad de seguridad. Que tenga como destino una clave alternativa, debe realizar configuración adicional mediante la API fluida.

### <a name="fully-defined-relationships"></a>Relaciones definidas totalmente

El modelo más común para las relaciones es tener propiedades de navegación definidas en ambos extremos de la relación y una propiedad de clave externa definidas en la clase de entidad dependiente.

* Si se encuentra algún par de propiedades de navegación entre dos tipos, configurará como propiedades de navegación inversa de la misma relación.

* Si la entidad dependiente contiene una propiedad denominada `<primary key property name>`, `<navigation property name><primary key property name>`, o `<principal entity name><primary key property name>` , a continuación, se configurará como la clave externa.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Si hay varias propiedades de navegación definidas entre dos tipos (es decir, más de un par distinto de las exploraciones que señalan a entre sí), a continuación, no se creará ninguna relación por convención y debe configurarlos manualmente para identificar cómo propiedades de navegación emparejar las.

### <a name="no-foreign-key-property"></a>Ninguna propiedad de clave externa

Aunque se recomienda tener una propiedad de clave externa definida en la clase de entidad dependiente, no es necesario. Si no se encuentra ninguna propiedad de clave externa, una propiedad de clave externa de instantáneas se introducirán con el nombre `<navigation property name><principal key property name>` (consulte [reemplazar propiedades](shadow-properties.md) para obtener más información).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Propiedad de navegación único

Incluido sólo una propiedad de navegación (ninguna navegación inversa y ninguna propiedad de clave externa) es suficiente para tener una relación definida por convención. También puede tener una propiedad de navegación único y una propiedad de clave externa.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Eliminación en cascada

Por convención, se establecerá la eliminación en cascada en *Cascade* para relaciones necesarias y *ClientSetNull* para las relaciones opcionales. *CASCADE* significa entidades dependientes también se eliminan. *ClientSetNull* significa que las entidades dependientes que no se cargan en memoria permanecerá sin cambios y debe ser eliminado manualmente o actualiza para señalar a una entidad de seguridad válida. Para las entidades que se cargan en memoria, núcleo de EF intentará establecer las propiedades de clave externas como null.

Consulte la [relaciones obligatorios y opcionales](#required-and-optional-relationships) sección de la diferencia entre las relaciones necesarias y opcionales.

Vea [eliminación en cascada](../saving/cascade-delete.md) para obtener más detalles sobre los diferentes de eliminación de comportamientos y los valores predeterminados utilizados por convención.

## <a name="data-annotations"></a>Anotaciones de datos

Hay dos de las anotaciones de datos que pueden usarse para configurar las relaciones, `[ForeignKey]` y `[InverseProperty]`.

### <a name="foreignkey"></a>[ForeignKey]

Puede usar las anotaciones de datos para configurar la propiedad que debe usarse como la propiedad de clave externa de una relación determinada. Esto se hace normalmente cuando la propiedad de clave externa no se detecta por convención.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> El `[ForeignKey]` anotación puede colocarse en cualquier propiedad de navegación en la relación. No necesita ir en la propiedad de navegación en la clase de entidad dependiente.

### <a name="inverseproperty"></a>[InverseProperty]

Puede usar las anotaciones de datos para configurar cómo las propiedades de navegación en las entidades dependientes y principales emparejar las. Esto se hace normalmente cuando hay más de un par de propiedades de navegación entre los dos tipos de entidad.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>API fluida

Para configurar una relación en la API fluida, primero debe identificar las propiedades de navegación que componen la relación. `HasOne`o `HasMany` identifica el tipo de entidad que se va a iniciar la configuración de la propiedad de navegación. A continuación, encadenar una llamada a `WithOne` o `WithMany` para identificar el panel de navegación inversa. `HasOne`/`WithOne`se utilizan para las propiedades de navegación de referencia y `HasMany` / `WithMany` se utilizan para las propiedades de navegación de colección.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>Propiedad de navegación único

Si solo tiene una propiedad de navegación, a continuación, hay sobrecargas sin parámetros del `WithOne` y `WithMany`. Esto indica que hay conceptualmente una referencia o una colección en el otro extremo de la relación, pero no hay ninguna propiedad de navegación que se incluyen en la clase de entidad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>Clave externa

Puede usar la API fluida para configurar la propiedad que debe usarse como la propiedad de clave externa de una relación determinada.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

La lista de código siguiente muestra cómo configurar una clave externa compuesta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

Puede utilizar la sobrecarga de la cadena de `HasForeignKey(...)` para configurar una propiedad de instantáneas como una clave externa (vea [reemplazar propiedades](shadow-properties.md) para obtener más información). Se recomienda agregar explícitamente la propiedad de sombra para el modelo antes de usarlo como clave externa (como se muestra a continuación).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Clave de entidad de seguridad

Si desea que la clave externa que hacen referencia a una propiedad que no sea la clave principal, puede usar la API fluida para configurar la propiedad de clave principal para la relación. La propiedad que se configura como la clave principal no se realizará automáticamente estar configurados como una clave alternativa (vea [claves alternativas](alternate-keys.md) para obtener más información).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

La lista de código siguiente muestra cómo configurar una clave principal compuesta.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> El orden en el que especificar las propiedades de clave principales debe coincidir con el orden en que se especifican para la clave externa.

### <a name="required-and-optional-relationships"></a>Relaciones necesarias y opcionales

Puede usar la API fluida para configurar si la relación es obligatorio u opcional. En última instancia, esto controla si la propiedad de clave externa es obligatorio u opcional. Esto es muy útil cuando se usa una clave externa de estado de instantáneas. Si tiene una propiedad de clave externa en la clase de entidad, a continuación, el requiredness de la relación se determina en función de si la propiedad de clave externa es obligatorio u opcional (consulte [propiedades obligatorias y opcionales](required-optional.md) para obtener más información información).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a>Eliminación en cascada

Puede utilizar la API fluida para configurar el comportamiento de eliminación en cascada de una relación determinada de forma explícita.

Vea [eliminación en cascada](../saving/cascade-delete.md) en la sección de guardar los datos para obtener una explicación detallada de cada opción.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a>Otros modelos de relación

### <a name="one-to-one"></a>Uno a uno

Relaciones de uno a uno tienen una propiedad de navegación de referencia en ambos lados. Siguen las mismas convenciones que relaciones uno a varios, pero se incluye un índice único en la propiedad de clave externa para asegurarse de que solo una dependiente está relacionado con cada entidad de seguridad.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> EF tendrá que elegir una de las entidades que dependent basándose en su capacidad para detectar una propiedad de clave externa. Si la entidad incorrecta se elige como el dependiente, puede usar la API fluida para corregir este problema.

Al configurar la relación con la API fluida, use la `HasOne` y `WithOne` métodos.

Al configurar la clave externa debe especificar el tipo de entidad dependiente: tenga en cuenta el parámetro genérico proporcionado para `HasForeignKey` en la lista siguiente. En una relación uno a varios es evidente que la entidad con la navegación de referencia es dependiente y la con la colección es la entidad de seguridad. Pero esto no es así en una relación uno a uno - por lo tanto, la necesidad de definir explícitamente.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a>Varios a varios

Aún no se admiten las relaciones de varios a varios sin una clase de entidad para representar la tabla de combinación. Sin embargo, puede representar una relación de varios a varios mediante la inclusión de una clase de entidad para la tabla de combinación y dos relaciones uno a varios independientes de asignación.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
