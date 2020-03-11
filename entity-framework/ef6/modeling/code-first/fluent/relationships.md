---
title: API fluida-Relationships-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415764"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="c1f12-102">API fluidas: relaciones</span><span class="sxs-lookup"><span data-stu-id="c1f12-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="c1f12-103">En esta página se proporciona información sobre cómo configurar las relaciones en el modelo de Code First mediante la API fluida.</span><span class="sxs-lookup"><span data-stu-id="c1f12-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="c1f12-104">Para obtener información general sobre las relaciones en EF y cómo obtener acceso a los datos y manipularlos mediante relaciones, vea [relaciones & propiedades de navegación](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="c1f12-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="c1f12-105">Cuando se trabaja con Code First, se define el modelo definiendo las clases CLR del dominio.</span><span class="sxs-lookup"><span data-stu-id="c1f12-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="c1f12-106">De forma predeterminada, Entity Framework utiliza las convenciones de Code First para asignar las clases al esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c1f12-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="c1f12-107">Si utiliza las convenciones de nomenclatura de Code First, en la mayoría de los casos puede confiar en Code First para configurar las relaciones entre las tablas en función de las claves externas y las propiedades de navegación que defina en las clases.</span><span class="sxs-lookup"><span data-stu-id="c1f12-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="c1f12-108">Si no sigue las convenciones al definir las clases, o si desea cambiar la forma en que funcionan las convenciones, puede usar la API fluida o las anotaciones de datos para configurar las clases de modo que Code First pueda asignar las relaciones entre las tablas.</span><span class="sxs-lookup"><span data-stu-id="c1f12-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="c1f12-109">Introducción</span><span class="sxs-lookup"><span data-stu-id="c1f12-109">Introduction</span></span>  

