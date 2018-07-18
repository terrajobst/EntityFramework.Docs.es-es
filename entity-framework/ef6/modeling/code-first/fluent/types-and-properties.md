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
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="a7500-102">API de Fluent: configuración y asignación de tipos y propiedades</span><span class="sxs-lookup"><span data-stu-id="a7500-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="a7500-103">Cuando se trabaja con Entity Framework Code First es el comportamiento predeterminado asignar las clases POCO a tablas mediante un conjunto de convenciones incorporada en EF.</span><span class="sxs-lookup"><span data-stu-id="a7500-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="a7500-104">A veces, sin embargo, no puede ni no desea seguir estas convenciones y deba asignar entidades a un valor distinto de lo que dictan las convenciones.</span><span class="sxs-lookup"><span data-stu-id="a7500-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="a7500-105">Hay dos formas principales de EF para usar un valor distinto de las convenciones, es decir, puede configurar [anotaciones](~/ef6/modeling/code-first/data-annotations.md) o API fluida de EFs.</span><span class="sxs-lookup"><span data-stu-id="a7500-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="a7500-106">Las anotaciones abarcan solo un subconjunto de la funcionalidad de la API fluida, por lo que hay escenarios de asignación que no se puede lograr mediante anotaciones.</span><span class="sxs-lookup"><span data-stu-id="a7500-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="a7500-107">En este artículo está diseñado para demostrar cómo usar la API fluida para configurar las propiedades.</span><span class="sxs-lookup"><span data-stu-id="a7500-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="a7500-108">Normalmente se tiene acceso a la API fluida de code first invalidando el [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) método en su derivada [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="a7500-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="a7500-109">Los ejemplos siguientes están diseñados para mostrar cómo realizar diversas tareas con la api fluida y le permiten copiar el código y personalizarla para adaptarla a su modelo, si desea ver el modelo que puede utilizarse con como-es, a continuación, se proporciona al final de este artículo.</span><span class="sxs-lookup"><span data-stu-id="a7500-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="a7500-110">Configuración de todo el modelo</span><span class="sxs-lookup"><span data-stu-id="a7500-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="a7500-111">Esquema predeterminado (EF6 y versiones posteriores)</span><span class="sxs-lookup"><span data-stu-id="a7500-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="a7500-112">A partir de con EF6, puede usar el método HasDefaultSchema en DbModelBuilder para especificar el esquema de base de datos que se usará para todas las tablas, procedimientos almacenados, etcetera. Para los objetos que se configure explícitamente un esquema diferente para se invalidará esta configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="a7500-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="a7500-113">Convenciones personalizadas (EF6 y versiones posteriores)</span><span class="sxs-lookup"><span data-stu-id="a7500-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="a7500-114">A partir de EF6, puede crear sus propias convenciones para complementar los que se incluyen en Code First.</span><span class="sxs-lookup"><span data-stu-id="a7500-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="a7500-115">Para obtener más información, consulte [convenciones de Code First personalizadas](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="a7500-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="a7500-116">Asignación de propiedades</span><span class="sxs-lookup"><span data-stu-id="a7500-116">Property Mapping</span></span>  

<span data-ttu-id="a7500-117">El [propiedad](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) método se utiliza para configurar los atributos para cada propiedad que pertenecen a una entidad o tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="a7500-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="a7500-118">El método de propiedad se utiliza para obtener un objeto de configuración para una propiedad determinada.</span><span class="sxs-lookup"><span data-stu-id="a7500-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="a7500-119">Las opciones en el objeto de configuración son específicas del tipo que se va a configurar; IsUnicode solo está disponible en las propiedades de cadena por ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a7500-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="a7500-120">Configuración de una clave principal</span><span class="sxs-lookup"><span data-stu-id="a7500-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="a7500-121">Es la convención de Entity Framework para las claves principales:</span><span class="sxs-lookup"><span data-stu-id="a7500-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="a7500-122">Su clase define una propiedad cuyo nombre es "ID" o "ID".</span><span class="sxs-lookup"><span data-stu-id="a7500-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="a7500-123">o un nombre de clase seguido de "ID" o "ID".</span><span class="sxs-lookup"><span data-stu-id="a7500-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="a7500-124">Para establecer explícitamente una propiedad puede ser una clave principal, puede usar el método HasKey.</span><span class="sxs-lookup"><span data-stu-id="a7500-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="a7500-125">En el ejemplo siguiente, se usa el método HasKey para configurar la clave principal InstructorID en el tipo OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="a7500-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="a7500-126">Configuración de una clave principal compuesta</span><span class="sxs-lookup"><span data-stu-id="a7500-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="a7500-127">El ejemplo siguiente configura las propiedades DepartmentID y Name para que sea la clave principal compuesta del tipo de departamento.</span><span class="sxs-lookup"><span data-stu-id="a7500-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="a7500-128">Para desactivar la identidad para numérico de las claves principales</span><span class="sxs-lookup"><span data-stu-id="a7500-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="a7500-129">En el ejemplo siguiente se establece la propiedad DepartmentID en System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None para indicar que no se generará el valor de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a7500-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="a7500-130">Especifica la longitud máxima en una propiedad</span><span class="sxs-lookup"><span data-stu-id="a7500-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="a7500-131">En el ejemplo siguiente, la propiedad Name debe ser no más de 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="a7500-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="a7500-132">Si realiza el valor de más de 50 caracteres, obtendrá un [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) excepción.</span><span class="sxs-lookup"><span data-stu-id="a7500-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="a7500-133">Si Code First crea una base de datos de este modelo también establecerá la longitud máxima de la columna Name y 50 caracteres.</span><span class="sxs-lookup"><span data-stu-id="a7500-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="a7500-134">Configurar la propiedad para que sea necesaria</span><span class="sxs-lookup"><span data-stu-id="a7500-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="a7500-135">En el ejemplo siguiente, se requiere la propiedad Name.</span><span class="sxs-lookup"><span data-stu-id="a7500-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="a7500-136">Si no especifica el nombre, obtendrá una excepción DbEntityValidationException.</span><span class="sxs-lookup"><span data-stu-id="a7500-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="a7500-137">Si Code First crea una base de datos de este modelo de la columna utilizada para almacenar esta propiedad normalmente será distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="a7500-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="a7500-138">En algunos casos puede no ser posible que la columna de la base de datos sea distinto de null, aunque la propiedad es obligatoria.</span><span class="sxs-lookup"><span data-stu-id="a7500-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="a7500-139">Por ejemplo, cuando usa una estrategia de herencia de TPH datos para varios tipos se almacena en una sola tabla.</span><span class="sxs-lookup"><span data-stu-id="a7500-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="a7500-140">Si un tipo derivado incluye una propiedad necesaria de que la columna no puede hacerse que no aceptan valores NULL ya que no todos los tipos en la jerarquía tienen esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="a7500-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="a7500-141">Configuración de un índice en una o varias propiedades</span><span class="sxs-lookup"><span data-stu-id="a7500-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="a7500-142">**EF6.1 y versiones posteriores solo** -atributo el índice se introdujo en Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="a7500-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="a7500-143">Si usa una versión anterior no se aplica la información de esta sección.</span><span class="sxs-lookup"><span data-stu-id="a7500-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="a7500-144">Creación de índices no se admite de forma nativa la API Fluent, pero puede hacer uso de la compatibilidad con **IndexAttribute** a través de la API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a7500-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="a7500-145">Atributos de índice se procesan mediante la inclusión de una anotación de modelo en el modelo que, a continuación, se convierte en un índice en la base de datos más adelante en la canalización.</span><span class="sxs-lookup"><span data-stu-id="a7500-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="a7500-146">Puede agregar manualmente estos mismos mediante la API Fluent de anotaciones.</span><span class="sxs-lookup"><span data-stu-id="a7500-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="a7500-147">La manera más fácil de hacerlo es crear una instancia de **IndexAttribute** que contiene toda la configuración para el nuevo índice.</span><span class="sxs-lookup"><span data-stu-id="a7500-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="a7500-148">A continuación, puede crear una instancia de **IndexAnnotation** que es un tipo específico de EF que convertirá el **IndexAttribute** la configuración de una anotación de modelo que se pueden almacenar en el modelo de EF.</span><span class="sxs-lookup"><span data-stu-id="a7500-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="a7500-149">A continuación, estos se pasan a la **HasColumnAnnotation** método en la API Fluent, especificando el nombre **índice** para la anotación.</span><span class="sxs-lookup"><span data-stu-id="a7500-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="a7500-150">Para obtener una lista completa de las opciones disponibles en **IndexAttribute**, consulte el *índice* sección de [anotaciones de datos de Code First](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="a7500-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="a7500-151">Esto incluye personalizar el nombre del índice, crear índices únicos y creación de índices de varias columnas.</span><span class="sxs-lookup"><span data-stu-id="a7500-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="a7500-152">Puede especificar varias anotaciones de índice en una sola propiedad pasando una matriz de **IndexAttribute** al constructor de **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="a7500-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="a7500-153">No especifica una propiedad de CLR se asignan a una columna de la base de datos</span><span class="sxs-lookup"><span data-stu-id="a7500-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="a7500-154">El ejemplo siguiente muestra cómo especificar que una propiedad en un tipo CLR no está asignada a una columna de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a7500-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="a7500-155">Asignación de una propiedad de CLR a una columna específica de la base de datos</span><span class="sxs-lookup"><span data-stu-id="a7500-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="a7500-156">El ejemplo siguiente asigna la propiedad CLR de nombre a la columna de base de datos DepartmentName.</span><span class="sxs-lookup"><span data-stu-id="a7500-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="a7500-157">Cambiar el nombre de una clave externa que no está definida en el modelo</span><span class="sxs-lookup"><span data-stu-id="a7500-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="a7500-158">Si decide no definir una clave externa en un tipo CLR, pero desea especificar qué nombre debe tener la base de datos, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a7500-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="a7500-159">Configurar si una propiedad de cadena admite contenido Unicode</span><span class="sxs-lookup"><span data-stu-id="a7500-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="a7500-160">De forma predeterminada las cadenas son Unicode (nvarchar en SQL Server).</span><span class="sxs-lookup"><span data-stu-id="a7500-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="a7500-161">Puede usar el método IsUnicode para especificar que debe ser una cadena de tipo varchar.</span><span class="sxs-lookup"><span data-stu-id="a7500-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="a7500-162">Configuración del tipo de datos de una columna de base de datos</span><span class="sxs-lookup"><span data-stu-id="a7500-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="a7500-163">El [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) método permite la asignación a distintas representaciones del mismo tipo básico.</span><span class="sxs-lookup"><span data-stu-id="a7500-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="a7500-164">Con este método no permite realizar cualquier conversión de datos en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a7500-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="a7500-165">Tenga en cuenta que IsUnicode es la mejor manera de columnas de valor varchar, ya que es independiente de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a7500-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="a7500-166">Configuración de las propiedades en un tipo complejo</span><span class="sxs-lookup"><span data-stu-id="a7500-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="a7500-167">Hay dos formas de configurar las propiedades escalares en un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="a7500-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="a7500-168">Se puede llamar a propiedad ComplexTypeConfiguration.</span><span class="sxs-lookup"><span data-stu-id="a7500-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="a7500-169">También puede usar la notación de puntos para obtener acceso a una propiedad de un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="a7500-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="a7500-170">Configuración de una propiedad que se usará como un Token de simultaneidad optimista</span><span class="sxs-lookup"><span data-stu-id="a7500-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="a7500-171">Para especificar que una propiedad en una entidad representa un token de simultaneidad, puede usar el atributo ConcurrencyCheck o el método IsConcurrencyToken.</span><span class="sxs-lookup"><span data-stu-id="a7500-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="a7500-172">También puede usar el método IsRowVersion para configurar la propiedad para que sea una versión de fila en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a7500-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="a7500-173">Establecer la propiedad sea que una versión de fila configura automáticamente para que sea un token de simultaneidad optimista.</span><span class="sxs-lookup"><span data-stu-id="a7500-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="a7500-174">Asignación de tipos</span><span class="sxs-lookup"><span data-stu-id="a7500-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="a7500-175">Especifica que una clase es un tipo complejo</span><span class="sxs-lookup"><span data-stu-id="a7500-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="a7500-176">Por convención, un tipo que no tiene ninguna clave principal especificada se trata como un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="a7500-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="a7500-177">Hay algunos escenarios donde Code First no detectará un tipo complejo (por ejemplo, si tiene una propiedad llamada ID, pero no significan que una clave principal).</span><span class="sxs-lookup"><span data-stu-id="a7500-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="a7500-178">En tales casos, podría usar la API fluida para especificar explícitamente que un tipo es un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="a7500-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="a7500-179">No especifica un tipo de entidad CLR se asignan a una tabla en la base de datos</span><span class="sxs-lookup"><span data-stu-id="a7500-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="a7500-180">El ejemplo siguiente muestra cómo excluir un tipo CLR de la que se asigna a una tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a7500-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="a7500-181">Asignación de un tipo de entidad a una tabla específica en la base de datos</span><span class="sxs-lookup"><span data-stu-id="a7500-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="a7500-182">Todas las propiedades de departamento se asignarán a las columnas en una tabla denominada t_ departamento.</span><span class="sxs-lookup"><span data-stu-id="a7500-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="a7500-183">También puede especificar el nombre del esquema similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a7500-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="a7500-184">Asignación de la herencia de tabla por jerarquía (TPH)</span><span class="sxs-lookup"><span data-stu-id="a7500-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="a7500-185">En el escenario de asignación de TPH, todos los tipos en una jerarquía de herencia se asignan a una sola tabla.</span><span class="sxs-lookup"><span data-stu-id="a7500-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="a7500-186">Se usa una columna discriminadora para identificar el tipo de cada fila.</span><span class="sxs-lookup"><span data-stu-id="a7500-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="a7500-187">Al crear el modelo con Code First, TPH es la estrategia predeterminada para los tipos que participan en la jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="a7500-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="a7500-188">De forma predeterminada, la columna discriminadora se agrega a la tabla con el nombre "Discriminador" y se usa el nombre del tipo CLR de cada tipo en la jerarquía de los valores del discriminador.</span><span class="sxs-lookup"><span data-stu-id="a7500-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="a7500-189">Puede modificar el comportamiento predeterminado mediante el uso de la API fluida.</span><span class="sxs-lookup"><span data-stu-id="a7500-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="a7500-190">Asignación de la herencia de tabla por tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="a7500-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="a7500-191">En el escenario de asignación de TPT, todos los tipos se asignan a tablas individuales.</span><span class="sxs-lookup"><span data-stu-id="a7500-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="a7500-192">Las propiedades que pertenecen solamente a un tipo base o a un tipo derivado se almacenan en una tabla que se asigna a ese tipo.</span><span class="sxs-lookup"><span data-stu-id="a7500-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="a7500-193">Las tablas que se asignan a tipos derivados también almacenan una clave externa que une la tabla derivada con la tabla base.</span><span class="sxs-lookup"><span data-stu-id="a7500-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="a7500-194">Asignación de la herencia de tabla-por clase concreta (TPC)</span><span class="sxs-lookup"><span data-stu-id="a7500-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="a7500-195">En el escenario de asignación TPC, todos los tipos no abstractos de la jerarquía se asignan a tablas individuales.</span><span class="sxs-lookup"><span data-stu-id="a7500-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="a7500-196">Las tablas que se asignan a las clases derivadas no tienen ninguna relación a la tabla que se asigna a la clase base en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a7500-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="a7500-197">Todas las propiedades de una clase, incluidas las propiedades heredadas, se asignan a columnas de la tabla correspondiente.</span><span class="sxs-lookup"><span data-stu-id="a7500-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="a7500-198">Llame al método MapInheritedProperties para configurar cada tipo derivado.</span><span class="sxs-lookup"><span data-stu-id="a7500-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="a7500-199">MapInheritedProperties reasigna todas las propiedades que se han heredado de la clase base para las nuevas columnas en la tabla de la clase derivada.</span><span class="sxs-lookup"><span data-stu-id="a7500-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="a7500-200">Tenga en cuenta que dado que no comparten las tablas que participan en la jerarquía de herencia de TPC existe una clave principal será claves de entidad duplicados cuando se inserta en las tablas que se asignan a las subclases si tienen valores de la base de datos generada con el mismo valor de inicialización de identidad.</span><span class="sxs-lookup"><span data-stu-id="a7500-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="a7500-201">Para resolver este problema, puede especificar un valor de inicialización inicial diferente para cada tabla o desactivar la identidad en la propiedad de clave principal.</span><span class="sxs-lookup"><span data-stu-id="a7500-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="a7500-202">Identidad es el valor predeterminado para las propiedades de clave de entero al trabajar con Code First.</span><span class="sxs-lookup"><span data-stu-id="a7500-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="a7500-203">Asignación de propiedades de un tipo de entidad a varias tablas en la base de datos (división de entidades)</span><span class="sxs-lookup"><span data-stu-id="a7500-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="a7500-204">División de entidades permite que las propiedades de un tipo de entidad se distribuyan entre varias tablas.</span><span class="sxs-lookup"><span data-stu-id="a7500-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="a7500-205">En el ejemplo siguiente, la entidad Department se divide en dos tablas: departamento y DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="a7500-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="a7500-206">Separación de entidades usa varias llamadas al método de asignación para asignar un subconjunto de propiedades a una tabla específica.</span><span class="sxs-lookup"><span data-stu-id="a7500-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="a7500-207">Asignación de varios tipos de entidad a una tabla en la base de datos (división de tablas)</span><span class="sxs-lookup"><span data-stu-id="a7500-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="a7500-208">El ejemplo siguiente asigna a dos tipos de entidad que comparten una clave principal a una tabla.</span><span class="sxs-lookup"><span data-stu-id="a7500-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="a7500-209">Asignación de un tipo de entidad a procedimientos almacenados Insert, Update o Delete (EF6 y versiones posteriores)</span><span class="sxs-lookup"><span data-stu-id="a7500-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="a7500-210">A partir de con EF6, puede asignar una entidad para usar procedimientos almacenados con insert, update y delete.</span><span class="sxs-lookup"><span data-stu-id="a7500-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="a7500-211">Para obtener más información, consulte [código primera Insert, Update o Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="a7500-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="a7500-212">Modelo que se usa en los ejemplos</span><span class="sxs-lookup"><span data-stu-id="a7500-212">Model Used in Samples</span></span>  

<span data-ttu-id="a7500-213">El siguiente modelo de Code First se usa para los ejemplos en esta página.</span><span class="sxs-lookup"><span data-stu-id="a7500-213">The following Code First model is used for the samples on this page.</span></span>  

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
