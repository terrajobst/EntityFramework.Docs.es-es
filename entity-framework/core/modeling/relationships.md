---
title: 'Relaciones: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e9c62bec47263ef452c7ac425a0bb446f9371d8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197657"
---
# <a name="relationships"></a><span data-ttu-id="98e3c-102">Relaciones</span><span class="sxs-lookup"><span data-stu-id="98e3c-102">Relationships</span></span>

<span data-ttu-id="98e3c-103">Una relación define el modo en que dos entidades se relacionan entre sí.</span><span class="sxs-lookup"><span data-stu-id="98e3c-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="98e3c-104">En una base de datos relacional, se representa mediante una restricción FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="98e3c-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="98e3c-105">La mayoría de los ejemplos de este artículo usan una relación de uno a varios para demostrar los conceptos.</span><span class="sxs-lookup"><span data-stu-id="98e3c-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="98e3c-106">Para obtener ejemplos de relaciones de uno a uno y de varios a varios, consulte la sección [otros patrones de relación](#other-relationship-patterns) al final del artículo.</span><span class="sxs-lookup"><span data-stu-id="98e3c-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="98e3c-107">Definición de términos</span><span class="sxs-lookup"><span data-stu-id="98e3c-107">Definition of Terms</span></span>

<span data-ttu-id="98e3c-108">Hay una serie de términos que se usan para describir las relaciones</span><span class="sxs-lookup"><span data-stu-id="98e3c-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="98e3c-109">**Entidad dependiente:** Esta es la entidad que contiene las propiedades de clave externa.</span><span class="sxs-lookup"><span data-stu-id="98e3c-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="98e3c-110">A veces se conoce como "secundario" de la relación.</span><span class="sxs-lookup"><span data-stu-id="98e3c-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="98e3c-111">**Entidad de entidad de seguridad:** Esta es la entidad que contiene las propiedades de clave principal/alternativa.</span><span class="sxs-lookup"><span data-stu-id="98e3c-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="98e3c-112">A veces se denomina "primario" de la relación.</span><span class="sxs-lookup"><span data-stu-id="98e3c-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="98e3c-113">**Clave externa:** Las propiedades de la entidad dependiente que se usa para almacenar los valores de la propiedad de clave principal con la que está relacionada la entidad.</span><span class="sxs-lookup"><span data-stu-id="98e3c-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="98e3c-114">**Clave principal:** Las propiedades que identifican de forma única la entidad principal.</span><span class="sxs-lookup"><span data-stu-id="98e3c-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="98e3c-115">Puede ser la clave principal o una clave alternativa.</span><span class="sxs-lookup"><span data-stu-id="98e3c-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="98e3c-116">**Propiedad de navegación:** Propiedad definida en la entidad de seguridad o dependiente que contiene una o más referencias a las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="98e3c-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="98e3c-117">**Propiedad de navegación de colección:** Propiedad de navegación que contiene referencias a muchas entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="98e3c-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="98e3c-118">**Propiedad de navegación de referencia:** Propiedad de navegación que contiene una referencia a una sola entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="98e3c-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="98e3c-119">**Propiedad de navegación inversa:** Al discutir una propiedad de navegación determinada, este término hace referencia a la propiedad de navegación en el otro extremo de la relación.</span><span class="sxs-lookup"><span data-stu-id="98e3c-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="98e3c-120">En la lista de código siguiente se muestra una relación de uno a `Blog` varios entre y`Post`</span><span class="sxs-lookup"><span data-stu-id="98e3c-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="98e3c-121">`Post`es la entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="98e3c-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="98e3c-122">`Blog`es la entidad principal</span><span class="sxs-lookup"><span data-stu-id="98e3c-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="98e3c-123">`Post.BlogId`es la clave externa</span><span class="sxs-lookup"><span data-stu-id="98e3c-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="98e3c-124">`Blog.BlogId`es la clave principal (en este caso, es una clave principal en lugar de una clave alternativa)</span><span class="sxs-lookup"><span data-stu-id="98e3c-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="98e3c-125">`Post.Blog`propiedad de navegación de referencia</span><span class="sxs-lookup"><span data-stu-id="98e3c-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="98e3c-126">`Blog.Posts`es una propiedad de navegación de colección</span><span class="sxs-lookup"><span data-stu-id="98e3c-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="98e3c-127">`Post.Blog`es la propiedad de navegación inversa de `Blog.Posts` (y viceversa).</span><span class="sxs-lookup"><span data-stu-id="98e3c-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="98e3c-128">Convenciones</span><span class="sxs-lookup"><span data-stu-id="98e3c-128">Conventions</span></span>

