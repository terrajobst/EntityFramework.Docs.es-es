---
title: Glosario de Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
caps.latest.revision: 3
ms.openlocfilehash: cbd61838afc23dfb37cee7c624c65476c5270099
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122735"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="0fad6-102">Glosario de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0fad6-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="0fad6-103">Code First</span><span class="sxs-lookup"><span data-stu-id="0fad6-103">Code First</span></span>
<span data-ttu-id="0fad6-104">Creación de un modelo de Entity Framework mediante código.</span><span class="sxs-lookup"><span data-stu-id="0fad6-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="0fad6-105">Puede tener como destino el modelo y base de datos existente o una base de datos.</span><span class="sxs-lookup"><span data-stu-id="0fad6-105">The model can target and existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="0fad6-106">Contexto</span><span class="sxs-lookup"><span data-stu-id="0fad6-106">Context</span></span>
<span data-ttu-id="0fad6-107">Una clase que representa una sesión con la base de datos, lo que le permite consultar y guardar los datos.</span><span class="sxs-lookup"><span data-stu-id="0fad6-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="0fad6-108">Un contexto se deriva de la clase DbContext o ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="0fad6-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="0fad6-109">Convención (Code First)</span><span class="sxs-lookup"><span data-stu-id="0fad6-109">Convention (Code First)</span></span>
<span data-ttu-id="0fad6-110">Una regla que Entity Framework usa para inferir la forma del modelo desde las clases.</span><span class="sxs-lookup"><span data-stu-id="0fad6-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="0fad6-111">En primer lugar la base de datos</span><span class="sxs-lookup"><span data-stu-id="0fad6-111">Database First</span></span>
<span data-ttu-id="0fad6-112">Creación de un modelo de Entity Framework mediante el Diseñador de EF, destinado a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="0fad6-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="0fad6-113">Carga diligente</span><span class="sxs-lookup"><span data-stu-id="0fad6-113">Eager loading</span></span>
<span data-ttu-id="0fad6-114">Modelo de carga de datos relacionados en una consulta para un tipo de entidad también carga las entidades relacionadas como parte de la consulta.</span><span class="sxs-lookup"><span data-stu-id="0fad6-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="0fad6-115">Diseñador de EF</span><span class="sxs-lookup"><span data-stu-id="0fad6-115">EF Designer</span></span>
<span data-ttu-id="0fad6-116">Un diseñador visual de Visual Studio que le permite crear un modelo de Entity Framework con cuadros y líneas.</span><span class="sxs-lookup"><span data-stu-id="0fad6-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="0fad6-117">Entity</span><span class="sxs-lookup"><span data-stu-id="0fad6-117">Entity</span></span>
<span data-ttu-id="0fad6-118">Clase u objeto que representa datos de aplicación como clientes, productos y pedidos.</span><span class="sxs-lookup"><span data-stu-id="0fad6-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="0fad6-119">Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="0fad6-119">Entity Data Model</span></span>
<span data-ttu-id="0fad6-120">Un modelo que describe las entidades y las relaciones entre ellos.</span><span class="sxs-lookup"><span data-stu-id="0fad6-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="0fad6-121">EF usa el EDM para describir el modelo conceptual en el que los programas de desarrollador.</span><span class="sxs-lookup"><span data-stu-id="0fad6-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="0fad6-122">EDM se basa en el modelo de entidad relación introducido por la recuperación ante desastres. Peter Chen.</span><span class="sxs-lookup"><span data-stu-id="0fad6-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="0fad6-123">El EDM se desarrolló originalmente con el objetivo principal de convertirse en el modelo de datos comunes a través de un conjunto de tecnologías de desarrollador y servidor de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0fad6-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="0fad6-124">EDM también se usa como parte del Protocolo OData.</span><span class="sxs-lookup"><span data-stu-id="0fad6-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="0fad6-125">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="0fad6-125">Explicit loading</span></span>
<span data-ttu-id="0fad6-126">Modelo de carga de datos relacionados que se cargan los objetos relacionados mediante una llamada a una API.</span><span class="sxs-lookup"><span data-stu-id="0fad6-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0fad6-127">API fluida</span><span class="sxs-lookup"><span data-stu-id="0fad6-127">Fluent API</span></span>
<span data-ttu-id="0fad6-128">Una API que puede usarse para configurar un modelo de Code First.</span><span class="sxs-lookup"><span data-stu-id="0fad6-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="0fad6-129">Asociación de clave externa</span><span class="sxs-lookup"><span data-stu-id="0fad6-129">Foreign key association</span></span>
<span data-ttu-id="0fad6-130">Una asociación entre entidades que se incluye una propiedad que representa la clave externa en la clase de la entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="0fad6-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="0fad6-131">Por ejemplo, el producto contiene una propiedad de Id. de categoría.</span><span class="sxs-lookup"><span data-stu-id="0fad6-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="0fad6-132">Relación de identificación</span><span class="sxs-lookup"><span data-stu-id="0fad6-132">Identifying relationship</span></span>
<span data-ttu-id="0fad6-133">Relación donde la clave principal de la entidad de seguridad también forma parte de la clave principal de la entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="0fad6-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="0fad6-134">En este tipo de relación, la entidad dependiente no puede existir sin la entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="0fad6-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="0fad6-135">Asociación independiente</span><span class="sxs-lookup"><span data-stu-id="0fad6-135">Independent association</span></span>
<span data-ttu-id="0fad6-136">Una asociación entre entidades que no hay ninguna propiedad que representa la clave externa de la clase de la entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="0fad6-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="0fad6-137">Por ejemplo, una clase de producto contiene una relación con categoría pero ninguna propiedad CategoryId.</span><span class="sxs-lookup"><span data-stu-id="0fad6-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="0fad6-138">Entity Framework realiza un seguimiento del estado de la asociación con independencia del estado de las entidades en los extremos de asociación de dos.</span><span class="sxs-lookup"><span data-stu-id="0fad6-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="0fad6-139">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="0fad6-139">Lazy loading</span></span>
<span data-ttu-id="0fad6-140">Modelo de carga de datos relacionados, donde los objetos relacionados se cargan automáticamente cuando se tiene acceso a una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="0fad6-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="0fad6-141">En primer lugar del modelo</span><span class="sxs-lookup"><span data-stu-id="0fad6-141">Model First</span></span>
<span data-ttu-id="0fad6-142">Creación de un modelo de Entity Framework mediante el Diseñador de EF, que, a continuación, sirve para crear una nueva base de datos.</span><span class="sxs-lookup"><span data-stu-id="0fad6-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="0fad6-143">Propiedad de navegación</span><span class="sxs-lookup"><span data-stu-id="0fad6-143">Navigation property</span></span>
<span data-ttu-id="0fad6-144">Una propiedad de una entidad que hace referencia a otra entidad.</span><span class="sxs-lookup"><span data-stu-id="0fad6-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="0fad6-145">Por ejemplo, el producto contiene una propiedad de navegación de la categoría y categoría contiene una propiedad de navegación de productos.</span><span class="sxs-lookup"><span data-stu-id="0fad6-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="0fad6-146">POCO</span><span class="sxs-lookup"><span data-stu-id="0fad6-146">POCO</span></span>
<span data-ttu-id="0fad6-147">Acrónimo de objetos CLR antiguos sin formato.</span><span class="sxs-lookup"><span data-stu-id="0fad6-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="0fad6-148">Una clase de usuario simple que no tiene ninguna dependencia con cualquier marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="0fad6-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="0fad6-149">En el contexto de EF, un una clase de entidad que no se deriva de EntityObject, implementa las interfaces o lleva a cabo los atributos definidos en EF.</span><span class="sxs-lookup"><span data-stu-id="0fad6-149">In the context of EF, a an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="0fad6-150">Estas clases de entidad que se desconectan entre el marco de persistencia también se dice que "que ignoran la persistencia".</span><span class="sxs-lookup"><span data-stu-id="0fad6-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="0fad6-151">Inverso de relación</span><span class="sxs-lookup"><span data-stu-id="0fad6-151">Relationship inverse</span></span>
<span data-ttu-id="0fad6-152">El extremo opuesto de una relación, por ejemplo, el producto. Categoría y categoría. Producto.</span><span class="sxs-lookup"><span data-stu-id="0fad6-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="0fad6-153">Entidad con seguimiento propio</span><span class="sxs-lookup"><span data-stu-id="0fad6-153">Self-tracking entity</span></span>
<span data-ttu-id="0fad6-154">Una entidad creada desde una plantilla de generación de código que ayuda a con el desarrollo de N niveles.</span><span class="sxs-lookup"><span data-stu-id="0fad6-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="0fad6-155">Tipo de tabla por concreta (TPC)</span><span class="sxs-lookup"><span data-stu-id="0fad6-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="0fad6-156">Un método de asignación de la herencia donde cada tipo no abstracto en la jerarquía se asigna a una tabla en la base de datos independiente.</span><span class="sxs-lookup"><span data-stu-id="0fad6-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="0fad6-157">Tabla por jerarquía (TPH)</span><span class="sxs-lookup"><span data-stu-id="0fad6-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="0fad6-158">Un método de asignación de la herencia donde todos los tipos de la jerarquía se asignan a la misma tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0fad6-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="0fad6-159">Está asociado el discriminador columnas se usan para identificar qué tipo de cada fila.</span><span class="sxs-lookup"><span data-stu-id="0fad6-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="0fad6-160">Tabla por tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="0fad6-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="0fad6-161">Un método de asignación de la herencia, donde las propiedades comunes de todos los tipos de la jerarquía se asignan a la misma tabla en la base de datos, pero las propiedades únicas para cada tipo se asignan a una tabla independiente.</span><span class="sxs-lookup"><span data-stu-id="0fad6-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="0fad6-162">Detección de tipos</span><span class="sxs-lookup"><span data-stu-id="0fad6-162">Type discovery</span></span>
<span data-ttu-id="0fad6-163">El proceso de identificación de los tipos que deben formar parte de un modelo de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0fad6-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
