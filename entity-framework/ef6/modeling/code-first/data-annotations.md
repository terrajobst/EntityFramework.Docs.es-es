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
# <a name="code-first-data-annotations"></a><span data-ttu-id="1d585-102">Anotaciones de datos de Code First</span><span class="sxs-lookup"><span data-stu-id="1d585-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="1d585-103">**EF4.1 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="1d585-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="1d585-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="1d585-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="1d585-105">El contenido de esta página se ha adaptado del y el artículo originalmente escrito por Julie Lerman (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="1d585-105">The content on this page is adapted from and article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="1d585-106">Entity Framework Code First le permite usar sus propias clases de dominio para representar el modelo de EF para realizar las consultas, que se basa en cambiar seguimiento y actualización de las funciones.</span><span class="sxs-lookup"><span data-stu-id="1d585-106">Entity Framework Code First allows you to use your own domain classes to represent the model which EF relies on to perform querying, change tracking and updating functions.</span></span> <span data-ttu-id="1d585-107">Código primero aprovecha un modelo de programación que se conoce como Convención sobre configuración.</span><span class="sxs-lookup"><span data-stu-id="1d585-107">Code first leverages a programming pattern referred to as convention over configuration.</span></span> <span data-ttu-id="1d585-108">Esto significa que el código primero supondrá que sus clases siguen las convenciones de EF usa.</span><span class="sxs-lookup"><span data-stu-id="1d585-108">What this means is that code first will assume that your classes follow the conventions that EF uses.</span></span> <span data-ttu-id="1d585-109">En ese caso, EF podrá elaborar los detalles que necesita para realizar su trabajo.</span><span class="sxs-lookup"><span data-stu-id="1d585-109">In that case, EF will be able to work out the details it needs to do its job.</span></span> <span data-ttu-id="1d585-110">Sin embargo, si las clases no siguen estas convenciones, tiene la capacidad de agregar las configuraciones a las clases para proporcionar EF con la información que necesita.</span><span class="sxs-lookup"><span data-stu-id="1d585-110">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the information it needs.</span></span>

<span data-ttu-id="1d585-111">Código primero le ofrece dos maneras de agregar estas configuraciones a las clases.</span><span class="sxs-lookup"><span data-stu-id="1d585-111">Code first gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="1d585-112">Una es usar atributos simple denominados DataAnnotations y la otra es mediante código en primer lugar es la API Fluent, que le brinda una forma de describir las configuraciones de forma imperativa, en el código.</span><span class="sxs-lookup"><span data-stu-id="1d585-112">One is using simple attributes called DataAnnotations and the other is using code first’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="1d585-113">En este artículo se centrará en mediante DataAnnotations (en el espacio de nombres System.ComponentModel.DataAnnotations) para configurar las clases: resaltado de las configuraciones necesarias con más frecuencia.</span><span class="sxs-lookup"><span data-stu-id="1d585-113">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="1d585-114">DataAnnotations también se entiende por un número de aplicaciones. NET, como ASP.NET MVC, lo que permite a estas aplicaciones aprovechar las mismas anotaciones para las validaciones del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="1d585-114">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="1d585-115">El modelo</span><span class="sxs-lookup"><span data-stu-id="1d585-115">The model</span></span>

<span data-ttu-id="1d585-116">Voy a demostrar DataAnnotations primero con un simple par de clases de código: Blog y Post.</span><span class="sxs-lookup"><span data-stu-id="1d585-116">I’ll demonstrate code first DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

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

<span data-ttu-id="1d585-117">Tal como están, las clases de Blog y Post cómodamente siguen la convención de primer código y no necesario ajustes para ayudar a EF a trabajar con ellos.</span><span class="sxs-lookup"><span data-stu-id="1d585-117">As they are, the Blog and Post classes conveniently follow code first convention and required no tweaks to help EF work with them.</span></span> <span data-ttu-id="1d585-118">Pero también puede usar para proporcionar más información a EF acerca de las clases y la base de datos que se asignan a las anotaciones.</span><span class="sxs-lookup"><span data-stu-id="1d585-118">But you can also use the annotations to provide more information to EF about the classes and the database that they map to.</span></span>

 