<span data-ttu-id="98e3c-129">Por Convención, se creará una relación cuando se detecte una propiedad de navegación en un tipo.</span><span class="sxs-lookup"><span data-stu-id="98e3c-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="98e3c-130">Una propiedad se considera una propiedad de navegación si el tipo al que señala no se puede asignar como un tipo escalar por el proveedor de base de datos actual.</span><span class="sxs-lookup"><span data-stu-id="98e3c-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="98e3c-131">Las relaciones detectadas por la Convención siempre tendrán como destino la clave principal de la entidad principal.</span><span class="sxs-lookup"><span data-stu-id="98e3c-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="98e3c-132">Para elegir como destino una clave alternativa, se debe realizar una configuración adicional mediante la API fluida.</span><span class="sxs-lookup"><span data-stu-id="98e3c-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="98e3c-133">Relaciones totalmente definidas</span><span class="sxs-lookup"><span data-stu-id="98e3c-133">Fully Defined Relationships</span></span>

<span data-ttu-id="98e3c-134">El patrón más común para las relaciones es tener propiedades de navegación definidas en ambos extremos de la relación y una propiedad de clave externa definida en la clase de entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="98e3c-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="98e3c-135">Si se encuentra un par de propiedades de navegación entre dos tipos, se configurarán como propiedades de navegación inversa de la misma relación.</span><span class="sxs-lookup"><span data-stu-id="98e3c-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="98e3c-136">Si la entidad dependiente contiene una propiedad denominada `<primary key property name>`, `<navigation property name><primary key property name>`o `<principal entity name><primary key property name>` , se configurará como la clave externa.</span><span class="sxs-lookup"><span data-stu-id="98e3c-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="98e3c-137">Si hay varias propiedades de navegación definidas entre dos tipos (es decir, más de un par de navegación distinto que apuntan entre sí), no se creará ninguna relación por Convención y tendrá que configurarlas manualmente para identificar cómo pareja de propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="98e3c-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="98e3c-138">No hay propiedad de clave externa</span><span class="sxs-lookup"><span data-stu-id="98e3c-138">No Foreign Key Property</span></span>

<span data-ttu-id="98e3c-139">Aunque se recomienda tener una propiedad de clave externa definida en la clase de entidad dependiente, no es necesario.</span><span class="sxs-lookup"><span data-stu-id="98e3c-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="98e3c-140">Si no se encuentra ninguna propiedad de clave externa, se introducirá una propiedad de clave externa de `<navigation property name><principal key property name>` sombra con el nombre (vea [propiedades de sombra](shadow-properties.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="98e3c-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="98e3c-141">Propiedad de navegación única</span><span class="sxs-lookup"><span data-stu-id="98e3c-141">Single Navigation Property</span></span>

<span data-ttu-id="98e3c-142">Incluir solo una propiedad de navegación (sin navegación inversa y sin propiedad de clave externa) es suficiente para tener una relación definida por Convención.</span><span class="sxs-lookup"><span data-stu-id="98e3c-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="98e3c-143">También puede tener una propiedad de navegación única y una propiedad de clave externa.</span><span class="sxs-lookup"><span data-stu-id="98e3c-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="98e3c-144">Eliminación en cascada</span><span class="sxs-lookup"><span data-stu-id="98e3c-144">Cascade Delete</span></span>

