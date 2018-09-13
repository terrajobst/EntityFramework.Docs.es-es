---
title: Convenciones de código personalizado First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489849"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="814af-102">Primera convenciones de código personalizado</span><span class="sxs-lookup"><span data-stu-id="814af-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="814af-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="814af-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="814af-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="814af-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="814af-105">Cuando se usa el modelo se calcula a partir de sus clases mediante un conjunto de convenciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="814af-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="814af-106">El valor predeterminado [convenciones de Code First](~/ef6/modeling/code-first/conventions/built-in.md) determinar aspectos como la que propiedad pasa a ser la clave principal de una entidad, el nombre de la tabla que se asigna a una entidad y qué precisión y escala de una columna decimal tiene de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="814af-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="814af-107">A veces estas convenciones predeterminadas no son ideales para el modelo, y tendrá que trabajar en torno a ellas mediante la configuración de muchas entidades individuales mediante las anotaciones de datos o la API Fluent.</span><span class="sxs-lookup"><span data-stu-id="814af-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="814af-108">Convenciones de Code First personalizadas le permiten definir sus propias convenciones que proporcionan los valores predeterminados de configuración para el modelo.</span><span class="sxs-lookup"><span data-stu-id="814af-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="814af-109">En este tutorial, se explorará los diferentes tipos de convenciones personalizadas y cómo crear cada uno de ellos.</span><span class="sxs-lookup"><span data-stu-id="814af-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="814af-110">Convenciones de modelo</span><span class="sxs-lookup"><span data-stu-id="814af-110">Model-Based Conventions</span></span>

<span data-ttu-id="814af-111">Esta página cubre la API DbModelBuilder para convenciones personalizadas.</span><span class="sxs-lookup"><span data-stu-id="814af-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="814af-112">Esta API debería ser suficiente para la creación de la mayoría de convenciones personalizadas.</span><span class="sxs-lookup"><span data-stu-id="814af-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="814af-113">Sin embargo, es también la capacidad para crear convenciones basadas en modelos - convenciones que manipulan el modelo final una vez que se crea, para controlar escenarios avanzados.</span><span class="sxs-lookup"><span data-stu-id="814af-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="814af-114">Para obtener más información, consulte [convenciones basadas en modelos](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="814af-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="814af-115">Nuestro modelo</span><span class="sxs-lookup"><span data-stu-id="814af-115">Our Model</span></span>

<span data-ttu-id="814af-116">Comenzaremos por definir un modelo simple que podemos usar con las convenciones.</span><span class="sxs-lookup"><span data-stu-id="814af-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="814af-117">Agregue las siguientes clases al proyecto.</span><span class="sxs-lookup"><span data-stu-id="814af-117">Add the following classes to your project.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="814af-118">Introducción a las convenciones personalizadas</span><span class="sxs-lookup"><span data-stu-id="814af-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="814af-119">Vamos a escribir una convención que se configura ninguna propiedad clave como clave principal para su tipo de entidad con nombre.</span><span class="sxs-lookup"><span data-stu-id="814af-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="814af-120">Las convenciones están habilitadas en el generador de modelos, que se puede acceder mediante la invalidación de OnModelCreating en el contexto.</span><span class="sxs-lookup"><span data-stu-id="814af-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="814af-121">Actualice la clase ProductContext como sigue:</span><span class="sxs-lookup"><span data-stu-id="814af-121">Update the ProductContext class as follows:</span></span>

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

