---
title: Relaciones - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 0e60ade605ffb73b02934ea32f1c757affce970d
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949223"
---
# <a name="relationships"></a><span data-ttu-id="66516-102">Relaciones</span><span class="sxs-lookup"><span data-stu-id="66516-102">Relationships</span></span>

<span data-ttu-id="66516-103">Una relación define cómo dos entidades se relacionan entre sí.</span><span class="sxs-lookup"><span data-stu-id="66516-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="66516-104">En una base de datos relacional, esto se representa mediante una restricción foreign key.</span><span class="sxs-lookup"><span data-stu-id="66516-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="66516-105">La mayoría de los ejemplos de este artículo usa una relación uno a varios para mostrar los conceptos.</span><span class="sxs-lookup"><span data-stu-id="66516-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="66516-106">Para obtener ejemplos de las relaciones uno a uno y varios a varios, consulte el [otros patrones de relación](#other-relationship-patterns) sección al final del artículo.</span><span class="sxs-lookup"><span data-stu-id="66516-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="66516-107">Definición de términos</span><span class="sxs-lookup"><span data-stu-id="66516-107">Definition of Terms</span></span>

<span data-ttu-id="66516-108">Hay una serie de términos que se usan para describir relaciones</span><span class="sxs-lookup"><span data-stu-id="66516-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="66516-109">**Entidad dependiente:** se trata de la entidad que contiene las propiedades de clave externa.</span><span class="sxs-lookup"><span data-stu-id="66516-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="66516-110">A veces se denomina "secundario" de la relación.</span><span class="sxs-lookup"><span data-stu-id="66516-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="66516-111">**Entidad de seguridad:** se trata de la entidad que contiene las propiedades de clave principal o alternativa.</span><span class="sxs-lookup"><span data-stu-id="66516-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="66516-112">A veces se denomina el 'parent' de la relación.</span><span class="sxs-lookup"><span data-stu-id="66516-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="66516-113">**Clave externa:** las propiedades de la entidad dependiente que se usa para almacenar los valores de la propiedad de clave principal que está relacionado con la entidad.</span><span class="sxs-lookup"><span data-stu-id="66516-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="66516-114">**Clave principal:** las propiedades que identifica la entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="66516-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="66516-115">Esto puede ser la clave principal o una clave alternativa.</span><span class="sxs-lookup"><span data-stu-id="66516-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="66516-116">**Propiedad de navegación:** una propiedad definida en la entidad dependiente o principal que contenga una referencias a las entidades relacionadas compradoras.</span><span class="sxs-lookup"><span data-stu-id="66516-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="66516-117">**Propiedad de navegación de colección:** una propiedad de navegación que contiene referencias a muchas de las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="66516-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="66516-118">**Referencia de propiedad de navegación:** una propiedad de navegación que contiene una referencia a una única entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="66516-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="66516-119">**Propiedad de navegación inversa:** al hablar de una propiedad de navegación determinado, este término hace referencia a la propiedad de navegación en el otro extremo de la relación.</span><span class="sxs-lookup"><span data-stu-id="66516-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="66516-120">La lista de código siguiente muestra una relación de uno a varios entre `Blog` y `Post`</span><span class="sxs-lookup"><span data-stu-id="66516-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="66516-121">`Post` es la entidad dependiente</span><span class="sxs-lookup"><span data-stu-id="66516-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="66516-122">`Blog` es la entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="66516-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="66516-123">`Post.BlogId` es la clave externa</span><span class="sxs-lookup"><span data-stu-id="66516-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="66516-124">`Blog.BlogId` es la clave principal (en este caso es una clave principal en lugar de una clave alternativa)</span><span class="sxs-lookup"><span data-stu-id="66516-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="66516-125">`Post.Blog` es una propiedad de navegación de referencia</span><span class="sxs-lookup"><span data-stu-id="66516-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="66516-126">`Blog.Posts` es una propiedad de navegación de colección</span><span class="sxs-lookup"><span data-stu-id="66516-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="66516-127">`Post.Blog` es la propiedad de navegación inversa de `Blog.Posts` (y viceversa)</span><span class="sxs-lookup"><span data-stu-id="66516-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="66516-128">Convenciones</span><span class="sxs-lookup"><span data-stu-id="66516-128">Conventions</span></span>

