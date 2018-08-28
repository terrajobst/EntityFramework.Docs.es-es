---
title: Especificación de MSL - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 77dc7072c70b104188cd23974f32308960daebb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996036"
---
# <a name="msl-specification"></a><span data-ttu-id="15852-102">Especificación MSL</span><span class="sxs-lookup"><span data-stu-id="15852-102">MSL Specification</span></span>
<span data-ttu-id="15852-103">Lenguaje de especificación de asignaciones (MSL) es un lenguaje basado en XML que describe la asignación entre el modelo conceptual y el modelo de almacenamiento de una aplicación de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="15852-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="15852-104">En una aplicación de Entity Framework, los metadatos de asignación se cargan desde un archivo .msl (escrito en MSL) en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="15852-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="15852-105">Entity Framework usa los metadatos de asignación en tiempo de ejecución para traducir las consultas con el modelo conceptual en comandos específicos del almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="15852-106">El Diseñador de Entity Framework (EF Designer) almacena información de asignación en un archivo .edmx en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="15852-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="15852-107">En tiempo de compilación, Entity Designer usa información en un archivo .edmx para crear el archivo .msl que Entity Framework necesita en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="15852-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="15852-108">Los nombres de todos los tipos de modelos conceptuales o de almacenamiento a los que se hace referencia en MSL deben estar calificados con sus respectivos nombres de espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="15852-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="15852-109">Para obtener información sobre el nombre de espacio de nombres del modelo conceptual, vea [especificación de CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="15852-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="15852-110">Para obtener información sobre el nombre de espacio de nombres del modelo de almacenamiento, consulte [especificación de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="15852-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="15852-111">Las versiones de MSL se diferencian por espacios de nombres XML.</span><span class="sxs-lookup"><span data-stu-id="15852-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="15852-112">Versión MSL</span><span class="sxs-lookup"><span data-stu-id="15852-112">MSL Version</span></span> | <span data-ttu-id="15852-113">Namespace XML</span><span class="sxs-lookup"><span data-stu-id="15852-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="15852-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="15852-114">MSL v1</span></span>      | <span data-ttu-id="15852-115">urn: schemas-microsoft-cs</span><span class="sxs-lookup"><span data-stu-id="15852-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="15852-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="15852-116">MSL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| <span data-ttu-id="15852-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="15852-117">MSL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="15852-118">Alias (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-118">Alias Element (MSL)</span></span>

<span data-ttu-id="15852-119">El **Alias** en el lenguaje de especificación de (asignaciones MSL) es un elemento secundario del elemento de asignación que se utiliza para definir los alias de espacios de nombres modelo almacenamiento y el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="15852-120">Los nombres de todos los tipos de modelos conceptuales o de almacenamiento a los que se hace referencia en MSL deben estar calificados con sus respectivos nombres de espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="15852-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="15852-121">Para obtener información sobre el nombre de espacio de nombres del modelo conceptual, vea el elemento de esquema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="15852-122">Para obtener información sobre el nombre de espacio de nombres del modelo de almacenamiento, vea el elemento de esquema de almacenamiento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="15852-123">El **Alias** elemento no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="15852-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-124">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-124">Applicable Attributes</span></span>

<span data-ttu-id="15852-125">En la tabla siguiente se describe los atributos que se pueden aplicar a la **Alias** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="15852-126">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-126">Attribute Name</span></span> | <span data-ttu-id="15852-127">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-127">Is Required</span></span> | <span data-ttu-id="15852-128">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="15852-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="15852-129">**Key**</span></span>        | <span data-ttu-id="15852-130">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-130">Yes</span></span>         | <span data-ttu-id="15852-131">El alias del espacio de nombres especificado por el **valor** atributo.</span><span class="sxs-lookup"><span data-stu-id="15852-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="15852-132">**Valor**</span><span class="sxs-lookup"><span data-stu-id="15852-132">**Value**</span></span>      | <span data-ttu-id="15852-133">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-133">Yes</span></span>         | <span data-ttu-id="15852-134">El espacio de nombres para el que el valor de la **clave** elemento es un alias.</span><span class="sxs-lookup"><span data-stu-id="15852-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="15852-135">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-135">Example</span></span>

<span data-ttu-id="15852-136">El ejemplo siguiente se muestra un **Alias** elemento que define un alias, `c`, para los tipos que se definen en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="associationend-element-msl"></a><span data-ttu-id="15852-137">AssociationEnd (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="15852-138">El **AssociationEnd** elemento en el lenguaje de especificación de (asignaciones MSL) se usa cuando las funciones de modificación de un tipo de entidad en el modelo conceptual se asignan a los procedimientos almacenados en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="15852-139">Si una modificación de procedimiento almacenado toma un parámetro cuyo valor se mantiene en una propiedad de asociación, el **AssociationEnd** elemento asigna el valor de propiedad para el parámetro.</span><span class="sxs-lookup"><span data-stu-id="15852-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="15852-140">Para obtener más información, vea el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="15852-140">For more information, see the example below.</span></span>

<span data-ttu-id="15852-141">Para obtener más información sobre cómo asignar funciones de modificación de tipos de entidad a procedimientos almacenados, vea el elemento ModificationFunctionMapping (MSL) y Tutorial: asignar una entidad a procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="15852-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="15852-142">El **AssociationEnd** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="15852-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="15852-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-144">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-144">Applicable Attributes</span></span>

<span data-ttu-id="15852-145">En la tabla siguiente se describe los atributos que se aplican a la **AssociationEnd** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="15852-146">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-146">Attribute Name</span></span>     | <span data-ttu-id="15852-147">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-147">Is Required</span></span> | <span data-ttu-id="15852-148">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="15852-149">**AssociationSet**</span></span> | <span data-ttu-id="15852-150">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-150">Yes</span></span>         | <span data-ttu-id="15852-151">El nombre de la asociación que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="15852-152">**From**</span><span class="sxs-lookup"><span data-stu-id="15852-152">**From**</span></span>           | <span data-ttu-id="15852-153">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-153">Yes</span></span>         | <span data-ttu-id="15852-154">El valor de la **FromRole** atributo de la propiedad de navegación que corresponde a la asociación que se está asigna.</span><span class="sxs-lookup"><span data-stu-id="15852-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="15852-155">Para obtener más información, vea el elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="15852-156">**En**</span><span class="sxs-lookup"><span data-stu-id="15852-156">**To**</span></span>             | <span data-ttu-id="15852-157">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-157">Yes</span></span>         | <span data-ttu-id="15852-158">El valor de la **ToRole** atributo de la propiedad de navegación que corresponde a la asociación que se está asigna.</span><span class="sxs-lookup"><span data-stu-id="15852-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="15852-159">Para obtener más información, vea el elemento NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="15852-160">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-160">Example</span></span>

<span data-ttu-id="15852-161">Considere el siguiente tipo de entidad del modelo conceptual:</span><span class="sxs-lookup"><span data-stu-id="15852-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="15852-162">Considere también el siguiente procedimiento almacenado:</span><span class="sxs-lookup"><span data-stu-id="15852-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="15852-163">Para asignar la función de actualización de la `Course` entidad a este procedimiento almacenado, debe proporcionar un valor para el **DepartmentID** parámetro.</span><span class="sxs-lookup"><span data-stu-id="15852-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="15852-164">El valor para `DepartmentID` no corresponde a una propiedad del tipo de entidad; está contenido en una asociación independiente cuya asignación se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="15852-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="15852-165">El siguiente código muestra la **AssociationEnd** elemento que se usa para asignar el **DepartmentID** propiedad de la **FK\_curso\_departamento** asociación a la **UpdateCourse** procedimiento almacenado (a la que la función de actualización de la **curso** se asigna el tipo de entidad):</span><span class="sxs-lookup"><span data-stu-id="15852-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="15852-166">AssociationSetMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="15852-167">El **AssociationSetMapping** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre una asociación de las columnas de tabla y modelo conceptuales en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="15852-168">Las asociaciones del modelo conceptual son tipos cuyas propiedades representan columnas de clave primaria y clave externa de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="15852-169">El **AssociationSetMapping** elemento utiliza dos elementos de EndProperty para definir las asignaciones entre las propiedades de tipo de asociación y columnas en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="15852-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="15852-170">Puede definir condiciones en estas asignaciones con el elemento Condition.</span><span class="sxs-lookup"><span data-stu-id="15852-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="15852-171">Asignar el insert, update y delete de funciones para las asociaciones a procedimientos almacenados en la base de datos con el elemento ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="15852-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="15852-172">Definir asignaciones de solo lectura entre las asociaciones y columnas de la tabla mediante una cadena de Entity SQL en un QueryView (elemento).</span><span class="sxs-lookup"><span data-stu-id="15852-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="15852-173">Si se define una restricción referencial para una asociación en el modelo conceptual, la asociación no es necesario que debe asignarse un **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="15852-174">Si un **AssociationSetMapping** elemento está presente para una asociación que tiene una restricción referencial, las asignaciones definidas en el **AssociationSetMapping** se omitirá el elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="15852-175">Para obtener más información, vea el elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="15852-176">El **AssociationSetMapping** elemento puede tener los siguientes elementos secundarios</span><span class="sxs-lookup"><span data-stu-id="15852-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="15852-177">QueryView (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="15852-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="15852-178">EndProperty (cero o dos)</span><span class="sxs-lookup"><span data-stu-id="15852-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="15852-179">Condición (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="15852-180">ModificationFunctionMapping (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="15852-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-181">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-181">Applicable Attributes</span></span>

<span data-ttu-id="15852-182">En la tabla siguiente se describe los atributos que se pueden aplicar a la **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="15852-183">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-183">Attribute Name</span></span>     | <span data-ttu-id="15852-184">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-184">Is Required</span></span> | <span data-ttu-id="15852-185">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-186">**Name**</span><span class="sxs-lookup"><span data-stu-id="15852-186">**Name**</span></span>           | <span data-ttu-id="15852-187">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-187">Yes</span></span>         | <span data-ttu-id="15852-188">El nombre del conjunto de asociaciones del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="15852-189">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="15852-189">**TypeName**</span></span>       | <span data-ttu-id="15852-190">No</span><span class="sxs-lookup"><span data-stu-id="15852-190">No</span></span>          | <span data-ttu-id="15852-191">El nombre completo, calificado con el espacio de nombres, del tipo de asociación del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="15852-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="15852-192">**StoreEntitySet**</span></span> | <span data-ttu-id="15852-193">No</span><span class="sxs-lookup"><span data-stu-id="15852-193">No</span></span>          | <span data-ttu-id="15852-194">El nombre de la tabla que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="15852-195">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-195">Example</span></span>

<span data-ttu-id="15852-196">El ejemplo siguiente se muestra un **AssociationSetMapping** elemento en el que el **FK\_curso\_departamento** conjunto de asociaciones en el modelo conceptual se asignan a la  **Curso** tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="15852-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="15852-197">Las asignaciones entre columnas de tabla y las propiedades de tipo de asociación se especifican en el secundario **EndProperty** elementos.</span><span class="sxs-lookup"><span data-stu-id="15852-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="15852-198">ComplexProperty (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="15852-199">Un **ComplexProperty** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre una propiedad de tipo complejo en un modelo conceptual entidad tipo y columnas de tabla en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="15852-200">Las asignaciones de columnas y propiedades se especifican en los elementos ScalarProperty secundarios.</span><span class="sxs-lookup"><span data-stu-id="15852-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="15852-201">El **ComplexType** elemento de propiedad puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="15852-202">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="15852-203">**ComplexProperty** (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="15852-204">ComplextTypeMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="15852-205">Condición (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-206">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-206">Applicable Attributes</span></span>

<span data-ttu-id="15852-207">En la tabla siguiente se describe los atributos que se aplican a la **ComplexProperty** elemento:</span><span class="sxs-lookup"><span data-stu-id="15852-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="15852-208">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-208">Attribute Name</span></span> | <span data-ttu-id="15852-209">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-209">Is Required</span></span> | <span data-ttu-id="15852-210">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-211">**Name**</span><span class="sxs-lookup"><span data-stu-id="15852-211">**Name**</span></span>       | <span data-ttu-id="15852-212">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-212">Yes</span></span>         | <span data-ttu-id="15852-213">El nombre de la propiedad compleja de un tipo de entidad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="15852-214">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="15852-214">**TypeName**</span></span>   | <span data-ttu-id="15852-215">No</span><span class="sxs-lookup"><span data-stu-id="15852-215">No</span></span>          | <span data-ttu-id="15852-216">El nombre completo, calificado con el espacio de nombres, del tipo de propiedad del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="15852-217">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-217">Example</span></span>

<span data-ttu-id="15852-218">En el siguiente ejemplo se basa en el modelo School.</span><span class="sxs-lookup"><span data-stu-id="15852-218">The following example is based on the School model.</span></span> <span data-ttu-id="15852-219">El siguiente tipo complejo se ha agregado al modelo conceptual:</span><span class="sxs-lookup"><span data-stu-id="15852-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="15852-220">El **LastName** y **FirstName** propiedades de la **persona** tipo de entidad se han reemplazado por una propiedad compleja, **nombre**:</span><span class="sxs-lookup"><span data-stu-id="15852-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="15852-221">El siguiente MSL muestra la **ComplexProperty** elemento que se usa para asignar el **nombre** propiedad a las columnas de la base de datos subyacente:</span><span class="sxs-lookup"><span data-stu-id="15852-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="15852-222">ComplexTypeMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="15852-223">El **ComplexTypeMapping** elemento en el lenguaje de especificación de (asignaciones MSL) es un elemento secundario del elemento ResultMapping y define la asignación entre una importación de función en el modelo conceptual y un procedimiento almacenado de subyacente base de datos cuando se cumple lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="15852-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="15852-224">La importación de función devuelve un tipo complejo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="15852-225">Los nombres de las columnas devueltas por el procedimiento almacenado no coinciden exactamente con los nombres de las propiedades en el tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="15852-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="15852-226">De forma predeterminada, la asignación entre las columnas devueltas por un procedimiento almacenado y un tipo complejo se basa en los nombres de las propiedades y de las columnas.</span><span class="sxs-lookup"><span data-stu-id="15852-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="15852-227">Si los nombres de columna no coincidan con los nombres de propiedad, se debe utilizar el **ComplexTypeMapping** elemento para definir la asignación.</span><span class="sxs-lookup"><span data-stu-id="15852-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="15852-228">Para obtener un ejemplo de la asignación predeterminada, vea el elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="15852-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="15852-229">El **ComplexTypeMapping** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="15852-230">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-231">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-231">Applicable Attributes</span></span>

<span data-ttu-id="15852-232">En la tabla siguiente se describe los atributos que se aplican a la **ComplexTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="15852-233">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-233">Attribute Name</span></span> | <span data-ttu-id="15852-234">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-234">Is Required</span></span> | <span data-ttu-id="15852-235">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="15852-236">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="15852-236">**TypeName**</span></span>   | <span data-ttu-id="15852-237">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-237">Yes</span></span>         | <span data-ttu-id="15852-238">El nombre completo, incluido el espacio de nombres, del tipo complejo que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="15852-239">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-239">Example</span></span>

<span data-ttu-id="15852-240">Considere el siguiente procedimiento almacenado:</span><span class="sxs-lookup"><span data-stu-id="15852-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="15852-241">Considere también el siguiente tipo complejo del modelo conceptual:</span><span class="sxs-lookup"><span data-stu-id="15852-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="15852-242">Para crear una importación de función que devuelva instancias del tipo complejo anterior, la asignación entre las columnas devueltas por el procedimiento almacenado y el tipo de entidad debe definirse en un **ComplexTypeMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="15852-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="15852-243">Condition (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-243">Condition Element (MSL)</span></span>

<span data-ttu-id="15852-244">El **condición** elemento en el lenguaje de especificación de (asignaciones MSL) establece condiciones en las asignaciones entre el modelo conceptual y la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="15852-245">La asignación que se define dentro de un nodo XML es válidas si todas las condiciones, como elemento secundario especificado en **condición** elementos, se cumplen.</span><span class="sxs-lookup"><span data-stu-id="15852-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="15852-246">De lo contrario, la asignación no es válida.</span><span class="sxs-lookup"><span data-stu-id="15852-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="15852-247">Por ejemplo, si un elemento MappingFragment contiene uno o varios **condición** elementos secundarios, la asignación definida dentro del **MappingFragment** nodo solo serán válido si todas las condiciones del elemento secundario  **Condición** se cumplen los elementos.</span><span class="sxs-lookup"><span data-stu-id="15852-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="15852-248">Cada condición se puede aplicar a un **nombre** (el nombre de una propiedad de entidad del modelo conceptual, especificada por el **nombre** atributo), o un **ColumnName** (el nombre de una columna de la base de datos, especificado por el **ColumnName** atributo).</span><span class="sxs-lookup"><span data-stu-id="15852-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="15852-249">Cuando el **nombre** atributo está establecido, la condición se comprueba contra un valor de propiedad de entidad.</span><span class="sxs-lookup"><span data-stu-id="15852-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="15852-250">Cuando el **ColumnName** atributo está establecido, la condición se comprueba contra un valor de columna.</span><span class="sxs-lookup"><span data-stu-id="15852-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="15852-251">Solo uno de los **nombre** o **ColumnName** atributo puede especificarse en un **condición** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="15852-252">Cuando el **condición** elemento se usa dentro de un elemento FunctionImportMapping, sólo el **nombre** atributo no es aplicable.</span><span class="sxs-lookup"><span data-stu-id="15852-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="15852-253">El **condición** elemento puede ser un elemento secundario de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="15852-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="15852-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="15852-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="15852-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="15852-255">ComplexProperty</span></span>
-   <span data-ttu-id="15852-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="15852-256">EntitySetMapping</span></span>
-   <span data-ttu-id="15852-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="15852-257">MappingFragment</span></span>
-   <span data-ttu-id="15852-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="15852-258">EntityTypeMapping</span></span>

<span data-ttu-id="15852-259">El **condición** elemento no puede tener ningún elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="15852-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-260">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-260">Applicable Attributes</span></span>

<span data-ttu-id="15852-261">En la tabla siguiente se describe los atributos que se aplican a la **condición** elemento:</span><span class="sxs-lookup"><span data-stu-id="15852-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="15852-262">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-262">Attribute Name</span></span> | <span data-ttu-id="15852-263">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-263">Is Required</span></span> | <span data-ttu-id="15852-264">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="15852-265">**ColumnName**</span></span> | <span data-ttu-id="15852-266">No</span><span class="sxs-lookup"><span data-stu-id="15852-266">No</span></span>          | <span data-ttu-id="15852-267">El nombre de la columna de la tabla cuyo valor se utiliza para evaluar la condición.</span><span class="sxs-lookup"><span data-stu-id="15852-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="15852-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="15852-268">**IsNull**</span></span>     | <span data-ttu-id="15852-269">No</span><span class="sxs-lookup"><span data-stu-id="15852-269">No</span></span>          | <span data-ttu-id="15852-270">**True** o **False**.</span><span class="sxs-lookup"><span data-stu-id="15852-270">**True** or **False**.</span></span> <span data-ttu-id="15852-271">Si el valor es **True** y el valor de columna es **null**, o si el valor es **False** y el valor de columna no es **null**, la condición es true .</span><span class="sxs-lookup"><span data-stu-id="15852-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="15852-272">De lo contrario, la condición es falsa.</span><span class="sxs-lookup"><span data-stu-id="15852-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="15852-273">El **IsNull** y **valor** atributos no se puede usar al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="15852-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="15852-274">**Valor**</span><span class="sxs-lookup"><span data-stu-id="15852-274">**Value**</span></span>      | <span data-ttu-id="15852-275">No</span><span class="sxs-lookup"><span data-stu-id="15852-275">No</span></span>          | <span data-ttu-id="15852-276">El valor con el que se compara el valor de la columna.</span><span class="sxs-lookup"><span data-stu-id="15852-276">The value with which the column value is compared.</span></span> <span data-ttu-id="15852-277">Si los valores son los mismos, la condición es verdadera.</span><span class="sxs-lookup"><span data-stu-id="15852-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="15852-278">De lo contrario, la condición es falsa.</span><span class="sxs-lookup"><span data-stu-id="15852-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="15852-279">El **IsNull** y **valor** atributos no se puede usar al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="15852-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="15852-280">**Name**</span><span class="sxs-lookup"><span data-stu-id="15852-280">**Name**</span></span>       | <span data-ttu-id="15852-281">No</span><span class="sxs-lookup"><span data-stu-id="15852-281">No</span></span>          | <span data-ttu-id="15852-282">El nombre de la propiedad de entidad del modelo conceptual cuyo valor se utiliza para evaluar la condición.</span><span class="sxs-lookup"><span data-stu-id="15852-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="15852-283">Este atributo no es aplicable si la **condición** elemento se usa dentro de un elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="15852-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="15852-284">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-284">Example</span></span>

<span data-ttu-id="15852-285">El ejemplo siguiente muestra **condición** elementos como elementos secundarios de **MappingFragment** elementos.</span><span class="sxs-lookup"><span data-stu-id="15852-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="15852-286">Cuando **HireDate** no es null y **EnrollmentDate** es null, se asignan los datos entre el **SchoolModel.Instructor** tipo y el **PersonID**y **HireDate** columnas de la **persona** tabla.</span><span class="sxs-lookup"><span data-stu-id="15852-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="15852-287">Cuando **EnrollmentDate** no es null y **HireDate** es null, se asignan los datos entre el **SchoolModel.Student** tipo y el **PersonID** y **inscripción** columnas de la **persona** tabla.</span><span class="sxs-lookup"><span data-stu-id="15852-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="15852-288">DeleteFunction (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="15852-289">El **DeleteFunction** elemento en el lenguaje de especificación de (asignaciones MSL) asigna la función de eliminación de un tipo de entidad o asociación en el modelo conceptual a un procedimiento almacenado en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="15852-290">Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="15852-291">Para obtener más información, vea el elemento de función de almacenamiento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="15852-292">Si no asigna las tres de la inserción, actualización o eliminación de las operaciones de un tipo de entidad a procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecuta en tiempo de ejecución y se produce un UpdateException.</span><span class="sxs-lookup"><span data-stu-id="15852-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="15852-293">DeleteFunction aplicado a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="15852-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="15852-294">Cuando se aplica al elemento EntityTypeMapping, el **DeleteFunction** elemento la función de eliminación de un tipo de entidad en el modelo conceptual asigna a un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="15852-295">El **DeleteFunction** elemento puede tener los elementos secundarios siguientes cuando se aplica a un **EntityTypeMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="15852-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="15852-296">AssociationEnd (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="15852-297">ComplexProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="15852-298">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="15852-299">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-299">Applicable Attributes</span></span>

<span data-ttu-id="15852-300">En la tabla siguiente se describe los atributos que se pueden aplicar a la **DeleteFunction** elemento cuando se aplica a un **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="15852-301">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-301">Attribute Name</span></span>            | <span data-ttu-id="15852-302">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-302">Is Required</span></span> | <span data-ttu-id="15852-303">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="15852-304">**FunctionName**</span></span>          | <span data-ttu-id="15852-305">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-305">Yes</span></span>         | <span data-ttu-id="15852-306">El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de eliminación está asignada.</span><span class="sxs-lookup"><span data-stu-id="15852-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="15852-307">El procedimiento almacenado se debe declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="15852-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="15852-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="15852-309">No</span><span class="sxs-lookup"><span data-stu-id="15852-309">No</span></span>          | <span data-ttu-id="15852-310">El nombre del parámetro de salida que devuelve el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="15852-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="15852-311">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-311">Example</span></span>

<span data-ttu-id="15852-312">El siguiente ejemplo se basa en el modelo School y se muestra el **DeleteFunction** elemento de asignación de la función de eliminación de la **persona** tipo de entidad a la **DeletePerson** procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="15852-313">El **DeletePerson** se declara un procedimiento almacenado en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="15852-314">DeleteFunction aplicado a AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="15852-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="15852-315">Cuando se aplica al elemento AssociationSetMapping, el **DeleteFunction** elemento la función de eliminación de una asociación en el modelo conceptual asigna a un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="15852-316">El **DeleteFunction** elemento puede tener los elementos secundarios siguientes cuando se aplica a la **AssociationSetMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="15852-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="15852-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="15852-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="15852-318">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-318">Applicable Attributes</span></span>

<span data-ttu-id="15852-319">En la tabla siguiente se describe los atributos que se pueden aplicar a la **DeleteFunction** elemento cuando se aplica a la **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="15852-320">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-320">Attribute Name</span></span>            | <span data-ttu-id="15852-321">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-321">Is Required</span></span> | <span data-ttu-id="15852-322">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="15852-323">**FunctionName**</span></span>          | <span data-ttu-id="15852-324">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-324">Yes</span></span>         | <span data-ttu-id="15852-325">El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de eliminación está asignada.</span><span class="sxs-lookup"><span data-stu-id="15852-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="15852-326">El procedimiento almacenado se debe declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="15852-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="15852-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="15852-328">No</span><span class="sxs-lookup"><span data-stu-id="15852-328">No</span></span>          | <span data-ttu-id="15852-329">El nombre del parámetro de salida que devuelve el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="15852-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="15852-330">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-330">Example</span></span>

<span data-ttu-id="15852-331">El siguiente ejemplo se basa en el modelo School y se muestra el **DeleteFunction** elemento que se usa para asignar la función de eliminación de la **CourseInstructor** asociación a la  **DeleteCourseInstructor** procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="15852-332">El **DeleteCourseInstructor** se declara un procedimiento almacenado en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="15852-333">EndProperty (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="15852-334">El **EndProperty** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre un extremo o una función de modificación de una asociación del modelo conceptual y la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="15852-335">La asignación de columnas de la propiedad se especifica en un elemento de ScalarProperty secundario.</span><span class="sxs-lookup"><span data-stu-id="15852-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="15852-336">Cuando un **EndProperty** elemento se usa para definir la asignación para el extremo de asociación de un modelo conceptual, es un elemento secundario de un elemento AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="15852-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="15852-337">Cuando el **EndProperty** elemento se usa para definir la asignación para una función de modificación de una asociación del modelo conceptual, es un elemento secundario de un elemento InsertFunction o DeleteFunction (elemento).</span><span class="sxs-lookup"><span data-stu-id="15852-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="15852-338">El **EndProperty** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="15852-339">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-340">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-340">Applicable Attributes</span></span>

<span data-ttu-id="15852-341">En la tabla siguiente se describe los atributos que se aplican a la **EndProperty** elemento:</span><span class="sxs-lookup"><span data-stu-id="15852-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="15852-342">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-342">Attribute Name</span></span> | <span data-ttu-id="15852-343">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-343">Is Required</span></span> | <span data-ttu-id="15852-344">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="15852-345">nombre</span><span class="sxs-lookup"><span data-stu-id="15852-345">Name</span></span>           | <span data-ttu-id="15852-346">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-346">Yes</span></span>         | <span data-ttu-id="15852-347">El nombre del extremo de la asociación que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="15852-348">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-348">Example</span></span>

<span data-ttu-id="15852-349">El ejemplo siguiente se muestra un **AssociationSetMapping** elemento en el que el **FK\_curso\_departamento** asociación del modelo conceptual se asigna a la **Curso** tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="15852-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="15852-350">Las asignaciones entre columnas de tabla y las propiedades de tipo de asociación se especifican en el secundario **EndProperty** elementos.</span><span class="sxs-lookup"><span data-stu-id="15852-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="15852-351">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-351">Example</span></span>

<span data-ttu-id="15852-352">El ejemplo siguiente se muestra el **EndProperty** las funciones de inserción y eliminación de una asociación de asignación de elemento (**CourseInstructor**) a los procedimientos almacenados en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="15852-353">Las funciones asignadas se declaran en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="15852-354">EntityContainerMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="15852-355">El **EntityContainerMapping** elemento en el lenguaje de especificación de (asignaciones MSL) asigna el contenedor de entidades en el modelo conceptual al contenedor de entidades del modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="15852-356">El **EntityContainerMapping** es un elemento secundario del elemento de asignación.</span><span class="sxs-lookup"><span data-stu-id="15852-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="15852-357">El **EntityContainerMapping** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="15852-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="15852-358">EntitySetMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="15852-359">AssociationSetMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="15852-360">FunctionImportMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-361">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-361">Applicable Attributes</span></span>

<span data-ttu-id="15852-362">En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntityContainerMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="15852-363">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-363">Attribute Name</span></span>            | <span data-ttu-id="15852-364">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-364">Is Required</span></span> | <span data-ttu-id="15852-365">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="15852-366">**StorageModelContainer**</span></span> | <span data-ttu-id="15852-367">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-367">Yes</span></span>         | <span data-ttu-id="15852-368">El nombre del contenedor de entidades del modelo de almacenamiento que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="15852-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="15852-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="15852-370">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-370">Yes</span></span>         | <span data-ttu-id="15852-371">El nombre del contenedor de entidades del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="15852-372">**Generateupdateviews como**</span><span class="sxs-lookup"><span data-stu-id="15852-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="15852-373">No</span><span class="sxs-lookup"><span data-stu-id="15852-373">No</span></span>          | <span data-ttu-id="15852-374">**True** o **False**.</span><span class="sxs-lookup"><span data-stu-id="15852-374">**True** or **False**.</span></span> <span data-ttu-id="15852-375">Si **False**, no hay ninguna vista de actualización se genera.</span><span class="sxs-lookup"><span data-stu-id="15852-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="15852-376">Este atributo debe establecerse en **False** cuando haya una asignación de solo lectura que podría no ser válida porque los datos pueden no ida y vuelta correctamente.</span><span class="sxs-lookup"><span data-stu-id="15852-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="15852-377">El valor predeterminado es **True**.</span><span class="sxs-lookup"><span data-stu-id="15852-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="15852-378">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-378">Example</span></span>

<span data-ttu-id="15852-379">El ejemplo siguiente se muestra un **EntityContainerMapping** elemento que se asigna el **SchoolModelEntities** contenedor (el contenedor de entidades del modelo conceptual) a la  **SchoolModelStoreContainer** contenedor (el contenedor de entidades del modelo de almacenamiento):</span><span class="sxs-lookup"><span data-stu-id="15852-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="15852-380">EntitySetMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="15852-381">El **EntitySetMapping** establece el elemento en la especificación de lenguaje (MSL) asigna todos los tipos de entidades del modelo conceptual se establecen en la entidad del modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="15852-382">Un conjunto de entidades en el modelo conceptual es un contenedor lógico para instancias de entidades del mismo tipo (y los tipos derivados).</span><span class="sxs-lookup"><span data-stu-id="15852-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="15852-383">Un conjunto de entidades en el modelo de almacenamiento representa una tabla o vista en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="15852-384">El conjunto de entidades del modelo conceptual especificado por el valor de la **nombre** atributo de la **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="15852-385">La asigna a la tabla o vista es especificada por el **StoreEntitySet** atributo en cada MappingFragment, elemento secundario o en el **EntitySetMapping** propio elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="15852-386">El **EntitySetMapping** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="15852-387">EntityTypeMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="15852-388">QueryView (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="15852-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="15852-389">MappingFragment (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-390">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-390">Applicable Attributes</span></span>

<span data-ttu-id="15852-391">En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="15852-392">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-392">Attribute Name</span></span>           | <span data-ttu-id="15852-393">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-393">Is Required</span></span> | <span data-ttu-id="15852-394">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-395">**Name**</span><span class="sxs-lookup"><span data-stu-id="15852-395">**Name**</span></span>                 | <span data-ttu-id="15852-396">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-396">Yes</span></span>         | <span data-ttu-id="15852-397">El nombre del conjunto de entidades del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="15852-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="15852-398">**TypeName** **1**</span></span>       | <span data-ttu-id="15852-399">No</span><span class="sxs-lookup"><span data-stu-id="15852-399">No</span></span>          | <span data-ttu-id="15852-400">El nombre del tipo de entidad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="15852-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="15852-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="15852-402">No</span><span class="sxs-lookup"><span data-stu-id="15852-402">No</span></span>          | <span data-ttu-id="15852-403">El nombre del conjunto de entidades del modelo de almacenamiento al que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="15852-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="15852-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="15852-405">No</span><span class="sxs-lookup"><span data-stu-id="15852-405">No</span></span>          | <span data-ttu-id="15852-406">**True** o **False** dependiendo de si se devuelven filas distintas solo.</span><span class="sxs-lookup"><span data-stu-id="15852-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="15852-407">Si este atributo se establece en **True**, **generateupdateviews como** atributo del elemento EntityContainerMapping debe establecerse en **False**.</span><span class="sxs-lookup"><span data-stu-id="15852-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="15852-408">**1** el **TypeName** y **StoreEntitySet** atributos se pueden usar en lugar de los elementos secundarios EntityTypeMapping y MappingFragment para asignar un tipo de entidad única a una sola tabla.</span><span class="sxs-lookup"><span data-stu-id="15852-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="15852-409">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-409">Example</span></span>

<span data-ttu-id="15852-410">El ejemplo siguiente se muestra un **EntitySetMapping** elemento que asigna tres tipos (un tipo base y dos tipos derivados) en el **cursos** conjunto de entidades del modelo conceptual a tres tablas diferentes en el base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="15852-411">Las tablas se especifican mediante el **StoreEntitySet** atributo en cada **MappingFragment** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="15852-412">EntityTypeMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="15852-413">El **EntityTypeMapping** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre un tipo de entidad en el modelo conceptual y las tablas o vistas en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="15852-414">Para obtener información sobre los tipos de entidad del modelo conceptual y las tablas de base de datos o vistas subyacente, vea el elemento EntityType (CSDL) y el elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="15852-415">El tipo de entidad del modelo conceptual que se está asignando especificado por el **TypeName** atributo de la **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="15852-416">Especifica la tabla o vista que se está asignando el **StoreEntitySet** atributo del elemento MappingFragment secundario.</span><span class="sxs-lookup"><span data-stu-id="15852-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="15852-417">El ModificationFunctionMapping elemento secundario se puede usar para asignar la inserción, actualización o eliminación de funciones de tipos de entidad a procedimientos almacenados de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="15852-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="15852-418">El **EntityTypeMapping** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="15852-419">MappingFragment (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="15852-420">ModificationFunctionMapping (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="15852-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="15852-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="15852-421">ScalarProperty</span></span>
-   <span data-ttu-id="15852-422">Condición</span><span class="sxs-lookup"><span data-stu-id="15852-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="15852-423">**MappingFragment** y **ModificationFunctionMapping** elementos no pueden ser elementos secundarios de la **EntityTypeMapping** elemento al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="15852-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="15852-424">El **ScalarProperty** y **condición** elementos sólo pueden ser elementos secundarios de la **EntityTypeMapping** elemento cuando se utiliza dentro de un elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="15852-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-425">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-425">Applicable Attributes</span></span>

<span data-ttu-id="15852-426">En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="15852-427">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-427">Attribute Name</span></span> | <span data-ttu-id="15852-428">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-428">Is Required</span></span> | <span data-ttu-id="15852-429">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-430">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="15852-430">**TypeName**</span></span>   | <span data-ttu-id="15852-431">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-431">Yes</span></span>         | <span data-ttu-id="15852-432">El nombre completo, calificado con el espacio de nombres, del tipo de entidad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="15852-433">Si el tipo es abstracto o un tipo derivado, el valor debe ser `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="15852-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="15852-434">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-434">Example</span></span>

<span data-ttu-id="15852-435">El ejemplo siguiente muestra un elemento EntitySetMapping con dos secundarios **EntityTypeMapping** elementos.</span><span class="sxs-lookup"><span data-stu-id="15852-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="15852-436">En la primera **EntityTypeMapping** elemento, el **SchoolModel.Person** tipo de entidad se asigna a la **persona** tabla.</span><span class="sxs-lookup"><span data-stu-id="15852-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="15852-437">En el segundo **EntityTypeMapping** elemento, la funcionalidad de actualización de la **SchoolModel.Person** tipo se asigna a un procedimiento almacenado, **UpdatePerson**, en la base de datos .</span><span class="sxs-lookup"><span data-stu-id="15852-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="15852-438">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-438">Example</span></span>

<span data-ttu-id="15852-439">En el ejemplo siguiente se muestra la asignación de una jerarquía de tipos en la que el tipo raíz es abstracto.</span><span class="sxs-lookup"><span data-stu-id="15852-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="15852-440">Tenga en cuenta el uso de la `IsOfType` sintaxis para el **TypeName** atributos.</span><span class="sxs-lookup"><span data-stu-id="15852-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="15852-441">FunctionImportMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="15852-442">El **FunctionImportMapping** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre una importación de función en el modelo conceptual y un procedimiento almacenado o función en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="15852-443">Las importaciones de función se deben declarar en el modelo conceptual, mientras que los procedimientos almacenados se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="15852-444">Para obtener más información, vea el elemento FunctionImport (CSDL) y el elemento de función de almacenamiento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="15852-445">De forma predeterminada, si una importación de función devuelve un tipo de entidad del modelo conceptual o un tipo complejo, entonces los nombres de las columnas devueltas por el procedimiento almacenado subyacente deben coincidir exactamente con los nombres de las propiedades del tipo del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="15852-446">Si los nombres de columna no coincidan con los nombres de propiedad, la asignación debe definirse en un elemento ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="15852-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="15852-447">El **FunctionImportMapping** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="15852-448">ResultMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-449">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-449">Applicable Attributes</span></span>

<span data-ttu-id="15852-450">En la tabla siguiente se describe los atributos que se aplican a la **FunctionImportMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="15852-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="15852-451">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-451">Attribute Name</span></span>         | <span data-ttu-id="15852-452">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-452">Is Required</span></span> | <span data-ttu-id="15852-453">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="15852-454">**FunctionImportName**</span></span> | <span data-ttu-id="15852-455">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-455">Yes</span></span>         | <span data-ttu-id="15852-456">El nombre de la importación de función del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="15852-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="15852-457">**FunctionName**</span></span>       | <span data-ttu-id="15852-458">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-458">Yes</span></span>         | <span data-ttu-id="15852-459">El nombre, calificado con el espacio de nombres, de la función del modelo de almacenamiento que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="15852-460">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-460">Example</span></span>

<span data-ttu-id="15852-461">En el siguiente ejemplo se basa en el modelo School.</span><span class="sxs-lookup"><span data-stu-id="15852-461">The following example is based on the School model.</span></span> <span data-ttu-id="15852-462">Considere la siguiente función en el modelo de almacenamiento:</span><span class="sxs-lookup"><span data-stu-id="15852-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="15852-463">También considere esta importación de función en el modelo conceptual:</span><span class="sxs-lookup"><span data-stu-id="15852-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="15852-464">El siguiente ejemplo muestra un **FunctionImportMapping** elemento que se usa para asignar la función y la importación de función anteriores entre sí:</span><span class="sxs-lookup"><span data-stu-id="15852-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="15852-465">InsertFunction (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="15852-466">El **InsertFunction** elemento en el lenguaje de especificación de (asignaciones MSL) asigna la función de inserción de un tipo de entidad o asociación en el modelo conceptual a un procedimiento almacenado en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="15852-467">Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="15852-468">Para obtener más información, vea el elemento de función de almacenamiento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="15852-469">Si no asigna las tres de la inserción, actualización o eliminación de las operaciones de un tipo de entidad a procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecuta en tiempo de ejecución y se produce un UpdateException.</span><span class="sxs-lookup"><span data-stu-id="15852-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="15852-470">El **InsertFunction** elemento puede ser un elemento secundario del elemento ModificationFunctionMapping y aplicado a EntityTypeMapping (elemento) o AssociationSetMapping (elemento).</span><span class="sxs-lookup"><span data-stu-id="15852-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="15852-471">InsertFunction aplicado a EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="15852-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="15852-472">Cuando se aplica al elemento EntityTypeMapping, el **InsertFunction** elemento la función de inserción de un tipo de entidad en el modelo conceptual asigna a un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="15852-473">El **InsertFunction** elemento puede tener los elementos secundarios siguientes cuando se aplica a un **EntityTypeMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="15852-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="15852-474">AssociationEnd (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="15852-475">ComplexProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="15852-476">ResultBinding (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="15852-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="15852-477">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="15852-478">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-478">Applicable Attributes</span></span>

<span data-ttu-id="15852-479">En la tabla siguiente se describe los atributos que se pueden aplicar a la **InsertFunction** elemento cuando se aplica a un **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="15852-480">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-480">Attribute Name</span></span>            | <span data-ttu-id="15852-481">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-481">Is Required</span></span> | <span data-ttu-id="15852-482">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="15852-483">**FunctionName**</span></span>          | <span data-ttu-id="15852-484">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-484">Yes</span></span>         | <span data-ttu-id="15852-485">El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de inserción está asignada.</span><span class="sxs-lookup"><span data-stu-id="15852-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="15852-486">El procedimiento almacenado se debe declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="15852-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="15852-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="15852-488">No</span><span class="sxs-lookup"><span data-stu-id="15852-488">No</span></span>          | <span data-ttu-id="15852-489">El nombre del parámetro de salida que devuelve el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="15852-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="15852-490">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-490">Example</span></span>

<span data-ttu-id="15852-491">El siguiente ejemplo se basa en el modelo School y se muestra el **InsertFunction** elemento que se usa para asignar la función de inserción del tipo de entidad Person a la **InsertPerson** procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="15852-492">El **InsertPerson** se declara un procedimiento almacenado en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="15852-493">InsertFunction aplicado a AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="15852-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="15852-494">Cuando se aplica al elemento AssociationSetMapping, el **InsertFunction** elemento la función de inserción de una asociación en el modelo conceptual asigna a un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="15852-495">El **InsertFunction** elemento puede tener los elementos secundarios siguientes cuando se aplica a la **AssociationSetMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="15852-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="15852-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="15852-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="15852-497">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-497">Applicable Attributes</span></span>

<span data-ttu-id="15852-498">En la tabla siguiente se describe los atributos que se pueden aplicar a la **InsertFunction** elemento cuando se aplica a la **AssociationSetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="15852-499">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-499">Attribute Name</span></span>            | <span data-ttu-id="15852-500">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-500">Is Required</span></span> | <span data-ttu-id="15852-501">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="15852-502">**FunctionName**</span></span>          | <span data-ttu-id="15852-503">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-503">Yes</span></span>         | <span data-ttu-id="15852-504">El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de inserción está asignada.</span><span class="sxs-lookup"><span data-stu-id="15852-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="15852-505">El procedimiento almacenado se debe declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="15852-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="15852-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="15852-507">No</span><span class="sxs-lookup"><span data-stu-id="15852-507">No</span></span>          | <span data-ttu-id="15852-508">El nombre del parámetro de salida que devuelve el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="15852-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="15852-509">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-509">Example</span></span>

<span data-ttu-id="15852-510">El siguiente ejemplo se basa en el modelo School y se muestra el **InsertFunction** elemento que se usa para asignar la función de inserción de la **CourseInstructor** asociación a la  **InsertCourseInstructor** procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="15852-511">El **InsertCourseInstructor** se declara un procedimiento almacenado en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="15852-512">Mapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="15852-513">El **asignación** elemento en el lenguaje de especificación de (asignaciones MSL) contiene información para la asignación de objetos que se definen en un modelo conceptual a una base de datos (como se describe en un modelo de almacenamiento).</span><span class="sxs-lookup"><span data-stu-id="15852-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="15852-514">Para obtener más información, vea la especificación de CSDL y SSDL especificación.</span><span class="sxs-lookup"><span data-stu-id="15852-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="15852-515">El **asignación** es el elemento raíz de una especificación de asignaciones.</span><span class="sxs-lookup"><span data-stu-id="15852-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="15852-516">El espacio de nombres XML para la asignación de las especificaciones http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="15852-516">The XML namespace for mapping specifications is http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="15852-517">El elemento de asignación puede tener los elementos secundarios siguientes (en el orden mostrado):</span><span class="sxs-lookup"><span data-stu-id="15852-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="15852-518">Alias (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="15852-519">EntityContainerMapping (exactamente uno)</span><span class="sxs-lookup"><span data-stu-id="15852-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="15852-520">Los nombres de los tipos de modelos conceptuales y de almacenamiento a los que se hace referencia en MSL deben estar calificados con sus respectivos nombres de espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="15852-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="15852-521">Para obtener información sobre el nombre de espacio de nombres del modelo conceptual, vea el elemento de esquema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="15852-522">Para obtener información sobre el nombre de espacio de nombres del modelo de almacenamiento, vea el elemento de esquema de almacenamiento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="15852-523">Alias de espacios de nombres que se utilizan en MSL se pueden definir con el elemento de Alias.</span><span class="sxs-lookup"><span data-stu-id="15852-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-524">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-524">Applicable Attributes</span></span>

<span data-ttu-id="15852-525">En la tabla siguiente se describe los atributos que se pueden aplicar a la **asignación** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="15852-526">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-526">Attribute Name</span></span> | <span data-ttu-id="15852-527">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-527">Is Required</span></span> | <span data-ttu-id="15852-528">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="15852-529">**Espacio**</span><span class="sxs-lookup"><span data-stu-id="15852-529">**Space**</span></span>      | <span data-ttu-id="15852-530">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-530">Yes</span></span>         | <span data-ttu-id="15852-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="15852-531">**C-S**.</span></span> <span data-ttu-id="15852-532">Este es un valor fijo y no se puede cambiar.</span><span class="sxs-lookup"><span data-stu-id="15852-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="15852-533">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-533">Example</span></span>

<span data-ttu-id="15852-534">El ejemplo siguiente se muestra un **asignación** elemento que se basa en parte del modelo School.</span><span class="sxs-lookup"><span data-stu-id="15852-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="15852-535">Para obtener más información sobre el modelo School, vea la Guía de inicio rápido (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="15852-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="15852-536">MappingFragment (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="15852-537">El **MappingFragment** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre las propiedades de un tipo de entidad del modelo conceptual y una tabla o vista en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="15852-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="15852-538">Para obtener información sobre los tipos de entidad del modelo conceptual y las tablas de base de datos o vistas subyacente, vea el elemento EntityType (CSDL) y el elemento EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="15852-539">El **MappingFragment** puede ser un elemento secundario de EntityTypeMapping (elemento) o el elemento EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="15852-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="15852-540">El **MappingFragment** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="15852-541">ComplexType (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="15852-542">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="15852-543">Condición (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-544">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-544">Applicable Attributes</span></span>

<span data-ttu-id="15852-545">En la tabla siguiente se describe los atributos que se pueden aplicar a la **MappingFragment** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="15852-546">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-546">Attribute Name</span></span>          | <span data-ttu-id="15852-547">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-547">Is Required</span></span> | <span data-ttu-id="15852-548">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="15852-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="15852-550">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-550">Yes</span></span>         | <span data-ttu-id="15852-551">El nombre de la tabla o vista que se está asociando.</span><span class="sxs-lookup"><span data-stu-id="15852-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="15852-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="15852-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="15852-553">No</span><span class="sxs-lookup"><span data-stu-id="15852-553">No</span></span>          | <span data-ttu-id="15852-554">**True** o **False** dependiendo de si se devuelven filas distintas solo.</span><span class="sxs-lookup"><span data-stu-id="15852-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="15852-555">Si este atributo se establece en **True**, **generateupdateviews como** atributo del elemento EntityContainerMapping debe establecerse en **False**.</span><span class="sxs-lookup"><span data-stu-id="15852-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="15852-556">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-556">Example</span></span>

<span data-ttu-id="15852-557">El ejemplo siguiente se muestra un **MappingFragment** elemento como elemento secundario de un **EntityTypeMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="15852-558">En este ejemplo, las propiedades de la **curso** tipo en el modelo conceptual se asignan a columnas de la **curso** tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="15852-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="15852-559">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-559">Example</span></span>

<span data-ttu-id="15852-560">El ejemplo siguiente se muestra un **MappingFragment** elemento como elemento secundario de un **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="15852-561">Como se muestra en el ejemplo anterior, las propiedades de la **curso** tipo en el modelo conceptual se asignan a columnas de la **curso** tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="15852-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="15852-562">ModificationFunctionMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="15852-563">El **ModificationFunctionMapping** elemento en el lenguaje de especificación de (asignaciones MSL) asigna la inserción, actualización y eliminación de las funciones de un tipo de entidad del modelo conceptual a procedimientos almacenados de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="15852-564">El **ModificationFunctionMapping** elemento también puede asignar la inserción y eliminación de funciones para las asociaciones de varios a varios en el modelo conceptual a procedimientos almacenados de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="15852-565">Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="15852-566">Para obtener más información, vea el elemento de función de almacenamiento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="15852-567">Si no asigna las tres de la inserción, actualización o eliminación de las operaciones de un tipo de entidad a procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecuta en tiempo de ejecución y se produce un UpdateException.</span><span class="sxs-lookup"><span data-stu-id="15852-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="15852-568">Si las funciones de modificación para una entidad de una jerarquía de herencia están asignadas a procedimientos almacenados, entonces las funciones de modificación para todos los tipos de la jerarquía deben estar asignadas a procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="15852-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="15852-569">El **ModificationFunctionMapping** elemento puede ser un elemento secundario de EntityTypeMapping (elemento) o AssociationSetMapping (elemento).</span><span class="sxs-lookup"><span data-stu-id="15852-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="15852-570">El **ModificationFunctionMapping** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="15852-571">DeleteFunction (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="15852-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="15852-572">InsertFunction (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="15852-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="15852-573">UpdateFunction (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="15852-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="15852-574">No hay atributos son aplicables a la **ModificationFunctionMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="15852-575">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-575">Example</span></span>

<span data-ttu-id="15852-576">El ejemplo siguiente muestra la entidad de asignación del conjunto de la **personas** conjunto de entidades en el modelo School.</span><span class="sxs-lookup"><span data-stu-id="15852-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="15852-577">Además de la asignación de columna para el **persona** tipo de entidad, la asignación de la inserción, actualización y eliminación de las funciones de la **persona** se muestran el tipo.</span><span class="sxs-lookup"><span data-stu-id="15852-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="15852-578">Las funciones asignadas se declaran en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="15852-579">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-579">Example</span></span>

<span data-ttu-id="15852-580">En el ejemplo siguiente se muestra la asociación de establecer la asignación de la **CourseInstructor** conjunto de asociaciones en el modelo School.</span><span class="sxs-lookup"><span data-stu-id="15852-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="15852-581">Además de la asignación de columna para el **CourseInstructor** asociación, la asignación de las funciones de inserción y eliminación de la **CourseInstructor** asociación se muestran.</span><span class="sxs-lookup"><span data-stu-id="15852-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="15852-582">Las funciones asignadas se declaran en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="15852-583">QueryView (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="15852-584">El **QueryView** elemento en el lenguaje de especificación de (asignaciones MSL) define una asignación entre un tipo de entidad o asociación de solo lectura en el modelo conceptual y una tabla en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="15852-585">La asignación se define con una consulta de Entity SQL que se evalúa con el modelo de almacenamiento, y expresar el resultado establecido en términos de una entidad o asociación en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="15852-586">Dado que las vistas de consulta son de solo lectura, no puede utilizar los comandos de actualización estándar para actualizar los tipos que se definen mediante vistas de consulta.</span><span class="sxs-lookup"><span data-stu-id="15852-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="15852-587">Puede realizar las actualizaciones de estos tipos utilizando funciones de modificación.</span><span class="sxs-lookup"><span data-stu-id="15852-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="15852-588">Para obtener más información, consulte Cómo: asignación de funciones de modificación a procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="15852-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="15852-589">En el **QueryView** elemento, las expresiones de Entity SQL que contienen **GroupBy**, no se admiten los agregados de grupo o las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="15852-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="15852-590">El **QueryView** elemento puede ser un elemento secundario del elemento EntitySetMapping o AssociationSetMapping (elemento).</span><span class="sxs-lookup"><span data-stu-id="15852-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="15852-591">En el primer caso, la vista de consulta define una asignación de solo lectura para una entidad del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="15852-592">En el último caso, la vista de consulta define una asignación de solo lectura para una asociación del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="15852-593">Si el **AssociationSetMapping** elemento se utiliza en una asociación con una restricción referencial, el **AssociationSetMapping** se omite el elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="15852-594">Para obtener más información, vea el elemento ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="15852-595">El **QueryView** elemento no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="15852-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-596">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-596">Applicable Attributes</span></span>

<span data-ttu-id="15852-597">En la tabla siguiente se describe los atributos que se pueden aplicar a la **QueryView** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="15852-598">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-598">Attribute Name</span></span> | <span data-ttu-id="15852-599">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-599">Is Required</span></span> | <span data-ttu-id="15852-600">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="15852-601">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="15852-601">**TypeName**</span></span>   | <span data-ttu-id="15852-602">No</span><span class="sxs-lookup"><span data-stu-id="15852-602">No</span></span>          | <span data-ttu-id="15852-603">El nombre del tipo de modelo conceptual que se está asignando mediante la vista de consulta.</span><span class="sxs-lookup"><span data-stu-id="15852-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="15852-604">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-604">Example</span></span>

<span data-ttu-id="15852-605">El ejemplo siguiente se muestra el **QueryView** como elemento secundario de la **EntitySetMapping** elemento y define una asignación de la vista de consulta para el **departamento** tipo de entidad en el Modelo School.</span><span class="sxs-lookup"><span data-stu-id="15852-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="15852-606">Dado que la consulta devuelve solo un subconjunto de los miembros de la **departamento** tipo en el modelo de almacenamiento, el **departamento** tipo en el modelo School se ha modificado según esta asignación como sigue:</span><span class="sxs-lookup"><span data-stu-id="15852-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="15852-607">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-607">Example</span></span>

<span data-ttu-id="15852-608">El ejemplo siguiente se muestra el **QueryView** elemento como elemento secundario de un **AssociationSetMapping** elemento y define una asignación de solo lectura para el `FK_Course_Department` asociación del modelo School.</span><span class="sxs-lookup"><span data-stu-id="15852-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="15852-609">Comentarios</span><span class="sxs-lookup"><span data-stu-id="15852-609">Comments</span></span>

<span data-ttu-id="15852-610">Puede definir vistas de consulta para habilitar los escenarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="15852-611">Defina una entidad en el modelo conceptual que no incluya todas las propiedades de la entidad en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="15852-612">Esto incluye las propiedades que no tienen valores predeterminados y no admiten **null** valores.</span><span class="sxs-lookup"><span data-stu-id="15852-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="15852-613">Asigne las columnas calculadas del modelo de almacenamiento a las propiedades de los tipos de entidad del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="15852-614">Defina una asignación en la que las condiciones utilizadas para dividir las entidades del modelo conceptual no se basen en la igualdad.</span><span class="sxs-lookup"><span data-stu-id="15852-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="15852-615">Al especificar una asignación condicional mediante el **condición** elemento, la condición proporcionada debe igualar el valor especificado.</span><span class="sxs-lookup"><span data-stu-id="15852-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="15852-616">Para obtener más información, vea el elemento Condition (MSL).</span><span class="sxs-lookup"><span data-stu-id="15852-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="15852-617">Asigne la misma columna en el modelo de almacenamiento a varios tipos en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="15852-618">Asigne varios tipos a la misma tabla.</span><span class="sxs-lookup"><span data-stu-id="15852-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="15852-619">Defina asociaciones en el modelo conceptual que no se basen en claves externas en el esquema relacional.</span><span class="sxs-lookup"><span data-stu-id="15852-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="15852-620">Utilice la lógica de negocios personalizada para establecer el valor de las propiedades del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="15852-621">Por ejemplo, podría asignar el valor de cadena "T" en el origen de datos en un valor de **true**, un valor booleano, en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="15852-622">Defina filtros condicionales para los resultados de la consulta.</span><span class="sxs-lookup"><span data-stu-id="15852-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="15852-623">Aplique menos restricciones en los datos del modelo conceptual que en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="15852-624">Por ejemplo, puede crear una propiedad en el modelo conceptual que acepta valores NULL incluso si no es compatible con la columna a la que está asignada **null**valores.</span><span class="sxs-lookup"><span data-stu-id="15852-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="15852-625">Las consideraciones siguientes se aplican al definir las vistas de consulta para las entidades:</span><span class="sxs-lookup"><span data-stu-id="15852-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="15852-626">Las vistas de consulta son de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="15852-626">Query views are read-only.</span></span> <span data-ttu-id="15852-627">Solo puede realizar las actualizaciones a las entidades utilizando las funciones de modificación.</span><span class="sxs-lookup"><span data-stu-id="15852-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="15852-628">Al definir un tipo de entidad mediante una vista de consulta, también debe definir todas las entidades relacionadas mediante vistas de consulta.</span><span class="sxs-lookup"><span data-stu-id="15852-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="15852-629">Al asignar una asociación varios a varios a una entidad en el modelo de almacenamiento que representa una tabla de vínculos en el esquema relacional, debe definir un **QueryView** elemento en el **AssociationSetMapping** elemento para esta tabla de vínculos.</span><span class="sxs-lookup"><span data-stu-id="15852-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="15852-630">Las vistas de consulta se deben definir para todos los tipos de una jerarquía de tipos.</span><span class="sxs-lookup"><span data-stu-id="15852-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="15852-631">Puede hacer esto de las siguientes maneras:</span><span class="sxs-lookup"><span data-stu-id="15852-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="15852-632">Con una sola **QueryView** elemento que especifica una única consulta de Entity SQL que devuelve una unión de todos los tipos de entidad en la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="15852-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="15852-633">Con una sola **QueryView** elemento que especifica una única consulta de Entity SQL que utiliza el operador CASE para devolver un tipo de entidad concreto de la jerarquía según una condición específica.</span><span class="sxs-lookup"><span data-stu-id="15852-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="15852-634">Con más **QueryView** (elemento) para un tipo específico de la jerarquía.</span><span class="sxs-lookup"><span data-stu-id="15852-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="15852-635">En este caso, utilice el **TypeName** atributo de la **QueryView** elemento para especificar el tipo de entidad para cada vista.</span><span class="sxs-lookup"><span data-stu-id="15852-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="15852-636">Cuando se define una vista de consulta, no puede especificar el **StorageSetName** atributo el **EntitySetMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="15852-637">Cuando se define una vista de consulta, el **EntitySetMapping**elemento no puede contener también **propiedad** asignaciones.</span><span class="sxs-lookup"><span data-stu-id="15852-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="15852-638">ResultBinding (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="15852-639">El **ResultBinding** elemento en el lenguaje de especificación de (asignaciones MSL) asigna valores de columna que se devuelven los procedimientos almacenados las propiedades de entidad en el modelo conceptual cuando las funciones de modificación del tipo de entidad se asignan a almacenado procedimientos de la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="15852-640">Por ejemplo, cuando se devuelve el valor de una columna de identidad, insertar un procedimiento almacenado, el **ResultBinding** elemento el valor devuelto asigna a una propiedad de tipo de entidad en el modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="15852-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="15852-641">El **ResultBinding** elemento puede ser secundario del elemento InsertFunction o UpdateFunction (elemento).</span><span class="sxs-lookup"><span data-stu-id="15852-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="15852-642">El **ResultBinding** elemento no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="15852-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-643">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-643">Applicable Attributes</span></span>

<span data-ttu-id="15852-644">En la tabla siguiente se describe los atributos que se aplican a la **ResultBinding** elemento:</span><span class="sxs-lookup"><span data-stu-id="15852-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="15852-645">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-645">Attribute Name</span></span> | <span data-ttu-id="15852-646">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-646">Is Required</span></span> | <span data-ttu-id="15852-647">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="15852-648">**Name**</span><span class="sxs-lookup"><span data-stu-id="15852-648">**Name**</span></span>       | <span data-ttu-id="15852-649">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-649">Yes</span></span>         | <span data-ttu-id="15852-650">El nombre de la propiedad de entidad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="15852-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="15852-651">**ColumnName**</span></span> | <span data-ttu-id="15852-652">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-652">Yes</span></span>         | <span data-ttu-id="15852-653">El nombre de la columna que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="15852-654">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-654">Example</span></span>

<span data-ttu-id="15852-655">El siguiente ejemplo se basa en el modelo School y se muestra un **InsertFunction** elemento que se usa para asignar la función de inserción de la **persona** tipo de entidad a la **InsertPerson** procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="15852-656">(El **InsertPerson** procedimiento almacenado se muestra a continuación y se declara en el modelo de almacenamiento.) Un **ResultBinding** elemento se usa para asignar un valor de columna devuelto por el procedimiento almacenado (**NewPersonID**) a una propiedad de tipo de entidad (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="15852-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="15852-657">La instrucción Transact-SQL siguiente se describe la **InsertPerson** procedimiento almacenado:</span><span class="sxs-lookup"><span data-stu-id="15852-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="15852-658">ResultMapping (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="15852-659">El **ResultMapping** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre una importación de función en el modelo conceptual y un procedimiento almacenado en la base de datos subyacente cuando se cumple lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="15852-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="15852-660">La importación de función devuelve un tipo de entidad del modelo conceptual o un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="15852-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="15852-661">Los nombres de las columnas devueltas por el procedimiento almacenado no coinciden exactamente con los nombres de las propiedades del tipo de entidad o el tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="15852-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="15852-662">De forma predeterminada, la asignación entre las columnas devueltas por un procedimiento almacenado y un tipo de entidad o un tipo complejo se basa en los nombres de las propiedades y de las columnas.</span><span class="sxs-lookup"><span data-stu-id="15852-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="15852-663">Si los nombres de columna no coincidan con los nombres de propiedad, se debe utilizar el **ResultMapping** elemento para definir la asignación.</span><span class="sxs-lookup"><span data-stu-id="15852-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="15852-664">Para obtener un ejemplo de la asignación predeterminada, vea el elemento FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="15852-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="15852-665">El **ResultMapping** elemento es un elemento secundario del elemento FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="15852-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="15852-666">El **ResultMapping** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="15852-667">EntityTypeMapping (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="15852-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="15852-668">ComplexTypeMapping</span></span>

<span data-ttu-id="15852-669">No hay atributos son aplicables a la **ResultMapping** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="15852-670">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-670">Example</span></span>

<span data-ttu-id="15852-671">Considere el siguiente procedimiento almacenado:</span><span class="sxs-lookup"><span data-stu-id="15852-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="15852-672">Considere también el siguiente tipo de entidad del modelo conceptual:</span><span class="sxs-lookup"><span data-stu-id="15852-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="15852-673">Para crear una importación de función que devuelva instancias del tipo de entidad anterior, la asignación entre las columnas devueltas por el procedimiento almacenado y el tipo de entidad debe definirse en un **ResultMapping** elemento:</span><span class="sxs-lookup"><span data-stu-id="15852-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="15852-674">ScalarProperty (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="15852-675">El **ScalarProperty** elemento en el lenguaje de especificación de (asignaciones MSL) asigna una propiedad de un tipo de entidad del modelo conceptual, un tipo complejo o una asociación a una columna de tabla o un parámetro de procedimiento almacenado en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="15852-676">Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="15852-677">Para obtener más información, vea el elemento de función de almacenamiento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="15852-678">El **ScalarProperty** elemento puede ser un elemento secundario de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="15852-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="15852-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="15852-679">MappingFragment</span></span>
-   <span data-ttu-id="15852-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="15852-680">InsertFunction</span></span>
-   <span data-ttu-id="15852-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="15852-681">UpdateFunction</span></span>
-   <span data-ttu-id="15852-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="15852-682">DeleteFunction</span></span>
-   <span data-ttu-id="15852-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="15852-683">EndProperty</span></span>
-   <span data-ttu-id="15852-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="15852-684">ComplexProperty</span></span>
-   <span data-ttu-id="15852-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="15852-685">ResultMapping</span></span>

<span data-ttu-id="15852-686">Como elemento secundario de la **MappingFragment**, **ComplexProperty**, o **EndProperty** elemento, el **ScalarProperty** elemento asigna una propiedad en el modelo conceptual a una columna de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="15852-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="15852-687">Como elemento secundario de la **InsertFunction**, **UpdateFunction**, o **DeleteFunction** elemento, el **ScalarProperty** elemento asigna una propiedad en el modelo conceptual a un parámetro de procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="15852-688">El **ScalarProperty** elemento no puede tener elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="15852-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-689">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-689">Applicable Attributes</span></span>

<span data-ttu-id="15852-690">Los atributos que se aplican a la **ScalarProperty** elemento varían según el rol del elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="15852-691">En la tabla siguiente se describe los atributos que son aplicables cuando el **ScalarProperty** elemento se usa para asignar una propiedad de modelo conceptual a una columna en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="15852-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="15852-692">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-692">Attribute Name</span></span> | <span data-ttu-id="15852-693">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-693">Is Required</span></span> | <span data-ttu-id="15852-694">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="15852-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="15852-695">**Name**</span></span>       | <span data-ttu-id="15852-696">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-696">Yes</span></span>         | <span data-ttu-id="15852-697">El nombre de la propiedad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="15852-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="15852-698">**ColumnName**</span></span> | <span data-ttu-id="15852-699">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-699">Yes</span></span>         | <span data-ttu-id="15852-700">El nombre de la columna de tabla que se está asociando.</span><span class="sxs-lookup"><span data-stu-id="15852-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="15852-701">En la tabla siguiente se describe los atributos que se aplican a la **ScalarProperty** elemento cuando se usa para asignar una propiedad de modelo conceptual a un parámetro de procedimiento almacenado:</span><span class="sxs-lookup"><span data-stu-id="15852-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="15852-702">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-702">Attribute Name</span></span>    | <span data-ttu-id="15852-703">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-703">Is Required</span></span> | <span data-ttu-id="15852-704">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-705">**Name**</span><span class="sxs-lookup"><span data-stu-id="15852-705">**Name**</span></span>          | <span data-ttu-id="15852-706">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-706">Yes</span></span>         | <span data-ttu-id="15852-707">El nombre de la propiedad del modelo conceptual que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="15852-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="15852-708">**ParameterName**</span></span> | <span data-ttu-id="15852-709">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-709">Yes</span></span>         | <span data-ttu-id="15852-710">El nombre del parámetro que se está asignando.</span><span class="sxs-lookup"><span data-stu-id="15852-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="15852-711">**Versión**</span><span class="sxs-lookup"><span data-stu-id="15852-711">**Version**</span></span>       | <span data-ttu-id="15852-712">No</span><span class="sxs-lookup"><span data-stu-id="15852-712">No</span></span>          | <span data-ttu-id="15852-713">**Actual** o **Original** dependiendo de si se debe usar el valor actual o el valor original de la propiedad para las comprobaciones de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="15852-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="15852-714">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-714">Example</span></span>

<span data-ttu-id="15852-715">El ejemplo siguiente se muestra el **ScalarProperty** elemento que se utiliza de dos maneras:</span><span class="sxs-lookup"><span data-stu-id="15852-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="15852-716">Para asignar las propiedades de la **persona** tipo de entidad a las columnas de la **persona**tabla.</span><span class="sxs-lookup"><span data-stu-id="15852-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="15852-717">Para asignar las propiedades de la **persona** los parámetros de tipo de entidad del **UpdatePerson** procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="15852-718">Los procedimientos almacenados se declaran en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="15852-719">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-719">Example</span></span>

<span data-ttu-id="15852-720">El ejemplo siguiente se muestra el **ScalarProperty** elemento que se usa para asignar la inserción y eliminación de funciones de una asociación del modelo conceptual a procedimientos almacenados de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="15852-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="15852-721">Los procedimientos almacenados se declaran en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="15852-722">UpdateFunction (Elemento) (MSL)</span><span class="sxs-lookup"><span data-stu-id="15852-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="15852-723">El **UpdateFunction** elemento en el lenguaje de especificación de (asignaciones MSL) asigna la función de actualización de un tipo de entidad en el modelo conceptual a un procedimiento almacenado en la base de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="15852-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="15852-724">Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="15852-725">Para obtener más información, vea el elemento de función de almacenamiento (SSDL).</span><span class="sxs-lookup"><span data-stu-id="15852-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="15852-726">Si no asigna las tres de la inserción, actualización o eliminación de las operaciones de un tipo de entidad a procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecuta en tiempo de ejecución y se produce un UpdateException.</span><span class="sxs-lookup"><span data-stu-id="15852-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="15852-727">El **UpdateFunction** elemento puede ser un elemento secundario del elemento ModificationFunctionMapping y aplicado a EntityTypeMapping (elemento).</span><span class="sxs-lookup"><span data-stu-id="15852-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="15852-728">El **UpdateFunction** elemento puede tener los elementos secundarios siguientes:</span><span class="sxs-lookup"><span data-stu-id="15852-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="15852-729">AssociationEnd (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="15852-730">ComplexProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="15852-731">ResultBinding (cero o uno)</span><span class="sxs-lookup"><span data-stu-id="15852-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="15852-732">ScalarProperty (cero o más)</span><span class="sxs-lookup"><span data-stu-id="15852-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="15852-733">Atributos aplicables</span><span class="sxs-lookup"><span data-stu-id="15852-733">Applicable Attributes</span></span>

<span data-ttu-id="15852-734">En la tabla siguiente se describe los atributos que se pueden aplicar a la **UpdateFunction** elemento.</span><span class="sxs-lookup"><span data-stu-id="15852-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="15852-735">Nombre de atributo</span><span class="sxs-lookup"><span data-stu-id="15852-735">Attribute Name</span></span>            | <span data-ttu-id="15852-736">Es necesario</span><span class="sxs-lookup"><span data-stu-id="15852-736">Is Required</span></span> | <span data-ttu-id="15852-737">Valor</span><span class="sxs-lookup"><span data-stu-id="15852-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="15852-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="15852-738">**FunctionName**</span></span>          | <span data-ttu-id="15852-739">Sí</span><span class="sxs-lookup"><span data-stu-id="15852-739">Yes</span></span>         | <span data-ttu-id="15852-740">El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de actualización está asignada.</span><span class="sxs-lookup"><span data-stu-id="15852-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="15852-741">El procedimiento almacenado se debe declarar en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="15852-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="15852-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="15852-743">No</span><span class="sxs-lookup"><span data-stu-id="15852-743">No</span></span>          | <span data-ttu-id="15852-744">El nombre del parámetro de salida que devuelve el número de filas afectadas.</span><span class="sxs-lookup"><span data-stu-id="15852-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="15852-745">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="15852-745">Example</span></span>

<span data-ttu-id="15852-746">El siguiente ejemplo se basa en el modelo School y se muestra el **UpdateFunction** elemento que se usa para asignar la función de actualización de la **persona** tipo de entidad a la **UpdatePerson** procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="15852-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="15852-747">El **UpdatePerson** se declara un procedimiento almacenado en el modelo de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="15852-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