## <a name="key"></a><span data-ttu-id="1d585-119">Key</span><span class="sxs-lookup"><span data-stu-id="1d585-119">Key</span></span>

<span data-ttu-id="1d585-120">Entity Framework se basa en todas las entidades que contienen un valor de clave que se usa para las entidades de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="1d585-120">Entity Framework relies on every entity having a key value that it uses for tracking entities.</span></span> <span data-ttu-id="1d585-121">Una de las convenciones de código en primer lugar depende es cómo implica qué propiedad es la clave en cada una de las clases de código primero.</span><span class="sxs-lookup"><span data-stu-id="1d585-121">One of the conventions that code first depends on is how it implies which property is the key in each of the code first classes.</span></span> <span data-ttu-id="1d585-122">Esta convención consiste en Buscar una propiedad denominada "Id" o que combina el nombre de clase y el "Id", por ejemplo, "BlogId".</span><span class="sxs-lookup"><span data-stu-id="1d585-122">That convention is to look for a property named “Id” or one that combines the class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="1d585-123">La propiedad se asignará a una columna de clave principal en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1d585-123">The property will map to a primary key column in the database.</span></span>

<span data-ttu-id="1d585-124">Las clases Blog y Post siguen esta convención.</span><span class="sxs-lookup"><span data-stu-id="1d585-124">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="1d585-125">Pero ¿qué ocurre si no?</span><span class="sxs-lookup"><span data-stu-id="1d585-125">But what if they didn’t?</span></span> <span data-ttu-id="1d585-126">¿Qué ocurre si Blog usa el nombre *PrimaryTrackingKey* en su lugar o incluso *foo*?</span><span class="sxs-lookup"><span data-stu-id="1d585-126">What if Blog used the name *PrimaryTrackingKey* instead or even *foo*?</span></span> <span data-ttu-id="1d585-127">Si el código en primer lugar no encuentra una propiedad que coincide con esta convención producirá una excepción debido a requisitos de Entity Framework que debe tener una propiedad de clave.</span><span class="sxs-lookup"><span data-stu-id="1d585-127">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="1d585-128">Puede usar la anotación de clave para especificar qué propiedad es que se usará como valor EntityKey.</span><span class="sxs-lookup"><span data-stu-id="1d585-128">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

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

<span data-ttu-id="1d585-129">Si está utilizando código en primer lugar es la característica de generación de base de datos, la tabla Blog tendrá una columna de clave principal denominada PrimaryTrackingKey que también se define como identidad de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="1d585-129">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey which is also defined as Identity by default.</span></span>

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="1d585-131">Claves compuestas</span><span class="sxs-lookup"><span data-stu-id="1d585-131">Composite keys</span></span>

<span data-ttu-id="1d585-132">Entity Framework admite las claves compuestas - claves principales que constan de más de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="1d585-132">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="1d585-133">Por ejemplo, su podría tener una clase de Passport cuya clave principal es una combinación de PassportNumber y IssuingCountry.</span><span class="sxs-lookup"><span data-stu-id="1d585-133">For example, your could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

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

<span data-ttu-id="1d585-134">Si desea probar y usar la clase anterior en el modelo EF, que obtendría un informando de excepciones InvalidOperationException;</span><span class="sxs-lookup"><span data-stu-id="1d585-134">If you were to try and use the above class in your EF model you wuld get an InvalidOperationExceptions stating;</span></span>

