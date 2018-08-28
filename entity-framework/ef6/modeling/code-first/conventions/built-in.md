---
title: Convenciones de Code First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: c5fa580879a4b53fed34d94b737988875f38c62c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995531"
---
# <a name="code-first-conventions"></a><span data-ttu-id="5c056-102">Convenciones de Code First</span><span class="sxs-lookup"><span data-stu-id="5c056-102">Code First Conventions</span></span>
<span data-ttu-id="5c056-103">Código primero le permite describir un modelo mediante las clases de C# o Visual Basic. NET.</span><span class="sxs-lookup"><span data-stu-id="5c056-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="5c056-104">La forma básica del modelo se detecta mediante convenciones.</span><span class="sxs-lookup"><span data-stu-id="5c056-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="5c056-105">Las convenciones son conjuntos de reglas que se usan para configurar automáticamente un modelo conceptual basándose en las definiciones de clase cuando se trabaja con Code First.</span><span class="sxs-lookup"><span data-stu-id="5c056-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="5c056-106">Las convenciones se definen en el espacio de nombres System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="5c056-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="5c056-107">Puede configurar aún más el modelo mediante el uso de anotaciones de datos o la API fluida.</span><span class="sxs-lookup"><span data-stu-id="5c056-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="5c056-108">Configuración a través de la API fluida, seguida de las convenciones y, a continuación, las anotaciones de datos tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="5c056-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="5c056-109">Para obtener más información, consulte [anotaciones de datos](~/ef6/modeling/code-first/data-annotations.md), [API Fluent - relaciones](~/ef6/modeling/code-first/fluent/relationships.md), [API Fluent - tipos y propiedades](~/ef6/modeling/code-first/fluent/types-and-properties.md) y [API Fluent con VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="5c056-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="5c056-110">Está disponible en una lista detallada de las convenciones de Code First la [documentación de API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="5c056-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="5c056-111">En este tema se proporciona información general de las convenciones utilizadas por Code First.</span><span class="sxs-lookup"><span data-stu-id="5c056-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="5c056-112">Detección de tipos</span><span class="sxs-lookup"><span data-stu-id="5c056-112">Type Discovery</span></span>  

<span data-ttu-id="5c056-113">Cuando usando desarrollo Code First suele comenzar mediante la escritura de las clases de .NET Framework que definen el modelo conceptual (dominio).</span><span class="sxs-lookup"><span data-stu-id="5c056-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="5c056-114">Además de definir las clases, también debe permitir que **DbContext** saber los tipos que van a incluir en el modelo.</span><span class="sxs-lookup"><span data-stu-id="5c056-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="5c056-115">Para ello, defina una clase de contexto que se derive de **DbContext** y expone **DbSet** propiedades para los tipos que se desean que forme parte del modelo.</span><span class="sxs-lookup"><span data-stu-id="5c056-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="5c056-116">Código primero incluirá estos tipos y también se extrae en todos los tipos que se hace referencia, incluso si se definen los tipos que se hace referencia en un ensamblado diferente.</span><span class="sxs-lookup"><span data-stu-id="5c056-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="5c056-117">Si los tipos que participan en una jerarquía de herencia, es suficiente para definir un **DbSet** propiedad para la clase base y los tipos derivados se incluirá automáticamente, si se encuentran en el mismo ensamblado que la clase base.</span><span class="sxs-lookup"><span data-stu-id="5c056-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="5c056-118">En el ejemplo siguiente, solo hay un **DbSet** propiedad definida en el **SchoolEntities** clase (**departamentos**).</span><span class="sxs-lookup"><span data-stu-id="5c056-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="5c056-119">Código primero usa esta propiedad para detectar y extraer en todos los tipos que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="5c056-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

<span data-ttu-id="5c056-120">Si desea excluir un tipo de modelo, utilice el **NotMapped** atributo o la **DbModelBuilder.Ignore** API fluida.</span><span class="sxs-lookup"><span data-stu-id="5c056-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="5c056-121">Convención de clave principal</span><span class="sxs-lookup"><span data-stu-id="5c056-121">Primary Key Convention</span></span>  

<span data-ttu-id="5c056-122">Código infiere en primer lugar que una propiedad es una clave principal si una propiedad en una clase se denomina "ID" (no distingue mayúsculas de minúsculas) o el nombre de la clase seguido por "ID".</span><span class="sxs-lookup"><span data-stu-id="5c056-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="5c056-123">Si el tipo de la propiedad de clave principal es numérico o GUID estará configurado como una columna de identidad.</span><span class="sxs-lookup"><span data-stu-id="5c056-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="5c056-124">Convención de relación</span><span class="sxs-lookup"><span data-stu-id="5c056-124">Relationship Convention</span></span>  

