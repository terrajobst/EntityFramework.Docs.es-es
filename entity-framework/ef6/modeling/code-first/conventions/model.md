---
title: Convenciones basadas en modelos - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: fb79164f71cb3afff705a83f5078a13d043abca8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490940"
---
# <a name="model-based-conventions"></a>Convenciones de modelo
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.  

Convenciones de modelo basado son un método avanzado de configuración basada en convenciones del modelo. Para la mayoría de los escenarios del [personalizado convención de API de Code First en DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) debe usarse. Se recomienda una descripción de la API DbModelBuilder para las convenciones de antes de usar las convenciones de modelo que se basa.  

Convenciones de modelo en función de permiten la creación de convenciones que afectan a las propiedades y las tablas que no son configurables a través de las convenciones estándar. Ejemplos de estas son las columnas de discriminador de tabla por los modelos de la jerarquía y asociación independiente.  

## <a name="creating-a-convention"></a>Creación de convenciones   

El primer paso para crear una convención de modelo que se basa está decidiendo si cuando en la canalización debe aplicarse al modelo de la convención. Hay dos tipos de las convenciones de modelo Conceptual (espacio-c) y Store (espacio-s). Se aplica una convención de espacio de C para el modelo que se compila la aplicación, mientras que una convención de espacio de S se aplica a la versión del modelo que representa la base de datos y se denominan controles cosas como columnas generadas cómo automáticamente.  

Una convención de modelo es una clase que se extiende desde IConceptualModelConvention o IStoreModelConvention.  Estas interfaces de que ambos aceptan un tipo genérico que puede ser tipo MetadataItem que se utiliza para filtrar el tipo de datos que se aplica la convención.  

## <a name="adding-a-convention"></a>Agregar una convención   

Convenciones de modelo se agregan en la misma manera que las clases de las convenciones normales. En el **OnModelCreating** método, agregue la convención a la lista de convenciones para un modelo.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

También se puede agregar una convención en relación con otra convención con el Conventions.AddBefore\< \> o Conventions.AddAfter\< \> métodos. Para obtener más información sobre las convenciones que se aplica de Entity Framework, vea la sección Notas.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Ejemplo: Convención de modelo de discriminador  

Cambiar el nombre de las columnas generadas por EF es un ejemplo de algo que no se puede hacer con otras convenciones de la API.  Esta es una situación donde el uso de convenciones de modelo es la única opción.  

Un ejemplo de cómo usar una convención de modelo en función de para configurar columnas generadas es personalizar la forma en que se denominan columnas de discriminador.  A continuación es un ejemplo de una convención de modelo simple basado en el que se cambia el nombre de todas las columnas del modelo denominado "Discriminador" a "EntityType".  Esto incluye las columnas que el desarrollador simplemente denominado "Discriminador". Puesto que la columna "Discriminador" es una columna generada, que esto se debe ejecutar en el espacio de S.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a>Ejemplo: Cambiar el nombre de convención de IA General   

Otro ejemplo más complicado de convenciones de modelo basado en acción es configurar la forma en que se denominan asociaciones independientes (IAs).  Esta es una situación donde las convenciones de modelo son aplicables porque IAs generados por EF y no están presentes en el modelo que puede tener acceso la API DbModelBuilder.  

Cuando genera un IA EF crea una columna denominada EntityType_KeyName. Por ejemplo, para una asociación denominada a Customer con una columna de clave denominado CustomerId generaría una columna denominada Customer_CustomerId. Las siguientes bandas convención el '\_' carácter fuera el nombre de columna que se genera para la IA.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i \< properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a>Extender las convenciones de existentes   

Si tiene que escribir una convención que es similar a una de las convenciones que Entity Framework ya se aplica a su modelo que siempre puede extender esta convención para evitar tener que escribirlo desde cero.  Un ejemplo de esto es reemplazar el identificador existente que coincide con la convención con uno personalizado.   Una ventaja adicional para reemplazar la convención de clave es que se llamará el método invalidado solo si no hay ninguna clave ya se ha detectado o se haya configurado explícitamente. Una lista de las convenciones que usa Entity Framework está disponible aquí: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

A continuación, necesitamos agregar nuestra nueva convención antes de la convención de clave existente. Después de incorporar el CustomKeyDiscoveryConvention, podemos quitar el IdKeyDiscoveryConvention.  Si no Quitamos el existente IdKeyDiscoveryConvention esta convención todavía tendría prioridad sobre la convención de detección de Id. Dado que se ejecuta en primer lugar, pero en el caso donde no se encuentra ninguna propiedad "key", se ejecutará la convención "id".  Veremos este comportamiento porque cada convención ve el modelo como actualizado mediante la convención anterior (en lugar de operar en ella de forma independiente y todos los que se va a combinar entre sí) para que si, por ejemplo, una convención anterior actualizado para que coincida con un elemento de un nombre de columna interés para la convención personalizada (cuando antes de que el nombre no era de interés), a continuación, se aplicará a esa columna.  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a>Notas  

Una lista de las convenciones que se aplican actualmente por Entity Framework está disponible en la documentación de MSDN aquí: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Esta lista se extrae directamente desde el código fuente.  El código fuente de Entity Framework 6 está disponible en [GitHub](https://github.com/aspnet/entityframework6/) y muchas de las convenciones empleadas por Entity Framework son buenos puntos de partida para crear un modelo personalizado según las convenciones.  