<span data-ttu-id="814af-122">Ahora, cualquier propiedad en nuestro modelo de clave con nombre será configurada como la clave principal de cualquier entidad su parte.</span><span class="sxs-lookup"><span data-stu-id="814af-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="814af-123">También se puede hacer las convenciones más específico mediante el filtrado en el tipo de propiedad que vamos a configurar:</span><span class="sxs-lookup"><span data-stu-id="814af-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="814af-124">Esto configurará todas las propiedades que se denomina clave para que sea la principal clave de su entidad, pero sólo si se trata de un entero.</span><span class="sxs-lookup"><span data-stu-id="814af-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="814af-125">Una característica del método IsKey interesante es que es aditiva.</span><span class="sxs-lookup"><span data-stu-id="814af-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="814af-126">Lo que significa que si se llama IsKey en varias propiedades y que todos pasará a formar parte de una clave compuesta.</span><span class="sxs-lookup"><span data-stu-id="814af-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="814af-127">Para esto tenerse en cuenta es que al especificar varias propiedades para una clave también debe especificar un orden de esas propiedades.</span><span class="sxs-lookup"><span data-stu-id="814af-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="814af-128">Puede hacerlo mediante una llamada a la HasColumnOrder como el método siguiente:</span><span class="sxs-lookup"><span data-stu-id="814af-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="814af-129">Este código configurará los tipos en nuestro modelo de tener una clave compuesta que consta de la columna de clave de tipo int y la columna de nombre de cadena.</span><span class="sxs-lookup"><span data-stu-id="814af-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="814af-130">Si observamos el modelo en el diseñador sería similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="814af-130">If we view the model in the designer it would look like this:</span></span>

![Clave compuesta](~/ef6/media/compositekey.png)

<span data-ttu-id="814af-132">Otro ejemplo de convenciones de la propiedad es configurar todas las propiedades de fecha y hora en mi modelo para asignar al tipo datetime2 en SQL Server en lugar de fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="814af-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="814af-133">Puede lograr esto con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="814af-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="814af-134">Clases de convención</span><span class="sxs-lookup"><span data-stu-id="814af-134">Convention Classes</span></span>

<span data-ttu-id="814af-135">Otra manera de definir las convenciones es usar una clase de convención para encapsular la convención.</span><span class="sxs-lookup"><span data-stu-id="814af-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="814af-136">Cuando se usa una clase de convención de crear un tipo que hereda de la clase convención en el espacio de nombres System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="814af-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="814af-137">Podemos crear una clase de convención con la convención de datetime2 que se mostraba anteriormente haciendo lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="814af-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

<span data-ttu-id="814af-138">Para indicar a EF que use esta convención de que agregarlo a la colección de convenciones en OnModelCreating, que si ha seguido todo el tutorial tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="814af-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="814af-139">Como puede ver, agregar una instancia de nuestra convención a la colección de convenciones.</span><span class="sxs-lookup"><span data-stu-id="814af-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="814af-140">Heredar de convención proporciona una manera cómoda de agrupar y compartir convenciones entre equipos o proyectos.</span><span class="sxs-lookup"><span data-stu-id="814af-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="814af-141">Por ejemplo, podría tener una biblioteca de clases con un conjunto común de todas las organizaciones proyectos usen las convenciones.</span><span class="sxs-lookup"><span data-stu-id="814af-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="814af-142">Atributos personalizados</span><span class="sxs-lookup"><span data-stu-id="814af-142">Custom Attributes</span></span>

<span data-ttu-id="814af-143">Es otro excelente uso de convenciones habilitar nuevos atributos que se usará al configurar un modelo.</span><span class="sxs-lookup"><span data-stu-id="814af-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="814af-144">Para ilustrar esto, vamos a crear un atributo que podemos usar para marcar las propiedades de cadena como no Unicode.</span><span class="sxs-lookup"><span data-stu-id="814af-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="814af-145">Ahora, vamos a crear una convención para aplicar este atributo a nuestro modelo:</span><span class="sxs-lookup"><span data-stu-id="814af-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="814af-146">Con esta convención, podemos agregar el atributo no Unicode a cualquiera de nuestras propiedades de cadena, lo que significa que la columna de la base de datos se almacenarán como varchar en lugar de nvarchar.</span><span class="sxs-lookup"><span data-stu-id="814af-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="814af-147">Hay que destacar sobre esta convención es si coloca el atributo no Unicode en algo distinto de una propiedad de cadena, a continuación, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="814af-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="814af-148">Para hacerlo porque no se puede configurar IsUnicode en cualquier tipo distinto de una cadena.</span><span class="sxs-lookup"><span data-stu-id="814af-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="814af-149">Si esto ocurre, entonces puede hacer su convención de más específico, para que filtra todo lo que no es una cadena.</span><span class="sxs-lookup"><span data-stu-id="814af-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="814af-150">Aunque la convención anterior funciona para definir atributos personalizados, hay otra API que puede ser mucho más fácil de usar, especialmente cuando desee usar las propiedades de la clase de atributo.</span><span class="sxs-lookup"><span data-stu-id="814af-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="814af-151">En este ejemplo, vamos a actualizar el atributo y cámbielo a un atributo IsUnicode, por lo que este aspecto:</span><span class="sxs-lookup"><span data-stu-id="814af-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

