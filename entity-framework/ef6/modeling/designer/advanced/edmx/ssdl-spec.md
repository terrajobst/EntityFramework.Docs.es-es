---
title: 'Especificación de SSDL: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182543"
---
# <a name="ssdl-specification"></a><span data-ttu-id="8ace8-102">Especificación SSDL</span><span class="sxs-lookup"><span data-stu-id="8ace8-102">SSDL Specification</span></span>
<span data-ttu-id="8ace8-103">El lenguaje de definición de esquemas de almacenamiento (SSDL) es un lenguaje basado en XML que describe el modelo de almacenamiento de una aplicación Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8ace8-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="8ace8-104">En una aplicación Entity Framework, los metadatos del modelo de almacenamiento se cargan desde un archivo. SSDL (escrito en SSDL) en una instancia de System. Data. Metadata. Edm. StoreItemCollection y es accesible mediante métodos en el Clase System. Data. Metadata. Edm. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="8ace8-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="8ace8-105">Entity Framework usa los metadatos del modelo de almacenamiento para traducir las consultas en el modelo conceptual para almacenar comandos específicos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="8ace8-106">El Entity Framework Designer (EF Designer) almacena la información del modelo de almacenamiento en un archivo. edmx en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="8ace8-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="8ace8-107">En tiempo de compilación, Entity Designer usa información en un archivo. edmx para crear el archivo. SSDL que Entity Framework necesita en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="8ace8-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="8ace8-108">Las versiones de SSDL se diferencian por los espacios de nombres XML.</span><span class="sxs-lookup"><span data-stu-id="8ace8-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="8ace8-109">Versión de SSDL</span><span class="sxs-lookup"><span data-stu-id="8ace8-109">SSDL Version</span></span> | <span data-ttu-id="8ace8-110">Espacio de nombres XML</span><span class="sxs-lookup"><span data-stu-id="8ace8-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="8ace8-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="8ace8-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="8ace8-112">SSDL V2</span><span class="sxs-lookup"><span data-stu-id="8ace8-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="8ace8-113">SSDL V3</span><span class="sxs-lookup"><span data-stu-id="8ace8-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="8ace8-114">Association (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-114">Association Element (SSDL)</span></span>

