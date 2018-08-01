---
title: Las relaciones, las propiedades de navegación y las claves externas - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
caps.latest.revision: 3
ms.openlocfilehash: 28dab25c7d19a117e594b1761f7bc745b684f7b3
ms.sourcegitcommit: 5c2634c546720902fe01935f4fc031d73aa3ebde
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393768"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="1b708-102">Las relaciones, las propiedades de navegación y las claves externas</span><span class="sxs-lookup"><span data-stu-id="1b708-102">Relationships, navigation properties and foreign keys</span></span>
<span data-ttu-id="1b708-103">En este tema se proporciona información general de cómo Entity Framework administra las relaciones entre entidades.</span><span class="sxs-lookup"><span data-stu-id="1b708-103">This topic gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="1b708-104">También se ofrece orientación sobre cómo asignar y manipular las relaciones.</span><span class="sxs-lookup"><span data-stu-id="1b708-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="1b708-105">Relaciones de EF</span><span class="sxs-lookup"><span data-stu-id="1b708-105">Relationships in EF</span></span>

<span data-ttu-id="1b708-106">En las bases de datos relacionales, se definen relaciones (también denominadas asociaciones) entre las tablas a través de las claves externas.</span><span class="sxs-lookup"><span data-stu-id="1b708-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="1b708-107">Una clave externa (FK) es una columna o combinación de columnas que se usa para establecer y exigir un vínculo entre los datos de dos tablas.</span><span class="sxs-lookup"><span data-stu-id="1b708-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="1b708-108">Generalmente, hay tres tipos de relaciones: uno a uno, uno a varios y varios a varios.</span><span class="sxs-lookup"><span data-stu-id="1b708-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="1b708-109">En una relación uno a varios, la clave externa se define en la tabla que representa el extremo de la relación de varios.</span><span class="sxs-lookup"><span data-stu-id="1b708-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="1b708-110">La relación de varios a varios implica definir una tercera tabla (denominada tabla de unión o de combinación), cuya clave principal se compone de las claves externas de ambas tablas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="1b708-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="1b708-111">En una relación uno a uno, la clave principal actúa además como una clave externa y no hay ninguna columna de clave externa independiente para cualquiera de las tablas.</span><span class="sxs-lookup"><span data-stu-id="1b708-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="1b708-112">La siguiente imagen muestra dos tablas que participan en la relación de uno a varios.</span><span class="sxs-lookup"><span data-stu-id="1b708-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="1b708-113">El **curso** tabla es la tabla dependiente porque contiene la **DepartmentID** columna que se vincula a la **departamento** tabla.</span><span class="sxs-lookup"><span data-stu-id="1b708-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Database2](~/ef6/media/database2.png)

