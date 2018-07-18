---
title: Convenciones basadas en modelos - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
caps.latest.revision: 3
ms.openlocfilehash: 135a51d93a06c0d64732438f067df4ce2675fbe2
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122766"
---
# <a name="model-based-conventions"></a><span data-ttu-id="38665-102">Convenciones de modelo</span><span class="sxs-lookup"><span data-stu-id="38665-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="38665-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="38665-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="38665-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="38665-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="38665-105">Convenciones de modelo basado son un método avanzado de configuración basada en convenciones del modelo.</span><span class="sxs-lookup"><span data-stu-id="38665-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="38665-106">Para la mayoría de los escenarios del [personalizado convención de API de Code First en DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) debe usarse.</span><span class="sxs-lookup"><span data-stu-id="38665-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="38665-107">Se recomienda una descripción de la API DbModelBuilder para las convenciones de antes de usar las convenciones de modelo que se basa.</span><span class="sxs-lookup"><span data-stu-id="38665-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="38665-108">Convenciones de modelo en función de permiten la creación de convenciones que afectan a las propiedades y las tablas que no son configurables a través de las convenciones estándar.</span><span class="sxs-lookup"><span data-stu-id="38665-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="38665-109">Ejemplos de estas son las columnas de discriminador de tabla por los modelos de la jerarquía y asociación independiente.</span><span class="sxs-lookup"><span data-stu-id="38665-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="38665-110">Creación de convenciones</span><span class="sxs-lookup"><span data-stu-id="38665-110">Creating a Convention</span></span>   

<span data-ttu-id="38665-111">El primer paso para crear una convención de modelo que se basa está decidiendo si cuando en la canalización debe aplicarse al modelo de la convención.</span><span class="sxs-lookup"><span data-stu-id="38665-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="38665-112">Hay dos tipos de las convenciones de modelo Conceptual (espacio-c) y Store (espacio-s).</span><span class="sxs-lookup"><span data-stu-id="38665-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="38665-113">Se aplica una convención de espacio de C para el modelo que se compila la aplicación, mientras que una convención de espacio de S se aplica a la versión del modelo que representa la base de datos y se denominan controles cosas como columnas generadas cómo automáticamente.</span><span class="sxs-lookup"><span data-stu-id="38665-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="38665-114">Una convención de modelo es una clase que se extiende desde IConceptualModelConvention o IStoreModelConvention.</span><span class="sxs-lookup"><span data-stu-id="38665-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="38665-115">Estas interfaces de que ambos aceptan un tipo genérico que puede ser tipo MetadataItem que se utiliza para filtrar el tipo de datos que se aplica la convención.</span><span class="sxs-lookup"><span data-stu-id="38665-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="38665-116">Agregar una convención</span><span class="sxs-lookup"><span data-stu-id="38665-116">Adding a Convention</span></span>   

<span data-ttu-id="38665-117">Convenciones de modelo se agregan en la misma manera que las clases de las convenciones normales.</span><span class="sxs-lookup"><span data-stu-id="38665-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="38665-118">En el **OnModelCreating** método, agregue la convención a la lista de convenciones para un modelo.</span><span class="sxs-lookup"><span data-stu-id="38665-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

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

