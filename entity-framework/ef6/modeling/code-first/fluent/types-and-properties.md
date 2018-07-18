---
title: 'EF6 API Fluent: configuración y asignación de tipos y propiedades:'
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
caps.latest.revision: 3
ms.openlocfilehash: ec8b484433d13899a88f44e37823dd1a4bed6530
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122615"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>API de Fluent: configuración y asignación de tipos y propiedades
Cuando se trabaja con Entity Framework Code First es el comportamiento predeterminado asignar las clases POCO a tablas mediante un conjunto de convenciones incorporada en EF. A veces, sin embargo, no puede ni no desea seguir estas convenciones y deba asignar entidades a un valor distinto de lo que dictan las convenciones.  

Hay dos formas principales de EF para usar un valor distinto de las convenciones, es decir, puede configurar [anotaciones](~/ef6/modeling/code-first/data-annotations.md) o API fluida de EFs. Las anotaciones abarcan solo un subconjunto de la funcionalidad de la API fluida, por lo que hay escenarios de asignación que no se puede lograr mediante anotaciones. En este artículo está diseñado para demostrar cómo usar la API fluida para configurar las propiedades.  

Normalmente se tiene acceso a la API fluida de code first invalidando el [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) método en su derivada [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Los ejemplos siguientes están diseñados para mostrar cómo realizar diversas tareas con la api fluida y le permiten copiar el código y personalizarla para adaptarla a su modelo, si desea ver el modelo que puede utilizarse con como-es, a continuación, se proporciona al final de este artículo.  

## <a name="model-wide-settings"></a>Configuración de todo el modelo  

### <a name="default-schema-ef6-onwards"></a>Esquema predeterminado (EF6 y versiones posteriores)  

A partir de con EF6, puede usar el método HasDefaultSchema en DbModelBuilder para especificar el esquema de base de datos que se usará para todas las tablas, procedimientos almacenados, etcetera. Para los objetos que se configure explícitamente un esquema diferente para se invalidará esta configuración predeterminada.  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a>Convenciones personalizadas (EF6 y versiones posteriores)  

A partir de EF6, puede crear sus propias convenciones para complementar los que se incluyen en Code First. Para obtener más información, consulte [convenciones de Code First personalizadas](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Asignación de propiedades  

El [propiedad](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) método se utiliza para configurar los atributos para cada propiedad que pertenecen a una entidad o tipo complejo. El método de propiedad se utiliza para obtener un objeto de configuración para una propiedad determinada. Las opciones en el objeto de configuración son específicas del tipo que se va a configurar; IsUnicode solo está disponible en las propiedades de cadena por ejemplo.  

### <a name="configuring-a-primary-key"></a>Configuración de una clave principal  

Es la convención de Entity Framework para las claves principales:  

1. Su clase define una propiedad cuyo nombre es "ID" o "ID".  
2. o un nombre de clase seguido de "ID" o "ID".  

Para establecer explícitamente una propiedad puede ser una clave principal, puede usar el método HasKey. En el ejemplo siguiente, se usa el método HasKey para configurar la clave principal InstructorID en el tipo OfficeAssignment.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Configuración de una clave principal compuesta  

El ejemplo siguiente configura las propiedades DepartmentID y Name para que sea la clave principal compuesta del tipo de departamento.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Para desactivar la identidad para numérico de las claves principales  

En el ejemplo siguiente se establece la propiedad DepartmentID en System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None para indicar que no se generará el valor de la base de datos.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Especifica la longitud máxima en una propiedad  

En el ejemplo siguiente, la propiedad Name debe ser no más de 50 caracteres. Si realiza el valor de más de 50 caracteres, obtendrá un [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) excepción. Si Code First crea una base de datos de este modelo también establecerá la longitud máxima de la columna Name y 50 caracteres.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Configurar la propiedad para que sea necesaria  

En el ejemplo siguiente, se requiere la propiedad Name. Si no especifica el nombre, obtendrá una excepción DbEntityValidationException. Si Code First crea una base de datos de este modelo de la columna utilizada para almacenar esta propiedad normalmente será distinto de NULL.  

> [!NOTE]
> En algunos casos puede no ser posible que la columna de la base de datos sea distinto de null, aunque la propiedad es obligatoria. Por ejemplo, cuando usa una estrategia de herencia de TPH datos para varios tipos se almacena en una sola tabla. Si un tipo derivado incluye una propiedad necesaria de que la columna no puede hacerse que no aceptan valores NULL ya que no todos los tipos en la jerarquía tienen esta propiedad.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Configuración de un índice en una o varias propiedades  

> [!NOTE]
> **EF6.1 y versiones posteriores solo** -atributo el índice se introdujo en Entity Framework 6.1. Si usa una versión anterior no se aplica la información de esta sección.  

Creación de índices no se admite de forma nativa la API Fluent, pero puede hacer uso de la compatibilidad con **IndexAttribute** a través de la API Fluent. Atributos de índice se procesan mediante la inclusión de una anotación de modelo en el modelo que, a continuación, se convierte en un índice en la base de datos más adelante en la canalización. Puede agregar manualmente estos mismos mediante la API Fluent de anotaciones.  

La manera más fácil de hacerlo es crear una instancia de **IndexAttribute** que contiene toda la configuración para el nuevo índice. A continuación, puede crear una instancia de **IndexAnnotation** que es un tipo específico de EF que convertirá el **IndexAttribute** la configuración de una anotación de modelo que se pueden almacenar en el modelo de EF. A continuación, estos se pasan a la **HasColumnAnnotation** método en la API Fluent, especificando el nombre **índice** para la anotación.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Para obtener una lista completa de las opciones disponibles en **IndexAttribute**, consulte el *índice* sección de [anotaciones de datos de Code First](~/ef6/modeling/code-first/data-annotations.md). Esto incluye personalizar el nombre del índice, crear índices únicos y creación de índices de varias columnas.  

Puede especificar varias anotaciones de índice en una sola propiedad pasando una matriz de **IndexAttribute** al constructor de **IndexAnnotation**.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation(
        "Index",  
        new IndexAnnotation(new[]
            {
                new IndexAttribute("Index1"),
                new IndexAttribute("Index2") { IsUnique = true }
            })));
```  

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>No especifica una propiedad de CLR se asignan a una columna de la base de datos  

El ejemplo siguiente muestra cómo especificar que una propiedad en un tipo CLR no está asignada a una columna de la base de datos.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Asignación de una propiedad de CLR a una columna específica de la base de datos  

El ejemplo siguiente asigna la propiedad CLR de nombre a la columna de base de datos DepartmentName.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Cambiar el nombre de una clave externa que no está definida en el modelo  

Si decide no definir una clave externa en un tipo CLR, pero desea especificar qué nombre debe tener la base de datos, realice lo siguiente:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Configurar si una propiedad de cadena admite contenido Unicode  

De forma predeterminada las cadenas son Unicode (nvarchar en SQL Server). Puede usar el método IsUnicode para especificar que debe ser una cadena de tipo varchar.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Configuración del tipo de datos de una columna de base de datos  

El [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) método permite la asignación a distintas representaciones del mismo tipo básico. Con este método no permite realizar cualquier conversión de datos en tiempo de ejecución. Tenga en cuenta que IsUnicode es la mejor manera de columnas de valor varchar, ya que es independiente de la base de datos.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Configuración de las propiedades en un tipo complejo  

Hay dos formas de configurar las propiedades escalares en un tipo complejo.  

Se puede llamar a propiedad ComplexTypeConfiguration.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

También puede usar la notación de puntos para obtener acceso a una propiedad de un tipo complejo.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Configuración de una propiedad que se usará como un Token de simultaneidad optimista  

Para especificar que una propiedad en una entidad representa un token de simultaneidad, puede usar el atributo ConcurrencyCheck o el método IsConcurrencyToken.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

También puede usar el método IsRowVersion para configurar la propiedad para que sea una versión de fila en la base de datos. Establecer la propiedad sea que una versión de fila configura automáticamente para que sea un token de simultaneidad optimista.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Asignación de tipos  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Especifica que una clase es un tipo complejo  

Por convención, un tipo que no tiene ninguna clave principal especificada se trata como un tipo complejo. Hay algunos escenarios donde Code First no detectará un tipo complejo (por ejemplo, si tiene una propiedad llamada ID, pero no significan que una clave principal). En tales casos, podría usar la API fluida para especificar explícitamente que un tipo es un tipo complejo.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>No especifica un tipo de entidad CLR se asignan a una tabla en la base de datos  

El ejemplo siguiente muestra cómo excluir un tipo CLR de la que se asigna a una tabla en la base de datos.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Asignación de un tipo de entidad a una tabla específica en la base de datos  

Todas las propiedades de departamento se asignarán a las columnas en una tabla denominada t_ departamento.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

También puede especificar el nombre del esquema similar al siguiente:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Asignación de la herencia de tabla por jerarquía (TPH)  

En el escenario de asignación de TPH, todos los tipos en una jerarquía de herencia se asignan a una sola tabla. Se usa una columna discriminadora para identificar el tipo de cada fila. Al crear el modelo con Code First, TPH es la estrategia predeterminada para los tipos que participan en la jerarquía de herencia. De forma predeterminada, la columna discriminadora se agrega a la tabla con el nombre "Discriminador" y se usa el nombre del tipo CLR de cada tipo en la jerarquía de los valores del discriminador. Puede modificar el comportamiento predeterminado mediante el uso de la API fluida.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Asignación de la herencia de tabla por tipo (TPT)  

En el escenario de asignación de TPT, todos los tipos se asignan a tablas individuales. Las propiedades que pertenecen solamente a un tipo base o a un tipo derivado se almacenan en una tabla que se asigna a ese tipo. Las tablas que se asignan a tipos derivados también almacenan una clave externa que une la tabla derivada con la tabla base.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Asignación de la herencia de tabla-por clase concreta (TPC)  

En el escenario de asignación TPC, todos los tipos no abstractos de la jerarquía se asignan a tablas individuales. Las tablas que se asignan a las clases derivadas no tienen ninguna relación a la tabla que se asigna a la clase base en la base de datos. Todas las propiedades de una clase, incluidas las propiedades heredadas, se asignan a columnas de la tabla correspondiente.  

Llame al método MapInheritedProperties para configurar cada tipo derivado. MapInheritedProperties reasigna todas las propiedades que se han heredado de la clase base para las nuevas columnas en la tabla de la clase derivada.  

> [!NOTE]
> Tenga en cuenta que dado que no comparten las tablas que participan en la jerarquía de herencia de TPC existe una clave principal será claves de entidad duplicados cuando se inserta en las tablas que se asignan a las subclases si tienen valores de la base de datos generada con el mismo valor de inicialización de identidad. Para resolver este problema, puede especificar un valor de inicialización inicial diferente para cada tabla o desactivar la identidad en la propiedad de clave principal. Identidad es el valor predeterminado para las propiedades de clave de entero al trabajar con Code First.  

``` csharp
modelBuilder.Entity<Course>()
    .Property(c => c.CourseID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);

modelBuilder.Entity<OnsiteCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnsiteCourse");
});

modelBuilder.Entity<OnlineCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnlineCourse");
});
```  

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Asignación de propiedades de un tipo de entidad a varias tablas en la base de datos (división de entidades)  

División de entidades permite que las propiedades de un tipo de entidad se distribuyan entre varias tablas. En el ejemplo siguiente, la entidad Department se divide en dos tablas: departamento y DepartmentDetails. Separación de entidades usa varias llamadas al método de asignación para asignar un subconjunto de propiedades a una tabla específica.  

``` csharp
modelBuilder.Entity<Department>()
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Name });
        m.ToTable("Department");
    })
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Administrator, t.StartDate, t.Budget });
        m.ToTable("DepartmentDetails");
    });
```  

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Asignación de varios tipos de entidad a una tabla en la base de datos (división de tablas)  

El ejemplo siguiente asigna a dos tipos de entidad que comparten una clave principal a una tabla.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Asignación de un tipo de entidad a procedimientos almacenados Insert, Update o Delete (EF6 y versiones posteriores)  

A partir de con EF6, puede asignar una entidad para usar procedimientos almacenados con insert, update y delete. Para obtener más información, consulte [código primera Insert, Update o Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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
