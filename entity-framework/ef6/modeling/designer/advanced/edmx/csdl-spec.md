---
title: 'Especificación de CSDL: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415518"
---
# <a name="csdl-specification"></a><span data-ttu-id="94233-102">Especificación CSDL</span><span class="sxs-lookup"><span data-stu-id="94233-102">CSDL Specification</span></span>
<span data-ttu-id="94233-103">El lenguaje de definición de esquemas conceptuales (CSDL) es un lenguaje basado en XML que describe las entidades, las relaciones y las funciones que conforman un modelo conceptual de una aplicación controlada por datos.</span><span class="sxs-lookup"><span data-stu-id="94233-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="94233-104">Este modelo conceptual puede ser utilizado por el Entity Framework o WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="94233-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="94233-105">Los metadatos que se describen con CSDL los utiliza el Entity Framework para asignar entidades y relaciones que se definen en un modelo conceptual a un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="94233-106">Para obtener más información, vea [especificación de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) y especificación de [MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="94233-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="94233-107">CSDL es la implementación del Entity Framework del Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="94233-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="94233-108">En una aplicación Entity Framework, los metadatos del modelo conceptual se cargan desde un archivo. CSDL (escrito en CSDL) en una instancia de System. Data. Metadata. Edm. EdmItemCollection y es accesible mediante métodos en el Clase System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="94233-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="94233-109">Entity Framework utiliza los metadatos del modelo conceptual para traducir las consultas del modelo conceptual a comandos específicos del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="94233-110">El diseñador de EF almacena la información del modelo conceptual en un archivo. edmx en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="94233-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="94233-111">En el momento de la compilación, el diseñador de EF usa información en un archivo. edmx para crear el archivo. CSDL que necesita Entity Framework en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="94233-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="94233-112">Las versiones de CSDL se diferencian por los espacios de nombres XML.</span><span class="sxs-lookup"><span data-stu-id="94233-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="94233-113">Versión de CSDL</span><span class="sxs-lookup"><span data-stu-id="94233-113">CSDL Version</span></span> | <span data-ttu-id="94233-114">Espacio de nombres XML</span><span class="sxs-lookup"><span data-stu-id="94233-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="94233-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="94233-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="94233-116">CSDL V2</span><span class="sxs-lookup"><span data-stu-id="94233-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="94233-117">CSDL V3</span><span class="sxs-lookup"><span data-stu-id="94233-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="94233-118">Elemento Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-118">Association Element (CSDL)</span></span>

<span data-ttu-id="94233-119">Un elemento **Association** define una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="94233-120">Una asociación debe especificar los tipos de entidad que están implicados en la relación y el posible número de tipos de entidad en cada extremo de la relación, que se conoce como multiplicidad.</span><span class="sxs-lookup"><span data-stu-id="94233-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="94233-121">La multiplicidad de un extremo de asociación puede tener un valor de uno (1), cero o uno (0.. 1) o varios (\*).</span><span class="sxs-lookup"><span data-stu-id="94233-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="94233-122">Esta información se especifica en dos elementos End secundarios.</span><span class="sxs-lookup"><span data-stu-id="94233-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="94233-123">Es posible obtener acceso a las instancias de tipo de entidad situadas en un extremo de la asociación a través de las propiedades de navegación o las claves externas, si estas se exponen en un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="94233-124">En una aplicación, una instancia de una asociación representa una asociación concreta entre las instancias de tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="94233-125">Las instancias de la asociación se agrupan de manera lógica en un conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="94233-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="94233-126">Un elemento **Association** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-127">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-128">End (exactamente 2 elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="94233-129">ReferentialConstraint (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="94233-130">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-131">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-131">Applicable Attributes</span></span>

<span data-ttu-id="94233-132">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="94233-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="94233-133">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-133">Attribute Name</span></span> | <span data-ttu-id="94233-134">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-134">Is Required</span></span> | <span data-ttu-id="94233-135">Value</span><span class="sxs-lookup"><span data-stu-id="94233-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="94233-136">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-136">**Name**</span></span>       | <span data-ttu-id="94233-137">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-137">Yes</span></span>         | <span data-ttu-id="94233-138">El nombre de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-139">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="94233-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="94233-140">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-141">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-142">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-142">Example</span></span>

<span data-ttu-id="94233-143">En el ejemplo siguiente se muestra un elemento **Association** que define la Asociación **CustomerOrders** cuando las claves externas no se han expuesto en los tipos de entidad **Customer** y **Order** .</span><span class="sxs-lookup"><span data-stu-id="94233-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="94233-144">Los valores de **multiplicidad** de cada **extremo** de la Asociación indican que se pueden asociar muchos **pedidos** a un **cliente**, pero solo un **cliente** puede asociarse a un **pedido**.</span><span class="sxs-lookup"><span data-stu-id="94233-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="94233-145">Además, el elemento **aleliminar** indica que todos los **pedidos** relacionados con un **cliente** determinado y que se han cargado en el ObjectContext se eliminarán si se elimina el **cliente** .</span><span class="sxs-lookup"><span data-stu-id="94233-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="94233-146">En el ejemplo siguiente se muestra un elemento **Association** que define la Asociación **CustomerOrders** cuando las claves externas se han expuesto en los tipos de entidad **Customer** y **Order** .</span><span class="sxs-lookup"><span data-stu-id="94233-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="94233-147">Con las claves externas expuestas, la relación entre las entidades se administra con un elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="94233-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="94233-148">Un elemento AssociationSetMapping correspondiente no es necesario para asignar esta asociación al origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="94233-149">AssociationSet (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="94233-150">El elemento **AssociationSet** en el lenguaje de definición de esquemas conceptuales (CSDL) es un contenedor lógico para las instancias de asociación del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="94233-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="94233-151">Un conjunto de asociaciones proporciona una definición para agrupar las instancias de la asociación con objeto de que se puedan asignar a un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="94233-152">El elemento **AssociationSet** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-153">Documentación (cero o un elemento permitido)</span><span class="sxs-lookup"><span data-stu-id="94233-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="94233-154">End (exactamente dos elementos necesarios)</span><span class="sxs-lookup"><span data-stu-id="94233-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="94233-155">Elementos Annotation (cero o más elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="94233-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="94233-156">El atributo **Association** especifica el tipo de asociación que contiene un conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="94233-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="94233-157">Los conjuntos de entidades que componen los extremos de un conjunto de asociaciones se especifican con exactamente dos elementos **finales** secundarios.</span><span class="sxs-lookup"><span data-stu-id="94233-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-158">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-158">Applicable Attributes</span></span>

<span data-ttu-id="94233-159">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="94233-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="94233-160">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-160">Attribute Name</span></span>  | <span data-ttu-id="94233-161">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-161">Is Required</span></span> | <span data-ttu-id="94233-162">Value</span><span class="sxs-lookup"><span data-stu-id="94233-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-163">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-163">**Name**</span></span>        | <span data-ttu-id="94233-164">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-164">Yes</span></span>         | <span data-ttu-id="94233-165">El nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="94233-165">The name of the entity set.</span></span> <span data-ttu-id="94233-166">El valor del atributo **Name** no puede ser el mismo que el valor del atributo **Association** .</span><span class="sxs-lookup"><span data-stu-id="94233-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="94233-167">**Asociación**</span><span class="sxs-lookup"><span data-stu-id="94233-167">**Association**</span></span> | <span data-ttu-id="94233-168">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-168">Yes</span></span>         | <span data-ttu-id="94233-169">Nombre completo de la asociación cuyas instancias contiene el conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="94233-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="94233-170">La asociación debe estar en el mismo espacio de nombres que el conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="94233-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-171">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="94233-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="94233-172">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-173">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-174">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-174">Example</span></span>

<span data-ttu-id="94233-175">En el ejemplo siguiente se muestra un elemento **EntityContainer** con dos elementos **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="94233-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="94233-176">CollectionType (Elemento de CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="94233-177">El elemento **CollectionType** del lenguaje de definición de esquemas conceptuales (CSDL) especifica que un parámetro de función o un tipo de valor devuelto de función es una colección.</span><span class="sxs-lookup"><span data-stu-id="94233-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="94233-178">El elemento **CollectionType** puede ser un elemento secundario del elemento Parameter o del elemento ReturnType (function).</span><span class="sxs-lookup"><span data-stu-id="94233-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="94233-179">El tipo de colección se puede especificar mediante el atributo **Type** o uno de los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="94233-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="94233-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="94233-180">**CollectionType**</span></span>
-   <span data-ttu-id="94233-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="94233-181">ReferenceType</span></span>
-   <span data-ttu-id="94233-182">RowType</span><span class="sxs-lookup"><span data-stu-id="94233-182">RowType</span></span>
-   <span data-ttu-id="94233-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="94233-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="94233-184">Un modelo no se validará si el tipo de una colección se especifica con el atributo **Type** y un elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="94233-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="94233-185">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-185">Applicable Attributes</span></span>

<span data-ttu-id="94233-186">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="94233-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="94233-187">Tenga en cuenta que los atributos **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**y **collation** solo se aplican a las colecciones de **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="94233-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="94233-188">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-188">Attribute Name</span></span>                                                          | <span data-ttu-id="94233-189">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-189">Is Required</span></span> | <span data-ttu-id="94233-190">Value</span><span class="sxs-lookup"><span data-stu-id="94233-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-191">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="94233-191">**Type**</span></span>                                                                | <span data-ttu-id="94233-192">No</span><span class="sxs-lookup"><span data-stu-id="94233-192">No</span></span>          | <span data-ttu-id="94233-193">Tipo de la colección.</span><span class="sxs-lookup"><span data-stu-id="94233-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="94233-194">**Admisión de valores NULL**</span><span class="sxs-lookup"><span data-stu-id="94233-194">**Nullable**</span></span>                                                            | <span data-ttu-id="94233-195">No</span><span class="sxs-lookup"><span data-stu-id="94233-195">No</span></span>          | <span data-ttu-id="94233-196">**True** (el valor predeterminado) o **False**, según si la propiedad puede tener un valor nulo.</span><span class="sxs-lookup"><span data-stu-id="94233-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="94233-197">> En CSDL v1, una propiedad de tipo complejo debe tener `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="94233-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="94233-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="94233-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="94233-199">No</span><span class="sxs-lookup"><span data-stu-id="94233-199">No</span></span>          | <span data-ttu-id="94233-200">Valor predeterminado de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="94233-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="94233-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="94233-202">No</span><span class="sxs-lookup"><span data-stu-id="94233-202">No</span></span>          | <span data-ttu-id="94233-203">La longitud máxima del valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="94233-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="94233-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="94233-205">No</span><span class="sxs-lookup"><span data-stu-id="94233-205">No</span></span>          | <span data-ttu-id="94233-206">**True** o **false** , dependiendo de si el valor de la propiedad se almacenará como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="94233-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="94233-207">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="94233-207">**Precision**</span></span>                                                           | <span data-ttu-id="94233-208">No</span><span class="sxs-lookup"><span data-stu-id="94233-208">No</span></span>          | <span data-ttu-id="94233-209">Precisión del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="94233-210">**Escala**</span><span class="sxs-lookup"><span data-stu-id="94233-210">**Scale**</span></span>                                                               | <span data-ttu-id="94233-211">No</span><span class="sxs-lookup"><span data-stu-id="94233-211">No</span></span>          | <span data-ttu-id="94233-212">Escala del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="94233-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="94233-213">**SRID**</span></span>                                                                | <span data-ttu-id="94233-214">No</span><span class="sxs-lookup"><span data-stu-id="94233-214">No</span></span>          | <span data-ttu-id="94233-215">Identificador de referencia del sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="94233-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="94233-216">Solo es válido para las propiedades de los tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="94233-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="94233-217">   Para obtener más información, consulte [SRID](https://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) .</span><span class="sxs-lookup"><span data-stu-id="94233-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="94233-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="94233-218">**Unicode**</span></span>                                                             | <span data-ttu-id="94233-219">No</span><span class="sxs-lookup"><span data-stu-id="94233-219">No</span></span>          | <span data-ttu-id="94233-220">**True** o **false** , dependiendo de si el valor de la propiedad se almacenará como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="94233-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="94233-221">**Intercalación**</span><span class="sxs-lookup"><span data-stu-id="94233-221">**Collation**</span></span>                                                           | <span data-ttu-id="94233-222">No</span><span class="sxs-lookup"><span data-stu-id="94233-222">No</span></span>          | <span data-ttu-id="94233-223">Cadena que especifica la secuencia de intercalación que se usará en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="94233-224">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="94233-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="94233-225">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-226">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-227">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-227">Example</span></span>

<span data-ttu-id="94233-228">En el ejemplo siguiente se muestra una función definida por el modelo que utiliza un elemento **CollectionType** para especificar que la función devuelve una colección de tipos de entidad **Person** (tal y como se especifica con el atributo **elementType** ).</span><span class="sxs-lookup"><span data-stu-id="94233-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="94233-229">En el ejemplo siguiente se muestra una función definida por el modelo que utiliza un elemento **CollectionType** para especificar que la función devuelve una colección de filas (tal y como se especifica en el elemento **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="94233-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="94233-230">En el ejemplo siguiente se muestra una función definida por el modelo que usa el elemento **CollectionType** para especificar que la función acepta como parámetro una colección de tipos de entidad **Department** .</span><span class="sxs-lookup"><span data-stu-id="94233-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="94233-231">ComplexType (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="94233-232">Un elemento **complexType** define una estructura de datos formada por propiedades **EdmSimpleType** u otros tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="94233-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="94233-233">  Un tipo complejo puede ser una propiedad de un tipo de entidad u otro tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="94233-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="94233-234">Un tipo complejo se parece a un tipo de entidad en que también define datos.</span><span class="sxs-lookup"><span data-stu-id="94233-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="94233-235">Sin embargo, existen algunas diferencias clave entre los tipos complejos y los tipos de entidad:</span><span class="sxs-lookup"><span data-stu-id="94233-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="94233-236">Los tipos complejos no tienen identidades (o claves) y, por consiguiente, no pueden existir de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="94233-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="94233-237">Los tipos complejos solo pueden existir como propiedades de tipos de entidad u otros tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="94233-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="94233-238">Los tipos complejos no pueden participar en asociaciones.</span><span class="sxs-lookup"><span data-stu-id="94233-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="94233-239">Los extremos de una asociación no pueden ser tipos complejos y, por consiguiente, no se pueden definir propiedades de navegación para tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="94233-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="94233-240">Una propiedad de tipo complejo no puede tener un valor nulo, aunque las propiedades escalares de un tipo complejo se pueden establecer cada una con el valor nulo.</span><span class="sxs-lookup"><span data-stu-id="94233-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="94233-241">Un elemento **complexType** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-242">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-243">Property (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="94233-244">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="94233-245">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **complexType** .</span><span class="sxs-lookup"><span data-stu-id="94233-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="94233-246">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="94233-247">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-247">Is Required</span></span> | <span data-ttu-id="94233-248">Value</span><span class="sxs-lookup"><span data-stu-id="94233-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-249">Nombre</span><span class="sxs-lookup"><span data-stu-id="94233-249">Name</span></span>                                                                                                           | <span data-ttu-id="94233-250">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-250">Yes</span></span>         | <span data-ttu-id="94233-251">El nombre del tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="94233-251">The name of the complex type.</span></span> <span data-ttu-id="94233-252">El nombre de un tipo complejo no puede ser igual que el nombre de otro tipo complejo, tipo de entidad o asociación que esté dentro del ámbito del modelo.</span><span class="sxs-lookup"><span data-stu-id="94233-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="94233-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="94233-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="94233-254">No</span><span class="sxs-lookup"><span data-stu-id="94233-254">No</span></span>          | <span data-ttu-id="94233-255">El nombre de otro tipo complejo que es el tipo base del tipo complejo que se define.</span><span class="sxs-lookup"><span data-stu-id="94233-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="94233-256">> Este atributo no es aplicable en CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="94233-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="94233-257">La herencia de los tipos complejos no se admite en esa versión.</span><span class="sxs-lookup"><span data-stu-id="94233-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="94233-258">Descripción breve</span><span class="sxs-lookup"><span data-stu-id="94233-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="94233-259">No</span><span class="sxs-lookup"><span data-stu-id="94233-259">No</span></span>          | <span data-ttu-id="94233-260">**True** o **false** (valor predeterminado) en función de si el tipo complejo es un tipo abstracto.</span><span class="sxs-lookup"><span data-stu-id="94233-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="94233-261">> Este atributo no es aplicable en CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="94233-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="94233-262">Los tipos complejos de esa versión no pueden ser tipos abstractos.</span><span class="sxs-lookup"><span data-stu-id="94233-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="94233-263">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **complexType** .</span><span class="sxs-lookup"><span data-stu-id="94233-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="94233-264">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-265">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-266">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-266">Example</span></span>

<span data-ttu-id="94233-267">En el ejemplo siguiente se muestra un tipo complejo, **Address**, con las propiedades **EdmSimpleType** **StreetAddress**, **City**, **StateOrProvince**, **Country**y **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="94233-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="94233-268">Para definir la **Dirección** de tipo complejo (anterior) como una propiedad de un tipo de entidad, debe declarar el tipo de propiedad en la definición de tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="94233-269">En el ejemplo siguiente se muestra la propiedad **Address** como un tipo complejo en un tipo de entidad (**publicador**):</span><span class="sxs-lookup"><span data-stu-id="94233-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="94233-270">DefiningExpression (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="94233-271">El elemento **DefiningExpression** en el lenguaje de definición de esquemas conceptuales (CSDL) contiene una expresión Entity SQL que define una función en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="94233-272">Para fines de validación, un elemento **DefiningExpression** puede contener contenido arbitrario.</span><span class="sxs-lookup"><span data-stu-id="94233-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="94233-273">Sin embargo, Entity Framework producirá una excepción en tiempo de ejecución si un elemento **DefiningExpression** no contiene Entity SQL válidos.</span><span class="sxs-lookup"><span data-stu-id="94233-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="94233-274">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-274">Applicable Attributes</span></span>

<span data-ttu-id="94233-275">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **DefiningExpression** .</span><span class="sxs-lookup"><span data-stu-id="94233-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="94233-276">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-277">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="94233-278">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-278">Example</span></span>

<span data-ttu-id="94233-279">En el ejemplo siguiente se usa un elemento **DefiningExpression** para definir una función que devuelve el número de años transcurridos desde que se publicó un libro.</span><span class="sxs-lookup"><span data-stu-id="94233-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="94233-280">El contenido del elemento **DefiningExpression** se escribe en Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="94233-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="94233-281">Dependent (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="94233-282">El elemento **dependiente** en el lenguaje de definición de esquemas conceptuales (CSDL) es un elemento secundario del elemento ReferentialConstraint y define el extremo dependiente de una restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="94233-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="94233-283">Un elemento **ReferentialConstraint** define una funcionalidad similar a una restricción de integridad referencial en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="94233-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="94233-284">Del mismo modo que una columna (o columnas) de una tabla de base de datos puede hacer referencia a la clave principal de otra tabla, una propiedad (o propiedades) de un tipo de entidad puede hacer referencia a la clave de entidad de otro tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="94233-285">El tipo de entidad al que se hace referencia se denomina *extremo principal* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="94233-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="94233-286">El tipo de entidad que hace referencia al extremo principal se denomina *extremo dependiente* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="94233-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="94233-287">Los elementos **PropertyRef** se usan para especificar qué claves hacen referencia al extremo principal.</span><span class="sxs-lookup"><span data-stu-id="94233-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="94233-288">El elemento **dependiente** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-289">PropertyRef (uno o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="94233-290">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-291">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-291">Applicable Attributes</span></span>

<span data-ttu-id="94233-292">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **dependiente** .</span><span class="sxs-lookup"><span data-stu-id="94233-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="94233-293">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-293">Attribute Name</span></span> | <span data-ttu-id="94233-294">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-294">Is Required</span></span> | <span data-ttu-id="94233-295">Value</span><span class="sxs-lookup"><span data-stu-id="94233-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="94233-296">**Rol**</span><span class="sxs-lookup"><span data-stu-id="94233-296">**Role**</span></span>       | <span data-ttu-id="94233-297">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-297">Yes</span></span>         | <span data-ttu-id="94233-298">Nombre del tipo de entidad del extremo dependiente de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-299">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **dependiente** .</span><span class="sxs-lookup"><span data-stu-id="94233-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="94233-300">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-301">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-302">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-302">Example</span></span>

<span data-ttu-id="94233-303">En el ejemplo siguiente se muestra un elemento **ReferentialConstraint** que se usa como parte de la definición de la Asociación **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="94233-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="94233-304">La propiedad **PublisherId** del tipo de entidad **book** constituye el extremo dependiente de la restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="94233-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="94233-305">Documentation (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="94233-306">El elemento **Documentation** del lenguaje de definición de esquemas conceptuales (CSDL) se puede usar para proporcionar información sobre un objeto que se define en un elemento primario.</span><span class="sxs-lookup"><span data-stu-id="94233-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="94233-307">En un archivo. edmx, cuando el elemento **Documentation** es un elemento secundario de un elemento que aparece como un objeto en la superficie de diseño del diseñador EF (como una entidad, asociación o propiedad), el contenido del elemento **Documentation** aparecerá en la ventana **propiedades** de Visual Studio para el objeto.</span><span class="sxs-lookup"><span data-stu-id="94233-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="94233-308">El elemento **Documentation** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-309">**Summary**: breve descripción del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="94233-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="94233-310">(cero o un elemento).</span><span class="sxs-lookup"><span data-stu-id="94233-310">(zero or one element)</span></span>
-   <span data-ttu-id="94233-311">**LongDescription**: una descripción amplia del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="94233-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="94233-312">(cero o un elemento).</span><span class="sxs-lookup"><span data-stu-id="94233-312">(zero or one element)</span></span>
-   <span data-ttu-id="94233-313">Elementos de anotación.</span><span class="sxs-lookup"><span data-stu-id="94233-313">Annotation elements.</span></span> <span data-ttu-id="94233-314">(cero o más elementos).</span><span class="sxs-lookup"><span data-stu-id="94233-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-315">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-315">Applicable Attributes</span></span>

<span data-ttu-id="94233-316">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **Documentation** .</span><span class="sxs-lookup"><span data-stu-id="94233-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="94233-317">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-318">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="94233-319">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-319">Example</span></span>

<span data-ttu-id="94233-320">En el ejemplo siguiente se muestra el elemento **Documentation** como un elemento secundario de un elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="94233-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="94233-321">Si el fragmento de código siguiente estaba en el contenido CSDL de un archivo. edmx, el contenido de los elementos **Summary** y **longDescription** aparecerá en la ventana **propiedades** de Visual Studio al hacer clic en el tipo de entidad `Customer`.</span><span class="sxs-lookup"><span data-stu-id="94233-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="94233-322">End (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-322">End Element (CSDL)</span></span>

<span data-ttu-id="94233-323">El elemento **End** en el lenguaje de definición de esquemas conceptuales (CSDL) puede ser un elemento secundario del elemento Association o del elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="94233-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="94233-324">En cada caso, el rol del elemento **final** es diferente y los atributos correspondientes son diferentes.</span><span class="sxs-lookup"><span data-stu-id="94233-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="94233-325">Elemento End como elemento secundario del elemento Association</span><span class="sxs-lookup"><span data-stu-id="94233-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="94233-326">Un elemento **End** (como elemento secundario del elemento **Association** ) identifica el tipo de entidad en un extremo de una asociación y el número de instancias de tipo de entidad que pueden existir en ese extremo de una asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="94233-327">Los extremos de asociación se definen como parte de una asociación, y esta debe tener exactamente dos extremos.</span><span class="sxs-lookup"><span data-stu-id="94233-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="94233-328">Es posible obtener acceso a las instancias de tipo de entidad situadas en un extremo de la asociación a través de las propiedades de navegación o las claves externas si estas se exponen en un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="94233-329">Un elemento **End** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-330">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-331">Aleliminar (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="94233-332">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="94233-333">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-333">Applicable Attributes</span></span>

<span data-ttu-id="94233-334">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **End** cuando es el elemento secundario de un elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="94233-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="94233-335">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-335">Attribute Name</span></span>   | <span data-ttu-id="94233-336">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-336">Is Required</span></span> | <span data-ttu-id="94233-337">Value</span><span class="sxs-lookup"><span data-stu-id="94233-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-338">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="94233-338">**Type**</span></span>         | <span data-ttu-id="94233-339">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-339">Yes</span></span>         | <span data-ttu-id="94233-340">Nombre del tipo de entidad de un extremo de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="94233-341">**Rol**</span><span class="sxs-lookup"><span data-stu-id="94233-341">**Role**</span></span>         | <span data-ttu-id="94233-342">No</span><span class="sxs-lookup"><span data-stu-id="94233-342">No</span></span>          | <span data-ttu-id="94233-343">Nombre para el extremo de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-343">A name for the association end.</span></span> <span data-ttu-id="94233-344">Si no se proporciona ningún nombre, se usará el nombre del tipo de entidad del extremo de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="94233-345">**Multiplicidad**</span><span class="sxs-lookup"><span data-stu-id="94233-345">**Multiplicity**</span></span> | <span data-ttu-id="94233-346">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-346">Yes</span></span>         | <span data-ttu-id="94233-347">**1**, **0.. 1**o **\*** en función del número de instancias de tipo de entidad que pueden estar al final de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="94233-348">**1** indica que existe exactamente una instancia de tipo de entidad en el extremo de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="94233-349">**0.. 1** indica que hay cero o una instancia de tipo de entidad en el extremo de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="94233-350">**\*** indica que hay cero, una o más instancias de tipo de entidad en el extremo de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-351">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **final** .</span><span class="sxs-lookup"><span data-stu-id="94233-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="94233-352">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-353">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="94233-354">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-354">Example</span></span>

<span data-ttu-id="94233-355">En el ejemplo siguiente se muestra un elemento **Association** que define la Asociación **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="94233-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="94233-356">Los valores de **multiplicidad** de cada **extremo** de la Asociación indican que se pueden asociar muchos **pedidos** a un **cliente**, pero solo un **cliente** puede asociarse a un **pedido**.</span><span class="sxs-lookup"><span data-stu-id="94233-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="94233-357">Además, el elemento **aleliminar** indica que todos los **pedidos** relacionados con un **cliente** determinado y que se han cargado en el ObjectContext se eliminarán si se elimina el **cliente** .</span><span class="sxs-lookup"><span data-stu-id="94233-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="94233-358">Elemento End como elemento secundario del elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="94233-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="94233-359">El elemento **End** especifica un extremo de un conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="94233-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="94233-360">El elemento **AssociationSet** debe contener dos elementos **End** .</span><span class="sxs-lookup"><span data-stu-id="94233-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="94233-361">La información contenida en un elemento **End** se utiliza para asignar un conjunto de asociaciones a un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="94233-362">Un elemento **End** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-363">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-364">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="94233-365">Los elementos de anotación deben aparecer después de todos los demás elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="94233-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="94233-366">Los elementos Annotation solo se permiten en CSDL V2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="94233-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="94233-367">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-367">Applicable Attributes</span></span>

<span data-ttu-id="94233-368">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **End** cuando es el elemento secundario de un elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="94233-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="94233-369">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-369">Attribute Name</span></span> | <span data-ttu-id="94233-370">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-370">Is Required</span></span> | <span data-ttu-id="94233-371">Value</span><span class="sxs-lookup"><span data-stu-id="94233-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-372">**#A4**</span><span class="sxs-lookup"><span data-stu-id="94233-372">**EntitySet**</span></span>  | <span data-ttu-id="94233-373">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-373">Yes</span></span>         | <span data-ttu-id="94233-374">Nombre del elemento **EntitySet** que define un extremo del elemento primario **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="94233-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="94233-375">El elemento **EntitySet** debe definirse en el mismo contenedor de entidades que el elemento primario **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="94233-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="94233-376">**Rol**</span><span class="sxs-lookup"><span data-stu-id="94233-376">**Role**</span></span>       | <span data-ttu-id="94233-377">No</span><span class="sxs-lookup"><span data-stu-id="94233-377">No</span></span>          | <span data-ttu-id="94233-378">Nombre del extremo del conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="94233-378">The name of the association set end.</span></span> <span data-ttu-id="94233-379">Si no se utiliza el atributo **role** , el nombre del extremo del conjunto de asociaciones será el nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="94233-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="94233-380">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **final** .</span><span class="sxs-lookup"><span data-stu-id="94233-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="94233-381">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-382">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="94233-383">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-383">Example</span></span>

<span data-ttu-id="94233-384">En el ejemplo siguiente se muestra un elemento **EntityContainer** con dos elementos **AssociationSet** , cada uno con dos elementos **End** :</span><span class="sxs-lookup"><span data-stu-id="94233-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="94233-385">Elemento EntityContainer (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="94233-386">El elemento **EntityContainer** en el lenguaje de definición de esquemas conceptuales (CSDL) es un contenedor lógico para conjuntos de entidades, conjuntos de asociaciones e importaciones de funciones.</span><span class="sxs-lookup"><span data-stu-id="94233-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="94233-387">Un contenedor de entidades de modelo conceptual se asigna a un contenedor de entidades de modelo de almacenamiento a través del elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="94233-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="94233-388">Un contenedor de entidades de modelo de almacenamiento describe la estructura de la base de datos: los conjuntos de entidades describen las tablas, los conjuntos de asociaciones describen las restricciones de clave externa y las importaciones de función describen los procedimientos almacenados en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="94233-389">Un elemento **EntityContainer** puede tener cero o un elemento de documentación.</span><span class="sxs-lookup"><span data-stu-id="94233-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="94233-390">Si hay un elemento de **documentación** , debe preceder a todos los elementos **EntitySet**, **AssociationSet**y **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="94233-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="94233-391">Un elemento **EntityContainer** puede tener cero o más de los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="94233-392">EntitySet</span></span>
-   <span data-ttu-id="94233-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="94233-393">AssociationSet</span></span>
-   <span data-ttu-id="94233-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="94233-394">FunctionImport</span></span>
-   <span data-ttu-id="94233-395">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="94233-395">Annotation elements</span></span>

<span data-ttu-id="94233-396">Puede extender un elemento **EntityContainer** para incluir el contenido de otro **EntityContainer** que esté en el mismo espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="94233-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="94233-397">Para incluir el contenido de otro **EntityContainer**, en el elemento **EntityContainer** de referencia, establezca el valor del atributo **extends** en el nombre del elemento **EntityContainer** que desea incluir.</span><span class="sxs-lookup"><span data-stu-id="94233-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="94233-398">Todos los elementos secundarios del elemento **EntityContainer** incluido se tratarán como elementos secundarios del elemento **EntityContainer** que hace la referencia.</span><span class="sxs-lookup"><span data-stu-id="94233-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-399">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-399">Applicable Attributes</span></span>

<span data-ttu-id="94233-400">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **using** .</span><span class="sxs-lookup"><span data-stu-id="94233-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="94233-401">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-401">Attribute Name</span></span> | <span data-ttu-id="94233-402">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-402">Is Required</span></span> | <span data-ttu-id="94233-403">Value</span><span class="sxs-lookup"><span data-stu-id="94233-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="94233-404">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-404">**Name**</span></span>       | <span data-ttu-id="94233-405">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-405">Yes</span></span>         | <span data-ttu-id="94233-406">Nombre del contenedor de entidades.</span><span class="sxs-lookup"><span data-stu-id="94233-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="94233-407">**Llegar**</span><span class="sxs-lookup"><span data-stu-id="94233-407">**Extends**</span></span>    | <span data-ttu-id="94233-408">No</span><span class="sxs-lookup"><span data-stu-id="94233-408">No</span></span>          | <span data-ttu-id="94233-409">Nombre de otro contenedor de entidades dentro del mismo espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="94233-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-410">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="94233-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="94233-411">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-412">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-413">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-413">Example</span></span>

<span data-ttu-id="94233-414">En el ejemplo siguiente se muestra un elemento **EntityContainer** que define tres conjuntos de entidades y dos conjuntos de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="94233-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="94233-415">EntitySet (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="94233-416">El elemento **EntitySet** del lenguaje de definición de esquemas conceptuales es un contenedor lógico para las instancias de un tipo de entidad y las instancias de cualquier tipo que se deriva de ese tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="94233-417">La relación entre un tipo de entidad y un conjunto de entidades es análoga a la relación entre una fila y una tabla en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="94233-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="94233-418">Igual que una fila, un tipo de entidad define un conjunto de datos relacionados y, lo mismo que una tabla, un conjunto de entidades contiene instancias de esa definición.</span><span class="sxs-lookup"><span data-stu-id="94233-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="94233-419">Un conjunto de entidades proporciona una construcción para agrupar las instancias del tipo de entidad para que se pueden asignar a las estructuras de datos relacionadas en un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="94233-420">Se pueden definir varios conjuntos de entidades para un tipo de entidad determinado.</span><span class="sxs-lookup"><span data-stu-id="94233-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="94233-421">El diseñador de EF no es compatible con los modelos conceptuales que contienen varios conjuntos de entidades por tipo.</span><span class="sxs-lookup"><span data-stu-id="94233-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="94233-422">El elemento **EntitySet** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-423">Elemento Documentation (cero o un elemento permitidos)</span><span class="sxs-lookup"><span data-stu-id="94233-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="94233-424">Elementos Annotation (cero o más elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="94233-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-425">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-425">Applicable Attributes</span></span>

<span data-ttu-id="94233-426">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="94233-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="94233-427">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-427">Attribute Name</span></span> | <span data-ttu-id="94233-428">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-428">Is Required</span></span> | <span data-ttu-id="94233-429">Value</span><span class="sxs-lookup"><span data-stu-id="94233-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-430">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-430">**Name**</span></span>       | <span data-ttu-id="94233-431">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-431">Yes</span></span>         | <span data-ttu-id="94233-432">El nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="94233-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="94233-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="94233-433">**EntityType**</span></span> | <span data-ttu-id="94233-434">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-434">Yes</span></span>         | <span data-ttu-id="94233-435">El nombre completo del tipo de entidad para el que el conjunto de entidades contiene las instancias.</span><span class="sxs-lookup"><span data-stu-id="94233-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-436">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="94233-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="94233-437">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-438">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-439">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-439">Example</span></span>

<span data-ttu-id="94233-440">En el ejemplo siguiente se muestra un elemento **EntityContainer** con tres elementos **EntitySet** :</span><span class="sxs-lookup"><span data-stu-id="94233-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

<span data-ttu-id="94233-441">Es posible definir varios conjuntos de entidades por tipo (MEST).</span><span class="sxs-lookup"><span data-stu-id="94233-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="94233-442">En el ejemplo siguiente se define un contenedor de entidades con dos conjuntos de entidades para el tipo de entidad **book** :</span><span class="sxs-lookup"><span data-stu-id="94233-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="94233-443">Elemento EntityType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="94233-444">El elemento **EntityType** representa la estructura de un concepto de nivel superior, como un cliente o un pedido, en un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="94233-445">Un tipo de entidad es una plantilla para las instancias de los tipos de entidad de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="94233-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="94233-446">Cada plantilla contiene la información siguiente:</span><span class="sxs-lookup"><span data-stu-id="94233-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="94233-447">Un nombre único.</span><span class="sxs-lookup"><span data-stu-id="94233-447">A unique name.</span></span> <span data-ttu-id="94233-448">(Requerido)</span><span class="sxs-lookup"><span data-stu-id="94233-448">(Required.)</span></span>
-   <span data-ttu-id="94233-449">Una clave de entidad definida por una o varias propiedades.</span><span class="sxs-lookup"><span data-stu-id="94233-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="94233-450">(Requerido)</span><span class="sxs-lookup"><span data-stu-id="94233-450">(Required.)</span></span>
-   <span data-ttu-id="94233-451">Propiedades para el almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-451">Properties for containing data.</span></span> <span data-ttu-id="94233-452">(Opcional).</span><span class="sxs-lookup"><span data-stu-id="94233-452">(Optional.)</span></span>
-   <span data-ttu-id="94233-453">Propiedades de navegación que permiten desplazarse de un extremo de la asociación al otro.</span><span class="sxs-lookup"><span data-stu-id="94233-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="94233-454">(Opcional).</span><span class="sxs-lookup"><span data-stu-id="94233-454">(Optional.)</span></span>

<span data-ttu-id="94233-455">En una aplicación, una instancia de un tipo de entidad representa un objeto específico (como un cliente o un pedido concreto).</span><span class="sxs-lookup"><span data-stu-id="94233-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="94233-456">Cada instancia de un tipo de entidad debe tener una clave de entidad única dentro de un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="94233-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="94233-457">Dos instancias de tipo de entidad se consideran iguales solo si son del mismo tipo y los valores de sus claves de entidad son idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="94233-458">Un elemento **EntityType** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-459">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-460">Key (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="94233-461">Property (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="94233-462">NavigationProperty (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="94233-463">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-464">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-464">Applicable Attributes</span></span>

<span data-ttu-id="94233-465">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="94233-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="94233-466">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="94233-467">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-467">Is Required</span></span> | <span data-ttu-id="94233-468">Value</span><span class="sxs-lookup"><span data-stu-id="94233-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-469">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="94233-470">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-470">Yes</span></span>         | <span data-ttu-id="94233-471">El nombre del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="94233-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="94233-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="94233-473">No</span><span class="sxs-lookup"><span data-stu-id="94233-473">No</span></span>          | <span data-ttu-id="94233-474">El nombre de otro tipo de entidad que sea el tipo base del tipo de entidad que se define.</span><span class="sxs-lookup"><span data-stu-id="94233-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="94233-475">**Descripción**</span><span class="sxs-lookup"><span data-stu-id="94233-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="94233-476">No</span><span class="sxs-lookup"><span data-stu-id="94233-476">No</span></span>          | <span data-ttu-id="94233-477">**True** o **false**, dependiendo de si el tipo de entidad es un tipo abstracto.</span><span class="sxs-lookup"><span data-stu-id="94233-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="94233-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="94233-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="94233-479">No</span><span class="sxs-lookup"><span data-stu-id="94233-479">No</span></span>          | <span data-ttu-id="94233-480">**True** o **false** , dependiendo de si el tipo de entidad es un tipo de entidad abierto.</span><span class="sxs-lookup"><span data-stu-id="94233-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="94233-481">> El atributo **OpenType** solo es aplicable a los tipos de entidad que se definen en los modelos conceptuales que se usan con el Data Services ADO.net.</span><span class="sxs-lookup"><span data-stu-id="94233-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="94233-482">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="94233-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="94233-483">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-484">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-485">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-485">Example</span></span>

<span data-ttu-id="94233-486">En el ejemplo siguiente se muestra un elemento **EntityType** con tres elementos **Property** y dos elementos **NavigationProperty** :</span><span class="sxs-lookup"><span data-stu-id="94233-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="94233-487">Elemento EnumType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="94233-488">El elemento **enumType** representa un tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="94233-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="94233-489">Un elemento **enumType** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-490">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-491">Miembro (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="94233-492">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-493">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-493">Applicable Attributes</span></span>

<span data-ttu-id="94233-494">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **enumType** .</span><span class="sxs-lookup"><span data-stu-id="94233-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="94233-495">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-495">Attribute Name</span></span>     | <span data-ttu-id="94233-496">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-496">Is Required</span></span> | <span data-ttu-id="94233-497">Value</span><span class="sxs-lookup"><span data-stu-id="94233-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-498">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-498">**Name**</span></span>           | <span data-ttu-id="94233-499">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-499">Yes</span></span>         | <span data-ttu-id="94233-500">El nombre del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="94233-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="94233-501">**IsFlags**</span></span>        | <span data-ttu-id="94233-502">No</span><span class="sxs-lookup"><span data-stu-id="94233-502">No</span></span>          | <span data-ttu-id="94233-503">**True** o **false**, dependiendo de si el tipo de enumeración se puede usar como un conjunto de marcas.</span><span class="sxs-lookup"><span data-stu-id="94233-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="94233-504">El valor predeterminado es **false.**</span><span class="sxs-lookup"><span data-stu-id="94233-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="94233-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="94233-505">**UnderlyingType**</span></span> | <span data-ttu-id="94233-506">No</span><span class="sxs-lookup"><span data-stu-id="94233-506">No</span></span>          | <span data-ttu-id="94233-507">**EDM. Byte**, **EDM. Int16**, **EDM. Int32**, **EDM. Int64** o **EDM. SByte** que define el intervalo de valores del tipo.</span><span class="sxs-lookup"><span data-stu-id="94233-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="94233-508">  El tipo subyacente predeterminado de los elementos de enumeración es **EDM. Int32.** .</span><span class="sxs-lookup"><span data-stu-id="94233-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-509">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **enumType** .</span><span class="sxs-lookup"><span data-stu-id="94233-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="94233-510">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-511">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-512">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-512">Example</span></span>

<span data-ttu-id="94233-513">En el ejemplo siguiente se muestra un elemento **enumType** con tres elementos **member** :</span><span class="sxs-lookup"><span data-stu-id="94233-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="94233-514">Function (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-514">Function Element (CSDL)</span></span>

<span data-ttu-id="94233-515">El elemento **function** en el lenguaje de definición de esquemas conceptuales (CSDL) se utiliza para definir o declarar funciones en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="94233-516">Una función se define utilizando un elemento DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="94233-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="94233-517">Un elemento **function** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-518">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-519">Parameter (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="94233-520">DefiningExpression (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="94233-521">ReturnType (función) (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="94233-522">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="94233-523">Un tipo de valor devuelto para una función debe especificarse con el elemento **ReturnType** (function) o el atributo **ReturnType** (consulte a continuación), pero no ambos.</span><span class="sxs-lookup"><span data-stu-id="94233-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="94233-524">Los tipos de valores devueltos posibles son EdmSimpleType, tipo de entidad, tipo complejo, tipo de fila o tipo ref (o una colección de uno de estos tipos).</span><span class="sxs-lookup"><span data-stu-id="94233-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-525">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-525">Applicable Attributes</span></span>

<span data-ttu-id="94233-526">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento de **función** .</span><span class="sxs-lookup"><span data-stu-id="94233-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="94233-527">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-527">Attribute Name</span></span> | <span data-ttu-id="94233-528">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-528">Is Required</span></span> | <span data-ttu-id="94233-529">Value</span><span class="sxs-lookup"><span data-stu-id="94233-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="94233-530">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-530">**Name**</span></span>       | <span data-ttu-id="94233-531">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-531">Yes</span></span>         | <span data-ttu-id="94233-532">El nombre de la función.</span><span class="sxs-lookup"><span data-stu-id="94233-532">The name of the function.</span></span>          |
| <span data-ttu-id="94233-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="94233-533">**ReturnType**</span></span> | <span data-ttu-id="94233-534">No</span><span class="sxs-lookup"><span data-stu-id="94233-534">No</span></span>          | <span data-ttu-id="94233-535">El tipo devuelto por la función.</span><span class="sxs-lookup"><span data-stu-id="94233-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-536">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento de **función** .</span><span class="sxs-lookup"><span data-stu-id="94233-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="94233-537">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-538">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-539">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-539">Example</span></span>

<span data-ttu-id="94233-540">En el ejemplo siguiente se usa un elemento **function** para definir una función que devuelve el número de años transcurridos desde que se contrató a un instructor.</span><span class="sxs-lookup"><span data-stu-id="94233-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="94233-541">FunctionImport (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="94233-542">El elemento **FunctionImport** del lenguaje de definición de esquemas conceptuales (CSDL) representa una función que se define en el origen de datos, pero que está disponible para los objetos a través del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="94233-543">Por ejemplo, un elemento Function del modelo de almacenamiento se puede usar para representar un procedimiento almacenado en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="94233-544">Un elemento **FunctionImport** del modelo conceptual representa la función correspondiente en una aplicación Entity Framework y se asigna a la función de modelo de almacenamiento mediante el elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="94233-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="94233-545">Cuando se llama a la función en la aplicación, el procedimiento almacenado correspondiente se ejecuta en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="94233-546">El elemento **FunctionImport** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-547">Documentación (cero o un elemento permitido)</span><span class="sxs-lookup"><span data-stu-id="94233-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="94233-548">Parameter (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="94233-549">Elementos Annotation (cero o más elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="94233-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="94233-550">ReturnType (FunctionImport) (cero o más elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="94233-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="94233-551">Debe definirse un elemento de **parámetro** para cada parámetro aceptado por la función.</span><span class="sxs-lookup"><span data-stu-id="94233-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="94233-552">Un tipo de valor devuelto para una función debe especificarse con el elemento **ReturnType** (FunctionImport) o el atributo **ReturnType** (consulte a continuación), pero no ambos.</span><span class="sxs-lookup"><span data-stu-id="94233-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="94233-553">El valor de tipo devuelto debe ser una colección de EdmSimpleType, EntityType o ComplexType.</span><span class="sxs-lookup"><span data-stu-id="94233-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-554">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-554">Applicable Attributes</span></span>

<span data-ttu-id="94233-555">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="94233-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="94233-556">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-556">Attribute Name</span></span>   | <span data-ttu-id="94233-557">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-557">Is Required</span></span> | <span data-ttu-id="94233-558">Value</span><span class="sxs-lookup"><span data-stu-id="94233-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-559">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-559">**Name**</span></span>         | <span data-ttu-id="94233-560">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-560">Yes</span></span>         | <span data-ttu-id="94233-561">Nombre de la función importada.</span><span class="sxs-lookup"><span data-stu-id="94233-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="94233-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="94233-562">**ReturnType**</span></span>   | <span data-ttu-id="94233-563">No</span><span class="sxs-lookup"><span data-stu-id="94233-563">No</span></span>          | <span data-ttu-id="94233-564">Tipo devuelto por la función.</span><span class="sxs-lookup"><span data-stu-id="94233-564">The type that the function returns.</span></span> <span data-ttu-id="94233-565">No utilice este atributo si la función no devuelve un valor.</span><span class="sxs-lookup"><span data-stu-id="94233-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="94233-566">De lo contrario, el valor debe ser una colección de ComplexType, EntityType o EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="94233-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="94233-567">**#A4**</span><span class="sxs-lookup"><span data-stu-id="94233-567">**EntitySet**</span></span>    | <span data-ttu-id="94233-568">No</span><span class="sxs-lookup"><span data-stu-id="94233-568">No</span></span>          | <span data-ttu-id="94233-569">Si la función devuelve una colección de tipos de entidad, el valor de **EntitySet** debe ser el conjunto de entidades al que pertenece la colección.</span><span class="sxs-lookup"><span data-stu-id="94233-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="94233-570">De lo contrario, el atributo **EntitySet** no debe usarse.</span><span class="sxs-lookup"><span data-stu-id="94233-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="94233-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="94233-571">**IsComposable**</span></span> | <span data-ttu-id="94233-572">No</span><span class="sxs-lookup"><span data-stu-id="94233-572">No</span></span>          | <span data-ttu-id="94233-573">Si el valor se establece en true, la función es ajustable (función con valores de tabla) y se puede usar en una consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="94233-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="94233-574">  El valor predeterminado es **false**.</span><span class="sxs-lookup"><span data-stu-id="94233-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="94233-575">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="94233-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="94233-576">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-577">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-578">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-578">Example</span></span>

<span data-ttu-id="94233-579">En el ejemplo siguiente se muestra un elemento **FunctionImport** que acepta un parámetro y devuelve una colección de tipos de entidad:</span><span class="sxs-lookup"><span data-stu-id="94233-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="94233-580">Key (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-580">Key Element (CSDL)</span></span>

<span data-ttu-id="94233-581">El elemento **key** es un elemento secundario del elemento EntityType y define una *clave de entidad* (una propiedad o un conjunto de propiedades de un tipo de entidad que determinan la identidad).</span><span class="sxs-lookup"><span data-stu-id="94233-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="94233-582">Las propiedades que constituyen una entidad se eligen en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="94233-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="94233-583">Los valores de las propiedades de clave de entidad deben identificar de forma inequívoca en tiempo de ejecución una instancia de tipo de entidad dentro de un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="94233-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="94233-584">Las propiedades que constituyen una clave de entidad se deben elegir de tal forma que garanticen la unicidad de las instancias de un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="94233-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="94233-585">El elemento **key** define una clave de entidad haciendo referencia a una o más de las propiedades de un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="94233-586">El elemento **key** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="94233-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="94233-587">PropertyRef (uno o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="94233-588">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-589">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-589">Applicable Attributes</span></span>

<span data-ttu-id="94233-590">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **key** .</span><span class="sxs-lookup"><span data-stu-id="94233-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="94233-591">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-592">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="94233-593">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-593">Example</span></span>

<span data-ttu-id="94233-594">En el ejemplo siguiente se define un tipo de entidad denominado **book**.</span><span class="sxs-lookup"><span data-stu-id="94233-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="94233-595">La clave de entidad se define haciendo referencia a la propiedad **ISBN** del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="94233-596">La propiedad **ISBN** es una buena elección para la clave de entidad porque un número de libro estándar internacional (ISBN) identifica de forma única un libro.</span><span class="sxs-lookup"><span data-stu-id="94233-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="94233-597">En el ejemplo siguiente se muestra un tipo de entidad (**autor**) que tiene una clave de entidad que consta de dos propiedades, **nombre** y **Dirección**.</span><span class="sxs-lookup"><span data-stu-id="94233-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

<span data-ttu-id="94233-598">Usar el **nombre** y la **Dirección** de la clave de entidad es una opción razonable, ya que es poco probable que dos autores del mismo nombre se encuentren en la misma dirección.</span><span class="sxs-lookup"><span data-stu-id="94233-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="94233-599">Sin embargo, esta opción no garantiza por completo la existencia de claves de entidad únicas en un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="94233-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="94233-600">En este caso, se recomienda agregar una propiedad, como **AuthorId**, que pueda usarse para identificar de forma única un autor.</span><span class="sxs-lookup"><span data-stu-id="94233-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="94233-601">Elemento Member (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-601">Member Element (CSDL)</span></span>

<span data-ttu-id="94233-602">El elemento **member** es un elemento secundario del elemento enumType y define un miembro del tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="94233-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-603">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-603">Applicable Attributes</span></span>

<span data-ttu-id="94233-604">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="94233-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="94233-605">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-605">Attribute Name</span></span> | <span data-ttu-id="94233-606">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-606">Is Required</span></span> | <span data-ttu-id="94233-607">Value</span><span class="sxs-lookup"><span data-stu-id="94233-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-608">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-608">**Name**</span></span>       | <span data-ttu-id="94233-609">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-609">Yes</span></span>         | <span data-ttu-id="94233-610">Nombre del miembro.</span><span class="sxs-lookup"><span data-stu-id="94233-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="94233-611">**Valor**</span><span class="sxs-lookup"><span data-stu-id="94233-611">**Value**</span></span>      | <span data-ttu-id="94233-612">No</span><span class="sxs-lookup"><span data-stu-id="94233-612">No</span></span>          | <span data-ttu-id="94233-613">Valor del miembro.</span><span class="sxs-lookup"><span data-stu-id="94233-613">The value of the member.</span></span> <span data-ttu-id="94233-614">De forma predeterminada, el primer miembro tiene el valor 0 y el valor de cada enumerador sucesivo se incrementa en 1.</span><span class="sxs-lookup"><span data-stu-id="94233-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="94233-615">Pueden existir varios miembros con los mismos valores.</span><span class="sxs-lookup"><span data-stu-id="94233-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-616">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="94233-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="94233-617">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-618">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-619">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-619">Example</span></span>

<span data-ttu-id="94233-620">En el ejemplo siguiente se muestra un elemento **enumType** con tres elementos **member** :</span><span class="sxs-lookup"><span data-stu-id="94233-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="94233-621">Elemento NavigationProperty (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="94233-622">Un elemento **NavigationProperty** define una propiedad de navegación, que proporciona una referencia al otro extremo de una asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="94233-623">A diferencia de las propiedades definidas con el elemento Property, las propiedades de navegación no definen la forma ni las características de los datos.</span><span class="sxs-lookup"><span data-stu-id="94233-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="94233-624">Proporcionan una manera de desplazarse por una asociación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="94233-625">Las propiedades de navegación son opcionales en los dos tipos de entidad de los extremos de una asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="94233-626">Si define una propiedad de navegación en un tipo de entidad del extremo de una asociación, no tiene que definir una propiedad de navegación en el tipo de entidad del otro extremo de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="94233-627">El tipo de datos devuelto por una propiedad de navegación viene determinado por la multiplicidad de su extremo remoto de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="94233-628">Por ejemplo, supongamos que existe una propiedad de navegación, **OrdersNavProp**, en un tipo de entidad **Customer** y navega por una asociación uno a varios entre **Customer** y **Order**.</span><span class="sxs-lookup"><span data-stu-id="94233-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="94233-629">Dado que el extremo remoto de la Asociación para la propiedad de navegación tiene una multiplicidad de muchos (\*), su tipo de datos es una colección (de **orden**).</span><span class="sxs-lookup"><span data-stu-id="94233-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="94233-630">Del mismo modo, si una propiedad de navegación, **CustomerNavProp**, existe en el tipo de entidad **Order** , su tipo de datos sería **Customer** , ya que la multiplicidad del extremo remoto es uno (1).</span><span class="sxs-lookup"><span data-stu-id="94233-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="94233-631">Un elemento **NavigationProperty** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-632">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-633">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-634">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-634">Applicable Attributes</span></span>

<span data-ttu-id="94233-635">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="94233-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="94233-636">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-636">Attribute Name</span></span>   | <span data-ttu-id="94233-637">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-637">Is Required</span></span> | <span data-ttu-id="94233-638">Value</span><span class="sxs-lookup"><span data-stu-id="94233-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-639">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-639">**Name**</span></span>         | <span data-ttu-id="94233-640">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-640">Yes</span></span>         | <span data-ttu-id="94233-641">El nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="94233-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="94233-642">**Vínculo**</span><span class="sxs-lookup"><span data-stu-id="94233-642">**Relationship**</span></span> | <span data-ttu-id="94233-643">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-643">Yes</span></span>         | <span data-ttu-id="94233-644">Nombre de una asociación que se encuentra dentro del ámbito del modelo.</span><span class="sxs-lookup"><span data-stu-id="94233-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="94233-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="94233-645">**ToRole**</span></span>       | <span data-ttu-id="94233-646">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-646">Yes</span></span>         | <span data-ttu-id="94233-647">Extremo de la asociación en el que finaliza la navegación.</span><span class="sxs-lookup"><span data-stu-id="94233-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="94233-648">El valor del atributo **ToRole** debe ser el mismo que el valor de uno de los atributos de **rol** definidos en uno de los extremos de la Asociación (definidos en el elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="94233-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="94233-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="94233-649">**FromRole**</span></span>     | <span data-ttu-id="94233-650">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-650">Yes</span></span>         | <span data-ttu-id="94233-651">Extremo de la asociación desde el que comienza la navegación.</span><span class="sxs-lookup"><span data-stu-id="94233-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="94233-652">El valor del atributo **FromRole** debe ser el mismo que el valor de uno de los atributos de **rol** definidos en uno de los extremos de la Asociación (definidos en el elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="94233-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-653">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="94233-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="94233-654">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-655">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-656">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-656">Example</span></span>

<span data-ttu-id="94233-657">En el ejemplo siguiente se define un tipo de entidad (**book**) con dos propiedades de navegación (**PublishedBy** y **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="94233-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="94233-658">OnDelete (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="94233-659">El elemento **aldelete** en el lenguaje de definición de esquemas conceptuales (CSDL) define el comportamiento que está conectado a una asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="94233-660">Si el atributo **Action** se establece en **Cascade** en un extremo de una asociación, los tipos de entidad relacionados en el otro extremo de la asociación se eliminan cuando se elimina el tipo de entidad del primer extremo.</span><span class="sxs-lookup"><span data-stu-id="94233-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="94233-661">Si la asociación entre dos tipos de entidad es una relación entre clave principal y clave principal, se elimina un objeto dependiente cargado cuando se elimina el objeto de entidad de seguridad del otro extremo de la asociación, independientemente de la especificación de la **eliminación** .</span><span class="sxs-lookup"><span data-stu-id="94233-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="94233-662">El elemento **aldelete** solo afecta al comportamiento de tiempo de ejecución de una aplicación; no afecta al comportamiento en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="94233-663">El comportamiento definido en el origen de datos debe ser igual que el definido en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="94233-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="94233-664">Un elemento **aldelete** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-665">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-666">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-667">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-667">Applicable Attributes</span></span>

<span data-ttu-id="94233-668">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **aleliminar** .</span><span class="sxs-lookup"><span data-stu-id="94233-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="94233-669">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-669">Attribute Name</span></span> | <span data-ttu-id="94233-670">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-670">Is Required</span></span> | <span data-ttu-id="94233-671">Value</span><span class="sxs-lookup"><span data-stu-id="94233-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-672">**Acción**</span><span class="sxs-lookup"><span data-stu-id="94233-672">**Action**</span></span>     | <span data-ttu-id="94233-673">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-673">Yes</span></span>         | <span data-ttu-id="94233-674">**Cascade** o **None**.</span><span class="sxs-lookup"><span data-stu-id="94233-674">**Cascade** or **None**.</span></span> <span data-ttu-id="94233-675">Si **en cascada**, los tipos de entidad dependientes se eliminarán cuando se elimine el tipo de entidad de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="94233-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="94233-676">Si no hay **ninguno**, los tipos de entidad dependientes no se eliminarán cuando se elimine el tipo de entidad de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="94233-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-677">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="94233-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="94233-678">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-679">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-680">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-680">Example</span></span>

<span data-ttu-id="94233-681">En el ejemplo siguiente se muestra un elemento **Association** que define la Asociación **CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="94233-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="94233-682">El elemento **aleliminar** indica que todos los **pedidos** relacionados con un **cliente** determinado y que se han cargado en el ObjectContext se eliminarán cuando se elimine el **cliente** .</span><span class="sxs-lookup"><span data-stu-id="94233-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="94233-683">Parameter (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="94233-684">El elemento **Parameter** en el lenguaje de definición de esquemas conceptuales (CSDL) puede ser un elemento secundario del elemento FunctionImport o del elemento function.</span><span class="sxs-lookup"><span data-stu-id="94233-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="94233-685">Aplicación para el elemento FunctionImport</span><span class="sxs-lookup"><span data-stu-id="94233-685">FunctionImport Element Application</span></span>

<span data-ttu-id="94233-686">Un elemento **Parameter** (como elemento secundario del elemento **FunctionImport** ) se usa para definir los parámetros de entrada y salida para las importaciones de funciones que se declaran en CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="94233-687">El elemento **Parameter** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-688">Documentación (cero o un elemento permitido)</span><span class="sxs-lookup"><span data-stu-id="94233-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="94233-689">Elementos Annotation (cero o más elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="94233-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="94233-690">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-690">Applicable Attributes</span></span>

<span data-ttu-id="94233-691">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="94233-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="94233-692">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-692">Attribute Name</span></span> | <span data-ttu-id="94233-693">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-693">Is Required</span></span> | <span data-ttu-id="94233-694">Value</span><span class="sxs-lookup"><span data-stu-id="94233-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-695">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-695">**Name**</span></span>       | <span data-ttu-id="94233-696">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-696">Yes</span></span>         | <span data-ttu-id="94233-697">El nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="94233-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="94233-698">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="94233-698">**Type**</span></span>       | <span data-ttu-id="94233-699">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-699">Yes</span></span>         | <span data-ttu-id="94233-700">El tipo de parámetro.</span><span class="sxs-lookup"><span data-stu-id="94233-700">The parameter type.</span></span> <span data-ttu-id="94233-701">El valor tiene que ser un tipo **EDMSimpleType** o un tipo complejo que se encuentre dentro del ámbito del modelo.</span><span class="sxs-lookup"><span data-stu-id="94233-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="94233-702">**Modo**</span><span class="sxs-lookup"><span data-stu-id="94233-702">**Mode**</span></span>       | <span data-ttu-id="94233-703">No</span><span class="sxs-lookup"><span data-stu-id="94233-703">No</span></span>          | <span data-ttu-id="94233-704">**In**, **out**o **INOUT** dependiendo de si el parámetro es un parámetro de entrada, de salida o de entrada/salida.</span><span class="sxs-lookup"><span data-stu-id="94233-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="94233-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="94233-705">**MaxLength**</span></span>  | <span data-ttu-id="94233-706">No</span><span class="sxs-lookup"><span data-stu-id="94233-706">No</span></span>          | <span data-ttu-id="94233-707">La longitud máxima permitida del parámetro.</span><span class="sxs-lookup"><span data-stu-id="94233-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="94233-708">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="94233-708">**Precision**</span></span>  | <span data-ttu-id="94233-709">No</span><span class="sxs-lookup"><span data-stu-id="94233-709">No</span></span>          | <span data-ttu-id="94233-710">La precisión del parámetro.</span><span class="sxs-lookup"><span data-stu-id="94233-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="94233-711">**Escala**</span><span class="sxs-lookup"><span data-stu-id="94233-711">**Scale**</span></span>      | <span data-ttu-id="94233-712">No</span><span class="sxs-lookup"><span data-stu-id="94233-712">No</span></span>          | <span data-ttu-id="94233-713">La escala del parámetro.</span><span class="sxs-lookup"><span data-stu-id="94233-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="94233-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="94233-714">**SRID**</span></span>       | <span data-ttu-id="94233-715">No</span><span class="sxs-lookup"><span data-stu-id="94233-715">No</span></span>          | <span data-ttu-id="94233-716">Identificador de referencia del sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="94233-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="94233-717">Válido solo para los parámetros de los tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="94233-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="94233-718">Para obtener más información, vea [SRID](https://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="94233-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-719">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="94233-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="94233-720">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-721">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="94233-722">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-722">Example</span></span>

<span data-ttu-id="94233-723">En el ejemplo siguiente se muestra un elemento **FunctionImport** con un elemento secundario **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="94233-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="94233-724">La función acepta un parámetro de entrada y devuelve una colección de tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="94233-725">Aplicación para el elemento Function</span><span class="sxs-lookup"><span data-stu-id="94233-725">Function Element Application</span></span>

<span data-ttu-id="94233-726">Un elemento **Parameter** (como elemento secundario del elemento **function** ) define los parámetros de las funciones que se definen o declaran en un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="94233-727">El elemento **Parameter** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-728">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="94233-729">CollectionType (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="94233-730">ReferenceType (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="94233-731">RowType (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="94233-732">Solo uno de los elementos **CollectionType**, **referenceType**o **RowType** puede ser un elemento secundario de un elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="94233-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="94233-733">Elementos Annotation (cero o más elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="94233-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="94233-734">Los elementos de anotación deben aparecer después de todos los demás elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="94233-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="94233-735">Los elementos Annotation solo se permiten en CSDL V2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="94233-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="94233-736">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-736">Applicable Attributes</span></span>

<span data-ttu-id="94233-737">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="94233-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="94233-738">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-738">Attribute Name</span></span>   | <span data-ttu-id="94233-739">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-739">Is Required</span></span> | <span data-ttu-id="94233-740">Value</span><span class="sxs-lookup"><span data-stu-id="94233-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-741">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-741">**Name**</span></span>         | <span data-ttu-id="94233-742">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-742">Yes</span></span>         | <span data-ttu-id="94233-743">El nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="94233-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="94233-744">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="94233-744">**Type**</span></span>         | <span data-ttu-id="94233-745">No</span><span class="sxs-lookup"><span data-stu-id="94233-745">No</span></span>          | <span data-ttu-id="94233-746">El tipo de parámetro.</span><span class="sxs-lookup"><span data-stu-id="94233-746">The parameter type.</span></span> <span data-ttu-id="94233-747">Un parámetro puede ser de cualquiera de los siguientes tipos (o colecciones de estos tipos):</span><span class="sxs-lookup"><span data-stu-id="94233-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="94233-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="94233-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="94233-749">tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="94233-749">entity type</span></span> <br/> <span data-ttu-id="94233-750">tipo complejo</span><span class="sxs-lookup"><span data-stu-id="94233-750">complex type</span></span> <br/> <span data-ttu-id="94233-751">tipo de fila</span><span class="sxs-lookup"><span data-stu-id="94233-751">row type</span></span> <br/> <span data-ttu-id="94233-752">tipo de referencia</span><span class="sxs-lookup"><span data-stu-id="94233-752">reference type</span></span>                             |
| <span data-ttu-id="94233-753">**Admisión de valores NULL**</span><span class="sxs-lookup"><span data-stu-id="94233-753">**Nullable**</span></span>     | <span data-ttu-id="94233-754">No</span><span class="sxs-lookup"><span data-stu-id="94233-754">No</span></span>          | <span data-ttu-id="94233-755">**True** (valor predeterminado) o **false** , dependiendo de si la propiedad puede tener un valor **null** .</span><span class="sxs-lookup"><span data-stu-id="94233-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="94233-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="94233-756">**DefaultValue**</span></span> | <span data-ttu-id="94233-757">No</span><span class="sxs-lookup"><span data-stu-id="94233-757">No</span></span>          | <span data-ttu-id="94233-758">Valor predeterminado de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="94233-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="94233-759">**MaxLength**</span></span>    | <span data-ttu-id="94233-760">No</span><span class="sxs-lookup"><span data-stu-id="94233-760">No</span></span>          | <span data-ttu-id="94233-761">La longitud máxima del valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="94233-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="94233-762">**FixedLength**</span></span>  | <span data-ttu-id="94233-763">No</span><span class="sxs-lookup"><span data-stu-id="94233-763">No</span></span>          | <span data-ttu-id="94233-764">**True** o **false** , dependiendo de si el valor de la propiedad se almacenará como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="94233-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="94233-765">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="94233-765">**Precision**</span></span>    | <span data-ttu-id="94233-766">No</span><span class="sxs-lookup"><span data-stu-id="94233-766">No</span></span>          | <span data-ttu-id="94233-767">Precisión del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="94233-768">**Escala**</span><span class="sxs-lookup"><span data-stu-id="94233-768">**Scale**</span></span>        | <span data-ttu-id="94233-769">No</span><span class="sxs-lookup"><span data-stu-id="94233-769">No</span></span>          | <span data-ttu-id="94233-770">Escala del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="94233-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="94233-771">**SRID**</span></span>         | <span data-ttu-id="94233-772">No</span><span class="sxs-lookup"><span data-stu-id="94233-772">No</span></span>          | <span data-ttu-id="94233-773">Identificador de referencia del sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="94233-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="94233-774">Solo es válido para las propiedades de los tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="94233-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="94233-775">Para obtener más información, vea [SRID](https://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="94233-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="94233-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="94233-776">**Unicode**</span></span>      | <span data-ttu-id="94233-777">No</span><span class="sxs-lookup"><span data-stu-id="94233-777">No</span></span>          | <span data-ttu-id="94233-778">**True** o **false** , dependiendo de si el valor de la propiedad se almacenará como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="94233-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="94233-779">**Intercalación**</span><span class="sxs-lookup"><span data-stu-id="94233-779">**Collation**</span></span>    | <span data-ttu-id="94233-780">No</span><span class="sxs-lookup"><span data-stu-id="94233-780">No</span></span>          | <span data-ttu-id="94233-781">Cadena que especifica la secuencia de intercalación que se usará en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="94233-782">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="94233-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="94233-783">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-784">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="94233-785">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-785">Example</span></span>

<span data-ttu-id="94233-786">En el ejemplo siguiente se muestra un elemento de **función** que usa un elemento secundario **Parameter** para definir un parámetro de función.</span><span class="sxs-lookup"><span data-stu-id="94233-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="94233-787">Principal (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="94233-788">El elemento **principal** del lenguaje de definición de esquemas conceptuales (CSDL) es un elemento secundario del elemento ReferentialConstraint que define el extremo principal de una restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="94233-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="94233-789">Un elemento **ReferentialConstraint** define una funcionalidad similar a una restricción de integridad referencial en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="94233-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="94233-790">Del mismo modo que una columna (o columnas) de una tabla de base de datos puede hacer referencia a la clave principal de otra tabla, una propiedad (o propiedades) de un tipo de entidad puede hacer referencia a la clave de entidad de otro tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="94233-791">El tipo de entidad al que se hace referencia se denomina *extremo principal* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="94233-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="94233-792">El tipo de entidad que hace referencia al extremo principal se denomina *extremo dependiente* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="94233-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="94233-793">Los elementos **PropertyRef** se utilizan para especificar las claves a las que hace referencia el extremo dependiente.</span><span class="sxs-lookup"><span data-stu-id="94233-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="94233-794">El elemento **principal** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-795">PropertyRef (uno o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="94233-796">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-797">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-797">Applicable Attributes</span></span>

<span data-ttu-id="94233-798">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="94233-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="94233-799">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-799">Attribute Name</span></span> | <span data-ttu-id="94233-800">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-800">Is Required</span></span> | <span data-ttu-id="94233-801">Value</span><span class="sxs-lookup"><span data-stu-id="94233-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="94233-802">**Rol**</span><span class="sxs-lookup"><span data-stu-id="94233-802">**Role**</span></span>       | <span data-ttu-id="94233-803">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-803">Yes</span></span>         | <span data-ttu-id="94233-804">Nombre del tipo de entidad del extremo principal de la asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-805">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="94233-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="94233-806">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-807">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-808">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-808">Example</span></span>

<span data-ttu-id="94233-809">En el ejemplo siguiente se muestra un elemento **ReferentialConstraint** que forma parte de la definición de la Asociación **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="94233-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="94233-810">La propiedad **ID** del tipo de entidad **Publisher** constituye el extremo principal de la restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="94233-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="94233-811">Property (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-811">Property Element (CSDL)</span></span>

<span data-ttu-id="94233-812">El elemento **Property** del lenguaje de definición de esquemas conceptuales (CSDL) puede ser un elemento secundario del elemento EntityType, el elemento complexType o el elemento RowType.</span><span class="sxs-lookup"><span data-stu-id="94233-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="94233-813">Aplicaciones de elemento EntityType y ComplexType</span><span class="sxs-lookup"><span data-stu-id="94233-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="94233-814">Los elementos de **propiedad** (como elementos secundarios de los elementos **EntityType** o **complexType** ) definen la forma y las características de los datos que contendrá una instancia de tipo de entidad o una instancia de tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="94233-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="94233-815">Las propiedades en un modelo conceptual son análogas a las propiedades que se definen en una clase.</span><span class="sxs-lookup"><span data-stu-id="94233-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="94233-816">Del mismo modo que las propiedades en una clase definen la forma de la clase y proporcionan información sobre los objetos, las propiedades en un modelo conceptual definen la forma de un tipo de entidad y proporcionan información sobre las instancias del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="94233-817">El elemento **Property** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-818">Elemento Documentation (cero o un elemento permitidos)</span><span class="sxs-lookup"><span data-stu-id="94233-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="94233-819">Elementos Annotation (cero o más elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="94233-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="94233-820">Las siguientes caras se pueden aplicar a un elemento **Property** : **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **collation**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="94233-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="94233-821">Las facetas son atributos XML que proporcionan información sobre cómo los valores de propiedad se almacenan en el almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="94233-822">Las caras solo se pueden aplicar a propiedades de tipo **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="94233-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="94233-823">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-823">Applicable Attributes</span></span>

<span data-ttu-id="94233-824">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="94233-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="94233-825">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-825">Attribute Name</span></span>                                                         | <span data-ttu-id="94233-826">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-826">Is Required</span></span> | <span data-ttu-id="94233-827">Value</span><span class="sxs-lookup"><span data-stu-id="94233-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-828">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-828">**Name**</span></span>                                                               | <span data-ttu-id="94233-829">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-829">Yes</span></span>         | <span data-ttu-id="94233-830">El nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="94233-831">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="94233-831">**Type**</span></span>                                                               | <span data-ttu-id="94233-832">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-832">Yes</span></span>         | <span data-ttu-id="94233-833">El tipo de valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-833">The type of the property value.</span></span> <span data-ttu-id="94233-834">El tipo de valor de la propiedad debe ser un tipo **EDMSimpleType** o un tipo complejo (indicado mediante un nombre completo) que se encuentre dentro del ámbito del modelo.</span><span class="sxs-lookup"><span data-stu-id="94233-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="94233-835">**Admisión de valores NULL**</span><span class="sxs-lookup"><span data-stu-id="94233-835">**Nullable**</span></span>                                                           | <span data-ttu-id="94233-836">No</span><span class="sxs-lookup"><span data-stu-id="94233-836">No</span></span>          | <span data-ttu-id="94233-837">**True** (el valor predeterminado) o <strong>False</strong>, según si la propiedad puede tener un valor nulo.</span><span class="sxs-lookup"><span data-stu-id="94233-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="94233-838">> En el CSDL v1, una propiedad de tipo complejo debe tener `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="94233-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="94233-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="94233-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="94233-840">No</span><span class="sxs-lookup"><span data-stu-id="94233-840">No</span></span>          | <span data-ttu-id="94233-841">Valor predeterminado de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="94233-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="94233-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="94233-843">No</span><span class="sxs-lookup"><span data-stu-id="94233-843">No</span></span>          | <span data-ttu-id="94233-844">La longitud máxima del valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="94233-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="94233-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="94233-846">No</span><span class="sxs-lookup"><span data-stu-id="94233-846">No</span></span>          | <span data-ttu-id="94233-847">**True** o **false** , dependiendo de si el valor de la propiedad se almacenará como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="94233-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="94233-848">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="94233-848">**Precision**</span></span>                                                          | <span data-ttu-id="94233-849">No</span><span class="sxs-lookup"><span data-stu-id="94233-849">No</span></span>          | <span data-ttu-id="94233-850">Precisión del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="94233-851">**Escala**</span><span class="sxs-lookup"><span data-stu-id="94233-851">**Scale**</span></span>                                                              | <span data-ttu-id="94233-852">No</span><span class="sxs-lookup"><span data-stu-id="94233-852">No</span></span>          | <span data-ttu-id="94233-853">Escala del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="94233-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="94233-854">**SRID**</span></span>                                                               | <span data-ttu-id="94233-855">No</span><span class="sxs-lookup"><span data-stu-id="94233-855">No</span></span>          | <span data-ttu-id="94233-856">Identificador de referencia del sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="94233-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="94233-857">Solo es válido para las propiedades de los tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="94233-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="94233-858">Para obtener más información, vea [SRID](https://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="94233-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="94233-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="94233-859">**Unicode**</span></span>                                                            | <span data-ttu-id="94233-860">No</span><span class="sxs-lookup"><span data-stu-id="94233-860">No</span></span>          | <span data-ttu-id="94233-861">**True** o **false** , dependiendo de si el valor de la propiedad se almacenará como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="94233-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="94233-862">**Intercalación**</span><span class="sxs-lookup"><span data-stu-id="94233-862">**Collation**</span></span>                                                          | <span data-ttu-id="94233-863">No</span><span class="sxs-lookup"><span data-stu-id="94233-863">No</span></span>          | <span data-ttu-id="94233-864">Cadena que especifica la secuencia de intercalación que se usará en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="94233-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="94233-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="94233-866">No</span><span class="sxs-lookup"><span data-stu-id="94233-866">No</span></span>          | <span data-ttu-id="94233-867">**None** (el valor predeterminado) o **Fixed**.</span><span class="sxs-lookup"><span data-stu-id="94233-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="94233-868">Si el valor se establece en **Fixed**, el valor de la propiedad se usará en las comprobaciones de simultaneidad optimista.</span><span class="sxs-lookup"><span data-stu-id="94233-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="94233-869">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento de **propiedad** .</span><span class="sxs-lookup"><span data-stu-id="94233-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="94233-870">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-871">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="94233-872">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-872">Example</span></span>

<span data-ttu-id="94233-873">En el ejemplo siguiente se muestra un elemento **EntityType** con tres elementos **Property** :</span><span class="sxs-lookup"><span data-stu-id="94233-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="94233-874">En el ejemplo siguiente se muestra un elemento **complexType** con cinco elementos **Property** :</span><span class="sxs-lookup"><span data-stu-id="94233-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="94233-875">Aplicación de elemento RowType</span><span class="sxs-lookup"><span data-stu-id="94233-875">RowType Element Application</span></span>

<span data-ttu-id="94233-876">Los elementos de **propiedad** (como elementos secundarios de un elemento **RowType** ) definen la forma y las características de los datos que se pueden pasar o devolver desde una función definida por el modelo.</span><span class="sxs-lookup"><span data-stu-id="94233-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="94233-877">El elemento **Property** puede tener exactamente uno de los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="94233-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="94233-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="94233-878">CollectionType</span></span>
-   <span data-ttu-id="94233-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="94233-879">ReferenceType</span></span>
-   <span data-ttu-id="94233-880">RowType</span><span class="sxs-lookup"><span data-stu-id="94233-880">RowType</span></span>

<span data-ttu-id="94233-881">El elemento **Property** puede tener cualquier número de elementos Annotation secundarios.</span><span class="sxs-lookup"><span data-stu-id="94233-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="94233-882">Los elementos Annotation solo se permiten en CSDL V2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="94233-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="94233-883">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-883">Applicable Attributes</span></span>

<span data-ttu-id="94233-884">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="94233-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="94233-885">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-885">Attribute Name</span></span>                                                     | <span data-ttu-id="94233-886">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-886">Is Required</span></span> | <span data-ttu-id="94233-887">Value</span><span class="sxs-lookup"><span data-stu-id="94233-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-888">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-888">**Name**</span></span>                                                           | <span data-ttu-id="94233-889">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-889">Yes</span></span>         | <span data-ttu-id="94233-890">El nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="94233-891">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="94233-891">**Type**</span></span>                                                           | <span data-ttu-id="94233-892">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-892">Yes</span></span>         | <span data-ttu-id="94233-893">El tipo de valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="94233-894">**Admisión de valores NULL**</span><span class="sxs-lookup"><span data-stu-id="94233-894">**Nullable**</span></span>                                                       | <span data-ttu-id="94233-895">No</span><span class="sxs-lookup"><span data-stu-id="94233-895">No</span></span>          | <span data-ttu-id="94233-896">**True** (el valor predeterminado) o **False**, según si la propiedad puede tener un valor nulo.</span><span class="sxs-lookup"><span data-stu-id="94233-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="94233-897">> En CSDL v1, una propiedad de tipo complejo debe tener `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="94233-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="94233-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="94233-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="94233-899">No</span><span class="sxs-lookup"><span data-stu-id="94233-899">No</span></span>          | <span data-ttu-id="94233-900">Valor predeterminado de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="94233-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="94233-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="94233-902">No</span><span class="sxs-lookup"><span data-stu-id="94233-902">No</span></span>          | <span data-ttu-id="94233-903">La longitud máxima del valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="94233-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="94233-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="94233-905">No</span><span class="sxs-lookup"><span data-stu-id="94233-905">No</span></span>          | <span data-ttu-id="94233-906">**True** o **false** , dependiendo de si el valor de la propiedad se almacenará como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="94233-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="94233-907">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="94233-907">**Precision**</span></span>                                                      | <span data-ttu-id="94233-908">No</span><span class="sxs-lookup"><span data-stu-id="94233-908">No</span></span>          | <span data-ttu-id="94233-909">Precisión del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="94233-910">**Escala**</span><span class="sxs-lookup"><span data-stu-id="94233-910">**Scale**</span></span>                                                          | <span data-ttu-id="94233-911">No</span><span class="sxs-lookup"><span data-stu-id="94233-911">No</span></span>          | <span data-ttu-id="94233-912">Escala del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="94233-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="94233-913">**SRID**</span></span>                                                           | <span data-ttu-id="94233-914">No</span><span class="sxs-lookup"><span data-stu-id="94233-914">No</span></span>          | <span data-ttu-id="94233-915">Identificador de referencia del sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="94233-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="94233-916">Solo es válido para las propiedades de los tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="94233-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="94233-917">Para obtener más información, vea [SRID](https://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="94233-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="94233-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="94233-918">**Unicode**</span></span>                                                        | <span data-ttu-id="94233-919">No</span><span class="sxs-lookup"><span data-stu-id="94233-919">No</span></span>          | <span data-ttu-id="94233-920">**True** o **false** , dependiendo de si el valor de la propiedad se almacenará como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="94233-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="94233-921">**Intercalación**</span><span class="sxs-lookup"><span data-stu-id="94233-921">**Collation**</span></span>                                                      | <span data-ttu-id="94233-922">No</span><span class="sxs-lookup"><span data-stu-id="94233-922">No</span></span>          | <span data-ttu-id="94233-923">Cadena que especifica la secuencia de intercalación que se usará en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="94233-924">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento de **propiedad** .</span><span class="sxs-lookup"><span data-stu-id="94233-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="94233-925">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-926">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="94233-927">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-927">Example</span></span>

<span data-ttu-id="94233-928">En el ejemplo siguiente se muestran los elementos de **propiedad** que se utilizan para definir la forma del tipo de valor devuelto de una función definida por el modelo.</span><span class="sxs-lookup"><span data-stu-id="94233-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="94233-929">PropertyRef (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="94233-930">El elemento **PropertyRef** en el lenguaje de definición de esquemas conceptuales (CSDL) hace referencia a una propiedad de un tipo de entidad para indicar que la propiedad realizará uno de los roles siguientes:</span><span class="sxs-lookup"><span data-stu-id="94233-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="94233-931">Parte de la clave de la entidad (una propiedad o un conjunto de propiedades de un tipo de entidad que determinan la identidad).</span><span class="sxs-lookup"><span data-stu-id="94233-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="94233-932">Se pueden usar uno o varios elementos **PropertyRef** para definir una clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="94233-933">El extremo dependiente o principal de una restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="94233-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="94233-934">El elemento **PropertyRef** solo puede tener elementos Annotation (cero o más) como elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="94233-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="94233-935">Los elementos Annotation solo se permiten en CSDL V2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="94233-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="94233-936">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-936">Applicable Attributes</span></span>

<span data-ttu-id="94233-937">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="94233-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="94233-938">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-938">Attribute Name</span></span> | <span data-ttu-id="94233-939">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-939">Is Required</span></span> | <span data-ttu-id="94233-940">Value</span><span class="sxs-lookup"><span data-stu-id="94233-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="94233-941">**Nombre**</span><span class="sxs-lookup"><span data-stu-id="94233-941">**Name**</span></span>       | <span data-ttu-id="94233-942">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-942">Yes</span></span>         | <span data-ttu-id="94233-943">Nombre de la propiedad a la que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="94233-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-944">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="94233-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="94233-945">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-946">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-947">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-947">Example</span></span>

<span data-ttu-id="94233-948">En el ejemplo siguiente se define un tipo de entidad (**book**).</span><span class="sxs-lookup"><span data-stu-id="94233-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="94233-949">La clave de entidad se define haciendo referencia a la propiedad **ISBN** del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="94233-950">En el ejemplo siguiente, se usan dos elementos **PropertyRef** para indicar que dos propiedades (**ID** y **PublisherId**) son los extremos principal y dependiente de una restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="94233-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="94233-951">ReferenceType (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="94233-952">El elemento **referenceType** en el lenguaje de definición de esquemas conceptuales (CSDL) especifica una referencia a un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="94233-953">El elemento **referenceType** puede ser un elemento secundario de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="94233-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="94233-954">ReturnType (función)</span><span class="sxs-lookup"><span data-stu-id="94233-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="94233-955">Parámetro</span><span class="sxs-lookup"><span data-stu-id="94233-955">Parameter</span></span>
-   <span data-ttu-id="94233-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="94233-956">CollectionType</span></span>

<span data-ttu-id="94233-957">El elemento **referenceType** se utiliza al definir un parámetro o un tipo de valor devuelto para una función.</span><span class="sxs-lookup"><span data-stu-id="94233-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="94233-958">Un elemento **referenceType** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-959">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-960">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-961">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-961">Applicable Attributes</span></span>

<span data-ttu-id="94233-962">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **referenceType** .</span><span class="sxs-lookup"><span data-stu-id="94233-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="94233-963">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-963">Attribute Name</span></span> | <span data-ttu-id="94233-964">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-964">Is Required</span></span> | <span data-ttu-id="94233-965">Value</span><span class="sxs-lookup"><span data-stu-id="94233-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="94233-966">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="94233-966">**Type**</span></span>       | <span data-ttu-id="94233-967">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-967">Yes</span></span>         | <span data-ttu-id="94233-968">Nombre del tipo de entidad al que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="94233-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-969">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **referenceType** .</span><span class="sxs-lookup"><span data-stu-id="94233-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="94233-970">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-971">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-972">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-972">Example</span></span>

<span data-ttu-id="94233-973">En el ejemplo siguiente se muestra el elemento **referenceType** que se usa como elemento secundario de un elemento **Parameter** en una función definida por el modelo que acepta una referencia a un tipo de entidad **Person** :</span><span class="sxs-lookup"><span data-stu-id="94233-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="94233-974">En el ejemplo siguiente se muestra el elemento **referenceType** usado como elemento secundario de un elemento **ReturnType** (function) en una función definida por el modelo que devuelve una referencia a un tipo de entidad **Person** :</span><span class="sxs-lookup"><span data-stu-id="94233-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="94233-975">ReferentialConstraint (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="94233-976">Un elemento **ReferentialConstraint** en el lenguaje de definición de esquemas conceptuales (CSDL) define una funcionalidad similar a una restricción de integridad referencial en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="94233-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="94233-977">Del mismo modo que una columna (o columnas) de una tabla de base de datos puede hacer referencia a la clave principal de otra tabla, una propiedad (o propiedades) de un tipo de entidad puede hacer referencia a la clave de entidad de otro tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="94233-978">El tipo de entidad al que se hace referencia se denomina *extremo principal* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="94233-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="94233-979">El tipo de entidad que hace referencia al extremo principal se denomina *extremo dependiente* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="94233-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="94233-980">Si una clave externa que se expone en un tipo de entidad hace referencia a una propiedad en otro tipo de entidad, el elemento **ReferentialConstraint** define una asociación entre los dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="94233-981">Dado que el elemento **ReferentialConstraint** proporciona información sobre cómo se relacionan dos tipos de entidad, no es necesario ningún elemento AssociationSetMapping correspondiente en el lenguaje de especificación de asignaciones (MSL).</span><span class="sxs-lookup"><span data-stu-id="94233-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="94233-982">Una asociación entre dos tipos de entidad que no tienen claves externas expuestas debe tener un elemento **AssociationSetMapping** correspondiente para asignar información de asociación al origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="94233-983">Si una clave externa no se expone en un tipo de entidad, el elemento **ReferentialConstraint** solo puede definir una restricción PRIMARY KEY-PRIMARY KEY entre el tipo de entidad y otro tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="94233-984">Un elemento **ReferentialConstraint** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-985">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-986">Principal (exactamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="94233-987">Dependent (exactamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="94233-988">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-989">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-989">Applicable Attributes</span></span>

<span data-ttu-id="94233-990">El elemento **ReferentialConstraint** puede tener cualquier número de atributos de anotación (atributos XML personalizados).</span><span class="sxs-lookup"><span data-stu-id="94233-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="94233-991">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-992">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="94233-993">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-993">Example</span></span>

<span data-ttu-id="94233-994">En el ejemplo siguiente se muestra un elemento **ReferentialConstraint** que se usa como parte de la definición de la Asociación **PublishedBy** .</span><span class="sxs-lookup"><span data-stu-id="94233-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="94233-995">ReturnType (function) (elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="94233-996">El elemento **ReturnType** (function) en el lenguaje de definición de esquemas conceptuales (CSDL) especifica el tipo de valor devuelto para una función que se define en un elemento de función.</span><span class="sxs-lookup"><span data-stu-id="94233-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="94233-997">Un tipo de valor devuelto de función también se puede especificar con un atributo **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="94233-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="94233-998">Los tipos de valor devuelto pueden ser cualquier **EdmSimpleType**, tipo de entidad, tipo complejo, tipo de fila, tipo de referencia o una colección de uno de estos tipos.</span><span class="sxs-lookup"><span data-stu-id="94233-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="94233-999">El tipo de valor devuelto de una función se puede especificar con el atributo **Type** del elemento **ReturnType** (function) o con uno de los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="94233-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="94233-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="94233-1000">CollectionType</span></span>
-   <span data-ttu-id="94233-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="94233-1001">ReferenceType</span></span>
-   <span data-ttu-id="94233-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="94233-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="94233-1003">Un modelo no se validará si especifica un tipo de valor devuelto de función con el atributo **Type** del elemento **ReturnType** (function) y uno de los elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="94233-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="94233-1004">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-1004">Applicable Attributes</span></span>

<span data-ttu-id="94233-1005">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **ReturnType** (function).</span><span class="sxs-lookup"><span data-stu-id="94233-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="94233-1006">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-1006">Attribute Name</span></span> | <span data-ttu-id="94233-1007">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-1007">Is Required</span></span> | <span data-ttu-id="94233-1008">Value</span><span class="sxs-lookup"><span data-stu-id="94233-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="94233-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="94233-1009">**ReturnType**</span></span> | <span data-ttu-id="94233-1010">No</span><span class="sxs-lookup"><span data-stu-id="94233-1010">No</span></span>          | <span data-ttu-id="94233-1011">El tipo devuelto por la función.</span><span class="sxs-lookup"><span data-stu-id="94233-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-1012">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **ReturnType** (function).</span><span class="sxs-lookup"><span data-stu-id="94233-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="94233-1013">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-1014">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-1015">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-1015">Example</span></span>

<span data-ttu-id="94233-1016">En el ejemplo siguiente se usa un elemento **function** para definir una función que devuelve el número de años que un libro se ha impreso.</span><span class="sxs-lookup"><span data-stu-id="94233-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="94233-1017">Tenga en cuenta que el tipo de valor devuelto se especifica mediante el atributo **Type** de un elemento **ReturnType** (function).</span><span class="sxs-lookup"><span data-stu-id="94233-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="94233-1018">ReturnType (FunctionImport) (elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="94233-1019">El elemento **ReturnType** (FunctionImport) en el lenguaje de definición de esquemas conceptuales (CSDL) especifica el tipo de valor devuelto para una función que se define en un elemento FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="94233-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="94233-1020">Un tipo de valor devuelto de función también se puede especificar con un atributo **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="94233-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="94233-1021">Los tipos devueltos pueden ser cualquier colección de tipo de entidad, tipo complejo o **EdmSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="94233-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="94233-1022">El tipo de valor devuelto de una función se especifica con el atributo **Type** del elemento **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="94233-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-1023">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-1023">Applicable Attributes</span></span>

<span data-ttu-id="94233-1024">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="94233-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="94233-1025">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-1025">Attribute Name</span></span> | <span data-ttu-id="94233-1026">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-1026">Is Required</span></span> | <span data-ttu-id="94233-1027">Value</span><span class="sxs-lookup"><span data-stu-id="94233-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-1028">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="94233-1028">**Type**</span></span>       | <span data-ttu-id="94233-1029">No</span><span class="sxs-lookup"><span data-stu-id="94233-1029">No</span></span>          | <span data-ttu-id="94233-1030">Tipo devuelto por la función.</span><span class="sxs-lookup"><span data-stu-id="94233-1030">The type that the function returns.</span></span> <span data-ttu-id="94233-1031">El valor debe ser una colección de ComplexType, EntityType o EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="94233-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="94233-1032">**#A4**</span><span class="sxs-lookup"><span data-stu-id="94233-1032">**EntitySet**</span></span>  | <span data-ttu-id="94233-1033">No</span><span class="sxs-lookup"><span data-stu-id="94233-1033">No</span></span>          | <span data-ttu-id="94233-1034">Si la función devuelve una colección de tipos de entidad, el valor de **EntitySet** debe ser el conjunto de entidades al que pertenece la colección.</span><span class="sxs-lookup"><span data-stu-id="94233-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="94233-1035">De lo contrario, el atributo **EntitySet** no debe usarse.</span><span class="sxs-lookup"><span data-stu-id="94233-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-1036">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="94233-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="94233-1037">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-1038">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-1039">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-1039">Example</span></span>

<span data-ttu-id="94233-1040">En el ejemplo siguiente se usa una **FunctionImport** que devuelve libros y publicadores.</span><span class="sxs-lookup"><span data-stu-id="94233-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="94233-1041">Tenga en cuenta que la función devuelve dos conjuntos de resultados y, por tanto, se especifican dos elementos **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="94233-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="94233-1042">RowType (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="94233-1043">Un elemento **RowType** en el lenguaje de definición de esquemas conceptuales (CSDL) define una estructura sin nombre como un parámetro o tipo de valor devuelto para una función definida en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="94233-1044">Un elemento **RowType** puede ser el elemento secundario de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="94233-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="94233-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="94233-1045">CollectionType</span></span>
-   <span data-ttu-id="94233-1046">Parámetro</span><span class="sxs-lookup"><span data-stu-id="94233-1046">Parameter</span></span>
-   <span data-ttu-id="94233-1047">ReturnType (función)</span><span class="sxs-lookup"><span data-stu-id="94233-1047">ReturnType (Function)</span></span>

<span data-ttu-id="94233-1048">Un elemento **RowType** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-1049">Property (uno o varios)</span><span class="sxs-lookup"><span data-stu-id="94233-1049">Property (one or more)</span></span>
-   <span data-ttu-id="94233-1050">Elementos Annotation (cero o más)</span><span class="sxs-lookup"><span data-stu-id="94233-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-1051">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-1051">Applicable Attributes</span></span>

<span data-ttu-id="94233-1052">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **RowType** .</span><span class="sxs-lookup"><span data-stu-id="94233-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="94233-1053">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-1054">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="94233-1055">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-1055">Example</span></span>

<span data-ttu-id="94233-1056">En el ejemplo siguiente se muestra una función definida por el modelo que utiliza un elemento **CollectionType** para especificar que la función devuelve una colección de filas (tal y como se especifica en el elemento **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="94233-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a><span data-ttu-id="94233-1057">Schema (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="94233-1058">El elemento **Schema** es el elemento raíz de una definición de modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="94233-1059">Contiene las definiciones para los objetos, las funciones y los contenedores que conforman un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="94233-1060">El elemento **Schema** puede contener cero o más de los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="94233-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="94233-1061">Uso</span><span class="sxs-lookup"><span data-stu-id="94233-1061">Using</span></span>
-   <span data-ttu-id="94233-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="94233-1062">EntityContainer</span></span>
-   <span data-ttu-id="94233-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="94233-1063">EntityType</span></span>
-   <span data-ttu-id="94233-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="94233-1064">EnumType</span></span>
-   <span data-ttu-id="94233-1065">Asociación</span><span class="sxs-lookup"><span data-stu-id="94233-1065">Association</span></span>
-   <span data-ttu-id="94233-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="94233-1066">ComplexType</span></span>
-   <span data-ttu-id="94233-1067">Función</span><span class="sxs-lookup"><span data-stu-id="94233-1067">Function</span></span>

<span data-ttu-id="94233-1068">Un elemento **Schema** puede contener cero o un elemento Annotation.</span><span class="sxs-lookup"><span data-stu-id="94233-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="94233-1069">El elemento de **función** y los elementos de anotación solo se permiten en CSDL V2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="94233-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="94233-1070">El elemento **Schema** usa el atributo **namespace** para definir el espacio de nombres para el tipo de entidad, el tipo complejo y los objetos de Asociación de un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="94233-1071">Dentro de un espacio de nombres, no puede haber dos objetos con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="94233-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="94233-1072">Los espacios de nombres pueden abarcar varios elementos de **esquema** y varios archivos. CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="94233-1073">Un espacio de nombres del modelo conceptual es diferente del espacio de nombres XML del elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="94233-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="94233-1074">Un espacio de nombres del modelo conceptual (tal y como se define en el atributo de **espacio de nombres** ) es un contenedor lógico para tipos de entidad, tipos complejos y tipos de asociación.</span><span class="sxs-lookup"><span data-stu-id="94233-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="94233-1075">El espacio de nombres XML (indicado por el atributo **xmlns** ) de un elemento **Schema** es el espacio de nombres predeterminado para los elementos secundarios y los atributos del elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="94233-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="94233-1076">Los espacios de nombres XML con el formato https://schemas.microsoft.com/ado/YYYY/MM/edm (donde YYYY y MM representan un año y un mes respectivamente) se reservan para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="94233-1077">No puede haber elementos y atributos personalizados en espacios de nombres que tengan este formato.</span><span class="sxs-lookup"><span data-stu-id="94233-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-1078">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-1078">Applicable Attributes</span></span>

<span data-ttu-id="94233-1079">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="94233-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="94233-1080">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-1080">Attribute Name</span></span> | <span data-ttu-id="94233-1081">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-1081">Is Required</span></span> | <span data-ttu-id="94233-1082">Value</span><span class="sxs-lookup"><span data-stu-id="94233-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-1083">**Espacio de nombres**</span><span class="sxs-lookup"><span data-stu-id="94233-1083">**Namespace**</span></span>  | <span data-ttu-id="94233-1084">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1084">Yes</span></span>         | <span data-ttu-id="94233-1085">El espacio de nombres del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="94233-1086">El valor del atributo **namespace** se usa para formar el nombre completo de un tipo.</span><span class="sxs-lookup"><span data-stu-id="94233-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="94233-1087">Por ejemplo, si un **EntityType** denominado *Customer* está en el espacio de nombres simple. example. Model, el nombre completo del **EntityType** es SimpleExampleModel. Customer.</span><span class="sxs-lookup"><span data-stu-id="94233-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="94233-1088">Las siguientes cadenas no se pueden usar como el valor del atributo **namespace** : **System**, **Transient**o **EDM**.</span><span class="sxs-lookup"><span data-stu-id="94233-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="94233-1089">El valor del atributo **namespace** no puede ser el mismo que el valor del atributo **namespace** del elemento Schema de SSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="94233-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="94233-1090">**Alias**</span></span>      | <span data-ttu-id="94233-1091">No</span><span class="sxs-lookup"><span data-stu-id="94233-1091">No</span></span>          | <span data-ttu-id="94233-1092">Un identificador usado en lugar del nombre del espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="94233-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="94233-1093">Por ejemplo, si un **EntityType** denominado *Customer* está en el espacio de nombres simple. example. Model y el valor del atributo **alias** es *Model*, puede usar Model. Customer como nombre completo del **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="94233-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="94233-1094">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="94233-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="94233-1095">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-1096">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-1097">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-1097">Example</span></span>

<span data-ttu-id="94233-1098">En el ejemplo siguiente se muestra un elemento **Schema** que contiene un elemento **EntityContainer** , dos elementos **EntityType** y un elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="94233-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="94233-1099">TypeRef (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="94233-1100">El elemento **TypeRef** en el lenguaje de definición de esquemas conceptuales (CSDL) proporciona una referencia a un tipo con nombre existente.</span><span class="sxs-lookup"><span data-stu-id="94233-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="94233-1101">El elemento **TypeRef** puede ser un elemento secundario del elemento CollectionType, que se usa para especificar que una función tiene una colección como parámetro o tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="94233-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="94233-1102">Un elemento **TypeRef** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="94233-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="94233-1103">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="94233-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="94233-1104">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="94233-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-1105">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-1105">Applicable Attributes</span></span>

<span data-ttu-id="94233-1106">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **TypeRef** .</span><span class="sxs-lookup"><span data-stu-id="94233-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="94233-1107">Tenga en cuenta que los atributos **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**y **collation** solo se aplican a **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="94233-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="94233-1108">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="94233-1109">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-1109">Is Required</span></span> | <span data-ttu-id="94233-1110">Value</span><span class="sxs-lookup"><span data-stu-id="94233-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-1111">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="94233-1111">**Type**</span></span>                                                           | <span data-ttu-id="94233-1112">No</span><span class="sxs-lookup"><span data-stu-id="94233-1112">No</span></span>          | <span data-ttu-id="94233-1113">El nombre del tipo al que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="94233-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="94233-1114">**Admisión de valores NULL**</span><span class="sxs-lookup"><span data-stu-id="94233-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="94233-1115">No</span><span class="sxs-lookup"><span data-stu-id="94233-1115">No</span></span>          | <span data-ttu-id="94233-1116">**True** (el valor predeterminado) o **False**, según si la propiedad puede tener un valor nulo.</span><span class="sxs-lookup"><span data-stu-id="94233-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="94233-1117">> En CSDL v1, una propiedad de tipo complejo debe tener `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="94233-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="94233-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="94233-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="94233-1119">No</span><span class="sxs-lookup"><span data-stu-id="94233-1119">No</span></span>          | <span data-ttu-id="94233-1120">Valor predeterminado de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="94233-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="94233-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="94233-1122">No</span><span class="sxs-lookup"><span data-stu-id="94233-1122">No</span></span>          | <span data-ttu-id="94233-1123">La longitud máxima del valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="94233-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="94233-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="94233-1125">No</span><span class="sxs-lookup"><span data-stu-id="94233-1125">No</span></span>          | <span data-ttu-id="94233-1126">**True** o **false** , dependiendo de si el valor de la propiedad se almacenará como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="94233-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="94233-1127">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="94233-1127">**Precision**</span></span>                                                      | <span data-ttu-id="94233-1128">No</span><span class="sxs-lookup"><span data-stu-id="94233-1128">No</span></span>          | <span data-ttu-id="94233-1129">Precisión del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="94233-1130">**Escala**</span><span class="sxs-lookup"><span data-stu-id="94233-1130">**Scale**</span></span>                                                          | <span data-ttu-id="94233-1131">No</span><span class="sxs-lookup"><span data-stu-id="94233-1131">No</span></span>          | <span data-ttu-id="94233-1132">Escala del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="94233-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="94233-1133">**SRID**</span></span>                                                           | <span data-ttu-id="94233-1134">No</span><span class="sxs-lookup"><span data-stu-id="94233-1134">No</span></span>          | <span data-ttu-id="94233-1135">Identificador de referencia del sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="94233-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="94233-1136">Solo es válido para las propiedades de los tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="94233-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="94233-1137">Para obtener más información, vea [SRID](https://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="94233-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="94233-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="94233-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="94233-1139">No</span><span class="sxs-lookup"><span data-stu-id="94233-1139">No</span></span>          | <span data-ttu-id="94233-1140">**True** o **false** , dependiendo de si el valor de la propiedad se almacenará como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="94233-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="94233-1141">**Intercalación**</span><span class="sxs-lookup"><span data-stu-id="94233-1141">**Collation**</span></span>                                                      | <span data-ttu-id="94233-1142">No</span><span class="sxs-lookup"><span data-stu-id="94233-1142">No</span></span>          | <span data-ttu-id="94233-1143">Cadena que especifica la secuencia de intercalación que se usará en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="94233-1144">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="94233-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="94233-1145">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-1146">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-1147">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-1147">Example</span></span>

<span data-ttu-id="94233-1148">En el ejemplo siguiente se muestra una función definida por el modelo que usa el elemento **TypeRef** (como elemento secundario de un elemento **CollectionType** ) para especificar que la función acepta una colección de tipos de entidad **Department** .</span><span class="sxs-lookup"><span data-stu-id="94233-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="94233-1149">Using (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="94233-1150">El elemento **using** del lenguaje de definición de esquemas conceptuales (CSDL) importa el contenido de un modelo conceptual que existe en un espacio de nombres diferente.</span><span class="sxs-lookup"><span data-stu-id="94233-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="94233-1151">Al establecer el valor del atributo de **espacio de nombres** , puede hacer referencia a los tipos de entidad, tipos complejos y tipos de asociación que se definen en otro modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="94233-1152">Más de un elemento **using** puede ser un elemento secundario de un elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="94233-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="94233-1153">El elemento **using** en CSDL no funciona exactamente igual que una instrucción **using** en un lenguaje de programación.</span><span class="sxs-lookup"><span data-stu-id="94233-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="94233-1154">Al importar un espacio de nombres con una instrucción **using** en un lenguaje de programación, no afecta a los objetos del espacio de nombres original.</span><span class="sxs-lookup"><span data-stu-id="94233-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="94233-1155">En CSDL, un espacio de nombres importado puede contener un tipo de entidad derivado de un tipo de entidad del espacio de nombres original.</span><span class="sxs-lookup"><span data-stu-id="94233-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="94233-1156">Esto puede afectar a los conjuntos de entidades declarados en el espacio de nombres original.</span><span class="sxs-lookup"><span data-stu-id="94233-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="94233-1157">El elemento **using** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="94233-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="94233-1158">Documentación (cero o un elemento permitido)</span><span class="sxs-lookup"><span data-stu-id="94233-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="94233-1159">Elementos Annotation (cero o más elementos permitidos)</span><span class="sxs-lookup"><span data-stu-id="94233-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="94233-1160">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-1160">Applicable Attributes</span></span>

<span data-ttu-id="94233-1161">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **using** .</span><span class="sxs-lookup"><span data-stu-id="94233-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="94233-1162">Nombre del atributo</span><span class="sxs-lookup"><span data-stu-id="94233-1162">Attribute Name</span></span> | <span data-ttu-id="94233-1163">Es obligatorio</span><span class="sxs-lookup"><span data-stu-id="94233-1163">Is Required</span></span> | <span data-ttu-id="94233-1164">Value</span><span class="sxs-lookup"><span data-stu-id="94233-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="94233-1165">**Espacio de nombres**</span><span class="sxs-lookup"><span data-stu-id="94233-1165">**Namespace**</span></span>  | <span data-ttu-id="94233-1166">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1166">Yes</span></span>         | <span data-ttu-id="94233-1167">Nombre del espacio de nombres importado.</span><span class="sxs-lookup"><span data-stu-id="94233-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="94233-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="94233-1168">**Alias**</span></span>      | <span data-ttu-id="94233-1169">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1169">Yes</span></span>         | <span data-ttu-id="94233-1170">Un identificador usado en lugar del nombre del espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="94233-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="94233-1171">Aunque este atributo es obligatorio, no es necesario usarlo en lugar del nombre del espacio de nombres para calificar los nombres de los objetos.</span><span class="sxs-lookup"><span data-stu-id="94233-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="94233-1172">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **using** .</span><span class="sxs-lookup"><span data-stu-id="94233-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="94233-1173">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="94233-1174">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="94233-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="94233-1175">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-1175">Example</span></span>

<span data-ttu-id="94233-1176">En el ejemplo siguiente se muestra el elemento **using** que se usa para importar un espacio de nombres que se define en otro lugar.</span><span class="sxs-lookup"><span data-stu-id="94233-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="94233-1177">Tenga en cuenta que el espacio de nombres para el elemento de **esquema** que se muestra es `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="94233-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="94233-1178">La propiedad `Address` en el `Publisher`**EntityType** es un tipo complejo que se define en el espacio de nombres `ExtendedBooksModel` (importado con el elemento **using** ).</span><span class="sxs-lookup"><span data-stu-id="94233-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="94233-1179">Atributos de anotación (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="94233-1180">En el lenguaje de definición de esquemas conceptuales (CSDL), los atributos de anotación son atributos XML personalizados del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="94233-1181">Además de tener una estructura XML válida, los atributos de anotación deben cumplir las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="94233-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="94233-1182">Los atributos de anotación no deben estar en ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="94233-1183">Se pueden aplicar varios atributos de anotación a un elemento CSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="94233-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="94233-1184">Dos atributos de anotación cualesquiera no pueden tener el mismo nombre completo.</span><span class="sxs-lookup"><span data-stu-id="94233-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="94233-1185">Los atributos de anotación se pueden usar para proporcionar metadatos adicionales sobre los elementos en un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="94233-1186">Se puede tener acceso a los metadatos contenidos en los elementos Annotation en tiempo de ejecución mediante el uso de clases en el espacio de nombres System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="94233-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="94233-1187">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-1187">Example</span></span>

<span data-ttu-id="94233-1188">En el ejemplo siguiente se muestra un elemento **EntityType** con un atributo Annotation (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="94233-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="94233-1189">El ejemplo también muestra un elemento Annotation aplicado al elemento de tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="94233-1190">El siguiente código recupera los metadatos del atributo Annotation y los escribe en la consola:</span><span class="sxs-lookup"><span data-stu-id="94233-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="94233-1191">El código anterior supone que el archivo `School.csdl` está en el directorio de resultados del proyecto y que se han agregado las siguientes instrucciones `Imports` y `Using` al proyecto:</span><span class="sxs-lookup"><span data-stu-id="94233-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="94233-1192">Elementos Annotation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="94233-1193">Los elementos Annotation en el lenguaje de definición de esquemas conceptuales (CSDL) son los elementos XML personalizados del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="94233-1194">Además de tener una estructura XML válida, lo siguiente debe ser verdadero para los elementos Annotation:</span><span class="sxs-lookup"><span data-stu-id="94233-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="94233-1195">Los elementos Annotation no deben estar en un espacio de nombres XML que esté reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="94233-1196">Más de un elemento Annotation puede ser un elemento secundario de un elemento CSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="94233-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="94233-1197">Los nombres completos de dos elementos Annotation cualesquiera no deben ser los mismos.</span><span class="sxs-lookup"><span data-stu-id="94233-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="94233-1198">Los elementos Annotation deben aparecer después de todos los demás elementos secundarios de un elemento CSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="94233-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="94233-1199">Los elementos Annotation pueden utilizarse para proporcionar metadatos adicionales sobre los elementos en un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="94233-1200">A partir de la .NET Framework versión 4, se puede tener acceso a los metadatos contenidos en los elementos Annotation en tiempo de ejecución mediante el uso de clases en el espacio de nombres System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="94233-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="94233-1201">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-1201">Example</span></span>

<span data-ttu-id="94233-1202">En el ejemplo siguiente se muestra un elemento **EntityType** con un elemento Annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="94233-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="94233-1203">El ejemplo también muestra un atributo de anotación aplicado al elemento de tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="94233-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="94233-1204">El siguiente código recupera los metadatos del elemento de anotación y los escribe en la consola:</span><span class="sxs-lookup"><span data-stu-id="94233-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="94233-1205">El código anterior supone que el archivo School.csdl está en el directorio de resultados del proyecto y que se han agregado las siguientes instrucciones `Imports` y `Using` al proyecto:</span><span class="sxs-lookup"><span data-stu-id="94233-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="94233-1206">Tipos de modelos conceptuales (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="94233-1207">El lenguaje de definición de esquemas conceptuales (CSDL) admite un conjunto de tipos de datos primitivos abstractos, denominados **EDMSimpleTypes**, que definen propiedades en un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="94233-1208">Los **EDMSimpleTypes** son proxies para los tipos de datos primitivos que se admiten en el entorno de almacenamiento o de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="94233-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="94233-1209">En la tabla siguiente se enumeran los tipos de datos primitivos admitidos por CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="94233-1210">En la tabla también se enumeran las caras que se pueden aplicar a cada **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="94233-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="94233-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="94233-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="94233-1212">Descripción</span><span class="sxs-lookup"><span data-stu-id="94233-1212">Description</span></span>                                                | <span data-ttu-id="94233-1213">Facetas aplicables</span><span class="sxs-lookup"><span data-stu-id="94233-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="94233-1214">**EDM. Binary**</span><span class="sxs-lookup"><span data-stu-id="94233-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="94233-1215">Contiene datos binarios.</span><span class="sxs-lookup"><span data-stu-id="94233-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="94233-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="94233-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="94233-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="94233-1218">Contiene el valor **true** o **false**.</span><span class="sxs-lookup"><span data-stu-id="94233-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="94233-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="94233-1220">**EDM. Byte**</span><span class="sxs-lookup"><span data-stu-id="94233-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="94233-1221">Contiene un valor entero de 8 bits sin signo.</span><span class="sxs-lookup"><span data-stu-id="94233-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="94233-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1223">**EDM. DateTime**</span><span class="sxs-lookup"><span data-stu-id="94233-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="94233-1224">Representa una fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="94233-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="94233-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="94233-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="94233-1227">Contiene una fecha y hora como desplazamiento en minutos respecto de GMT.</span><span class="sxs-lookup"><span data-stu-id="94233-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="94233-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1229">**EDM. decimal**</span><span class="sxs-lookup"><span data-stu-id="94233-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="94233-1230">Contiene un valor numérico con una precisión y escala fijas.</span><span class="sxs-lookup"><span data-stu-id="94233-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="94233-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="94233-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="94233-1233">Contiene un número de punto flotante con una precisión de 15 dígitos</span><span class="sxs-lookup"><span data-stu-id="94233-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="94233-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1235">**EDM. Float**</span><span class="sxs-lookup"><span data-stu-id="94233-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="94233-1236">Contiene un número de punto flotante con una precisión de 7 dígitos.</span><span class="sxs-lookup"><span data-stu-id="94233-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="94233-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1238">**EDM. GUID**</span><span class="sxs-lookup"><span data-stu-id="94233-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="94233-1239">Contiene un identificador único de 16 bytes.</span><span class="sxs-lookup"><span data-stu-id="94233-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="94233-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1241">**EDM. Int16**</span><span class="sxs-lookup"><span data-stu-id="94233-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="94233-1242">Contiene un valor entero de 16 bits con signo.</span><span class="sxs-lookup"><span data-stu-id="94233-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="94233-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="94233-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="94233-1245">Contiene un valor entero de 32 bits con signo.</span><span class="sxs-lookup"><span data-stu-id="94233-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="94233-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="94233-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="94233-1248">Contiene un valor entero de 64 bits con signo.</span><span class="sxs-lookup"><span data-stu-id="94233-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="94233-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1250">**EDM. SByte**</span><span class="sxs-lookup"><span data-stu-id="94233-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="94233-1251">Contiene un valor entero de 8 bits con signo.</span><span class="sxs-lookup"><span data-stu-id="94233-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="94233-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="94233-1253">**Edm.String**</span></span>                   | <span data-ttu-id="94233-1254">Contiene datos de caracteres.</span><span class="sxs-lookup"><span data-stu-id="94233-1254">Contains character data.</span></span>                                   | <span data-ttu-id="94233-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="94233-1256">**EDM. Time**</span><span class="sxs-lookup"><span data-stu-id="94233-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="94233-1257">Contiene una hora del día.</span><span class="sxs-lookup"><span data-stu-id="94233-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="94233-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="94233-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="94233-1259">**EDM. Geography**</span><span class="sxs-lookup"><span data-stu-id="94233-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="94233-1260">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="94233-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="94233-1262">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1263">**EDM. GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="94233-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="94233-1264">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1265">**EDM. GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="94233-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="94233-1266">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1267">**EDM. GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="94233-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="94233-1268">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1269">**EDM. GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="94233-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="94233-1270">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1271">**EDM. GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="94233-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="94233-1272">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1273">**EDM. GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="94233-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="94233-1274">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1275">**EDM. Geometry**</span><span class="sxs-lookup"><span data-stu-id="94233-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="94233-1276">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1277">**EDM. GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="94233-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="94233-1278">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1279">**EDM. GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="94233-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="94233-1280">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1281">**EDM. GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="94233-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="94233-1282">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1283">**EDM. GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="94233-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="94233-1284">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1285">**EDM. GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="94233-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="94233-1286">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1287">**EDM. GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="94233-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="94233-1288">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="94233-1289">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="94233-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="94233-1290">Nullable, default, SRID</span><span class="sxs-lookup"><span data-stu-id="94233-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="94233-1291">Facets (CSDL)</span><span class="sxs-lookup"><span data-stu-id="94233-1291">Facets (CSDL)</span></span>

<span data-ttu-id="94233-1292">En el lenguaje de definición de esquemas conceptuales (CSDL), las facetas representan restricciones en las propiedades de tipos de entidad y tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="94233-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="94233-1293">Las facetas aparecen como atributos XML en los elementos CSDL siguientes:</span><span class="sxs-lookup"><span data-stu-id="94233-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="94233-1294">Propiedad</span><span class="sxs-lookup"><span data-stu-id="94233-1294">Property</span></span>
-   <span data-ttu-id="94233-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="94233-1295">TypeRef</span></span>
-   <span data-ttu-id="94233-1296">Parámetro</span><span class="sxs-lookup"><span data-stu-id="94233-1296">Parameter</span></span>

<span data-ttu-id="94233-1297">En la tabla siguiente se describen las facetas que se admiten en CSDL.</span><span class="sxs-lookup"><span data-stu-id="94233-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="94233-1298">Todas las facetas son opcionales.</span><span class="sxs-lookup"><span data-stu-id="94233-1298">All facets are optional.</span></span> <span data-ttu-id="94233-1299">El Entity Framework usa algunas de las caras que se indican a continuación al generar una base de datos a partir de un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="94233-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="94233-1300">Para obtener información sobre los tipos de datos de un modelo conceptual, vea tipos de modelos conceptuales (CSDL).</span><span class="sxs-lookup"><span data-stu-id="94233-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="94233-1301">Faceta</span><span class="sxs-lookup"><span data-stu-id="94233-1301">Facet</span></span>               | <span data-ttu-id="94233-1302">Descripción</span><span class="sxs-lookup"><span data-stu-id="94233-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="94233-1303">Se aplica a</span><span class="sxs-lookup"><span data-stu-id="94233-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="94233-1304">Se utiliza para la generación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="94233-1304">Used for the database generation</span></span> | <span data-ttu-id="94233-1305">La usa el runtime.</span><span class="sxs-lookup"><span data-stu-id="94233-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="94233-1306">**Intercalación**</span><span class="sxs-lookup"><span data-stu-id="94233-1306">**Collation**</span></span>       | <span data-ttu-id="94233-1307">Especifica la secuencia de intercalación (o secuencia de orden) que se va a usar cuando se realicen las operaciones de comparación y ordenación sobre los valores de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="94233-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="94233-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="94233-1309">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1309">Yes</span></span>                              | <span data-ttu-id="94233-1310">No</span><span class="sxs-lookup"><span data-stu-id="94233-1310">No</span></span>                  |
| <span data-ttu-id="94233-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="94233-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="94233-1312">Indica que el valor de propiedad se debería utilizar para las comprobaciones de la simultaneidad optimista.</span><span class="sxs-lookup"><span data-stu-id="94233-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="94233-1313">Todas las propiedades de **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="94233-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="94233-1314">No</span><span class="sxs-lookup"><span data-stu-id="94233-1314">No</span></span>                               | <span data-ttu-id="94233-1315">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1315">Yes</span></span>                 |
| <span data-ttu-id="94233-1316">**Valor predeterminado**</span><span class="sxs-lookup"><span data-stu-id="94233-1316">**Default**</span></span>         | <span data-ttu-id="94233-1317">Especifica el valor predeterminado de la propiedad si no se proporciona ningún valor al crear las instancias.</span><span class="sxs-lookup"><span data-stu-id="94233-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="94233-1318">Todas las propiedades de **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="94233-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="94233-1319">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1319">Yes</span></span>                              | <span data-ttu-id="94233-1320">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1320">Yes</span></span>                 |
| <span data-ttu-id="94233-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="94233-1321">**FixedLength**</span></span>     | <span data-ttu-id="94233-1322">Especifica si la longitud del valor de propiedad puede variar.</span><span class="sxs-lookup"><span data-stu-id="94233-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="94233-1323">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="94233-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="94233-1324">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1324">Yes</span></span>                              | <span data-ttu-id="94233-1325">No</span><span class="sxs-lookup"><span data-stu-id="94233-1325">No</span></span>                  |
| <span data-ttu-id="94233-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="94233-1326">**MaxLength**</span></span>       | <span data-ttu-id="94233-1327">Especifica la longitud máxima del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="94233-1328">**EDM. Binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="94233-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="94233-1329">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1329">Yes</span></span>                              | <span data-ttu-id="94233-1330">No</span><span class="sxs-lookup"><span data-stu-id="94233-1330">No</span></span>                  |
| <span data-ttu-id="94233-1331">**Admisión de valores NULL**</span><span class="sxs-lookup"><span data-stu-id="94233-1331">**Nullable**</span></span>        | <span data-ttu-id="94233-1332">Especifica si la propiedad puede tener un valor **null** .</span><span class="sxs-lookup"><span data-stu-id="94233-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="94233-1333">Todas las propiedades de **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="94233-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="94233-1334">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1334">Yes</span></span>                              | <span data-ttu-id="94233-1335">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1335">Yes</span></span>                 |
| <span data-ttu-id="94233-1336">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="94233-1336">**Precision**</span></span>       | <span data-ttu-id="94233-1337">Para las propiedades de tipo **decimal**, especifica el número de dígitos que puede tener un valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="94233-1338">En el caso de las propiedades de tipo **Time**, **DateTime**y **DateTimeOffset**, especifica el número de dígitos para la parte fraccionaria de los segundos del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="94233-1339">**EDM. DateTime**, **EDM. DateTimeOffset**, **EDM. decimal**, **EDM. Time**</span><span class="sxs-lookup"><span data-stu-id="94233-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="94233-1340">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1340">Yes</span></span>                              | <span data-ttu-id="94233-1341">No</span><span class="sxs-lookup"><span data-stu-id="94233-1341">No</span></span>                  |
| <span data-ttu-id="94233-1342">**Escala**</span><span class="sxs-lookup"><span data-stu-id="94233-1342">**Scale**</span></span>           | <span data-ttu-id="94233-1343">Especifica el número de dígitos que puede haber a la derecha del separador decimal para el valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="94233-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="94233-1344">**EDM. decimal**</span><span class="sxs-lookup"><span data-stu-id="94233-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="94233-1345">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1345">Yes</span></span>                              | <span data-ttu-id="94233-1346">No</span><span class="sxs-lookup"><span data-stu-id="94233-1346">No</span></span>                  |
| <span data-ttu-id="94233-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="94233-1347">**SRID**</span></span>            | <span data-ttu-id="94233-1348">Especifica el identificador del sistema espacial de referencia del sistema.</span><span class="sxs-lookup"><span data-stu-id="94233-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="94233-1349">Para obtener más información, vea [SRID](https://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="94233-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="94233-1350">**EDM. Geography, EDM. GeographyPoint, EDM. GeographyLineString, EDM. GeographyPolygon, EDM. GeographyMultiPoint, EDM. GeographyMultiLineString, EDM. GeographyMultiPolygon, EDM. GeographyCollection, EDM. Geometry, EDM. GeometryPoint, EDM. GeometryLineString, EDM. GeometryPolygon, EDM. GeometryMultiPoint, EDM. GeometryMultiLineString, EDM. GeometryMultiPolygon, EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="94233-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="94233-1351">No</span><span class="sxs-lookup"><span data-stu-id="94233-1351">No</span></span>                               | <span data-ttu-id="94233-1352">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1352">Yes</span></span>                 |
| <span data-ttu-id="94233-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="94233-1353">**Unicode**</span></span>         | <span data-ttu-id="94233-1354">Indica si el valor de propiedad está almacenado como Unicode.</span><span class="sxs-lookup"><span data-stu-id="94233-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="94233-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="94233-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="94233-1356">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1356">Yes</span></span>                              | <span data-ttu-id="94233-1357">Sí</span><span class="sxs-lookup"><span data-stu-id="94233-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="94233-1358">Al generar una base de datos a partir de un modelo conceptual, el Asistente para generar base de datos reconocerá el valor del atributo **StoreGeneratedPattern** en un elemento **Property** si está en el siguiente espacio de nombres: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="94233-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="94233-1359">Los valores admitidos para el atributo son **Identity** y **Computed**.</span><span class="sxs-lookup"><span data-stu-id="94233-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="94233-1360">Un valor de **Identity** producirá una columna de base de datos con un valor de identidad que se genera en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="94233-1361">Un valor de **calculado** producirá una columna con un valor que se calcula en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="94233-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="94233-1362">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="94233-1362">Example</span></span>

<span data-ttu-id="94233-1363">En el ejemplo siguiente se muestran facetas aplicadas a las propiedades de un tipo de entidad:</span><span class="sxs-lookup"><span data-stu-id="94233-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
