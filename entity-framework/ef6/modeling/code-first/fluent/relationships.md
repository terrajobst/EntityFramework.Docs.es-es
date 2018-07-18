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
# <a name="fluent-api---relationships"></a>API de Fluent: relaciones
> [!NOTE]
> Esta página proporciona información sobre cómo configurar las relaciones en el modelo de Code First mediante la API fluida. Para obtener información general acerca de las relaciones de EF y cómo obtener acceso y manipular datos mediante relaciones, vea [& Propiedades de navegación de relaciones](~/ef6/fundamentals/relationships.md).  

Cuando se trabaja con Code First, definir su modelo mediante clases CLR para su dominio. De forma predeterminada, Entity Framework usa las convenciones de Code First para asignar las clases en el esquema de base de datos. Si usa las convenciones de nomenclatura de Code First, en la mayoría de los casos que puede basarse en Code First para establecer relaciones entre las tablas según las claves externas y las propiedades de navegación que se definen en las clases. Si no sigue las convenciones al definir las clases, o si desea cambiar la forma en que funcionan las convenciones, puede usar la API fluida o las anotaciones de datos para configurar las clases para que Code First pueda asignar las relaciones entre las tablas.  

## <a name="introduction"></a>Introducción  

Al configurar una relación con la API fluida, comenzar con la instancia de EntityTypeConfiguration y, a continuación, utilice el método HasRequired, HasOptional o HasMany para especificar el tipo de relación que participa esta entidad en. Los métodos HasRequired y HasOptional toman una expresión lambda que representa una propiedad de navegación de referencia. El método HasMany toma una expresión lambda que representa una propiedad de navegación de colección. A continuación, puede configurar una propiedad de navegación inversa mediante los métodos WithRequired, WithOptional y WithMany. Estos métodos tienen sobrecargas que no aceptan argumentos y pueden utilizarse para especificar la cardinalidad con navegación unidireccional.  

A continuación, puede configurar propiedades de clave externa mediante el método HasForeignKey. Este método toma una expresión lambda que representa la propiedad que se usará como la clave externa.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Configurar una relación necesaria-a-opcional (uno-a, cero o uno)  

El ejemplo siguiente configura una relación de uno a cero o uno. El OfficeAssignment tiene la propiedad InstructorID es una clave principal y una clave externa, porque el nombre de la propiedad no sigue la convención que se usa el método HasKey para configurar la clave principal.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Configurar una relación donde ambos extremos necesarios (uno a uno)  

En la mayoría de los casos, Entity Framework puede deducir qué tipo es dependiente y cuál es la entidad de seguridad en una relación. Sin embargo, cuando ambos extremos de la relación se requieren o ambos lados son opcionales Entity Framework no puede identificar el dependiente y principal. Cuando se requieren ambos extremos de la relación, utilice WithRequiredPrincipal o WithRequiredDependent después del método HasRequired. Cuando ambos extremos de la relación son opcionales, utilice WithOptionalPrincipal o WithOptionalDependent después del método HasOptional.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Configurar una relación varios a varios  

El código siguiente configura una relación de varios a varios entre los tipos de curso e Instructor. En el ejemplo siguiente, se utilizan las convenciones de Code First predeterminado para crear una tabla de combinación. Como resultado se crea la tabla CourseInstructor con columnas Course_CourseID y Instructor_InstructorID.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Si desea especificar el nombre de la tabla de combinación y los nombres de las columnas de la tabla que es necesario realizar configuración adicional mediante el método de asignación. El código siguiente genera la tabla CourseInstructor con columnas CourseID y InstructorID.  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a>Configurar una relación con una propiedad de navegación  

Un unidireccional (también denominado unidireccional) relación es cuando se define una propiedad de navegación en solo uno de los extremos de la relación y no en ambos. Por convención, Code First siempre interpreta una relación unidireccional como uno a varios. Por ejemplo, si desea que una relación uno a uno entre Instructor y OfficeAssignment, donde tiene una propiedad de navegación en sólo el tipo Instructor, deberá usar la API fluida para configurar esta relación.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Habilitación de la eliminación en cascada  

Puede configurar la eliminación en cascada en una relación con el método WillCascadeOnDelete. Si una clave externa en la entidad dependiente no admite valores NULL, a continuación, Code First establece eliminación en cascada en la relación. Si una clave externa en la entidad dependiente es que acepta valores NULL, Code First no establecido eliminación en cascada en la relación y, cuando se elimina la entidad de seguridad de la clave externa se establecerá en null.  

Puede quitar estas convenciones de eliminación en cascada mediante:  

modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)  
modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)  

El código siguiente configura la relación para que sea necesaria y, a continuación, deshabilita la eliminación en cascada.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Configuración de una clave externa compuesta  

Si la clave principal en el tipo de departamento está formada por propiedades DepartmentID y Name, podría configurar la clave principal para el departamento y la clave externa en los tipos de curso como sigue:  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Cambiar el nombre de una clave externa que no está definida en el modelo  

Si decide no define una clave externa en el tipo CLR, pero desea especificar qué nombre debe tener la base de datos, realice lo siguiente:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Configurar un nombre de clave externo que no sigue la convención de primer código  

Si la propiedad de clave externa en la clase de curso se llamó a SomeDepartmentID en lugar de DepartmentID, deberá hacer lo siguiente para especificar que desea SomeDepartmentID como clave externa:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Modelo que se usa en los ejemplos  

El siguiente modelo de Code First se usa para los ejemplos en esta página.  

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