<span data-ttu-id="1d585-135">*No se puede determinar compuesto clave orden primario para el tipo "Passport". Utilice el ColumnAttribute o el método HasKey para especificar un orden para las claves principales compuestas.*</span><span class="sxs-lookup"><span data-stu-id="1d585-135">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="1d585-136">Cuando tenga las claves compuestas, Entity Framework requiere que definen el orden de las propiedades de clave.</span><span class="sxs-lookup"><span data-stu-id="1d585-136">When you have composite keys, Entity Framework requires you to define an order of the key properties.</span></span> <span data-ttu-id="1d585-137">Puede hacerlo mediante la anotación de columna para especificar un orden.</span><span class="sxs-lookup"><span data-stu-id="1d585-137">You can do this using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="1d585-138">El valor de orden es relativo (en lugar de index) por lo que se puede usar cualquier valor.</span><span class="sxs-lookup"><span data-stu-id="1d585-138">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="1d585-139">Por ejemplo, 100 y 200 sería aceptable en lugar de 1 y 2.</span><span class="sxs-lookup"><span data-stu-id="1d585-139">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

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

<span data-ttu-id="1d585-140">Si tiene entidades con claves externas compuestas, a continuación, debe especificar la misma columna de ordenación que usa para las propiedades de clave principales correspondientes.</span><span class="sxs-lookup"><span data-stu-id="1d585-140">If you have entities with composite foreign keys then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="1d585-141">Solo el orden relativo dentro de las propiedades de clave externas debe ser el mismo, los valores exactos que se asigna a **orden** no es necesario hacer coincidir.</span><span class="sxs-lookup"><span data-stu-id="1d585-141">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="1d585-142">Por ejemplo, en la siguiente clase, 3 y 4 podría usarse en lugar de 1 y 2.</span><span class="sxs-lookup"><span data-stu-id="1d585-142">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

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

## <a name="required"></a><span data-ttu-id="1d585-143">Obligatorio</span><span class="sxs-lookup"><span data-stu-id="1d585-143">Required</span></span>

<span data-ttu-id="1d585-144">La anotación requiere indica a EF que se requiere una propiedad determinada.</span><span class="sxs-lookup"><span data-stu-id="1d585-144">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="1d585-145">Agregar necesarios para la propiedad Title obligará a EF (y MVC) para asegurarse de que la propiedad tiene datos en ella.</span><span class="sxs-lookup"><span data-stu-id="1d585-145">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="1d585-146">Con ningún adicional n cambios de código o marcado en la aplicación, una aplicación MVC realizará la validación del lado cliente, crear incluso dinámicamente un mensaje con los nombres de propiedad y anotación.</span><span class="sxs-lookup"><span data-stu-id="1d585-146">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="1d585-148">El atributo Required también afectará a la base de datos generado mediante la realización de la propiedad asignada que no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="1d585-148">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="1d585-149">Tenga en cuenta que el campo de título ha cambiado a "not null".</span><span class="sxs-lookup"><span data-stu-id="1d585-149">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="1d585-150">En algunos casos puede no ser posible que la columna de la base de datos sea distinto de null, aunque la propiedad es obligatoria.</span><span class="sxs-lookup"><span data-stu-id="1d585-150">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="1d585-151">Por ejemplo, cuando usa una estrategia de herencia de TPH datos para varios tipos se almacena en una sola tabla.</span><span class="sxs-lookup"><span data-stu-id="1d585-151">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="1d585-152">Si un tipo derivado incluye una propiedad necesaria de que la columna no puede hacerse que no aceptan valores NULL ya que no todos los tipos en la jerarquía tienen esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="1d585-152">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="1d585-154">MaxLength y MinLength</span><span class="sxs-lookup"><span data-stu-id="1d585-154">MaxLength and MinLength</span></span>

