---
title: Trabajar con valores de propiedad - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
caps.latest.revision: 3
ms.openlocfilehash: 07b71c9efe4e1fc3fd25a52c9cfb25f61e92f859
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121919"
---
# <a name="working-with-property-values"></a>Trabajar con valores de propiedad
La mayor parte, Entity Framework se encargará de seguimiento del estado, los valores originales y los valores actuales de las propiedades de las instancias de entidad. Sin embargo, puede haber algunos casos, como los escenarios desconectados - donde desea ver o manipular la información de que EF tiene acerca de las propiedades. Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.  

Entity Framework realiza un seguimiento de los dos valores para cada propiedad de una entidad con seguimiento. El valor actual es, como su nombre indica, el valor actual de la propiedad de la entidad. El valor original es el valor de la propiedad que tenía cuando se consultan desde la base de datos o adjunta al contexto de la entidad.  

Existen dos mecanismos generales para trabajar con valores de propiedad:  

- El valor de una propiedad única puede obtenerse de una manera fuertemente tipada mediante el método de propiedad.  
- Los valores para todas las propiedades de una entidad pueden leerse en un objeto DbPropertyValues. DbPropertyValues, a continuación, actúa como un objeto de diccionario para permitir que se puede leer y establecer los valores de propiedad. Los valores de un objeto DbPropertyValues pueden establecerse de valores en otro objeto DbPropertyValues o de los valores de otro objeto, como otra copia de la entidad o un objeto de transferencia de datos simples (DTO).  

Las secciones siguientes muestran ejemplos del uso de los mecanismos anteriores ambos.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Obtener y establecer el valor original o actual de una propiedad individual  

El ejemplo siguiente muestra cómo se puede leer el valor actual de una propiedad y, a continuación, se establece en un nuevo valor:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(name).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Utilice la propiedad OriginalValue en lugar de la propiedad CurrentValue para leer o establecer el valor original.  

Tenga en cuenta que el valor devuelto se escribe como "objeto" cuando se usa una cadena para especificar el nombre de propiedad. Por otro lado, el valor devuelto está fuertemente tipado si se usa una expresión lambda.  

Establecer el valor de propiedad similar al siguiente solo marcar la propiedad como modificada si el nuevo valor es diferente del valor antiguo.  

Cuando se establece un valor de propiedad de este modo el cambio se detecta automáticamente, incluso si se desactiva AutoDetectChanges.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Obtener y establecer el valor actual de una propiedad sin asignar  

También se puede leer el valor actual de una propiedad que no está asignado a la base de datos. Un ejemplo de una propiedad sin asignar podría ser una propiedad RssLink en el Blog. Este valor se puede calcular basándose en la BlogId y, por tanto, no necesita almacenarse en la base de datos. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    // Read the current value of an unmapped property
    var rssLink = context.Entry(blog).Property(p => p.RssLink).CurrentValue;

    // Use a string to specify the property name
    var rssLinkAgain = context.Entry(blog).Property("RssLink").CurrentValue;
}
```  

También se puede establecer el valor actual si la propiedad expone un establecedor.  

Lectura de los valores de las propiedades sin asignar es útil al realizar la validación de Entity Framework de las propiedades sin asignar. Por la misma razón se pueden leer y establecer para propiedades de entidades que no contarán actualmente por el contexto de los valores actuales. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    // Create an entity that is not being tracked
    var blog = new Blog { Name = "ADO.NET Blog" };

    // Read and set the current value of Name as before
    var currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";
    var currentName2 = context.Entry(blog).Property("Name").CurrentValue;
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Tenga en cuenta que los valores originales no están disponibles para las propiedades sin asignar o de las propiedades de entidades que no se realiza el seguimiento por el contexto.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Comprobar si una propiedad está marcada como modificada  

El ejemplo siguiente muestra cómo comprobar si una propiedad individual se marca como modificado:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Los valores de las propiedades modificadas se envían como las actualizaciones a la base de datos cuando se llama a SaveChanges.  

##  <a name="marking-a-property-as-modified"></a>Marcar una propiedad como modificada  

El ejemplo siguiente muestra cómo forzar una propiedad individual esté marcado como modificado:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Al marcar una propiedad como modificada fuerza una actualización se puede enviar a la base de datos para la propiedad cuando se llama a SaveChanges incluso si el valor actual de la propiedad es igual que su valor original.  

No es actualmente posible restablecer una propiedad individual que no se puede modificar después de que se ha marcado como modificado. Esto es algo que tenemos previsto admitir en una versión futura.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>Lectura actuales, originales y los valores de la base de datos para todas las propiedades de una entidad  

El ejemplo siguiente muestra cómo leer los valores actuales, los valores originales y los valores realmente en la base de datos para todas las propiedades asignadas de una entidad.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Make a modification to Name in the tracked entity
    blog.Name = "My Cool Blog";

    // Make a modification to Name in the database
    context.Database.SqlCommand("update dbo.Blogs set Name = 'My Boring Blog' where Id = 1");

    // Print out current, original, and database values
    Console.WriteLine("Current values:");
    PrintValues(context.Entry(blog).CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(context.Entry(blog).OriginalValues);

    Console.WriteLine("\nDatabase values:");
    PrintValues(context.Entry(blog).GetDatabaseValues());
}

public static void PrintValues(DbPropertyValues values)
{
    foreach (var propertyName in values.PropertyNames)
    {
        Console.WriteLine("Property {0} has value {1}",
                          propertyName, values[propertyName]);
    }
}
```  