<span data-ttu-id="5c056-125">En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="5c056-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="5c056-126">Cada objeto puede tener una propiedad de navegación para cada relación en la que participa.</span><span class="sxs-lookup"><span data-stu-id="5c056-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="5c056-127">Propiedades de navegación permiten navegar y administrar relaciones en ambas direcciones, devolviendo un objeto de referencia (si la multiplicidad es uno o cero o uno) o una colección (si la multiplicidad es varios).</span><span class="sxs-lookup"><span data-stu-id="5c056-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="5c056-128">Código infiere en primer lugar en función de las propiedades de navegación definidas en los tipos de relaciones.</span><span class="sxs-lookup"><span data-stu-id="5c056-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="5c056-129">Además de las propiedades de navegación, se recomienda que incluya las propiedades de clave externa en los tipos que representan los objetos dependientes.</span><span class="sxs-lookup"><span data-stu-id="5c056-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="5c056-130">Cualquier propiedad con el mismo tipo de datos como la principal propiedad de clave principal y con un nombre que se ajustan a uno de los siguientes formatos representa una clave externa para la relación: '\<nombre de la propiedad de navegación\>\<principal nombre de propiedad de clave principal\>','\<nombre de la clase de entidad de seguridad\>\<nombre de propiedad de clave principal\>', o '\<nombre de entidad de propiedad de clave principal\>'.</span><span class="sxs-lookup"><span data-stu-id="5c056-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="5c056-131">Si se encuentran varias coincidencias en el orden mostrado anteriormente tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="5c056-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="5c056-132">Detección de clave externa no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="5c056-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="5c056-133">Cuando se detecta una propiedad de clave externa, Code First deduce la multiplicidad de la relación según la nulabilidad de la clave externa.</span><span class="sxs-lookup"><span data-stu-id="5c056-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="5c056-134">Si la propiedad acepta valores NULL, a continuación, se registra la relación como opcional; en caso contrario, se registra la relación según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="5c056-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="5c056-135">Si una clave externa en la entidad dependiente no admite valores NULL, a continuación, Code First establece eliminación en cascada en la relación.</span><span class="sxs-lookup"><span data-stu-id="5c056-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="5c056-136">Si una clave externa en la entidad dependiente es que acepta valores NULL, Code First no establecido eliminación en cascada en la relación y, cuando se elimina la entidad de seguridad de la clave externa se establecerá en null.</span><span class="sxs-lookup"><span data-stu-id="5c056-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="5c056-137">La multiplicidad y cascade delete comportamiento detectado por convención puede invalidarse mediante el uso de la API fluida.</span><span class="sxs-lookup"><span data-stu-id="5c056-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="5c056-138">En el ejemplo siguiente se usan las propiedades de navegación y una clave externa para definir la relación entre las clases de departamento y curso.</span><span class="sxs-lookup"><span data-stu-id="5c056-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> <span data-ttu-id="5c056-139">Si tiene varias relaciones entre los mismos tipos (por ejemplo, supongamos que define el **persona** y **libro** clases, donde el **persona** clase contiene la  **ReviewedBooks** y **AuthoredBooks** las propiedades de navegación y la **libro** clase contiene la **autor** y  **Revisor** las propiedades de navegación) deberá configurar manualmente las relaciones mediante el uso de anotaciones de datos o la API fluida.</span><span class="sxs-lookup"><span data-stu-id="5c056-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="5c056-140">Para obtener más información, consulte [anotaciones de datos - relaciones](~/ef6/modeling/code-first/data-annotations.md) y [API Fluent - relaciones](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="5c056-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="5c056-141">Convención de tipos complejos</span><span class="sxs-lookup"><span data-stu-id="5c056-141">Complex Types Convention</span></span>  

<span data-ttu-id="5c056-142">Cuando Code First detecta una definición de clase donde no se puede deducir una clave principal y no se registra ninguna clave principal a través de las anotaciones de datos o la API fluida, el tipo se registra automáticamente como un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="5c056-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="5c056-143">Detección de tipos complejos también requiere que el tipo no tiene propiedades que hacen referencia a tipos de entidad y no se hace referencia desde una propiedad de colección en otro tipo.</span><span class="sxs-lookup"><span data-stu-id="5c056-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="5c056-144">Dadas las siguientes definiciones de clase Code First podría deducir que **detalles** es un tipo complejo, porque no tiene ninguna clave principal.</span><span class="sxs-lookup"><span data-stu-id="5c056-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

``` csharp
public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}
```  

## <a name="connection-string-convention"></a><span data-ttu-id="5c056-145">Convención de cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="5c056-145">Connection String Convention</span></span>  

<span data-ttu-id="5c056-146">Para obtener información sobre las convenciones que DbContext se usa para detectar la conexión con el uso del [modelos y las conexiones](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="5c056-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="5c056-147">Quitar las convenciones</span><span class="sxs-lookup"><span data-stu-id="5c056-147">Removing Conventions</span></span>  

<span data-ttu-id="5c056-148">Puede quitar cualquiera de las convenciones definidas en el espacio de nombres System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="5c056-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="5c056-149">En el ejemplo siguiente se quita **PluralizingTableNameConvention**.</span><span class="sxs-lookup"><span data-stu-id="5c056-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a><span data-ttu-id="5c056-150">Convenciones personalizadas</span><span class="sxs-lookup"><span data-stu-id="5c056-150">Custom Conventions</span></span>  

<span data-ttu-id="5c056-151">Convenciones personalizadas se admiten en EF6 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="5c056-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="5c056-152">Para obtener más información, consulte [convenciones de Code First personalizadas](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="5c056-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