<span data-ttu-id="1d585-155">Los atributos MaxLength y MinLength le permiten especificar las validaciones de propiedad adicional, tal como hizo con los necesarios.</span><span class="sxs-lookup"><span data-stu-id="1d585-155">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="1d585-156">Este es el BloggerName con los requisitos de longitud.</span><span class="sxs-lookup"><span data-stu-id="1d585-156">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="1d585-157">El ejemplo también muestra cómo combinar atributos.</span><span class="sxs-lookup"><span data-stu-id="1d585-157">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="1d585-158">La anotación MaxLength afectará a la base de datos estableciendo la longitud de la propiedad a 10.</span><span class="sxs-lookup"><span data-stu-id="1d585-158">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="1d585-160">Anotación de MVC del lado cliente y anotación de EF 4.1 del lado servidor respetan esta validación, crear nuevo dinámicamente un mensaje de error: "el campo BloggerName debe ser un tipo de cadena o matriz con una longitud máxima de '10'." Ese mensaje es un poco largo.</span><span class="sxs-lookup"><span data-stu-id="1d585-160">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="1d585-161">Muchas de las anotaciones le permiten especificar un mensaje de error con el atributo de mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="1d585-161">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="1d585-162">También puede especificar el mensaje de error en la anotación requiere.</span><span class="sxs-lookup"><span data-stu-id="1d585-162">You can also specify ErrorMessage in the Required annotation.</span></span>

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="1d585-164">NotMapped</span><span class="sxs-lookup"><span data-stu-id="1d585-164">NotMapped</span></span>

<span data-ttu-id="1d585-165">Convención de primer código dicta que todas las propiedades que son de un tipo de datos admitidos se representan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1d585-165">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="1d585-166">Pero esto no siempre es el caso en las aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="1d585-166">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="1d585-167">Por ejemplo podría tener una propiedad en la clase de Blog que crea un código basado en los campos título y BloggerName.</span><span class="sxs-lookup"><span data-stu-id="1d585-167">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="1d585-168">Esa propiedad se puede crear dinámicamente y no deben almacenarse.</span><span class="sxs-lookup"><span data-stu-id="1d585-168">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="1d585-169">Puede marcar las propiedades que no se asignan a la base de datos con la anotación NotMapped como esta propiedad BlogCode.</span><span class="sxs-lookup"><span data-stu-id="1d585-169">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

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

 

## <a name="complextype"></a><span data-ttu-id="1d585-170">ComplexType</span><span class="sxs-lookup"><span data-stu-id="1d585-170">ComplexType</span></span>

<span data-ttu-id="1d585-171">No es raro para describir las entidades de dominio a través de un conjunto de clases y, luego, colocar esas clases para describir una entidad completa.</span><span class="sxs-lookup"><span data-stu-id="1d585-171">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="1d585-172">Por ejemplo, puede agregar una clase denominada BlogDetails al modelo.</span><span class="sxs-lookup"><span data-stu-id="1d585-172">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="1d585-173">Tenga en cuenta que BlogDetails no tiene ningún tipo de propiedad de clave.</span><span class="sxs-lookup"><span data-stu-id="1d585-173">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="1d585-174">En el diseño controlado por dominio BlogDetails se conoce como un objeto de valor.</span><span class="sxs-lookup"><span data-stu-id="1d585-174">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="1d585-175">Entity Framework hace referencia a objetos de valor como tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="1d585-175">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="1d585-176">Tipos complejos no pueden controlarse por sí solos.</span><span class="sxs-lookup"><span data-stu-id="1d585-176">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="1d585-177">Sin embargo como una propiedad en la clase de Blog, BlogDetails se realizará un seguimiento como parte de un objeto de Blog.</span><span class="sxs-lookup"><span data-stu-id="1d585-177">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="1d585-178">En orden para code first para reconocer esto, debe marcar la clase BlogDetails como de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="1d585-178">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="1d585-179">Ahora puede agregar una propiedad en la clase de Blog para representar el BlogDetails para dicho blog.</span><span class="sxs-lookup"><span data-stu-id="1d585-179">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="1d585-180">En la base de datos, la tabla Blog contendrá todas las propiedades del blog, incluidas las propiedades contenidas en la propiedad BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="1d585-180">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="1d585-181">De forma predeterminada, cada uno de ellos está precedido por el nombre del tipo complejo, BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="1d585-181">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="1d585-183">Otro aspecto interesante es que aunque la propiedad DateCreated se definió como una fecha y hora que no aceptan valores NULL en la clase, el campo de base de datos pertinente que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="1d585-183">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="1d585-184">Debe usar la anotación necesaria si lo desea influir en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="1d585-184">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="1d585-185">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="1d585-185">ConcurrencyCheck</span></span>