<span data-ttu-id="98e3c-145">Por Convención, la eliminación en cascada se establecerá en *Cascade* para las relaciones necesarias y *ClientSetNull* para las relaciones opcionales.</span><span class="sxs-lookup"><span data-stu-id="98e3c-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="98e3c-146">*Cascade* significa que las entidades dependientes también se eliminan.</span><span class="sxs-lookup"><span data-stu-id="98e3c-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="98e3c-147">*ClientSetNull* significa que las entidades dependientes que no se cargan en la memoria permanecerán sin cambios y deben eliminarse manualmente o actualizarse para que apunten a una entidad principal válida.</span><span class="sxs-lookup"><span data-stu-id="98e3c-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="98e3c-148">En el caso de las entidades que se cargan en memoria, EF Core intentará establecer las propiedades de clave externa en NULL.</span><span class="sxs-lookup"><span data-stu-id="98e3c-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="98e3c-149">Vea la sección [relaciones obligatorias y opcionales](#required-and-optional-relationships) para ver la diferencia entre las relaciones obligatorias y opcionales.</span><span class="sxs-lookup"><span data-stu-id="98e3c-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="98e3c-150">Consulte [eliminación en cascada](../saving/cascade-delete.md) para obtener más detalles sobre los distintos comportamientos de eliminación y los valores predeterminados que usa la Convención.</span><span class="sxs-lookup"><span data-stu-id="98e3c-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="98e3c-151">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="98e3c-151">Data Annotations</span></span>

<span data-ttu-id="98e3c-152">Hay dos anotaciones de datos que se pueden usar para configurar relaciones, `[ForeignKey]` y. `[InverseProperty]`</span><span class="sxs-lookup"><span data-stu-id="98e3c-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="98e3c-153">Están disponibles en el `System.ComponentModel.DataAnnotations.Schema` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="98e3c-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="98e3c-154">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="98e3c-154">[ForeignKey]</span></span>

<span data-ttu-id="98e3c-155">Puede usar las anotaciones de datos para configurar qué propiedad se debe usar como la propiedad de clave externa para una relación determinada.</span><span class="sxs-lookup"><span data-stu-id="98e3c-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="98e3c-156">Normalmente, esto se hace cuando la Convención no detecta la propiedad de clave externa.</span><span class="sxs-lookup"><span data-stu-id="98e3c-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="98e3c-157">La `[ForeignKey]` anotación puede colocarse en cualquier propiedad de navegación de la relación.</span><span class="sxs-lookup"><span data-stu-id="98e3c-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="98e3c-158">No es necesario que vaya a la propiedad de navegación en la clase de entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="98e3c-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="98e3c-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="98e3c-159">[InverseProperty]</span></span>

<span data-ttu-id="98e3c-160">Puede usar las anotaciones de datos para configurar cómo se emparejan las propiedades de navegación en las entidades de entidad de seguridad y dependencias.</span><span class="sxs-lookup"><span data-stu-id="98e3c-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="98e3c-161">Normalmente, esto se hace cuando hay más de un par de propiedades de navegación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="98e3c-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="98e3c-162">API fluida</span><span class="sxs-lookup"><span data-stu-id="98e3c-162">Fluent API</span></span>

<span data-ttu-id="98e3c-163">Para configurar una relación en la API fluida, empiece por identificar las propiedades de navegación que componen la relación.</span><span class="sxs-lookup"><span data-stu-id="98e3c-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="98e3c-164">`HasOne`o `HasMany` identifica la propiedad de navegación en el tipo de entidad en el que va a comenzar la configuración.</span><span class="sxs-lookup"><span data-stu-id="98e3c-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="98e3c-165">A continuación, encadenar `WithOne` una `WithMany` llamada a o para identificar la navegación inversa.</span><span class="sxs-lookup"><span data-stu-id="98e3c-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="98e3c-166">`HasOne`/`WithOne`se utilizan para las propiedades de navegación `HasMany` de referencia y / `WithMany` se utilizan para las propiedades de navegación de colección.</span><span class="sxs-lookup"><span data-stu-id="98e3c-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="98e3c-167">Propiedad de navegación única</span><span class="sxs-lookup"><span data-stu-id="98e3c-167">Single Navigation Property</span></span>

<span data-ttu-id="98e3c-168">Si solo tiene una propiedad de navegación, hay sobrecargas sin parámetros de `WithOne` y. `WithMany`</span><span class="sxs-lookup"><span data-stu-id="98e3c-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="98e3c-169">Esto indica que hay conceptualmente una referencia o una colección en el otro extremo de la relación, pero no hay ninguna propiedad de navegación incluida en la clase de entidad.</span><span class="sxs-lookup"><span data-stu-id="98e3c-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="98e3c-170">Clave externa</span><span class="sxs-lookup"><span data-stu-id="98e3c-170">Foreign Key</span></span>

<span data-ttu-id="98e3c-171">Puede usar la API fluida para configurar qué propiedad se debe usar como la propiedad de clave externa para una relación determinada.</span><span class="sxs-lookup"><span data-stu-id="98e3c-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="98e3c-172">En la lista de código siguiente se muestra cómo configurar una clave externa compuesta.</span><span class="sxs-lookup"><span data-stu-id="98e3c-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="98e3c-173">Puede usar la sobrecarga de cadena de `HasForeignKey(...)` para configurar una propiedad Shadow como clave externa (consulte [propiedades de sombra](shadow-properties.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="98e3c-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="98e3c-174">Se recomienda agregar explícitamente la propiedad Shadow al modelo antes de usarla como clave externa (como se muestra a continuación).</span><span class="sxs-lookup"><span data-stu-id="98e3c-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="98e3c-175">Sin propiedad de navegación</span><span class="sxs-lookup"><span data-stu-id="98e3c-175">Without Navigation Property</span></span>

<span data-ttu-id="98e3c-176">No es necesario proporcionar una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="98e3c-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="98e3c-177">Simplemente puede proporcionar una clave externa en un lado de la relación.</span><span class="sxs-lookup"><span data-stu-id="98e3c-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="98e3c-178">Clave principal</span><span class="sxs-lookup"><span data-stu-id="98e3c-178">Principal Key</span></span>

<span data-ttu-id="98e3c-179">Si desea que la clave externa haga referencia a una propiedad que no sea la clave principal, puede usar la API fluida para configurar la propiedad de clave principal de la relación.</span><span class="sxs-lookup"><span data-stu-id="98e3c-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="98e3c-180">La propiedad que configure como clave principal se configurará automáticamente como clave alternativa (consulte [claves alternativas](alternate-keys.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="98e3c-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?highlight=11)] -->
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

<span data-ttu-id="98e3c-181">En la lista de código siguiente se muestra cómo configurar una clave principal compuesta.</span><span class="sxs-lookup"><span data-stu-id="98e3c-181">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
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
> <span data-ttu-id="98e3c-182">El orden en el que se especifican las propiedades de clave principal debe coincidir con el orden en que se especifican para la clave externa.</span><span class="sxs-lookup"><span data-stu-id="98e3c-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="98e3c-183">Relaciones obligatorias y opcionales</span><span class="sxs-lookup"><span data-stu-id="98e3c-183">Required and Optional Relationships</span></span>

<span data-ttu-id="98e3c-184">Puede usar la API fluida para configurar si la relación es obligatoria u opcional.</span><span class="sxs-lookup"><span data-stu-id="98e3c-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="98e3c-185">En última instancia, controla si la propiedad de clave externa es obligatoria u opcional.</span><span class="sxs-lookup"><span data-stu-id="98e3c-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="98e3c-186">Esto es muy útil cuando se usa una clave externa de estado de sombra.</span><span class="sxs-lookup"><span data-stu-id="98e3c-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="98e3c-187">Si tiene una propiedad de clave externa en la clase de entidad, la necesidad de la relación se determina en función de si la propiedad de clave externa es necesaria u opcional (vea [propiedades obligatorias y opcionales](required-optional.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="98e3c-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?highlight=11)] -->
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