<span data-ttu-id="66516-129">Por convención, se creará una relación cuando hay una propiedad de navegación detectada en un tipo.</span><span class="sxs-lookup"><span data-stu-id="66516-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="66516-130">Una propiedad se considera una propiedad de navegación si el tipo a que apunta no puede asignarse como un tipo escalar por el proveedor de base de datos actual.</span><span class="sxs-lookup"><span data-stu-id="66516-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="66516-131">Las relaciones que se detectan por convención, siempre tendrá como destino la clave principal de la entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="66516-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="66516-132">Para establecer el destino de una clave alternativa, debe realizarse una configuración adicional mediante la API Fluent.</span><span class="sxs-lookup"><span data-stu-id="66516-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="66516-133">Relaciones definidas completamente</span><span class="sxs-lookup"><span data-stu-id="66516-133">Fully Defined Relationships</span></span>

<span data-ttu-id="66516-134">El modelo más común para las relaciones es que las propiedades de navegación en ambos extremos de la relación y una propiedad de clave externa definidas en la clase de entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="66516-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="66516-135">Si un par de propiedades de navegación se encuentra entre dos tipos, se configurarán como propiedades de navegación inversa de la misma relación.</span><span class="sxs-lookup"><span data-stu-id="66516-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="66516-136">Si la entidad dependiente contiene una propiedad denominada `<primary key property name>`, `<navigation property name><primary key property name>`, o `<principal entity name><primary key property name>` , a continuación, se configurará como la clave externa.</span><span class="sxs-lookup"><span data-stu-id="66516-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="66516-137">Si hay varias propiedades de navegación definidas entre dos tipos (es decir, más de un par distinto de las navegaciones que señalan entre sí), a continuación, no se creará ninguna relación por convención y deberá configurarlas manualmente para identificar el modo las propiedades de navegación se empareje.</span><span class="sxs-lookup"><span data-stu-id="66516-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="66516-138">Ninguna propiedad de clave externa</span><span class="sxs-lookup"><span data-stu-id="66516-138">No Foreign Key Property</span></span>

<span data-ttu-id="66516-139">Aunque se recomienda tener una propiedad de clave externa definida en la clase de entidad dependiente, no es necesario.</span><span class="sxs-lookup"><span data-stu-id="66516-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="66516-140">Si no se encuentra ninguna propiedad de clave externa, se introducirá una propiedad de clave externa de instantáneas con el nombre `<navigation property name><principal key property name>` (consulte [reemplazar propiedades](shadow-properties.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="66516-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="66516-141">Propiedad de navegación único</span><span class="sxs-lookup"><span data-stu-id="66516-141">Single Navigation Property</span></span>

<span data-ttu-id="66516-142">Incluido sólo una propiedad de navegación (ninguna navegación inversa y ninguna propiedad de clave externa) es suficiente para tener una relación definida por convención.</span><span class="sxs-lookup"><span data-stu-id="66516-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="66516-143">También puede tener una propiedad de navegación único y una propiedad de clave externa.</span><span class="sxs-lookup"><span data-stu-id="66516-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="66516-144">Eliminación en cascada</span><span class="sxs-lookup"><span data-stu-id="66516-144">Cascade Delete</span></span>