<span data-ttu-id="1d585-186">La anotación ConcurrencyCheck le permite marcar una o varias propiedades que se usará para la comprobación de simultaneidad en la base de datos cuando un usuario edita o elimina una entidad.</span><span class="sxs-lookup"><span data-stu-id="1d585-186">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="1d585-187">Si ha estado trabajando con EF Designer, esto se alinea con la configuración ConcurrencyMode de la propiedad en Fixed.</span><span class="sxs-lookup"><span data-stu-id="1d585-187">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="1d585-188">Veamos cómo funciona ConcurrencyCheck agregándolo a la propiedad BloggerName.</span><span class="sxs-lookup"><span data-stu-id="1d585-188">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="1d585-189">Cuando se llama a SaveChanges, debido a la anotación ConcurrencyCheck en el campo BloggerName, el valor original de dicha propiedad se usará en la actualización.</span><span class="sxs-lookup"><span data-stu-id="1d585-189">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="1d585-190">El comando intentará encontrar la fila correcta mediante el filtrado no solo en el valor de clave, sino también en el valor original de BloggerName.</span><span class="sxs-lookup"><span data-stu-id="1d585-190">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="1d585-191">Estas son las partes críticas del comando UPDATE enviados a la base de datos, donde puede ver el comando actualizará la fila que tiene un PrimaryTrackingKey es 1 y un BloggerName de "Julie", que es el valor original cuando se recuperó el blog de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1d585-191">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="1d585-192">Si alguien ha cambiado el nombre de blogger para blog mientras tanto, se producirá un error en esta actualización y obtendrá un DbUpdateConcurrencyException que necesitará para controlar.</span><span class="sxs-lookup"><span data-stu-id="1d585-192">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="1d585-193">Marca de tiempo</span><span class="sxs-lookup"><span data-stu-id="1d585-193">TimeStamp</span></span>

<span data-ttu-id="1d585-194">Es más habitual utilizar campos rowversion o marca de tiempo para la comprobación de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="1d585-194">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="1d585-195">Pero en lugar de usar la anotación ConcurrencyCheck, puede usar la anotación de marca de tiempo más específica, siempre y cuando el tipo de la propiedad es la matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="1d585-195">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="1d585-196">Código primero tratará las propiedades de la marca de tiempo del mismo como propiedades ConcurrencyCheck, pero también se asegurará que el campo de base de datos que genera el código en primer lugar es distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="1d585-196">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="1d585-197">Solo puede tener una propiedad de marca de tiempo en una clase determinada.</span><span class="sxs-lookup"><span data-stu-id="1d585-197">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="1d585-198">Agregando la siguiente propiedad a la clase de Blog:</span><span class="sxs-lookup"><span data-stu-id="1d585-198">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="1d585-199">resultados en el código que empiece a crear una columna de marca de tiempo que no aceptan valores NULL en la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="1d585-199">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="1d585-201">Tablas y columnas</span><span class="sxs-lookup"><span data-stu-id="1d585-201">Table and Column</span></span>