<span data-ttu-id="8ace8-115">Un elemento **Association** en el lenguaje de definición de esquemas de almacenamiento (SSDL) especifica las columnas de tabla que participan en una restricción FOREIGN KEY en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="8ace8-116">Dos elementos end secundarios obligatorios especifican las tablas en los extremos de la asociación y la multiplicidad en cada extremo.</span><span class="sxs-lookup"><span data-stu-id="8ace8-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="8ace8-117">Un elemento ReferentialConstraint opcional especifica los extremos principal y dependiente de la asociación, así como las columnas participantes.</span><span class="sxs-lookup"><span data-stu-id="8ace8-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="8ace8-118">Si no hay ningún elemento **ReferentialConstraint** , se debe usar un elemento AssociationSetMapping para especificar las asignaciones de columnas para la asociación.</span><span class="sxs-lookup"><span data-stu-id="8ace8-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="8ace8-119">El elemento **Association** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-120">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8ace8-121">End (exactamente dos)</span><span class="sxs-lookup"><span data-stu-id="8ace8-121">End (exactly two)</span></span>
-   <span data-ttu-id="8ace8-122">ReferentialConstraint (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="8ace8-123">Elementos Annotation (cero o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-124">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-124">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-125">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="8ace8-126">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-126">Attribute Name</span></span> | <span data-ttu-id="8ace8-127">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-127">Is Required</span></span> | <span data-ttu-id="8ace8-128">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="8ace8-129">**Name**</span></span>       | <span data-ttu-id="8ace8-130">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-130">Yes</span></span>         | <span data-ttu-id="8ace8-131">El nombre de la restricción de clave externa correspondiente de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-132">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="8ace8-133">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-134">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-135">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-135">Example</span></span>

<span data-ttu-id="8ace8-136">En el ejemplo siguiente se muestra un elemento **Association** que usa un elemento **ReferentialConstraint** para especificar las columnas que participan en la restricción de clave externa **FK @ no__t-3CustomerOrders** :</span><span class="sxs-lookup"><span data-stu-id="8ace8-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="8ace8-137">AssociationSet (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="8ace8-138">El elemento **AssociationSet** en el lenguaje de definición de esquemas de almacenamiento (SSDL) representa una restricción de clave externa entre dos tablas en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="8ace8-139">Las columnas de la tabla que participan en la restricción Foreign Key se especifican en un elemento Association.</span><span class="sxs-lookup"><span data-stu-id="8ace8-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="8ace8-140">El elemento **Association** que corresponde a un elemento **AssociationSet** determinado se especifica en el atributo **Association** del elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="8ace8-141">Los conjuntos de asociaciones SSDL se asignan a los conjuntos de asociaciones CSDL mediante un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="8ace8-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="8ace8-142">Sin embargo, si la Asociación CSDL para un conjunto de asociaciones CSDL determinado se define mediante un elemento ReferentialConstraint, no es necesario ningún elemento **AssociationSetMapping** correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="8ace8-143">En este caso, si hay un elemento **AssociationSetMapping** , las asignaciones que define se reemplazarán por el elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="8ace8-144">El elemento **AssociationSet** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-145">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8ace8-146">End (cero o dos)</span><span class="sxs-lookup"><span data-stu-id="8ace8-146">End (zero or two)</span></span>
-   <span data-ttu-id="8ace8-147">Elementos Annotation (cero o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-148">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-148">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-149">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="8ace8-150">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-150">Attribute Name</span></span>  | <span data-ttu-id="8ace8-151">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-151">Is Required</span></span> | <span data-ttu-id="8ace8-152">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="8ace8-153">**Name**</span></span>        | <span data-ttu-id="8ace8-154">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-154">Yes</span></span>         | <span data-ttu-id="8ace8-155">El nombre de la restricción de clave externa representada por el conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="8ace8-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="8ace8-156">**Asociación**</span><span class="sxs-lookup"><span data-stu-id="8ace8-156">**Association**</span></span> | <span data-ttu-id="8ace8-157">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-157">Yes</span></span>         | <span data-ttu-id="8ace8-158">El nombre de la asociación que define las columnas que participan en la restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="8ace8-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-159">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="8ace8-160">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-161">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-162">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-162">Example</span></span>

<span data-ttu-id="8ace8-163">En el ejemplo siguiente se muestra un elemento **AssociationSet** que representa la restricción foreign Key `FK_CustomerOrders` en la base de datos subyacente:</span><span class="sxs-lookup"><span data-stu-id="8ace8-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="8ace8-164">CollectionType (elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="8ace8-165">El elemento **CollectionType** del lenguaje de definición de esquemas de almacenamiento (SSDL) especifica que el tipo de valor devuelto de una función es una colección.</span><span class="sxs-lookup"><span data-stu-id="8ace8-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="8ace8-166">El elemento **CollectionType** es un elemento secundario del elemento ReturnType.</span><span class="sxs-lookup"><span data-stu-id="8ace8-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="8ace8-167">El tipo de colección se especifica mediante el elemento secundario RowType:</span><span class="sxs-lookup"><span data-stu-id="8ace8-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="8ace8-168">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="8ace8-169">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-170">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-171">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-171">Example</span></span>

<span data-ttu-id="8ace8-172">En el ejemplo siguiente se muestra una función que usa un elemento **CollectionType** para especificar que la función devuelve una colección de filas.</span><span class="sxs-lookup"><span data-stu-id="8ace8-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="8ace8-173">CommandText (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="8ace8-174">El elemento **CommandText** del lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario del elemento function que permite definir una instrucción SQL que se ejecuta en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="8ace8-175">El elemento **CommandText** permite agregar funcionalidad similar a un procedimiento almacenado en la base de datos, pero se define el elemento **CommandText** en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="8ace8-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="8ace8-176">El elemento **CommandText** no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="8ace8-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="8ace8-177">El cuerpo del elemento **CommandText** debe ser una instrucción SQL válida para la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="8ace8-178">No hay atributos aplicables al elemento **CommandText** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-179">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-179">Example</span></span>

<span data-ttu-id="8ace8-180">En el ejemplo siguiente se muestra un elemento **function** con un elemento **CommandText** secundario.</span><span class="sxs-lookup"><span data-stu-id="8ace8-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="8ace8-181">Exponga la función **UpdateProductInOrder** como un método en el ObjectContext importándolos en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="8ace8-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="8ace8-182">DefiningQuery (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="8ace8-183">El elemento **DefiningQuery** del lenguaje de definición de esquemas de almacenamiento (SSDL) permite ejecutar una instrucción SQL directamente en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="8ace8-184">El elemento **DefiningQuery** se utiliza normalmente como una vista de base de datos, pero la vista se define en el modelo de almacenamiento en lugar de en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="8ace8-185">La vista definida en un elemento **DefiningQuery** se puede asignar a un tipo de entidad en el modelo conceptual a través de un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="8ace8-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="8ace8-186">Estas asignaciones son de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="8ace8-186">These mappings are read-only.</span></span>  

<span data-ttu-id="8ace8-187">La sintaxis de SSDL siguiente muestra la declaración de un **EntitySet** seguido del elemento **DefiningQuery** que contiene una consulta utilizada para recuperar la vista.</span><span class="sxs-lookup"><span data-stu-id="8ace8-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="8ace8-188">Puede usar procedimientos almacenados en el Entity Framework para habilitar escenarios de lectura y escritura en las vistas.</span><span class="sxs-lookup"><span data-stu-id="8ace8-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="8ace8-189"> Puede usar una vista del origen de datos o una vista de Entity SQL como tabla base para recuperar datos y para el procesamiento de cambios por parte de los procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="8ace8-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="8ace8-190">Puede usar el elemento **DefiningQuery** para tener como destino Microsoft SQL Server Compact 3,5.</span><span class="sxs-lookup"><span data-stu-id="8ace8-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="8ace8-191">Aunque SQL Server Compact 3,5 no admite procedimientos almacenados, puede implementar una funcionalidad similar con el elemento **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="8ace8-192">Otro caso donde puede ser útil es en la creación de procedimientos almacenados para resolver una desigualdad entre los tipos de datos utilizados en el lenguaje de programación y los del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="8ace8-193">Podría escribir una **DefiningQuery** que toma un determinado conjunto de parámetros y, a continuación, llama a un procedimiento almacenado con un conjunto diferente de parámetros, por ejemplo, un procedimiento almacenado que elimina datos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="8ace8-194">Dependent (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="8ace8-195">El elemento **dependiente** del lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario del elemento ReferentialConstraint que define el extremo dependiente de una restricción FOREIGN KEY (también denominada restricción referencial).</span><span class="sxs-lookup"><span data-stu-id="8ace8-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="8ace8-196">El elemento **dependiente** especifica la columna (o columnas) de una tabla que hace referencia a una columna de clave principal (o columnas).</span><span class="sxs-lookup"><span data-stu-id="8ace8-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="8ace8-197">Los elementos **PropertyRef** especifican a qué columnas se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="8ace8-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="8ace8-198">El elemento principal especifica las columnas de clave principal a las que hacen referencia las columnas que se especifican en el elemento **dependiente** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="8ace8-199">El elemento **dependiente** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-200">PropertyRef (uno o varios)</span><span class="sxs-lookup"><span data-stu-id="8ace8-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="8ace8-201">Elementos Annotation (cero o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-202">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-202">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-203">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **dependiente** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="8ace8-204">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-204">Attribute Name</span></span> | <span data-ttu-id="8ace8-205">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-205">Is Required</span></span> | <span data-ttu-id="8ace8-206">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-207">**Role**</span><span class="sxs-lookup"><span data-stu-id="8ace8-207">**Role**</span></span>       | <span data-ttu-id="8ace8-208">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-208">Yes</span></span>         | <span data-ttu-id="8ace8-209">El mismo valor que el atributo **role** (si se usa) del elemento end correspondiente; de lo contrario, el nombre de la tabla que contiene la columna de referencia.</span><span class="sxs-lookup"><span data-stu-id="8ace8-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-210">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **dependiente** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="8ace8-211">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8ace8-212">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-213">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-213">Example</span></span>

<span data-ttu-id="8ace8-214">En el ejemplo siguiente se muestra un elemento Association que utiliza un elemento **ReferentialConstraint** para especificar las columnas que participan en la restricción de clave externa **FK @ no__t-2CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="8ace8-215">El elemento **dependiente** especifica la columna **CustomerID** de la tabla **Order** como el extremo dependiente de la restricción.</span><span class="sxs-lookup"><span data-stu-id="8ace8-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="8ace8-216">Documentation (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="8ace8-217">El elemento **Documentation** del lenguaje de definición de esquemas de almacenamiento (SSDL) se puede usar para proporcionar información sobre un objeto que se define en un elemento primario.</span><span class="sxs-lookup"><span data-stu-id="8ace8-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="8ace8-218">El elemento **Documentation** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-219">**Resumen**: Breve descripción del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="8ace8-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="8ace8-220">(cero o un elemento).</span><span class="sxs-lookup"><span data-stu-id="8ace8-220">(zero or one element)</span></span>
-   <span data-ttu-id="8ace8-221">**LongDescription**: Una descripción amplia del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="8ace8-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="8ace8-222">(cero o un elemento).</span><span class="sxs-lookup"><span data-stu-id="8ace8-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-223">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-223">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-224">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **Documentation** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="8ace8-225">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8ace8-226">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-227">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-227">Example</span></span>

<span data-ttu-id="8ace8-228">En el ejemplo siguiente se muestra el elemento **Documentation** como un elemento secundario de un elemento EntityType.</span><span class="sxs-lookup"><span data-stu-id="8ace8-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="8ace8-229">End (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-229">End Element (SSDL)</span></span>

<span data-ttu-id="8ace8-230">El elemento **End** en el lenguaje de definición de esquemas de almacenamiento (SSDL) especifica la tabla y el número de filas de un extremo de una restricción FOREIGN KEY en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="8ace8-231">El elemento **End** puede ser un elemento secundario del elemento Association o del elemento AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="8ace8-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="8ace8-232">En cada caso, los posibles elementos secundarios y atributos aplicables son diferentes.</span><span class="sxs-lookup"><span data-stu-id="8ace8-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="8ace8-233">Elemento End como elemento secundario del elemento Association</span><span class="sxs-lookup"><span data-stu-id="8ace8-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="8ace8-234">Un elemento **End** (como elemento secundario del elemento **Association** ) especifica la tabla y el número de filas al final de una restricción FOREIGN KEY con los atributos **Type** y **Multiplicity** , respectivamente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="8ace8-235">Los extremos de una restricción de clave externa se definen como parte de una asociación SSDL, la cual debe tener exactamente dos extremos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="8ace8-236">Un elemento **End** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-237">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="8ace8-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="8ace8-238">Aleliminar (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="8ace8-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="8ace8-239">Elementos Annotation (cero o más elementos)</span><span class="sxs-lookup"><span data-stu-id="8ace8-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-240">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-240">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-241">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **End** cuando es el elemento secundario de un elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="8ace8-242">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-242">Attribute Name</span></span>   | <span data-ttu-id="8ace8-243">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-243">Is Required</span></span> | <span data-ttu-id="8ace8-244">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-245">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="8ace8-245">**Type**</span></span>         | <span data-ttu-id="8ace8-246">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-246">Yes</span></span>         | <span data-ttu-id="8ace8-247">Nombre completo del conjunto de entidades SSDL que está al final de la restricción FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="8ace8-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="8ace8-248">**Role**</span><span class="sxs-lookup"><span data-stu-id="8ace8-248">**Role**</span></span>         | <span data-ttu-id="8ace8-249">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-249">No</span></span>          | <span data-ttu-id="8ace8-250">El valor del atributo **role** en el elemento principal o dependiente del elemento ReferentialConstraint correspondiente (si se usa).</span><span class="sxs-lookup"><span data-stu-id="8ace8-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="8ace8-251">**Multiplicidad**</span><span class="sxs-lookup"><span data-stu-id="8ace8-251">**Multiplicity**</span></span> | <span data-ttu-id="8ace8-252">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-252">Yes</span></span>         | <span data-ttu-id="8ace8-253">**1**, **0.. 1**o **\*** dependiendo del número de filas que pueden estar al final de la restricción FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="8ace8-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="8ace8-254">**1** indica que existe exactamente una fila en el extremo de la restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="8ace8-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="8ace8-255">**0.. 1** indica que hay cero o una fila en el extremo de la restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="8ace8-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="8ace8-256">**\*** indica que hay cero, una o más filas en el extremo de la restricción de clave externa.</span><span class="sxs-lookup"><span data-stu-id="8ace8-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-257">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **final** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="8ace8-258">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8ace8-259">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="8ace8-260">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-260">Example</span></span>

<span data-ttu-id="8ace8-261">En el ejemplo siguiente se muestra un elemento **Association** que define la restricción de clave externa **FK @ no__t-2CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="8ace8-262">Los valores de **multiplicidad** especificados en cada elemento **final** indican que muchas filas de la tabla **Orders** se pueden asociar a una fila de la tabla **Customers** , pero solo se puede asociar una fila de la tabla **Customers** a una fila en la tabla **Orders** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="8ace8-263">Además, el elemento **aldelete** indica que todas las filas de la tabla **Orders** que hacen referencia a una fila determinada de la tabla **Customers** se eliminarán si se elimina la fila de la tabla **Customers** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="8ace8-264">Elemento End como elemento secundario del elemento AssociationSet</span><span class="sxs-lookup"><span data-stu-id="8ace8-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="8ace8-265">El elemento **End** (como elemento secundario del elemento **AssociationSet** ) especifica una tabla en un extremo de una restricción FOREIGN KEY en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="8ace8-266">Un elemento **End** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-267">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8ace8-268">Elementos Annotation (cero o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-269">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-269">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-270">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **End** cuando es el elemento secundario de un elemento **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="8ace8-271">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-271">Attribute Name</span></span> | <span data-ttu-id="8ace8-272">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-272">Is Required</span></span> | <span data-ttu-id="8ace8-273">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-274">**#A4**</span><span class="sxs-lookup"><span data-stu-id="8ace8-274">**EntitySet**</span></span>  | <span data-ttu-id="8ace8-275">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-275">Yes</span></span>         | <span data-ttu-id="8ace8-276">Nombre del conjunto de entidades SSDL que está al final de la restricción FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="8ace8-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="8ace8-277">**Role**</span><span class="sxs-lookup"><span data-stu-id="8ace8-277">**Role**</span></span>       | <span data-ttu-id="8ace8-278">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-278">No</span></span>          | <span data-ttu-id="8ace8-279">El valor de uno de los atributos de **rol** especificados en un elemento **final** del elemento Association correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-280">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **final** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="8ace8-281">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8ace8-282">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="8ace8-283">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-283">Example</span></span>

<span data-ttu-id="8ace8-284">En el ejemplo siguiente se muestra un elemento **EntityContainer** con un elemento **AssociationSet** con dos elementos **End** :</span><span class="sxs-lookup"><span data-stu-id="8ace8-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="8ace8-285">EntityContainer (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="8ace8-286">Un elemento **EntityContainer** en el lenguaje de definición de esquemas de almacenamiento (SSDL) describe la estructura del origen de datos subyacente en una aplicación Entity Framework: Los conjuntos de entidades SSDL (definidos en los elementos EntitySet) representan las tablas de una base de datos, los tipos de entidad SSDL (definidos en los elementos EntityType) representan las filas de una tabla, y los conjuntos de asociaciones (definidos en los elementos AssociationSet) representan restricciones de clave externa en un base.</span><span class="sxs-lookup"><span data-stu-id="8ace8-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="8ace8-287">Un contenedor de entidades del modelo de almacenamiento se asigna a un contenedor de entidades del modelo conceptual a través del elemento EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="8ace8-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="8ace8-288">Un elemento **EntityContainer** puede tener cero o un elemento de documentación.</span><span class="sxs-lookup"><span data-stu-id="8ace8-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="8ace8-289">Si hay un elemento de **documentación** , debe preceder a todos los demás elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="8ace8-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="8ace8-290">Un elemento **EntityContainer** puede tener cero o más de los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="8ace8-291">EntitySet</span></span>
-   <span data-ttu-id="8ace8-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="8ace8-292">AssociationSet</span></span>
-   <span data-ttu-id="8ace8-293">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="8ace8-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-294">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-294">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-295">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="8ace8-296">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-296">Attribute Name</span></span> | <span data-ttu-id="8ace8-297">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-297">Is Required</span></span> | <span data-ttu-id="8ace8-298">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="8ace8-299">**Name**</span></span>       | <span data-ttu-id="8ace8-300">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-300">Yes</span></span>         | <span data-ttu-id="8ace8-301">Nombre del contenedor de entidades.</span><span class="sxs-lookup"><span data-stu-id="8ace8-301">The name of the entity container.</span></span> <span data-ttu-id="8ace8-302">Este nombre no puede contener puntos (.).</span><span class="sxs-lookup"><span data-stu-id="8ace8-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-303">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="8ace8-304">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-305">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-306">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-306">Example</span></span>

<span data-ttu-id="8ace8-307">En el ejemplo siguiente se muestra un elemento **EntityContainer** que define dos conjuntos de entidades y un conjunto de asociaciones.</span><span class="sxs-lookup"><span data-stu-id="8ace8-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="8ace8-308">Observe que los nombres de los tipos de entidad y tipos de asociación están calificados mediante el nombre del espacio de nombres del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="8ace8-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="8ace8-309">EntitySet (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="8ace8-310">Un elemento **EntitySet** en el lenguaje de definición de esquemas de almacenamiento (SSDL) representa una tabla o una vista en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="8ace8-311">Un elemento EntityType en SSDL representa una fila de la tabla o vista.</span><span class="sxs-lookup"><span data-stu-id="8ace8-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="8ace8-312">El atributo **EntityType** de un elemento **EntitySet** especifica el tipo de entidad SSDL determinado que representa las filas de un conjunto de entidades SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="8ace8-313">La asignación entre un conjunto de entidades CSDL y un conjunto de entidades SSDL se especifica en un elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="8ace8-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="8ace8-314">El elemento **EntitySet** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-315">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="8ace8-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="8ace8-316">DefiningQuery (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="8ace8-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="8ace8-317">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="8ace8-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-318">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-318">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-319">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="8ace8-320">Algunos atributos (que no se enumeran aquí) pueden estar calificados con el alias de **almacén** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="8ace8-321">Este atributo lo usa el Asistente para actualizar modelo al actualizar un modelo.</span><span class="sxs-lookup"><span data-stu-id="8ace8-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="8ace8-322">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-322">Attribute Name</span></span> | <span data-ttu-id="8ace8-323">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-323">Is Required</span></span> | <span data-ttu-id="8ace8-324">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="8ace8-325">**Name**</span></span>       | <span data-ttu-id="8ace8-326">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-326">Yes</span></span>         | <span data-ttu-id="8ace8-327">Nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="8ace8-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="8ace8-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="8ace8-328">**EntityType**</span></span> | <span data-ttu-id="8ace8-329">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-329">Yes</span></span>         | <span data-ttu-id="8ace8-330">El nombre completo del tipo de entidad para el que el conjunto de entidades contiene las instancias.</span><span class="sxs-lookup"><span data-stu-id="8ace8-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="8ace8-331">**Esquema**</span><span class="sxs-lookup"><span data-stu-id="8ace8-331">**Schema**</span></span>     | <span data-ttu-id="8ace8-332">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-332">No</span></span>          | <span data-ttu-id="8ace8-333">El esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="8ace8-334">**Table**</span><span class="sxs-lookup"><span data-stu-id="8ace8-334">**Table**</span></span>      | <span data-ttu-id="8ace8-335">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-335">No</span></span>          | <span data-ttu-id="8ace8-336">La tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="8ace8-337">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="8ace8-338">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-339">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-340">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-340">Example</span></span>

<span data-ttu-id="8ace8-341">En el ejemplo siguiente se muestra un elemento **EntityContainer** que tiene dos elementos **EntitySet** y un elemento **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="8ace8-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="8ace8-342">EntityType (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="8ace8-343">Un elemento **EntityType** en el lenguaje de definición de esquemas de almacenamiento (SSDL) representa una fila de una tabla o vista de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="8ace8-344">Un elemento EntitySet en SSDL representa la tabla o vista en la que se producen las filas.</span><span class="sxs-lookup"><span data-stu-id="8ace8-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="8ace8-345">El atributo **EntityType** de un elemento **EntitySet** especifica el tipo de entidad SSDL determinado que representa las filas de un conjunto de entidades SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="8ace8-346">La asignación entre un tipo de entidad SSDL y un tipo de entidad CSDL se especifica en un elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="8ace8-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="8ace8-347">El elemento **EntityType** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-348">Documentation (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="8ace8-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="8ace8-349">Key (cero o un elemento)</span><span class="sxs-lookup"><span data-stu-id="8ace8-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="8ace8-350">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="8ace8-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-351">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-351">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-352">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="8ace8-353">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-353">Attribute Name</span></span> | <span data-ttu-id="8ace8-354">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-354">Is Required</span></span> | <span data-ttu-id="8ace8-355">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="8ace8-356">**Name**</span></span>       | <span data-ttu-id="8ace8-357">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-357">Yes</span></span>         | <span data-ttu-id="8ace8-358">Nombre del tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="8ace8-358">The name of the entity type.</span></span> <span data-ttu-id="8ace8-359">Este valor normalmente es igual que el nombre de la tabla en la que el tipo de entidad representa una fila.</span><span class="sxs-lookup"><span data-stu-id="8ace8-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="8ace8-360">Este valor no puede contener ningún punto (.).</span><span class="sxs-lookup"><span data-stu-id="8ace8-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-361">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="8ace8-362">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-363">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-364">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-364">Example</span></span>

<span data-ttu-id="8ace8-365">En el ejemplo siguiente se muestra un elemento **EntityType** con dos propiedades:</span><span class="sxs-lookup"><span data-stu-id="8ace8-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="8ace8-366">Function (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-366">Function Element (SSDL)</span></span>

<span data-ttu-id="8ace8-367">El elemento **function** del lenguaje de definición de esquemas de almacenamiento (SSDL) especifica un procedimiento almacenado que existe en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="8ace8-368">El elemento **function** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-369">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8ace8-370">Parámetro (cero o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="8ace8-371">CommandText (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="8ace8-372">ReturnType (cero o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="8ace8-373">Elementos Annotation (cero o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="8ace8-374">Un tipo de valor devuelto para una función debe especificarse con el elemento **ReturnType** o el atributo **ReturnType** (consulte a continuación), pero no ambos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="8ace8-375">Los procedimientos almacenados que se especifican en el modelo de almacenamiento se pueden importar en el modelo conceptual de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="8ace8-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="8ace8-376">Para obtener más información, vea [consultar con procedimientos almacenados](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="8ace8-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="8ace8-377">El elemento de **función** también se puede utilizar para definir funciones personalizadas en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="8ace8-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-378">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-378">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-379">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento de **función** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="8ace8-380">Algunos atributos (que no se enumeran aquí) pueden estar calificados con el alias de **almacén** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="8ace8-381">Este atributo lo usa el Asistente para actualizar modelo al actualizar un modelo.</span><span class="sxs-lookup"><span data-stu-id="8ace8-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="8ace8-382">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-382">Attribute Name</span></span>             | <span data-ttu-id="8ace8-383">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-383">Is Required</span></span> | <span data-ttu-id="8ace8-384">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="8ace8-385">**Name**</span></span>                   | <span data-ttu-id="8ace8-386">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-386">Yes</span></span>         | <span data-ttu-id="8ace8-387">Nombre del procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="8ace8-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="8ace8-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="8ace8-388">**ReturnType**</span></span>             | <span data-ttu-id="8ace8-389">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-389">No</span></span>          | <span data-ttu-id="8ace8-390">El tipo de valor devuelto del procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="8ace8-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="8ace8-391">**Aggregate**</span><span class="sxs-lookup"><span data-stu-id="8ace8-391">**Aggregate**</span></span>              | <span data-ttu-id="8ace8-392">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-392">No</span></span>          | <span data-ttu-id="8ace8-393">**True** si el procedimiento almacenado devuelve un valor agregado; en caso contrario, **false**.</span><span class="sxs-lookup"><span data-stu-id="8ace8-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="8ace8-394">**BuiltIn**</span><span class="sxs-lookup"><span data-stu-id="8ace8-394">**BuiltIn**</span></span>                | <span data-ttu-id="8ace8-395">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-395">No</span></span>          | <span data-ttu-id="8ace8-396">**True** si la función es una función integrada<sup>1</sup> ; en caso contrario, **false**.</span><span class="sxs-lookup"><span data-stu-id="8ace8-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="8ace8-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="8ace8-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="8ace8-398">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-398">No</span></span>          | <span data-ttu-id="8ace8-399">Nombre del procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="8ace8-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="8ace8-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="8ace8-400">**NiladicFunction**</span></span>        | <span data-ttu-id="8ace8-401">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-401">No</span></span>          | <span data-ttu-id="8ace8-402">**True** si la función es una función niládicas<sup>2</sup> ; De lo contrario, **false** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="8ace8-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="8ace8-403">**IsComposable**</span></span>           | <span data-ttu-id="8ace8-404">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-404">No</span></span>          | <span data-ttu-id="8ace8-405">**True** si la función es una función que admite composición<sup>3</sup> ; De lo contrario, **false** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="8ace8-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="8ace8-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="8ace8-407">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-407">No</span></span>          | <span data-ttu-id="8ace8-408">La enumeración que define la semántica de tipos que se utiliza para resolver sobrecargas de función.</span><span class="sxs-lookup"><span data-stu-id="8ace8-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="8ace8-409">La enumeración se define en el manifiesto del proveedor por cada definición de función.</span><span class="sxs-lookup"><span data-stu-id="8ace8-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="8ace8-410">El valor predeterminado es **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="8ace8-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="8ace8-411">**Esquema**</span><span class="sxs-lookup"><span data-stu-id="8ace8-411">**Schema**</span></span>                 | <span data-ttu-id="8ace8-412">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-412">No</span></span>          | <span data-ttu-id="8ace8-413">El nombre del esquema donde se define el procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="8ace8-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="8ace8-414"><sup>1</sup> una función integrada es una función que se define en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="8ace8-415">Para obtener información sobre las funciones que se definen en el modelo de almacenamiento, vea CommandText (elemento) (SSDL).</span><span class="sxs-lookup"><span data-stu-id="8ace8-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="8ace8-416"><sup>2</sup> una función niládicas es una función que no acepta parámetros y, cuando se llama, no requiere paréntesis.</span><span class="sxs-lookup"><span data-stu-id="8ace8-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="8ace8-417"><sup>3</sup> dos funciones son comparables si la salida de una función puede ser la entrada para la otra función.</span><span class="sxs-lookup"><span data-stu-id="8ace8-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="8ace8-418">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento de **función** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="8ace8-419">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-420">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-421">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-421">Example</span></span>

<span data-ttu-id="8ace8-422">En el ejemplo siguiente se muestra un elemento **function** que corresponde al procedimiento almacenado **UpdateOrderQuantity** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="8ace8-423">El procedimiento almacenado acepta dos parámetros y no devuelve ningún valor.</span><span class="sxs-lookup"><span data-stu-id="8ace8-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="8ace8-424">Key (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-424">Key Element (SSDL)</span></span>

<span data-ttu-id="8ace8-425">El elemento **key** del lenguaje de definición de esquemas de almacenamiento (SSDL) representa la clave principal de una tabla en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="8ace8-426">**Key** es un elemento secundario de un elemento EntityType, que representa una fila de una tabla.</span><span class="sxs-lookup"><span data-stu-id="8ace8-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="8ace8-427">La clave principal se define en el elemento **key** haciendo referencia a uno o varios elementos de propiedad que se definen en el elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="8ace8-428">El elemento **key** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-429">PropertyRef (uno o varios)</span><span class="sxs-lookup"><span data-stu-id="8ace8-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="8ace8-430">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="8ace8-430">Annotation elements</span></span>

<span data-ttu-id="8ace8-431">No hay atributos aplicables al elemento **key** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-432">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-432">Example</span></span>

<span data-ttu-id="8ace8-433">En el ejemplo siguiente se muestra un elemento **EntityType** con una clave que hace referencia a una propiedad:</span><span class="sxs-lookup"><span data-stu-id="8ace8-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="8ace8-434">OnDelete (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="8ace8-435">El elemento **aldelete** del lenguaje de definición de esquemas de almacenamiento (SSDL) refleja el comportamiento de la base de datos cuando se elimina una fila que participa en una restricción FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="8ace8-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="8ace8-436">Si la acción se establece en **Cascade**, también se eliminarán las filas que hacen referencia a una fila que se va a eliminar.</span><span class="sxs-lookup"><span data-stu-id="8ace8-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="8ace8-437">Si la acción se establece en **None**, las filas que hacen referencia a una fila que se va a eliminar también no se eliminan.</span><span class="sxs-lookup"><span data-stu-id="8ace8-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="8ace8-438">Un elemento **aldelete** es un elemento secundario de un elemento end.</span><span class="sxs-lookup"><span data-stu-id="8ace8-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="8ace8-439">Un elemento **aldelete** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-440">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8ace8-441">Elementos Annotation (cero o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-442">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-442">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-443">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **aleliminar** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="8ace8-444">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-444">Attribute Name</span></span> | <span data-ttu-id="8ace8-445">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-445">Is Required</span></span> | <span data-ttu-id="8ace8-446">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-447">**Acción**</span><span class="sxs-lookup"><span data-stu-id="8ace8-447">**Action**</span></span>     | <span data-ttu-id="8ace8-448">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-448">Yes</span></span>         | <span data-ttu-id="8ace8-449">**Cascade** o **None**.</span><span class="sxs-lookup"><span data-stu-id="8ace8-449">**Cascade** or **None**.</span></span> <span data-ttu-id="8ace8-450">(El valor **restringido** es válido, pero tiene el mismo comportamiento que **ninguno**).</span><span class="sxs-lookup"><span data-stu-id="8ace8-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-451">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **aleliminar** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="8ace8-452">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-453">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-454">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-454">Example</span></span>

<span data-ttu-id="8ace8-455">En el ejemplo siguiente se muestra un elemento **Association** que define la restricción de clave externa **FK @ no__t-2CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="8ace8-456">El elemento **aldelete** indica que se eliminarán todas las filas de la tabla **Orders** que hagan referencia a una fila determinada de la tabla **Customers** si se elimina la fila de la tabla **Customers** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="8ace8-457">Parameter (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="8ace8-458">El elemento **Parameter** del lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario del elemento function que especifica los parámetros de un procedimiento almacenado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="8ace8-459">El elemento **Parameter** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-460">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8ace8-461">Elementos Annotation (cero o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-462">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-462">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-463">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="8ace8-464">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-464">Attribute Name</span></span> | <span data-ttu-id="8ace8-465">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-465">Is Required</span></span> | <span data-ttu-id="8ace8-466">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="8ace8-467">**Name**</span></span>       | <span data-ttu-id="8ace8-468">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-468">Yes</span></span>         | <span data-ttu-id="8ace8-469">Nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="8ace8-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="8ace8-470">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="8ace8-470">**Type**</span></span>       | <span data-ttu-id="8ace8-471">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-471">Yes</span></span>         | <span data-ttu-id="8ace8-472">Tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="8ace8-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="8ace8-473">**Modo**</span><span class="sxs-lookup"><span data-stu-id="8ace8-473">**Mode**</span></span>       | <span data-ttu-id="8ace8-474">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-474">No</span></span>          | <span data-ttu-id="8ace8-475">**In**, **out**o **INOUT** dependiendo de si el parámetro es un parámetro de entrada, de salida o de entrada/salida.</span><span class="sxs-lookup"><span data-stu-id="8ace8-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="8ace8-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="8ace8-476">**MaxLength**</span></span>  | <span data-ttu-id="8ace8-477">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-477">No</span></span>          | <span data-ttu-id="8ace8-478">Longitud máxima permitida del parámetro.</span><span class="sxs-lookup"><span data-stu-id="8ace8-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="8ace8-479">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="8ace8-479">**Precision**</span></span>  | <span data-ttu-id="8ace8-480">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-480">No</span></span>          | <span data-ttu-id="8ace8-481">Precisión del parámetro.</span><span class="sxs-lookup"><span data-stu-id="8ace8-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="8ace8-482">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="8ace8-482">**Scale**</span></span>      | <span data-ttu-id="8ace8-483">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-483">No</span></span>          | <span data-ttu-id="8ace8-484">Escala del parámetro.</span><span class="sxs-lookup"><span data-stu-id="8ace8-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="8ace8-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="8ace8-485">**SRID**</span></span>       | <span data-ttu-id="8ace8-486">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-486">No</span></span>          | <span data-ttu-id="8ace8-487">Identificador de referencia del sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="8ace8-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="8ace8-488">Válido solo para los parámetros de los tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="8ace8-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="8ace8-489">Para obtener más información, vea [SRID](https://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ace8-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-490">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="8ace8-491">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-492">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-493">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-493">Example</span></span>

<span data-ttu-id="8ace8-494">En el ejemplo siguiente se muestra un elemento **function** que tiene dos elementos **Parameter** que especifican parámetros de entrada:</span><span class="sxs-lookup"><span data-stu-id="8ace8-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="8ace8-495">Principal (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="8ace8-496">El elemento **principal** del lenguaje de definición de esquemas de almacenamiento (SSDL) es un elemento secundario del elemento ReferentialConstraint que define el extremo principal de una restricción FOREIGN KEY (también denominada restricción referencial).</span><span class="sxs-lookup"><span data-stu-id="8ace8-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="8ace8-497">El elemento **principal** especifica la columna (o columnas) de clave principal de una tabla a la que hace referencia otra columna (o columnas).</span><span class="sxs-lookup"><span data-stu-id="8ace8-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="8ace8-498">Los elementos **PropertyRef** especifican a qué columnas se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="8ace8-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="8ace8-499">El elemento dependiente especifica las columnas que hacen referencia a las columnas de clave principal que se especifican en el elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="8ace8-500">El elemento **principal** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="8ace8-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8ace8-501">PropertyRef (uno o varios)</span><span class="sxs-lookup"><span data-stu-id="8ace8-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="8ace8-502">Elementos Annotation (cero o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-503">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-503">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-504">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="8ace8-505">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-505">Attribute Name</span></span> | <span data-ttu-id="8ace8-506">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-506">Is Required</span></span> | <span data-ttu-id="8ace8-507">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-508">**Role**</span><span class="sxs-lookup"><span data-stu-id="8ace8-508">**Role**</span></span>       | <span data-ttu-id="8ace8-509">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-509">Yes</span></span>         | <span data-ttu-id="8ace8-510">El mismo valor que el atributo **role** (si se usa) del elemento end correspondiente; de lo contrario, el nombre de la tabla que contiene la columna a la que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="8ace8-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-511">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **principal** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="8ace8-512">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8ace8-513">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-514">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-514">Example</span></span>

<span data-ttu-id="8ace8-515">En el ejemplo siguiente se muestra un elemento Association que utiliza un elemento **ReferentialConstraint** para especificar las columnas que participan en la restricción de clave externa **FK @ no__t-2CustomerOrders** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="8ace8-516">El elemento **principal** especifica la columna **CustomerID** de la tabla **Customer** como el extremo principal de la restricción.</span><span class="sxs-lookup"><span data-stu-id="8ace8-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="8ace8-517">Property (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-517">Property Element (SSDL)</span></span>

<span data-ttu-id="8ace8-518">El elemento **Property** del lenguaje de definición de esquemas de almacenamiento (SSDL) representa una columna de una tabla en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="8ace8-519">Los elementos de **propiedad** son elementos secundarios de elementos EntityType, que representan las filas de una tabla.</span><span class="sxs-lookup"><span data-stu-id="8ace8-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="8ace8-520">Cada elemento de **propiedad** definido en un elemento **EntityType** representa una columna.</span><span class="sxs-lookup"><span data-stu-id="8ace8-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="8ace8-521">Un elemento **Property** no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="8ace8-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-522">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-522">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-523">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **Property** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="8ace8-524">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-524">Attribute Name</span></span>            | <span data-ttu-id="8ace8-525">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-525">Is Required</span></span> | <span data-ttu-id="8ace8-526">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="8ace8-527">**Name**</span></span>                  | <span data-ttu-id="8ace8-528">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-528">Yes</span></span>         | <span data-ttu-id="8ace8-529">El nombre de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="8ace8-530">**Tipo**</span><span class="sxs-lookup"><span data-stu-id="8ace8-530">**Type**</span></span>                  | <span data-ttu-id="8ace8-531">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-531">Yes</span></span>         | <span data-ttu-id="8ace8-532">El tipo de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="8ace8-533">**Acepta valores NULL**</span><span class="sxs-lookup"><span data-stu-id="8ace8-533">**Nullable**</span></span>              | <span data-ttu-id="8ace8-534">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-534">No</span></span>          | <span data-ttu-id="8ace8-535">**True** (valor predeterminado) o **false** , dependiendo de si la columna correspondiente puede tener un valor null.</span><span class="sxs-lookup"><span data-stu-id="8ace8-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="8ace8-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="8ace8-536">**DefaultValue**</span></span>          | <span data-ttu-id="8ace8-537">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-537">No</span></span>          | <span data-ttu-id="8ace8-538">El valor predeterminado de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="8ace8-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="8ace8-539">**MaxLength**</span></span>             | <span data-ttu-id="8ace8-540">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-540">No</span></span>          | <span data-ttu-id="8ace8-541">La longitud máxima de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="8ace8-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="8ace8-542">**FixedLength**</span></span>           | <span data-ttu-id="8ace8-543">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-543">No</span></span>          | <span data-ttu-id="8ace8-544">**True** o **false** , dependiendo de si el valor de columna correspondiente se almacenará como una cadena de longitud fija.</span><span class="sxs-lookup"><span data-stu-id="8ace8-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="8ace8-545">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="8ace8-545">**Precision**</span></span>             | <span data-ttu-id="8ace8-546">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-546">No</span></span>          | <span data-ttu-id="8ace8-547">La precisión de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="8ace8-548">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="8ace8-548">**Scale**</span></span>                 | <span data-ttu-id="8ace8-549">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-549">No</span></span>          | <span data-ttu-id="8ace8-550">La escala de la columna correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="8ace8-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="8ace8-551">**Unicode**</span></span>               | <span data-ttu-id="8ace8-552">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-552">No</span></span>          | <span data-ttu-id="8ace8-553">**True** o **false** , dependiendo de si el valor de columna correspondiente se almacenará como una cadena Unicode.</span><span class="sxs-lookup"><span data-stu-id="8ace8-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="8ace8-554">**Intercalación**</span><span class="sxs-lookup"><span data-stu-id="8ace8-554">**Collation**</span></span>             | <span data-ttu-id="8ace8-555">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-555">No</span></span>          | <span data-ttu-id="8ace8-556">Cadena que especifica la secuencia de intercalación que se va a usar en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="8ace8-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="8ace8-557">**SRID**</span></span>                  | <span data-ttu-id="8ace8-558">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-558">No</span></span>          | <span data-ttu-id="8ace8-559">Identificador de referencia del sistema espacial.</span><span class="sxs-lookup"><span data-stu-id="8ace8-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="8ace8-560">Solo es válido para las propiedades de los tipos espaciales.</span><span class="sxs-lookup"><span data-stu-id="8ace8-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="8ace8-561">Para obtener más información, vea [SRID](https://en.wikipedia.org/wiki/SRID) y [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="8ace8-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="8ace8-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="8ace8-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="8ace8-563">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-563">No</span></span>          | <span data-ttu-id="8ace8-564">**None**, **Identity** (si el valor de la columna correspondiente es una identidad que se genera en la base de datos) o **calculado** (si el valor de la columna correspondiente se calcula en la base de datos).</span><span class="sxs-lookup"><span data-stu-id="8ace8-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="8ace8-565">No es válido para las propiedades RowType.</span><span class="sxs-lookup"><span data-stu-id="8ace8-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-566">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento de **propiedad** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="8ace8-567">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-568">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-569">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-569">Example</span></span>

<span data-ttu-id="8ace8-570">En el ejemplo siguiente se muestra un elemento **EntityType** con dos elementos **Property** secundarios:</span><span class="sxs-lookup"><span data-stu-id="8ace8-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="8ace8-571">PropertyRef (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="8ace8-572">El elemento **PropertyRef** en el lenguaje de definición de esquemas de almacenamiento (SSDL) hace referencia a una propiedad definida en un elemento EntityType para indicar que la propiedad realizará uno de los roles siguientes:</span><span class="sxs-lookup"><span data-stu-id="8ace8-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="8ace8-573">Formar parte de la clave principal de la tabla que el **EntityType** representa.</span><span class="sxs-lookup"><span data-stu-id="8ace8-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="8ace8-574">Se pueden usar uno o varios elementos **PropertyRef** para definir una clave principal.</span><span class="sxs-lookup"><span data-stu-id="8ace8-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="8ace8-575">Para obtener más información, vea Key (elemento).</span><span class="sxs-lookup"><span data-stu-id="8ace8-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="8ace8-576">Ser el extremo dependiente o principal de una restricción referencial.</span><span class="sxs-lookup"><span data-stu-id="8ace8-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="8ace8-577">Para obtener más información, vea ReferentialConstraint (elemento).</span><span class="sxs-lookup"><span data-stu-id="8ace8-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="8ace8-578">El elemento **PropertyRef** solo puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="8ace8-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="8ace8-579">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8ace8-580">Elementos Annotation</span><span class="sxs-lookup"><span data-stu-id="8ace8-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-581">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-581">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-582">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="8ace8-583">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-583">Attribute Name</span></span> | <span data-ttu-id="8ace8-584">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-584">Is Required</span></span> | <span data-ttu-id="8ace8-585">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="8ace8-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="8ace8-586">**Name**</span></span>       | <span data-ttu-id="8ace8-587">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-587">Yes</span></span>         | <span data-ttu-id="8ace8-588">Nombre de la propiedad a la que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="8ace8-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="8ace8-589">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="8ace8-590">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para CSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="8ace8-591">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-592">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-592">Example</span></span>

<span data-ttu-id="8ace8-593">En el ejemplo siguiente se muestra un elemento **PropertyRef** que se usa para definir una clave principal haciendo referencia a una propiedad que se define en un elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="8ace8-594">ReferentialConstraint (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="8ace8-595">El elemento **ReferentialConstraint** del lenguaje de definición de esquemas de almacenamiento (SSDL) representa una restricción de clave externa (también denominada restricción de integridad referencial) en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="8ace8-596">Los extremos principal y dependiente de la restricción se especifican mediante los elementos secundarios principales y dependientes, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="8ace8-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="8ace8-597">Se hace referencia a las columnas que participan en los extremos principal y dependiente con elementos PropertyRef.</span><span class="sxs-lookup"><span data-stu-id="8ace8-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="8ace8-598">El elemento **ReferentialConstraint** es un elemento secundario opcional del elemento Association.</span><span class="sxs-lookup"><span data-stu-id="8ace8-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="8ace8-599">Si no se utiliza un elemento **ReferentialConstraint** para asignar la restricción FOREIGN KEY especificada en el elemento **Association** , se debe usar un elemento AssociationSetMapping para hacerlo.</span><span class="sxs-lookup"><span data-stu-id="8ace8-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="8ace8-600">El elemento **ReferentialConstraint** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="8ace8-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="8ace8-601">Documentación (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="8ace8-602">Principal (exactamente uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="8ace8-603">Dependiente (exactamente uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="8ace8-604">Elementos Annotation (cero o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-605">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-605">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-606">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="8ace8-607">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-608">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-609">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-609">Example</span></span>

<span data-ttu-id="8ace8-610">En el ejemplo siguiente se muestra un elemento **Association** que usa un elemento **ReferentialConstraint** para especificar las columnas que participan en la restricción de clave externa **FK @ no__t-3CustomerOrders** :</span><span class="sxs-lookup"><span data-stu-id="8ace8-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="8ace8-611">ReturnType (elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="8ace8-612">El elemento **ReturnType** del lenguaje de definición de esquemas de almacenamiento (SSDL) especifica el tipo de valor devuelto para una función que se define en un elemento de **función** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="8ace8-613">Un tipo de valor devuelto de función también se puede especificar con un atributo **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="8ace8-614">El tipo de valor devuelto de una función se especifica con el atributo **Type** o el elemento **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="8ace8-615">El elemento **ReturnType** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="8ace8-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="8ace8-616">CollectionType (uno)</span><span class="sxs-lookup"><span data-stu-id="8ace8-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="8ace8-617">Se puede aplicar cualquier número de atributos de anotación (atributos XML personalizados) al elemento **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="8ace8-618">Sin embargo, es posible que los atributos personalizados no pertenezcan a ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="8ace8-619">Dos atributos personalizados cualesquiera no pueden tener nombres completos idénticos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-620">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-620">Example</span></span>

<span data-ttu-id="8ace8-621">En el ejemplo siguiente se utiliza una **función** que devuelve una colección de filas.</span><span class="sxs-lookup"><span data-stu-id="8ace8-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="8ace8-622">RowType (elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="8ace8-623">Un elemento **RowType** en el lenguaje de definición de esquemas de almacenamiento (SSDL) define una estructura sin nombre como un tipo de valor devuelto para una función definida en el almacén.</span><span class="sxs-lookup"><span data-stu-id="8ace8-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="8ace8-624">Un elemento **RowType** es el elemento secundario del elemento **CollectionType** :</span><span class="sxs-lookup"><span data-stu-id="8ace8-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="8ace8-625">Un elemento **RowType** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="8ace8-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="8ace8-626">Propiedad (uno o más)</span><span class="sxs-lookup"><span data-stu-id="8ace8-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="8ace8-627">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-627">Example</span></span>

<span data-ttu-id="8ace8-628">En el ejemplo siguiente se muestra una función de almacenamiento que usa un elemento **CollectionType** para especificar que la función devuelve una colección de filas (tal y como se especifica en el elemento **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="8ace8-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="8ace8-629">Schema (Elemento) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="8ace8-630">El elemento **Schema** en el lenguaje de definición de esquemas de almacenamiento (SSDL) es el elemento raíz de una definición de modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="8ace8-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="8ace8-631">Contiene las definiciones para los objetos, las funciones y los contenedores que conforman un modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="8ace8-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="8ace8-632">El elemento **Schema** puede contener cero o más de los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="8ace8-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="8ace8-633">Asociación</span><span class="sxs-lookup"><span data-stu-id="8ace8-633">Association</span></span>
-   <span data-ttu-id="8ace8-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="8ace8-634">EntityType</span></span>
-   <span data-ttu-id="8ace8-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="8ace8-635">EntityContainer</span></span>
-   <span data-ttu-id="8ace8-636">Función</span><span class="sxs-lookup"><span data-stu-id="8ace8-636">Function</span></span>

<span data-ttu-id="8ace8-637">El elemento **Schema** usa el atributo **namespace** para definir el espacio de nombres para el tipo de entidad y los objetos de asociación en un modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="8ace8-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="8ace8-638">Dentro de un espacio de nombres, no puede haber dos objetos con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="8ace8-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="8ace8-639">Un espacio de nombres del modelo de almacenamiento es diferente del espacio de nombres XML del elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="8ace8-640">Un espacio de nombres del modelo de almacenamiento (tal y como se define en el atributo de **espacio de nombres** ) es un contenedor lógico para tipos de entidad y tipos de asociación.</span><span class="sxs-lookup"><span data-stu-id="8ace8-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="8ace8-641">El espacio de nombres XML (indicado por el atributo **xmlns** ) de un elemento **Schema** es el espacio de nombres predeterminado para los elementos secundarios y los atributos del elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="8ace8-642">Los espacios de nombres XML con el formato https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (donde YYYY y MM representan un año y un mes respectivamente) se reservan para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="8ace8-643">No puede haber elementos y atributos personalizados en espacios de nombres que tengan este formato.</span><span class="sxs-lookup"><span data-stu-id="8ace8-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8ace8-644">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="8ace8-644">Applicable Attributes</span></span>

<span data-ttu-id="8ace8-645">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **Schema** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="8ace8-646">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="8ace8-646">Attribute Name</span></span>            | <span data-ttu-id="8ace8-647">Es necesario</span><span class="sxs-lookup"><span data-stu-id="8ace8-647">Is Required</span></span> | <span data-ttu-id="8ace8-648">Valor</span><span class="sxs-lookup"><span data-stu-id="8ace8-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-649">**Espacio de nombres**</span><span class="sxs-lookup"><span data-stu-id="8ace8-649">**Namespace**</span></span>             | <span data-ttu-id="8ace8-650">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-650">Yes</span></span>         | <span data-ttu-id="8ace8-651">El espacio de nombres del modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="8ace8-651">The namespace of the storage model.</span></span> <span data-ttu-id="8ace8-652">El valor del atributo **namespace** se usa para formar el nombre completo de un tipo.</span><span class="sxs-lookup"><span data-stu-id="8ace8-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="8ace8-653">Por ejemplo, si un **EntityType** denominado *Customer* está en el espacio de nombres ExampleModel. Store, el nombre completo del **EntityType** es ExampleModel. Store. Customer.</span><span class="sxs-lookup"><span data-stu-id="8ace8-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="8ace8-654">Las siguientes cadenas no se pueden usar como valor para el atributo de **espacio de nombres** : **Sistema**, **transitorio**o **EDM**.</span><span class="sxs-lookup"><span data-stu-id="8ace8-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="8ace8-655">El valor del atributo **namespace** no puede ser el mismo que el valor del atributo **namespace** del elemento Schema de CSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="8ace8-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="8ace8-656">**Alias**</span></span>                 | <span data-ttu-id="8ace8-657">No</span><span class="sxs-lookup"><span data-stu-id="8ace8-657">No</span></span>          | <span data-ttu-id="8ace8-658">Un identificador usado en lugar del nombre del espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="8ace8-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="8ace8-659">Por ejemplo, si un **EntityType** denominado *Customer* está en el espacio de nombres ExampleModel. Store y el valor del atributo **alias** es *StorageModel*, puede usar StorageModel. Customer como nombre completo del  **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="8ace8-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="8ace8-660">**Proveedor**</span><span class="sxs-lookup"><span data-stu-id="8ace8-660">**Provider**</span></span>              | <span data-ttu-id="8ace8-661">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-661">Yes</span></span>         | <span data-ttu-id="8ace8-662">El proveedor de datos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="8ace8-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="8ace8-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="8ace8-664">Sí</span><span class="sxs-lookup"><span data-stu-id="8ace8-664">Yes</span></span>         | <span data-ttu-id="8ace8-665">Un token que indica al proveedor qué manifiesto del proveedor debe devolver.</span><span class="sxs-lookup"><span data-stu-id="8ace8-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="8ace8-666">No se define ningún formato para el token.</span><span class="sxs-lookup"><span data-stu-id="8ace8-666">No format for the token is defined.</span></span> <span data-ttu-id="8ace8-667">El proveedor define los valores para el token.</span><span class="sxs-lookup"><span data-stu-id="8ace8-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="8ace8-668">Para obtener información sobre los tokens del manifiesto del proveedor de SQL Server, vea SqlClient para Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8ace8-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="8ace8-669">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-669">Example</span></span>

<span data-ttu-id="8ace8-670">En el ejemplo siguiente se muestra un elemento **Schema** que contiene un elemento **EntityContainer** , dos elementos **EntityType** y un elemento **Association** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="https://schemas.microsoft.com/ado/2009/11/edm/ssdl">
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

## <a name="annotation-attributes"></a><span data-ttu-id="8ace8-671">Atributos de anotación</span><span class="sxs-lookup"><span data-stu-id="8ace8-671">Annotation Attributes</span></span>

<span data-ttu-id="8ace8-672">Los atributos de anotación (Annotation) del lenguaje de definición de esquemas de almacenamiento (SSDL) son atributos XML personalizados del modelo de almacenamiento que proporcionan metadatos adicionales sobre los elementos del modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="8ace8-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="8ace8-673">Además de tener una estructura XML válida, las siguientes restricciones se aplican a los atributos de anotación:</span><span class="sxs-lookup"><span data-stu-id="8ace8-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="8ace8-674">Los atributos de anotación no deben estar en ningún espacio de nombres XML reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="8ace8-675">Dos atributos de anotación cualesquiera no pueden tener el mismo nombre completo.</span><span class="sxs-lookup"><span data-stu-id="8ace8-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="8ace8-676">Se pueden aplicar varios atributos de anotación a un elemento SSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="8ace8-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="8ace8-677">Se puede tener acceso a los metadatos contenidos en los elementos Annotation en tiempo de ejecución mediante el uso de clases en el espacio de nombres System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="8ace8-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-678">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-678">Example</span></span>

<span data-ttu-id="8ace8-679">En el ejemplo siguiente se muestra un elemento EntityType que tiene un atributo Annotation aplicado a la propiedad **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="8ace8-680">En el ejemplo también se muestra un elemento Annotation agregado al elemento **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="8ace8-681">Annotation (Elementos) (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="8ace8-682">Los elementos Annotation del lenguaje de definición de esquemas de almacenamiento (SSDL) son elementos XML personalizados del modelo de almacenamiento que proporcionan metadatos adicionales sobre el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="8ace8-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="8ace8-683">Además de tener una estructura XML válida, las siguientes restricciones se aplican a los elementos Annotation:</span><span class="sxs-lookup"><span data-stu-id="8ace8-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="8ace8-684">Los elementos Annotation no deben estar en un espacio de nombres de XML que esté reservado para SSDL.</span><span class="sxs-lookup"><span data-stu-id="8ace8-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="8ace8-685">Los nombres completos de dos elementos Annotation cualesquiera no deben ser los mismos.</span><span class="sxs-lookup"><span data-stu-id="8ace8-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="8ace8-686">Los elementos Annotation deben aparecer después de todos los demás elementos secundarios de un elemento SSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="8ace8-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="8ace8-687">Más de un elemento Annotation puede ser un elemento secundario de un elemento SSDL determinado.</span><span class="sxs-lookup"><span data-stu-id="8ace8-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="8ace8-688">A partir de la .NET Framework versión 4, se puede tener acceso a los metadatos contenidos en los elementos Annotation en tiempo de ejecución mediante el uso de clases en el espacio de nombres System. Data. Metadata. Edm.</span><span class="sxs-lookup"><span data-stu-id="8ace8-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="8ace8-689">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="8ace8-689">Example</span></span>

<span data-ttu-id="8ace8-690">En el ejemplo siguiente se muestra un elemento EntityType que tiene un elemento Annotation (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="8ace8-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="8ace8-691">En el ejemplo también se muestra un atributo de anotación aplicado a la propiedad **OrderID** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="8ace8-692">Facetas (SSDL)</span><span class="sxs-lookup"><span data-stu-id="8ace8-692">Facets (SSDL)</span></span>

<span data-ttu-id="8ace8-693">Las facetas del lenguaje de definición de esquemas de almacenamiento (SSDL) representan restricciones en los tipos de columna que se especifican en los elementos de propiedad.</span><span class="sxs-lookup"><span data-stu-id="8ace8-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="8ace8-694">Las caras se implementan como atributos XML en los elementos de **propiedad** .</span><span class="sxs-lookup"><span data-stu-id="8ace8-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="8ace8-695">En la tabla siguiente se describen las facetas que se admiten en SSDL:</span><span class="sxs-lookup"><span data-stu-id="8ace8-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="8ace8-696">Faceta</span><span class="sxs-lookup"><span data-stu-id="8ace8-696">Facet</span></span>           | <span data-ttu-id="8ace8-697">Descripción</span><span class="sxs-lookup"><span data-stu-id="8ace8-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8ace8-698">**Intercalación**</span><span class="sxs-lookup"><span data-stu-id="8ace8-698">**Collation**</span></span>   | <span data-ttu-id="8ace8-699">Especifica la secuencia de intercalación (o secuencia de orden) que se va a usar cuando se realicen las operaciones de comparación y ordenación sobre los valores de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="8ace8-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="8ace8-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="8ace8-700">**FixedLength**</span></span> | <span data-ttu-id="8ace8-701">Especifica si la longitud del valor de la columna puede variar.</span><span class="sxs-lookup"><span data-stu-id="8ace8-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="8ace8-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="8ace8-702">**MaxLength**</span></span>   | <span data-ttu-id="8ace8-703">Especifica la longitud máxima del valor de la columna.</span><span class="sxs-lookup"><span data-stu-id="8ace8-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="8ace8-704">**Precisión**</span><span class="sxs-lookup"><span data-stu-id="8ace8-704">**Precision**</span></span>   | <span data-ttu-id="8ace8-705">Para las propiedades de tipo **decimal**, especifica el número de dígitos que puede tener un valor de propiedad.</span><span class="sxs-lookup"><span data-stu-id="8ace8-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="8ace8-706">Para las propiedades de tipo **Time**, **DateTime**y **DateTimeOffset**, especifica el número de dígitos para la parte fraccionaria de los segundos del valor de la columna.</span><span class="sxs-lookup"><span data-stu-id="8ace8-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="8ace8-707">**Escalar**</span><span class="sxs-lookup"><span data-stu-id="8ace8-707">**Scale**</span></span>       | <span data-ttu-id="8ace8-708">Especifica el número de dígitos que puede haber a la derecha del separador de decimales para el valor de la columna.</span><span class="sxs-lookup"><span data-stu-id="8ace8-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="8ace8-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="8ace8-709">**Unicode**</span></span>     | <span data-ttu-id="8ace8-710">Indica si el valor de la columna está almacenado como Unicode.</span><span class="sxs-lookup"><span data-stu-id="8ace8-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
