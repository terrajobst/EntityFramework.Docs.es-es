---
title: La API Fluent - relaciones - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
caps.latest.revision: 3
ms.openlocfilehash: 7437b395fbed07dcb8c93cd8418317611e14a60b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122256"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="d67c5-102">API de Fluent: relaciones</span><span class="sxs-lookup"><span data-stu-id="d67c5-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="d67c5-103">Esta página proporciona información sobre cómo configurar las relaciones en el modelo de Code First mediante la API fluida.</span><span class="sxs-lookup"><span data-stu-id="d67c5-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="d67c5-104">Para obtener información general acerca de las relaciones de EF y cómo obtener acceso y manipular datos mediante relaciones, vea [& Propiedades de navegación de relaciones](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="d67c5-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="d67c5-105">Cuando se trabaja con Code First, definir su modelo mediante clases CLR para su dominio.</span><span class="sxs-lookup"><span data-stu-id="d67c5-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="d67c5-106">De forma predeterminada, Entity Framework usa las convenciones de Code First para asignar las clases en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="d67c5-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="d67c5-107">Si usa las convenciones de nomenclatura de Code First, en la mayoría de los casos que puede basarse en Code First para establecer relaciones entre las tablas según las claves externas y las propiedades de navegación que se definen en las clases.</span><span class="sxs-lookup"><span data-stu-id="d67c5-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="d67c5-108">Si no sigue las convenciones al definir las clases, o si desea cambiar la forma en que funcionan las convenciones, puede usar la API fluida o las anotaciones de datos para configurar las clases para que Code First pueda asignar las relaciones entre las tablas.</span><span class="sxs-lookup"><span data-stu-id="d67c5-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="d67c5-109">Introducción</span><span class="sxs-lookup"><span data-stu-id="d67c5-109">Introduction</span></span>  

<span data-ttu-id="d67c5-110">Al configurar una relación con la API fluida, comenzar con la instancia de EntityTypeConfiguration y, a continuación, utilice el método HasRequired, HasOptional o HasMany para especificar el tipo de relación que participa esta entidad en.</span><span class="sxs-lookup"><span data-stu-id="d67c5-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="d67c5-111">Los métodos HasRequired y HasOptional toman una expresión lambda que representa una propiedad de navegación de referencia.</span><span class="sxs-lookup"><span data-stu-id="d67c5-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="d67c5-112">El método HasMany toma una expresión lambda que representa una propiedad de navegación de colección.</span><span class="sxs-lookup"><span data-stu-id="d67c5-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="d67c5-113">A continuación, puede configurar una propiedad de navegación inversa mediante los métodos WithRequired, WithOptional y WithMany.</span><span class="sxs-lookup"><span data-stu-id="d67c5-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="d67c5-114">Estos métodos tienen sobrecargas que no aceptan argumentos y pueden utilizarse para especificar la cardinalidad con navegación unidireccional.</span><span class="sxs-lookup"><span data-stu-id="d67c5-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="d67c5-115">A continuación, puede configurar propiedades de clave externa mediante el método HasForeignKey.</span><span class="sxs-lookup"><span data-stu-id="d67c5-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="d67c5-116">Este método toma una expresión lambda que representa la propiedad que se usará como la clave externa.</span><span class="sxs-lookup"><span data-stu-id="d67c5-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="d67c5-117">Configurar una relación necesaria-a-opcional (uno-a, cero o uno)</span><span class="sxs-lookup"><span data-stu-id="d67c5-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="d67c5-118">El ejemplo siguiente configura una relación de uno a cero o uno.</span><span class="sxs-lookup"><span data-stu-id="d67c5-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="d67c5-119">El OfficeAssignment tiene la propiedad InstructorID es una clave principal y una clave externa, porque el nombre de la propiedad no sigue la convención que se usa el método HasKey para configurar la clave principal.</span><span class="sxs-lookup"><span data-stu-id="d67c5-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="d67c5-120">Configurar una relación donde ambos extremos necesarios (uno a uno)</span><span class="sxs-lookup"><span data-stu-id="d67c5-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="d67c5-121">En la mayoría de los casos, Entity Framework puede deducir qué tipo es dependiente y cuál es la entidad de seguridad en una relación.</span><span class="sxs-lookup"><span data-stu-id="d67c5-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="d67c5-122">Sin embargo, cuando ambos extremos de la relación se requieren o ambos lados son opcionales Entity Framework no puede identificar el dependiente y principal.</span><span class="sxs-lookup"><span data-stu-id="d67c5-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="d67c5-123">Cuando se requieren ambos extremos de la relación, utilice WithRequiredPrincipal o WithRequiredDependent después del método HasRequired.</span><span class="sxs-lookup"><span data-stu-id="d67c5-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="d67c5-124">Cuando ambos extremos de la relación son opcionales, utilice WithOptionalPrincipal o WithOptionalDependent después del método HasOptional.</span><span class="sxs-lookup"><span data-stu-id="d67c5-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="d67c5-125">Configurar una relación varios a varios</span><span class="sxs-lookup"><span data-stu-id="d67c5-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="d67c5-126">El código siguiente configura una relación de varios a varios entre los tipos de curso e Instructor.</span><span class="sxs-lookup"><span data-stu-id="d67c5-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="d67c5-127">En el ejemplo siguiente, se utilizan las convenciones de Code First predeterminado para crear una tabla de combinación.</span><span class="sxs-lookup"><span data-stu-id="d67c5-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="d67c5-128">Como resultado se crea la tabla CourseInstructor con columnas Course_CourseID y Instructor_InstructorID.</span><span class="sxs-lookup"><span data-stu-id="d67c5-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="d67c5-129">Si desea especificar el nombre de la tabla de combinación y los nombres de las columnas de la tabla que es necesario realizar configuración adicional mediante el método de asignación.</span><span class="sxs-lookup"><span data-stu-id="d67c5-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="d67c5-130">El código siguiente genera la tabla CourseInstructor con columnas CourseID y InstructorID.</span><span class="sxs-lookup"><span data-stu-id="d67c5-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
    .Map(m =>
    {
        m.ToTable("CourseInstructor");
        m.MapLeftKey("CourseID");
        m.MapRightKey("InstructorID");
    });