<span data-ttu-id="1d585-202">Si deja que Code First crea la base de datos, desea cambiar el nombre de las tablas y columnas que se está creando.</span><span class="sxs-lookup"><span data-stu-id="1d585-202">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="1d585-203">También puede usar Code First con una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="1d585-203">You can also use Code First with an existing database.</span></span> <span data-ttu-id="1d585-204">Pero no siempre es el caso de que los nombres de las clases y propiedades en el dominio coinciden con los nombres de las tablas y columnas en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1d585-204">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="1d585-205">Mi clase se denomina Blog y por convención, código en primer lugar se da por supuesto que esto asignará a una tabla denominada Blogs.</span><span class="sxs-lookup"><span data-stu-id="1d585-205">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="1d585-206">Si no es así puede especificar el nombre de la tabla con el atributo de tabla.</span><span class="sxs-lookup"><span data-stu-id="1d585-206">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="1d585-207">Aquí, por ejemplo, la anotación se especifica que el nombre de tabla es InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="1d585-207">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="1d585-208">La anotación de la columna es una Adept ha más en la especificación de los atributos de una columna asignada.</span><span class="sxs-lookup"><span data-stu-id="1d585-208">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="1d585-209">Puede estipular un nombre, tipo de datos o incluso el orden en que una columna aparece en la tabla.</span><span class="sxs-lookup"><span data-stu-id="1d585-209">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="1d585-210">Este es un ejemplo del atributo de columna.</span><span class="sxs-lookup"><span data-stu-id="1d585-210">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="1d585-211">No confunda el TypeName (atributo) de la columna con el DataType DataAnnotation.</span><span class="sxs-lookup"><span data-stu-id="1d585-211">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="1d585-212">Tipo de datos es una anotación que se usa para la interfaz de usuario y se omite por Code First.</span><span class="sxs-lookup"><span data-stu-id="1d585-212">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="1d585-213">Esta es la tabla después de que se ha vuelto a generar.</span><span class="sxs-lookup"><span data-stu-id="1d585-213">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="1d585-214">El nombre de la tabla ha cambiado a InternalBlogs y columna de descripción de tipo complejo es ahora BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="1d585-214">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="1d585-215">Porque se especificó el nombre de la anotación, code first no usará la convención de iniciar el nombre de columna con el nombre del tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="1d585-215">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="1d585-217">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="1d585-217">DatabaseGenerated</span></span>

<span data-ttu-id="1d585-218">Características de una base de datos importantes es la capacidad para que se han calculado las propiedades.</span><span class="sxs-lookup"><span data-stu-id="1d585-218">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="1d585-219">Si está asignando las clases de Code First en las tablas que contienen columnas calculan, no desea que Entity Framework al intentar actualizar las columnas.</span><span class="sxs-lookup"><span data-stu-id="1d585-219">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="1d585-220">Pero desea EF al devolver esos valores de la base de datos después de insertar o actualizar datos.</span><span class="sxs-lookup"><span data-stu-id="1d585-220">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="1d585-221">Puede usar la anotación DatabaseGenerated para marcar aquellas propiedades en la clase junto con la enumeración calculado.</span><span class="sxs-lookup"><span data-stu-id="1d585-221">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="1d585-222">Otras enumeraciones son ninguno y la identidad.</span><span class="sxs-lookup"><span data-stu-id="1d585-222">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="1d585-223">Puede usar la base de datos generada en las columnas byte o marca de tiempo cuando el código está generando en primer lugar la base de datos, en caso contrario, solo debe usar esto al señalar a bases de datos existentes de porque el código en primer lugar no podrá determinar la fórmula para la columna calculada.</span><span class="sxs-lookup"><span data-stu-id="1d585-223">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="1d585-224">Mencionamos anteriormente de forma predeterminada, una propiedad de clave que es un entero se convertirá en una clave de identidad en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1d585-224">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="1d585-225">Eso sería lo mismo que establecer DatabaseGenerated a DatabaseGenerationOption.Identity.</span><span class="sxs-lookup"><span data-stu-id="1d585-225">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="1d585-226">Si no desea que sea una clave de identidad, puede establecer el valor en DatabaseGenerationOption.None.</span><span class="sxs-lookup"><span data-stu-id="1d585-226">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="1d585-227">Índice</span><span class="sxs-lookup"><span data-stu-id="1d585-227">Index</span></span>

