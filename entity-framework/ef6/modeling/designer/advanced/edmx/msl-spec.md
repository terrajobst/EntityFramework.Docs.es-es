---
title: 'Especificación de MSL: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182552"
---
# <a name="msl-specification"></a><span data-ttu-id="965c4-102">Especificación MSL</span><span class="sxs-lookup"><span data-stu-id="965c4-102">MSL Specification</span></span>
<span data-ttu-id="965c4-103">El lenguaje de especificación de asignaciones (MSL) es un lenguaje basado en XML que describe la asignación entre el modelo conceptual y el modelo de almacenamiento de una aplicación Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="965c4-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="965c4-104">En una aplicación Entity Framework, la asignación de metadatos se carga desde un archivo. MSL (escrito en MSL) en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="965c4-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="965c4-105">Entity Framework usa la asignación de metadatos en tiempo de ejecución para traducir las consultas en el modelo conceptual para almacenar comandos específicos de.</span><span class="sxs-lookup"><span data-stu-id="965c4-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="965c4-106">El Entity Framework Designer (EF Designer) almacena la información de asignación en un archivo. edmx en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="965c4-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="965c4-107">En tiempo de compilación, Entity Designer usa información en un archivo. edmx para crear el archivo. MSL que Entity Framework necesita en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="965c4-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="965c4-108">Los nombres de todos los tipos de modelos conceptuales o de almacenamiento a los que se hace referencia en MSL deben estar calificados con sus respectivos nombres de espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="965c4-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="965c4-109">Para obtener información sobre el nombre del espacio de nombres del modelo conceptual, vea [especificación de CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="965c4-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="965c4-110">Para obtener información sobre el nombre del espacio de nombres del modelo de almacenamiento, vea [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="965c4-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="965c4-111">Las versiones de MSL se diferencian en los espacios de nombres XML.</span><span class="sxs-lookup"><span data-stu-id="965c4-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="965c4-112">Versión de MSL</span><span class="sxs-lookup"><span data-stu-id="965c4-112">MSL Version</span></span> | <span data-ttu-id="965c4-113">Espacio de nombres XML</span><span class="sxs-lookup"><span data-stu-id="965c4-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="965c4-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="965c4-114">MSL v1</span></span>      | <span data-ttu-id="965c4-115">urn: schemas-microsoft-com: Windows: Storage: asignación: CS</span><span class="sxs-lookup"><span data-stu-id="965c4-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="965c4-116">MSL V2</span><span class="sxs-lookup"><span data-stu-id="965c4-116">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="965c4-117">MSL V3</span><span class="sxs-lookup"><span data-stu-id="965c4-117">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="965c4-118">Alias (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-118">Alias Element (MSL)</span></span>

<span data-ttu-id="965c4-119">El elemento **alias** del lenguaje de especificación de asignaciones (MSL) es un elemento secundario del elemento mapping que se utiliza para definir los alias para los espacios de nombres de modelos conceptuales y de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="965c4-120">Los nombres de todos los tipos de modelos conceptuales o de almacenamiento a los que se hace referencia en MSL deben estar calificados con sus respectivos nombres de espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="965c4-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="965c4-121">Para obtener información sobre el nombre del espacio de nombres del modelo conceptual, vea elemento Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="965c4-122">Para obtener información sobre el nombre del espacio de nombres del modelo de almacenamiento, vea schema (elemento) (SSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="965c4-123">El elemento **alias** no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="965c4-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-124">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-124">Applicable Attributes</span></span>

<span data-ttu-id="965c4-125">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **alias** .</span><span class="sxs-lookup"><span data-stu-id="965c4-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="965c4-126">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-126">Attribute Name</span></span> | <span data-ttu-id="965c4-127">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-127">Is Required</span></span> | <span data-ttu-id="965c4-128">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="965c4-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="965c4-129">**Key**</span></span>        | <span data-ttu-id="965c4-130">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-130">Yes</span></span>         | <span data-ttu-id="965c4-131">El alias para el espacio de nombres especificado por el atributo de **valor** .</span><span class="sxs-lookup"><span data-stu-id="965c4-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="965c4-132">**Valor**</span><span class="sxs-lookup"><span data-stu-id="965c4-132">**Value**</span></span>      | <span data-ttu-id="965c4-133">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-133">Yes</span></span>         | <span data-ttu-id="965c4-134">Espacio de nombres para el que el valor del elemento **key** es un alias.</span><span class="sxs-lookup"><span data-stu-id="965c4-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="965c4-135">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-135">Example</span></span>

<span data-ttu-id="965c4-136">En el ejemplo siguiente se muestra un elemento **alias** que define un alias, `c`, para los tipos que se definen en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a><span data-ttu-id="965c4-137">AssociationEnd (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="965c4-138">El elemento **AssociationEnd** del lenguaje de especificación de asignaciones (MSL) se utiliza cuando las funciones de modificación de un tipo de entidad en el modelo conceptual se asignan a los procedimientos almacenados en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="965c4-139">Si un procedimiento almacenado de modificación toma un parámetro cuyo valor se mantiene en una propiedad de asociación, el elemento **AssociationEnd** asigna el valor de la propiedad al parámetro.</span><span class="sxs-lookup"><span data-stu-id="965c4-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="965c4-140">Para obtener más información, vea el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="965c4-140">For more information, see the example below.</span></span>

<span data-ttu-id="965c4-141">Para obtener más información sobre la asignación de funciones de modificación de tipos de entidad a procedimientos almacenados, vea ModificationFunctionMapping (elemento) (MSL) y Walkthrough: Asignar una entidad a procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="965c4-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="965c4-142">El elemento **AssociationEnd** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="965c4-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="965c4-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="965c4-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-144">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-144">Applicable Attributes</span></span>

<span data-ttu-id="965c4-145">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **AssociationEnd** .</span><span class="sxs-lookup"><span data-stu-id="965c4-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="965c4-146">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-146">Attribute Name</span></span>     | <span data-ttu-id="965c4-147">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-147">Is Required</span></span> | <span data-ttu-id="965c4-148">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="965c4-149">**AssociationSet**</span></span> | <span data-ttu-id="965c4-150">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-150">Yes</span></span>         | <span data-ttu-id="965c4-151">El nombre de la asociación que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="965c4-152">**From**</span><span class="sxs-lookup"><span data-stu-id="965c4-152">**From**</span></span>           | <span data-ttu-id="965c4-153">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-153">Yes</span></span>         | <span data-ttu-id="965c4-154">Valor del atributo **FromRole** de la propiedad de navegación que corresponde a la asociación que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="965c4-155">Para obtener más información, vea elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="965c4-156">**En**</span><span class="sxs-lookup"><span data-stu-id="965c4-156">**To**</span></span>             | <span data-ttu-id="965c4-157">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-157">Yes</span></span>         | <span data-ttu-id="965c4-158">Valor del atributo **ToRole** de la propiedad de navegación que corresponde a la asociación que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="965c4-159">Para obtener más información, vea elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="965c4-160">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-160">Example</span></span>

<span data-ttu-id="965c4-161">Considere el siguiente tipo de entidad del modelo conceptual:</span><span class="sxs-lookup"><span data-stu-id="965c4-161">Consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

<span data-ttu-id="965c4-162">Considere también el siguiente procedimiento almacenado:</span><span class="sxs-lookup"><span data-stu-id="965c4-162">Also consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

<span data-ttu-id="965c4-163">Para asignar la función de actualización de la entidad `Course` a este procedimiento almacenado, debe proporcionar un valor al parámetro **departmentId** .</span><span class="sxs-lookup"><span data-stu-id="965c4-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="965c4-164">El valor para `DepartmentID` no corresponde a una propiedad del tipo de entidad; está contenido en una asociación independiente cuya asignación se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="965c4-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

<span data-ttu-id="965c4-165">En el código siguiente se muestra el elemento **AssociationEnd** que se usa para asignar la propiedad **departmentId** de la Asociación **FK @ no__t-3Course @ no__t-4Department** al procedimiento almacenado **UpdateCourse** (al que la función de actualización del El tipo de entidad **Course** está asignado):</span><span class="sxs-lookup"><span data-stu-id="965c4-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="965c4-166">AssociationSetMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="965c4-167">El elemento **AssociationSetMapping** del lenguaje de especificación de asignaciones (MSL) define la asignación entre una asociación en el modelo conceptual y las columnas de tabla en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="965c4-168">Las asociaciones del modelo conceptual son tipos cuyas propiedades representan columnas de clave primaria y clave externa de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="965c4-169">El elemento **AssociationSetMapping** utiliza dos elementos EndProperty para definir las asignaciones entre las propiedades de tipo de asociación y las columnas de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="965c4-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="965c4-170">Puede colocar condiciones en estas asignaciones con el elemento condition.</span><span class="sxs-lookup"><span data-stu-id="965c4-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="965c4-171">Asigne las funciones de inserción, actualización y eliminación para las asociaciones a los procedimientos almacenados en la base de datos con el elemento ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="965c4-172">Defina las asignaciones de solo lectura entre las asociaciones y las columnas de la tabla mediante una Entity SQL cadena en un elemento QueryView.</span><span class="sxs-lookup"><span data-stu-id="965c4-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="965c4-173">Si se define una restricción referencial para una asociación en el modelo conceptual, no es necesario asignar la asociación con un elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="965c4-174">Si hay un elemento **AssociationSetMapping** para una asociación que tiene una restricción referencial, se omitirán las asignaciones definidas en el elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="965c4-175">Para obtener más información, vea ReferentialConstraint (elemento) (CSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="965c4-176">El elemento **AssociationSetMapping** puede tener los siguientes elementos secundarios</span><span class="sxs-lookup"><span data-stu-id="965c4-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="965c4-177">QueryView (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="965c4-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="965c4-178">EndProperty (cero o dos)</span><span class="sxs-lookup"><span data-stu-id="965c4-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="965c4-179">Condición (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="965c4-180">ModificationFunctionMapping (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="965c4-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-181">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-181">Applicable Attributes</span></span>

<span data-ttu-id="965c4-182">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="965c4-183">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-183">Attribute Name</span></span>     | <span data-ttu-id="965c4-184">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-184">Is Required</span></span> | <span data-ttu-id="965c4-185">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-186">**Name**</span><span class="sxs-lookup"><span data-stu-id="965c4-186">**Name**</span></span>           | <span data-ttu-id="965c4-187">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-187">Yes</span></span>         | <span data-ttu-id="965c4-188">El nombre del conjunto de asociaciones del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="965c4-189">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="965c4-189">**TypeName**</span></span>       | <span data-ttu-id="965c4-190">No</span><span class="sxs-lookup"><span data-stu-id="965c4-190">No</span></span>          | <span data-ttu-id="965c4-191">El nombre completo, calificado con el espacio de nombres, del tipo de asociación del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="965c4-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="965c4-192">**StoreEntitySet**</span></span> | <span data-ttu-id="965c4-193">No</span><span class="sxs-lookup"><span data-stu-id="965c4-193">No</span></span>          | <span data-ttu-id="965c4-194">El nombre de la tabla que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="965c4-195">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-195">Example</span></span>

<span data-ttu-id="965c4-196">En el ejemplo siguiente se muestra un elemento **AssociationSetMapping** en el que el conjunto de asociaciones **FK @ no__t-2Course @ no__t-3Department** del modelo conceptual se asigna a la tabla **Course** en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="965c4-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="965c4-197">Las asignaciones entre las propiedades de tipo de asociación y las columnas de tabla se especifican en los elementos secundarios **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="965c4-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

## <a name="complexproperty-element-msl"></a><span data-ttu-id="965c4-198">ComplexProperty (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="965c4-199">Un elemento **ComplexProperty** en el lenguaje de especificación de asignaciones (MSL) define la asignación entre una propiedad de tipo complejo en un tipo de entidad del modelo conceptual y las columnas de la tabla en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="965c4-200">Las asignaciones de columna de propiedades se especifican en los elementos de ScalarProperty secundarios.</span><span class="sxs-lookup"><span data-stu-id="965c4-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="965c4-201">El elemento de propiedad **complexType** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="965c4-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="965c4-202">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="965c4-203">**ComplexProperty** (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="965c4-204">ComplextTypeMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="965c4-205">Condición (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-206">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-206">Applicable Attributes</span></span>

<span data-ttu-id="965c4-207">En la tabla siguiente se describen los atributos aplicables al elemento **ComplexProperty** :</span><span class="sxs-lookup"><span data-stu-id="965c4-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="965c4-208">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-208">Attribute Name</span></span> | <span data-ttu-id="965c4-209">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-209">Is Required</span></span> | <span data-ttu-id="965c4-210">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-211">**Name**</span><span class="sxs-lookup"><span data-stu-id="965c4-211">**Name**</span></span>       | <span data-ttu-id="965c4-212">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-212">Yes</span></span>         | <span data-ttu-id="965c4-213">El nombre de la propiedad compleja de un tipo de entidad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="965c4-214">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="965c4-214">**TypeName**</span></span>   | <span data-ttu-id="965c4-215">No</span><span class="sxs-lookup"><span data-stu-id="965c4-215">No</span></span>          | <span data-ttu-id="965c4-216">El nombre completo, calificado con el espacio de nombres, del tipo de propiedad del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="965c4-217">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-217">Example</span></span>

<span data-ttu-id="965c4-218">El ejemplo siguiente se basa en el modelo School.</span><span class="sxs-lookup"><span data-stu-id="965c4-218">The following example is based on the School model.</span></span> <span data-ttu-id="965c4-219">El siguiente tipo complejo se ha agregado al modelo conceptual:</span><span class="sxs-lookup"><span data-stu-id="965c4-219">The following complex type has been added to the conceptual model:</span></span>

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

<span data-ttu-id="965c4-220">Las propiedades **LastName** y **FirstName** del tipo de entidad **Person** se han reemplazado por una propiedad compleja, **Name**:</span><span class="sxs-lookup"><span data-stu-id="965c4-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

<span data-ttu-id="965c4-221">En el siguiente MSL se muestra el elemento **ComplexProperty** que se usa para asignar la propiedad **Name** a las columnas de la base de datos subyacente:</span><span class="sxs-lookup"><span data-stu-id="965c4-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="965c4-222">ComplexTypeMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="965c4-223">El elemento **ComplexTypeMapping** del lenguaje de especificación de asignaciones (MSL) es un elemento secundario del elemento ResultMapping y define la asignación entre una importación de función en el modelo conceptual y un procedimiento almacenado en la base de datos subyacente cuando lo siguiente son true:</span><span class="sxs-lookup"><span data-stu-id="965c4-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="965c4-224">La importación de función devuelve un tipo complejo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="965c4-225">Los nombres de las columnas devueltas por el procedimiento almacenado no coinciden exactamente con los nombres de las propiedades en el tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="965c4-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="965c4-226">De forma predeterminada, la asignación entre las columnas devueltas por un procedimiento almacenado y un tipo complejo se basa en los nombres de las propiedades y de las columnas.</span><span class="sxs-lookup"><span data-stu-id="965c4-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="965c4-227">Si los nombres de columna no coinciden exactamente con los nombres de propiedad, debe utilizar el elemento **ComplexTypeMapping** para definir la asignación.</span><span class="sxs-lookup"><span data-stu-id="965c4-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="965c4-228">Para obtener un ejemplo de la asignación predeterminada, vea FunctionImportMapping (elemento) (MSL).</span><span class="sxs-lookup"><span data-stu-id="965c4-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="965c4-229">El elemento **ComplexTypeMapping** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="965c4-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="965c4-230">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-231">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-231">Applicable Attributes</span></span>

<span data-ttu-id="965c4-232">En la tabla siguiente se describen los atributos aplicables al elemento **ComplexTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="965c4-233">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-233">Attribute Name</span></span> | <span data-ttu-id="965c4-234">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-234">Is Required</span></span> | <span data-ttu-id="965c4-235">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="965c4-236">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="965c4-236">**TypeName**</span></span>   | <span data-ttu-id="965c4-237">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-237">Yes</span></span>         | <span data-ttu-id="965c4-238">El nombre completo, incluido el espacio de nombres, del tipo complejo que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="965c4-239">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-239">Example</span></span>

<span data-ttu-id="965c4-240">Considere el siguiente procedimiento almacenado:</span><span class="sxs-lookup"><span data-stu-id="965c4-240">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="965c4-241">Considere también el siguiente tipo complejo del modelo conceptual:</span><span class="sxs-lookup"><span data-stu-id="965c4-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="965c4-242">Para crear una importación de función que devuelva instancias del tipo complejo anterior, la asignación entre las columnas devueltas por el procedimiento almacenado y el tipo de entidad debe definirse en un elemento **ComplexTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="965c4-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a><span data-ttu-id="965c4-243">Condition (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-243">Condition Element (MSL)</span></span>

<span data-ttu-id="965c4-244">El elemento **Condition** del lenguaje de especificación de asignaciones (MSL) coloca condiciones en las asignaciones entre el modelo conceptual y la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="965c4-245">La asignación que se define dentro de un nodo XML es válida si se cumplen todas las condiciones, como se especifica en los elementos de **condición** secundarios.</span><span class="sxs-lookup"><span data-stu-id="965c4-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="965c4-246">De lo contrario, la asignación no es válida.</span><span class="sxs-lookup"><span data-stu-id="965c4-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="965c4-247">Por ejemplo, si un elemento MappingFragment contiene uno o varios elementos secundarios **Condition** , la asignación definida dentro del nodo **MappingFragment** solo será válida si se cumplen todas las condiciones de los elementos de **condición** secundarios.</span><span class="sxs-lookup"><span data-stu-id="965c4-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="965c4-248">Cada condición se puede aplicar a un **nombre** (el nombre de una propiedad de entidad del modelo conceptual, especificado por el atributo de **nombre** ) o un **columnName** (el nombre de una columna de la base de datos, especificado por el atributo **columnName** ).</span><span class="sxs-lookup"><span data-stu-id="965c4-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="965c4-249">Cuando se establece el atributo **Name** , la condición se comprueba con respecto a un valor de propiedad de entidad.</span><span class="sxs-lookup"><span data-stu-id="965c4-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="965c4-250">Cuando se establece el atributo **columnName** , la condición se comprueba con respecto a un valor de columna.</span><span class="sxs-lookup"><span data-stu-id="965c4-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="965c4-251">Solo se puede especificar uno de los atributos **Name** o **columnName** en un elemento **Condition** .</span><span class="sxs-lookup"><span data-stu-id="965c4-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="965c4-252">Cuando el elemento **Condition** se usa dentro de un elemento FunctionImportMapping, solo el atributo **Name** no es aplicable.</span><span class="sxs-lookup"><span data-stu-id="965c4-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="965c4-253">El elemento **Condition** puede ser un elemento secundario de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="965c4-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="965c4-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="965c4-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="965c4-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="965c4-255">ComplexProperty</span></span>
-   <span data-ttu-id="965c4-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="965c4-256">EntitySetMapping</span></span>
-   <span data-ttu-id="965c4-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="965c4-257">MappingFragment</span></span>
-   <span data-ttu-id="965c4-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="965c4-258">EntityTypeMapping</span></span>

<span data-ttu-id="965c4-259">El elemento **Condition** no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="965c4-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-260">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-260">Applicable Attributes</span></span>

<span data-ttu-id="965c4-261">En la tabla siguiente se describen los atributos aplicables al elemento **Condition** :</span><span class="sxs-lookup"><span data-stu-id="965c4-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="965c4-262">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-262">Attribute Name</span></span> | <span data-ttu-id="965c4-263">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-263">Is Required</span></span> | <span data-ttu-id="965c4-264">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="965c4-265">**ColumnName**</span></span> | <span data-ttu-id="965c4-266">No</span><span class="sxs-lookup"><span data-stu-id="965c4-266">No</span></span>          | <span data-ttu-id="965c4-267">El nombre de la columna de la tabla cuyo valor se utiliza para evaluar la condición.</span><span class="sxs-lookup"><span data-stu-id="965c4-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="965c4-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="965c4-268">**IsNull**</span></span>     | <span data-ttu-id="965c4-269">No</span><span class="sxs-lookup"><span data-stu-id="965c4-269">No</span></span>          | <span data-ttu-id="965c4-270">**True** o **false**.</span><span class="sxs-lookup"><span data-stu-id="965c4-270">**True** or **False**.</span></span> <span data-ttu-id="965c4-271">Si el valor es **true** y el valor de la columna es **null**, o si el valor es **false** y el valor de la columna no es **null**, la condición es true.</span><span class="sxs-lookup"><span data-stu-id="965c4-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="965c4-272">De lo contrario, la condición es falsa.</span><span class="sxs-lookup"><span data-stu-id="965c4-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="965c4-273">Los atributos **IsNull** y **Value** no se pueden usar al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="965c4-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="965c4-274">**Valor**</span><span class="sxs-lookup"><span data-stu-id="965c4-274">**Value**</span></span>      | <span data-ttu-id="965c4-275">No</span><span class="sxs-lookup"><span data-stu-id="965c4-275">No</span></span>          | <span data-ttu-id="965c4-276">El valor con el que se compara el valor de la columna.</span><span class="sxs-lookup"><span data-stu-id="965c4-276">The value with which the column value is compared.</span></span> <span data-ttu-id="965c4-277">Si los valores son los mismos, la condición es verdadera.</span><span class="sxs-lookup"><span data-stu-id="965c4-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="965c4-278">De lo contrario, la condición es falsa.</span><span class="sxs-lookup"><span data-stu-id="965c4-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="965c4-279">Los atributos **IsNull** y **Value** no se pueden usar al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="965c4-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="965c4-280">**Name**</span><span class="sxs-lookup"><span data-stu-id="965c4-280">**Name**</span></span>       | <span data-ttu-id="965c4-281">No</span><span class="sxs-lookup"><span data-stu-id="965c4-281">No</span></span>          | <span data-ttu-id="965c4-282">El nombre de la propiedad de entidad del modelo conceptual cuyo valor se utiliza para evaluar la condición.</span><span class="sxs-lookup"><span data-stu-id="965c4-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="965c4-283">Este atributo no es aplicable si el elemento **Condition** se usa dentro de un elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="965c4-284">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-284">Example</span></span>

<span data-ttu-id="965c4-285">En el ejemplo siguiente se muestran los elementos **Condition** como elementos secundarios de los elementos **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="965c4-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="965c4-286">Cuando **HireDate** no es NULL y **EnrollmentDate** es null, los datos se asignan entre el tipo **SchoolModel. instructor** y las columnas **PersonID** e **HireDate** de la tabla **Person** .</span><span class="sxs-lookup"><span data-stu-id="965c4-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="965c4-287">Cuando **EnrollmentDate** no es NULL y **HireDate** es null, los datos se asignan entre el tipo **SchoolModel. Student** y las columnas **PersonID** e **Enrollment** de la tabla **Person** .</span><span class="sxs-lookup"><span data-stu-id="965c4-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a><span data-ttu-id="965c4-288">DeleteFunction (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="965c4-289">El elemento **InsertFunction** del lenguaje de especificación de asignaciones (MSL) asigna la función de eliminación de un tipo de entidad o asociación del modelo conceptual a un procedimiento almacenado en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="965c4-290">Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="965c4-291">Para obtener más información, vea elemento function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="965c4-292">Si no asigna las tres operaciones de inserción, actualización o eliminación de un tipo de entidad a los procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecutan en tiempo de ejecución y se genera un UpdateException.</span><span class="sxs-lookup"><span data-stu-id="965c4-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="965c4-293">DeleteFunction aplicado a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="965c4-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="965c4-294">Cuando se aplica al elemento EntityTypeMapping, el elemento **DeleteFunction** asigna la función de eliminación de un tipo de entidad del modelo conceptual a un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="965c4-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="965c4-295">El elemento **DeleteFunction** puede tener los elementos secundarios siguientes cuando se aplica a un elemento **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="965c4-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="965c4-296">AssociationEnd (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="965c4-297">ComplexProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="965c4-298">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="965c4-299">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-299">Applicable Attributes</span></span>

<span data-ttu-id="965c4-300">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **DeleteFunction** cuando se aplica a un elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="965c4-301">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-301">Attribute Name</span></span>            | <span data-ttu-id="965c4-302">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-302">Is Required</span></span> | <span data-ttu-id="965c4-303">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-304">**Nombrefunción**</span><span class="sxs-lookup"><span data-stu-id="965c4-304">**FunctionName**</span></span>          | <span data-ttu-id="965c4-305">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-305">Yes</span></span>         | <span data-ttu-id="965c4-306">El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de eliminación está asignada.</span><span class="sxs-lookup"><span data-stu-id="965c4-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="965c4-307">El procedimiento almacenado se debe declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="965c4-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="965c4-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="965c4-309">No</span><span class="sxs-lookup"><span data-stu-id="965c4-309">No</span></span>          | <span data-ttu-id="965c4-310">El nombre del parámetro de salida que devuelve el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="965c4-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="965c4-311">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-311">Example</span></span>

<span data-ttu-id="965c4-312">El ejemplo siguiente se basa en el modelo School y muestra el elemento **DeleteFunction** que asigna la función Delete del tipo de entidad **Person** al procedimiento almacenado **DeletePerson** .</span><span class="sxs-lookup"><span data-stu-id="965c4-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="965c4-313">El procedimiento almacenado **DeletePerson** se declara en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="965c4-314">DeleteFunction aplicado a AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="965c4-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="965c4-315">Cuando se aplica al elemento AssociationSetMapping, el elemento **DeleteFunction** asigna la función de eliminación de una asociación del modelo conceptual a un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="965c4-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="965c4-316">El elemento **DeleteFunction** puede tener los elementos secundarios siguientes cuando se aplica al elemento **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="965c4-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="965c4-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="965c4-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="965c4-318">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-318">Applicable Attributes</span></span>

<span data-ttu-id="965c4-319">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **DeleteFunction** cuando se aplica al elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="965c4-320">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-320">Attribute Name</span></span>            | <span data-ttu-id="965c4-321">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-321">Is Required</span></span> | <span data-ttu-id="965c4-322">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-323">**Nombrefunción**</span><span class="sxs-lookup"><span data-stu-id="965c4-323">**FunctionName**</span></span>          | <span data-ttu-id="965c4-324">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-324">Yes</span></span>         | <span data-ttu-id="965c4-325">El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de eliminación está asignada.</span><span class="sxs-lookup"><span data-stu-id="965c4-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="965c4-326">El procedimiento almacenado se debe declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="965c4-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="965c4-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="965c4-328">No</span><span class="sxs-lookup"><span data-stu-id="965c4-328">No</span></span>          | <span data-ttu-id="965c4-329">El nombre del parámetro de salida que devuelve el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="965c4-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="965c4-330">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-330">Example</span></span>

<span data-ttu-id="965c4-331">El ejemplo siguiente se basa en el modelo School y muestra el elemento **DeleteFunction** que se usa para asignar la función de eliminación de la Asociación **CourseInstructor** al procedimiento almacenado **DeleteCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="965c4-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="965c4-332">El procedimiento almacenado **DeleteCourseInstructor** se declara en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a><span data-ttu-id="965c4-333">EndProperty (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="965c4-334">El elemento **EndProperty** del lenguaje de especificación de asignaciones (MSL) define la asignación entre un extremo o una función de modificación de una asociación del modelo conceptual y la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="965c4-335">La asignación de columna de propiedades se especifica en un elemento de ScalarProperty secundario.</span><span class="sxs-lookup"><span data-stu-id="965c4-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="965c4-336">Cuando se utiliza un elemento **EndProperty** para definir la asignación para el final de una asociación del modelo conceptual, es un elemento secundario de un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="965c4-337">Cuando el elemento **EndProperty** se utiliza para definir la asignación para una función de modificación de una asociación del modelo conceptual, es un elemento secundario de un elemento InsertFunction o un elemento InsertFunction.</span><span class="sxs-lookup"><span data-stu-id="965c4-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="965c4-338">El elemento **EndProperty** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="965c4-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="965c4-339">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-340">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-340">Applicable Attributes</span></span>

<span data-ttu-id="965c4-341">En la tabla siguiente se describen los atributos aplicables al elemento **EndProperty** :</span><span class="sxs-lookup"><span data-stu-id="965c4-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="965c4-342">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-342">Attribute Name</span></span> | <span data-ttu-id="965c4-343">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-343">Is Required</span></span> | <span data-ttu-id="965c4-344">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="965c4-345">Name</span><span class="sxs-lookup"><span data-stu-id="965c4-345">Name</span></span>           | <span data-ttu-id="965c4-346">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-346">Yes</span></span>         | <span data-ttu-id="965c4-347">El nombre del extremo de la asociación que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="965c4-348">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-348">Example</span></span>

<span data-ttu-id="965c4-349">En el ejemplo siguiente se muestra un elemento **AssociationSetMapping** en el que la Asociación **FK @ no__t-2Course @ no__t-3Department** del modelo conceptual se asigna a la tabla **Course** en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="965c4-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="965c4-350">Las asignaciones entre las propiedades de tipo de asociación y las columnas de tabla se especifican en los elementos secundarios **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="965c4-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a><span data-ttu-id="965c4-351">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-351">Example</span></span>

<span data-ttu-id="965c4-352">En el ejemplo siguiente se muestra el elemento **EndProperty** que asigna las funciones de inserción y eliminación de una asociación (**CourseInstructor**) a los procedimientos almacenados en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="965c4-353">Las funciones asignadas se declaran en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-353">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="965c4-354">EntityContainerMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="965c4-355">El elemento **EntityContainerMapping** del lenguaje de especificación de asignaciones (MSL) asigna el contenedor de entidades del modelo conceptual al contenedor de entidades en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="965c4-356">El elemento **EntityContainerMapping** es un elemento secundario del elemento Mapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="965c4-357">El elemento **EntityContainerMapping** puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="965c4-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="965c4-358">EntitySetMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="965c4-359">AssociationSetMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="965c4-360">FunctionImportMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-361">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-361">Applicable Attributes</span></span>

<span data-ttu-id="965c4-362">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **EntityContainerMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="965c4-363">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-363">Attribute Name</span></span>            | <span data-ttu-id="965c4-364">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-364">Is Required</span></span> | <span data-ttu-id="965c4-365">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="965c4-366">**StorageModelContainer**</span></span> | <span data-ttu-id="965c4-367">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-367">Yes</span></span>         | <span data-ttu-id="965c4-368">El nombre del contenedor de entidades del modelo de almacenamiento que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="965c4-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="965c4-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="965c4-370">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-370">Yes</span></span>         | <span data-ttu-id="965c4-371">El nombre del contenedor de entidades del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="965c4-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="965c4-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="965c4-373">No</span><span class="sxs-lookup"><span data-stu-id="965c4-373">No</span></span>          | <span data-ttu-id="965c4-374">**True** o **false**.</span><span class="sxs-lookup"><span data-stu-id="965c4-374">**True** or **False**.</span></span> <span data-ttu-id="965c4-375">Si **es false**, no se genera ninguna vista de actualización.</span><span class="sxs-lookup"><span data-stu-id="965c4-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="965c4-376">Este atributo debe establecerse en **false** si tiene una asignación de solo lectura que no sería válida porque los datos no pueden realizar una operación de ida y vuelta correctamente.</span><span class="sxs-lookup"><span data-stu-id="965c4-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="965c4-377">El valor predeterminado es **true**.</span><span class="sxs-lookup"><span data-stu-id="965c4-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="965c4-378">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-378">Example</span></span>

<span data-ttu-id="965c4-379">En el ejemplo siguiente se muestra un elemento **EntityContainerMapping** que asigna el contenedor **SchoolModelEntities** (el contenedor de entidades del modelo conceptual) al contenedor **SchoolModelStoreContainer** (la entidad del modelo de almacenamiento contenedor):</span><span class="sxs-lookup"><span data-stu-id="965c4-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="965c4-380">EntitySetMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="965c4-381">El elemento **EntitySetMapping** del lenguaje de especificación de asignaciones (MSL) asigna todos los tipos de un conjunto de entidades del modelo conceptual a los conjuntos de entidades del modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="965c4-382">Un conjunto de entidades en el modelo conceptual es un contenedor lógico para instancias de entidades del mismo tipo (y tipos derivados).</span><span class="sxs-lookup"><span data-stu-id="965c4-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="965c4-383">Un conjunto de entidades en el modelo de almacenamiento representa una tabla o una vista en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="965c4-384">El conjunto de entidades del modelo conceptual se especifica mediante el valor del atributo **Name** del elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="965c4-385">La tabla o vista asignada a se especifica mediante el atributo **StoreEntitySet** en cada elemento MappingFragment secundario o en el propio elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="965c4-386">El elemento **EntitySetMapping** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="965c4-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="965c4-387">EntityTypeMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="965c4-388">QueryView (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="965c4-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="965c4-389">MappingFragment (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-390">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-390">Applicable Attributes</span></span>

<span data-ttu-id="965c4-391">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="965c4-392">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-392">Attribute Name</span></span>           | <span data-ttu-id="965c4-393">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-393">Is Required</span></span> | <span data-ttu-id="965c4-394">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-395">**Name**</span><span class="sxs-lookup"><span data-stu-id="965c4-395">**Name**</span></span>                 | <span data-ttu-id="965c4-396">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-396">Yes</span></span>         | <span data-ttu-id="965c4-397">El nombre del conjunto de entidades del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="965c4-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="965c4-398">**TypeName** **1**</span></span>       | <span data-ttu-id="965c4-399">No</span><span class="sxs-lookup"><span data-stu-id="965c4-399">No</span></span>          | <span data-ttu-id="965c4-400">El nombre del tipo de entidad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="965c4-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="965c4-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="965c4-402">No</span><span class="sxs-lookup"><span data-stu-id="965c4-402">No</span></span>          | <span data-ttu-id="965c4-403">El nombre del conjunto de entidades del modelo de almacenamiento al que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="965c4-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="965c4-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="965c4-405">No</span><span class="sxs-lookup"><span data-stu-id="965c4-405">No</span></span>          | <span data-ttu-id="965c4-406">**True** o **false** , dependiendo de si solo se devuelven filas distintas.</span><span class="sxs-lookup"><span data-stu-id="965c4-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="965c4-407">Si este atributo se establece en **true**, el atributo **GenerateUpdateViews** del elemento EntityContainerMapping debe establecerse en **false**.</span><span class="sxs-lookup"><span data-stu-id="965c4-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="965c4-408">**1** los atributos **TypeName** y **StoreEntitySet** se pueden usar en lugar de los elementos secundarios EntityTypeMapping y MappingFragment para asignar un tipo de entidad único a una sola tabla.</span><span class="sxs-lookup"><span data-stu-id="965c4-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="965c4-409">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-409">Example</span></span>

<span data-ttu-id="965c4-410">En el ejemplo siguiente se muestra un elemento **EntitySetMapping** que asigna tres tipos (un tipo base y dos tipos derivados) en el conjunto de entidades **Courses** del modelo conceptual a tres tablas diferentes en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="965c4-411">Las tablas se especifican mediante el atributo **StoreEntitySet** en cada elemento **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="965c4-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="965c4-412">EntityTypeMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="965c4-413">El elemento **EntityTypeMapping** del lenguaje de especificación de asignaciones (MSL) define la asignación entre un tipo de entidad en el modelo conceptual y las tablas o vistas de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="965c4-414">Para obtener información sobre los tipos de entidad del modelo conceptual y las tablas o vistas de base de datos subyacentes, vea EntityType (elemento) (CSDL) y EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="965c4-415">El tipo de entidad del modelo conceptual que se está asignando se especifica mediante el atributo **TypeName** del elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="965c4-416">La tabla o vista que se está asignando se especifica mediante el atributo **StoreEntitySet** del elemento MappingFragment secundario.</span><span class="sxs-lookup"><span data-stu-id="965c4-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="965c4-417">El elemento secundario ModificationFunctionMapping se puede usar para asignar las funciones de inserción, actualización o eliminación de tipos de entidad a procedimientos almacenados en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="965c4-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="965c4-418">El elemento **EntityTypeMapping** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="965c4-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="965c4-419">MappingFragment (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="965c4-420">ModificationFunctionMapping (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="965c4-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="965c4-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="965c4-421">ScalarProperty</span></span>
-   <span data-ttu-id="965c4-422">Condición</span><span class="sxs-lookup"><span data-stu-id="965c4-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="965c4-423">Los elementos **MappingFragment** y **ModificationFunctionMapping** no pueden ser elementos secundarios del elemento **EntityTypeMapping** al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="965c4-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="965c4-424">Los elementos **ScalarProperty** y **Condition** solo pueden ser elementos secundarios del elemento **EntityTypeMapping** cuando se usa dentro de un elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-425">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-425">Applicable Attributes</span></span>

<span data-ttu-id="965c4-426">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="965c4-427">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-427">Attribute Name</span></span> | <span data-ttu-id="965c4-428">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-428">Is Required</span></span> | <span data-ttu-id="965c4-429">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-430">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="965c4-430">**TypeName**</span></span>   | <span data-ttu-id="965c4-431">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-431">Yes</span></span>         | <span data-ttu-id="965c4-432">El nombre completo, calificado con el espacio de nombres, del tipo de entidad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="965c4-433">Si el tipo es abstracto o un tipo derivado, el valor debe ser `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="965c4-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="965c4-434">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-434">Example</span></span>

<span data-ttu-id="965c4-435">En el ejemplo siguiente se muestra un elemento EntitySetMapping con dos elementos **EntityTypeMapping** secundarios.</span><span class="sxs-lookup"><span data-stu-id="965c4-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="965c4-436">En el primer elemento **EntityTypeMapping** , el tipo de entidad **SchoolModel. person** se asigna a la tabla **Person** .</span><span class="sxs-lookup"><span data-stu-id="965c4-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="965c4-437">En el segundo elemento **EntityTypeMapping** , la funcionalidad de actualización del tipo **SchoolModel. person** se asigna a un procedimiento almacenado, **UpdatePerson**, en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="965c4-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="965c4-438">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-438">Example</span></span>

<span data-ttu-id="965c4-439">En el ejemplo siguiente se muestra la asignación de una jerarquía de tipos en la que el tipo raíz es abstracto.</span><span class="sxs-lookup"><span data-stu-id="965c4-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="965c4-440">Tenga en cuenta el uso de la sintaxis `IsOfType` para los atributos **TypeName** .</span><span class="sxs-lookup"><span data-stu-id="965c4-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="965c4-441">FunctionImportMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="965c4-442">El elemento **FunctionImportMapping** del lenguaje de especificación de asignaciones (MSL) define la asignación entre una importación de función en el modelo conceptual y un procedimiento almacenado o una función en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="965c4-443">Las importaciones de función se deben declarar en el modelo conceptual, mientras que los procedimientos almacenados se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="965c4-444">Para obtener más información, vea elemento FunctionImport (CSDL) y elemento function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="965c4-445">De forma predeterminada, si una importación de función devuelve un tipo de entidad del modelo conceptual o un tipo complejo, entonces los nombres de las columnas devueltas por el procedimiento almacenado subyacente deben coincidir exactamente con los nombres de las propiedades del tipo del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="965c4-446">Si los nombres de columna no coinciden exactamente con los nombres de propiedad, la asignación se debe definir en un elemento ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="965c4-447">El elemento **FunctionImportMapping** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="965c4-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="965c4-448">ResultMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-449">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-449">Applicable Attributes</span></span>

<span data-ttu-id="965c4-450">En la tabla siguiente se describen los atributos aplicables al elemento **FunctionImportMapping** :</span><span class="sxs-lookup"><span data-stu-id="965c4-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="965c4-451">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-451">Attribute Name</span></span>         | <span data-ttu-id="965c4-452">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-452">Is Required</span></span> | <span data-ttu-id="965c4-453">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="965c4-454">**FunctionImportName**</span></span> | <span data-ttu-id="965c4-455">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-455">Yes</span></span>         | <span data-ttu-id="965c4-456">El nombre de la importación de función del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="965c4-457">**Nombrefunción**</span><span class="sxs-lookup"><span data-stu-id="965c4-457">**FunctionName**</span></span>       | <span data-ttu-id="965c4-458">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-458">Yes</span></span>         | <span data-ttu-id="965c4-459">El nombre, calificado con el espacio de nombres, de la función del modelo de almacenamiento que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="965c4-460">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-460">Example</span></span>

<span data-ttu-id="965c4-461">El ejemplo siguiente se basa en el modelo School.</span><span class="sxs-lookup"><span data-stu-id="965c4-461">The following example is based on the School model.</span></span> <span data-ttu-id="965c4-462">Considere la siguiente función en el modelo de almacenamiento:</span><span class="sxs-lookup"><span data-stu-id="965c4-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="965c4-463">También considere esta importación de función en el modelo conceptual:</span><span class="sxs-lookup"><span data-stu-id="965c4-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="965c4-464">En el ejemplo siguiente se muestra un elemento **FunctionImportMapping** que se usa para asignar la función y la importación de funciones anteriores entre sí:</span><span class="sxs-lookup"><span data-stu-id="965c4-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="965c4-465">InsertFunction (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="965c4-466">El elemento **InsertFunction** del lenguaje de especificación de asignaciones (MSL) asigna la función de inserción de un tipo de entidad o asociación del modelo conceptual a un procedimiento almacenado en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="965c4-467">Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="965c4-468">Para obtener más información, vea elemento function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="965c4-469">Si no asigna las tres operaciones de inserción, actualización o eliminación de un tipo de entidad a los procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecutan en tiempo de ejecución y se genera un UpdateException.</span><span class="sxs-lookup"><span data-stu-id="965c4-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="965c4-470">El elemento **InsertFunction** puede ser un elemento secundario del elemento ModificationFunctionMapping y aplicarse al elemento EntityTypeMapping o al elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="965c4-471">InsertFunction aplicado a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="965c4-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="965c4-472">Cuando se aplica al elemento EntityTypeMapping, el elemento **InsertFunction** asigna la función de inserción de un tipo de entidad del modelo conceptual a un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="965c4-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="965c4-473">El elemento **InsertFunction** puede tener los elementos secundarios siguientes cuando se aplica a un elemento **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="965c4-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="965c4-474">AssociationEnd (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="965c4-475">ComplexProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="965c4-476">ResultBinding (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="965c4-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="965c4-477">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="965c4-478">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-478">Applicable Attributes</span></span>

<span data-ttu-id="965c4-479">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **InsertFunction** cuando se aplican a un elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="965c4-480">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-480">Attribute Name</span></span>            | <span data-ttu-id="965c4-481">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-481">Is Required</span></span> | <span data-ttu-id="965c4-482">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-483">**Nombrefunción**</span><span class="sxs-lookup"><span data-stu-id="965c4-483">**FunctionName**</span></span>          | <span data-ttu-id="965c4-484">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-484">Yes</span></span>         | <span data-ttu-id="965c4-485">El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de inserción está asignada.</span><span class="sxs-lookup"><span data-stu-id="965c4-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="965c4-486">El procedimiento almacenado se debe declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="965c4-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="965c4-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="965c4-488">No</span><span class="sxs-lookup"><span data-stu-id="965c4-488">No</span></span>          | <span data-ttu-id="965c4-489">El nombre del parámetro de salida que devuelve el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="965c4-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="965c4-490">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-490">Example</span></span>

<span data-ttu-id="965c4-491">El ejemplo siguiente se basa en el modelo School y muestra el elemento **InsertFunction** que se usa para asignar la función de inserción del tipo de entidad person al procedimiento almacenado **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="965c4-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="965c4-492">El procedimiento almacenado **InsertPerson** se declara en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="965c4-493">InsertFunction aplicado a AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="965c4-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="965c4-494">Cuando se aplica al elemento AssociationSetMapping, el elemento **InsertFunction** asigna la función de inserción de una asociación del modelo conceptual a un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="965c4-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="965c4-495">El elemento **InsertFunction** puede tener los elementos secundarios siguientes cuando se aplica al elemento **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="965c4-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="965c4-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="965c4-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="965c4-497">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-497">Applicable Attributes</span></span>

<span data-ttu-id="965c4-498">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **InsertFunction** cuando se aplica al elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="965c4-499">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-499">Attribute Name</span></span>            | <span data-ttu-id="965c4-500">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-500">Is Required</span></span> | <span data-ttu-id="965c4-501">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-502">**Nombrefunción**</span><span class="sxs-lookup"><span data-stu-id="965c4-502">**FunctionName**</span></span>          | <span data-ttu-id="965c4-503">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-503">Yes</span></span>         | <span data-ttu-id="965c4-504">El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de inserción está asignada.</span><span class="sxs-lookup"><span data-stu-id="965c4-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="965c4-505">El procedimiento almacenado se debe declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="965c4-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="965c4-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="965c4-507">No</span><span class="sxs-lookup"><span data-stu-id="965c4-507">No</span></span>          | <span data-ttu-id="965c4-508">El nombre del parámetro de salida que devuelve el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="965c4-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="965c4-509">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-509">Example</span></span>

<span data-ttu-id="965c4-510">El ejemplo siguiente se basa en el modelo School y muestra el elemento **InsertFunction** que se usa para asignar la función de inserción de la Asociación **CourseInstructor** al procedimiento almacenado **InsertCourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="965c4-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="965c4-511">El procedimiento almacenado **InsertCourseInstructor** se declara en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a><span data-ttu-id="965c4-512">Mapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="965c4-513">El elemento **mapping** del lenguaje de especificación de asignaciones (MSL) contiene información para la asignación de objetos que se definen en un modelo conceptual a una base de datos (tal y como se describe en un modelo de almacenamiento).</span><span class="sxs-lookup"><span data-stu-id="965c4-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="965c4-514">Para obtener más información, vea especificación de CSDL y especificación de SSDL.</span><span class="sxs-lookup"><span data-stu-id="965c4-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="965c4-515">El elemento **mapping** es el elemento raíz de una especificación de asignación.</span><span class="sxs-lookup"><span data-stu-id="965c4-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="965c4-516">El espacio de nombres XML para la asignación de especificaciones es https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="965c4-516">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="965c4-517">El elemento de asignación puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="965c4-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="965c4-518">Alias (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="965c4-519">EntityContainerMapping (exactamente uno)</span><span class="sxs-lookup"><span data-stu-id="965c4-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="965c4-520">Los nombres de los tipos de modelos conceptuales y de almacenamiento a los que se hace referencia en MSL deben estar calificados con sus respectivos nombres de espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="965c4-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="965c4-521">Para obtener información sobre el nombre del espacio de nombres del modelo conceptual, vea elemento Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="965c4-522">Para obtener información sobre el nombre del espacio de nombres del modelo de almacenamiento, vea schema (elemento) (SSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="965c4-523">Los alias para los espacios de nombres que se usan en MSL se pueden definir con el elemento alias.</span><span class="sxs-lookup"><span data-stu-id="965c4-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-524">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-524">Applicable Attributes</span></span>

<span data-ttu-id="965c4-525">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento de **asignación** .</span><span class="sxs-lookup"><span data-stu-id="965c4-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="965c4-526">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-526">Attribute Name</span></span> | <span data-ttu-id="965c4-527">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-527">Is Required</span></span> | <span data-ttu-id="965c4-528">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="965c4-529">**Espacio**</span><span class="sxs-lookup"><span data-stu-id="965c4-529">**Space**</span></span>      | <span data-ttu-id="965c4-530">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-530">Yes</span></span>         | <span data-ttu-id="965c4-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="965c4-531">**C-S**.</span></span> <span data-ttu-id="965c4-532">Este es un valor fijo y no se puede cambiar.</span><span class="sxs-lookup"><span data-stu-id="965c4-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="965c4-533">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-533">Example</span></span>

<span data-ttu-id="965c4-534">En el ejemplo siguiente se muestra un elemento de **asignación** basado en parte del modelo School.</span><span class="sxs-lookup"><span data-stu-id="965c4-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="965c4-535">Para obtener más información sobre el modelo School, consulte Inicio rápido (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="965c4-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="965c4-536">MappingFragment (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="965c4-537">El elemento **MappingFragment** del lenguaje de especificación de asignaciones (MSL) define la asignación entre las propiedades de un tipo de entidad del modelo conceptual y una tabla o vista en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="965c4-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="965c4-538">Para obtener información sobre los tipos de entidad del modelo conceptual y las tablas o vistas de base de datos subyacentes, vea EntityType (elemento) (CSDL) y EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="965c4-539">El **MappingFragment** puede ser un elemento secundario del elemento EntityTypeMapping o el elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="965c4-540">El elemento **MappingFragment** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="965c4-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="965c4-541">ComplexType (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="965c4-542">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="965c4-543">Condición (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-544">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-544">Applicable Attributes</span></span>

<span data-ttu-id="965c4-545">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="965c4-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="965c4-546">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-546">Attribute Name</span></span>          | <span data-ttu-id="965c4-547">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-547">Is Required</span></span> | <span data-ttu-id="965c4-548">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="965c4-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="965c4-550">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-550">Yes</span></span>         | <span data-ttu-id="965c4-551">El nombre de la tabla o vista que se está asociando.</span><span class="sxs-lookup"><span data-stu-id="965c4-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="965c4-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="965c4-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="965c4-553">No</span><span class="sxs-lookup"><span data-stu-id="965c4-553">No</span></span>          | <span data-ttu-id="965c4-554">**True** o **false** , dependiendo de si solo se devuelven filas distintas.</span><span class="sxs-lookup"><span data-stu-id="965c4-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="965c4-555">Si este atributo se establece en **true**, el atributo **GenerateUpdateViews** del elemento EntityContainerMapping debe establecerse en **false**.</span><span class="sxs-lookup"><span data-stu-id="965c4-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="965c4-556">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-556">Example</span></span>

<span data-ttu-id="965c4-557">En el ejemplo siguiente se muestra un elemento **MappingFragment** como elemento secundario de un elemento **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="965c4-558">En este ejemplo, las propiedades del tipo de **curso** en el modelo conceptual se asignan a las columnas de la tabla **Course** en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="965c4-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="965c4-559">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-559">Example</span></span>

<span data-ttu-id="965c4-560">En el ejemplo siguiente se muestra un elemento **MappingFragment** como el elemento secundario de un elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="965c4-561">Como en el ejemplo anterior, las propiedades del tipo de **curso** en el modelo conceptual se asignan a las columnas de la tabla **Course** en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="965c4-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="965c4-562">ModificationFunctionMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="965c4-563">El elemento **ModificationFunctionMapping** del lenguaje de especificación de asignaciones (MSL) asigna las funciones de inserción, actualización y eliminación de un tipo de entidad del modelo conceptual a los procedimientos almacenados en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="965c4-564">El elemento **ModificationFunctionMapping** también puede asignar las funciones de inserción y eliminación para las asociaciones de varios a varios del modelo conceptual a los procedimientos almacenados en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="965c4-565">Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="965c4-566">Para obtener más información, vea elemento function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="965c4-567">Si no asigna las tres operaciones de inserción, actualización o eliminación de un tipo de entidad a los procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecutan en tiempo de ejecución y se genera un UpdateException.</span><span class="sxs-lookup"><span data-stu-id="965c4-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="965c4-568">Si las funciones de modificación para una entidad de una jerarquía de herencia están asignadas a procedimientos almacenados, entonces las funciones de modificación para todos los tipos de la jerarquía deben estar asignadas a procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="965c4-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="965c4-569">El elemento **ModificationFunctionMapping** puede ser un elemento secundario del elemento EntityTypeMapping o el elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="965c4-570">El elemento **ModificationFunctionMapping** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="965c4-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="965c4-571">DeleteFunction (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="965c4-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="965c4-572">InsertFunction (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="965c4-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="965c4-573">UpdateFunction (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="965c4-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="965c4-574">No hay atributos aplicables al elemento **ModificationFunctionMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="965c4-575">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-575">Example</span></span>

<span data-ttu-id="965c4-576">En el ejemplo siguiente se muestra la asignación de conjunto de entidades para el conjunto de entidades **People** del modelo School.</span><span class="sxs-lookup"><span data-stu-id="965c4-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="965c4-577">Además de la asignación de columnas para el tipo de entidad **Person** , se muestra la asignación de las funciones INSERT, Update y DELETE del tipo **Person** .</span><span class="sxs-lookup"><span data-stu-id="965c4-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="965c4-578">Las funciones asignadas se declaran en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-578">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="965c4-579">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-579">Example</span></span>

<span data-ttu-id="965c4-580">En el ejemplo siguiente se muestra la asignación del conjunto de asociaciones para el conjunto de asociaciones **CourseInstructor** en el modelo School.</span><span class="sxs-lookup"><span data-stu-id="965c4-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="965c4-581">Además de la asignación de columnas para la Asociación **CourseInstructor** , se muestra la asignación de las funciones de inserción y eliminación de la Asociación **CourseInstructor** .</span><span class="sxs-lookup"><span data-stu-id="965c4-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="965c4-582">Las funciones asignadas se declaran en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-582">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="965c4-583">QueryView (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="965c4-584">El elemento **QueryView** del lenguaje de especificación de asignaciones (MSL) define una asignación de solo lectura entre un tipo de entidad o una asociación en el modelo conceptual y una tabla en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="965c4-585">La asignación se define con una consulta Entity SQL que se evalúa con el modelo de almacenamiento y se expresa el conjunto de resultados en términos de una entidad o asociación en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="965c4-586">Dado que las vistas de consulta son de solo lectura, no puede utilizar los comandos de actualización estándar para actualizar los tipos que se definen mediante vistas de consulta.</span><span class="sxs-lookup"><span data-stu-id="965c4-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="965c4-587">Puede realizar las actualizaciones de estos tipos utilizando funciones de modificación.</span><span class="sxs-lookup"><span data-stu-id="965c4-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="965c4-588">Para obtener más información, vea el artículo sobre cómo Asignar funciones de modificación a procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="965c4-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="965c4-589">En el elemento **QueryView** , no se admiten Entity SQL expresiones que contengan **GroupBy**, agregados de grupo o propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="965c4-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="965c4-590">El elemento **QueryView** puede ser un elemento secundario del elemento EntitySetMapping o del elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="965c4-591">En el primer caso, la vista de consulta define una asignación de solo lectura para una entidad del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="965c4-592">En el último caso, la vista de consulta define una asignación de solo lectura para una asociación del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="965c4-593">Si el elemento **AssociationSetMapping** es para una asociación con una restricción referencial, se omite el elemento **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="965c4-594">Para obtener más información, vea ReferentialConstraint (elemento) (CSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="965c4-595">El elemento **QueryView** no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="965c4-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-596">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-596">Applicable Attributes</span></span>

<span data-ttu-id="965c4-597">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **QueryView** .</span><span class="sxs-lookup"><span data-stu-id="965c4-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="965c4-598">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-598">Attribute Name</span></span> | <span data-ttu-id="965c4-599">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-599">Is Required</span></span> | <span data-ttu-id="965c4-600">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-601">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="965c4-601">**TypeName**</span></span>   | <span data-ttu-id="965c4-602">No</span><span class="sxs-lookup"><span data-stu-id="965c4-602">No</span></span>          | <span data-ttu-id="965c4-603">El nombre del tipo de modelo conceptual que se está asignando mediante la vista de consulta.</span><span class="sxs-lookup"><span data-stu-id="965c4-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="965c4-604">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-604">Example</span></span>

<span data-ttu-id="965c4-605">En el ejemplo siguiente se muestra el elemento **QueryView** como elemento secundario del elemento **EntitySetMapping** y se define una asignación de vista de consulta para el tipo de entidad **Department** en el modelo School.</span><span class="sxs-lookup"><span data-stu-id="965c4-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

<span data-ttu-id="965c4-606">Dado que la consulta solo devuelve un subconjunto de los miembros del tipo **Department** en el modelo de almacenamiento, el tipo **Department** del modelo School se ha modificado según esta asignación de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="965c4-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a><span data-ttu-id="965c4-607">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-607">Example</span></span>

<span data-ttu-id="965c4-608">En el ejemplo siguiente se muestra el elemento **QueryView** como elemento secundario de un elemento **AssociationSetMapping** y se define una asignación de solo lectura para la Asociación `FK_Course_Department` del modelo School.</span><span class="sxs-lookup"><span data-stu-id="965c4-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a><span data-ttu-id="965c4-609">Comentarios</span><span class="sxs-lookup"><span data-stu-id="965c4-609">Comments</span></span>

<span data-ttu-id="965c4-610">Puede definir vistas de consulta para habilitar los escenarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="965c4-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="965c4-611">Defina una entidad en el modelo conceptual que no incluya todas las propiedades de la entidad en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="965c4-612">Esto incluye propiedades que no tienen valores predeterminados y no admiten valores **null** .</span><span class="sxs-lookup"><span data-stu-id="965c4-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="965c4-613">Asigne las columnas calculadas del modelo de almacenamiento a las propiedades de los tipos de entidad del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="965c4-614">Defina una asignación en la que las condiciones utilizadas para dividir las entidades del modelo conceptual no se basen en la igualdad.</span><span class="sxs-lookup"><span data-stu-id="965c4-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="965c4-615">Cuando se especifica una asignación condicional mediante el elemento **Condition** , la condición proporcionada debe ser igual al valor especificado.</span><span class="sxs-lookup"><span data-stu-id="965c4-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="965c4-616">Para obtener más información, vea condition (elemento) (MSL).</span><span class="sxs-lookup"><span data-stu-id="965c4-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="965c4-617">Asigne la misma columna en el modelo de almacenamiento a varios tipos en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="965c4-618">Asigne varios tipos a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="965c4-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="965c4-619">Defina asociaciones en el modelo conceptual que no se basen en claves externas en el esquema relacional.</span><span class="sxs-lookup"><span data-stu-id="965c4-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="965c4-620">Utilice la lógica de negocios personalizada para establecer el valor de las propiedades del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="965c4-621">Por ejemplo, podría asignar el valor de cadena "T" en el origen de datos a un valor **true**, un valor booleano, en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="965c4-622">Defina filtros condicionales para los resultados de la consulta.</span><span class="sxs-lookup"><span data-stu-id="965c4-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="965c4-623">Aplique menos restricciones en los datos del modelo conceptual que en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="965c4-624">Por ejemplo, podría hacer que una propiedad en el modelo conceptual acepte valores NULL incluso si la columna a la que está asignada no admite valores **null**.</span><span class="sxs-lookup"><span data-stu-id="965c4-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="965c4-625">Las consideraciones siguientes se aplican al definir las vistas de consulta para las entidades:</span><span class="sxs-lookup"><span data-stu-id="965c4-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="965c4-626">Las vistas de consulta son de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="965c4-626">Query views are read-only.</span></span> <span data-ttu-id="965c4-627">Solo puede realizar las actualizaciones a las entidades utilizando las funciones de modificación.</span><span class="sxs-lookup"><span data-stu-id="965c4-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="965c4-628">Al definir un tipo de entidad mediante una vista de consulta, también debe definir todas las entidades relacionadas mediante vistas de consulta.</span><span class="sxs-lookup"><span data-stu-id="965c4-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="965c4-629">Al asignar una asociación varios a varios a una entidad en el modelo de almacenamiento que representa una tabla de vínculos en el esquema relacional, debe definir un elemento **QueryView** en el elemento **AssociationSetMapping** para esta tabla de vínculos.</span><span class="sxs-lookup"><span data-stu-id="965c4-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="965c4-630">Las vistas de consulta se deben definir para todos los tipos de una jerarquía de tipos.</span><span class="sxs-lookup"><span data-stu-id="965c4-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="965c4-631">Puede hacer esto de las siguientes maneras:</span><span class="sxs-lookup"><span data-stu-id="965c4-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="965c4-632">Con un único elemento **QueryView** que especifica una única Entity SQL consulta que devuelve una Unión de todos los tipos de entidad de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="965c4-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="965c4-633">Con un único elemento **QueryView** que especifica una única Entity SQL consulta que usa el operador Case para devolver un tipo de entidad específico en la jerarquía basándose en una condición específica.</span><span class="sxs-lookup"><span data-stu-id="965c4-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="965c4-634">Con un elemento **QueryView** adicional para un tipo específico en la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="965c4-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="965c4-635">En este caso, use el atributo **TypeName** del elemento **QueryView** para especificar el tipo de entidad para cada vista.</span><span class="sxs-lookup"><span data-stu-id="965c4-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="965c4-636">Cuando se define una vista de consulta, no se puede especificar el atributo **StorageSetName** en el elemento **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="965c4-637">Cuando se define una vista de consulta, el elemento **EntitySetMapping**no puede contener también asignaciones de **propiedades** .</span><span class="sxs-lookup"><span data-stu-id="965c4-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="965c4-638">ResultBinding (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="965c4-639">El elemento **ResultBinding** del lenguaje de especificación de asignaciones (MSL) asigna los valores de columna devueltos por los procedimientos almacenados a las propiedades de entidad del modelo conceptual cuando las funciones de modificación de tipo de entidad se asignan a los procedimientos almacenados en el base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="965c4-640">Por ejemplo, cuando un procedimiento almacenado de inserción devuelve el valor de una columna de identidad, el elemento **ResultBinding** asigna el valor devuelto a una propiedad de tipo de entidad en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="965c4-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="965c4-641">El elemento **ResultBinding** puede ser secundario del elemento InsertFunction o del elemento UpdateFunction.</span><span class="sxs-lookup"><span data-stu-id="965c4-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="965c4-642">El elemento **ResultBinding** no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="965c4-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-643">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-643">Applicable Attributes</span></span>

<span data-ttu-id="965c4-644">En la tabla siguiente se describen los atributos aplicables al elemento **ResultBinding** :</span><span class="sxs-lookup"><span data-stu-id="965c4-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="965c4-645">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-645">Attribute Name</span></span> | <span data-ttu-id="965c4-646">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-646">Is Required</span></span> | <span data-ttu-id="965c4-647">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-648">**Name**</span><span class="sxs-lookup"><span data-stu-id="965c4-648">**Name**</span></span>       | <span data-ttu-id="965c4-649">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-649">Yes</span></span>         | <span data-ttu-id="965c4-650">El nombre de la propiedad de entidad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="965c4-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="965c4-651">**ColumnName**</span></span> | <span data-ttu-id="965c4-652">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-652">Yes</span></span>         | <span data-ttu-id="965c4-653">El nombre de la columna que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="965c4-654">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-654">Example</span></span>

<span data-ttu-id="965c4-655">El ejemplo siguiente se basa en el modelo School y muestra un elemento **InsertFunction** que se usa para asignar la función de inserción del tipo de entidad **Person** al procedimiento almacenado **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="965c4-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="965c4-656">(El procedimiento almacenado **InsertPerson** se muestra a continuación y se declara en el modelo de almacenamiento). Se usa un elemento **ResultBinding** para asignar un valor de columna devuelto por el procedimiento almacenado (**NewPersonID**) a una propiedad de tipo de entidad (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="965c4-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

<span data-ttu-id="965c4-657">En el siguiente código de Transact-SQL se describe el procedimiento almacenado **InsertPerson** :</span><span class="sxs-lookup"><span data-stu-id="965c4-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a><span data-ttu-id="965c4-658">ResultMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="965c4-659">El elemento **ResultMapping** del lenguaje de especificación de asignaciones (MSL) define la asignación entre una importación de función en el modelo conceptual y un procedimiento almacenado en la base de datos subyacente cuando se cumplen las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="965c4-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="965c4-660">La importación de función devuelve un tipo de entidad del modelo conceptual o un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="965c4-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="965c4-661">Los nombres de las columnas devueltas por el procedimiento almacenado no coinciden exactamente con los nombres de las propiedades del tipo de entidad o el tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="965c4-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="965c4-662">De forma predeterminada, la asignación entre las columnas devueltas por un procedimiento almacenado y un tipo de entidad o un tipo complejo se basa en los nombres de las propiedades y de las columnas.</span><span class="sxs-lookup"><span data-stu-id="965c4-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="965c4-663">Si los nombres de columna no coinciden exactamente con los nombres de propiedad, debe utilizar el elemento **ResultMapping** para definir la asignación.</span><span class="sxs-lookup"><span data-stu-id="965c4-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="965c4-664">Para obtener un ejemplo de la asignación predeterminada, vea FunctionImportMapping (elemento) (MSL).</span><span class="sxs-lookup"><span data-stu-id="965c4-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="965c4-665">El elemento **ResultMapping** es un elemento secundario del elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="965c4-666">El elemento **ResultMapping** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="965c4-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="965c4-667">EntityTypeMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="965c4-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="965c4-668">ComplexTypeMapping</span></span>

<span data-ttu-id="965c4-669">No hay atributos aplicables al elemento **ResultMapping** .</span><span class="sxs-lookup"><span data-stu-id="965c4-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="965c4-670">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-670">Example</span></span>

<span data-ttu-id="965c4-671">Considere el siguiente procedimiento almacenado:</span><span class="sxs-lookup"><span data-stu-id="965c4-671">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="965c4-672">Considere también el siguiente tipo de entidad del modelo conceptual:</span><span class="sxs-lookup"><span data-stu-id="965c4-672">Also consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

<span data-ttu-id="965c4-673">Para crear una importación de función que devuelva instancias del tipo de entidad anterior, la asignación entre las columnas devueltas por el procedimiento almacenado y el tipo de entidad debe definirse en un elemento **ResultMapping** :</span><span class="sxs-lookup"><span data-stu-id="965c4-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="965c4-674">ScalarProperty (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="965c4-675">El elemento **ScalarProperty** del lenguaje de especificación de asignaciones (MSL) asigna una propiedad en un tipo de entidad del modelo conceptual, un tipo complejo o una asociación a una columna de tabla o a un parámetro de procedimiento almacenado en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="965c4-676">Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="965c4-677">Para obtener más información, vea elemento function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="965c4-678">El elemento **ScalarProperty** puede ser un elemento secundario de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="965c4-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="965c4-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="965c4-679">MappingFragment</span></span>
-   <span data-ttu-id="965c4-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="965c4-680">InsertFunction</span></span>
-   <span data-ttu-id="965c4-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="965c4-681">UpdateFunction</span></span>
-   <span data-ttu-id="965c4-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="965c4-682">DeleteFunction</span></span>
-   <span data-ttu-id="965c4-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="965c4-683">EndProperty</span></span>
-   <span data-ttu-id="965c4-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="965c4-684">ComplexProperty</span></span>
-   <span data-ttu-id="965c4-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="965c4-685">ResultMapping</span></span>

<span data-ttu-id="965c4-686">Como elemento secundario del elemento **MappingFragment**, **ComplexProperty**o **EndProperty** , el elemento **ScalarProperty** asigna una propiedad del modelo conceptual a una columna de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="965c4-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="965c4-687">Como elemento secundario del elemento **InsertFunction**, **UpdateFunction**o **DeleteFunction** , el elemento **ScalarProperty** asigna una propiedad del modelo conceptual a un parámetro de procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="965c4-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="965c4-688">El elemento **ScalarProperty** no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="965c4-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-689">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-689">Applicable Attributes</span></span>

<span data-ttu-id="965c4-690">Los atributos que se aplican al elemento **ScalarProperty** difieren en función del rol del elemento.</span><span class="sxs-lookup"><span data-stu-id="965c4-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="965c4-691">En la tabla siguiente se describen los atributos que se pueden aplicar cuando el elemento **ScalarProperty** se utiliza para asignar una propiedad del modelo conceptual a una columna de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="965c4-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="965c4-692">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-692">Attribute Name</span></span> | <span data-ttu-id="965c4-693">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-693">Is Required</span></span> | <span data-ttu-id="965c4-694">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="965c4-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="965c4-695">**Name**</span></span>       | <span data-ttu-id="965c4-696">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-696">Yes</span></span>         | <span data-ttu-id="965c4-697">El nombre de la propiedad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="965c4-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="965c4-698">**ColumnName**</span></span> | <span data-ttu-id="965c4-699">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-699">Yes</span></span>         | <span data-ttu-id="965c4-700">El nombre de la columna de tabla que se está asociando.</span><span class="sxs-lookup"><span data-stu-id="965c4-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="965c4-701">En la tabla siguiente se describen los atributos aplicables al elemento **ScalarProperty** cuando se utiliza para asignar una propiedad del modelo conceptual a un parámetro de procedimiento almacenado:</span><span class="sxs-lookup"><span data-stu-id="965c4-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="965c4-702">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-702">Attribute Name</span></span>    | <span data-ttu-id="965c4-703">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-703">Is Required</span></span> | <span data-ttu-id="965c4-704">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-705">**Name**</span><span class="sxs-lookup"><span data-stu-id="965c4-705">**Name**</span></span>          | <span data-ttu-id="965c4-706">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-706">Yes</span></span>         | <span data-ttu-id="965c4-707">El nombre de la propiedad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="965c4-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="965c4-708">**ParameterName**</span></span> | <span data-ttu-id="965c4-709">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-709">Yes</span></span>         | <span data-ttu-id="965c4-710">El nombre del parámetro que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="965c4-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="965c4-711">**Versión**</span><span class="sxs-lookup"><span data-stu-id="965c4-711">**Version**</span></span>       | <span data-ttu-id="965c4-712">No</span><span class="sxs-lookup"><span data-stu-id="965c4-712">No</span></span>          | <span data-ttu-id="965c4-713">**Actual** o **original** , dependiendo de si el valor actual o el valor original de la propiedad se deben usar para las comprobaciones de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="965c4-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="965c4-714">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-714">Example</span></span>

<span data-ttu-id="965c4-715">En el ejemplo siguiente se muestra el elemento **ScalarProperty** utilizado de dos maneras:</span><span class="sxs-lookup"><span data-stu-id="965c4-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="965c4-716">Para asignar las propiedades del tipo de entidad **Person** a las columnas de la tabla **Person**.</span><span class="sxs-lookup"><span data-stu-id="965c4-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="965c4-717">Para asignar las propiedades del tipo de entidad **Person** a los parámetros del procedimiento almacenado **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="965c4-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="965c4-718">Los procedimientos almacenados se declaran en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-718">The stored procedures are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="965c4-719">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-719">Example</span></span>

<span data-ttu-id="965c4-720">En el ejemplo siguiente se muestra el elemento **ScalarProperty** que se usa para asignar las funciones de inserción y eliminación de una asociación del modelo conceptual a los procedimientos almacenados en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="965c4-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="965c4-721">Los procedimientos almacenados se declaran en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-721">The stored procedures are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a><span data-ttu-id="965c4-722">UpdateFunction (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="965c4-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="965c4-723">El elemento **UpdateFunction** del lenguaje de especificación de asignaciones (MSL) asigna la función de actualización de un tipo de entidad del modelo conceptual a un procedimiento almacenado en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="965c4-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="965c4-724">Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="965c4-725">Para obtener más información, vea elemento function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="965c4-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="965c4-726">Si no asigna las tres operaciones de inserción, actualización o eliminación de un tipo de entidad a los procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecutan en tiempo de ejecución y se genera un UpdateException.</span><span class="sxs-lookup"><span data-stu-id="965c4-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="965c4-727">El elemento **UpdateFunction** puede ser un elemento secundario del elemento ModificationFunctionMapping y aplicarse al elemento EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="965c4-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="965c4-728">El elemento **UpdateFunction** puede tener los siguientes elementos secundarios:</span><span class="sxs-lookup"><span data-stu-id="965c4-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="965c4-729">AssociationEnd (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="965c4-730">ComplexProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="965c4-731">ResultBinding (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="965c4-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="965c4-732">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="965c4-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="965c4-733">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="965c4-733">Applicable Attributes</span></span>

<span data-ttu-id="965c4-734">En la tabla siguiente se describen los atributos que se pueden aplicar al elemento **UpdateFunction** .</span><span class="sxs-lookup"><span data-stu-id="965c4-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="965c4-735">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="965c4-735">Attribute Name</span></span>            | <span data-ttu-id="965c4-736">Es necesario</span><span class="sxs-lookup"><span data-stu-id="965c4-736">Is Required</span></span> | <span data-ttu-id="965c4-737">Valor</span><span class="sxs-lookup"><span data-stu-id="965c4-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="965c4-738">**Nombrefunción**</span><span class="sxs-lookup"><span data-stu-id="965c4-738">**FunctionName**</span></span>          | <span data-ttu-id="965c4-739">Sí</span><span class="sxs-lookup"><span data-stu-id="965c4-739">Yes</span></span>         | <span data-ttu-id="965c4-740">El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de actualización está asignada.</span><span class="sxs-lookup"><span data-stu-id="965c4-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="965c4-741">El procedimiento almacenado se debe declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="965c4-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="965c4-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="965c4-743">No</span><span class="sxs-lookup"><span data-stu-id="965c4-743">No</span></span>          | <span data-ttu-id="965c4-744">El nombre del parámetro de salida que devuelve el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="965c4-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="965c4-745">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="965c4-745">Example</span></span>

<span data-ttu-id="965c4-746">El ejemplo siguiente se basa en el modelo School y muestra el elemento **UpdateFunction** que se usa para asignar la función de actualización del tipo de entidad **Person** al procedimiento almacenado **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="965c4-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="965c4-747">El procedimiento almacenado **UpdatePerson** se declara en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="965c4-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