```  

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="d67c5-131">Configurar una relación con una propiedad de navegación</span><span class="sxs-lookup"><span data-stu-id="d67c5-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="d67c5-132">Un unidireccional (también denominado unidireccional) relación es cuando se define una propiedad de navegación en solo uno de los extremos de la relación y no en ambos.</span><span class="sxs-lookup"><span data-stu-id="d67c5-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="d67c5-133">Por convención, Code First siempre interpreta una relación unidireccional como uno a varios.</span><span class="sxs-lookup"><span data-stu-id="d67c5-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="d67c5-134">Por ejemplo, si desea que una relación uno a uno entre Instructor y OfficeAssignment, donde tiene una propiedad de navegación en sólo el tipo Instructor, deberá usar la API fluida para configurar esta relación.</span><span class="sxs-lookup"><span data-stu-id="d67c5-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="d67c5-135">Habilitación de la eliminación en cascada</span><span class="sxs-lookup"><span data-stu-id="d67c5-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="d67c5-136">Puede configurar la eliminación en cascada en una relación con el método WillCascadeOnDelete.</span><span class="sxs-lookup"><span data-stu-id="d67c5-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="d67c5-137">Si una clave externa en la entidad dependiente no admite valores NULL, a continuación, Code First establece eliminación en cascada en la relación.</span><span class="sxs-lookup"><span data-stu-id="d67c5-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="d67c5-138">Si una clave externa en la entidad dependiente es que acepta valores NULL, Code First no establecido eliminación en cascada en la relación y, cuando se elimina la entidad de seguridad de la clave externa se establecerá en null.</span><span class="sxs-lookup"><span data-stu-id="d67c5-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="d67c5-139">Puede quitar estas convenciones de eliminación en cascada mediante:</span><span class="sxs-lookup"><span data-stu-id="d67c5-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="d67c5-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="d67c5-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="d67c5-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="d67c5-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="d67c5-142">El código siguiente configura la relación para que sea necesaria y, a continuación, deshabilita la eliminación en cascada.</span><span class="sxs-lookup"><span data-stu-id="d67c5-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="d67c5-143">Configuración de una clave externa compuesta</span><span class="sxs-lookup"><span data-stu-id="d67c5-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="d67c5-144">Si la clave principal en el tipo de departamento está formada por propiedades DepartmentID y Name, podría configurar la clave principal para el departamento y la clave externa en los tipos de curso como sigue:</span><span class="sxs-lookup"><span data-stu-id="d67c5-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

``` csharp
// Composite primary key
modelBuilder.Entity<Department>()
.HasKey(d => new { d.DepartmentID, d.Name });

// Composite foreign key
modelBuilder.Entity<Course>()  
    .HasRequired(c => c.Department)  
    .WithMany(d => d.Courses)
    .HasForeignKey(d => new { d.DepartmentID, d.DepartmentName });
```  

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="d67c5-145">Cambiar el nombre de una clave externa que no está definida en el modelo</span><span class="sxs-lookup"><span data-stu-id="d67c5-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="d67c5-146">Si decide no define una clave externa en el tipo CLR, pero desea especificar qué nombre debe tener la base de datos, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d67c5-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="d67c5-147">Configurar un nombre de clave externo que no sigue la convención de primer código</span><span class="sxs-lookup"><span data-stu-id="d67c5-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="d67c5-148">Si la propiedad de clave externa en la clase de curso se llamó a SomeDepartmentID en lugar de DepartmentID, deberá hacer lo siguiente para especificar que desea SomeDepartmentID como clave externa:</span><span class="sxs-lookup"><span data-stu-id="d67c5-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="d67c5-149">Modelo que se usa en los ejemplos</span><span class="sxs-lookup"><span data-stu-id="d67c5-149">Model Used in Samples</span></span>  

<span data-ttu-id="d67c5-150">El siguiente modelo de Code First se usa para los ejemplos en esta página.</span><span class="sxs-lookup"><span data-stu-id="d67c5-150">The following Code First model is used for the samples on this page.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

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

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  