<span data-ttu-id="814af-152">Una vez que tenemos esto, podemos establecer un valor booleano en el atributo para indicar a la convención o no una propiedad debe ser Unicode.</span><span class="sxs-lookup"><span data-stu-id="814af-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="814af-153">Podríamos hacer esto en la convención que ya tenemos mediante el acceso a la ClrProperty de la clase de configuración similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="814af-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="814af-154">Esto es bastante fácil, pero hay una manera más concisa de conseguir esto mediante el uso de la necesidad de método de la API de convenciones.</span><span class="sxs-lookup"><span data-stu-id="814af-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="814af-155">La necesidad de método tiene un parámetro de tipo Func&lt;PropertyInfo, T&gt; que acepta PropertyInfo igual que el lugar en el método, pero se espera que devuelva un objeto.</span><span class="sxs-lookup"><span data-stu-id="814af-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="814af-156">Si el objeto devuelto es null, a continuación, la propiedad no se configurarán, lo que significa que puede filtrar las propiedades con él como Where, pero es diferente que también se captura el objeto devuelto y pasarlo al método Configure.</span><span class="sxs-lookup"><span data-stu-id="814af-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="814af-157">Esto funciona del modo siguiente:</span><span class="sxs-lookup"><span data-stu-id="814af-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="814af-158">Los atributos personalizados no son la única razón para usar la necesidad de método, es útil en cualquier lugar que necesita para razonar sobre algo que se va a filtrar al configurar los tipos o las propiedades.</span><span class="sxs-lookup"><span data-stu-id="814af-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="814af-159">Configuración de tipos</span><span class="sxs-lookup"><span data-stu-id="814af-159">Configuring Types</span></span>