<span data-ttu-id="1b708-115">En Entity Framework, una entidad puede deberse a otras entidades a través de una asociación o relación.</span><span class="sxs-lookup"><span data-stu-id="1b708-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="1b708-116">Cada relación contiene dos extremos que describen el tipo de entidad y la multiplicidad del tipo (uno, cero o uno o muchos) para las dos entidades en dicha relación.</span><span class="sxs-lookup"><span data-stu-id="1b708-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="1b708-117">La relación puede venir gobernada por una restricción referencial, que describe qué extremo de la relación es un rol principal y que es un rol dependiente.</span><span class="sxs-lookup"><span data-stu-id="1b708-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="1b708-118">Las propiedades de navegación proporcionan una manera de navegar por una asociación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="1b708-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="1b708-119">Cada objeto puede tener una propiedad de navegación para cada relación en la que participa.</span><span class="sxs-lookup"><span data-stu-id="1b708-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="1b708-120">Propiedades de navegación permiten navegar y administrar relaciones en ambas direcciones, devolviendo un objeto de referencia (si la multiplicidad es uno o cero o uno) o una colección (si la multiplicidad es varios).</span><span class="sxs-lookup"><span data-stu-id="1b708-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="1b708-121">También puede tener una navegación unidireccional, en cuyo caso se define la propiedad de navegación solo en uno de los tipos que participa en la relación y no en ambos.</span><span class="sxs-lookup"><span data-stu-id="1b708-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="1b708-122">Se recomienda incluir propiedades en el modelo que se asignan a las claves externas de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1b708-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="1b708-123">Con las propiedades de clave externa incluidas, puede crear o cambiar una relación modificando el valor de clave externa sobre un objeto dependiente.</span><span class="sxs-lookup"><span data-stu-id="1b708-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="1b708-124">Este tipo de asociación se denomina asociación de clave externa.</span><span class="sxs-lookup"><span data-stu-id="1b708-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="1b708-125">Mediante claves externas es incluso más esencial al trabajar con entidades desconectadas.</span><span class="sxs-lookup"><span data-stu-id="1b708-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="1b708-126">Tenga en cuenta que, cuando trabaja con 1 a 1 o 1 a 0.. relaciones 1, no hay ninguna columna de clave externa independiente, la propiedad de clave principal actúa como la clave externa y siempre se incluye en el modelo.</span><span class="sxs-lookup"><span data-stu-id="1b708-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="1b708-127">Cuando las columnas de clave externas no se incluyen en el modelo, la información de asociación se administra como un objeto independiente.</span><span class="sxs-lookup"><span data-stu-id="1b708-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="1b708-128">Se realiza un seguimiento de las relaciones a través de referencias de objeto en lugar de propiedades de clave externa.</span><span class="sxs-lookup"><span data-stu-id="1b708-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="1b708-129">Este tipo de asociación se denomina un *asociación independiente*.</span><span class="sxs-lookup"><span data-stu-id="1b708-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="1b708-130">La manera más común para modificar un *asociación independiente* consiste en modificar las propiedades de navegación que se generan para cada entidad que participa en la asociación.</span><span class="sxs-lookup"><span data-stu-id="1b708-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="1b708-131">Puede decidir entre utilizar uno o ambos tipos de asociaciones en su modelo.</span><span class="sxs-lookup"><span data-stu-id="1b708-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="1b708-132">Sin embargo, si tiene una relación muchos a muchos pura que está conectada mediante una tabla de combinación que solo contiene claves externas, EF utilizará una asociación independiente para administrar este tipo de relación muchos a muchos.</span><span class="sxs-lookup"><span data-stu-id="1b708-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="1b708-133">La siguiente imagen muestra un modelo conceptual que se creó con Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="1b708-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="1b708-134">El modelo contiene dos entidades que participan en la relación de uno a varios.</span><span class="sxs-lookup"><span data-stu-id="1b708-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="1b708-135">Ambas entidades tienen propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="1b708-135">Both entities have navigation properties.</span></span> <span data-ttu-id="1b708-136">**Curso** es la entidad de dependencia y tiene la **DepartmentID** propiedad de clave externa definida.</span><span class="sxs-lookup"><span data-stu-id="1b708-136">**Course** is the depend entity and has the **DepartmentID** foreign key property defined.</span></span>

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="1b708-138">El fragmento de código siguiente muestra el mismo modelo que se creó con Code First.</span><span class="sxs-lookup"><span data-stu-id="1b708-138">The following code snippet shows the same model that was created with Code First.</span></span>

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="1b708-139">Configurar o asignación de relaciones</span><span class="sxs-lookup"><span data-stu-id="1b708-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="1b708-140">El resto de esta página cubre cómo obtener acceso y manipular datos mediante relaciones.</span><span class="sxs-lookup"><span data-stu-id="1b708-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="1b708-141">Para obtener información acerca de cómo configurar las relaciones en el modelo, vea las páginas siguientes.</span><span class="sxs-lookup"><span data-stu-id="1b708-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="1b708-142">Para configurar las relaciones en Code First, consulte [anotaciones de datos](~/ef6/modeling/code-first/data-annotations.md) y [API Fluent-relaciones](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="1b708-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="1b708-143">Para configurar las relaciones mediante Entity Framework Designer, vea [relaciones con EF Designer](~/ef6/modeling/designer/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="1b708-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="1b708-144">Crear y modificar relaciones</span><span class="sxs-lookup"><span data-stu-id="1b708-144">Creating and modifying relationships</span></span>

<span data-ttu-id="1b708-145">En un *asociación de clave externa*, al cambiar la relación, el estado de un objeto dependiente con un EntityState.Unchanged estado cambia a EntityState.Modified.</span><span class="sxs-lookup"><span data-stu-id="1b708-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an EntityState.Unchanged state changes to EntityState.Modified.</span></span> <span data-ttu-id="1b708-146">En una relación independiente, al cambiar la relación no actualiza el estado del objeto dependiente.</span><span class="sxs-lookup"><span data-stu-id="1b708-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="1b708-147">Los ejemplos siguientes muestran cómo usar las propiedades de clave externa y las propiedades de navegación para asociar los objetos relacionados.</span><span class="sxs-lookup"><span data-stu-id="1b708-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="1b708-148">Con asociaciones de clave externa, puede usar cualquiera de estos métodos para cambiar, crear o modificar las relaciones.</span><span class="sxs-lookup"><span data-stu-id="1b708-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="1b708-149">Con asociaciones independientes, no puede utilizar la propiedad de clave externa.</span><span class="sxs-lookup"><span data-stu-id="1b708-149">With independent associations, you cannot use the foreign key property.</span></span>

-   <span data-ttu-id="1b708-150">Asignando un nuevo valor a una propiedad de clave externa, como en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="1b708-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
    ``` csharp
    course.DepartmentID = newCourse.DepartmentID;
    ```

-   <span data-ttu-id="1b708-151">El código siguiente quita una relación estableciendo la clave externa en **null**.</span><span class="sxs-lookup"><span data-stu-id="1b708-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="1b708-152">Tenga en cuenta que la propiedad de clave externa deben aceptar valores NULL.</span><span class="sxs-lookup"><span data-stu-id="1b708-152">Note, that the foreign key property must be nullable.</span></span>  
    ``` csharp
    course.DepartmentID = null;
    ```  
    >[!NOTE]
    > <span data-ttu-id="1b708-153">Si la referencia está en el estado agregado (en este ejemplo, el objeto de curso), la propiedad de navegación de referencia no se sincronizarán con los valores de clave de un nuevo objeto hasta que se llama a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="1b708-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="1b708-154">La sincronización no se produce porque el contexto del objeto no contiene claves permanentes para objetos agregados hasta que se guardan.</span><span class="sxs-lookup"><span data-stu-id="1b708-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="1b708-155">Si debe tener los nuevos objetos totalmente sincronizados tan pronto como establecer la relación, utilice uno de los siguientes methods.\*</span><span class="sxs-lookup"><span data-stu-id="1b708-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

-   <span data-ttu-id="1b708-156">Asignando un nuevo objeto a una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="1b708-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="1b708-157">El código siguiente crea una relación entre un curso y un `department`.</span><span class="sxs-lookup"><span data-stu-id="1b708-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="1b708-158">Si los objetos se asocian al contexto, el `course` también se agrega a la `department.Courses` colección y la clave externa correspondiente propiedad en el `course` objeto se establece en el valor de propiedad de clave del departamento.</span><span class="sxs-lookup"><span data-stu-id="1b708-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
    ``` csharp
    course.Department = department;
    ```

 -   <span data-ttu-id="1b708-159">Para eliminar la relación, establezca la propiedad de navegación en `null`.</span><span class="sxs-lookup"><span data-stu-id="1b708-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="1b708-160">Si trabaja con Entity Framework que se basa en .NET 4.0, el extremo relacionado debe cargarse antes de que se establece en null.</span><span class="sxs-lookup"><span data-stu-id="1b708-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="1b708-161">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1b708-161">For example:</span></span>  
    ``` chsarp
    context.Entry(course).Reference(c => c.Department).Load();  
    course.Department = null;
    ```  
    <span data-ttu-id="1b708-162">A partir de Entity Framework 5.0, que se basa en .NET 4.5, puede establecer la relación en null sin tener que cargar el extremo relacionado.</span><span class="sxs-lookup"><span data-stu-id="1b708-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="1b708-163">También puede establecer el valor actual en null mediante el método siguiente.</span><span class="sxs-lookup"><span data-stu-id="1b708-163">You can also set the current value to null using the following method.</span></span>  
    ``` csharp
    context.Entry(course).Reference(c => c.Department).CurrentValue = null;
    ```

-   <span data-ttu-id="1b708-164">Eliminando o agregando un objeto en una colección de entidades.</span><span class="sxs-lookup"><span data-stu-id="1b708-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="1b708-165">Por ejemplo, puede agregar un objeto de tipo `Course` a la `department.Courses` colección.</span><span class="sxs-lookup"><span data-stu-id="1b708-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="1b708-166">Esta operación crea una relación entre un determinado **curso** y un determinado `department`.</span><span class="sxs-lookup"><span data-stu-id="1b708-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="1b708-167">Si los objetos se asocian para el contexto, la referencia de departamento y la propiedad de clave externa en el **curso** objeto se establecerá en adecuado `department`.</span><span class="sxs-lookup"><span data-stu-id="1b708-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
    ``` csharp
    department.Courses.Add(newCourse);
    ```

- <span data-ttu-id="1b708-168">Mediante el uso de la `ChangeRelationshipState` método para cambiar el estado de la relación entre dos objetos entidad especificado.</span><span class="sxs-lookup"><span data-stu-id="1b708-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="1b708-169">Este método se usa con más frecuencia cuando se trabaja con aplicaciones de N niveles y una *asociación independiente* (no se puede usar con una asociación de clave externa).</span><span class="sxs-lookup"><span data-stu-id="1b708-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="1b708-170">Además, para usar este método debe quitar hasta `ObjectContext`, tal y como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="1b708-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="1b708-171">En el ejemplo siguiente, hay una relación de varios a varios entre los instructores y a los cursos.</span><span class="sxs-lookup"><span data-stu-id="1b708-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="1b708-172">Una llamada a la `ChangeRelationshipState` método y pasando el `EntityState.Added` parámetro, permite la `SchoolContext` saber que se ha agregado una relación entre los dos objetos:</span><span class="sxs-lookup"><span data-stu-id="1b708-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>

``` csharp

       ((IObjectContextAdapter)context).ObjectContext.
                 ObjectStateManager.
                  ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
```

    Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:

``` csharp
       ((IObjectContextAdapter)context).ObjectContext.
                  ObjectStateManager.
                  ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="1b708-173">Sincronizar los cambios entre las propiedades de navegación y las claves externas</span><span class="sxs-lookup"><span data-stu-id="1b708-173">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="1b708-174">Al cambiar la relación de los objetos asociados al contexto mediante uno de los métodos descritos anteriormente, Entity Framework necesita mantener sincronizadas las claves externas, referencias y colecciones. Entity Framework administra automáticamente esta sincronización (también conocido como relación corrección-arriba) para las entidades POCO con servidores proxy.</span><span class="sxs-lookup"><span data-stu-id="1b708-174">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="1b708-175">Para obtener más información, consulte [trabajar con servidores proxy](~/ef6/fundamentals/proxies.md).</span><span class="sxs-lookup"><span data-stu-id="1b708-175">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="1b708-176">Si está utilizando entidades POCO sin objetos proxy, debe asegurarse que la **DetectChanges** método se llama para sincronizar los objetos relacionados en el contexto.</span><span class="sxs-lookup"><span data-stu-id="1b708-176">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="1b708-177">Tenga en cuenta que las siguientes API desencadenan automáticamente un **DetectChanges** llamar.</span><span class="sxs-lookup"><span data-stu-id="1b708-177">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   <span data-ttu-id="1b708-178">Ejecutar un LINQ a consultas en un `DbSet`</span><span class="sxs-lookup"><span data-stu-id="1b708-178">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="1b708-179">Cargar objetos relacionados</span><span class="sxs-lookup"><span data-stu-id="1b708-179">Loading related objects</span></span>

<span data-ttu-id="1b708-180">En Entity Framework que usa con más frecuencia, utilice las propiedades de navegación para cargar entidades relacionadas con la entidad devuelta por la asociación definida.</span><span class="sxs-lookup"><span data-stu-id="1b708-180">In Entity Framework you use most commonly use the navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="1b708-181">Para obtener más información, consulte [cargar objetos relacionados](~/ef6/querying/related-data.md).</span><span class="sxs-lookup"><span data-stu-id="1b708-181">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1b708-182">En una asociación de clave externa, al cargar un extremo relacionado de un objeto dependiente, el objeto relacionado se cargará dependiendo del valor de clave externa del objeto dependiente actualmente en memoria:</span><span class="sxs-lookup"><span data-stu-id="1b708-182">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="1b708-183">En una asociación independiente, se consulta el extremo relacionado de un objeto dependiente de acuerdo con el valor de clave externa actualmente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1b708-183">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="1b708-184">Sin embargo, si se modificó la relación y la propiedad de referencia en el objeto dependiente señala a un objeto de entidad de seguridad diferente que se carga en el contexto del objeto, Entity Framework intentará crear una relación como se define en el cliente.</span><span class="sxs-lookup"><span data-stu-id="1b708-184">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="1b708-185">Administrar la simultaneidad</span><span class="sxs-lookup"><span data-stu-id="1b708-185">Managing concurrency</span></span>

<span data-ttu-id="1b708-186">En la clave externa y asociaciones independientes, las comprobaciones de simultaneidad se basan en las claves de entidad y otras propiedades de entidad que se definen en el modelo.</span><span class="sxs-lookup"><span data-stu-id="1b708-186">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="1b708-187">Cuando se usa EF Designer para crear un modelo, establecer el `ConcurrencyMode` atributo **fijo** para especificar que se debe comprobar la propiedad de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="1b708-187">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="1b708-188">Cuando utilice Code First para definir un modelo, use el `ConcurrencyCheck` anotación en las propiedades que desea que se comprobará para simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="1b708-188">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="1b708-189">Cuando se trabaja con Code First también puede usar el `TimeStamp` anotación para especificar que se debe comprobar la propiedad de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="1b708-189">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="1b708-190">Puede tener solo una propiedad de marca de tiempo en una clase determinada.</span><span class="sxs-lookup"><span data-stu-id="1b708-190">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="1b708-191">Code First asigna esta propiedad a un campo que no aceptan valores NULL en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1b708-191">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="1b708-192">Se recomienda usar siempre la asociación de clave externa cuando se trabaja con entidades que participan en la comprobación de simultaneidad y la resolución.</span><span class="sxs-lookup"><span data-stu-id="1b708-192">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="1b708-193">Para obtener más información, consulte [controlar los conflictos de simultaneidad](~/ef6/saving/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="1b708-193">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="1b708-194">Trabajar con claves superpuestas</span><span class="sxs-lookup"><span data-stu-id="1b708-194">Working with overlapping Keys</span></span>

<span data-ttu-id="1b708-195">Las claves superpuestas son claves compuestas en las que algunas propiedades de la clave también forman parte de otra clave de la entidad.</span><span class="sxs-lookup"><span data-stu-id="1b708-195">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="1b708-196">No es posible tener una clave superpuesta en una asociación independiente.</span><span class="sxs-lookup"><span data-stu-id="1b708-196">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="1b708-197">Para cambiar una asociación de clave externa que incluya claves superpuestas, recomendamos modificar los valores de clave externa en lugar de utilizar las referencias a objetos.</span><span class="sxs-lookup"><span data-stu-id="1b708-197">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
