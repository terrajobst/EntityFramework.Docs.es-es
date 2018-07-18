---
title: Especificación de SSDL - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
caps.latest.revision: 3
ms.openlocfilehash: a9977c80d9a9401afdcad2284a705bcb28790fb8
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122720"
---
# <a name="ssdl-specification"></a><span data-ttu-id="90d99-102">Especificación SSDL</span><span class="sxs-lookup"><span data-stu-id="90d99-102">SSDL Specification</span></span>
<span data-ttu-id="90d99-103">El lenguaje de definición de esquemas de almacenamiento (SSDL) es un lenguaje basado en XML que describe el modelo de almacenamiento de una aplicación Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="90d99-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="90d99-104">En una aplicación de Entity Framework, los metadatos del modelo de almacenamiento se carga desde un archivo .ssdl (escrito en SSDL) en una instancia de la System.Data.Metadata.Edm.StoreItemCollection y es accesible mediante el uso de métodos en el Clase System.Data.Metadata.Edm.MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="90d99-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="90d99-105">Entity Framework usa los metadatos del modelo de almacenamiento para traducir las consultas con el modelo conceptual en comandos específicos del almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="90d99-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="90d99-106">El Diseñador de Entity Framework (EF Designer) almacena información de modelo de almacenamiento en un archivo .edmx en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="90d99-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="90d99-107">En tiempo de compilación Entity Designer usa información en un archivo .edmx para crear el archivo .ssdl que Entity Framework necesita en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="90d99-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="90d99-108">Las versiones de SSDL se diferencian por los espacios de nombres XML.</span><span class="sxs-lookup"><span data-stu-id="90d99-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="90d99-109">Versión SSDL</span><span class="sxs-lookup"><span data-stu-id="90d99-109">SSDL Version</span></span> | <span data-ttu-id="90d99-110">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="90d99-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="90d99-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="90d99-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="90d99-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="90d99-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="90d99-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="90d99-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="90d99-114">Association (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-114">Association Element (SSDL)</span></span>

