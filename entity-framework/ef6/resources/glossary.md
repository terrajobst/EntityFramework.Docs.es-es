---
title: Entity Framework Glosario-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656156"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="a4949-102">Entity Framework Glosario</span><span class="sxs-lookup"><span data-stu-id="a4949-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="a4949-103">Code First</span><span class="sxs-lookup"><span data-stu-id="a4949-103">Code First</span></span>
<span data-ttu-id="a4949-104">Crear un modelo de Entity Framework mediante código.</span><span class="sxs-lookup"><span data-stu-id="a4949-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="a4949-105">El modelo puede tener como destino una base de datos existente o una nueva.</span><span class="sxs-lookup"><span data-stu-id="a4949-105">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="a4949-106">Contexto</span><span class="sxs-lookup"><span data-stu-id="a4949-106">Context</span></span>
<span data-ttu-id="a4949-107">Una clase que representa una sesión con la base de datos, lo que permite consultar y guardar datos.</span><span class="sxs-lookup"><span data-stu-id="a4949-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="a4949-108">Un contexto se deriva de la clase DbContext o ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="a4949-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="a4949-109">Convención (Code First)</span><span class="sxs-lookup"><span data-stu-id="a4949-109">Convention (Code First)</span></span>
<span data-ttu-id="a4949-110">Una regla que Entity Framework usa para inferir la forma de su modelo a partir de las clases.</span><span class="sxs-lookup"><span data-stu-id="a4949-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="a4949-111">Database First</span><span class="sxs-lookup"><span data-stu-id="a4949-111">Database First</span></span>
<span data-ttu-id="a4949-112">Crear un modelo de Entity Framework, mediante el diseñador de EF, que tiene como destino una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="a4949-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="a4949-113">Carga diligente</span><span class="sxs-lookup"><span data-stu-id="a4949-113">Eager loading</span></span>
<span data-ttu-id="a4949-114">Patrón de carga de datos relacionados donde una consulta para un tipo de entidad también carga las entidades relacionadas como parte de la consulta.</span><span class="sxs-lookup"><span data-stu-id="a4949-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="a4949-115">EF Designer</span><span class="sxs-lookup"><span data-stu-id="a4949-115">EF Designer</span></span>
<span data-ttu-id="a4949-116">Diseñador visual de Visual Studio que permite crear un modelo de Entity Framework mediante cuadros y líneas.</span><span class="sxs-lookup"><span data-stu-id="a4949-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="a4949-117">Entity</span><span class="sxs-lookup"><span data-stu-id="a4949-117">Entity</span></span>
<span data-ttu-id="a4949-118">Clase u objeto que representa datos de aplicación como clientes, productos y pedidos.</span><span class="sxs-lookup"><span data-stu-id="a4949-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="a4949-119">Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="a4949-119">Entity Data Model</span></span>
<span data-ttu-id="a4949-120">Modelo que describe las entidades y las relaciones entre ellas.</span><span class="sxs-lookup"><span data-stu-id="a4949-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="a4949-121">EF usa EDM para describir el modelo conceptual en el que los programas de desarrollador.</span><span class="sxs-lookup"><span data-stu-id="a4949-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="a4949-122">EDM se basa en el modelo de relación de entidades introducido por Dr. Peter Chen.</span><span class="sxs-lookup"><span data-stu-id="a4949-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="a4949-123">El EDM se desarrolló originalmente con el objetivo principal de convertirse en el modelo de datos común en un conjunto de tecnologías de desarrollador y de servidor de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a4949-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="a4949-124">EDM también se usa como parte del protocolo OData.</span><span class="sxs-lookup"><span data-stu-id="a4949-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="a4949-125">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="a4949-125">Explicit loading</span></span>
<span data-ttu-id="a4949-126">Patrón de carga de datos relacionados en los que los objetos relacionados se cargan llamando a una API.</span><span class="sxs-lookup"><span data-stu-id="a4949-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a4949-127">API fluida</span><span class="sxs-lookup"><span data-stu-id="a4949-127">Fluent API</span></span>
<span data-ttu-id="a4949-128">Una API que se puede usar para configurar un modelo de Code First.</span><span class="sxs-lookup"><span data-stu-id="a4949-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="a4949-129">Asociación de clave externa</span><span class="sxs-lookup"><span data-stu-id="a4949-129">Foreign key association</span></span>
<span data-ttu-id="a4949-130">Asociación entre entidades donde una propiedad que representa la clave externa se incluye en la clase de la entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="a4949-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="a4949-131">Por ejemplo, Product contiene una propiedad CategoryId.</span><span class="sxs-lookup"><span data-stu-id="a4949-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="a4949-132">Relación de identificación</span><span class="sxs-lookup"><span data-stu-id="a4949-132">Identifying relationship</span></span>
<span data-ttu-id="a4949-133">Relación donde la clave principal de la entidad de seguridad también forma parte de la clave principal de la entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="a4949-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="a4949-134">En este tipo de relación, la entidad dependiente no puede existir sin la entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="a4949-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="a4949-135">Asociación independiente</span><span class="sxs-lookup"><span data-stu-id="a4949-135">Independent association</span></span>
<span data-ttu-id="a4949-136">Asociación entre entidades en las que no hay ninguna propiedad que represente la clave externa en la clase de la entidad dependiente.</span><span class="sxs-lookup"><span data-stu-id="a4949-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="a4949-137">Por ejemplo, una clase de producto contiene una relación con la categoría pero no tiene la propiedad CategoryId.</span><span class="sxs-lookup"><span data-stu-id="a4949-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="a4949-138">Entity Framework realiza un seguimiento del estado de la Asociación independientemente del estado de las entidades en los dos extremos de la asociación.</span><span class="sxs-lookup"><span data-stu-id="a4949-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="a4949-139">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="a4949-139">Lazy loading</span></span>
<span data-ttu-id="a4949-140">Patrón de carga de datos relacionados en los que los objetos relacionados se cargan automáticamente cuando se tiene acceso a una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="a4949-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="a4949-141">Model First</span><span class="sxs-lookup"><span data-stu-id="a4949-141">Model First</span></span>
<span data-ttu-id="a4949-142">Crear un modelo de Entity Framework, mediante el diseñador de EF, que se utiliza para crear una nueva base de datos.</span><span class="sxs-lookup"><span data-stu-id="a4949-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="a4949-143">Propiedad de navegación</span><span class="sxs-lookup"><span data-stu-id="a4949-143">Navigation property</span></span>
<span data-ttu-id="a4949-144">Propiedad de una entidad que hace referencia a otra entidad.</span><span class="sxs-lookup"><span data-stu-id="a4949-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="a4949-145">Por ejemplo, el producto contiene una propiedad de navegación categoría y la categoría contiene una propiedad de navegación productos.</span><span class="sxs-lookup"><span data-stu-id="a4949-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="a4949-146">POCO</span><span class="sxs-lookup"><span data-stu-id="a4949-146">POCO</span></span>
<span data-ttu-id="a4949-147">Acrónimo para el objeto CLR antiguo.</span><span class="sxs-lookup"><span data-stu-id="a4949-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="a4949-148">Una clase de usuario simple que no tiene dependencias con ningún marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="a4949-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="a4949-149">En el contexto de EF, una clase de entidad que no se deriva de EntityObject, implementa las interfaces o incluye cualquier atributo definido en EF.</span><span class="sxs-lookup"><span data-stu-id="a4949-149">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="a4949-150">Estas clases de entidad que se desacoplan del marco de persistencia también se consideran "que ignoran la persistencia".</span><span class="sxs-lookup"><span data-stu-id="a4949-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="a4949-151">Inverso de relación</span><span class="sxs-lookup"><span data-stu-id="a4949-151">Relationship inverse</span></span>
<span data-ttu-id="a4949-152">Extremo opuesto de una relación, por ejemplo, product. Categoría y categoría. Manuales.</span><span class="sxs-lookup"><span data-stu-id="a4949-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="a4949-153">Entidad de seguimiento propio</span><span class="sxs-lookup"><span data-stu-id="a4949-153">Self-tracking entity</span></span>
<span data-ttu-id="a4949-154">Una entidad creada a partir de una plantilla de generación de código que ayuda con el desarrollo de N niveles.</span><span class="sxs-lookup"><span data-stu-id="a4949-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="a4949-155">Tabla por tipo concreto (TPC)</span><span class="sxs-lookup"><span data-stu-id="a4949-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="a4949-156">Método de asignación de la herencia en la que cada tipo no abstracto de la jerarquía se asigna a una tabla independiente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a4949-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="a4949-157">Tabla por jerarquía (TPH)</span><span class="sxs-lookup"><span data-stu-id="a4949-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="a4949-158">Método de asignación de la herencia en la que todos los tipos de la jerarquía se asignan a la misma tabla de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a4949-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="a4949-159">Se utiliza una o varias columnas de discriminador para identificar a qué tipo está asociada cada fila.</span><span class="sxs-lookup"><span data-stu-id="a4949-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="a4949-160">Tabla por tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="a4949-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="a4949-161">Método de asignación de la herencia en la que las propiedades comunes de todos los tipos de la jerarquía se asignan a la misma tabla de la base de datos, pero las propiedades exclusivas de cada tipo se asignan a una tabla independiente.</span><span class="sxs-lookup"><span data-stu-id="a4949-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="a4949-162">Detección de tipos</span><span class="sxs-lookup"><span data-stu-id="a4949-162">Type discovery</span></span>
<span data-ttu-id="a4949-163">El proceso de identificar los tipos que deben formar parte de un modelo de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a4949-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