<span data-ttu-id="814af-160">Hasta ahora, todas las convenciones han para las propiedades, pero hay otra área de las convenciones de API para configurar los tipos en el modelo.</span><span class="sxs-lookup"><span data-stu-id="814af-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="814af-161">La experiencia es similar a las convenciones que hemos visto hasta ahora, pero las opciones de configuración interior estará en la entidad en lugar de la propiedad de nivel.</span><span class="sxs-lookup"><span data-stu-id="814af-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="814af-162">Una de las cosas que las convenciones de nivel de tipo pueden ser muy útiles para cambiar la convención de nomenclatura de tabla, para asignar a un esquema existente que difiere del valor predeterminado EF o para crear una nueva base de datos con una convención de nomenclatura diferente.</span><span class="sxs-lookup"><span data-stu-id="814af-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="814af-163">Para hacer esto en primer lugar necesitamos un método que puede aceptar el TypeInfo para un tipo en nuestro modelo y devolver cuál debe ser el nombre de tabla para ese tipo:</span><span class="sxs-lookup"><span data-stu-id="814af-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="814af-164">Este método toma un tipo y devuelve una cadena que se usa en minúsculas, con caracteres de subrayado en lugar de CamelCase.</span><span class="sxs-lookup"><span data-stu-id="814af-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="814af-165">En nuestro modelo, esto significa que la clase ProductCategory se asignará a una tabla denominada product\_categoría en lugar de ProductCategories.</span><span class="sxs-lookup"><span data-stu-id="814af-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="814af-166">Una vez que tenemos ese método podemos llamarlo en una convención similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="814af-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="814af-167">Esta convención configura todos los tipos en nuestro modelo para asignar al nombre de tabla que se devuelve desde el método GetTableName.</span><span class="sxs-lookup"><span data-stu-id="814af-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="814af-168">Esta convención es el equivalente a llamar al método ToTable para cada entidad en el modelo mediante la API Fluent.</span><span class="sxs-lookup"><span data-stu-id="814af-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="814af-169">Una cosa a tener en cuenta acerca de esto es que cuando se llama a EF ToTable tendrá la cadena que se proporciona como el nombre de tabla exacto, sin cambios de la pluralización que lo haría normalmente al determinar los nombres de tabla.</span><span class="sxs-lookup"><span data-stu-id="814af-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="814af-170">Por el nombre de tabla desde nuestra convención es producto\_categoría en lugar de producto\_categorías.</span><span class="sxs-lookup"><span data-stu-id="814af-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="814af-171">Podemos resolver en nuestra convención mediante una llamada al servicio de pluralización nosotros mismos.</span><span class="sxs-lookup"><span data-stu-id="814af-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="814af-172">En el siguiente código se usará el [resolución de dependencia](~/ef6/fundamentals/configuring/dependency-resolution.md) característica agregada en EF6 para recuperar el servicio de pluralización que usaría EF y Pluralizar el nombre de la tabla.</span><span class="sxs-lookup"><span data-stu-id="814af-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> <span data-ttu-id="814af-173">La versión genérica de GetService es un método de extensión en el espacio de nombres System.Data.Entity.Infrastructure.DependencyResolution, debe agregar un mediante la instrucción a su contexto con el fin de usarlo.</span><span class="sxs-lookup"><span data-stu-id="814af-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="814af-174">ToTable y herencia</span><span class="sxs-lookup"><span data-stu-id="814af-174">ToTable and Inheritance</span></span>

<span data-ttu-id="814af-175">Otro aspecto importante de ToTable es que si se asigna explícitamente un tipo a una tabla determinada, a continuación, puede modificar la estrategia de asignación que va a usar EF.</span><span class="sxs-lookup"><span data-stu-id="814af-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="814af-176">Si se llama a ToTable para todos los tipos en una jerarquía de herencia, pasando el nombre de tipo como el nombre de la tabla como hicimos anteriormente, a continuación, cambiará la estrategia de asignación de tabla por jerarquía (TPH) predeterminada a la tabla por tipo (TPT).</span><span class="sxs-lookup"><span data-stu-id="814af-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="814af-177">La mejor manera de describir esto es un ejemplo concreto de whith:</span><span class="sxs-lookup"><span data-stu-id="814af-177">The best way to describe this is whith a concrete example:</span></span>

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

<span data-ttu-id="814af-178">De forma predeterminada el empleado y administrador se asignan a la misma tabla (empleados) en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="814af-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="814af-179">La tabla contendrá los empleados y los administradores de una columna discriminadora que le indicará qué tipo de instancia se almacena en cada fila.</span><span class="sxs-lookup"><span data-stu-id="814af-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="814af-180">Se trata de la asignación de TPH porque no hay una sola tabla para la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="814af-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="814af-181">Sin embargo, si se llama a ToTable en ambos classe, a continuación, cada tipo en su lugar, asignará a su propia tabla, también conocido como TPT, ya que cada tipo tiene su propia tabla.</span><span class="sxs-lookup"><span data-stu-id="814af-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="814af-182">El código anterior se asignará a una estructura de tabla con el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="814af-182">The code above will map to a table structure that looks like the following:</span></span>

![Ejemplo de TPT](~/ef6/media/tptexample.jpg)

