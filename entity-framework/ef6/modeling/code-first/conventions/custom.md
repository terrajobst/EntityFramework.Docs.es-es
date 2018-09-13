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
# <a name="custom-code-first-conventions"></a>Primera convenciones de código personalizado
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

Cuando se usa el modelo se calcula a partir de sus clases mediante un conjunto de convenciones de Code First. El valor predeterminado [convenciones de Code First](~/ef6/modeling/code-first/conventions/built-in.md) determinar aspectos como la que propiedad pasa a ser la clave principal de una entidad, el nombre de la tabla que se asigna a una entidad y qué precisión y escala de una columna decimal tiene de forma predeterminada.

A veces estas convenciones predeterminadas no son ideales para el modelo, y tendrá que trabajar en torno a ellas mediante la configuración de muchas entidades individuales mediante las anotaciones de datos o la API Fluent. Convenciones de Code First personalizadas le permiten definir sus propias convenciones que proporcionan los valores predeterminados de configuración para el modelo. En este tutorial, se explorará los diferentes tipos de convenciones personalizadas y cómo crear cada uno de ellos.


## <a name="model-based-conventions"></a>Convenciones de modelo

Esta página cubre la API DbModelBuilder para convenciones personalizadas. Esta API debería ser suficiente para la creación de la mayoría de convenciones personalizadas. Sin embargo, es también la capacidad para crear convenciones basadas en modelos - convenciones que manipulan el modelo final una vez que se crea, para controlar escenarios avanzados. Para obtener más información, consulte [convenciones basadas en modelos](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Nuestro modelo

Comenzaremos por definir un modelo simple que podemos usar con las convenciones. Agregue las siguientes clases al proyecto.

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

 

## <a name="introducing-custom-conventions"></a>Introducción a las convenciones personalizadas

Vamos a escribir una convención que se configura ninguna propiedad clave como clave principal para su tipo de entidad con nombre.

Las convenciones están habilitadas en el generador de modelos, que se puede acceder mediante la invalidación de OnModelCreating en el contexto. Actualice la clase ProductContext como sigue:

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

Ahora, cualquier propiedad en nuestro modelo de clave con nombre será configurada como la clave principal de cualquier entidad su parte.

También se puede hacer las convenciones más específico mediante el filtrado en el tipo de propiedad que vamos a configurar:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Esto configurará todas las propiedades que se denomina clave para que sea la principal clave de su entidad, pero sólo si se trata de un entero.

Una característica del método IsKey interesante es que es aditiva. Lo que significa que si se llama IsKey en varias propiedades y que todos pasará a formar parte de una clave compuesta. Para esto tenerse en cuenta es que al especificar varias propiedades para una clave también debe especificar un orden de esas propiedades. Puede hacerlo mediante una llamada a la HasColumnOrder como el método siguiente:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Este código configurará los tipos en nuestro modelo de tener una clave compuesta que consta de la columna de clave de tipo int y la columna de nombre de cadena. Si observamos el modelo en el diseñador sería similar al siguiente:

![Clave compuesta](~/ef6/media/compositekey.png)

Otro ejemplo de convenciones de la propiedad es configurar todas las propiedades de fecha y hora en mi modelo para asignar al tipo datetime2 en SQL Server en lugar de fecha y hora. Puede lograr esto con lo siguiente:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Clases de convención

Otra manera de definir las convenciones es usar una clase de convención para encapsular la convención. Cuando se usa una clase de convención de crear un tipo que hereda de la clase convención en el espacio de nombres System.Data.Entity.ModelConfiguration.Conventions.

Podemos crear una clase de convención con la convención de datetime2 que se mostraba anteriormente haciendo lo siguiente:

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

Para indicar a EF que use esta convención de que agregarlo a la colección de convenciones en OnModelCreating, que si ha seguido todo el tutorial tendrá un aspecto similar al siguiente:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Como puede ver, agregar una instancia de nuestra convención a la colección de convenciones. Heredar de convención proporciona una manera cómoda de agrupar y compartir convenciones entre equipos o proyectos. Por ejemplo, podría tener una biblioteca de clases con un conjunto común de todas las organizaciones proyectos usen las convenciones.

 

## <a name="custom-attributes"></a>Atributos personalizados

Es otro excelente uso de convenciones habilitar nuevos atributos que se usará al configurar un modelo. Para ilustrar esto, vamos a crear un atributo que podemos usar para marcar las propiedades de cadena como no Unicode.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Ahora, vamos a crear una convención para aplicar este atributo a nuestro modelo:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

Con esta convención, podemos agregar el atributo no Unicode a cualquiera de nuestras propiedades de cadena, lo que significa que la columna de la base de datos se almacenarán como varchar en lugar de nvarchar.

Hay que destacar sobre esta convención es si coloca el atributo no Unicode en algo distinto de una propiedad de cadena, a continuación, se producirá una excepción. Para hacerlo porque no se puede configurar IsUnicode en cualquier tipo distinto de una cadena. Si esto ocurre, entonces puede hacer su convención de más específico, para que filtra todo lo que no es una cadena.

Aunque la convención anterior funciona para definir atributos personalizados, hay otra API que puede ser mucho más fácil de usar, especialmente cuando desee usar las propiedades de la clase de atributo.

En este ejemplo, vamos a actualizar el atributo y cámbielo a un atributo IsUnicode, por lo que este aspecto:

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

Una vez que tenemos esto, podemos establecer un valor booleano en el atributo para indicar a la convención o no una propiedad debe ser Unicode. Podríamos hacer esto en la convención que ya tenemos mediante el acceso a la ClrProperty de la clase de configuración similar al siguiente:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Esto es bastante fácil, pero hay una manera más concisa de conseguir esto mediante el uso de la necesidad de método de la API de convenciones. La necesidad de método tiene un parámetro de tipo Func&lt;PropertyInfo, T&gt; que acepta PropertyInfo igual que el lugar en el método, pero se espera que devuelva un objeto. Si el objeto devuelto es null, a continuación, la propiedad no se configurarán, lo que significa que puede filtrar las propiedades con él como Where, pero es diferente que también se captura el objeto devuelto y pasarlo al método Configure. Esto funciona del modo siguiente:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Los atributos personalizados no son la única razón para usar la necesidad de método, es útil en cualquier lugar que necesita para razonar sobre algo que se va a filtrar al configurar los tipos o las propiedades.

 

## <a name="configuring-types"></a>Configuración de tipos

Hasta ahora, todas las convenciones han para las propiedades, pero hay otra área de las convenciones de API para configurar los tipos en el modelo. La experiencia es similar a las convenciones que hemos visto hasta ahora, pero las opciones de configuración interior estará en la entidad en lugar de la propiedad de nivel.

Una de las cosas que las convenciones de nivel de tipo pueden ser muy útiles para cambiar la convención de nomenclatura de tabla, para asignar a un esquema existente que difiere del valor predeterminado EF o para crear una nueva base de datos con una convención de nomenclatura diferente. Para hacer esto en primer lugar necesitamos un método que puede aceptar el TypeInfo para un tipo en nuestro modelo y devolver cuál debe ser el nombre de tabla para ese tipo:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Este método toma un tipo y devuelve una cadena que se usa en minúsculas, con caracteres de subrayado en lugar de CamelCase. En nuestro modelo, esto significa que la clase ProductCategory se asignará a una tabla denominada product\_categoría en lugar de ProductCategories.

Una vez que tenemos ese método podemos llamarlo en una convención similar al siguiente:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Esta convención configura todos los tipos en nuestro modelo para asignar al nombre de tabla que se devuelve desde el método GetTableName. Esta convención es el equivalente a llamar al método ToTable para cada entidad en el modelo mediante la API Fluent.

Una cosa a tener en cuenta acerca de esto es que cuando se llama a EF ToTable tendrá la cadena que se proporciona como el nombre de tabla exacto, sin cambios de la pluralización que lo haría normalmente al determinar los nombres de tabla. Por el nombre de tabla desde nuestra convención es producto\_categoría en lugar de producto\_categorías. Podemos resolver en nuestra convención mediante una llamada al servicio de pluralización nosotros mismos.

En el siguiente código se usará el [resolución de dependencia](~/ef6/fundamentals/configuring/dependency-resolution.md) característica agregada en EF6 para recuperar el servicio de pluralización que usaría EF y Pluralizar el nombre de la tabla.

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
> La versión genérica de GetService es un método de extensión en el espacio de nombres System.Data.Entity.Infrastructure.DependencyResolution, debe agregar un mediante la instrucción a su contexto con el fin de usarlo.

### <a name="totable-and-inheritance"></a>ToTable y herencia

Otro aspecto importante de ToTable es que si se asigna explícitamente un tipo a una tabla determinada, a continuación, puede modificar la estrategia de asignación que va a usar EF. Si se llama a ToTable para todos los tipos en una jerarquía de herencia, pasando el nombre de tipo como el nombre de la tabla como hicimos anteriormente, a continuación, cambiará la estrategia de asignación de tabla por jerarquía (TPH) predeterminada a la tabla por tipo (TPT). La mejor manera de describir esto es un ejemplo concreto de whith:

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

De forma predeterminada el empleado y administrador se asignan a la misma tabla (empleados) en la base de datos. La tabla contendrá los empleados y los administradores de una columna discriminadora que le indicará qué tipo de instancia se almacena en cada fila. Se trata de la asignación de TPH porque no hay una sola tabla para la jerarquía. Sin embargo, si se llama a ToTable en ambos classe, a continuación, cada tipo en su lugar, asignará a su propia tabla, también conocido como TPT, ya que cada tipo tiene su propia tabla.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

El código anterior se asignará a una estructura de tabla con el siguiente aspecto:

![Ejemplo de TPT](~/ef6/media/tptexample.jpg)

Puede evitar este problema y mantener la asignación de TPH de forma predeterminada, en un par de formas:

1.  Llame a ToTable con el mismo nombre de tabla para cada tipo de la jerarquía.
2.  Llame a ToTable solo en la clase base de la jerarquía, en nuestro ejemplo, este sería el empleado.

 

## <a name="execution-order"></a>Orden de ejecución

Convenciones de operan en una última manera wins, igual que la API Fluent. Esto significa que si escribe dos convenciones que configuran la misma opción de la misma propiedad y, a continuación, la última de ellas para ejecutar wins. Por ejemplo, en el código siguiente se establece la longitud máxima de todas las cadenas en 500, pero, a continuación, configure todas las propiedades que se llama nombre del modelo que tiene una longitud máxima de 250.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Dado que la convención para establecer la longitud máxima en 250 es posterior a la que todas las cadenas se establece en 500, todas las propiedades que se denomina nombre de nuestro modelo tendrá una longitud máxima de 250 mientras otras cadenas, como descripciones, sería 500. Convenciones de uso de esta manera significa que puede proporcionar una convención general para tipos o propiedades en el modelo y, a continuación, override a la de los subconjuntos que son diferentes.

La API Fluent y las anotaciones de datos pueden usarse también para reemplazar una convención en casos concretos. En nuestro ejemplo anterior si hubiéramos utilizado la API Fluent para establecer la longitud máxima de una propiedad, a continuación, podríamos haber colocamos lo antes o después de la convención, porque tendrán prioridad en la API Fluent más específica a través de la convención de configuración más generales.

 

## <a name="built-in-conventions"></a>Convenciones integradas

Dado que las convenciones personalizadas podrían verse afectadas por las convenciones de Code First de forma predeterminada, puede ser útil agregar convenciones que se ejecuten antes o después de otra convención. Para ello puede utilizar los métodos de AddBefore y AddAfter del conjunto de convenciones en DbContext derivada. El código siguiente agregaría la clase convención que creamos anteriormente, por lo que se ejecute antes de la compilación en la convención de detección de las claves.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Esto va a ser más útil al agregar las convenciones que se deben ejecutar antes o después de las convenciones integradas, una lista de las convenciones integradas puede encontrarse aquí: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .

También puede quitar las convenciones que no desee aplicadas al modelo. Para quitar una convención, use el método Remove. Este es un ejemplo de la eliminación de la PluralizingTableNameConvention.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