Los valores actuales son los valores que contienen las propiedades de la entidad. Los valores originales son los valores que se leyeron desde la base de datos cuando la entidad se consultó. Los valores de la base de datos son los valores que estén almacenadas actualmente en la base de datos. Obtener los valores de la base de datos es útil cuando los valores de la base de datos pueden haber cambiado desde que se consultó la entidad, como cuando edita un simultáneas a la base de datos se ha realizado por otro usuario.  

## <a name="setting-current-or-original-values-from-another-object"></a>Establecer valores originales actuales de otro objeto  

Los valores originales de una entidad con seguimiento actuales pueden actualizarse mediante la copia de los valores de otro objeto. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var coolBlog = new Blog { Id = 1, Name = "My Cool Blog" };
    var boringBlog = new BlogDto { Id = 1, Name = "My Boring Blog" };

    // Change the current and original values by copying the values from other objects
    var entry = context.Entry(blog);
    entry.CurrentValues.SetValues(coolBlog);
    entry.OriginalValues.SetValues(boringBlog);

    // Print out current and original values
    Console.WriteLine("Current values:");
    PrintValues(entry.CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(entry.OriginalValues);
}

public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```  

Ejecuta el código anterior imprimirá:  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Esta técnica a veces se utiliza al actualizar una entidad con los valores obtenidos de una llamada de servicio o un cliente en una aplicación de n niveles. Tenga en cuenta que usa el objeto no tiene que ser del mismo tipo que la entidad siempre y cuando dicha clase tiene propiedades cuyos nombres coinciden con los de la entidad. En el ejemplo anterior, se utiliza una instancia de BlogDTO para actualizar los valores originales.  

Tenga en cuenta que solo las propiedades que se establecen en valores diferentes cuando se copian desde el otro objeto se marcará como modificada.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Valores de configuración originales o actuales de un diccionario  

Los valores originales de una entidad con seguimiento actuales pueden actualizarse mediante la copia de los valores de un diccionario o alguna otra estructura de datos. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "The New ADO.NET Blog" },
        { "Url", "blogs.msdn.com/adonet" },
    };

    var currentValues = context.Entry(blog).CurrentValues;

    foreach (var propertyName in newValues.Keys)
    {
        currentValues[propertyName] = newValues[propertyName];
    }

    PrintValues(currentValues);
}
```  

Utilice la propiedad OriginalValues en lugar de la propiedad CurrentValues para establecer los valores originales.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Valores de configuración originales o actuales de un diccionario utilizando la propiedad  