<span data-ttu-id="814af-184">Puede evitar este problema y mantener la asignación de TPH de forma predeterminada, en un par de formas:</span><span class="sxs-lookup"><span data-stu-id="814af-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="814af-185">Llame a ToTable con el mismo nombre de tabla para cada tipo de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="814af-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="814af-186">Llame a ToTable solo en la clase base de la jerarquía, en nuestro ejemplo, este sería el empleado.</span><span class="sxs-lookup"><span data-stu-id="814af-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="814af-187">Orden de ejecución</span><span class="sxs-lookup"><span data-stu-id="814af-187">Execution Order</span></span>

<span data-ttu-id="814af-188">Convenciones de operan en una última manera wins, igual que la API Fluent.</span><span class="sxs-lookup"><span data-stu-id="814af-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="814af-189">Esto significa que si escribe dos convenciones que configuran la misma opción de la misma propiedad y, a continuación, la última de ellas para ejecutar wins.</span><span class="sxs-lookup"><span data-stu-id="814af-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="814af-190">Por ejemplo, en el código siguiente se establece la longitud máxima de todas las cadenas en 500, pero, a continuación, configure todas las propiedades que se llama nombre del modelo que tiene una longitud máxima de 250.</span><span class="sxs-lookup"><span data-stu-id="814af-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="814af-191">Dado que la convención para establecer la longitud máxima en 250 es posterior a la que todas las cadenas se establece en 500, todas las propiedades que se denomina nombre de nuestro modelo tendrá una longitud máxima de 250 mientras otras cadenas, como descripciones, sería 500.</span><span class="sxs-lookup"><span data-stu-id="814af-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="814af-192">Convenciones de uso de esta manera significa que puede proporcionar una convención general para tipos o propiedades en el modelo y, a continuación, override a la de los subconjuntos que son diferentes.</span><span class="sxs-lookup"><span data-stu-id="814af-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="814af-193">La API Fluent y las anotaciones de datos pueden usarse también para reemplazar una convención en casos concretos.</span><span class="sxs-lookup"><span data-stu-id="814af-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="814af-194">En nuestro ejemplo anterior si hubiéramos utilizado la API Fluent para establecer la longitud máxima de una propiedad, a continuación, podríamos haber colocamos lo antes o después de la convención, porque tendrán prioridad en la API Fluent más específica a través de la convención de configuración más generales.</span><span class="sxs-lookup"><span data-stu-id="814af-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="814af-195">Convenciones integradas</span><span class="sxs-lookup"><span data-stu-id="814af-195">Built-in Conventions</span></span>

<span data-ttu-id="814af-196">Dado que las convenciones personalizadas podrían verse afectadas por las convenciones de Code First de forma predeterminada, puede ser útil agregar convenciones que se ejecuten antes o después de otra convención.</span><span class="sxs-lookup"><span data-stu-id="814af-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="814af-197">Para ello puede utilizar los métodos de AddBefore y AddAfter del conjunto de convenciones en DbContext derivada.</span><span class="sxs-lookup"><span data-stu-id="814af-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="814af-198">El código siguiente agregaría la clase convención que creamos anteriormente, por lo que se ejecute antes de la compilación en la convención de detección de las claves.</span><span class="sxs-lookup"><span data-stu-id="814af-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="814af-199">Esto va a ser más útil al agregar las convenciones que se deben ejecutar antes o después de las convenciones integradas, una lista de las convenciones integradas puede encontrarse aquí: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .</span><span class="sxs-lookup"><span data-stu-id="814af-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="814af-200">También puede quitar las convenciones que no desee aplicadas al modelo.</span><span class="sxs-lookup"><span data-stu-id="814af-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="814af-201">Para quitar una convención, use el método Remove.</span><span class="sxs-lookup"><span data-stu-id="814af-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="814af-202">Este es un ejemplo de la eliminación de la PluralizingTableNameConvention.</span><span class="sxs-lookup"><span data-stu-id="814af-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