<span data-ttu-id="c1f12-110">Al configurar una relación con la API fluida, empiece con la instancia de EntityTypeConfiguration y, a continuación, use el método HasRequired, HasOptional o HasMany para especificar el tipo de relación en la que participa esta entidad.</span><span class="sxs-lookup"><span data-stu-id="c1f12-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="c1f12-111">Los métodos HasRequired y HasOptional toman una expresión lambda que representa una propiedad de navegación de referencia.</span><span class="sxs-lookup"><span data-stu-id="c1f12-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="c1f12-112">El método HasMany toma una expresión lambda que representa una propiedad de navegación de colección.</span><span class="sxs-lookup"><span data-stu-id="c1f12-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="c1f12-113">Después, puede configurar una propiedad de navegación inversa mediante los métodos WithRequired, WithOptional y WithMany.</span><span class="sxs-lookup"><span data-stu-id="c1f12-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="c1f12-114">Estos métodos tienen sobrecargas que no toman argumentos y se pueden usar para especificar la cardinalidad con navegaciones unidireccionales.</span><span class="sxs-lookup"><span data-stu-id="c1f12-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="c1f12-115">Después, puede configurar las propiedades de clave externa mediante el método HasForeignKey.</span><span class="sxs-lookup"><span data-stu-id="c1f12-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="c1f12-116">Este método toma una expresión lambda que representa la propiedad que se va a usar como clave externa.</span><span class="sxs-lookup"><span data-stu-id="c1f12-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="c1f12-117">Configuración de una relación requerida-to-Optional (uno a cero o uno)</span><span class="sxs-lookup"><span data-stu-id="c1f12-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="c1f12-118">En el ejemplo siguiente se configura una relación de uno a cero o uno.</span><span class="sxs-lookup"><span data-stu-id="c1f12-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="c1f12-119">OfficeAssignment tiene la propiedad InstructorID que es una clave principal y una clave externa, ya que el nombre de la propiedad no sigue la Convención que el método Haskey (utiliza para configurar la clave principal.</span><span class="sxs-lookup"><span data-stu-id="c1f12-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="c1f12-120">Configurar una relación en la que se requieren ambos extremos (uno a uno)</span><span class="sxs-lookup"><span data-stu-id="c1f12-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="c1f12-121">En la mayoría de los casos Entity Framework puede deducir qué tipo es el dependiente y cuál es la entidad de seguridad de una relación.</span><span class="sxs-lookup"><span data-stu-id="c1f12-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="c1f12-122">Sin embargo, cuando se requieren ambos extremos de la relación o ambos lados son opcionales Entity Framework no puede identificar el dependiente y el principal.</span><span class="sxs-lookup"><span data-stu-id="c1f12-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="c1f12-123">Cuando se requieran ambos extremos de la relación, use WithRequiredPrincipal o WithRequiredDependent después del método HasRequired.</span><span class="sxs-lookup"><span data-stu-id="c1f12-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="c1f12-124">Cuando ambos extremos de la relación sean opcionales, use WithOptionalPrincipal o WithOptionalDependent después del método HasOptional.</span><span class="sxs-lookup"><span data-stu-id="c1f12-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="c1f12-125">Configuración de una relación de varios a varios</span><span class="sxs-lookup"><span data-stu-id="c1f12-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="c1f12-126">El siguiente código configura una relación de varios a varios entre los tipos Course y instructor.</span><span class="sxs-lookup"><span data-stu-id="c1f12-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="c1f12-127">En el ejemplo siguiente, se usan las convenciones de Code First predeterminadas para crear una tabla de combinación.</span><span class="sxs-lookup"><span data-stu-id="c1f12-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="c1f12-128">Como resultado, la tabla CourseInstructor se crea con Course_CourseID y Instructor_InstructorID columnas.</span><span class="sxs-lookup"><span data-stu-id="c1f12-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="c1f12-129">Si desea especificar el nombre de la tabla de combinación y los nombres de las columnas de la tabla, debe realizar una configuración adicional mediante el método map.</span><span class="sxs-lookup"><span data-stu-id="c1f12-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="c1f12-130">El código siguiente genera la tabla CourseInstructor con las columnas CourseID y InstructorID.</span><span class="sxs-lookup"><span data-stu-id="c1f12-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="c1f12-131">Configurar una relación con una propiedad de navegación</span><span class="sxs-lookup"><span data-stu-id="c1f12-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="c1f12-132">Una relación unidireccional (también denominada unidireccional) es cuando se define una propiedad de navegación solo en uno de los extremos de la relación y no en ambos.</span><span class="sxs-lookup"><span data-stu-id="c1f12-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="c1f12-133">Por Convención, Code First siempre interpreta una relación unidireccional como uno a varios.</span><span class="sxs-lookup"><span data-stu-id="c1f12-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="c1f12-134">Por ejemplo, si desea una relación de uno a uno entre instructor y OfficeAssignment, donde tiene una propiedad de navegación solo en el tipo de instructor, debe usar la API fluida para configurar esta relación.</span><span class="sxs-lookup"><span data-stu-id="c1f12-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="c1f12-135">Habilitar la eliminación en cascada</span><span class="sxs-lookup"><span data-stu-id="c1f12-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="c1f12-136">Puede configurar la eliminación en cascada en una relación mediante el método WillCascadeOnDelete.</span><span class="sxs-lookup"><span data-stu-id="c1f12-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="c1f12-137">Si una clave externa de la entidad dependiente no admite valores NULL, Code First establece Cascade delete en la relación.</span><span class="sxs-lookup"><span data-stu-id="c1f12-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="c1f12-138">Si una clave externa de la entidad dependiente admite valores NULL, Code First no establece Cascade delete en la relación y, cuando se elimina la entidad de seguridad, la clave externa se establecerá en NULL.</span><span class="sxs-lookup"><span data-stu-id="c1f12-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="c1f12-139">Puede quitar estas convenciones de eliminación en cascada mediante:</span><span class="sxs-lookup"><span data-stu-id="c1f12-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="c1f12-140">modelBuilder. Conventions. Remove\<OneToManyCascadeDeleteConvention\>()</span><span class="sxs-lookup"><span data-stu-id="c1f12-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="c1f12-141">modelBuilder. Conventions. Remove\<ManyToManyCascadeDeleteConvention\>()</span><span class="sxs-lookup"><span data-stu-id="c1f12-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="c1f12-142">El código siguiente configura la relación para que sea necesaria y, a continuación, deshabilita la eliminación en cascada.</span><span class="sxs-lookup"><span data-stu-id="c1f12-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="c1f12-143">Configurar una clave externa compuesta</span><span class="sxs-lookup"><span data-stu-id="c1f12-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="c1f12-144">Si la clave principal del tipo de departamento consta de las propiedades DepartmentID y Name, debe configurar la clave principal para el Departamento y la clave externa en los tipos de curso como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="c1f12-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="c1f12-145">Cambiar el nombre de una clave externa que no está definida en el modelo</span><span class="sxs-lookup"><span data-stu-id="c1f12-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="c1f12-146">Si decide no definir una clave externa en el tipo CLR, pero desea especificar el nombre que debe tener en la base de datos, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c1f12-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="c1f12-147">Configuración de un nombre de clave externa que no sigue la Convención de Code First</span><span class="sxs-lookup"><span data-stu-id="c1f12-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="c1f12-148">Si la propiedad de clave externa de la clase Course se llama SomeDepartmentID en lugar de DepartmentID, deberá hacer lo siguiente para especificar que SomeDepartmentID será la clave externa:</span><span class="sxs-lookup"><span data-stu-id="c1f12-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="c1f12-149">Modelo usado en los ejemplos</span><span class="sxs-lookup"><span data-stu-id="c1f12-149">Model Used in Samples</span></span>  

<span data-ttu-id="c1f12-150">El siguiente modelo de Code First se usa para los ejemplos de esta página.</span><span class="sxs-lookup"><span data-stu-id="c1f12-150">The following Code First model is used for the samples on this page.</span></span>  

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