Una alternativa al uso CurrentValues o OriginalValues como se mostró anteriormente es usar el método de propiedad para establecer el valor de cada propiedad. Esto puede ser preferible cuando se necesita establecer los valores de propiedades complejas. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "John Doe" },
        { "Location.City", "Redmond" },
        { "Location.State.Name", "Washington" },
        { "Location.State.Code", "WA" },
    };

    var entry = context.Entry(user);

    foreach (var propertyName in newValues.Keys)
    {
        entry.Property(propertyName).CurrentValue = newValues[propertyName];
    }
}
```  

En el ejemplo anterior propiedades complejas son accesibles mediante nombres con puntos. Para que conocer otras formas de obtener acceso a propiedades complejas, vea las dos secciones más adelante en este tema en concreto sobre las propiedades complejas.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Creación de un objeto clonado que contiene los valores de base de datos, original o actual  

Devuelve el objeto de DbPropertyValues de CurrentValues, OriginalValues, o GetDatabaseValues puede usarse para crear un clon de la entidad. Este clon contendrá los valores de propiedad del objeto DbPropertyValues usado para crearlo. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Tenga en cuenta que el objeto devuelto no es la entidad y no se realiza el seguimiento del contexto. El objeto devuelto también no tiene ninguna relación establecido en otros objetos.  

El objeto clonado puede ser útil para resolver problemas relacionados con actualizaciones simultáneas de la base de datos, especialmente cuando se está usando una interfaz de usuario que implica el enlace de datos a los objetos de un tipo determinado.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Obtener y establecer los valores originales y actuales de propiedades complejas  

El valor de todo el objeto complejo se pueden leer y definir mediante el método de la propiedad tal y como puede ser para una propiedad primitiva. Además puede profundizar en el objeto complejo y leer o establecer propiedades de ese objeto, o incluso un objeto anidado. A continuación se muestran algunos ejemplos:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    // Get the Location complex object
    var location = context.Entry(user)
                       .Property(u => u.Location)
                       .CurrentValue;

    // Get the nested State complex object using chained calls
    var state1 = context.Entry(user)
                     .ComplexProperty(u => u.Location)
                     .Property(l => l.State)
                     .CurrentValue;

    // Get the nested State complex object using a single lambda expression
    var state2 = context.Entry(user)
                     .Property(u => u.Location.State)
                     .CurrentValue;

    // Get the nested State complex object using a dotted string
    var state3 = context.Entry(user)
                     .Property("Location.State")
                     .CurrentValue;

    // Get the value of the Name property on the nested State complex object using chained calls
    var name1 = context.Entry(user)
                       .ComplexProperty(u => u.Location)
                       .ComplexProperty(l => l.State)
                       .Property(s => s.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a single lambda expression
    var name2 = context.Entry(user)
                       .Property(u => u.Location.State.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a dotted string
    var name3 = context.Entry(user)
                       .Property("Location.State.Name")
                       .CurrentValue;
}
```  

Utilice la propiedad OriginalValue en lugar de la propiedad CurrentValue a obtener o establecer un valor original.  

Tenga en cuenta que la propiedad o el método ComplexProperty puede utilizarse para tener acceso a una propiedad compleja. Sin embargo, el método ComplexProperty debe usarse si desea profundizar en el objeto complejo con propiedades adicionales o llama ComplexProperty.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Uso de DbPropertyValues para tener acceso a propiedades complejas  

Cuando use CurrentValues, OriginalValues o GetDatabaseValues obtener original actual, o valores de la base de datos de una entidad, los valores de propiedades complejas se devuelven como objetos DbPropertyValues anidados. Estos anidar objetos puede, a continuación, usarse para obtener los valores del objeto complejo. Por ejemplo, el siguiente método imprimirá los valores de todas las propiedades, incluidos los valores de las propiedades complejas y las propiedades complejas anidadas.  

``` csharp
public static void WritePropertyValues(string parentPropertyName, DbPropertyValues propertyValues)
{
    foreach (var propertyName in propertyValues.PropertyNames)
    {
        var nestedValues = propertyValues[propertyName] as DbPropertyValues;
        if (nestedValues != null)
        {
            WritePropertyValues(parentPropertyName + propertyName + ".", nestedValues);
        }
        else
        {
            Console.WriteLine("Property {0}{1} has value {2}",
                              parentPropertyName, propertyName,
                              propertyValues[propertyName]);
        }
    }
}
```  

Para imprimir todos los valores de propiedad actuales del método se llamaría similar al siguiente:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