<span data-ttu-id="90d99-115">Un **asociación** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) especifica las columnas de tabla que participan en una restricción foreign key en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="90d99-116">Dos elementos de extremo secundario necesario especifican las tablas en los extremos de la asociación y la multiplicidad en cada extremo.</span><span class="sxs-lookup"><span data-stu-id="90d99-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="90d99-117">Un elemento ReferentialConstraint opcional especifica los extremos principales y dependientes de la asociación, así como las columnas que participan.</span><span class="sxs-lookup"><span data-stu-id="90d99-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="90d99-118">Si no hay ningún **ReferentialConstraint** elemento está presente, se debe usar un elemento AssociationSetMapping para especificar las asignaciones de columnas para la asociación.</span><span class="sxs-lookup"><span data-stu-id="90d99-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="90d99-119">El **asociación** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-120">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="90d99-121">End (exactamente dos)</span><span class="sxs-lookup"><span data-stu-id="90d99-121">End (exactly two)</span></span>
-   <span data-ttu-id="90d99-122">ReferentialConstraint (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="90d99-123">Elementos de anotación (cero o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-124">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-124">Applicable Attributes</span></span>

<span data-ttu-id="90d99-125">En la tabla siguiente se describe los atributos que se pueden aplicar a la **asociación** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="90d99-126">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-126">Attribute Name</span></span> | <span data-ttu-id="90d99-127">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-127">Is Required</span></span> | <span data-ttu-id="90d99-128">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="90d99-129">**Name**</span></span>       | <span data-ttu-id="90d99-130">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-130">Yes</span></span>         | <span data-ttu-id="90d99-131">El nombre de la restricción de clave externa correspondiente de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-132">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **asociación** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="90d99-133">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-134">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-135">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-135">Example</span></span>

<span data-ttu-id="90d99-136">El ejemplo siguiente se muestra un **asociación** elemento que se usa un **ReferentialConstraint** elemento para especificar las columnas que participan en la **FK\_CustomerOrders**  restricción de clave externa:</span><span class="sxs-lookup"><span data-stu-id="90d99-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a><span data-ttu-id="90d99-137">AssociationSet (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="90d99-138">El **AssociationSet** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa una restricción de clave externa entre dos tablas en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="90d99-139">Las columnas de tabla que participan en la restricción de clave externa se especifican en un elemento Association.</span><span class="sxs-lookup"><span data-stu-id="90d99-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="90d99-140">El **asociación** elemento que corresponde a un determinado **AssociationSet** elemento se especifica en el **asociación** atributo de la **AssociationSet**  elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="90d99-141">Conjuntos de asociaciones SSDL se asignan a los conjuntos de asociaciones CSDL por un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="90d99-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="90d99-142">Sin embargo, si establece la asociación CSDL para una determinada asociación CSDL se define mediante el uso de un elemento ReferentialConstraint, no existe la correspondiente **AssociationSetMapping** elemento es necesario.</span><span class="sxs-lookup"><span data-stu-id="90d99-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="90d99-143">En este caso, si un **AssociationSetMapping** elemento está presente, las asignaciones que define serán reemplazadas por el **ReferentialConstraint** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="90d99-144">El **AssociationSet** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-145">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="90d99-146">End (cero o dos)</span><span class="sxs-lookup"><span data-stu-id="90d99-146">End (zero or two)</span></span>
-   <span data-ttu-id="90d99-147">Elementos de anotación (cero o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-148">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-148">Applicable Attributes</span></span>

<span data-ttu-id="90d99-149">En la tabla siguiente se describe los atributos que se pueden aplicar a la **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="90d99-150">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-150">Attribute Name</span></span>  | <span data-ttu-id="90d99-151">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-151">Is Required</span></span> | <span data-ttu-id="90d99-152">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="90d99-153">**Name**</span></span>        | <span data-ttu-id="90d99-154">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-154">Yes</span></span>         | <span data-ttu-id="90d99-155">El nombre de la restricción de clave externa representada por el conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="90d99-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="90d99-156">**Asociación**</span><span class="sxs-lookup"><span data-stu-id="90d99-156">**Association**</span></span> | <span data-ttu-id="90d99-157">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-157">Yes</span></span>         | <span data-ttu-id="90d99-158">El nombre de la asociación que define las columnas que participan en la restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="90d99-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-159">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="90d99-160">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-161">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-162">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-162">Example</span></span>

<span data-ttu-id="90d99-163">El ejemplo siguiente se muestra un **AssociationSet** elemento que representa el `FK_CustomerOrders` restricción foreign key en la base de datos subyacente:</span><span class="sxs-lookup"><span data-stu-id="90d99-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="90d99-164">CollectionType (elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="90d99-165">El **CollectionType** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) especifica que el tipo de valor devuelto de una función es una colección.</span><span class="sxs-lookup"><span data-stu-id="90d99-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="90d99-166">El **CollectionType** es un elemento secundario del elemento ReturnType.</span><span class="sxs-lookup"><span data-stu-id="90d99-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="90d99-167">El tipo de colección se especifica mediante el elemento secundario de tipo de fila:</span><span class="sxs-lookup"><span data-stu-id="90d99-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="90d99-168">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **CollectionType** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="90d99-169">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-170">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-171">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-171">Example</span></span>

<span data-ttu-id="90d99-172">El ejemplo siguiente muestra una función que usa un **CollectionType** elemento para especificar que la función devuelve una colección de filas.</span><span class="sxs-lookup"><span data-stu-id="90d99-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="90d99-173">CommandText (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="90d99-174">El **CommandText** en lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario del elemento Function que le permite definir una instrucción SQL que se ejecuta en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="90d99-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="90d99-175">El **CommandText** elemento le permite agregar funcionalidad similar a un procedimiento almacenado en la base de datos, pero define el **CommandText** elemento del modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="90d99-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="90d99-176">El **CommandText** elemento no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="90d99-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="90d99-177">El cuerpo de la **CommandText** elemento debe ser una instrucción SQL válida para la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="90d99-178">No hay atributos son aplicables a la **CommandText** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-179">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-179">Example</span></span>

<span data-ttu-id="90d99-180">El ejemplo siguiente se muestra un **función** elemento con un elemento secundario **CommandText** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="90d99-181">Exponer el **UpdateProductInOrder** funcionar como un método en la clase ObjectContext, impórtelo en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="90d99-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="90d99-182">DefiningQuery (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="90d99-183">El **DefiningQuery** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) permite ejecutar una instrucción SQL directamente en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="90d99-184">El **DefiningQuery** elemento se utiliza normalmente como una vista de base de datos, pero la vista se define en el modelo de almacenamiento en lugar de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="90d99-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="90d99-185">La vista definida en un **DefiningQuery** elemento puede asignarse a un tipo de entidad en el modelo conceptual a través de un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="90d99-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="90d99-186">Estas asignaciones son de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="90d99-186">These mappings are read-only.</span></span>  

<span data-ttu-id="90d99-187">La sintaxis de SSDL siguiente muestra la declaración de un **EntitySet** seguido por el **DefiningQuery** elemento que contiene una consulta usada para recuperar la vista.</span><span class="sxs-lookup"><span data-stu-id="90d99-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

<span data-ttu-id="90d99-188">Puede usar los procedimientos almacenados en Entity Framework para habilitar escenarios de lectura y escritura a través de vistas.</span><span class="sxs-lookup"><span data-stu-id="90d99-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="90d99-189">Puede usar una vista del origen de datos o una vista de Entity SQL como la tabla base para recuperar datos y para el procesamiento de cambios por procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="90d99-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="90d99-190">Puede usar el **DefiningQuery** elemento de destino de Microsoft SQL Server Compact 3.5.</span><span class="sxs-lookup"><span data-stu-id="90d99-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="90d99-191">Aunque SQL Server Compact 3.5 no admite procedimientos almacenados, puede implementar una funcionalidad similar con el **DefiningQuery** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="90d99-192">Otro caso donde puede ser útil es en la creación de procedimientos almacenados para resolver una desigualdad entre los tipos de datos utilizados en el lenguaje de programación y los del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="90d99-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="90d99-193">Podría escribir un **DefiningQuery** que toma un cierto conjunto de parámetros y, a continuación, llama a un procedimiento almacenado con un conjunto diferente de parámetros, por ejemplo, un procedimiento almacenado que elimina los datos.</span><span class="sxs-lookup"><span data-stu-id="90d99-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="90d99-194">Dependent (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="90d99-195">El **dependientes** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario al elemento ReferentialConstraint que define el extremo dependiente de una restricción de clave externa (también llamada restricción referencial).</span><span class="sxs-lookup"><span data-stu-id="90d99-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="90d99-196">El **dependientes** elemento especifica la columna (o columnas) en una tabla que hacen referencia a una columna de clave principal (o columnas).</span><span class="sxs-lookup"><span data-stu-id="90d99-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="90d99-197">**PropertyRef** elementos especifican las columnas que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="90d99-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="90d99-198">La entidad de seguridad de elemento especifica las columnas de clave principal que hacen referencia las columnas que se especifican en el **dependientes** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="90d99-199">El **dependientes** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-200">PropertyRef (uno o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="90d99-201">Elementos de anotación (cero o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-202">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-202">Applicable Attributes</span></span>

<span data-ttu-id="90d99-203">En la tabla siguiente se describe los atributos que se pueden aplicar a la **dependientes** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="90d99-204">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-204">Attribute Name</span></span> | <span data-ttu-id="90d99-205">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-205">Is Required</span></span> | <span data-ttu-id="90d99-206">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-207">**Rol**</span><span class="sxs-lookup"><span data-stu-id="90d99-207">**Role**</span></span>       | <span data-ttu-id="90d99-208">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-208">Yes</span></span>         | <span data-ttu-id="90d99-209">El mismo valor que el **rol** atributo (si se utiliza) del elemento final correspondiente; en caso contrario, el nombre de la tabla que contiene la columna de referencia.</span><span class="sxs-lookup"><span data-stu-id="90d99-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-210">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **dependientes** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="90d99-211">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="90d99-212">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-213">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-213">Example</span></span>

<span data-ttu-id="90d99-214">En el ejemplo siguiente se muestra un elemento Association que usa un **ReferentialConstraint** elemento para especificar las columnas que participan en la **FK\_CustomerOrders** clave externa restricción.</span><span class="sxs-lookup"><span data-stu-id="90d99-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="90d99-215">El **dependientes** elemento especifica el **CustomerId** columna de la **orden** tabla como el extremo dependiente de la restricción.</span><span class="sxs-lookup"><span data-stu-id="90d99-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a><span data-ttu-id="90d99-216">Documentation (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="90d99-217">El **documentación** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) se puede usar para proporcionar información sobre un objeto que se define en un elemento primario.</span><span class="sxs-lookup"><span data-stu-id="90d99-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="90d99-218">El **documentación** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-219">**Resumen**: una breve descripción del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="90d99-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="90d99-220">(cero o un elemento).</span><span class="sxs-lookup"><span data-stu-id="90d99-220">(zero or one element)</span></span>
-   <span data-ttu-id="90d99-221">**LongDescription**: una descripción extensa del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="90d99-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="90d99-222">(cero o un elemento).</span><span class="sxs-lookup"><span data-stu-id="90d99-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-223">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-223">Applicable Attributes</span></span>

<span data-ttu-id="90d99-224">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **documentación** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="90d99-225">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="90d99-226">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-227">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-227">Example</span></span>

<span data-ttu-id="90d99-228">El ejemplo siguiente se muestra el **documentación** como un elemento secundario de un elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="90d99-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a><span data-ttu-id="90d99-229">End (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-229">End Element (SSDL)</span></span>

<span data-ttu-id="90d99-230">El **final** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) especifica la tabla y el número de filas de un extremo de una restricción foreign key en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="90d99-231">El **final** elemento puede ser un elemento secundario del elemento Association o el elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="90d99-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="90d99-232">En cada caso, los posibles elementos secundarios y atributos aplicables son diferentes.</span><span class="sxs-lookup"><span data-stu-id="90d99-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="90d99-233">Elemento End como elemento secundario del elemento Association</span><span class="sxs-lookup"><span data-stu-id="90d99-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="90d99-234">Un **final** elemento (como un elemento secundario de la **asociación** elemento) especifica la tabla y el número de filas al final de una restricción foreign key con la **tipo** y **Multiplicidad** respectivamente.</span><span class="sxs-lookup"><span data-stu-id="90d99-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="90d99-235">Los extremos de una restricción de clave externa se definen como parte de una asociación SSDL, la cual debe tener exactamente dos extremos.</span><span class="sxs-lookup"><span data-stu-id="90d99-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="90d99-236">Un **final** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-237">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="90d99-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="90d99-238">OnDelete (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="90d99-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="90d99-239">Elementos annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="90d99-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="90d99-240">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-240">Applicable Attributes</span></span>

<span data-ttu-id="90d99-241">En la tabla siguiente se describe los atributos que se pueden aplicar a la **final** elemento cuando está el elemento secundario de un **asociación** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="90d99-242">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-242">Attribute Name</span></span>   | <span data-ttu-id="90d99-243">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-243">Is Required</span></span> | <span data-ttu-id="90d99-244">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-245">**Type**</span><span class="sxs-lookup"><span data-stu-id="90d99-245">**Type**</span></span>         | <span data-ttu-id="90d99-246">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-246">Yes</span></span>         | <span data-ttu-id="90d99-247">El nombre completo del conjunto de entidades SSDL que es el final de la restricción foreign key.</span><span class="sxs-lookup"><span data-stu-id="90d99-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="90d99-248">**Rol**</span><span class="sxs-lookup"><span data-stu-id="90d99-248">**Role**</span></span>         | <span data-ttu-id="90d99-249">No</span><span class="sxs-lookup"><span data-stu-id="90d99-249">No</span></span>          | <span data-ttu-id="90d99-250">El valor de la **rol** atributo en la entidad de seguridad o dependiente del elemento del elemento ReferentialConstraint correspondiente (si se usa).</span><span class="sxs-lookup"><span data-stu-id="90d99-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="90d99-251">**Multiplicidad**</span><span class="sxs-lookup"><span data-stu-id="90d99-251">**Multiplicity**</span></span> | <span data-ttu-id="90d99-252">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-252">Yes</span></span>         | <span data-ttu-id="90d99-253">**1**, **de 0.. 1**, o **\*** según el número de filas que pueden estar al final de la restricción foreign key.</span><span class="sxs-lookup"><span data-stu-id="90d99-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="90d99-254">**1** indica que existe exactamente una fila al final de la restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="90d99-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="90d99-255">**de 0.. 1** indica que existe una o ninguna fila al final de la restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="90d99-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="90d99-256">**\*** indica que el cero, uno o más filas hay al final de la restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="90d99-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-257">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **final** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="90d99-258">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="90d99-259">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="90d99-260">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-260">Example</span></span>

<span data-ttu-id="90d99-261">El ejemplo siguiente se muestra un **asociación** elemento que define el **FK\_CustomerOrders** restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="90d99-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="90d99-262">El **multiplicidad** valores especificados en cada **final** elemento indican que muchas filas en el **pedidos** tabla puede asociarse con una fila en la **a los clientes**  tabla, pero solo una fila en la **clientes** tabla puede asociarse con una fila en la **pedidos** tabla.</span><span class="sxs-lookup"><span data-stu-id="90d99-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="90d99-263">Además, el **OnDelete** elemento indica que todas las filas de la **pedidos** tabla que hacen referencia a una fila determinada en el **clientes** se eliminará la tabla si la fila de el **clientes** se elimina la tabla.</span><span class="sxs-lookup"><span data-stu-id="90d99-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="90d99-264">Elemento End como elemento secundario del elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="90d99-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="90d99-265">El **final** elemento (como un elemento secundario de la **AssociationSet** elemento) especifica una tabla en un extremo de una restricción foreign key de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="90d99-266">Un **final** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-267">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="90d99-268">Elementos de anotación (cero o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="90d99-269">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-269">Applicable Attributes</span></span>

<span data-ttu-id="90d99-270">En la tabla siguiente se describe los atributos que se pueden aplicar a la **final** elemento cuando está el elemento secundario de un **AssociationSet** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="90d99-271">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-271">Attribute Name</span></span> | <span data-ttu-id="90d99-272">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-272">Is Required</span></span> | <span data-ttu-id="90d99-273">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="90d99-274">**EntitySet**</span></span>  | <span data-ttu-id="90d99-275">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-275">Yes</span></span>         | <span data-ttu-id="90d99-276">El nombre del conjunto de entidades SSDL que es el final de la restricción foreign key.</span><span class="sxs-lookup"><span data-stu-id="90d99-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="90d99-277">**Rol**</span><span class="sxs-lookup"><span data-stu-id="90d99-277">**Role**</span></span>       | <span data-ttu-id="90d99-278">No</span><span class="sxs-lookup"><span data-stu-id="90d99-278">No</span></span>          | <span data-ttu-id="90d99-279">El valor de uno de los **rol** especificados en uno de los atributos **final** elemento del elemento Association correspondiente.</span><span class="sxs-lookup"><span data-stu-id="90d99-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-280">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **final** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="90d99-281">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="90d99-282">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="90d99-283">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-283">Example</span></span>

<span data-ttu-id="90d99-284">El ejemplo siguiente se muestra un **EntityContainer** elemento con un **AssociationSet** elemento con dos **final** elementos:</span><span class="sxs-lookup"><span data-stu-id="90d99-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="90d99-285">EntityContainer (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="90d99-286">Un **EntityContainer** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) describe la estructura del origen de datos subyacente en una aplicación de Entity Framework: conjuntos de entidades SSDL (definidos en elementos EntitySet) representan las tablas en una base de datos, los tipos de entidad SSDL (definidos en elementos EntityType) representan filas de una tabla, y conjuntos de asociaciones (definidos en elementos de AssociationSet) representan restricciones de clave externa en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="90d99-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="90d99-287">Un contenedor de entidades del modelo de almacenamiento se asigna a un contenedor de entidades del modelo conceptual a través del elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="90d99-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="90d99-288">Un **EntityContainer** elemento puede tener cero o un elemento de la documentación.</span><span class="sxs-lookup"><span data-stu-id="90d99-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="90d99-289">Si un **documentación** elemento está presente, debe preceder a todos los demás elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="90d99-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="90d99-290">Un **EntityContainer** elemento puede tener cero o más de los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="90d99-291">EntitySet</span></span>
-   <span data-ttu-id="90d99-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="90d99-292">AssociationSet</span></span>
-   <span data-ttu-id="90d99-293">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="90d99-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-294">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-294">Applicable Attributes</span></span>

<span data-ttu-id="90d99-295">En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="90d99-296">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-296">Attribute Name</span></span> | <span data-ttu-id="90d99-297">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-297">Is Required</span></span> | <span data-ttu-id="90d99-298">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="90d99-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="90d99-299">**Name**</span></span>       | <span data-ttu-id="90d99-300">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-300">Yes</span></span>         | <span data-ttu-id="90d99-301">Nombre del contenedor de entidades.</span><span class="sxs-lookup"><span data-stu-id="90d99-301">The name of the entity container.</span></span> <span data-ttu-id="90d99-302">Este nombre no puede contener puntos (.).</span><span class="sxs-lookup"><span data-stu-id="90d99-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-303">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntityContainer** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="90d99-304">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-305">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-306">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-306">Example</span></span>

<span data-ttu-id="90d99-307">El ejemplo siguiente se muestra un **EntityContainer** elemento que define dos conjuntos de entidades y un conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="90d99-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="90d99-308">Observe que los nombres de los tipos de entidad y tipos de asociación están calificados mediante el nombre del espacio de nombres del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="90d99-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a><span data-ttu-id="90d99-309">EntitySet (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="90d99-310">Un **EntitySet** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa una tabla o vista en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="90d99-311">Un elemento EntityType en SSDL representa una fila en la tabla o vista.</span><span class="sxs-lookup"><span data-stu-id="90d99-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="90d99-312">El **EntityType** atributo de un **EntitySet** elemento especifica el tipo de entidad SSDL determinado que representa las filas de un conjunto de entidades SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="90d99-313">La asignación entre un conjunto de entidades CSDL y un conjunto de entidades SSDL se especifica en un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="90d99-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="90d99-314">El **EntitySet** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-315">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="90d99-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="90d99-316">DefiningQuery (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="90d99-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="90d99-317">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="90d99-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-318">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-318">Applicable Attributes</span></span>

<span data-ttu-id="90d99-319">En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="90d99-320">Algunos atributos (no mostrados aquí) se pueden calificar con el **almacenar** alias.</span><span class="sxs-lookup"><span data-stu-id="90d99-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="90d99-321">Estos atributos se utilizan el Asistente para actualizar modelo al actualizar un modelo.</span><span class="sxs-lookup"><span data-stu-id="90d99-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="90d99-322">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-322">Attribute Name</span></span> | <span data-ttu-id="90d99-323">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-323">Is Required</span></span> | <span data-ttu-id="90d99-324">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="90d99-325">**Name**</span></span>       | <span data-ttu-id="90d99-326">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-326">Yes</span></span>         | <span data-ttu-id="90d99-327">Nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="90d99-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="90d99-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="90d99-328">**EntityType**</span></span> | <span data-ttu-id="90d99-329">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-329">Yes</span></span>         | <span data-ttu-id="90d99-330">El nombre completo del tipo de entidad para el que el conjunto de entidades contiene las instancias.</span><span class="sxs-lookup"><span data-stu-id="90d99-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="90d99-331">**Esquema**</span><span class="sxs-lookup"><span data-stu-id="90d99-331">**Schema**</span></span>     | <span data-ttu-id="90d99-332">No</span><span class="sxs-lookup"><span data-stu-id="90d99-332">No</span></span>          | <span data-ttu-id="90d99-333">El esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="90d99-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="90d99-334">**Table**</span><span class="sxs-lookup"><span data-stu-id="90d99-334">**Table**</span></span>      | <span data-ttu-id="90d99-335">No</span><span class="sxs-lookup"><span data-stu-id="90d99-335">No</span></span>          | <span data-ttu-id="90d99-336">La tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="90d99-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="90d99-337">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntitySet** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="90d99-338">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-339">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-340">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-340">Example</span></span>

<span data-ttu-id="90d99-341">El ejemplo siguiente se muestra un **EntityContainer** elemento que tiene dos **EntitySet** elementos y otra **AssociationSet** elemento:</span><span class="sxs-lookup"><span data-stu-id="90d99-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="90d99-342">EntityType (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="90d99-343">Un **EntityType** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa una fila en una tabla o vista de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="90d99-344">Un elemento EntitySet de SSDL representa la tabla o vista en la que se producen las filas.</span><span class="sxs-lookup"><span data-stu-id="90d99-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="90d99-345">El **EntityType** atributo de un **EntitySet** elemento especifica el tipo de entidad SSDL determinado que representa las filas de un conjunto de entidades SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="90d99-346">La asignación entre un tipo de entidad SSDL y un tipo de entidad CSDL se especifica en un elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="90d99-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="90d99-347">El **EntityType** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-348">Documentación (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="90d99-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="90d99-349">Clave (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="90d99-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="90d99-350">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="90d99-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-351">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-351">Applicable Attributes</span></span>

<span data-ttu-id="90d99-352">En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="90d99-353">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-353">Attribute Name</span></span> | <span data-ttu-id="90d99-354">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-354">Is Required</span></span> | <span data-ttu-id="90d99-355">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="90d99-356">**Name**</span></span>       | <span data-ttu-id="90d99-357">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-357">Yes</span></span>         | <span data-ttu-id="90d99-358">Nombre del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="90d99-358">The name of the entity type.</span></span> <span data-ttu-id="90d99-359">Este valor normalmente es igual que el nombre de la tabla en la que el tipo de entidad representa una fila.</span><span class="sxs-lookup"><span data-stu-id="90d99-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="90d99-360">Este valor no puede contener ningún punto (.).</span><span class="sxs-lookup"><span data-stu-id="90d99-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-361">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="90d99-362">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-363">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-364">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-364">Example</span></span>

<span data-ttu-id="90d99-365">El ejemplo siguiente se muestra un **EntityType** elemento con dos propiedades:</span><span class="sxs-lookup"><span data-stu-id="90d99-365">The following example shows an **EntityType** element with two properties:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a><span data-ttu-id="90d99-366">Function (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-366">Function Element (SSDL)</span></span>

<span data-ttu-id="90d99-367">El **función** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) especifica un procedimiento almacenado que existe en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="90d99-368">El **función** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-369">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="90d99-370">Parámetro (cero o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="90d99-371">CommandText (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="90d99-372">ReturnType (cero o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="90d99-373">Elementos de anotación (cero o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="90d99-374">Un devuelto de tipo para una función debe especificarse con el **ReturnType** elemento o el **ReturnType** atributo (ver abajo), pero no ambos.</span><span class="sxs-lookup"><span data-stu-id="90d99-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="90d99-375">Los procedimientos almacenados que se especifican en el modelo de almacenamiento se pueden importar en el modelo conceptual de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="90d99-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="90d99-376">Para obtener más información, consulte [realizar consultas con procedimientos almacenados](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="90d99-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="90d99-377">El **función** elemento también se puede usar para definir funciones personalizadas en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="90d99-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-378">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-378">Applicable Attributes</span></span>

<span data-ttu-id="90d99-379">En la tabla siguiente se describe los atributos que se pueden aplicar a la **función** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="90d99-380">Algunos atributos (no mostrados aquí) se pueden calificar con el **almacenar** alias.</span><span class="sxs-lookup"><span data-stu-id="90d99-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="90d99-381">Estos atributos se utilizan el Asistente para actualizar modelo al actualizar un modelo.</span><span class="sxs-lookup"><span data-stu-id="90d99-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="90d99-382">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-382">Attribute Name</span></span>             | <span data-ttu-id="90d99-383">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-383">Is Required</span></span> | <span data-ttu-id="90d99-384">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="90d99-385">**Name**</span></span>                   | <span data-ttu-id="90d99-386">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-386">Yes</span></span>         | <span data-ttu-id="90d99-387">Nombre del procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="90d99-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="90d99-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="90d99-388">**ReturnType**</span></span>             | <span data-ttu-id="90d99-389">No</span><span class="sxs-lookup"><span data-stu-id="90d99-389">No</span></span>          | <span data-ttu-id="90d99-390">El tipo de valor devuelto del procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="90d99-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="90d99-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="90d99-391">**Aggregate**</span></span>              | <span data-ttu-id="90d99-392">No</span><span class="sxs-lookup"><span data-stu-id="90d99-392">No</span></span>          | <span data-ttu-id="90d99-393">**True** si el procedimiento almacenado devuelve un valor agregado; de lo contrario **False**.</span><span class="sxs-lookup"><span data-stu-id="90d99-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="90d99-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="90d99-394">**BuiltIn**</span></span>                | <span data-ttu-id="90d99-395">No</span><span class="sxs-lookup"><span data-stu-id="90d99-395">No</span></span>          | <span data-ttu-id="90d99-396">**True** si la función es una función integrada<sup>1</sup> función; en caso contrario **False**.</span><span class="sxs-lookup"><span data-stu-id="90d99-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="90d99-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="90d99-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="90d99-398">No</span><span class="sxs-lookup"><span data-stu-id="90d99-398">No</span></span>          | <span data-ttu-id="90d99-399">Nombre del procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="90d99-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="90d99-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="90d99-400">**NiladicFunction**</span></span>        | <span data-ttu-id="90d99-401">No</span><span class="sxs-lookup"><span data-stu-id="90d99-401">No</span></span>          | <span data-ttu-id="90d99-402">**True** si la función es una función niládica<sup>2</sup> función; **False** en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="90d99-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="90d99-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="90d99-403">**IsComposable**</span></span>           | <span data-ttu-id="90d99-404">No</span><span class="sxs-lookup"><span data-stu-id="90d99-404">No</span></span>          | <span data-ttu-id="90d99-405">**True** si la función es una composición<sup>3</sup> función; **False** en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="90d99-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="90d99-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="90d99-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="90d99-407">No</span><span class="sxs-lookup"><span data-stu-id="90d99-407">No</span></span>          | <span data-ttu-id="90d99-408">La enumeración que define la semántica de tipos que se utiliza para resolver sobrecargas de función.</span><span class="sxs-lookup"><span data-stu-id="90d99-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="90d99-409">La enumeración se define en el manifiesto del proveedor por cada definición de función.</span><span class="sxs-lookup"><span data-stu-id="90d99-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="90d99-410">El valor predeterminado es **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="90d99-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="90d99-411">**Esquema**</span><span class="sxs-lookup"><span data-stu-id="90d99-411">**Schema**</span></span>                 | <span data-ttu-id="90d99-412">No</span><span class="sxs-lookup"><span data-stu-id="90d99-412">No</span></span>          | <span data-ttu-id="90d99-413">El nombre del esquema donde se define el procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="90d99-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="90d99-414"><sup>1</sup> una función integrada es una función que se define en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="90d99-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="90d99-415">Para obtener información acerca de las funciones que se definen en el modelo de almacenamiento, vea el elemento CommandText (SSDL).</span><span class="sxs-lookup"><span data-stu-id="90d99-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="90d99-416"><sup>2</sup> una función niládica es una función que no acepta parámetros y, cuando se llama, no requiere paréntesis.</span><span class="sxs-lookup"><span data-stu-id="90d99-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="90d99-417"><sup>3</sup> dos funciones admiten composición si el resultado de una función puede ser la entrada para otra función.</span><span class="sxs-lookup"><span data-stu-id="90d99-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="90d99-418">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **función** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="90d99-419">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-420">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-421">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-421">Example</span></span>

<span data-ttu-id="90d99-422">El ejemplo siguiente se muestra un **función** elemento que corresponde a la **UpdateOrderQuantity** procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="90d99-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="90d99-423">El procedimiento almacenado acepta dos parámetros y no devuelve ningún valor.</span><span class="sxs-lookup"><span data-stu-id="90d99-423">The stored procedure accepts two parameters and does not return a value.</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a><span data-ttu-id="90d99-424">Key (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-424">Key Element (SSDL)</span></span>

<span data-ttu-id="90d99-425">El **clave** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa la clave principal de una tabla en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="90d99-426">**Clave** es un elemento secundario de un elemento EntityType, que representa una fila de una tabla.</span><span class="sxs-lookup"><span data-stu-id="90d99-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="90d99-427">La clave principal se define en el **clave** elemento haciendo referencia a uno o más elementos de propiedad que se definen en el **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="90d99-428">El **clave** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-429">PropertyRef (uno o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="90d99-430">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="90d99-430">Annotation elements</span></span>

<span data-ttu-id="90d99-431">No hay atributos son aplicables a la **clave** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-432">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-432">Example</span></span>

<span data-ttu-id="90d99-433">El ejemplo siguiente se muestra un **EntityType** elemento con una clave que hace referencia a una propiedad:</span><span class="sxs-lookup"><span data-stu-id="90d99-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="90d99-434">OnDelete (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="90d99-435">El **OnDelete** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) refleja el comportamiento de la base de datos cuando se elimina una fila que participa en una restricción foreign key.</span><span class="sxs-lookup"><span data-stu-id="90d99-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="90d99-436">Si la acción se establece en **Cascade**, a continuación, también se eliminarán las filas que hacen referencia a una fila que se va a eliminar.</span><span class="sxs-lookup"><span data-stu-id="90d99-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="90d99-437">Si la acción se establece en **ninguno**, a continuación, no se eliminan las filas que hacen referencia a una fila que se va a eliminar.</span><span class="sxs-lookup"><span data-stu-id="90d99-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="90d99-438">Un **OnDelete** elemento es un elemento secundario del elemento final.</span><span class="sxs-lookup"><span data-stu-id="90d99-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="90d99-439">Un **OnDelete** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-440">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="90d99-441">Elementos de anotación (cero o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-442">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-442">Applicable Attributes</span></span>

<span data-ttu-id="90d99-443">En la tabla siguiente se describe los atributos que se pueden aplicar a la **OnDelete** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="90d99-444">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-444">Attribute Name</span></span> | <span data-ttu-id="90d99-445">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-445">Is Required</span></span> | <span data-ttu-id="90d99-446">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-447">**Acción**</span><span class="sxs-lookup"><span data-stu-id="90d99-447">**Action**</span></span>     | <span data-ttu-id="90d99-448">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-448">Yes</span></span>         | <span data-ttu-id="90d99-449">**CASCADE** o **ninguno**.</span><span class="sxs-lookup"><span data-stu-id="90d99-449">**Cascade** or **None**.</span></span> <span data-ttu-id="90d99-450">(El valor **Restricted** es válido pero tiene el mismo comportamiento que **ninguno**.)</span><span class="sxs-lookup"><span data-stu-id="90d99-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-451">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **OnDelete** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="90d99-452">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-453">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-454">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-454">Example</span></span>

<span data-ttu-id="90d99-455">El ejemplo siguiente se muestra un **asociación** elemento que define el **FK\_CustomerOrders** restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="90d99-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="90d99-456">El **OnDelete** elemento indica que todas las filas de la **pedidos** tabla que hacen referencia a una fila determinada en el **clientes** se eliminará la tabla si la fila en la **Clientes** se elimina la tabla.</span><span class="sxs-lookup"><span data-stu-id="90d99-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a><span data-ttu-id="90d99-457">Parameter (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="90d99-458">El **parámetro** en lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario del elemento Function que especifica los parámetros para un procedimiento almacenado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="90d99-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="90d99-459">El **parámetro** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-460">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="90d99-461">Elementos de anotación (cero o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-462">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-462">Applicable Attributes</span></span>

<span data-ttu-id="90d99-463">En la tabla siguiente se describe los atributos que se pueden aplicar a la **parámetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="90d99-464">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-464">Attribute Name</span></span> | <span data-ttu-id="90d99-465">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-465">Is Required</span></span> | <span data-ttu-id="90d99-466">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="90d99-467">**Name**</span></span>       | <span data-ttu-id="90d99-468">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-468">Yes</span></span>         | <span data-ttu-id="90d99-469">Nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="90d99-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="90d99-470">**Type**</span><span class="sxs-lookup"><span data-stu-id="90d99-470">**Type**</span></span>       | <span data-ttu-id="90d99-471">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-471">Yes</span></span>         | <span data-ttu-id="90d99-472">Tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="90d99-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="90d99-473">**Modo**</span><span class="sxs-lookup"><span data-stu-id="90d99-473">**Mode**</span></span>       | <span data-ttu-id="90d99-474">No</span><span class="sxs-lookup"><span data-stu-id="90d99-474">No</span></span>          | <span data-ttu-id="90d99-475">**En**, **Out**, o **InOut** dependiendo de si el parámetro es una entrada, salida o parámetro de entrada/salida.</span><span class="sxs-lookup"><span data-stu-id="90d99-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="90d99-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="90d99-476">**MaxLength**</span></span>  | <span data-ttu-id="90d99-477">No</span><span class="sxs-lookup"><span data-stu-id="90d99-477">No</span></span>          | <span data-ttu-id="90d99-478">Longitud máxima permitida del parámetro.</span><span class="sxs-lookup"><span data-stu-id="90d99-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="90d99-479">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="90d99-479">**Precision**</span></span>  | <span data-ttu-id="90d99-480">No</span><span class="sxs-lookup"><span data-stu-id="90d99-480">No</span></span>          | <span data-ttu-id="90d99-481">Precisión del parámetro.</span><span class="sxs-lookup"><span data-stu-id="90d99-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="90d99-482">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="90d99-482">**Scale**</span></span>      | <span data-ttu-id="90d99-483">No</span><span class="sxs-lookup"><span data-stu-id="90d99-483">No</span></span>          | <span data-ttu-id="90d99-484">Escala del parámetro.</span><span class="sxs-lookup"><span data-stu-id="90d99-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="90d99-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="90d99-485">**SRID**</span></span>       | <span data-ttu-id="90d99-486">No</span><span class="sxs-lookup"><span data-stu-id="90d99-486">No</span></span>          | <span data-ttu-id="90d99-487">Identificador de referencia espacial de sistema.</span><span class="sxs-lookup"><span data-stu-id="90d99-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="90d99-488">Válido únicamente para los parámetros de tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="90d99-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="90d99-489">Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="90d99-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-490">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **parámetro** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="90d99-491">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-492">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-493">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-493">Example</span></span>

<span data-ttu-id="90d99-494">El ejemplo siguiente se muestra un **función** elemento que tiene dos **parámetro** elementos que especifican los parámetros de entrada:</span><span class="sxs-lookup"><span data-stu-id="90d99-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a><span data-ttu-id="90d99-495">Principal (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="90d99-496">El **Principal** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario al elemento ReferentialConstraint que define el extremo principal de una restricción de clave externa (también llamada restricción referencial).</span><span class="sxs-lookup"><span data-stu-id="90d99-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="90d99-497">El **Principal** elemento especifica la columna de clave principal (o columnas) en una tabla que hace referencia a otra columna (o columnas).</span><span class="sxs-lookup"><span data-stu-id="90d99-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="90d99-498">**PropertyRef** elementos especifican las columnas que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="90d99-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="90d99-499">El elemento dependiente especifica columnas que hacen referencia las columnas de clave principal que se especifican en el **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="90d99-500">El **Principal** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="90d99-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="90d99-501">PropertyRef (uno o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="90d99-502">Elementos de anotación (cero o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-503">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-503">Applicable Attributes</span></span>

<span data-ttu-id="90d99-504">En la tabla siguiente se describe los atributos que se pueden aplicar a la **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="90d99-505">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-505">Attribute Name</span></span> | <span data-ttu-id="90d99-506">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-506">Is Required</span></span> | <span data-ttu-id="90d99-507">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-508">**Rol**</span><span class="sxs-lookup"><span data-stu-id="90d99-508">**Role**</span></span>       | <span data-ttu-id="90d99-509">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-509">Yes</span></span>         | <span data-ttu-id="90d99-510">El mismo valor que el **rol** atributo (si se utiliza) del elemento final correspondiente; en caso contrario, el nombre de la tabla que contiene la columna que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="90d99-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-511">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **Principal** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="90d99-512">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="90d99-513">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-514">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-514">Example</span></span>

<span data-ttu-id="90d99-515">En el ejemplo siguiente se muestra un elemento Association que usa un **ReferentialConstraint** elemento para especificar las columnas que participan en la **FK\_CustomerOrders** clave externa restricción.</span><span class="sxs-lookup"><span data-stu-id="90d99-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="90d99-516">El **Principal** elemento especifica el **CustomerId** columna de la **cliente** tabla como el extremo principal de la restricción.</span><span class="sxs-lookup"><span data-stu-id="90d99-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a><span data-ttu-id="90d99-517">Property (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-517">Property Element (SSDL)</span></span>

<span data-ttu-id="90d99-518">El **propiedad** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa una columna de una tabla en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="90d99-519">**Propiedad** son elementos secundarios de los elementos EntityType, que representan las filas de una tabla.</span><span class="sxs-lookup"><span data-stu-id="90d99-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="90d99-520">Cada **propiedad** elemento definido en un **EntityType** elemento representa una columna.</span><span class="sxs-lookup"><span data-stu-id="90d99-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="90d99-521">Un **propiedad** elemento no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="90d99-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-522">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-522">Applicable Attributes</span></span>

<span data-ttu-id="90d99-523">En la tabla siguiente se describe los atributos que se pueden aplicar a la **propiedad** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="90d99-524">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-524">Attribute Name</span></span>            | <span data-ttu-id="90d99-525">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-525">Is Required</span></span> | <span data-ttu-id="90d99-526">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="90d99-527">**Name**</span></span>                  | <span data-ttu-id="90d99-528">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-528">Yes</span></span>         | <span data-ttu-id="90d99-529">El nombre de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="90d99-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="90d99-530">**Type**</span><span class="sxs-lookup"><span data-stu-id="90d99-530">**Type**</span></span>                  | <span data-ttu-id="90d99-531">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-531">Yes</span></span>         | <span data-ttu-id="90d99-532">El tipo de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="90d99-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="90d99-533">**Que acepta valores null**</span><span class="sxs-lookup"><span data-stu-id="90d99-533">**Nullable**</span></span>              | <span data-ttu-id="90d99-534">No</span><span class="sxs-lookup"><span data-stu-id="90d99-534">No</span></span>          | <span data-ttu-id="90d99-535">**True** (el valor predeterminado) o **False** dependiendo de si la columna correspondiente puede tener un valor null.</span><span class="sxs-lookup"><span data-stu-id="90d99-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="90d99-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="90d99-536">**DefaultValue**</span></span>          | <span data-ttu-id="90d99-537">No</span><span class="sxs-lookup"><span data-stu-id="90d99-537">No</span></span>          | <span data-ttu-id="90d99-538">El valor predeterminado de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="90d99-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="90d99-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="90d99-539">**MaxLength**</span></span>             | <span data-ttu-id="90d99-540">No</span><span class="sxs-lookup"><span data-stu-id="90d99-540">No</span></span>          | <span data-ttu-id="90d99-541">La longitud máxima de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="90d99-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="90d99-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="90d99-542">**FixedLength**</span></span>           | <span data-ttu-id="90d99-543">No</span><span class="sxs-lookup"><span data-stu-id="90d99-543">No</span></span>          | <span data-ttu-id="90d99-544">**True** o **False** dependiendo de si el valor de columna correspondiente se almacenarán como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="90d99-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="90d99-545">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="90d99-545">**Precision**</span></span>             | <span data-ttu-id="90d99-546">No</span><span class="sxs-lookup"><span data-stu-id="90d99-546">No</span></span>          | <span data-ttu-id="90d99-547">La precisión de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="90d99-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="90d99-548">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="90d99-548">**Scale**</span></span>                 | <span data-ttu-id="90d99-549">No</span><span class="sxs-lookup"><span data-stu-id="90d99-549">No</span></span>          | <span data-ttu-id="90d99-550">La escala de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="90d99-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="90d99-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="90d99-551">**Unicode**</span></span>               | <span data-ttu-id="90d99-552">No</span><span class="sxs-lookup"><span data-stu-id="90d99-552">No</span></span>          | <span data-ttu-id="90d99-553">**True** o **False** dependiendo de si el valor de columna correspondiente se almacenarán como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="90d99-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="90d99-554">**intercalación**</span><span class="sxs-lookup"><span data-stu-id="90d99-554">**Collation**</span></span>             | <span data-ttu-id="90d99-555">No</span><span class="sxs-lookup"><span data-stu-id="90d99-555">No</span></span>          | <span data-ttu-id="90d99-556">Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="90d99-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="90d99-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="90d99-557">**SRID**</span></span>                  | <span data-ttu-id="90d99-558">No</span><span class="sxs-lookup"><span data-stu-id="90d99-558">No</span></span>          | <span data-ttu-id="90d99-559">Identificador de referencia espacial de sistema.</span><span class="sxs-lookup"><span data-stu-id="90d99-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="90d99-560">Válido solo para las propiedades de tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="90d99-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="90d99-561">Para obtener más información, consulte [SRID](http://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="90d99-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="90d99-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="90d99-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="90d99-563">No</span><span class="sxs-lookup"><span data-stu-id="90d99-563">No</span></span>          | <span data-ttu-id="90d99-564">**Ninguno**, **identidad** (si el valor correspondiente de la columna es una identidad que se genera en la base de datos), o **calculado** (si el valor de columna correspondiente se calcula en la base de datos).</span><span class="sxs-lookup"><span data-stu-id="90d99-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="90d99-565">No es válido para las propiedades de tipo de fila.</span><span class="sxs-lookup"><span data-stu-id="90d99-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-566">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **propiedad** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="90d99-567">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-568">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-569">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-569">Example</span></span>

<span data-ttu-id="90d99-570">El ejemplo siguiente se muestra un **EntityType** elemento con dos secundarios **propiedad** elementos:</span><span class="sxs-lookup"><span data-stu-id="90d99-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="90d99-571">PropertyRef (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="90d99-572">El **PropertyRef** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) hace referencia a una propiedad definida en un elemento EntityType para indicar que la propiedad realizará uno de los roles siguientes:</span><span class="sxs-lookup"><span data-stu-id="90d99-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="90d99-573">Formar parte de la clave principal de la tabla que la **EntityType** representa.</span><span class="sxs-lookup"><span data-stu-id="90d99-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="90d99-574">Uno o varios **PropertyRef** elementos se pueden utilizar para definir una clave principal.</span><span class="sxs-lookup"><span data-stu-id="90d99-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="90d99-575">Para obtener más información, vea el elemento de clave.</span><span class="sxs-lookup"><span data-stu-id="90d99-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="90d99-576">Ser el extremo dependiente o principal de una restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="90d99-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="90d99-577">Para obtener más información, consulte ReferentialConstraint (elemento).</span><span class="sxs-lookup"><span data-stu-id="90d99-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="90d99-578">El **PropertyRef** elemento solo puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="90d99-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="90d99-579">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="90d99-580">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="90d99-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-581">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-581">Applicable Attributes</span></span>

<span data-ttu-id="90d99-582">En la tabla siguiente se describe los atributos que se pueden aplicar a la **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="90d99-583">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-583">Attribute Name</span></span> | <span data-ttu-id="90d99-584">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-584">Is Required</span></span> | <span data-ttu-id="90d99-585">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="90d99-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="90d99-586">**Name**</span></span>       | <span data-ttu-id="90d99-587">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-587">Yes</span></span>         | <span data-ttu-id="90d99-588">Nombre de la propiedad a la que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="90d99-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="90d99-589">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **PropertyRef** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="90d99-590">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="90d99-591">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-592">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-592">Example</span></span>

<span data-ttu-id="90d99-593">El ejemplo siguiente se muestra un **PropertyRef** elemento que se usa para definir una clave principal haciendo referencia a una propiedad que se define en un **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="90d99-594">ReferentialConstraint (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="90d99-595">El **ReferentialConstraint** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) representa una restricción de clave externa (también denominada una restricción de integridad referencial) en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="90d99-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="90d99-596">Los extremos principales y dependientes de la restricción se especifican respectivamente el principal y dependiente de los elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="90d99-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="90d99-597">Se hace referencia a columnas que participan en los extremos principal y dependientes con elementos PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="90d99-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="90d99-598">El **ReferentialConstraint** es un elemento secundario opcional del elemento Association.</span><span class="sxs-lookup"><span data-stu-id="90d99-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="90d99-599">Si un **ReferentialConstraint** elemento no se usa para asignar la restricción foreign key que se especifica en el **asociación** elemento AssociationSetMapping al elemento que se debe usar para hacer esto.</span><span class="sxs-lookup"><span data-stu-id="90d99-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="90d99-600">El **ReferentialConstraint** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="90d99-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="90d99-601">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="90d99-602">Entidad de seguridad (exactamente uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="90d99-603">Dependiente (exactamente uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="90d99-604">Elementos de anotación (cero o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-605">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-605">Applicable Attributes</span></span>

<span data-ttu-id="90d99-606">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ReferentialConstraint** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="90d99-607">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-608">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-609">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-609">Example</span></span>

<span data-ttu-id="90d99-610">El ejemplo siguiente se muestra un **asociación** elemento que se usa un **ReferentialConstraint** elemento para especificar las columnas que participan en la **FK\_CustomerOrders**  restricción de clave externa:</span><span class="sxs-lookup"><span data-stu-id="90d99-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a><span data-ttu-id="90d99-611">ReturnType (elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="90d99-612">El **ReturnType** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) especifica el tipo de valor devuelto para una función que se define en un **función** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="90d99-613">Una función también se puede especificar el tipo de valor devuelto con un **ReturnType** atributo.</span><span class="sxs-lookup"><span data-stu-id="90d99-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="90d99-614">Se especifica el tipo de valor devuelto de una función con el **tipo** atributo o la **ReturnType** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="90d99-615">El **ReturnType** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="90d99-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="90d99-616">CollectionType (uno)</span><span class="sxs-lookup"><span data-stu-id="90d99-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="90d99-617">Cualquier número de atributos de anotación (atributos XML personalizados) se puede aplicar a la **ReturnType** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="90d99-618">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="90d99-619">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="90d99-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-620">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-620">Example</span></span>

<span data-ttu-id="90d99-621">En el ejemplo siguiente se usa un **función** que devuelve una colección de filas.</span><span class="sxs-lookup"><span data-stu-id="90d99-621">The following example uses a **Function** that returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="90d99-622">RowType (elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="90d99-623">Un **RowType** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) define una estructura sin nombre como un retorno de tipo para una función definida en el almacén.</span><span class="sxs-lookup"><span data-stu-id="90d99-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="90d99-624">Un **RowType** es el elemento secundario del **CollectionType** elemento:</span><span class="sxs-lookup"><span data-stu-id="90d99-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="90d99-625">Un **RowType** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="90d99-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="90d99-626">Propiedad (uno o más)</span><span class="sxs-lookup"><span data-stu-id="90d99-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="90d99-627">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-627">Example</span></span>

<span data-ttu-id="90d99-628">El ejemplo siguiente muestra una función de almacenamiento que usa un **CollectionType** elemento para especificar que la función devuelve una colección de filas (como se especifica en el **RowType** elemento).</span><span class="sxs-lookup"><span data-stu-id="90d99-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a><span data-ttu-id="90d99-629">Schema (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="90d99-630">El **esquema** elemento de lenguaje de definición de esquemas de almacenamiento (SSDL) es el elemento raíz de una definición de modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="90d99-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="90d99-631">Contiene las definiciones para los objetos, las funciones y los contenedores que conforman un modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="90d99-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="90d99-632">El **esquema** elemento puede contener cero o más de los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="90d99-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="90d99-633">Asociación</span><span class="sxs-lookup"><span data-stu-id="90d99-633">Association</span></span>
-   <span data-ttu-id="90d99-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="90d99-634">EntityType</span></span>
-   <span data-ttu-id="90d99-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="90d99-635">EntityContainer</span></span>
-   <span data-ttu-id="90d99-636">Función</span><span class="sxs-lookup"><span data-stu-id="90d99-636">Function</span></span>

<span data-ttu-id="90d99-637">El **esquema** elemento utiliza la **Namespace** atributo para definir el espacio de nombres para los objetos de asociación y tipo de entidad en un modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="90d99-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="90d99-638">Dentro de un espacio de nombres, no puede haber dos objetos con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="90d99-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="90d99-639">Un espacio de nombres del modelo de almacenamiento es diferente del espacio de nombres XML de la **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="90d99-640">Un espacio de nombres del modelo de almacenamiento (como se define en el **espacio de nombres** atributo) es un contenedor lógico para tipos de entidad y tipos de asociación.</span><span class="sxs-lookup"><span data-stu-id="90d99-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="90d99-641">El espacio de nombres XML (indicado por el **xmlns** atributo) de un **esquema** elemento es el espacio de nombres predeterminado para los elementos secundarios y atributos de la **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="90d99-642">Espacios de nombres XML del formulario http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (donde AAAA y MM representan un año y mes, respectivamente) se reservan para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="90d99-643">No puede haber elementos y atributos personalizados en espacios de nombres que tengan este formato.</span><span class="sxs-lookup"><span data-stu-id="90d99-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="90d99-644">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="90d99-644">Applicable Attributes</span></span>

<span data-ttu-id="90d99-645">En la tabla siguiente se describe los atributos se pueden aplicar a la **esquema** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="90d99-646">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="90d99-646">Attribute Name</span></span>            | <span data-ttu-id="90d99-647">Es necesario</span><span class="sxs-lookup"><span data-stu-id="90d99-647">Is Required</span></span> | <span data-ttu-id="90d99-648">Valor</span><span class="sxs-lookup"><span data-stu-id="90d99-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-649">**Espacio de nombres**</span><span class="sxs-lookup"><span data-stu-id="90d99-649">**Namespace**</span></span>             | <span data-ttu-id="90d99-650">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-650">Yes</span></span>         | <span data-ttu-id="90d99-651">El espacio de nombres del modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="90d99-651">The namespace of the storage model.</span></span> <span data-ttu-id="90d99-652">El valor de la **Namespace** atributo se usa para formar el nombre completo de un tipo.</span><span class="sxs-lookup"><span data-stu-id="90d99-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="90d99-653">Por ejemplo, si un **EntityType** denominado *cliente* está en el espacio de nombres ExampleModel.Store, el nombre completo de la **EntityType** es ExampleModel.Store.Customer.</span><span class="sxs-lookup"><span data-stu-id="90d99-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="90d99-654">No se puede usar las siguientes cadenas como valor para el **espacio de nombres** atributo: **sistema**, **transitorios**, o **Edm**.</span><span class="sxs-lookup"><span data-stu-id="90d99-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="90d99-655">El valor de la **espacio de nombres** atributo no puede ser el mismo que el valor de la **espacio de nombres** atributo en el elemento de esquema de CSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="90d99-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="90d99-656">**Alias**</span></span>                 | <span data-ttu-id="90d99-657">No</span><span class="sxs-lookup"><span data-stu-id="90d99-657">No</span></span>          | <span data-ttu-id="90d99-658">Un identificador usado en lugar del nombre del espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="90d99-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="90d99-659">Por ejemplo, si un **EntityType** denominado *cliente* está en el espacio de nombres ExampleModel.Store y el valor de la **Alias** atributo es *StorageModel*, puede usar StorageModel.Customer como nombre completo de la **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="90d99-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="90d99-660">**Proveedor**</span><span class="sxs-lookup"><span data-stu-id="90d99-660">**Provider**</span></span>              | <span data-ttu-id="90d99-661">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-661">Yes</span></span>         | <span data-ttu-id="90d99-662">El proveedor de datos.</span><span class="sxs-lookup"><span data-stu-id="90d99-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="90d99-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="90d99-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="90d99-664">Sí</span><span class="sxs-lookup"><span data-stu-id="90d99-664">Yes</span></span>         | <span data-ttu-id="90d99-665">Un token que indica al proveedor qué manifiesto del proveedor debe devolver.</span><span class="sxs-lookup"><span data-stu-id="90d99-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="90d99-666">No se define ningún formato para el token.</span><span class="sxs-lookup"><span data-stu-id="90d99-666">No format for the token is defined.</span></span> <span data-ttu-id="90d99-667">El proveedor define los valores para el token.</span><span class="sxs-lookup"><span data-stu-id="90d99-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="90d99-668">Para obtener información acerca de los tokens de manifiesto de proveedor de SQL Server, vea SqlClient para Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="90d99-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="90d99-669">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-669">Example</span></span>

<span data-ttu-id="90d99-670">El ejemplo siguiente se muestra un **esquema** elemento que contiene un **EntityContainer** elemento, dos **EntityType** elementos y otra **asociación** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a><span data-ttu-id="90d99-671">Atributos de anotación</span><span class="sxs-lookup"><span data-stu-id="90d99-671">Annotation Attributes</span></span>

<span data-ttu-id="90d99-672">Los atributos de anotación (Annotation) del lenguaje de definición de esquemas de almacenamiento (SSDL) son atributos XML personalizados del modelo de almacenamiento que proporcionan metadatos adicionales sobre los elementos del modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="90d99-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="90d99-673">Además de tener una estructura XML válida, las siguientes restricciones se aplican a los atributos de anotación:</span><span class="sxs-lookup"><span data-stu-id="90d99-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="90d99-674">Los atributos de anotación no deben estar en ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="90d99-675">Dos atributos de anotación cualesquiera no pueden tener el mismo nombre completo.</span><span class="sxs-lookup"><span data-stu-id="90d99-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="90d99-676">Se pueden aplicar varios atributos de anotación a un elemento SSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="90d99-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="90d99-677">Pueden tener acceso a los metadatos contenidos en elementos de anotación en tiempo de ejecución mediante las clases del espacio de nombres System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="90d99-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-678">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-678">Example</span></span>

<span data-ttu-id="90d99-679">El ejemplo siguiente muestra un elemento EntityType que tiene un atributo de anotación aplicado a la **OrderId** propiedad.</span><span class="sxs-lookup"><span data-stu-id="90d99-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="90d99-680">El ejemplo también muestra un elemento de anotación que se agrega a la **EntityType** elemento.</span><span class="sxs-lookup"><span data-stu-id="90d99-680">The example also show an annotation element added to the **EntityType** element.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="90d99-681">Annotation (Elementos) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="90d99-682">Los elementos Annotation del lenguaje de definición de esquemas de almacenamiento (SSDL) son elementos XML personalizados del modelo de almacenamiento que proporcionan metadatos adicionales sobre el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="90d99-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="90d99-683">Además de tener una estructura XML válida, las siguientes restricciones se aplican a los elementos Annotation:</span><span class="sxs-lookup"><span data-stu-id="90d99-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="90d99-684">Los elementos Annotation no deben estar en un espacio de nombres de XML que esté reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="90d99-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="90d99-685">Los nombres completos de dos elementos Annotation cualesquiera no deben ser los mismos.</span><span class="sxs-lookup"><span data-stu-id="90d99-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="90d99-686">Los elementos Annotation deben aparecer después de todos los demás elementos secundarios de un elemento SSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="90d99-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="90d99-687">Más de un elemento Annotation puede ser un elemento secundario de un elemento SSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="90d99-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="90d99-688">A partir de .NET Framework versión 4, pueden obtenerse metadatos contenidos en elementos de anotación en tiempo de ejecución mediante las clases del espacio de nombres System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="90d99-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="90d99-689">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="90d99-689">Example</span></span>

<span data-ttu-id="90d99-690">El ejemplo siguiente muestra un elemento EntityType que tiene un elemento annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="90d99-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="90d99-691">El ejemplo también muestra un atributo de anotación aplicado a la **OrderId** propiedad.</span><span class="sxs-lookup"><span data-stu-id="90d99-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a><span data-ttu-id="90d99-692">Facetas (SSDL)</span><span class="sxs-lookup"><span data-stu-id="90d99-692">Facets (SSDL)</span></span>

<span data-ttu-id="90d99-693">Las facetas en lenguaje de definición de esquemas de almacenamiento (SSDL) representan restricciones de tipos de columnas que se especifican en elementos de propiedad.</span><span class="sxs-lookup"><span data-stu-id="90d99-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="90d99-694">Las facetas se implementan como atributos XML en **propiedad** elementos.</span><span class="sxs-lookup"><span data-stu-id="90d99-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="90d99-695">En la tabla siguiente se describen las facetas que se admiten en SSDL:</span><span class="sxs-lookup"><span data-stu-id="90d99-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="90d99-696">Faceta</span><span class="sxs-lookup"><span data-stu-id="90d99-696">Facet</span></span>           | <span data-ttu-id="90d99-697">Descripción</span><span class="sxs-lookup"><span data-stu-id="90d99-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="90d99-698">**intercalación**</span><span class="sxs-lookup"><span data-stu-id="90d99-698">**Collation**</span></span>   | <span data-ttu-id="90d99-699">Especifica la secuencia de intercalación (o secuencia de orden) que se va a usar cuando se realicen las operaciones de comparación y ordenación sobre los valores de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="90d99-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="90d99-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="90d99-700">**FixedLength**</span></span> | <span data-ttu-id="90d99-701">Especifica si la longitud del valor de la columna puede variar.</span><span class="sxs-lookup"><span data-stu-id="90d99-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="90d99-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="90d99-702">**MaxLength**</span></span>   | <span data-ttu-id="90d99-703">Especifica la longitud máxima del valor de la columna.</span><span class="sxs-lookup"><span data-stu-id="90d99-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="90d99-704">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="90d99-704">**Precision**</span></span>   | <span data-ttu-id="90d99-705">Las propiedades de tipo **Decimal**, especifica el número de dígitos que puede tener un valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="90d99-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="90d99-706">Las propiedades de tipo **tiempo**, **DateTime**, y **DateTimeOffset**, especifica el número de dígitos para la parte fraccionaria de segundos del valor de columna.</span><span class="sxs-lookup"><span data-stu-id="90d99-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="90d99-707">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="90d99-707">**Scale**</span></span>       | <span data-ttu-id="90d99-708">Especifica el número de dígitos que puede haber a la derecha del separador de decimales para el valor de la columna.</span><span class="sxs-lookup"><span data-stu-id="90d99-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="90d99-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="90d99-709">**Unicode**</span></span>     | <span data-ttu-id="90d99-710">Indica si el valor de la columna está almacenado como Unicode.</span><span class="sxs-lookup"><span data-stu-id="90d99-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
