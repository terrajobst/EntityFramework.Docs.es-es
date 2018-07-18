---
title: 'Código de la primera anotaciones de datos: EF6'
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
caps.latest.revision: 3
ms.openlocfilehash: 91d1d8c2608df8f7b38e70b565a4225cf10ae21f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122805"
---
# <a name="code-first-data-annotations"></a>Anotaciones de datos de Code First
> [!NOTE]
> **EF4.1 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 4.1. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

El contenido de esta página se ha adaptado del y el artículo originalmente escrito por Julie Lerman (\<http://thedatafarm.com>).

Entity Framework Code First le permite usar sus propias clases de dominio para representar el modelo de EF para realizar las consultas, que se basa en cambiar seguimiento y actualización de las funciones. Código primero aprovecha un modelo de programación que se conoce como Convención sobre configuración. Esto significa que el código primero supondrá que sus clases siguen las convenciones de EF usa. En ese caso, EF podrá elaborar los detalles que necesita para realizar su trabajo. Sin embargo, si las clases no siguen estas convenciones, tiene la capacidad de agregar las configuraciones a las clases para proporcionar EF con la información que necesita.

Código primero le ofrece dos maneras de agregar estas configuraciones a las clases. Una es usar atributos simple denominados DataAnnotations y la otra es mediante código en primer lugar es la API Fluent, que le brinda una forma de describir las configuraciones de forma imperativa, en el código.

En este artículo se centrará en mediante DataAnnotations (en el espacio de nombres System.ComponentModel.DataAnnotations) para configurar las clases: resaltado de las configuraciones necesarias con más frecuencia. DataAnnotations también se entiende por un número de aplicaciones. NET, como ASP.NET MVC, lo que permite a estas aplicaciones aprovechar las mismas anotaciones para las validaciones del lado cliente.


## <a name="the-model"></a>El modelo

Voy a demostrar DataAnnotations primero con un simple par de clases de código: Blog y Post.

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public DateTime DateCreated { get; set; }
        public string Content { get; set; }
        public int BlogId { get; set; }
        public ICollection<Comment> Comments { get; set; }
    }
```

Tal como están, las clases de Blog y Post cómodamente siguen la convención de primer código y no necesario ajustes para ayudar a EF a trabajar con ellos. Pero también puede usar para proporcionar más información a EF acerca de las clases y la base de datos que se asignan a las anotaciones.

 

## <a name="key"></a>Key

Entity Framework se basa en todas las entidades que contienen un valor de clave que se usa para las entidades de seguimiento. Una de las convenciones de código en primer lugar depende es cómo implica qué propiedad es la clave en cada una de las clases de código primero. Esta convención consiste en Buscar una propiedad denominada "Id" o que combina el nombre de clase y el "Id", por ejemplo, "BlogId". La propiedad se asignará a una columna de clave principal en la base de datos.

Las clases Blog y Post siguen esta convención. Pero ¿qué ocurre si no? ¿Qué ocurre si Blog usa el nombre *PrimaryTrackingKey* en su lugar o incluso *foo*? Si el código en primer lugar no encuentra una propiedad que coincide con esta convención producirá una excepción debido a requisitos de Entity Framework que debe tener una propiedad de clave. Puede usar la anotación de clave para especificar qué propiedad es que se usará como valor EntityKey.

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

Si está utilizando código en primer lugar es la característica de generación de base de datos, la tabla Blog tendrá una columna de clave principal denominada PrimaryTrackingKey que también se define como identidad de forma predeterminada.

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Claves compuestas

Entity Framework admite las claves compuestas - claves principales que constan de más de una propiedad. Por ejemplo, su podría tener una clase de Passport cuya clave principal es una combinación de PassportNumber y IssuingCountry.

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

Si desea probar y usar la clase anterior en el modelo EF, que obtendría un informando de excepciones InvalidOperationException;

*No se puede determinar compuesto clave orden primario para el tipo "Passport". Utilice el ColumnAttribute o el método HasKey para especificar un orden para las claves principales compuestas.*

Cuando tenga las claves compuestas, Entity Framework requiere que definen el orden de las propiedades de clave. Puede hacerlo mediante la anotación de columna para especificar un orden.

>[!NOTE]
> El valor de orden es relativo (en lugar de index) por lo que se puede usar cualquier valor. Por ejemplo, 100 y 200 sería aceptable en lugar de 1 y 2.

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

Si tiene entidades con claves externas compuestas, a continuación, debe especificar la misma columna de ordenación que usa para las propiedades de clave principales correspondientes.

Solo el orden relativo dentro de las propiedades de clave externas debe ser el mismo, los valores exactos que se asigna a **orden** no es necesario hacer coincidir. Por ejemplo, en la siguiente clase, 3 y 4 podría usarse en lugar de 1 y 2.

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a>Obligatorio

La anotación requiere indica a EF que se requiere una propiedad determinada.

Agregar necesarios para la propiedad Title obligará a EF (y MVC) para asegurarse de que la propiedad tiene datos en ella.

``` csharp
    [Required]
    public string Title { get; set; }
