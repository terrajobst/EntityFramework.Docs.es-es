---
title: Especificación de CSDL - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 88669cf80f9a792fda7d191d9f6be2b1734691df
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994734"
---
# <a name="csdl-specification"></a><span data-ttu-id="3336e-102">Especificación CSDL</span><span class="sxs-lookup"><span data-stu-id="3336e-102">CSDL Specification</span></span>
<span data-ttu-id="3336e-103">El lenguaje de definición de esquemas conceptuales (CSDL) es un lenguaje basado en XML que describe las entidades, las relaciones y las funciones que conforman un modelo conceptual de una aplicación controlada por datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="3336e-104">Este modelo conceptual puede utilizarse por Entity Framework o WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="3336e-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="3336e-105">Los metadatos descritos mediante CSDL se usan Entity Framework para asignar entidades y relaciones que se definen en un modelo conceptual a un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="3336e-106">Para obtener más información, consulte [especificación de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) y [especificación de MSL](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="3336e-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="3336e-107">CSDL es la implementación de Entity Framework del Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="3336e-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="3336e-108">En una aplicación de Entity Framework, los metadatos del modelo conceptual se carga desde un archivo .csdl (escrito en CSDL) en una instancia de la System.Data.Metadata.Edm.EdmItemCollection y es accesible mediante el uso de métodos en el Clase System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="3336e-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="3336e-109">Entity Framework usa los metadatos del modelo conceptual para traducir las consultas con el modelo conceptual en comandos específicos del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="3336e-110">EF Designer almacena información sobre el modelo conceptual en un archivo .edmx en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="3336e-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="3336e-111">En tiempo de compilación, EF Designer usa información en un archivo .edmx para crear el archivo .csdl que Entity Framework necesita en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="3336e-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="3336e-112">Las versiones de CSDL se diferencian por los espacios de nombres XML.</span><span class="sxs-lookup"><span data-stu-id="3336e-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="3336e-113">Versión CSDL</span><span class="sxs-lookup"><span data-stu-id="3336e-113">CSDL Version</span></span> | <span data-ttu-id="3336e-114">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="3336e-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="3336e-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="3336e-115">CSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="3336e-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="3336e-116">CSDL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="3336e-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="3336e-117">CSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="3336e-118">Association (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-118">Association Element (CSDL)</span></span>

<span data-ttu-id="3336e-119">Un **asociación** elemento define una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="3336e-120">Una asociación debe especificar los tipos de entidad que están implicados en la relación y el posible número de tipos de entidad en cada extremo de la relación, que se conoce como multiplicidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="3336e-121">La multiplicidad de un extremo de asociación puede tener un valor uno (1), cero o uno (0.. 1), o muchos (\*).</span><span class="sxs-lookup"><span data-stu-id="3336e-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="3336e-122">Esta información se especifica en los dos elementos de End secundarios.</span><span class="sxs-lookup"><span data-stu-id="3336e-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="3336e-123">Es posible obtener acceso a las instancias de tipo de entidad situadas en un extremo de la asociación a través de las propiedades de navegación o las claves externas, si estas se exponen en un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="3336e-124">En una aplicación, una instancia de una asociación representa una asociación concreta entre las instancias de tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="3336e-125">Las instancias de asociación se agrupan de manera lógica en un conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="3336e-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="3336e-126">Un **asociación** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-127">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-128">End (exactamente 2 elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="3336e-129">ReferentialConstraint (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="3336e-130">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-131">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-131">Applicable Attributes</span></span>

<span data-ttu-id="3336e-132">En la tabla siguiente se describe los atributos que se pueden aplicar a la **asociación** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="3336e-133">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-133">Attribute Name</span></span> | <span data-ttu-id="3336e-134">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-134">Is Required</span></span> | <span data-ttu-id="3336e-135">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="3336e-136">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-136">**Name**</span></span>       | <span data-ttu-id="3336e-137">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-137">Yes</span></span>         | <span data-ttu-id="3336e-138">Nombre de la asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-139">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **asociación** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="3336e-140">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-141">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-142">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-142">Example</span></span>