<span data-ttu-id="38665-119">También se puede agregar una convención en relación con otra convención con el Conventions.AddBefore\< \> o Conventions.AddAfter\< \> métodos.</span><span class="sxs-lookup"><span data-stu-id="38665-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="38665-120">Para obtener más información sobre las convenciones que se aplica de Entity Framework, vea la sección Notas.</span><span class="sxs-lookup"><span data-stu-id="38665-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="38665-121">Ejemplo: Convención de modelo de discriminador</span><span class="sxs-lookup"><span data-stu-id="38665-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="38665-122">Cambiar el nombre de las columnas generadas por EF es un ejemplo de algo que no se puede hacer con otras convenciones de la API.</span><span class="sxs-lookup"><span data-stu-id="38665-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="38665-123">Esta es una situación donde el uso de convenciones de modelo es la única opción.</span><span class="sxs-lookup"><span data-stu-id="38665-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="38665-124">Un ejemplo de cómo usar una convención de modelo en función de para configurar columnas generadas es personalizar la forma en que se denominan columnas de discriminador.</span><span class="sxs-lookup"><span data-stu-id="38665-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="38665-125">A continuación es un ejemplo de una convención de modelo simple basado en el que se cambia el nombre de todas las columnas del modelo denominado "Discriminador" a "EntityType".</span><span class="sxs-lookup"><span data-stu-id="38665-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="38665-126">Esto incluye las columnas que el desarrollador simplemente denominado "Discriminador".</span><span class="sxs-lookup"><span data-stu-id="38665-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="38665-127">Puesto que la columna "Discriminador" es una columna generada, que esto se debe ejecutar en el espacio de S.</span><span class="sxs-lookup"><span data-stu-id="38665-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

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

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="38665-128">Ejemplo: Cambiar el nombre de convención de IA General</span><span class="sxs-lookup"><span data-stu-id="38665-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="38665-129">Otro ejemplo más complicado de convenciones de modelo basado en acción es configurar la forma en que se denominan asociaciones independientes (IAs).</span><span class="sxs-lookup"><span data-stu-id="38665-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="38665-130">Esta es una situación donde las convenciones de modelo son aplicables porque IAs generados por EF y no están presentes en el modelo que puede tener acceso la API DbModelBuilder.</span><span class="sxs-lookup"><span data-stu-id="38665-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="38665-131">Cuando genera un IA EF crea una columna denominada EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="38665-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="38665-132">Por ejemplo, para una asociación denominada a Customer con una columna de clave denominado CustomerId generaría una columna denominada Customer_CustomerId.</span><span class="sxs-lookup"><span data-stu-id="38665-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="38665-133">Las siguientes bandas convención el '\_' carácter fuera el nombre de columna que se genera para la IA.</span><span class="sxs-lookup"><span data-stu-id="38665-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

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

## <a name="extending-existing-conventions"></a><span data-ttu-id="38665-134">Extender las convenciones de existentes</span><span class="sxs-lookup"><span data-stu-id="38665-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="38665-135">Si tiene que escribir una convención que es similar a una de las convenciones que Entity Framework ya se aplica a su modelo que siempre puede extender esta convención para evitar tener que escribirlo desde cero.</span><span class="sxs-lookup"><span data-stu-id="38665-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="38665-136">Un ejemplo de esto es reemplazar el identificador existente que coincide con la convención con uno personalizado.</span><span class="sxs-lookup"><span data-stu-id="38665-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="38665-137">Una ventaja adicional para reemplazar la convención de clave es que se llamará el método invalidado solo si no hay ninguna clave ya se ha detectado o se haya configurado explícitamente.</span><span class="sxs-lookup"><span data-stu-id="38665-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="38665-138">Una lista de las convenciones que usa Entity Framework está disponible aquí: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="38665-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

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

<span data-ttu-id="38665-139">A continuación, necesitamos agregar nuestra nueva convención antes de la convención de clave existente.</span><span class="sxs-lookup"><span data-stu-id="38665-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="38665-140">Después de incorporar el CustomKeyDiscoveryConvention, podemos quitar el IdKeyDiscoveryConvention.</span><span class="sxs-lookup"><span data-stu-id="38665-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="38665-141">Si no Quitamos el existente IdKeyDiscoveryConvention esta convención todavía tendría prioridad sobre la convención de detección de Id. Dado que se ejecuta en primer lugar, pero en el caso donde no se encuentra ninguna propiedad "key", se ejecutará la convención "id".</span><span class="sxs-lookup"><span data-stu-id="38665-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="38665-142">Veremos este comportamiento porque cada convención ve el modelo como actualizado mediante la convención anterior (en lugar de operar en ella de forma independiente y todos los que se va a combinar entre sí) para que si, por ejemplo, una convención anterior actualizado para que coincida con un elemento de un nombre de columna interés para la convención personalizada (cuando antes de que el nombre no era de interés), a continuación, se aplicará a esa columna.</span><span class="sxs-lookup"><span data-stu-id="38665-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

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

## <a name="notes"></a><span data-ttu-id="38665-143">Notas</span><span class="sxs-lookup"><span data-stu-id="38665-143">Notes</span></span>  

<span data-ttu-id="38665-144">Una lista de las convenciones que se aplican actualmente por Entity Framework está disponible en la documentación de MSDN aquí: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="38665-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="38665-145">Esta lista se extrae directamente desde el código fuente.</span><span class="sxs-lookup"><span data-stu-id="38665-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="38665-146">El código fuente de Entity Framework 6 está disponible en [GitHub](https://github.com/aspnet/entityframework6/) y muchas de las convenciones empleadas por Entity Framework son buenos puntos de partida para crear un modelo personalizado según las convenciones.</span><span class="sxs-lookup"><span data-stu-id="38665-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