<span data-ttu-id="66516-145">Por convención, la eliminación en cascada se establecerá en *Cascade* para relaciones necesarias y *ClientSetNull* para las relaciones opcionales.</span><span class="sxs-lookup"><span data-stu-id="66516-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="66516-146">*CASCADE* significa que las entidades dependientes también se eliminan.</span><span class="sxs-lookup"><span data-stu-id="66516-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="66516-147">*ClientSetNull* significa que las entidades dependientes que no se cargan en memoria permanecerá sin cambios y debe ser eliminado manualmente o actualiza para señalar a una entidad de seguridad válida.</span><span class="sxs-lookup"><span data-stu-id="66516-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="66516-148">Para las entidades que se cargan en memoria, EF Core intentará establecer las propiedades de clave externas en null.</span><span class="sxs-lookup"><span data-stu-id="66516-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="66516-149">Consulte la [relaciones obligatorios y opcionales](#required-and-optional-relationships) conocer la diferencia entre las relaciones necesarias y opcionales.</span><span class="sxs-lookup"><span data-stu-id="66516-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="66516-150">Consulte [de eliminación en cascada](../saving/cascade-delete.md) para obtener más detalles sobre los diferentes de eliminación de los comportamientos y los valores predeterminados usados por convención.</span><span class="sxs-lookup"><span data-stu-id="66516-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="66516-151">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="66516-151">Data Annotations</span></span>

<span data-ttu-id="66516-152">Hay dos de las anotaciones de datos que se pueden usar para configurar las relaciones, `[ForeignKey]` y `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="66516-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="66516-153">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="66516-153">[ForeignKey]</span></span>

<span data-ttu-id="66516-154">Puede utilizar las anotaciones de datos para configurar la propiedad que debe usarse como la propiedad de clave externa de una relación determinada.</span><span class="sxs-lookup"><span data-stu-id="66516-154">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="66516-155">Normalmente, esto se hace cuando la propiedad de clave externa no se detecta por convención.</span><span class="sxs-lookup"><span data-stu-id="66516-155">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> <span data-ttu-id="66516-156">El `[ForeignKey]` anotación puede colocarse en cualquiera de las propiedades de navegación en la relación.</span><span class="sxs-lookup"><span data-stu-id="66516-156">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="66516-157">No debe ir en la propiedad de navegación en la clase de entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="66516-157">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="66516-158">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="66516-158">[InverseProperty]</span></span>

<span data-ttu-id="66516-159">Puede utilizar las anotaciones de datos para configurar cómo se empareje las propiedades de navegación en las entidades dependientes y principales.</span><span class="sxs-lookup"><span data-stu-id="66516-159">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="66516-160">Normalmente, esto se hace cuando hay más de un par de propiedades de navegación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="66516-160">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a><span data-ttu-id="66516-161">API fluida</span><span class="sxs-lookup"><span data-stu-id="66516-161">Fluent API</span></span>

<span data-ttu-id="66516-162">Para configurar una relación en la API Fluent, primero debe identificar las propiedades de navegación que componen la relación.</span><span class="sxs-lookup"><span data-stu-id="66516-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="66516-163">`HasOne` o `HasMany` identifica el tipo de entidad que se va a iniciar la configuración de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="66516-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="66516-164">A continuación, encadenar una llamada a `WithOne` o `WithMany` para identificar el panel de navegación inversa.</span><span class="sxs-lookup"><span data-stu-id="66516-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="66516-165">`HasOne`/`WithOne` se usan para las propiedades de navegación de referencia y `HasMany` / `WithMany` se usan para las propiedades de navegación de colección.</span><span class="sxs-lookup"><span data-stu-id="66516-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a><span data-ttu-id="66516-166">Propiedad de navegación único</span><span class="sxs-lookup"><span data-stu-id="66516-166">Single Navigation Property</span></span>

<span data-ttu-id="66516-167">Si solo tiene una propiedad de navegación, hay sobrecargas sin parámetros de `WithOne` y `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="66516-167">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="66516-168">Esto indica que conceptualmente es una referencia o colección en el otro extremo de la relación, pero no hay ninguna propiedad de navegación incluida en la clase de entidad.</span><span class="sxs-lookup"><span data-stu-id="66516-168">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a><span data-ttu-id="66516-169">Clave externa</span><span class="sxs-lookup"><span data-stu-id="66516-169">Foreign Key</span></span>

<span data-ttu-id="66516-170">Puede usar la API Fluent para configurar la propiedad que debe usarse como la propiedad de clave externa de una relación determinada.</span><span class="sxs-lookup"><span data-stu-id="66516-170">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

<span data-ttu-id="66516-171">La lista de código siguiente muestra cómo configurar una clave externa compuesta.</span><span class="sxs-lookup"><span data-stu-id="66516-171">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

<span data-ttu-id="66516-172">Puede utilizar la sobrecarga de la cadena de `HasForeignKey(...)` para configurar una propiedad reemplazada como una clave externa (consulte [reemplazar propiedades](shadow-properties.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="66516-172">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="66516-173">Se recomienda agregar explícitamente la propiedad de la sombra para el modelo antes de usarlo como clave externa (como se muestra a continuación).</span><span class="sxs-lookup"><span data-stu-id="66516-173">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="66516-174">Clave de entidad de seguridad</span><span class="sxs-lookup"><span data-stu-id="66516-174">Principal Key</span></span>

<span data-ttu-id="66516-175">Si desea que la clave externa para hacer referencia a una propiedad que no sea la clave principal, puede usar la API Fluent para configurar la propiedad de clave principal para la relación.</span><span class="sxs-lookup"><span data-stu-id="66516-175">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="66516-176">La propiedad que se configura como la clave principal no se realizará automáticamente configurarse como una clave alternativa (vea [claves alternativas](alternate-keys.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="66516-176">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

<span data-ttu-id="66516-177">La lista de código siguiente muestra cómo configurar una clave principal compuesta.</span><span class="sxs-lookup"><span data-stu-id="66516-177">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> <span data-ttu-id="66516-178">El orden en el que especifica las propiedades de clave principales debe coincidir con el orden en que se especifican para la clave externa.</span><span class="sxs-lookup"><span data-stu-id="66516-178">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="66516-179">Relaciones necesarias y opcionales</span><span class="sxs-lookup"><span data-stu-id="66516-179">Required and Optional Relationships</span></span>

<span data-ttu-id="66516-180">Puede usar la API Fluent para configurar si la relación es obligatorio u opcional.</span><span class="sxs-lookup"><span data-stu-id="66516-180">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="66516-181">En última instancia, esto controla si la propiedad de clave externa es obligatorio u opcional.</span><span class="sxs-lookup"><span data-stu-id="66516-181">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="66516-182">Esto es más útil cuando se usa una clave externa del estado de sombra.</span><span class="sxs-lookup"><span data-stu-id="66516-182">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="66516-183">Si tiene una propiedad de clave externa en la clase de entidad, el requiredness de la relación se determina en función de si la propiedad de clave externa es obligatorio u opcional (vea [propiedades obligatorias y opcionales](required-optional.md) para obtener más información información).</span><span class="sxs-lookup"><span data-stu-id="66516-183">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a><span data-ttu-id="66516-184">Eliminación en cascada</span><span class="sxs-lookup"><span data-stu-id="66516-184">Cascade Delete</span></span>

<span data-ttu-id="66516-185">Puede usar la API Fluent para configurar el comportamiento de eliminación en cascada para una relación determinada de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="66516-185">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="66516-186">Consulte [de eliminación en cascada](../saving/cascade-delete.md) en la sección de guardado de datos para obtener una explicación detallada de cada opción.</span><span class="sxs-lookup"><span data-stu-id="66516-186">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a><span data-ttu-id="66516-187">Otros patrones de relación</span><span class="sxs-lookup"><span data-stu-id="66516-187">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="66516-188">Uno a uno</span><span class="sxs-lookup"><span data-stu-id="66516-188">One-to-one</span></span>

<span data-ttu-id="66516-189">Relaciones uno a uno tienen una propiedad de navegación de referencia en ambos lados.</span><span class="sxs-lookup"><span data-stu-id="66516-189">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="66516-190">Siguen las mismas convenciones que relaciones uno a varios, pero un índice único se introdujo en la propiedad de clave externa para asegurarse de que solo uno dependiente está relacionado con cada entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="66516-190">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> <span data-ttu-id="66516-191">EF elegirá una de las entidades en el dependiente en función de su capacidad para detectar una propiedad de clave externa.</span><span class="sxs-lookup"><span data-stu-id="66516-191">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="66516-192">Si la entidad incorrecta se elige como el dependiente, puede usar la API Fluent para corregir este problema.</span><span class="sxs-lookup"><span data-stu-id="66516-192">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="66516-193">Al configurar la relación con la API Fluent, usa el `HasOne` y `WithOne` métodos.</span><span class="sxs-lookup"><span data-stu-id="66516-193">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="66516-194">Al configurar la clave externa debe especificar el tipo de entidad dependiente: tenga en cuenta el parámetro genérico proporcionado para `HasForeignKey` en la lista siguiente.</span><span class="sxs-lookup"><span data-stu-id="66516-194">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="66516-195">En una relación uno a varios es evidente que la entidad con la navegación de referencia es dependiente y el otro con la colección es la entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="66516-195">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="66516-196">Pero esto no es así en una relación uno a uno: por lo tanto, la necesidad de definirla explícitamente.</span><span class="sxs-lookup"><span data-stu-id="66516-196">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a><span data-ttu-id="66516-197">Varios a varios</span><span class="sxs-lookup"><span data-stu-id="66516-197">Many-to-many</span></span>

<span data-ttu-id="66516-198">Relaciones varios a varios sin una clase de entidad para representar la tabla de combinación no se admiten aún.</span><span class="sxs-lookup"><span data-stu-id="66516-198">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="66516-199">Sin embargo, puede representar una relación varios a varios mediante la inclusión de una clase de entidad para la tabla de combinación y dos relaciones uno a varios independiente de asignación.</span><span class="sxs-lookup"><span data-stu-id="66516-199">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