```

Con ningún adicional n cambios de código o marcado en la aplicación, una aplicación MVC realizará la validación del lado cliente, crear incluso dinámicamente un mensaje con los nombres de propiedad y anotación.

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

El atributo Required también afectará a la base de datos generado mediante la realización de la propiedad asignada que no aceptan valores NULL. Tenga en cuenta que el campo de título ha cambiado a "not null".

>[!NOTE]
> En algunos casos puede no ser posible que la columna de la base de datos sea distinto de null, aunque la propiedad es obligatoria. Por ejemplo, cuando usa una estrategia de herencia de TPH datos para varios tipos se almacena en una sola tabla. Si un tipo derivado incluye una propiedad necesaria de que la columna no puede hacerse que no aceptan valores NULL ya que no todos los tipos en la jerarquía tienen esta propiedad.

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength y MinLength

Los atributos MaxLength y MinLength le permiten especificar las validaciones de propiedad adicional, tal como hizo con los necesarios.

Este es el BloggerName con los requisitos de longitud. El ejemplo también muestra cómo combinar atributos.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

La anotación MaxLength afectará a la base de datos estableciendo la longitud de la propiedad a 10.

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

Anotación de MVC del lado cliente y anotación de EF 4.1 del lado servidor respetan esta validación, crear nuevo dinámicamente un mensaje de error: "el campo BloggerName debe ser un tipo de cadena o matriz con una longitud máxima de '10'." Ese mensaje es un poco largo. Muchas de las anotaciones le permiten especificar un mensaje de error con el atributo de mensaje de error.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

También puede especificar el mensaje de error en la anotación requiere.

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

Convención de primer código dicta que todas las propiedades que son de un tipo de datos admitidos se representan en la base de datos. Pero esto no siempre es el caso en las aplicaciones. Por ejemplo podría tener una propiedad en la clase de Blog que crea un código basado en los campos título y BloggerName. Esa propiedad se puede crear dinámicamente y no deben almacenarse. Puede marcar las propiedades que no se asignan a la base de datos con la anotación NotMapped como esta propiedad BlogCode.

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a>ComplexType

No es raro para describir las entidades de dominio a través de un conjunto de clases y, luego, colocar esas clases para describir una entidad completa. Por ejemplo, puede agregar una clase denominada BlogDetails al modelo.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Tenga en cuenta que BlogDetails no tiene ningún tipo de propiedad de clave. En el diseño controlado por dominio BlogDetails se conoce como un objeto de valor. Entity Framework hace referencia a objetos de valor como tipos complejos.  Tipos complejos no pueden controlarse por sí solos.

Sin embargo como una propiedad en la clase de Blog, BlogDetails se realizará un seguimiento como parte de un objeto de Blog. En orden para code first para reconocer esto, debe marcar la clase BlogDetails como de tipo complejo.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Ahora puede agregar una propiedad en la clase de Blog para representar el BlogDetails para dicho blog.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

En la base de datos, la tabla Blog contendrá todas las propiedades del blog, incluidas las propiedades contenidas en la propiedad BlogDetail. De forma predeterminada, cada uno de ellos está precedido por el nombre del tipo complejo, BlogDetail.

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

Otro aspecto interesante es que aunque la propiedad DateCreated se definió como una fecha y hora que no aceptan valores NULL en la clase, el campo de base de datos pertinente que acepta valores NULL. Debe usar la anotación necesaria si lo desea influir en el esquema de base de datos.

 

## <a name="concurrencycheck"></a>ConcurrencyCheck

La anotación ConcurrencyCheck le permite marcar una o varias propiedades que se usará para la comprobación de simultaneidad en la base de datos cuando un usuario edita o elimina una entidad. Si ha estado trabajando con EF Designer, esto se alinea con la configuración ConcurrencyMode de la propiedad en Fixed.

Veamos cómo funciona ConcurrencyCheck agregándolo a la propiedad BloggerName.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Cuando se llama a SaveChanges, debido a la anotación ConcurrencyCheck en el campo BloggerName, el valor original de dicha propiedad se usará en la actualización. El comando intentará encontrar la fila correcta mediante el filtrado no solo en el valor de clave, sino también en el valor original de BloggerName.  Estas son las partes críticas del comando UPDATE enviados a la base de datos, donde puede ver el comando actualizará la fila que tiene un PrimaryTrackingKey es 1 y un BloggerName de "Julie", que es el valor original cuando se recuperó el blog de la base de datos.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Si alguien ha cambiado el nombre de blogger para blog mientras tanto, se producirá un error en esta actualización y obtendrá un DbUpdateConcurrencyException que necesitará para controlar.

 

## <a name="timestamp"></a>Marca de tiempo

Es más habitual utilizar campos rowversion o marca de tiempo para la comprobación de simultaneidad. Pero en lugar de usar la anotación ConcurrencyCheck, puede usar la anotación de marca de tiempo más específica, siempre y cuando el tipo de la propiedad es la matriz de bytes. Código primero tratará las propiedades de la marca de tiempo del mismo como propiedades ConcurrencyCheck, pero también se asegurará que el campo de base de datos que genera el código en primer lugar es distinto de NULL. Solo puede tener una propiedad de marca de tiempo en una clase determinada.

Agregando la siguiente propiedad a la clase de Blog:

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

resultados en el código que empiece a crear una columna de marca de tiempo que no aceptan valores NULL en la tabla de base de datos.

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Tablas y columnas

Si deja que Code First crea la base de datos, desea cambiar el nombre de las tablas y columnas que se está creando. También puede usar Code First con una base de datos existente. Pero no siempre es el caso de que los nombres de las clases y propiedades en el dominio coinciden con los nombres de las tablas y columnas en la base de datos.

Mi clase se denomina Blog y por convención, código en primer lugar se da por supuesto que esto asignará a una tabla denominada Blogs. Si no es así puede especificar el nombre de la tabla con el atributo de tabla. Aquí, por ejemplo, la anotación se especifica que el nombre de tabla es InternalBlogs.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

La anotación de la columna es una Adept ha más en la especificación de los atributos de una columna asignada. Puede estipular un nombre, tipo de datos o incluso el orden en que una columna aparece en la tabla. Este es un ejemplo del atributo de columna.

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

No confunda el TypeName (atributo) de la columna con el DataType DataAnnotation. Tipo de datos es una anotación que se usa para la interfaz de usuario y se omite por Code First.

Esta es la tabla después de que se ha vuelto a generar. El nombre de la tabla ha cambiado a InternalBlogs y columna de descripción de tipo complejo es ahora BlogDescription. Porque se especificó el nombre de la anotación, code first no usará la convención de iniciar el nombre de columna con el nombre del tipo complejo.

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>DatabaseGenerated

Características de una base de datos importantes es la capacidad para que se han calculado las propiedades. Si está asignando las clases de Code First en las tablas que contienen columnas calculan, no desea que Entity Framework al intentar actualizar las columnas. Pero desea EF al devolver esos valores de la base de datos después de insertar o actualizar datos. Puede usar la anotación DatabaseGenerated para marcar aquellas propiedades en la clase junto con la enumeración calculado. Otras enumeraciones son ninguno y la identidad.

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Puede usar la base de datos generada en las columnas byte o marca de tiempo cuando el código está generando en primer lugar la base de datos, en caso contrario, solo debe usar esto al señalar a bases de datos existentes de porque el código en primer lugar no podrá determinar la fórmula para la columna calculada.

Mencionamos anteriormente de forma predeterminada, una propiedad de clave que es un entero se convertirá en una clave de identidad en la base de datos. Eso sería lo mismo que establecer DatabaseGenerated a DatabaseGenerationOption.Identity. Si no desea que sea una clave de identidad, puede establecer el valor en DatabaseGenerationOption.None.

 

## <a name="index"></a>Índice

> [!NOTE]
> **EF6.1 y versiones posteriores solo** -atributo el índice se introdujo en Entity Framework 6.1. Si usa una versión anterior no se aplica la información de esta sección.

Puede crear un índice en una o varias columnas mediante el **IndexAttribute**. Agregar el atributo a una o varias propiedades impedirán EF crear el índice correspondiente en la base de datos cuando crea la base de datos, o aplicar la técnica scaffolding correspondiente **CreateIndex** llama si usa migraciones de Code First.

Por ejemplo, se producirá el siguiente código en un índice que se va a crear el **clasificación** columna de la **publicaciones** tabla en la base de datos.

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

De forma predeterminada, el índice se denominará **IX\_&lt;nombre de la propiedad&gt;**  (IX\_clasificación en el ejemplo anterior). Aunque también puede especificar un nombre para el índice. El ejemplo siguiente especifica que el índice debe denominarse **PostRatingIndex**.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

De forma predeterminada, los índices no son únicas, pero puede usar el **IsUnique** con el nombre de parámetro para especificar que un índice debe ser único. El ejemplo siguiente presenta un índice único en un **usuario**del nombre de inicio de sesión.

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a>Índices de varias columnas

Los índices que abarcan varias columnas se especifican con el mismo nombre en varias anotaciones de índice para una tabla determinada. Al crear los índices de varias columnas, deberá especificar un orden de las columnas en el índice. Por ejemplo, el código siguiente crea un índice de varias columna en **clasificación** y **BlogId** llamado **IX\_BlogAndRating**. **BlogId** es la primera columna en el índice y **clasificación** es el segundo.

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>Relación de atributos: InverseProperty y ForeignKey

> [!NOTE]
> Esta página proporciona información sobre cómo configurar las relaciones en el modelo de Code First usando anotaciones de datos. Para obtener información general acerca de las relaciones de EF y cómo obtener acceso y manipular datos mediante relaciones, vea [& Propiedades de navegación de relaciones](~/ef6/fundamentals/relationships.md). *

Convención de primer código que se encargará de las relaciones en el modelo más común, pero hay algunos casos donde necesita ayuda.

Cambiar el nombre de la propiedad de clave en la clase Blog supuso un problema con su relación con Post. 

Al generar la base de datos, código primero ve la propiedad BlogId en la clase de publicación y lo reconoce, la convención que coincide con un nombre de la clase más "Id", como una clave externa a la clase de Blog. Pero no hay ninguna propiedad BlogId en la clase de blog. La solución para esto consiste en crear una propiedad de navegación en la publicación y use el DataAnnotation externa para código entender primero cómo crear la relación entre las dos clases: mediante la propiedad Post.BlogId —, así como también especificar restricciones en el base de datos.

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

La restricción en la base de datos muestra una relación entre InternalBlogs.PrimaryTrackingKey y Posts.BlogId. 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

Se usa el InverseProperty cuando tiene varias relaciones entre las clases.

En la clase de publicación, puede realizar un seguimiento de quién escribió una entrada de blog, así como quién editó. Presentamos dos nuevas propiedades de navegación para la clase de publicación.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

También deberá agregar en la clase Person que se hace referencia a estas propiedades. La clase de persona tiene propiedades de navegación a la publicación, uno para todas las entradas escritas por la persona y otra para todas las publicaciones actualizadas por esa persona.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

En primer lugar no es capaz de hacer coincidir las propiedades en las dos clases en su propio código. La tabla de base de datos de publicaciones debe tener una clave externa de la persona CreatedBy y uno para la persona UpdatedBy pero el código en primer lugar creará cuatro propiedades clave externa de voluntad: persona\_Id, persona\_Id1, CreatedBy\_Id y UpdatedBy\_identificador.

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

Para corregir estos problemas, puede usar la anotación InverseProperty para especificar la alineación de las propiedades.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Dado que la propiedad PostsWritten en persona sabe que esto hace referencia al tipo de publicación, compilará la relación para Post.CreatedBy. Del mismo modo, se conectará PostsUpdated a Post.UpdatedBy. Y código primero no creará las claves externas adicionales.

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Resumen

DataAnnotations no sólo permiten describir la validación del lado de cliente y servidor en las clases de código primero, pero también permiten mejorar y corregir incluso las suposiciones que código hará que primero sobre las clases según las convenciones. Con DataAnnotations puede no solo unidad generación de esquema de base de datos, pero también puede asignar las clases de código primero a una base de datos ya existente.

Mientras están muy flexibles, tenga en cuenta que DataAnnotations se proporcionan solo más normalmente, son necesarios cambios de configuración que puede realizar en las clases de código primero. Para configurar las clases para algunos de los casos extremos, deberá tener en cuenta el mecanismo de configuración alternativa, la API Fluent de Code First.