<span data-ttu-id="3336e-143">El ejemplo siguiente se muestra un **asociación** elemento que define el **CustomerOrders** asociación cuando las claves externas no se han expuesto en el **cliente** y  **Orden** tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="3336e-144">El **multiplicidad** valores para cada **final** de la asociación indican que muchas **pedidos** se puede asociar un **cliente**, pero solo un **cliente** se puede asociar un **orden**.</span><span class="sxs-lookup"><span data-stu-id="3336e-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="3336e-145">Además, el **OnDelete** elemento indica que todos los **pedidos** relacionados con un determinado **cliente** y se han cargado en ObjectContext se eliminarán. Si el **cliente** se elimina.</span><span class="sxs-lookup"><span data-stu-id="3336e-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="3336e-146">El ejemplo siguiente se muestra un **asociación** elemento que define el **CustomerOrders** asociación cuando las claves externas se han expuesto en el **cliente** y  **Orden** tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="3336e-147">Con claves externas expuestas, la relación entre las entidades se administra con un **ReferentialConstraint** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="3336e-148">Un elemento AssociationSetMapping correspondiente no es necesario asignar esta asociación al origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="3336e-149">AssociationSet (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="3336e-150">El **AssociationSet** elemento de lenguaje de definición de esquemas conceptuales (CSDL) es un contenedor lógico para las instancias de asociación del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="3336e-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="3336e-151">Un conjunto de asociaciones proporciona una definición para agrupar las instancias de la asociación con objeto de que se puedan asignar a un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="3336e-152">El **AssociationSet** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-153">Documentación (cero o un elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3336e-154">End (exactamente dos elementos necesarios)</span><span class="sxs-lookup"><span data-stu-id="3336e-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="3336e-155">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="3336e-156">El **asociación** atributo especifica el tipo de asociación que contiene un conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="3336e-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="3336e-157">Los conjuntos de entidades que constituyen los extremos de un conjunto de asociaciones se especifican con exactamente dos secundarios **final** elementos.</span><span class="sxs-lookup"><span data-stu-id="3336e-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-158">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-158">Applicable Attributes</span></span>

<span data-ttu-id="3336e-159">En la tabla siguiente se describe los atributos que se pueden aplicar a la **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="3336e-160">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-160">Attribute Name</span></span>  | <span data-ttu-id="3336e-161">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-161">Is Required</span></span> | <span data-ttu-id="3336e-162">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-163">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-163">**Name**</span></span>        | <span data-ttu-id="3336e-164">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-164">Yes</span></span>         | <span data-ttu-id="3336e-165">Nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3336e-165">The name of the entity set.</span></span> <span data-ttu-id="3336e-166">El valor de la **nombre** atributo no puede ser el mismo que el valor de la **asociación** atributo.</span><span class="sxs-lookup"><span data-stu-id="3336e-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="3336e-167">**Asociación**</span><span class="sxs-lookup"><span data-stu-id="3336e-167">**Association**</span></span> | <span data-ttu-id="3336e-168">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-168">Yes</span></span>         | <span data-ttu-id="3336e-169">Nombre completo de la asociación cuyas instancias contiene el conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="3336e-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="3336e-170">La asociación debe estar en el mismo espacio de nombres que el conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="3336e-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-171">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="3336e-172">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-173">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-174">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-174">Example</span></span>

<span data-ttu-id="3336e-175">El ejemplo siguiente se muestra un **EntityContainer** elemento con dos **AssociationSet** elementos:</span><span class="sxs-lookup"><span data-stu-id="3336e-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="3336e-176">CollectionType (Elemento de CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="3336e-177">El **CollectionType** elemento de lenguaje de definición de esquemas conceptuales (CSDL) especifica que un parámetro de función o una función devuelve el tipo es una colección.</span><span class="sxs-lookup"><span data-stu-id="3336e-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="3336e-178">El **CollectionType** elemento puede ser un elemento secundario del elemento de parámetro o el elemento ReturnType (función).</span><span class="sxs-lookup"><span data-stu-id="3336e-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="3336e-179">Se puede especificar el tipo de colección mediante el uso del **tipo** atributo o uno de los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="3336e-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="3336e-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="3336e-180">**CollectionType**</span></span>
-   <span data-ttu-id="3336e-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="3336e-181">ReferenceType</span></span>
-   <span data-ttu-id="3336e-182">RowType</span><span class="sxs-lookup"><span data-stu-id="3336e-182">RowType</span></span>
-   <span data-ttu-id="3336e-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="3336e-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-184">Un modelo no se validará si se especifica el tipo de una colección con ambos el **tipo** atributo y un elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="3336e-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-185">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-185">Applicable Attributes</span></span>

<span data-ttu-id="3336e-186">En la tabla siguiente se describe los atributos que se pueden aplicar a la **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="3336e-187">Tenga en cuenta que el **DefaultValue**, **MaxLength**, **FixedLength**, **precisión**, **escala**,  **Unicode**, y **intercalación** atributos solo son aplicables a colecciones de **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="3336e-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="3336e-188">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-188">Attribute Name</span></span>                                                          | <span data-ttu-id="3336e-189">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-189">Is Required</span></span> | <span data-ttu-id="3336e-190">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="3336e-191">**Type**</span></span>                                                                | <span data-ttu-id="3336e-192">No</span><span class="sxs-lookup"><span data-stu-id="3336e-192">No</span></span>          | <span data-ttu-id="3336e-193">Tipo de la colección.</span><span class="sxs-lookup"><span data-stu-id="3336e-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="3336e-194">**Que acepta valores null**</span><span class="sxs-lookup"><span data-stu-id="3336e-194">**Nullable**</span></span>                                                            | <span data-ttu-id="3336e-195">No</span><span class="sxs-lookup"><span data-stu-id="3336e-195">No</span></span>          | <span data-ttu-id="3336e-196">**True** (el valor predeterminado) o **False** dependiendo de si la propiedad puede tener un valor null.</span><span class="sxs-lookup"><span data-stu-id="3336e-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="3336e-197">> En el CSDL v1, debe tener una propiedad de tipo complejo `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="3336e-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="3336e-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="3336e-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="3336e-199">No</span><span class="sxs-lookup"><span data-stu-id="3336e-199">No</span></span>          | <span data-ttu-id="3336e-200">Valor predeterminado de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="3336e-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="3336e-202">No</span><span class="sxs-lookup"><span data-stu-id="3336e-202">No</span></span>          | <span data-ttu-id="3336e-203">Longitud máxima del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="3336e-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="3336e-205">No</span><span class="sxs-lookup"><span data-stu-id="3336e-205">No</span></span>          | <span data-ttu-id="3336e-206">**True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="3336e-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="3336e-207">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="3336e-207">**Precision**</span></span>                                                           | <span data-ttu-id="3336e-208">No</span><span class="sxs-lookup"><span data-stu-id="3336e-208">No</span></span>          | <span data-ttu-id="3336e-209">Precisión del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="3336e-210">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="3336e-210">**Scale**</span></span>                                                               | <span data-ttu-id="3336e-211">No</span><span class="sxs-lookup"><span data-stu-id="3336e-211">No</span></span>          | <span data-ttu-id="3336e-212">Escala del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="3336e-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3336e-213">**SRID**</span></span>                                                                | <span data-ttu-id="3336e-214">No</span><span class="sxs-lookup"><span data-stu-id="3336e-214">No</span></span>          | <span data-ttu-id="3336e-215">Identificador de referencia espacial de sistema.</span><span class="sxs-lookup"><span data-stu-id="3336e-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3336e-216">Válido solo para las propiedades de tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="3336e-216">Valid only for properties of spatial types.</span></span>   <span data-ttu-id="3336e-217">Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="3336e-217">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="3336e-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3336e-218">**Unicode**</span></span>                                                             | <span data-ttu-id="3336e-219">No</span><span class="sxs-lookup"><span data-stu-id="3336e-219">No</span></span>          | <span data-ttu-id="3336e-220">**True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="3336e-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="3336e-221">**intercalación**</span><span class="sxs-lookup"><span data-stu-id="3336e-221">**Collation**</span></span>                                                           | <span data-ttu-id="3336e-222">No</span><span class="sxs-lookup"><span data-stu-id="3336e-222">No</span></span>          | <span data-ttu-id="3336e-223">Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="3336e-224">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="3336e-225">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-226">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-227">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-227">Example</span></span>

<span data-ttu-id="3336e-228">El ejemplo siguiente muestra una función definida por modelo que utiliza un **CollectionType** elemento para especificar que la función devuelve una colección de **persona** tipos de entidad (como se especifica en el **ElementType** atributo).</span><span class="sxs-lookup"><span data-stu-id="3336e-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="3336e-229">El ejemplo siguiente muestra una función definida por modelo que usa un **CollectionType** elemento para especificar que la función devuelve una colección de filas (como se especifica en el **RowType** elemento).</span><span class="sxs-lookup"><span data-stu-id="3336e-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="3336e-230">El ejemplo siguiente muestra una función definida por modelo que usa el **CollectionType** elemento para especificar que la función acepta como parámetro una colección de **departamento** tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="3336e-231">ComplexType (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="3336e-232">Un **ComplexType** elemento define una estructura de datos que se compone de **EdmSimpleType** propiedades u otros tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="3336e-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span>  <span data-ttu-id="3336e-233">Un tipo complejo puede ser una propiedad de un tipo de entidad o de otro tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="3336e-233">A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="3336e-234">Un tipo complejo se parece a un tipo de entidad en que también define datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="3336e-235">Sin embargo, existen algunas diferencias clave entre los tipos complejos y los tipos de entidad:</span><span class="sxs-lookup"><span data-stu-id="3336e-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="3336e-236">Los tipos complejos no tienen identidades (o claves) y, por consiguiente, no pueden existir de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="3336e-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="3336e-237">Los tipos complejos solo pueden existir como propiedades de tipos de entidad u otros tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="3336e-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="3336e-238">Tipos complejos no pueden participar en las asociaciones.</span><span class="sxs-lookup"><span data-stu-id="3336e-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="3336e-239">Ni el final de una asociación puede ser un tipo complejo y, por tanto, no se puede definir las propiedades de navegación para los tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="3336e-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="3336e-240">Una propiedad de tipo complejo no puede tener un valor nulo, aunque las propiedades escalares de un tipo complejo se pueden establecer cada una con el valor nulo.</span><span class="sxs-lookup"><span data-stu-id="3336e-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="3336e-241">Un **ComplexType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-242">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-243">Propiedad (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="3336e-244">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="3336e-245">En la tabla siguiente se describe los atributos que se pueden aplicar a la **ComplexType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="3336e-246">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="3336e-247">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-247">Is Required</span></span> | <span data-ttu-id="3336e-248">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-249">nombre</span><span class="sxs-lookup"><span data-stu-id="3336e-249">Name</span></span>                                                                                                           | <span data-ttu-id="3336e-250">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-250">Yes</span></span>         | <span data-ttu-id="3336e-251">El nombre del tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="3336e-251">The name of the complex type.</span></span> <span data-ttu-id="3336e-252">El nombre de un tipo complejo no puede ser igual que el nombre de otro tipo complejo, tipo de entidad o asociación que esté dentro del ámbito del modelo.</span><span class="sxs-lookup"><span data-stu-id="3336e-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="3336e-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="3336e-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="3336e-254">No</span><span class="sxs-lookup"><span data-stu-id="3336e-254">No</span></span>          | <span data-ttu-id="3336e-255">El nombre de otro tipo complejo que es el tipo base del tipo complejo que se define.</span><span class="sxs-lookup"><span data-stu-id="3336e-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="3336e-256">> Este atributo no es aplicable en CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="3336e-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="3336e-257">La herencia de los tipos complejos no se admite en esa versión.</span><span class="sxs-lookup"><span data-stu-id="3336e-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="3336e-258">Abstracto</span><span class="sxs-lookup"><span data-stu-id="3336e-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="3336e-259">No</span><span class="sxs-lookup"><span data-stu-id="3336e-259">No</span></span>          | <span data-ttu-id="3336e-260">**True** o **False** (el valor predeterminado) dependiendo de si el tipo complejo es un tipo abstracto.</span><span class="sxs-lookup"><span data-stu-id="3336e-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="3336e-261">> Este atributo no es aplicable en CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="3336e-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="3336e-262">Los tipos complejos de esa versión no pueden ser tipos abstractos.</span><span class="sxs-lookup"><span data-stu-id="3336e-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="3336e-263">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ComplexType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="3336e-264">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-265">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-266">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-266">Example</span></span>

<span data-ttu-id="3336e-267">El ejemplo siguiente muestra un tipo complejo, **dirección**, con el **EdmSimpleType** propiedades **StreetAddress**, **Ciudad**,  **StateOrProvince**, **país**, y **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="3336e-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="3336e-268">Para definir el tipo complejo **dirección** (anterior) como una propiedad de un tipo de entidad, debe declarar el tipo de propiedad en la definición de tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="3336e-269">El ejemplo siguiente se muestra el **dirección** propiedad como un tipo complejo en un tipo de entidad (**Publisher**):</span><span class="sxs-lookup"><span data-stu-id="3336e-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="3336e-270">DefiningExpression (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="3336e-271">El **DefiningExpression** elemento de lenguaje de definición de esquemas conceptuales (CSDL) contiene una expresión de Entity SQL que define una función en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="3336e-272">Con fines de validación, un **DefiningExpression** elemento puede incluir contenido arbitrario.</span><span class="sxs-lookup"><span data-stu-id="3336e-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="3336e-273">Sin embargo, Entity Framework generará una excepción en tiempo de ejecución si un **DefiningExpression** elemento no contiene SQL válido de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-274">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-274">Applicable Attributes</span></span>

<span data-ttu-id="3336e-275">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **DefiningExpression** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="3336e-276">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-277">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3336e-278">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-278">Example</span></span>

<span data-ttu-id="3336e-279">En el ejemplo siguiente se usa un **DefiningExpression** elemento para definir una función que devuelve el número de años transcurridos desde que se publicó un libro.</span><span class="sxs-lookup"><span data-stu-id="3336e-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="3336e-280">El contenido de la **DefiningExpression** se escribe el elemento de Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="3336e-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="3336e-281">Dependent (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="3336e-282">El **dependientes** elemento de lenguaje de definición de esquemas conceptuales (CSDL) es un elemento secundario al elemento ReferentialConstraint y define el extremo dependiente de una restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="3336e-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="3336e-283">Un **ReferentialConstraint** elemento define la funcionalidad que es similar a una restricción de integridad referencial en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="3336e-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="3336e-284">Del mismo modo que una columna (o columnas) de una tabla de base de datos puede hacer referencia a la clave principal de otra tabla, una propiedad (o propiedades) de un tipo de entidad puede hacer referencia a la clave de entidad de otro tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="3336e-285">El tipo de entidad que se hace referencia se denomina el *extremo principal* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="3336e-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="3336e-286">El tipo de entidad que hace referencia el extremo principal se denomina el *extremo dependiente* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="3336e-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="3336e-287">**PropertyRef** elementos se usan para especificar qué claves hacen referencia el extremo principal.</span><span class="sxs-lookup"><span data-stu-id="3336e-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="3336e-288">El **dependientes** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-289">PropertyRef (uno o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="3336e-290">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-291">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-291">Applicable Attributes</span></span>

<span data-ttu-id="3336e-292">En la tabla siguiente se describe los atributos que se pueden aplicar a la **dependientes** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="3336e-293">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-293">Attribute Name</span></span> | <span data-ttu-id="3336e-294">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-294">Is Required</span></span> | <span data-ttu-id="3336e-295">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="3336e-296">**Rol**</span><span class="sxs-lookup"><span data-stu-id="3336e-296">**Role**</span></span>       | <span data-ttu-id="3336e-297">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-297">Yes</span></span>         | <span data-ttu-id="3336e-298">Nombre del tipo de entidad del extremo dependiente de la asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-299">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **dependientes** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="3336e-300">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-301">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-302">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-302">Example</span></span>

<span data-ttu-id="3336e-303">El ejemplo siguiente se muestra un **ReferentialConstraint** elemento que se va a utilizar como parte de la definición de la **PublishedBy** asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="3336e-304">El **PublisherId** propiedad de la **libro** tipo de entidad constituye el extremo dependiente de la restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="3336e-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="3336e-305">Documentation (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="3336e-306">El **documentación** elemento de lenguaje de definición de esquemas conceptuales (CSDL) se puede usar para proporcionar información sobre un objeto que se define en un elemento primario.</span><span class="sxs-lookup"><span data-stu-id="3336e-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="3336e-307">En un archivo .edmx, cuando el **documentación** es un elemento secundario de un elemento que aparece como un objeto en la superficie de diseño del Diseñador de EF (por ejemplo, una entidad, asociación o propiedad), el contenido de la **documentación**  elemento aparecerá en Visual Studio **propiedades** ventana para el objeto.</span><span class="sxs-lookup"><span data-stu-id="3336e-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="3336e-308">El **documentación** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-309">**Resumen**: una breve descripción del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="3336e-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="3336e-310">(cero o un elemento).</span><span class="sxs-lookup"><span data-stu-id="3336e-310">(zero or one element)</span></span>
-   <span data-ttu-id="3336e-311">**LongDescription**: una descripción extensa del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="3336e-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="3336e-312">(cero o un elemento).</span><span class="sxs-lookup"><span data-stu-id="3336e-312">(zero or one element)</span></span>
-   <span data-ttu-id="3336e-313">Elementos de anotación.</span><span class="sxs-lookup"><span data-stu-id="3336e-313">Annotation elements.</span></span> <span data-ttu-id="3336e-314">(cero o más elementos).</span><span class="sxs-lookup"><span data-stu-id="3336e-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-315">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-315">Applicable Attributes</span></span>

<span data-ttu-id="3336e-316">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **documentación** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="3336e-317">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-318">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3336e-319">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-319">Example</span></span>

<span data-ttu-id="3336e-320">El ejemplo siguiente se muestra el **documentación** como un elemento secundario de un elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="3336e-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="3336e-321">Si el fragmento de código siguiente se encontraban en el CSDL contenido de un edmx archivo, el contenido de la **resumen** y **LongDescription** elementos aparecería en Visual Studio **propiedades** ventana al hacer clic en el `Customer` tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="3336e-322">End (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-322">End Element (CSDL)</span></span>

<span data-ttu-id="3336e-323">El **final** elemento de lenguaje de definición de esquemas conceptuales (CSDL) puede ser un elemento secundario del elemento Association o el elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="3336e-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="3336e-324">En cada caso, el rol de la **final** elemento es diferente y los atributos aplicables son diferentes.</span><span class="sxs-lookup"><span data-stu-id="3336e-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="3336e-325">Elemento End como elemento secundario del elemento Association</span><span class="sxs-lookup"><span data-stu-id="3336e-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="3336e-326">Un **final** elemento (como un elemento secundario de la **asociación** elemento) identifica el tipo de entidad en un extremo de una asociación y el número de instancias del tipo de entidad que pueden existir en dicho extremo de una asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="3336e-327">Los extremos de asociación se definen como parte de una asociación, y esta debe tener exactamente dos extremos.</span><span class="sxs-lookup"><span data-stu-id="3336e-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="3336e-328">Es posible obtener acceso a las instancias de tipo de entidad situadas en un extremo de la asociación a través de las propiedades de navegación o las claves externas si estas se exponen en un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="3336e-329">Un **final** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-330">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-331">OnDelete (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="3336e-332">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="3336e-333">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-333">Applicable Attributes</span></span>

<span data-ttu-id="3336e-334">En la tabla siguiente se describe los atributos que se pueden aplicar a la **final** elemento cuando está el elemento secundario de un **asociación** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="3336e-335">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-335">Attribute Name</span></span>   | <span data-ttu-id="3336e-336">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-336">Is Required</span></span> | <span data-ttu-id="3336e-337">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-338">**Type**</span><span class="sxs-lookup"><span data-stu-id="3336e-338">**Type**</span></span>         | <span data-ttu-id="3336e-339">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-339">Yes</span></span>         | <span data-ttu-id="3336e-340">Nombre del tipo de entidad de un extremo de la asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="3336e-341">**Rol**</span><span class="sxs-lookup"><span data-stu-id="3336e-341">**Role**</span></span>         | <span data-ttu-id="3336e-342">No</span><span class="sxs-lookup"><span data-stu-id="3336e-342">No</span></span>          | <span data-ttu-id="3336e-343">Nombre para el extremo de la asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-343">A name for the association end.</span></span> <span data-ttu-id="3336e-344">Si no se proporciona ningún nombre, se usará el nombre del tipo de entidad del extremo de la asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="3336e-345">**Multiplicidad**</span><span class="sxs-lookup"><span data-stu-id="3336e-345">**Multiplicity**</span></span> | <span data-ttu-id="3336e-346">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-346">Yes</span></span>         | <span data-ttu-id="3336e-347">**1**, **de 0.. 1**, o **\*** dependiendo del número de instancias del tipo de entidad que puede estar al final de la asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="3336e-348">**1** indica esa instancia de tipo exactamente una entidad existe en el extremo de asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="3336e-349">**de 0.. 1** indica que pueden existir cero o una instancia de tipo de entidad en el extremo de asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="3336e-350">**\*** indica que el cero, uno o más instancias del tipo de entidad existen en el extremo de asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-351">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **final** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="3336e-352">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-353">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3336e-354">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-354">Example</span></span>

<span data-ttu-id="3336e-355">El ejemplo siguiente se muestra un **asociación** elemento que define el **CustomerOrders** asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="3336e-356">El **multiplicidad** valores para cada **final** de la asociación indican que muchas **pedidos** se puede asociar un **cliente**, pero solo un **cliente** se puede asociar un **orden**.</span><span class="sxs-lookup"><span data-stu-id="3336e-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="3336e-357">Además, el **OnDelete** elemento indica que todos los **pedidos** relacionados con un determinado **cliente** y que se han cargado en el ObjectContext será if se eliminó el **cliente** se elimina.</span><span class="sxs-lookup"><span data-stu-id="3336e-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="3336e-358">Elemento End como elemento secundario del elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="3336e-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="3336e-359">El **final** elemento especifica un extremo de un conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="3336e-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="3336e-360">El **AssociationSet** elemento debe contener dos **final** elementos.</span><span class="sxs-lookup"><span data-stu-id="3336e-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="3336e-361">La información contenida en un **final** elemento se utiliza para asignar un conjunto de asociaciones a un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="3336e-362">Un **final** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-363">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-364">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-365">Los elementos de anotación deben aparecer después de todos los demás elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="3336e-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="3336e-366">Elementos de anotación solo se permiten en CSDL v2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="3336e-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="3336e-367">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-367">Applicable Attributes</span></span>

<span data-ttu-id="3336e-368">En la tabla siguiente se describe los atributos que se pueden aplicar a la **final** elemento cuando está el elemento secundario de un **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="3336e-369">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-369">Attribute Name</span></span> | <span data-ttu-id="3336e-370">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-370">Is Required</span></span> | <span data-ttu-id="3336e-371">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="3336e-372">**EntitySet**</span></span>  | <span data-ttu-id="3336e-373">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-373">Yes</span></span>         | <span data-ttu-id="3336e-374">El nombre de la **EntitySet** elemento que define un extremo del elemento primario **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="3336e-375">El **EntitySet** elemento debe definirse en el mismo contenedor de entidades como elemento primario **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="3336e-376">**Rol**</span><span class="sxs-lookup"><span data-stu-id="3336e-376">**Role**</span></span>       | <span data-ttu-id="3336e-377">No</span><span class="sxs-lookup"><span data-stu-id="3336e-377">No</span></span>          | <span data-ttu-id="3336e-378">Nombre del extremo del conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="3336e-378">The name of the association set end.</span></span> <span data-ttu-id="3336e-379">Si el **rol** no se usa el atributo, el nombre del conjunto del extremo de asociación será el nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3336e-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="3336e-380">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **final** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="3336e-381">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-382">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3336e-383">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-383">Example</span></span>

<span data-ttu-id="3336e-384">El ejemplo siguiente se muestra un **EntityContainer** elemento con dos **AssociationSet** elementos, cada uno con dos **final** elementos:</span><span class="sxs-lookup"><span data-stu-id="3336e-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="3336e-385">EntityContainer (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="3336e-386">El **EntityContainer** elemento de lenguaje de definición de esquemas conceptuales (CSDL) es un contenedor lógico para los conjuntos de entidades, conjuntos de asociaciones y las importaciones de función.</span><span class="sxs-lookup"><span data-stu-id="3336e-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="3336e-387">Un contenedor de entidades del modelo conceptual se asigna a un contenedor de entidades del modelo de almacenamiento a través del elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="3336e-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="3336e-388">Un contenedor de entidades de modelo de almacenamiento describe la estructura de la base de datos: los conjuntos de entidades describen las tablas, los conjuntos de asociaciones describen las restricciones de clave externa y las importaciones de función describen los procedimientos almacenados en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="3336e-389">Un **EntityContainer** elemento puede tener cero o un elemento de la documentación.</span><span class="sxs-lookup"><span data-stu-id="3336e-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="3336e-390">Si un **documentación** elemento está presente, debe preceder a todos los **EntitySet**, **AssociationSet**, y **FunctionImport** elementos.</span><span class="sxs-lookup"><span data-stu-id="3336e-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="3336e-391">Un **EntityContainer** elemento puede tener cero o más de los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="3336e-392">EntitySet</span></span>
-   <span data-ttu-id="3336e-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="3336e-393">AssociationSet</span></span>
-   <span data-ttu-id="3336e-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="3336e-394">FunctionImport</span></span>
-   <span data-ttu-id="3336e-395">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="3336e-395">Annotation elements</span></span>

<span data-ttu-id="3336e-396">Puede ampliar un **EntityContainer** elemento para incluir el contenido de otro **EntityContainer** que está dentro del mismo espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="3336e-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="3336e-397">Para incluir el contenido de otro **EntityContainer**, en la que hacen referencia a **EntityContainer** elemento, establezca el valor de la **extiende** atributo en el nombre de la  **EntityContainer** elemento que se va a incluir.</span><span class="sxs-lookup"><span data-stu-id="3336e-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="3336e-398">Todos los elementos secundarios de los incluidos **EntityContainer** elemento se tratará como elementos secundarios de la referencia **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-399">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-399">Applicable Attributes</span></span>

<span data-ttu-id="3336e-400">En la tabla siguiente se describe los atributos que se pueden aplicar a la **Using** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="3336e-401">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-401">Attribute Name</span></span> | <span data-ttu-id="3336e-402">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-402">Is Required</span></span> | <span data-ttu-id="3336e-403">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="3336e-404">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-404">**Name**</span></span>       | <span data-ttu-id="3336e-405">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-405">Yes</span></span>         | <span data-ttu-id="3336e-406">Nombre del contenedor de entidades.</span><span class="sxs-lookup"><span data-stu-id="3336e-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="3336e-407">**Extiende**</span><span class="sxs-lookup"><span data-stu-id="3336e-407">**Extends**</span></span>    | <span data-ttu-id="3336e-408">No</span><span class="sxs-lookup"><span data-stu-id="3336e-408">No</span></span>          | <span data-ttu-id="3336e-409">Nombre de otro contenedor de entidades dentro del mismo espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="3336e-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-410">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="3336e-411">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-412">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-413">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-413">Example</span></span>

<span data-ttu-id="3336e-414">El ejemplo siguiente se muestra un **EntityContainer** elemento que define tres conjuntos de entidades y dos conjuntos de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="3336e-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="3336e-415">EntitySet (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="3336e-416">El **EntitySet** elemento de lenguaje de definición de esquemas conceptuales es un contenedor lógico para instancias de un tipo de entidad y las instancias de cualquier tipo que se deriva de ese tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="3336e-417">La relación entre un tipo de entidad y un conjunto de entidades es análoga a la relación entre una fila y una tabla en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="3336e-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="3336e-418">Igual que una fila, un tipo de entidad define un conjunto de datos relacionados y, lo mismo que una tabla, un conjunto de entidades contiene instancias de esa definición.</span><span class="sxs-lookup"><span data-stu-id="3336e-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="3336e-419">Un conjunto de entidades proporciona una construcción para agrupar las instancias del tipo de entidad para que se pueden asignar a las estructuras de datos relacionadas en un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="3336e-420">Se pueden definir varios conjuntos de entidades para un tipo de entidad determinado.</span><span class="sxs-lookup"><span data-stu-id="3336e-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-421">EF Designer no admite los modelos conceptuales que contienen varios conjuntos de entidades por tipo.</span><span class="sxs-lookup"><span data-stu-id="3336e-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="3336e-422">El **EntitySet** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-423">Elemento Documentation (cero o un elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3336e-424">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-425">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-425">Applicable Attributes</span></span>

<span data-ttu-id="3336e-426">En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="3336e-427">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-427">Attribute Name</span></span> | <span data-ttu-id="3336e-428">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-428">Is Required</span></span> | <span data-ttu-id="3336e-429">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-430">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-430">**Name**</span></span>       | <span data-ttu-id="3336e-431">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-431">Yes</span></span>         | <span data-ttu-id="3336e-432">Nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3336e-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="3336e-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="3336e-433">**EntityType**</span></span> | <span data-ttu-id="3336e-434">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-434">Yes</span></span>         | <span data-ttu-id="3336e-435">El nombre completo del tipo de entidad para el que el conjunto de entidades contiene las instancias.</span><span class="sxs-lookup"><span data-stu-id="3336e-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-436">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="3336e-437">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-438">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-439">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-439">Example</span></span>

<span data-ttu-id="3336e-440">El ejemplo siguiente se muestra un **EntityContainer** elemento con tres **EntitySet** elementos:</span><span class="sxs-lookup"><span data-stu-id="3336e-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="3336e-441">Es posible definir varios conjuntos de entidades por tipo (MEST).</span><span class="sxs-lookup"><span data-stu-id="3336e-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="3336e-442">En el ejemplo siguiente se define un contenedor de entidades con dos conjuntos de entidades para el **libro** tipo de entidad:</span><span class="sxs-lookup"><span data-stu-id="3336e-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="3336e-443">EntityType (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="3336e-444">El **EntityType** elemento representa la estructura de un concepto de nivel superior, como un cliente o un pedido, en un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="3336e-445">Un tipo de entidad es una plantilla para las instancias de los tipos de entidad de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="3336e-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="3336e-446">Cada plantilla contiene la información siguiente:</span><span class="sxs-lookup"><span data-stu-id="3336e-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="3336e-447">Un nombre único.</span><span class="sxs-lookup"><span data-stu-id="3336e-447">A unique name.</span></span> <span data-ttu-id="3336e-448">(Requerido)</span><span class="sxs-lookup"><span data-stu-id="3336e-448">(Required.)</span></span>
-   <span data-ttu-id="3336e-449">Una clave de entidad definida por una o varias propiedades.</span><span class="sxs-lookup"><span data-stu-id="3336e-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="3336e-450">(Requerido)</span><span class="sxs-lookup"><span data-stu-id="3336e-450">(Required.)</span></span>
-   <span data-ttu-id="3336e-451">Propiedades para el almacenamiento de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-451">Properties for containing data.</span></span> <span data-ttu-id="3336e-452">(Opcional)</span><span class="sxs-lookup"><span data-stu-id="3336e-452">(Optional.)</span></span>
-   <span data-ttu-id="3336e-453">Propiedades de navegación que permiten desplazarse de un extremo de la asociación al otro.</span><span class="sxs-lookup"><span data-stu-id="3336e-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="3336e-454">(Opcional)</span><span class="sxs-lookup"><span data-stu-id="3336e-454">(Optional.)</span></span>

<span data-ttu-id="3336e-455">En una aplicación, una instancia de un tipo de entidad representa un objeto específico (como un cliente o un pedido concreto).</span><span class="sxs-lookup"><span data-stu-id="3336e-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="3336e-456">Cada una de las instancias de un tipo de entidad debe tener una clave de entidad única dentro de un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3336e-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="3336e-457">Dos instancias de tipo de entidad se consideran iguales solo si son del mismo tipo y los valores de sus claves de entidad son idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="3336e-458">Un **EntityType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-459">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-460">Clave (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="3336e-461">Propiedad (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="3336e-462">NavigationProperty (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="3336e-463">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-464">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-464">Applicable Attributes</span></span>

<span data-ttu-id="3336e-465">En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="3336e-466">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="3336e-467">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-467">Is Required</span></span> | <span data-ttu-id="3336e-468">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-469">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="3336e-470">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-470">Yes</span></span>         | <span data-ttu-id="3336e-471">Nombre del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="3336e-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="3336e-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="3336e-473">No</span><span class="sxs-lookup"><span data-stu-id="3336e-473">No</span></span>          | <span data-ttu-id="3336e-474">Nombre de otro tipo de entidad que es el tipo base del tipo de entidad que se define.</span><span class="sxs-lookup"><span data-stu-id="3336e-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="3336e-475">**Abstracto**</span><span class="sxs-lookup"><span data-stu-id="3336e-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="3336e-476">No</span><span class="sxs-lookup"><span data-stu-id="3336e-476">No</span></span>          | <span data-ttu-id="3336e-477">**True** o **False**, dependiendo de si el tipo de entidad es un tipo abstracto.</span><span class="sxs-lookup"><span data-stu-id="3336e-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="3336e-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="3336e-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="3336e-479">No</span><span class="sxs-lookup"><span data-stu-id="3336e-479">No</span></span>          | <span data-ttu-id="3336e-480">**True** o **False** dependiendo de si el tipo de entidad es un tipo de entidad abierto.</span><span class="sxs-lookup"><span data-stu-id="3336e-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="3336e-481">> El **OpenType** el atributo solo es aplicable a los tipos de entidad que se definen en los modelos conceptuales que se usan con ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="3336e-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="3336e-482">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="3336e-483">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-484">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-485">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-485">Example</span></span>

<span data-ttu-id="3336e-486">El ejemplo siguiente se muestra un **EntityType** elemento con tres **propiedad** elementos y dos **NavigationProperty** elementos:</span><span class="sxs-lookup"><span data-stu-id="3336e-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="3336e-487">EnumType (elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="3336e-488">El **EnumType** elemento representa un tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="3336e-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="3336e-489">Un **EnumType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-490">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-491">Miembro (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="3336e-492">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-493">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-493">Applicable Attributes</span></span>

<span data-ttu-id="3336e-494">En la tabla siguiente se describe los atributos que se pueden aplicar a la **EnumType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="3336e-495">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-495">Attribute Name</span></span>     | <span data-ttu-id="3336e-496">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-496">Is Required</span></span> | <span data-ttu-id="3336e-497">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-498">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-498">**Name**</span></span>           | <span data-ttu-id="3336e-499">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-499">Yes</span></span>         | <span data-ttu-id="3336e-500">Nombre del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="3336e-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="3336e-501">**IsFlags**</span></span>        | <span data-ttu-id="3336e-502">No</span><span class="sxs-lookup"><span data-stu-id="3336e-502">No</span></span>          | <span data-ttu-id="3336e-503">**True** o **False**, dependiendo de si se puede usar el tipo de enumeración como un conjunto de marcas.</span><span class="sxs-lookup"><span data-stu-id="3336e-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="3336e-504">El valor predeterminado es **False.**.</span><span class="sxs-lookup"><span data-stu-id="3336e-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="3336e-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="3336e-505">**UnderlyingType**</span></span> | <span data-ttu-id="3336e-506">No</span><span class="sxs-lookup"><span data-stu-id="3336e-506">No</span></span>          | <span data-ttu-id="3336e-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** o **Edm.SByte** definir el intervalo de valores del tipo.</span><span class="sxs-lookup"><span data-stu-id="3336e-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span>   <span data-ttu-id="3336e-508">Es el tipo subyacente predeterminado de elementos de la enumeración **Edm.Int32.**.</span><span class="sxs-lookup"><span data-stu-id="3336e-508">The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-509">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EnumType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="3336e-510">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-511">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-512">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-512">Example</span></span>

<span data-ttu-id="3336e-513">El ejemplo siguiente se muestra un **EnumType** elemento con tres **miembro** elementos:</span><span class="sxs-lookup"><span data-stu-id="3336e-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="3336e-514">Function (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-514">Function Element (CSDL)</span></span>

<span data-ttu-id="3336e-515">El **función** elemento de lenguaje de definición de esquemas conceptuales (CSDL) se usa para definir o declarar las funciones en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="3336e-516">Una función se define mediante el uso de un elemento DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="3336e-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="3336e-517">Un **función** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-518">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-519">Parámetro (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="3336e-520">DefiningExpression (elemento cero o uno)</span><span class="sxs-lookup"><span data-stu-id="3336e-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="3336e-521">ReturnType (función) (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="3336e-522">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="3336e-523">Un devuelto de tipo para una función debe especificarse con el **ReturnType** elemento (función) o el **ReturnType** atributo (ver abajo), pero no ambos.</span><span class="sxs-lookup"><span data-stu-id="3336e-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="3336e-524">Los tipos de valores devueltos posibles son EdmSimpleType, tipo de entidad, tipo complejo, tipo de fila o tipo ref (o una colección de uno de estos tipos).</span><span class="sxs-lookup"><span data-stu-id="3336e-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-525">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-525">Applicable Attributes</span></span>

<span data-ttu-id="3336e-526">En la tabla siguiente se describe los atributos que se pueden aplicar a la **función** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="3336e-527">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-527">Attribute Name</span></span> | <span data-ttu-id="3336e-528">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-528">Is Required</span></span> | <span data-ttu-id="3336e-529">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="3336e-530">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-530">**Name**</span></span>       | <span data-ttu-id="3336e-531">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-531">Yes</span></span>         | <span data-ttu-id="3336e-532">Nombre de la función.</span><span class="sxs-lookup"><span data-stu-id="3336e-532">The name of the function.</span></span>          |
| <span data-ttu-id="3336e-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="3336e-533">**ReturnType**</span></span> | <span data-ttu-id="3336e-534">No</span><span class="sxs-lookup"><span data-stu-id="3336e-534">No</span></span>          | <span data-ttu-id="3336e-535">El tipo devuelto por la función.</span><span class="sxs-lookup"><span data-stu-id="3336e-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-536">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **función** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="3336e-537">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-538">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-539">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-539">Example</span></span>

<span data-ttu-id="3336e-540">En el ejemplo siguiente se usa un **función** elemento para definir una función que devuelve el número de años transcurridos desde que se contrató a un instructor.</span><span class="sxs-lookup"><span data-stu-id="3336e-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="3336e-541">FunctionImport (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="3336e-542">El **FunctionImport** elemento de lenguaje de definición de esquemas conceptuales (CSDL) representa una función que está definido en el origen de datos pero está disponible para los objetos a través del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="3336e-543">Por ejemplo, un elemento de la función del modelo de almacenamiento puede usarse para representar un procedimiento almacenado en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="3336e-544">Un **FunctionImport** elemento en el modelo conceptual representa la función correspondiente en una aplicación de Entity Framework y se asigna a la función de modelo de almacenamiento mediante el elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="3336e-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="3336e-545">Cuando se llama a la función en la aplicación, el procedimiento almacenado correspondiente se ejecuta en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="3336e-546">El **FunctionImport** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-547">Documentación (cero o un elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3336e-548">Parámetro (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="3336e-549">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="3336e-550">ReturnType (FunctionImport) (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="3336e-551">Una **parámetro** elemento debe definirse para cada parámetro que acepta la función.</span><span class="sxs-lookup"><span data-stu-id="3336e-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="3336e-552">Un devuelto de tipo para una función debe especificarse con el **ReturnType** (FunctionImport) (elemento) o el **ReturnType** atributo (ver abajo), pero no ambos.</span><span class="sxs-lookup"><span data-stu-id="3336e-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="3336e-553">El valor de tipo de valor devuelto debe ser una colección de ComplexType, EntityType o EdmSimpleType.</span><span class="sxs-lookup"><span data-stu-id="3336e-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-554">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-554">Applicable Attributes</span></span>

<span data-ttu-id="3336e-555">En la tabla siguiente se describe los atributos que se pueden aplicar a la **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="3336e-556">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-556">Attribute Name</span></span>   | <span data-ttu-id="3336e-557">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-557">Is Required</span></span> | <span data-ttu-id="3336e-558">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-559">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-559">**Name**</span></span>         | <span data-ttu-id="3336e-560">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-560">Yes</span></span>         | <span data-ttu-id="3336e-561">Nombre de la función importada.</span><span class="sxs-lookup"><span data-stu-id="3336e-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="3336e-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="3336e-562">**ReturnType**</span></span>   | <span data-ttu-id="3336e-563">No</span><span class="sxs-lookup"><span data-stu-id="3336e-563">No</span></span>          | <span data-ttu-id="3336e-564">Tipo devuelto por la función.</span><span class="sxs-lookup"><span data-stu-id="3336e-564">The type that the function returns.</span></span> <span data-ttu-id="3336e-565">No use este atributo si la función no devuelve un valor.</span><span class="sxs-lookup"><span data-stu-id="3336e-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="3336e-566">En caso contrario, el valor debe ser una colección de ComplexType, EntityType o EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="3336e-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="3336e-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="3336e-567">**EntitySet**</span></span>    | <span data-ttu-id="3336e-568">No</span><span class="sxs-lookup"><span data-stu-id="3336e-568">No</span></span>          | <span data-ttu-id="3336e-569">Si la función devuelve una colección de entidades tipos, el valor de la **EntitySet** deben ser el conjunto de entidades al que pertenece la colección.</span><span class="sxs-lookup"><span data-stu-id="3336e-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="3336e-570">En caso contrario, el **EntitySet** no debe usarse el atributo.</span><span class="sxs-lookup"><span data-stu-id="3336e-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="3336e-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="3336e-571">**IsComposable**</span></span> | <span data-ttu-id="3336e-572">No</span><span class="sxs-lookup"><span data-stu-id="3336e-572">No</span></span>          | <span data-ttu-id="3336e-573">Si el valor se establece en true, la función es que se pueden componer (Table-valued Function) y se puede usar en una consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="3336e-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span>  <span data-ttu-id="3336e-574">El valor predeterminado es **false**.</span><span class="sxs-lookup"><span data-stu-id="3336e-574">The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="3336e-575">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="3336e-576">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-577">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-578">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-578">Example</span></span>

<span data-ttu-id="3336e-579">El ejemplo siguiente se muestra un **FunctionImport** elemento que acepta un parámetro y devuelve una colección de tipos de entidad:</span><span class="sxs-lookup"><span data-stu-id="3336e-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="3336e-580">Key (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-580">Key Element (CSDL)</span></span>

<span data-ttu-id="3336e-581">El **clave** elemento es un elemento secundario del elemento EntityType y define un *clave de entidad* (una propiedad o un conjunto de propiedades de un tipo de entidad que determinan la identidad).</span><span class="sxs-lookup"><span data-stu-id="3336e-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="3336e-582">Las propiedades que constituyen una entidad se eligen en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="3336e-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="3336e-583">Los valores de las propiedades de clave de entidad deben identificar de forma inequívoca en tiempo de ejecución una instancia de tipo de entidad dentro de un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3336e-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="3336e-584">Las propiedades que constituyen una clave de entidad se deben elegir de tal forma que garanticen la unicidad de las instancias de un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3336e-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="3336e-585">El **clave** elemento define una clave de entidad haciendo referencia a una o varias de las propiedades de un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="3336e-586">El **clave** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="3336e-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="3336e-587">PropertyRef (uno o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="3336e-588">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-589">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-589">Applicable Attributes</span></span>

<span data-ttu-id="3336e-590">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **clave** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="3336e-591">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-592">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3336e-593">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-593">Example</span></span>

<span data-ttu-id="3336e-594">El ejemplo siguiente define un tipo de entidad denominado **libro**.</span><span class="sxs-lookup"><span data-stu-id="3336e-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="3336e-595">La clave de entidad se define haciendo referencia a la **ISBN** propiedad del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="3336e-596">El **ISBN** propiedad es una buena elección para la clave de entidad porque una biblioteca International Standard Book Number (ISBN) identifica de forma única un libro.</span><span class="sxs-lookup"><span data-stu-id="3336e-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="3336e-597">El ejemplo siguiente muestra un tipo de entidad (**autor**) que tiene una clave de entidad que consta de dos propiedades, **nombre** y **dirección**.</span><span class="sxs-lookup"><span data-stu-id="3336e-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="3336e-598">Uso de **nombre** y **dirección** para la entidad de clave es una opción razonable, ya que dos autores con el mismo nombre son vivan en la misma dirección.</span><span class="sxs-lookup"><span data-stu-id="3336e-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="3336e-599">Sin embargo, esta opción no garantiza por completo la existencia de claves de entidad únicas en un conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="3336e-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="3336e-600">Agregar una propiedad, como **AuthorId**, que podría utilizarse para identificar de forma única un autor sería recomendable en este caso.</span><span class="sxs-lookup"><span data-stu-id="3336e-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="3336e-601">Elemento Member (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-601">Member Element (CSDL)</span></span>

<span data-ttu-id="3336e-602">El **miembro** elemento es un elemento secundario del elemento EnumType y define un miembro del tipo enumerado.</span><span class="sxs-lookup"><span data-stu-id="3336e-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-603">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-603">Applicable Attributes</span></span>

<span data-ttu-id="3336e-604">En la tabla siguiente se describe los atributos que se pueden aplicar a la **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="3336e-605">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-605">Attribute Name</span></span> | <span data-ttu-id="3336e-606">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-606">Is Required</span></span> | <span data-ttu-id="3336e-607">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-608">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-608">**Name**</span></span>       | <span data-ttu-id="3336e-609">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-609">Yes</span></span>         | <span data-ttu-id="3336e-610">Nombre del miembro.</span><span class="sxs-lookup"><span data-stu-id="3336e-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="3336e-611">**Valor**</span><span class="sxs-lookup"><span data-stu-id="3336e-611">**Value**</span></span>      | <span data-ttu-id="3336e-612">No</span><span class="sxs-lookup"><span data-stu-id="3336e-612">No</span></span>          | <span data-ttu-id="3336e-613">Valor del miembro.</span><span class="sxs-lookup"><span data-stu-id="3336e-613">The value of the member.</span></span> <span data-ttu-id="3336e-614">De forma predeterminada, el primer miembro tiene el valor 0 y el valor de cada enumerador sucesivo se incrementa en 1.</span><span class="sxs-lookup"><span data-stu-id="3336e-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="3336e-615">Pueden existir varios miembros que tienen los mismos valores.</span><span class="sxs-lookup"><span data-stu-id="3336e-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-616">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **FunctionImport** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="3336e-617">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-618">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-619">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-619">Example</span></span>

<span data-ttu-id="3336e-620">El ejemplo siguiente se muestra un **EnumType** elemento con tres **miembro** elementos:</span><span class="sxs-lookup"><span data-stu-id="3336e-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="3336e-621">NavigationProperty (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="3336e-622">Un **NavigationProperty** elemento define una propiedad de navegación, que proporciona una referencia al otro extremo de una asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="3336e-623">A diferencia de las propiedades definidas por el elemento de propiedad, las propiedades de navegación no definen la forma y características de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="3336e-624">Proporcionan una manera de desplazarse por una asociación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="3336e-625">Las propiedades de navegación son opcionales en los dos tipos de entidad de los extremos de una asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="3336e-626">Si define una propiedad de navegación en un tipo de entidad del extremo de una asociación, no tiene que definir una propiedad de navegación en el tipo de entidad del otro extremo de la asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="3336e-627">El tipo de datos devuelto por una propiedad de navegación viene determinado por la multiplicidad de su extremo remoto de la asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="3336e-628">Por ejemplo, supongamos que una propiedad de navegación, **OrdersNavProp**, existe en un **cliente** tipo de entidad y se desplaza una asociación uno a varios entre **cliente** y  **Orden**.</span><span class="sxs-lookup"><span data-stu-id="3336e-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="3336e-629">Dado que el extremo remoto de la asociación para la propiedad de navegación tiene la multiplicidad muchas (\*), su tipo de datos es una colección (de **orden**).</span><span class="sxs-lookup"><span data-stu-id="3336e-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="3336e-630">De forma similar, si una propiedad de navegación, **CustomerNavProp**, existe en el **orden** tipo de entidad, su tipo de datos sería **cliente** puesto que es la multiplicidad del extremo remoto uno (1).</span><span class="sxs-lookup"><span data-stu-id="3336e-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="3336e-631">Un **NavigationProperty** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-632">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-633">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-634">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-634">Applicable Attributes</span></span>

<span data-ttu-id="3336e-635">En la tabla siguiente se describe los atributos que se pueden aplicar a la **NavigationProperty** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="3336e-636">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-636">Attribute Name</span></span>   | <span data-ttu-id="3336e-637">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-637">Is Required</span></span> | <span data-ttu-id="3336e-638">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-639">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-639">**Name**</span></span>         | <span data-ttu-id="3336e-640">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-640">Yes</span></span>         | <span data-ttu-id="3336e-641">Nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="3336e-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="3336e-642">**Relación**</span><span class="sxs-lookup"><span data-stu-id="3336e-642">**Relationship**</span></span> | <span data-ttu-id="3336e-643">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-643">Yes</span></span>         | <span data-ttu-id="3336e-644">Nombre de una asociación que se encuentra dentro del ámbito del modelo.</span><span class="sxs-lookup"><span data-stu-id="3336e-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="3336e-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="3336e-645">**ToRole**</span></span>       | <span data-ttu-id="3336e-646">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-646">Yes</span></span>         | <span data-ttu-id="3336e-647">Extremo de la asociación en el que finaliza la navegación.</span><span class="sxs-lookup"><span data-stu-id="3336e-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="3336e-648">El valor de la **ToRole** atributo debe ser el mismo que el valor de uno de los **rol** atributos definidos en uno de los extremos de asociación (definidos en el elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="3336e-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="3336e-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="3336e-649">**FromRole**</span></span>     | <span data-ttu-id="3336e-650">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-650">Yes</span></span>         | <span data-ttu-id="3336e-651">Extremo de la asociación desde el que comienza la navegación.</span><span class="sxs-lookup"><span data-stu-id="3336e-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="3336e-652">El valor de la **FromRole** atributo debe ser el mismo que el valor de uno de los **rol** atributos definidos en uno de los extremos de asociación (definidos en el elemento AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="3336e-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-653">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **NavigationProperty** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="3336e-654">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-655">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-656">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-656">Example</span></span>

<span data-ttu-id="3336e-657">En el ejemplo siguiente se define un tipo de entidad (**libro**) con dos propiedades de navegación (**PublishedBy** y **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="3336e-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="3336e-658">OnDelete (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="3336e-659">El **OnDelete** elemento de lenguaje de definición de esquemas conceptuales (CSDL) define el comportamiento que está conectado con una asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="3336e-660">Si el **acción** atributo está establecido en **Cascade** en un extremo de una asociación, relacionados con los tipos de entidad en el otro extremo de la asociación se eliminan cuando se elimina el tipo de entidad del primer extremo.</span><span class="sxs-lookup"><span data-stu-id="3336e-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="3336e-661">Si la asociación entre dos tipos de entidad es una relación de clave principal de clave principal, se elimina un objeto dependiente cargado cuando se elimina el objeto principal en el otro extremo de la asociación con independencia de la **OnDelete** especificación.</span><span class="sxs-lookup"><span data-stu-id="3336e-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="3336e-662">El **OnDelete** elemento sólo afecta al comportamiento en tiempo de ejecución de una aplicación; no afecta al comportamiento del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="3336e-663">El comportamiento definido en el origen de datos debe ser igual que el definido en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3336e-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="3336e-664">Un **OnDelete** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-665">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-666">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-667">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-667">Applicable Attributes</span></span>

<span data-ttu-id="3336e-668">En la tabla siguiente se describe los atributos que se pueden aplicar a la **OnDelete** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="3336e-669">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-669">Attribute Name</span></span> | <span data-ttu-id="3336e-670">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-670">Is Required</span></span> | <span data-ttu-id="3336e-671">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-672">**Acción**</span><span class="sxs-lookup"><span data-stu-id="3336e-672">**Action**</span></span>     | <span data-ttu-id="3336e-673">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-673">Yes</span></span>         | <span data-ttu-id="3336e-674">**CASCADE** o **ninguno**.</span><span class="sxs-lookup"><span data-stu-id="3336e-674">**Cascade** or **None**.</span></span> <span data-ttu-id="3336e-675">Si **Cascade**, tipos de entidad dependiente se eliminarán cuando se elimina el tipo de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="3336e-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="3336e-676">Si **ninguno**, tipos de entidad dependiente no se eliminará cuando se elimina el tipo de entidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="3336e-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-677">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **asociación** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="3336e-678">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-679">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-680">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-680">Example</span></span>

<span data-ttu-id="3336e-681">El ejemplo siguiente se muestra un **asociación** elemento que define el **CustomerOrders** asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="3336e-682">El **OnDelete** elemento indica que todos los **pedidos** relacionados con un determinado **cliente** y se han cargado en la clase ObjectContext se eliminará cuando el  **Cliente** se elimina.</span><span class="sxs-lookup"><span data-stu-id="3336e-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="3336e-683">Parameter (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="3336e-684">El **parámetro** elemento de lenguaje de definición de esquemas conceptuales (CSDL) puede ser un elemento secundario del elemento FunctionImport o el elemento Function.</span><span class="sxs-lookup"><span data-stu-id="3336e-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="3336e-685">Aplicación para el elemento FunctionImport</span><span class="sxs-lookup"><span data-stu-id="3336e-685">FunctionImport Element Application</span></span>

<span data-ttu-id="3336e-686">Un **parámetro** elemento (como un elemento secundario de la **FunctionImport** elemento) se utiliza para definir los parámetros de entrada y salidos para las importaciones de función que se declaran en CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="3336e-687">El **parámetro** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-688">Documentación (cero o un elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3336e-689">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="3336e-690">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-690">Applicable Attributes</span></span>

<span data-ttu-id="3336e-691">En la tabla siguiente se describe los atributos que se pueden aplicar a la **parámetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="3336e-692">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-692">Attribute Name</span></span> | <span data-ttu-id="3336e-693">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-693">Is Required</span></span> | <span data-ttu-id="3336e-694">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-695">**Name**</span></span>       | <span data-ttu-id="3336e-696">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-696">Yes</span></span>         | <span data-ttu-id="3336e-697">Nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="3336e-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="3336e-698">**Type**</span><span class="sxs-lookup"><span data-stu-id="3336e-698">**Type**</span></span>       | <span data-ttu-id="3336e-699">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-699">Yes</span></span>         | <span data-ttu-id="3336e-700">Tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="3336e-700">The parameter type.</span></span> <span data-ttu-id="3336e-701">El valor debe ser un **EDMSimpleType** o un tipo complejo que esté dentro del ámbito del modelo.</span><span class="sxs-lookup"><span data-stu-id="3336e-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="3336e-702">**Modo**</span><span class="sxs-lookup"><span data-stu-id="3336e-702">**Mode**</span></span>       | <span data-ttu-id="3336e-703">No</span><span class="sxs-lookup"><span data-stu-id="3336e-703">No</span></span>          | <span data-ttu-id="3336e-704">**En**, **Out**, o **InOut** dependiendo de si el parámetro es una entrada, salida o parámetro de entrada/salida.</span><span class="sxs-lookup"><span data-stu-id="3336e-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="3336e-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-705">**MaxLength**</span></span>  | <span data-ttu-id="3336e-706">No</span><span class="sxs-lookup"><span data-stu-id="3336e-706">No</span></span>          | <span data-ttu-id="3336e-707">Longitud máxima permitida del parámetro.</span><span class="sxs-lookup"><span data-stu-id="3336e-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="3336e-708">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="3336e-708">**Precision**</span></span>  | <span data-ttu-id="3336e-709">No</span><span class="sxs-lookup"><span data-stu-id="3336e-709">No</span></span>          | <span data-ttu-id="3336e-710">Precisión del parámetro.</span><span class="sxs-lookup"><span data-stu-id="3336e-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="3336e-711">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="3336e-711">**Scale**</span></span>      | <span data-ttu-id="3336e-712">No</span><span class="sxs-lookup"><span data-stu-id="3336e-712">No</span></span>          | <span data-ttu-id="3336e-713">Escala del parámetro.</span><span class="sxs-lookup"><span data-stu-id="3336e-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="3336e-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3336e-714">**SRID**</span></span>       | <span data-ttu-id="3336e-715">No</span><span class="sxs-lookup"><span data-stu-id="3336e-715">No</span></span>          | <span data-ttu-id="3336e-716">Identificador de referencia espacial de sistema.</span><span class="sxs-lookup"><span data-stu-id="3336e-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3336e-717">Válido únicamente para los parámetros de tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="3336e-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="3336e-718">Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3336e-718">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-719">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **parámetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="3336e-720">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-721">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3336e-722">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-722">Example</span></span>

<span data-ttu-id="3336e-723">El ejemplo siguiente se muestra un **FunctionImport** elemento con una **parámetro** elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="3336e-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="3336e-724">La función acepta un parámetro de entrada y devuelve una colección de tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="3336e-725">Aplicación para el elemento Function</span><span class="sxs-lookup"><span data-stu-id="3336e-725">Function Element Application</span></span>

<span data-ttu-id="3336e-726">Un **parámetro** elemento (como un elemento secundario de la **función** elemento) define parámetros para las funciones que se definen o se declara en un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="3336e-727">El **parámetro** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-728">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="3336e-729">CollectionType (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="3336e-730">ReferenceType (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="3336e-731">RowType (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-732">Solo uno de los **CollectionType**, **ReferenceType**, o **RowType** elementos pueden ser un elemento secundario de un **propiedad** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="3336e-733">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-734">Los elementos de anotación deben aparecer después de todos los demás elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="3336e-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="3336e-735">Elementos de anotación solo se permiten en CSDL v2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="3336e-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="3336e-736">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-736">Applicable Attributes</span></span>

<span data-ttu-id="3336e-737">En la tabla siguiente se describe los atributos que se pueden aplicar a la **parámetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="3336e-738">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-738">Attribute Name</span></span>   | <span data-ttu-id="3336e-739">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-739">Is Required</span></span> | <span data-ttu-id="3336e-740">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-741">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-741">**Name**</span></span>         | <span data-ttu-id="3336e-742">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-742">Yes</span></span>         | <span data-ttu-id="3336e-743">Nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="3336e-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="3336e-744">**Type**</span><span class="sxs-lookup"><span data-stu-id="3336e-744">**Type**</span></span>         | <span data-ttu-id="3336e-745">No</span><span class="sxs-lookup"><span data-stu-id="3336e-745">No</span></span>          | <span data-ttu-id="3336e-746">Tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="3336e-746">The parameter type.</span></span> <span data-ttu-id="3336e-747">Un parámetro puede ser de cualquiera de los siguientes tipos (o colecciones de estos tipos):</span><span class="sxs-lookup"><span data-stu-id="3336e-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="3336e-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="3336e-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="3336e-749">tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="3336e-749">entity type</span></span> <br/> <span data-ttu-id="3336e-750">tipo complejo</span><span class="sxs-lookup"><span data-stu-id="3336e-750">complex type</span></span> <br/> <span data-ttu-id="3336e-751">tipo de fila</span><span class="sxs-lookup"><span data-stu-id="3336e-751">row type</span></span> <br/> <span data-ttu-id="3336e-752">tipo de referencia</span><span class="sxs-lookup"><span data-stu-id="3336e-752">reference type</span></span>                             |
| <span data-ttu-id="3336e-753">**Que acepta valores null**</span><span class="sxs-lookup"><span data-stu-id="3336e-753">**Nullable**</span></span>     | <span data-ttu-id="3336e-754">No</span><span class="sxs-lookup"><span data-stu-id="3336e-754">No</span></span>          | <span data-ttu-id="3336e-755">**True** (el valor predeterminado) o **False** dependiendo de si la propiedad puede tener un **null** valor.</span><span class="sxs-lookup"><span data-stu-id="3336e-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="3336e-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="3336e-756">**DefaultValue**</span></span> | <span data-ttu-id="3336e-757">No</span><span class="sxs-lookup"><span data-stu-id="3336e-757">No</span></span>          | <span data-ttu-id="3336e-758">Valor predeterminado de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="3336e-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-759">**MaxLength**</span></span>    | <span data-ttu-id="3336e-760">No</span><span class="sxs-lookup"><span data-stu-id="3336e-760">No</span></span>          | <span data-ttu-id="3336e-761">Longitud máxima del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="3336e-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-762">**FixedLength**</span></span>  | <span data-ttu-id="3336e-763">No</span><span class="sxs-lookup"><span data-stu-id="3336e-763">No</span></span>          | <span data-ttu-id="3336e-764">**True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="3336e-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="3336e-765">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="3336e-765">**Precision**</span></span>    | <span data-ttu-id="3336e-766">No</span><span class="sxs-lookup"><span data-stu-id="3336e-766">No</span></span>          | <span data-ttu-id="3336e-767">Precisión del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="3336e-768">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="3336e-768">**Scale**</span></span>        | <span data-ttu-id="3336e-769">No</span><span class="sxs-lookup"><span data-stu-id="3336e-769">No</span></span>          | <span data-ttu-id="3336e-770">Escala del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="3336e-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3336e-771">**SRID**</span></span>         | <span data-ttu-id="3336e-772">No</span><span class="sxs-lookup"><span data-stu-id="3336e-772">No</span></span>          | <span data-ttu-id="3336e-773">Identificador de referencia espacial de sistema.</span><span class="sxs-lookup"><span data-stu-id="3336e-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3336e-774">Válido solo para las propiedades de tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="3336e-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="3336e-775">Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3336e-775">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="3336e-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3336e-776">**Unicode**</span></span>      | <span data-ttu-id="3336e-777">No</span><span class="sxs-lookup"><span data-stu-id="3336e-777">No</span></span>          | <span data-ttu-id="3336e-778">**True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="3336e-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="3336e-779">**intercalación**</span><span class="sxs-lookup"><span data-stu-id="3336e-779">**Collation**</span></span>    | <span data-ttu-id="3336e-780">No</span><span class="sxs-lookup"><span data-stu-id="3336e-780">No</span></span>          | <span data-ttu-id="3336e-781">Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="3336e-782">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **parámetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="3336e-783">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-784">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3336e-785">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-785">Example</span></span>

<span data-ttu-id="3336e-786">El ejemplo siguiente se muestra un **función** elemento que utiliza uno **parámetro** elemento secundario para definir un parámetro de función.</span><span class="sxs-lookup"><span data-stu-id="3336e-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a><span data-ttu-id="3336e-787">Principal (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="3336e-788">El **Principal** elemento de lenguaje de definición de esquemas conceptuales (CSDL) es un elemento secundario al elemento ReferentialConstraint que define el extremo principal de una restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="3336e-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="3336e-789">Un **ReferentialConstraint** elemento define la funcionalidad que es similar a una restricción de integridad referencial en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="3336e-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="3336e-790">Del mismo modo que una columna (o columnas) de una tabla de base de datos puede hacer referencia a la clave principal de otra tabla, una propiedad (o propiedades) de un tipo de entidad puede hacer referencia a la clave de entidad de otro tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="3336e-791">El tipo de entidad que se hace referencia se denomina el *extremo principal* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="3336e-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="3336e-792">El tipo de entidad que hace referencia el extremo principal se denomina el *extremo dependiente* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="3336e-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="3336e-793">**PropertyRef** elementos se usan para especificar qué claves hace referencia el extremo dependiente.</span><span class="sxs-lookup"><span data-stu-id="3336e-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="3336e-794">El **Principal** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-795">PropertyRef (uno o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="3336e-796">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-797">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-797">Applicable Attributes</span></span>

<span data-ttu-id="3336e-798">En la tabla siguiente se describe los atributos que se pueden aplicar a la **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="3336e-799">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-799">Attribute Name</span></span> | <span data-ttu-id="3336e-800">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-800">Is Required</span></span> | <span data-ttu-id="3336e-801">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="3336e-802">**Rol**</span><span class="sxs-lookup"><span data-stu-id="3336e-802">**Role**</span></span>       | <span data-ttu-id="3336e-803">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-803">Yes</span></span>         | <span data-ttu-id="3336e-804">Nombre del tipo de entidad del extremo principal de la asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-805">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="3336e-806">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-807">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-808">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-808">Example</span></span>

<span data-ttu-id="3336e-809">El ejemplo siguiente se muestra un **ReferentialConstraint** elemento que forma parte de la definición de la **PublishedBy** asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="3336e-810">El **Id** propiedad de la **Publisher** tipo de entidad constituye el extremo principal de la restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="3336e-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="3336e-811">Property (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-811">Property Element (CSDL)</span></span>

<span data-ttu-id="3336e-812">El **propiedad** elemento de lenguaje de definición de esquemas conceptuales (CSDL) puede ser un elemento secundario del elemento EntityType, ComplexType (elemento) o el elemento RowType.</span><span class="sxs-lookup"><span data-stu-id="3336e-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="3336e-813">Aplicaciones de elemento EntityType y ComplexType</span><span class="sxs-lookup"><span data-stu-id="3336e-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="3336e-814">**Propiedad** elementos (como elementos secundarios de **EntityType** o **ComplexType** elementos) definen la forma y características de los datos que contendrá una instancia de tipo de entidad o la instancia de tipo complejo .</span><span class="sxs-lookup"><span data-stu-id="3336e-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="3336e-815">Las propiedades en un modelo conceptual son análogas a las propiedades que se definen en una clase.</span><span class="sxs-lookup"><span data-stu-id="3336e-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="3336e-816">Del mismo modo que las propiedades en una clase definen la forma de la clase y proporcionan información sobre los objetos, las propiedades en un modelo conceptual definen la forma de un tipo de entidad y proporcionan información sobre las instancias del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="3336e-817">El **propiedad** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-818">Elemento Documentation (cero o un elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3336e-819">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="3336e-820">Las siguientes facetas se pueden aplicar a un **propiedad** elemento: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **precisión**, **escala**, **Unicode**, **intercalación**,  **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="3336e-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="3336e-821">Las facetas son atributos XML que proporcionan información sobre cómo los valores de propiedad se almacenan en el almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-822">Las facetas solo se pueden aplicar a propiedades de tipo **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="3336e-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="3336e-823">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-823">Applicable Attributes</span></span>

<span data-ttu-id="3336e-824">En la tabla siguiente se describe los atributos que se pueden aplicar a la **propiedad** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="3336e-825">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-825">Attribute Name</span></span>                                                         | <span data-ttu-id="3336e-826">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-826">Is Required</span></span> | <span data-ttu-id="3336e-827">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-828">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-828">**Name**</span></span>                                                               | <span data-ttu-id="3336e-829">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-829">Yes</span></span>         | <span data-ttu-id="3336e-830">Nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="3336e-831">**Type**</span><span class="sxs-lookup"><span data-stu-id="3336e-831">**Type**</span></span>                                                               | <span data-ttu-id="3336e-832">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-832">Yes</span></span>         | <span data-ttu-id="3336e-833">Tipo del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-833">The type of the property value.</span></span> <span data-ttu-id="3336e-834">El tipo de valor de propiedad debe ser un **EDMSimpleType** o un tipo complejo (indicado por un nombre completo) que se encuentra dentro del ámbito del modelo.</span><span class="sxs-lookup"><span data-stu-id="3336e-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="3336e-835">**Que acepta valores null**</span><span class="sxs-lookup"><span data-stu-id="3336e-835">**Nullable**</span></span>                                                           | <span data-ttu-id="3336e-836">No</span><span class="sxs-lookup"><span data-stu-id="3336e-836">No</span></span>          | <span data-ttu-id="3336e-837">**True** (el valor predeterminado) o <strong>False</strong> dependiendo de si la propiedad puede tener un valor null.</span><span class="sxs-lookup"><span data-stu-id="3336e-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="3336e-838">> En la versión 1 CSDL debe tener una propiedad de tipo complejo `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="3336e-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="3336e-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="3336e-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="3336e-840">No</span><span class="sxs-lookup"><span data-stu-id="3336e-840">No</span></span>          | <span data-ttu-id="3336e-841">Valor predeterminado de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="3336e-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="3336e-843">No</span><span class="sxs-lookup"><span data-stu-id="3336e-843">No</span></span>          | <span data-ttu-id="3336e-844">Longitud máxima del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="3336e-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="3336e-846">No</span><span class="sxs-lookup"><span data-stu-id="3336e-846">No</span></span>          | <span data-ttu-id="3336e-847">**True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="3336e-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="3336e-848">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="3336e-848">**Precision**</span></span>                                                          | <span data-ttu-id="3336e-849">No</span><span class="sxs-lookup"><span data-stu-id="3336e-849">No</span></span>          | <span data-ttu-id="3336e-850">Precisión del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="3336e-851">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="3336e-851">**Scale**</span></span>                                                              | <span data-ttu-id="3336e-852">No</span><span class="sxs-lookup"><span data-stu-id="3336e-852">No</span></span>          | <span data-ttu-id="3336e-853">Escala del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="3336e-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3336e-854">**SRID**</span></span>                                                               | <span data-ttu-id="3336e-855">No</span><span class="sxs-lookup"><span data-stu-id="3336e-855">No</span></span>          | <span data-ttu-id="3336e-856">Identificador de referencia espacial de sistema.</span><span class="sxs-lookup"><span data-stu-id="3336e-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3336e-857">Válido solo para las propiedades de tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="3336e-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="3336e-858">Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3336e-858">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="3336e-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3336e-859">**Unicode**</span></span>                                                            | <span data-ttu-id="3336e-860">No</span><span class="sxs-lookup"><span data-stu-id="3336e-860">No</span></span>          | <span data-ttu-id="3336e-861">**True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="3336e-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="3336e-862">**intercalación**</span><span class="sxs-lookup"><span data-stu-id="3336e-862">**Collation**</span></span>                                                          | <span data-ttu-id="3336e-863">No</span><span class="sxs-lookup"><span data-stu-id="3336e-863">No</span></span>          | <span data-ttu-id="3336e-864">Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="3336e-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="3336e-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="3336e-866">No</span><span class="sxs-lookup"><span data-stu-id="3336e-866">No</span></span>          | <span data-ttu-id="3336e-867">**Ninguno** (el valor predeterminado) o **Fixed**.</span><span class="sxs-lookup"><span data-stu-id="3336e-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="3336e-868">Si el valor se establece en **Fixed**, el valor de propiedad se usará en las comprobaciones de simultaneidad optimista.</span><span class="sxs-lookup"><span data-stu-id="3336e-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="3336e-869">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **propiedad** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="3336e-870">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-871">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3336e-872">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-872">Example</span></span>

<span data-ttu-id="3336e-873">El ejemplo siguiente se muestra un **EntityType** elemento con tres **propiedad** elementos:</span><span class="sxs-lookup"><span data-stu-id="3336e-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="3336e-874">El ejemplo siguiente se muestra un **ComplexType** elemento con cinco **propiedad** elementos:</span><span class="sxs-lookup"><span data-stu-id="3336e-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="3336e-875">Aplicación de elemento RowType</span><span class="sxs-lookup"><span data-stu-id="3336e-875">RowType Element Application</span></span>

<span data-ttu-id="3336e-876">**Propiedad** elementos (como los elementos secundarios de un **RowType** elemento) definen la forma y características de los datos que se pueden pasar o devolver desde una función definida por el modelo.</span><span class="sxs-lookup"><span data-stu-id="3336e-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="3336e-877">El **propiedad** elemento puede tener exactamente uno de los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="3336e-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="3336e-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="3336e-878">CollectionType</span></span>
-   <span data-ttu-id="3336e-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="3336e-879">ReferenceType</span></span>
-   <span data-ttu-id="3336e-880">RowType</span><span class="sxs-lookup"><span data-stu-id="3336e-880">RowType</span></span>

<span data-ttu-id="3336e-881">El **propiedad** elemento puede tener cualquier número elementos de anotación de secundarios.</span><span class="sxs-lookup"><span data-stu-id="3336e-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-882">Elementos de anotación solo se permiten en CSDL v2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="3336e-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="3336e-883">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-883">Applicable Attributes</span></span>

<span data-ttu-id="3336e-884">En la tabla siguiente se describe los atributos que se pueden aplicar a la **propiedad** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="3336e-885">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-885">Attribute Name</span></span>                                                     | <span data-ttu-id="3336e-886">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-886">Is Required</span></span> | <span data-ttu-id="3336e-887">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-888">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-888">**Name**</span></span>                                                           | <span data-ttu-id="3336e-889">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-889">Yes</span></span>         | <span data-ttu-id="3336e-890">Nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="3336e-891">**Type**</span><span class="sxs-lookup"><span data-stu-id="3336e-891">**Type**</span></span>                                                           | <span data-ttu-id="3336e-892">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-892">Yes</span></span>         | <span data-ttu-id="3336e-893">Tipo del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="3336e-894">**Que acepta valores null**</span><span class="sxs-lookup"><span data-stu-id="3336e-894">**Nullable**</span></span>                                                       | <span data-ttu-id="3336e-895">No</span><span class="sxs-lookup"><span data-stu-id="3336e-895">No</span></span>          | <span data-ttu-id="3336e-896">**True** (el valor predeterminado) o **False** dependiendo de si la propiedad puede tener un valor null.</span><span class="sxs-lookup"><span data-stu-id="3336e-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="3336e-897">> En la versión 1 CSDL debe tener una propiedad de tipo complejo `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="3336e-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="3336e-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="3336e-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="3336e-899">No</span><span class="sxs-lookup"><span data-stu-id="3336e-899">No</span></span>          | <span data-ttu-id="3336e-900">Valor predeterminado de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="3336e-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="3336e-902">No</span><span class="sxs-lookup"><span data-stu-id="3336e-902">No</span></span>          | <span data-ttu-id="3336e-903">Longitud máxima del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="3336e-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="3336e-905">No</span><span class="sxs-lookup"><span data-stu-id="3336e-905">No</span></span>          | <span data-ttu-id="3336e-906">**True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="3336e-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="3336e-907">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="3336e-907">**Precision**</span></span>                                                      | <span data-ttu-id="3336e-908">No</span><span class="sxs-lookup"><span data-stu-id="3336e-908">No</span></span>          | <span data-ttu-id="3336e-909">Precisión del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="3336e-910">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="3336e-910">**Scale**</span></span>                                                          | <span data-ttu-id="3336e-911">No</span><span class="sxs-lookup"><span data-stu-id="3336e-911">No</span></span>          | <span data-ttu-id="3336e-912">Escala del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="3336e-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3336e-913">**SRID**</span></span>                                                           | <span data-ttu-id="3336e-914">No</span><span class="sxs-lookup"><span data-stu-id="3336e-914">No</span></span>          | <span data-ttu-id="3336e-915">Identificador de referencia espacial de sistema.</span><span class="sxs-lookup"><span data-stu-id="3336e-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3336e-916">Válido solo para las propiedades de tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="3336e-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="3336e-917">Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3336e-917">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="3336e-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3336e-918">**Unicode**</span></span>                                                        | <span data-ttu-id="3336e-919">No</span><span class="sxs-lookup"><span data-stu-id="3336e-919">No</span></span>          | <span data-ttu-id="3336e-920">**True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="3336e-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="3336e-921">**intercalación**</span><span class="sxs-lookup"><span data-stu-id="3336e-921">**Collation**</span></span>                                                      | <span data-ttu-id="3336e-922">No</span><span class="sxs-lookup"><span data-stu-id="3336e-922">No</span></span>          | <span data-ttu-id="3336e-923">Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="3336e-924">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **propiedad** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="3336e-925">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-926">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="3336e-927">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-927">Example</span></span>

<span data-ttu-id="3336e-928">El ejemplo siguiente muestra **propiedad** elementos que se usan para definir la forma del tipo de valor devuelto de una función definida por el modelo.</span><span class="sxs-lookup"><span data-stu-id="3336e-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="3336e-929">PropertyRef (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="3336e-930">El **PropertyRef** elemento de lenguaje de definición de esquemas conceptuales (CSDL) hace referencia a una propiedad de un tipo de entidad para indicar que la propiedad realizará uno de los roles siguientes:</span><span class="sxs-lookup"><span data-stu-id="3336e-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="3336e-931">Parte de la clave de la entidad (una propiedad o un conjunto de propiedades de un tipo de entidad que determinan la identidad).</span><span class="sxs-lookup"><span data-stu-id="3336e-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="3336e-932">Uno o varios **PropertyRef** elementos se pueden utilizar para definir una clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="3336e-933">El extremo dependiente o principal de una restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="3336e-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="3336e-934">El **PropertyRef** elemento solo puede tener elementos annotation (cero o más) como elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="3336e-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-935">Elementos de anotación solo se permiten en CSDL v2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="3336e-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-936">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-936">Applicable Attributes</span></span>

<span data-ttu-id="3336e-937">En la tabla siguiente se describe los atributos que se pueden aplicar a la **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="3336e-938">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-938">Attribute Name</span></span> | <span data-ttu-id="3336e-939">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-939">Is Required</span></span> | <span data-ttu-id="3336e-940">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="3336e-941">**Name**</span><span class="sxs-lookup"><span data-stu-id="3336e-941">**Name**</span></span>       | <span data-ttu-id="3336e-942">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-942">Yes</span></span>         | <span data-ttu-id="3336e-943">Nombre de la propiedad a la que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="3336e-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-944">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="3336e-945">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-946">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-947">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-947">Example</span></span>

<span data-ttu-id="3336e-948">El ejemplo siguiente define un tipo de entidad (**libro**).</span><span class="sxs-lookup"><span data-stu-id="3336e-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="3336e-949">La clave de entidad se define haciendo referencia a la **ISBN** propiedad del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="3336e-950">En el ejemplo siguiente, dos **PropertyRef** elementos se utilizan para indicar que dos propiedades (**Id** y **PublisherId**) son los extremos principales y dependientes de una referencial restricción.</span><span class="sxs-lookup"><span data-stu-id="3336e-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="3336e-951">ReferenceType (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="3336e-952">El **ReferenceType** elemento de lenguaje de definición de esquemas conceptuales (CSDL) especifica una referencia a un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="3336e-953">El **ReferenceType** elemento puede ser un elemento secundario de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="3336e-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="3336e-954">ReturnType (función)</span><span class="sxs-lookup"><span data-stu-id="3336e-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="3336e-955">Parámetro</span><span class="sxs-lookup"><span data-stu-id="3336e-955">Parameter</span></span>
-   <span data-ttu-id="3336e-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="3336e-956">CollectionType</span></span>

<span data-ttu-id="3336e-957">El **ReferenceType** elemento se usa al definir un tipo de parámetro o valor devuelto para una función.</span><span class="sxs-lookup"><span data-stu-id="3336e-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="3336e-958">Un **ReferenceType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-959">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-960">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-961">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-961">Applicable Attributes</span></span>

<span data-ttu-id="3336e-962">En la tabla siguiente se describe los atributos que se pueden aplicar a la **ReferenceType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="3336e-963">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-963">Attribute Name</span></span> | <span data-ttu-id="3336e-964">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-964">Is Required</span></span> | <span data-ttu-id="3336e-965">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="3336e-966">**Type**</span><span class="sxs-lookup"><span data-stu-id="3336e-966">**Type**</span></span>       | <span data-ttu-id="3336e-967">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-967">Yes</span></span>         | <span data-ttu-id="3336e-968">Nombre del tipo de entidad al que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="3336e-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-969">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ReferenceType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="3336e-970">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-971">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-972">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-972">Example</span></span>

<span data-ttu-id="3336e-973">El ejemplo siguiente se muestra el **ReferenceType** elemento que se utiliza como elemento secundario de un **parámetro** elemento en una función definida por modelo que acepta una referencia a un **persona** entidad tipo:</span><span class="sxs-lookup"><span data-stu-id="3336e-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="3336e-974">El ejemplo siguiente se muestra el **ReferenceType** elemento que se utiliza como elemento secundario de un **ReturnType** elemento (función) en una función definida por modelo que devuelve una referencia a un **persona**tipo de entidad:</span><span class="sxs-lookup"><span data-stu-id="3336e-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="3336e-975">ReferentialConstraint (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="3336e-976">Un **ReferentialConstraint** elemento de lenguaje de definición de esquemas conceptuales (CSDL) define la funcionalidad que es similar a una restricción de integridad referencial en una base de datos relacional.</span><span class="sxs-lookup"><span data-stu-id="3336e-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="3336e-977">Del mismo modo que una columna (o columnas) de una tabla de base de datos puede hacer referencia a la clave principal de otra tabla, una propiedad (o propiedades) de un tipo de entidad puede hacer referencia a la clave de entidad de otro tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="3336e-978">El tipo de entidad que se hace referencia se denomina el *extremo principal* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="3336e-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="3336e-979">El tipo de entidad que hace referencia el extremo principal se denomina el *extremo dependiente* de la restricción.</span><span class="sxs-lookup"><span data-stu-id="3336e-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="3336e-980">Si una clave externa que se expone en un tipo de entidad hace referencia a una propiedad en otro tipo de entidad, el **ReferentialConstraint** elemento define una asociación entre los dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="3336e-981">Dado que el **ReferentialConstraint** elemento proporciona información acerca de cómo dos tipos de entidad están relacionadas, ningún elemento AssociationSetMapping correspondiente es necesaria en el lenguaje de especificación de asignaciones (MSL).</span><span class="sxs-lookup"><span data-stu-id="3336e-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="3336e-982">Una asociación entre dos tipos de entidad que no tienen claves externas expuestas debe tener su correspondiente **AssociationSetMapping** elemento con el fin de asignar la información de asociación al origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="3336e-983">Si una clave externa no se expone en un tipo de entidad, el **ReferentialConstraint** elemento solo puede definir una restricción de clave principal de clave principal entre el tipo de entidad y otro tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="3336e-984">Un **ReferentialConstraint** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-985">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-986">Entidad de seguridad (exactamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="3336e-987">Dependiente (exactamente un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="3336e-988">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-989">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-989">Applicable Attributes</span></span>

<span data-ttu-id="3336e-990">El **ReferentialConstraint** elemento puede tener cualquier número de atributos de anotación (atributos XML personalizados).</span><span class="sxs-lookup"><span data-stu-id="3336e-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="3336e-991">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-992">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3336e-993">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-993">Example</span></span>

<span data-ttu-id="3336e-994">El ejemplo siguiente se muestra un **ReferentialConstraint** elemento que se va a utilizar como parte de la definición de la **PublishedBy** asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="3336e-995">ReturnType (función) (elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="3336e-996">El **ReturnType** elemento (función) en el lenguaje de definición de esquemas conceptuales (CSDL) especifica el tipo de valor devuelto para una función que se define en un elemento Function.</span><span class="sxs-lookup"><span data-stu-id="3336e-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="3336e-997">Una función también se puede especificar el tipo de valor devuelto con un **ReturnType** atributo.</span><span class="sxs-lookup"><span data-stu-id="3336e-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="3336e-998">Devolver tipos pueden ser cualquiera **EdmSimpleType**, tipo de entidad, tipo complejo, tipo de fila, tipo ref o una colección de uno de estos tipos.</span><span class="sxs-lookup"><span data-stu-id="3336e-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="3336e-999">Se puede especificar el tipo de valor devuelto de una función con cualquiera el **tipo** atributo de la **ReturnType** elemento (función), o con uno de los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="3336e-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="3336e-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="3336e-1000">CollectionType</span></span>
-   <span data-ttu-id="3336e-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="3336e-1001">ReferenceType</span></span>
-   <span data-ttu-id="3336e-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="3336e-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-1003">Un modelo no se validará si especifica una función de valor devuelto de tipo con ambos el **tipo** atributo de la **ReturnType** elemento (función) y uno de los elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="3336e-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-1004">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-1004">Applicable Attributes</span></span>

<span data-ttu-id="3336e-1005">En la tabla siguiente se describe los atributos que se pueden aplicar a la **ReturnType** elemento (función).</span><span class="sxs-lookup"><span data-stu-id="3336e-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="3336e-1006">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-1006">Attribute Name</span></span> | <span data-ttu-id="3336e-1007">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-1007">Is Required</span></span> | <span data-ttu-id="3336e-1008">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="3336e-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="3336e-1009">**ReturnType**</span></span> | <span data-ttu-id="3336e-1010">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1010">No</span></span>          | <span data-ttu-id="3336e-1011">El tipo devuelto por la función.</span><span class="sxs-lookup"><span data-stu-id="3336e-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-1012">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ReturnType** elemento (función).</span><span class="sxs-lookup"><span data-stu-id="3336e-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="3336e-1013">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-1014">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-1015">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-1015">Example</span></span>

<span data-ttu-id="3336e-1016">En el ejemplo siguiente se usa un **función** elemento para definir una función que devuelve el número de años que lleva un libro en la impresión.</span><span class="sxs-lookup"><span data-stu-id="3336e-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="3336e-1017">Tenga en cuenta que se especifica el tipo de valor devuelto por la **tipo** atributo de un **ReturnType** elemento (función).</span><span class="sxs-lookup"><span data-stu-id="3336e-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="3336e-1018">ReturnType (FunctionImport) (elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="3336e-1019">El **ReturnType** (FunctionImport) (elemento) en el lenguaje de definición de esquemas conceptuales (CSDL) especifica el tipo de valor devuelto para una función que se define en un elemento FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="3336e-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="3336e-1020">Una función también se puede especificar el tipo de valor devuelto con un **ReturnType** atributo.</span><span class="sxs-lookup"><span data-stu-id="3336e-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="3336e-1021">Devolver tipos pueden ser cualquier colección de tipo de entidad, tipo complejo, o **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="3336e-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="3336e-1022">Se especifica el tipo de valor devuelto de una función con el **tipo** atributo de la **ReturnType** (FunctionImport) (elemento).</span><span class="sxs-lookup"><span data-stu-id="3336e-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-1023">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-1023">Applicable Attributes</span></span>

<span data-ttu-id="3336e-1024">En la tabla siguiente se describe los atributos que se pueden aplicar a la **ReturnType** (FunctionImport) (elemento).</span><span class="sxs-lookup"><span data-stu-id="3336e-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="3336e-1025">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-1025">Attribute Name</span></span> | <span data-ttu-id="3336e-1026">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-1026">Is Required</span></span> | <span data-ttu-id="3336e-1027">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-1028">**Type**</span><span class="sxs-lookup"><span data-stu-id="3336e-1028">**Type**</span></span>       | <span data-ttu-id="3336e-1029">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1029">No</span></span>          | <span data-ttu-id="3336e-1030">Tipo devuelto por la función.</span><span class="sxs-lookup"><span data-stu-id="3336e-1030">The type that the function returns.</span></span> <span data-ttu-id="3336e-1031">El valor debe ser una colección de ComplexType, EntityType o EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="3336e-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="3336e-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="3336e-1032">**EntitySet**</span></span>  | <span data-ttu-id="3336e-1033">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1033">No</span></span>          | <span data-ttu-id="3336e-1034">Si la función devuelve una colección de entidades tipos, el valor de la **EntitySet** deben ser el conjunto de entidades al que pertenece la colección.</span><span class="sxs-lookup"><span data-stu-id="3336e-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="3336e-1035">En caso contrario, el **EntitySet** no debe usarse el atributo.</span><span class="sxs-lookup"><span data-stu-id="3336e-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-1036">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ReturnType** (FunctionImport) (elemento).</span><span class="sxs-lookup"><span data-stu-id="3336e-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="3336e-1037">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-1038">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-1039">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-1039">Example</span></span>

<span data-ttu-id="3336e-1040">En el ejemplo siguiente se usa un **FunctionImport** que devuelve los libros en pantalla y los publicadores.</span><span class="sxs-lookup"><span data-stu-id="3336e-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="3336e-1041">Tenga en cuenta que la función devuelve dos conjuntos de resultados y, por tanto, dos **ReturnType** se especifican los elementos (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="3336e-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="3336e-1042">RowType (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="3336e-1043">Un **RowType** elemento de lenguaje de definición de esquemas conceptuales (CSDL) define una estructura sin nombre como un parámetro o tipo de valor devuelto para una función definida en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="3336e-1044">Un **RowType** elemento puede ser el elemento secundario de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="3336e-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="3336e-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="3336e-1045">CollectionType</span></span>
-   <span data-ttu-id="3336e-1046">Parámetro</span><span class="sxs-lookup"><span data-stu-id="3336e-1046">Parameter</span></span>
-   <span data-ttu-id="3336e-1047">ReturnType (función)</span><span class="sxs-lookup"><span data-stu-id="3336e-1047">ReturnType (Function)</span></span>

<span data-ttu-id="3336e-1048">Un **RowType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-1049">Propiedad (uno o más)</span><span class="sxs-lookup"><span data-stu-id="3336e-1049">Property (one or more)</span></span>
-   <span data-ttu-id="3336e-1050">Elementos de anotación (cero o más)</span><span class="sxs-lookup"><span data-stu-id="3336e-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-1051">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-1051">Applicable Attributes</span></span>

<span data-ttu-id="3336e-1052">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **RowType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="3336e-1053">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-1054">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="3336e-1055">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-1055">Example</span></span>

<span data-ttu-id="3336e-1056">El ejemplo siguiente muestra una función definida por modelo que usa un **CollectionType** elemento para especificar que la función devuelve una colección de filas (como se especifica en el **RowType** elemento).</span><span class="sxs-lookup"><span data-stu-id="3336e-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="3336e-1057">Schema (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="3336e-1058">El **esquema** elemento es el elemento raíz de una definición de modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="3336e-1059">Contiene las definiciones para los objetos, las funciones y los contenedores que conforman un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="3336e-1060">El **esquema** elemento puede contener cero o más de los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="3336e-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="3336e-1061">Using</span><span class="sxs-lookup"><span data-stu-id="3336e-1061">Using</span></span>
-   <span data-ttu-id="3336e-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="3336e-1062">EntityContainer</span></span>
-   <span data-ttu-id="3336e-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="3336e-1063">EntityType</span></span>
-   <span data-ttu-id="3336e-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="3336e-1064">EnumType</span></span>
-   <span data-ttu-id="3336e-1065">Asociación</span><span class="sxs-lookup"><span data-stu-id="3336e-1065">Association</span></span>
-   <span data-ttu-id="3336e-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="3336e-1066">ComplexType</span></span>
-   <span data-ttu-id="3336e-1067">Función</span><span class="sxs-lookup"><span data-stu-id="3336e-1067">Function</span></span>

<span data-ttu-id="3336e-1068">Un **esquema** elemento puede contener cero o un elemento de anotación.</span><span class="sxs-lookup"><span data-stu-id="3336e-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-1069">El **función** elemento y los elementos de anotación solo se permiten en CSDL v2 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="3336e-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="3336e-1070">El **esquema** elemento utiliza la **Namespace** atributo para definir el espacio de nombres para los objetos de asociación, el tipo complejo y el tipo de entidad en un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="3336e-1071">Dentro de un espacio de nombres, no puede haber dos objetos con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="3336e-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="3336e-1072">Los espacios de nombres pueden abarcar varios **esquema** varios archivos .csdl y elementos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="3336e-1073">Un espacio de nombres del modelo conceptual es diferente del espacio de nombres XML de la **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="3336e-1074">Un espacio de nombres del modelo conceptual (tal como se define por la **Namespace** atributo) es un contenedor lógico para tipos de entidad, tipos complejos y tipos de asociación.</span><span class="sxs-lookup"><span data-stu-id="3336e-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="3336e-1075">El espacio de nombres XML (indicado por el **xmlns** atributo) de un **esquema** elemento es el espacio de nombres predeterminado para los elementos secundarios y atributos de la **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="3336e-1076">Espacios de nombres XML del formulario http://schemas.microsoft.com/ado/YYYY/MM/edm (donde AAAA y MM representan un año y mes, respectivamente) se reservan para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1076">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="3336e-1077">No puede haber elementos y atributos personalizados en espacios de nombres que tengan este formato.</span><span class="sxs-lookup"><span data-stu-id="3336e-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-1078">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-1078">Applicable Attributes</span></span>

<span data-ttu-id="3336e-1079">En la tabla siguiente se describe los atributos se pueden aplicar a la **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="3336e-1080">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-1080">Attribute Name</span></span> | <span data-ttu-id="3336e-1081">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-1081">Is Required</span></span> | <span data-ttu-id="3336e-1082">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-1083">**Espacio de nombres**</span><span class="sxs-lookup"><span data-stu-id="3336e-1083">**Namespace**</span></span>  | <span data-ttu-id="3336e-1084">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1084">Yes</span></span>         | <span data-ttu-id="3336e-1085">El espacio de nombres del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="3336e-1086">El valor de la **Namespace** atributo se usa para formar el nombre completo de un tipo.</span><span class="sxs-lookup"><span data-stu-id="3336e-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="3336e-1087">Por ejemplo, si un **EntityType** denominado *cliente* está en el espacio de nombres Simple.Example.Model, el nombre completo de la **EntityType** es SimpleExampleModel.Customer.</span><span class="sxs-lookup"><span data-stu-id="3336e-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="3336e-1088">No se puede usar las siguientes cadenas como valor para el **espacio de nombres** atributo: **sistema**, **transitorios**, o **Edm**.</span><span class="sxs-lookup"><span data-stu-id="3336e-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="3336e-1089">El valor de la **espacio de nombres** atributo no puede ser el mismo que el valor de la **espacio de nombres** atributo en el elemento de esquema de SSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="3336e-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="3336e-1090">**Alias**</span></span>      | <span data-ttu-id="3336e-1091">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1091">No</span></span>          | <span data-ttu-id="3336e-1092">Un identificador usado en lugar del nombre del espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="3336e-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="3336e-1093">Por ejemplo, si un **EntityType** denominado *cliente* está en el espacio de nombres Simple.Example.Model y el valor de la **Alias** atributo es *modelo*, puede usar Model.Customer como el nombre completo de la **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="3336e-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="3336e-1094">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="3336e-1095">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-1096">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-1097">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-1097">Example</span></span>

<span data-ttu-id="3336e-1098">El ejemplo siguiente se muestra un **esquema** elemento que contiene un **EntityContainer** elemento, dos **EntityType** elementos y otra **asociación** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="3336e-1099">TypeRef (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="3336e-1100">El **TypeRef** elemento de lenguaje de definición de esquemas conceptuales (CSDL) proporciona una referencia a un tipo con nombre existente.</span><span class="sxs-lookup"><span data-stu-id="3336e-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="3336e-1101">El **TypeRef** elemento puede ser un elemento secundario del elemento CollectionType, que se usa para especificar que una función tiene una colección como un parámetro o tipo de devolución.</span><span class="sxs-lookup"><span data-stu-id="3336e-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="3336e-1102">Un **TypeRef** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="3336e-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="3336e-1103">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="3336e-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="3336e-1104">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-1105">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-1105">Applicable Attributes</span></span>

<span data-ttu-id="3336e-1106">En la tabla siguiente se describe los atributos que se pueden aplicar a la **TypeRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="3336e-1107">Tenga en cuenta que el **DefaultValue**, **MaxLength**, **FixedLength**, **precisión**, **escala**,  **Unicode**, y **intercalación** atributos solo son aplicables a **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="3336e-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="3336e-1108">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="3336e-1109">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-1109">Is Required</span></span> | <span data-ttu-id="3336e-1110">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-1111">**Type**</span><span class="sxs-lookup"><span data-stu-id="3336e-1111">**Type**</span></span>                                                           | <span data-ttu-id="3336e-1112">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1112">No</span></span>          | <span data-ttu-id="3336e-1113">El nombre del tipo al que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="3336e-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="3336e-1114">**Que acepta valores null**</span><span class="sxs-lookup"><span data-stu-id="3336e-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="3336e-1115">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1115">No</span></span>          | <span data-ttu-id="3336e-1116">**True** (el valor predeterminado) o **False** dependiendo de si la propiedad puede tener un valor null.</span><span class="sxs-lookup"><span data-stu-id="3336e-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="3336e-1117">> En la versión 1 CSDL debe tener una propiedad de tipo complejo `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="3336e-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="3336e-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="3336e-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="3336e-1119">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1119">No</span></span>          | <span data-ttu-id="3336e-1120">Valor predeterminado de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="3336e-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="3336e-1122">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1122">No</span></span>          | <span data-ttu-id="3336e-1123">Longitud máxima del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="3336e-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="3336e-1125">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1125">No</span></span>          | <span data-ttu-id="3336e-1126">**True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="3336e-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="3336e-1127">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="3336e-1127">**Precision**</span></span>                                                      | <span data-ttu-id="3336e-1128">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1128">No</span></span>          | <span data-ttu-id="3336e-1129">Precisión del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="3336e-1130">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="3336e-1130">**Scale**</span></span>                                                          | <span data-ttu-id="3336e-1131">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1131">No</span></span>          | <span data-ttu-id="3336e-1132">Escala del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="3336e-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3336e-1133">**SRID**</span></span>                                                           | <span data-ttu-id="3336e-1134">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1134">No</span></span>          | <span data-ttu-id="3336e-1135">Identificador de referencia espacial de sistema.</span><span class="sxs-lookup"><span data-stu-id="3336e-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="3336e-1136">Válido solo para las propiedades de tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="3336e-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="3336e-1137">Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3336e-1137">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="3336e-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3336e-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="3336e-1139">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1139">No</span></span>          | <span data-ttu-id="3336e-1140">**True** o **False** dependiendo de si el valor de propiedad se almacenarán como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="3336e-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="3336e-1141">**intercalación**</span><span class="sxs-lookup"><span data-stu-id="3336e-1141">**Collation**</span></span>                                                      | <span data-ttu-id="3336e-1142">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1142">No</span></span>          | <span data-ttu-id="3336e-1143">Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="3336e-1144">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="3336e-1145">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-1146">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-1147">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-1147">Example</span></span>

<span data-ttu-id="3336e-1148">El ejemplo siguiente muestra una función definida por modelo que usa el **TypeRef** elemento (como un elemento secundario de un **CollectionType** elemento) para especificar que la función acepta una colección de  **Departamento** tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="3336e-1149">Using (Elemento) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="3336e-1150">El **Using** elemento de lenguaje de definición de esquemas conceptuales (CSDL) importa el contenido de un modelo conceptual que existe en un espacio de nombres diferente.</span><span class="sxs-lookup"><span data-stu-id="3336e-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="3336e-1151">Estableciendo el valor de la **Namespace** atributo, puede hacer referencia a tipos de entidad, tipos complejos y tipos de asociación que se definen en otro modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="3336e-1152">Más de un **Using** elemento puede ser un elemento secundario de un **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-1153">El **Using** elemento en CSDL no funciona exactamente igual que un **mediante** instrucción en un lenguaje de programación.</span><span class="sxs-lookup"><span data-stu-id="3336e-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="3336e-1154">Mediante la importación de un espacio de nombres con un **mediante** instrucción en un lenguaje de programación, no afectan a los objetos en el espacio de nombres original.</span><span class="sxs-lookup"><span data-stu-id="3336e-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="3336e-1155">En CSDL, un espacio de nombres importado puede contener un tipo de entidad derivado de un tipo de entidad del espacio de nombres original.</span><span class="sxs-lookup"><span data-stu-id="3336e-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="3336e-1156">Esto puede afectar a los conjuntos de entidades declarados en el espacio de nombres original.</span><span class="sxs-lookup"><span data-stu-id="3336e-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="3336e-1157">El **Using** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="3336e-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="3336e-1158">Documentación (cero o un elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="3336e-1159">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="3336e-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="3336e-1160">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-1160">Applicable Attributes</span></span>

<span data-ttu-id="3336e-1161">En la tabla siguiente se describe los atributos se pueden aplicar a la **Using** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="3336e-1162">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="3336e-1162">Attribute Name</span></span> | <span data-ttu-id="3336e-1163">Es necesario</span><span class="sxs-lookup"><span data-stu-id="3336e-1163">Is Required</span></span> | <span data-ttu-id="3336e-1164">Valor</span><span class="sxs-lookup"><span data-stu-id="3336e-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3336e-1165">**Espacio de nombres**</span><span class="sxs-lookup"><span data-stu-id="3336e-1165">**Namespace**</span></span>  | <span data-ttu-id="3336e-1166">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1166">Yes</span></span>         | <span data-ttu-id="3336e-1167">Nombre del espacio de nombres importado.</span><span class="sxs-lookup"><span data-stu-id="3336e-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="3336e-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="3336e-1168">**Alias**</span></span>      | <span data-ttu-id="3336e-1169">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1169">Yes</span></span>         | <span data-ttu-id="3336e-1170">Un identificador usado en lugar del nombre del espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="3336e-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="3336e-1171">Aunque este atributo es obligatorio, no es necesario usarlo en lugar del nombre del espacio de nombres para calificar los nombres de los objetos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="3336e-1172">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **Using** elemento.</span><span class="sxs-lookup"><span data-stu-id="3336e-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="3336e-1173">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="3336e-1174">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="3336e-1175">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-1175">Example</span></span>

<span data-ttu-id="3336e-1176">En el ejemplo siguiente se muestra el **Using** elemento que se usa para importar un espacio de nombres que está definida en otra parte.</span><span class="sxs-lookup"><span data-stu-id="3336e-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="3336e-1177">Tenga en cuenta que el espacio de nombres para el **esquema** elemento mostrado es `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="3336e-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="3336e-1178">El `Address` propiedad en el `Publisher` **EntityType** es un tipo complejo que se define en el `ExtendedBooksModel` espacio de nombres (importado con la **Using** elemento).</span><span class="sxs-lookup"><span data-stu-id="3336e-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="3336e-1179">Atributos de anotación (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="3336e-1180">En el lenguaje de definición de esquemas conceptuales (CSDL), los atributos de anotación son atributos XML personalizados del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="3336e-1181">Además de tener una estructura XML válida, los atributos de anotación deben cumplir las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="3336e-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="3336e-1182">Los atributos de anotación no deben estar en ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="3336e-1183">Se pueden aplicar varios atributos de anotación a un elemento CSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="3336e-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="3336e-1184">Dos atributos de anotación cualesquiera no pueden tener el mismo nombre completo.</span><span class="sxs-lookup"><span data-stu-id="3336e-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="3336e-1185">Los atributos de anotación se pueden usar para proporcionar metadatos adicionales sobre los elementos en un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="3336e-1186">Pueden tener acceso a los metadatos contenidos en elementos de anotación en tiempo de ejecución mediante las clases del espacio de nombres System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="3336e-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="3336e-1187">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-1187">Example</span></span>

<span data-ttu-id="3336e-1188">El ejemplo siguiente se muestra un **EntityType** elemento con un atributo de anotación (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="3336e-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="3336e-1189">El ejemplo también muestra un elemento Annotation aplicado al elemento de tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="3336e-1190">El siguiente código recupera los metadatos del atributo Annotation y los escribe en la consola:</span><span class="sxs-lookup"><span data-stu-id="3336e-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="3336e-1191">El código anterior supone que el archivo `School.csdl` está en el directorio de resultados del proyecto y que se han agregado las siguientes instrucciones `Imports` y `Using` al proyecto:</span><span class="sxs-lookup"><span data-stu-id="3336e-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="3336e-1192">Elementos Annotation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="3336e-1193">Los elementos Annotation en el lenguaje de definición de esquemas conceptuales (CSDL) son los elementos XML personalizados del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="3336e-1194">Además de tener una estructura XML válida, lo siguiente debe ser verdadero para los elementos Annotation:</span><span class="sxs-lookup"><span data-stu-id="3336e-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="3336e-1195">Los elementos Annotation no deben estar en un espacio de nombres XML que esté reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="3336e-1196">Más de un elemento Annotation puede ser un elemento secundario de un elemento CSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="3336e-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="3336e-1197">Los nombres completos de dos elementos Annotation cualesquiera no deben ser los mismos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="3336e-1198">Los elementos Annotation deben aparecer después de todos los demás elementos secundarios de un elemento CSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="3336e-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="3336e-1199">Los elementos Annotation pueden utilizarse para proporcionar metadatos adicionales sobre los elementos en un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="3336e-1200">A partir de .NET Framework versión 4, pueden obtenerse metadatos contenidos en elementos de anotación en tiempo de ejecución mediante las clases del espacio de nombres System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="3336e-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="3336e-1201">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-1201">Example</span></span>

<span data-ttu-id="3336e-1202">El ejemplo siguiente se muestra un **EntityType** elemento con un elemento annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="3336e-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="3336e-1203">El ejemplo también muestra un atributo de anotación aplicado al elemento de tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="3336e-1204">El siguiente código recupera los metadatos del elemento de anotación y los escribe en la consola:</span><span class="sxs-lookup"><span data-stu-id="3336e-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="3336e-1205">El código anterior supone que el archivo School.csdl está en el directorio de resultados del proyecto y que se han agregado las siguientes instrucciones `Imports` y `Using` al proyecto:</span><span class="sxs-lookup"><span data-stu-id="3336e-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="3336e-1206">Tipos de modelos conceptuales (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="3336e-1207">Lenguaje de definición de esquemas conceptuales (CSDL) admite un conjunto de tipos de datos primitivos abstractos, denominados **EDMSimpleTypes**, que definen las propiedades en un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="3336e-1208">**EDMSimpleTypes** son objetos proxy para los tipos de datos primitivos que se admiten en el almacenamiento o el entorno de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="3336e-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="3336e-1209">En la tabla siguiente se enumeran los tipos de datos primitivos admitidos por CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="3336e-1210">La tabla también enumeran las facetas que se pueden aplicar a cada **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="3336e-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="3336e-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="3336e-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="3336e-1212">Descripción</span><span class="sxs-lookup"><span data-stu-id="3336e-1212">Description</span></span>                                                | <span data-ttu-id="3336e-1213">Facetas aplicables</span><span class="sxs-lookup"><span data-stu-id="3336e-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="3336e-1214">**Edm.Binary**</span><span class="sxs-lookup"><span data-stu-id="3336e-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="3336e-1215">Contiene datos binarios.</span><span class="sxs-lookup"><span data-stu-id="3336e-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="3336e-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="3336e-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="3336e-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="3336e-1218">Contiene el valor **true** o **false**.</span><span class="sxs-lookup"><span data-stu-id="3336e-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="3336e-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="3336e-1220">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="3336e-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="3336e-1221">Contiene un valor entero de 8 bits sin signo.</span><span class="sxs-lookup"><span data-stu-id="3336e-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="3336e-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1223">**Edm.DateTime**</span><span class="sxs-lookup"><span data-stu-id="3336e-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="3336e-1224">Representa una fecha y hora.</span><span class="sxs-lookup"><span data-stu-id="3336e-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="3336e-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="3336e-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="3336e-1227">Contiene una fecha y hora como un desplazamiento en minutos con respecto a GMT.</span><span class="sxs-lookup"><span data-stu-id="3336e-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="3336e-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1229">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="3336e-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="3336e-1230">Contiene un valor numérico con una precisión y escala fijas.</span><span class="sxs-lookup"><span data-stu-id="3336e-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="3336e-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="3336e-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="3336e-1233">Contiene un número con 15 dígitos de precisión de punto flotante</span><span class="sxs-lookup"><span data-stu-id="3336e-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="3336e-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1235">**Edm.Float**</span><span class="sxs-lookup"><span data-stu-id="3336e-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="3336e-1236">Contiene un número de punto flotante con una precisión de 7 dígitos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="3336e-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1238">**Edm.Guid**</span><span class="sxs-lookup"><span data-stu-id="3336e-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="3336e-1239">Contiene un identificador único de 16 bytes.</span><span class="sxs-lookup"><span data-stu-id="3336e-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="3336e-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1241">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="3336e-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="3336e-1242">Contiene un valor entero de 16 bits con signo.</span><span class="sxs-lookup"><span data-stu-id="3336e-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="3336e-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="3336e-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="3336e-1245">Contiene un valor entero de 32 bits con signo.</span><span class="sxs-lookup"><span data-stu-id="3336e-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="3336e-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="3336e-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="3336e-1248">Contiene un valor entero de 64 bits con signo.</span><span class="sxs-lookup"><span data-stu-id="3336e-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="3336e-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1250">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="3336e-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="3336e-1251">Contiene un valor entero de 8 bits con signo.</span><span class="sxs-lookup"><span data-stu-id="3336e-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="3336e-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="3336e-1253">**Edm.String**</span></span>                   | <span data-ttu-id="3336e-1254">Contiene datos de caracteres.</span><span class="sxs-lookup"><span data-stu-id="3336e-1254">Contains character data.</span></span>                                   | <span data-ttu-id="3336e-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="3336e-1256">**Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="3336e-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="3336e-1257">Contiene una hora del día.</span><span class="sxs-lookup"><span data-stu-id="3336e-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="3336e-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="3336e-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="3336e-1259">**Edm.Geography**</span><span class="sxs-lookup"><span data-stu-id="3336e-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="3336e-1260">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="3336e-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="3336e-1262">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1263">**Edm.GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="3336e-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="3336e-1264">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1265">**Edm.GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="3336e-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="3336e-1266">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1267">**Edm.GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="3336e-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="3336e-1268">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1269">**Edm.GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="3336e-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="3336e-1270">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1271">**Edm.GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="3336e-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="3336e-1272">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1273">**Edm.GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="3336e-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="3336e-1274">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1275">**Edm.Geometry**</span><span class="sxs-lookup"><span data-stu-id="3336e-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="3336e-1276">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1277">**Edm.GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="3336e-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="3336e-1278">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1279">**Edm.GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="3336e-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="3336e-1280">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1281">**Edm.GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="3336e-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="3336e-1282">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1283">**Edm.GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="3336e-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="3336e-1284">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1285">**Edm.GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="3336e-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="3336e-1286">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1287">**Edm.GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="3336e-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="3336e-1288">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="3336e-1289">**Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="3336e-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="3336e-1290">Que acepta valores NULL, predeterminado, SRID</span><span class="sxs-lookup"><span data-stu-id="3336e-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="3336e-1291">Facets (CSDL)</span><span class="sxs-lookup"><span data-stu-id="3336e-1291">Facets (CSDL)</span></span>

<span data-ttu-id="3336e-1292">En el lenguaje de definición de esquemas conceptuales (CSDL), las facetas representan restricciones en las propiedades de tipos de entidad y tipos complejos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="3336e-1293">Las facetas aparecen como atributos XML en los elementos CSDL siguientes:</span><span class="sxs-lookup"><span data-stu-id="3336e-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="3336e-1294">Property</span><span class="sxs-lookup"><span data-stu-id="3336e-1294">Property</span></span>
-   <span data-ttu-id="3336e-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="3336e-1295">TypeRef</span></span>
-   <span data-ttu-id="3336e-1296">Parámetro</span><span class="sxs-lookup"><span data-stu-id="3336e-1296">Parameter</span></span>

<span data-ttu-id="3336e-1297">En la tabla siguiente se describen las facetas que se admiten en CSDL.</span><span class="sxs-lookup"><span data-stu-id="3336e-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="3336e-1298">Todas las facetas son opcionales.</span><span class="sxs-lookup"><span data-stu-id="3336e-1298">All facets are optional.</span></span> <span data-ttu-id="3336e-1299">Algunas facetas que se enumeran a continuación se usan por Entity Framework al generar una base de datos de un modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="3336e-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="3336e-1300">Para obtener información acerca de los tipos de datos en un modelo conceptual, vea tipos de modelos conceptuales (CSDL).</span><span class="sxs-lookup"><span data-stu-id="3336e-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="3336e-1301">Faceta</span><span class="sxs-lookup"><span data-stu-id="3336e-1301">Facet</span></span>               | <span data-ttu-id="3336e-1302">Descripción</span><span class="sxs-lookup"><span data-stu-id="3336e-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="3336e-1303">Se aplica a</span><span class="sxs-lookup"><span data-stu-id="3336e-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="3336e-1304">Se utiliza para la generación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="3336e-1304">Used for the database generation</span></span> | <span data-ttu-id="3336e-1305">La usa el runtime.</span><span class="sxs-lookup"><span data-stu-id="3336e-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="3336e-1306">**intercalación**</span><span class="sxs-lookup"><span data-stu-id="3336e-1306">**Collation**</span></span>       | <span data-ttu-id="3336e-1307">Especifica la secuencia de intercalación (o secuencia de orden) que se va a usar cuando se realicen las operaciones de comparación y ordenación sobre los valores de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="3336e-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="3336e-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="3336e-1309">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1309">Yes</span></span>                              | <span data-ttu-id="3336e-1310">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1310">No</span></span>                  |
| <span data-ttu-id="3336e-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="3336e-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="3336e-1312">Indica que el valor de propiedad se debería utilizar para las comprobaciones de la simultaneidad optimista.</span><span class="sxs-lookup"><span data-stu-id="3336e-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="3336e-1313">Todos los **EDMSimpleType** propiedades</span><span class="sxs-lookup"><span data-stu-id="3336e-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="3336e-1314">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1314">No</span></span>                               | <span data-ttu-id="3336e-1315">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1315">Yes</span></span>                 |
| <span data-ttu-id="3336e-1316">**Predetermiado**</span><span class="sxs-lookup"><span data-stu-id="3336e-1316">**Default**</span></span>         | <span data-ttu-id="3336e-1317">Especifica el valor predeterminado de la propiedad si no se proporciona ningún valor al crear las instancias.</span><span class="sxs-lookup"><span data-stu-id="3336e-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="3336e-1318">Todos los **EDMSimpleType** propiedades</span><span class="sxs-lookup"><span data-stu-id="3336e-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="3336e-1319">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1319">Yes</span></span>                              | <span data-ttu-id="3336e-1320">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1320">Yes</span></span>                 |
| <span data-ttu-id="3336e-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-1321">**FixedLength**</span></span>     | <span data-ttu-id="3336e-1322">Especifica si la longitud del valor de propiedad puede variar.</span><span class="sxs-lookup"><span data-stu-id="3336e-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="3336e-1323">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="3336e-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="3336e-1324">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1324">Yes</span></span>                              | <span data-ttu-id="3336e-1325">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1325">No</span></span>                  |
| <span data-ttu-id="3336e-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="3336e-1326">**MaxLength**</span></span>       | <span data-ttu-id="3336e-1327">Especifica la longitud máxima del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="3336e-1328">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="3336e-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="3336e-1329">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1329">Yes</span></span>                              | <span data-ttu-id="3336e-1330">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1330">No</span></span>                  |
| <span data-ttu-id="3336e-1331">**Que acepta valores null**</span><span class="sxs-lookup"><span data-stu-id="3336e-1331">**Nullable**</span></span>        | <span data-ttu-id="3336e-1332">Especifica si la propiedad puede tener un **null** valor.</span><span class="sxs-lookup"><span data-stu-id="3336e-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="3336e-1333">Todos los **EDMSimpleType** propiedades</span><span class="sxs-lookup"><span data-stu-id="3336e-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="3336e-1334">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1334">Yes</span></span>                              | <span data-ttu-id="3336e-1335">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1335">Yes</span></span>                 |
| <span data-ttu-id="3336e-1336">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="3336e-1336">**Precision**</span></span>       | <span data-ttu-id="3336e-1337">Las propiedades de tipo **Decimal**, especifica el número de dígitos que puede tener un valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="3336e-1338">Las propiedades de tipo **tiempo**, **DateTime**, y **DateTimeOffset**, especifica el número de dígitos para la parte fraccionaria de segundos del valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="3336e-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="3336e-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="3336e-1340">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1340">Yes</span></span>                              | <span data-ttu-id="3336e-1341">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1341">No</span></span>                  |
| <span data-ttu-id="3336e-1342">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="3336e-1342">**Scale**</span></span>           | <span data-ttu-id="3336e-1343">Especifica el número de dígitos que puede haber a la derecha del separador decimal para el valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="3336e-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="3336e-1344">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="3336e-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="3336e-1345">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1345">Yes</span></span>                              | <span data-ttu-id="3336e-1346">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1346">No</span></span>                  |
| <span data-ttu-id="3336e-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="3336e-1347">**SRID**</span></span>            | <span data-ttu-id="3336e-1348">Especifica el identificador de sistema de referencia espacial sistema.</span><span class="sxs-lookup"><span data-stu-id="3336e-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="3336e-1349">Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="3336e-1349">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="3336e-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="3336e-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="3336e-1351">No</span><span class="sxs-lookup"><span data-stu-id="3336e-1351">No</span></span>                               | <span data-ttu-id="3336e-1352">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1352">Yes</span></span>                 |
| <span data-ttu-id="3336e-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="3336e-1353">**Unicode**</span></span>         | <span data-ttu-id="3336e-1354">Indica si el valor de propiedad está almacenado como Unicode.</span><span class="sxs-lookup"><span data-stu-id="3336e-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="3336e-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="3336e-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="3336e-1356">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1356">Yes</span></span>                              | <span data-ttu-id="3336e-1357">Sí</span><span class="sxs-lookup"><span data-stu-id="3336e-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="3336e-1358">Al generar una base de datos de un modelo conceptual, el Asistente para generar base de datos reconocerá el valor de la **StoreGeneratedPattern** atributo un **propiedad** elemento si encuentra en la siguiente espacio de nombres: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="3336e-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="3336e-1359">Los valores admitidos para el atributo son **identidad** y **calculado**.</span><span class="sxs-lookup"><span data-stu-id="3336e-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="3336e-1360">Un valor de **identidad** generará una columna de base de datos con un valor de identidad que se genera en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="3336e-1361">Un valor de **calculado** generará una columna con un valor que se calcula en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3336e-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="3336e-1362">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3336e-1362">Example</span></span>

<span data-ttu-id="3336e-1363">En el ejemplo siguiente se muestran facetas aplicadas a las propiedades de un tipo de entidad:</span><span class="sxs-lookup"><span data-stu-id="3336e-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
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