### <a name="cascade-delete"></a><span data-ttu-id="98e3c-188">Eliminación en cascada</span><span class="sxs-lookup"><span data-stu-id="98e3c-188">Cascade Delete</span></span>

<span data-ttu-id="98e3c-189">Puede usar la API fluida para configurar explícitamente el comportamiento de eliminación en cascada para una relación determinada.</span><span class="sxs-lookup"><span data-stu-id="98e3c-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="98e3c-190">Consulte [eliminación en cascada](../saving/cascade-delete.md) en la sección guardar datos para obtener una explicación detallada de cada opción.</span><span class="sxs-lookup"><span data-stu-id="98e3c-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?highlight=11)] -->
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

## <a name="other-relationship-patterns"></a><span data-ttu-id="98e3c-191">Otros patrones de relación</span><span class="sxs-lookup"><span data-stu-id="98e3c-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="98e3c-192">Uno a uno</span><span class="sxs-lookup"><span data-stu-id="98e3c-192">One-to-one</span></span>

<span data-ttu-id="98e3c-193">Una relación de uno a uno tiene una propiedad de navegación de referencia en ambos lados.</span><span class="sxs-lookup"><span data-stu-id="98e3c-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="98e3c-194">Siguen las mismas convenciones que las relaciones uno a varios, pero se incluye un índice único en la propiedad de clave externa para asegurarse de que solo un dependiente esté relacionado con cada entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="98e3c-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Relationships/OneToOne.cs?highlight=6,15,16)] -->
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
> <span data-ttu-id="98e3c-195">EF elegirá una de las entidades como dependiente en función de su capacidad para detectar una propiedad de clave externa.</span><span class="sxs-lookup"><span data-stu-id="98e3c-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="98e3c-196">Si se elige la entidad equivocada como dependiente, puede usar la API fluida para corregir este problema.</span><span class="sxs-lookup"><span data-stu-id="98e3c-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="98e3c-197">Al configurar la relación con la API fluida, se usan los `HasOne` métodos `WithOne` y.</span><span class="sxs-lookup"><span data-stu-id="98e3c-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="98e3c-198">Al configurar la clave externa, debe especificar el tipo de entidad dependiente: Observe el parámetro genérico que `HasForeignKey` se proporciona en la siguiente lista.</span><span class="sxs-lookup"><span data-stu-id="98e3c-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="98e3c-199">En una relación uno a varios, es evidente que la entidad con la navegación de referencia es el dependiente y el que tiene la colección es la entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="98e3c-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="98e3c-200">Pero esto no es así en una relación uno a uno; por lo tanto, la necesidad de definirla explícitamente.</span><span class="sxs-lookup"><span data-stu-id="98e3c-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?highlight=11)] -->
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

### <a name="many-to-many"></a><span data-ttu-id="98e3c-201">Varios a varios</span><span class="sxs-lookup"><span data-stu-id="98e3c-201">Many-to-many</span></span>

<span data-ttu-id="98e3c-202">Todavía no se admiten las relaciones de varios a varios sin una clase de entidad para representar la tabla de combinación.</span><span class="sxs-lookup"><span data-stu-id="98e3c-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="98e3c-203">Sin embargo, puede representar una relación de varios a varios incluyendo una clase de entidad para la tabla de combinación y asignando dos relaciones uno a varios independientes.</span><span class="sxs-lookup"><span data-stu-id="98e3c-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(pt => new { pt.PostId, pt.TagId });

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