> [!NOTE]
> <span data-ttu-id="1d585-228">**EF6.1 y versiones posteriores solo** -atributo el índice se introdujo en Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="1d585-228">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="1d585-229">Si usa una versión anterior no se aplica la información de esta sección.</span><span class="sxs-lookup"><span data-stu-id="1d585-229">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="1d585-230">Puede crear un índice en una o varias columnas mediante el **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="1d585-230">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="1d585-231">Agregar el atributo a una o varias propiedades impedirán EF crear el índice correspondiente en la base de datos cuando crea la base de datos, o aplicar la técnica scaffolding correspondiente **CreateIndex** llama si usa migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="1d585-231">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="1d585-232">Por ejemplo, se producirá el siguiente código en un índice que se va a crear el **clasificación** columna de la **publicaciones** tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1d585-232">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

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

<span data-ttu-id="1d585-233">De forma predeterminada, el índice se denominará **IX\_&lt;nombre de la propiedad&gt;**  (IX\_clasificación en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="1d585-233">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="1d585-234">Aunque también puede especificar un nombre para el índice.</span><span class="sxs-lookup"><span data-stu-id="1d585-234">You can also specify a name for the index though.</span></span> <span data-ttu-id="1d585-235">El ejemplo siguiente especifica que el índice debe denominarse **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="1d585-235">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="1d585-236">De forma predeterminada, los índices no son únicas, pero puede usar el **IsUnique** con el nombre de parámetro para especificar que un índice debe ser único.</span><span class="sxs-lookup"><span data-stu-id="1d585-236">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="1d585-237">El ejemplo siguiente presenta un índice único en un **usuario**del nombre de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="1d585-237">The following example introduces a unique index on a **User**'s login name.</span></span>

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

### <a name="multiple-column-indexes"></a><span data-ttu-id="1d585-238">Índices de varias columnas</span><span class="sxs-lookup"><span data-stu-id="1d585-238">Multiple-Column Indexes</span></span>

<span data-ttu-id="1d585-239">Los índices que abarcan varias columnas se especifican con el mismo nombre en varias anotaciones de índice para una tabla determinada.</span><span class="sxs-lookup"><span data-stu-id="1d585-239">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="1d585-240">Al crear los índices de varias columnas, deberá especificar un orden de las columnas en el índice.</span><span class="sxs-lookup"><span data-stu-id="1d585-240">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="1d585-241">Por ejemplo, el código siguiente crea un índice de varias columna en **clasificación** y **BlogId** llamado **IX\_BlogAndRating**.</span><span class="sxs-lookup"><span data-stu-id="1d585-241">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="1d585-242">**BlogId** es la primera columna en el índice y **clasificación** es el segundo.</span><span class="sxs-lookup"><span data-stu-id="1d585-242">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="1d585-243">Relación de atributos: InverseProperty y ForeignKey</span><span class="sxs-lookup"><span data-stu-id="1d585-243">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="1d585-244">Esta página proporciona información sobre cómo configurar las relaciones en el modelo de Code First usando anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="1d585-244">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="1d585-245">Para obtener información general acerca de las relaciones de EF y cómo obtener acceso y manipular datos mediante relaciones, vea [& Propiedades de navegación de relaciones](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="1d585-245">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="1d585-246">Convención de primer código que se encargará de las relaciones en el modelo más común, pero hay algunos casos donde necesita ayuda.</span><span class="sxs-lookup"><span data-stu-id="1d585-246">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="1d585-247">Cambiar el nombre de la propiedad de clave en la clase Blog supuso un problema con su relación con Post.</span><span class="sxs-lookup"><span data-stu-id="1d585-247">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="1d585-248">Al generar la base de datos, código primero ve la propiedad BlogId en la clase de publicación y lo reconoce, la convención que coincide con un nombre de la clase más "Id", como una clave externa a la clase de Blog.</span><span class="sxs-lookup"><span data-stu-id="1d585-248">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="1d585-249">Pero no hay ninguna propiedad BlogId en la clase de blog.</span><span class="sxs-lookup"><span data-stu-id="1d585-249">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="1d585-250">La solución para esto consiste en crear una propiedad de navegación en la publicación y use el DataAnnotation externa para código entender primero cómo crear la relación entre las dos clases: mediante la propiedad Post.BlogId —, así como también especificar restricciones en el base de datos.</span><span class="sxs-lookup"><span data-stu-id="1d585-250">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

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

<span data-ttu-id="1d585-251">La restricción en la base de datos muestra una relación entre InternalBlogs.PrimaryTrackingKey y Posts.BlogId.</span><span class="sxs-lookup"><span data-stu-id="1d585-251">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="1d585-253">Se usa el InverseProperty cuando tiene varias relaciones entre las clases.</span><span class="sxs-lookup"><span data-stu-id="1d585-253">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="1d585-254">En la clase de publicación, puede realizar un seguimiento de quién escribió una entrada de blog, así como quién editó.</span><span class="sxs-lookup"><span data-stu-id="1d585-254">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="1d585-255">Presentamos dos nuevas propiedades de navegación para la clase de publicación.</span><span class="sxs-lookup"><span data-stu-id="1d585-255">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="1d585-256">También deberá agregar en la clase Person que se hace referencia a estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="1d585-256">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="1d585-257">La clase de persona tiene propiedades de navegación a la publicación, uno para todas las entradas escritas por la persona y otra para todas las publicaciones actualizadas por esa persona.</span><span class="sxs-lookup"><span data-stu-id="1d585-257">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="1d585-258">En primer lugar no es capaz de hacer coincidir las propiedades en las dos clases en su propio código.</span><span class="sxs-lookup"><span data-stu-id="1d585-258">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="1d585-259">La tabla de base de datos de publicaciones debe tener una clave externa de la persona CreatedBy y uno para la persona UpdatedBy pero el código en primer lugar creará cuatro propiedades clave externa de voluntad: persona\_Id, persona\_Id1, CreatedBy\_Id y UpdatedBy\_identificador.</span><span class="sxs-lookup"><span data-stu-id="1d585-259">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="1d585-261">Para corregir estos problemas, puede usar la anotación InverseProperty para especificar la alineación de las propiedades.</span><span class="sxs-lookup"><span data-stu-id="1d585-261">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="1d585-262">Dado que la propiedad PostsWritten en persona sabe que esto hace referencia al tipo de publicación, compilará la relación para Post.CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="1d585-262">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="1d585-263">Del mismo modo, se conectará PostsUpdated a Post.UpdatedBy.</span><span class="sxs-lookup"><span data-stu-id="1d585-263">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="1d585-264">Y código primero no creará las claves externas adicionales.</span><span class="sxs-lookup"><span data-stu-id="1d585-264">And code first will not create the extra foreign keys.</span></span>

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="1d585-266">Resumen</span><span class="sxs-lookup"><span data-stu-id="1d585-266">Summary</span></span>

<span data-ttu-id="1d585-267">DataAnnotations no sólo permiten describir la validación del lado de cliente y servidor en las clases de código primero, pero también permiten mejorar y corregir incluso las suposiciones que código hará que primero sobre las clases según las convenciones.</span><span class="sxs-lookup"><span data-stu-id="1d585-267">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="1d585-268">Con DataAnnotations puede no solo unidad generación de esquema de base de datos, pero también puede asignar las clases de código primero a una base de datos ya existente.</span><span class="sxs-lookup"><span data-stu-id="1d585-268">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="1d585-269">Mientras están muy flexibles, tenga en cuenta que DataAnnotations se proporcionan solo más normalmente, son necesarios cambios de configuración que puede realizar en las clases de código primero.</span><span class="sxs-lookup"><span data-stu-id="1d585-269">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="1d585-270">Para configurar las clases para algunos de los casos extremos, deberá tener en cuenta el mecanismo de configuración alternativa, la API Fluent de Code First.</span><span class="sxs-lookup"><span data-stu-id="1d585-270">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
