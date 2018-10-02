---
title: Especificación de MSL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 6bff1f5407bc0546e60b5bee1178be9aa4748bd8
ms.sourcegitcommit: 29f928a6116771fe78f306846e6f2d45cbe8d1f4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460142"
---
# <a name="msl-specification"></a>Especificación MSL
Lenguaje de especificación de asignaciones (MSL) es un lenguaje basado en XML que describe la asignación entre el modelo conceptual y el modelo de almacenamiento de una aplicación de Entity Framework.

En una aplicación de Entity Framework, los metadatos de asignación se cargan desde un archivo .msl (escrito en MSL) en tiempo de compilación. Entity Framework usa los metadatos de asignación en tiempo de ejecución para traducir las consultas con el modelo conceptual en comandos específicos del almacenamiento.

El Diseñador de Entity Framework (EF Designer) almacena información de asignación en un archivo .edmx en tiempo de diseño. En tiempo de compilación, Entity Designer usa información en un archivo .edmx para crear el archivo .msl que Entity Framework necesita en tiempo de ejecución

Los nombres de todos los tipos de modelos conceptuales o de almacenamiento a los que se hace referencia en MSL deben estar calificados con sus respectivos nombres de espacios de nombres. Para obtener información sobre el nombre de espacio de nombres del modelo conceptual, vea [especificación de CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Para obtener información sobre el nombre de espacio de nombres del modelo de almacenamiento, consulte [especificación de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Las versiones de MSL se diferencian por espacios de nombres XML.

| Versión MSL | Namespace XML                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | urn: schemas-microsoft-cs |
| MSL v2      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| MSL v3      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Alias (Elemento) (MSL)

El **Alias** en el lenguaje de especificación de (asignaciones MSL) es un elemento secundario del elemento de asignación que se utiliza para definir los alias de espacios de nombres modelo almacenamiento y el modelo conceptual. Los nombres de todos los tipos de modelos conceptuales o de almacenamiento a los que se hace referencia en MSL deben estar calificados con sus respectivos nombres de espacios de nombres. Para obtener información sobre el nombre de espacio de nombres del modelo conceptual, vea el elemento de esquema (CSDL). Para obtener información sobre el nombre de espacio de nombres del modelo de almacenamiento, vea el elemento de esquema de almacenamiento (SSDL).

El **Alias** elemento no puede tener elementos secundarios.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **Alias** elemento.

| Nombre de atributo | Es necesario | Valor                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Key**        | Sí         | El alias del espacio de nombres especificado por el **valor** atributo. |
| **Valor**      | Sí         | El espacio de nombres para el que el valor de la **clave** elemento es un alias.     |

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **Alias** elemento que define un alias, `c`, para los tipos que se definen en el modelo conceptual.

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

## <a name="associationend-element-msl"></a>AssociationEnd (Elemento) (MSL)

El **AssociationEnd** elemento en el lenguaje de especificación de (asignaciones MSL) se usa cuando las funciones de modificación de un tipo de entidad en el modelo conceptual se asignan a los procedimientos almacenados en la base de datos subyacente. Si una modificación de procedimiento almacenado toma un parámetro cuyo valor se mantiene en una propiedad de asociación, el **AssociationEnd** elemento asigna el valor de propiedad para el parámetro. Para obtener más información, vea el ejemplo siguiente.

Para obtener más información sobre cómo asignar funciones de modificación de tipos de entidad a procedimientos almacenados, vea el elemento ModificationFunctionMapping (MSL) y Tutorial: asignar una entidad a procedimientos almacenados.

El **AssociationEnd** elemento puede tener los elementos secundarios siguientes:

-   ScalarProperty

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se aplican a la **AssociationEnd** elemento.

| Nombre de atributo     | Es necesario | Valor                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | Sí         | El nombre de la asociación que se está asignando.                                                                                                                                 |
| **From**           | Sí         | El valor de la **FromRole** atributo de la propiedad de navegación que corresponde a la asociación que se está asigna. Para obtener más información, vea el elemento NavigationProperty (CSDL). |
| **En**             | Sí         | El valor de la **ToRole** atributo de la propiedad de navegación que corresponde a la asociación que se está asigna. Para obtener más información, vea el elemento NavigationProperty (CSDL).   |

### <a name="example"></a>Ejemplo

Considere el siguiente tipo de entidad del modelo conceptual:

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

Considere también el siguiente procedimiento almacenado:

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

Para asignar la función de actualización de la `Course` entidad a este procedimiento almacenado, debe proporcionar un valor para el **DepartmentID** parámetro. El valor para `DepartmentID` no corresponde a una propiedad del tipo de entidad; está contenido en una asociación independiente cuya asignación se muestra aquí:

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

El siguiente código muestra la **AssociationEnd** elemento que se usa para asignar el **DepartmentID** propiedad de la **FK\_curso\_departamento** asociación a la **UpdateCourse** procedimiento almacenado (a la que la función de actualización de la **curso** se asigna el tipo de entidad):

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

## <a name="associationsetmapping-element-msl"></a>AssociationSetMapping (Elemento) (MSL)

El **AssociationSetMapping** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre una asociación de las columnas de tabla y modelo conceptuales en la base de datos subyacente.

Las asociaciones del modelo conceptual son tipos cuyas propiedades representan columnas de clave primaria y clave externa de la base de datos subyacente. El **AssociationSetMapping** elemento utiliza dos elementos de EndProperty para definir las asignaciones entre las propiedades de tipo de asociación y columnas en la base de datos. Puede definir condiciones en estas asignaciones con el elemento Condition. Asignar el insert, update y delete de funciones para las asociaciones a procedimientos almacenados en la base de datos con el elemento ModificationFunctionMapping. Definir asignaciones de solo lectura entre las asociaciones y columnas de la tabla mediante una cadena de Entity SQL en un QueryView (elemento).

> [!NOTE]
> Si se define una restricción referencial para una asociación en el modelo conceptual, la asociación no es necesario que debe asignarse un **AssociationSetMapping** elemento. Si un **AssociationSetMapping** elemento está presente para una asociación que tiene una restricción referencial, las asignaciones definidas en el **AssociationSetMapping** se omitirá el elemento. Para obtener más información, vea el elemento ReferentialConstraint (CSDL).

El **AssociationSetMapping** elemento puede tener los siguientes elementos secundarios

-   QueryView (cero o uno)
-   EndProperty (cero o dos)
-   Condición (cero o más)
-   ModificationFunctionMapping (cero o uno)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **AssociationSetMapping** elemento.

| Nombre de atributo     | Es necesario | Valor                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Name**           | Sí         | El nombre del conjunto de asociaciones del modelo conceptual que se está asignando.                      |
| **TypeName**       | No          | El nombre completo, calificado con el espacio de nombres, del tipo de asociación del modelo conceptual que se está asignando. |
| **StoreEntitySet** | No          | El nombre de la tabla que se está asignando.                                                 |

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **AssociationSetMapping** elemento en el que el **FK\_curso\_departamento** conjunto de asociaciones en el modelo conceptual se asignan a la  **Curso** tabla en la base de datos. Las asignaciones entre columnas de tabla y las propiedades de tipo de asociación se especifican en el secundario **EndProperty** elementos.

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

## <a name="complexproperty-element-msl"></a>ComplexProperty (Elemento) (MSL)

Un **ComplexProperty** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre una propiedad de tipo complejo en un modelo conceptual entidad tipo y columnas de tabla en la base de datos subyacente. Las asignaciones de columnas y propiedades se especifican en los elementos ScalarProperty secundarios.

El **ComplexType** elemento de propiedad puede tener los elementos secundarios siguientes:

-   ScalarProperty (cero o más)
-   **ComplexProperty** (cero o más)
-   ComplextTypeMapping (cero o más)
-   Condición (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se aplican a la **ComplexProperty** elemento:

| Nombre de atributo | Es necesario | Valor                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**       | Sí         | El nombre de la propiedad compleja de un tipo de entidad del modelo conceptual que se está asignando. |
| **TypeName**   | No          | El nombre completo, calificado con el espacio de nombres, del tipo de propiedad del modelo conceptual.                              |

### <a name="example"></a>Ejemplo

En el siguiente ejemplo se basa en el modelo School. El siguiente tipo complejo se ha agregado al modelo conceptual:

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

El **LastName** y **FirstName** propiedades de la **persona** tipo de entidad se han reemplazado por una propiedad compleja, **nombre**:

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

El siguiente MSL muestra la **ComplexProperty** elemento que se usa para asignar el **nombre** propiedad a las columnas de la base de datos subyacente:

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

## <a name="complextypemapping-element-msl"></a>ComplexTypeMapping (Elemento) (MSL)

El **ComplexTypeMapping** elemento en el lenguaje de especificación de (asignaciones MSL) es un elemento secundario del elemento ResultMapping y define la asignación entre una importación de función en el modelo conceptual y un procedimiento almacenado de subyacente base de datos cuando se cumple lo siguiente:

-   La importación de función devuelve un tipo complejo conceptual.
-   Los nombres de las columnas devueltas por el procedimiento almacenado no coinciden exactamente con los nombres de las propiedades en el tipo complejo.

De forma predeterminada, la asignación entre las columnas devueltas por un procedimiento almacenado y un tipo complejo se basa en los nombres de las propiedades y de las columnas. Si los nombres de columna no coincidan con los nombres de propiedad, se debe utilizar el **ComplexTypeMapping** elemento para definir la asignación. Para obtener un ejemplo de la asignación predeterminada, vea el elemento FunctionImportMapping (MSL).

El **ComplexTypeMapping** elemento puede tener los elementos secundarios siguientes:

-   ScalarProperty (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se aplican a la **ComplexTypeMapping** elemento.

| Nombre de atributo | Es necesario | Valor                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **TypeName**   | Sí         | El nombre completo, incluido el espacio de nombres, del tipo complejo que se está asignando. |

### <a name="example"></a>Ejemplo

Considere el siguiente procedimiento almacenado:

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

Considere también el siguiente tipo complejo del modelo conceptual:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Para crear una importación de función que devuelva instancias del tipo complejo anterior, la asignación entre las columnas devueltas por el procedimiento almacenado y el tipo de entidad debe definirse en un **ComplexTypeMapping** elemento:

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

## <a name="condition-element-msl"></a>Condition (Elemento) (MSL)

El **condición** elemento en el lenguaje de especificación de (asignaciones MSL) establece condiciones en las asignaciones entre el modelo conceptual y la base de datos subyacente. La asignación que se define dentro de un nodo XML es válidas si todas las condiciones, como elemento secundario especificado en **condición** elementos, se cumplen. De lo contrario, la asignación no es válida. Por ejemplo, si un elemento MappingFragment contiene uno o varios **condición** elementos secundarios, la asignación definida dentro del **MappingFragment** nodo solo serán válido si todas las condiciones del elemento secundario  **Condición** se cumplen los elementos.

Cada condición se puede aplicar a un **nombre** (el nombre de una propiedad de entidad del modelo conceptual, especificada por el **nombre** atributo), o un **ColumnName** (el nombre de una columna de la base de datos, especificado por el **ColumnName** atributo). Cuando el **nombre** atributo está establecido, la condición se comprueba contra un valor de propiedad de entidad. Cuando el **ColumnName** atributo está establecido, la condición se comprueba contra un valor de columna. Solo uno de los **nombre** o **ColumnName** atributo puede especificarse en un **condición** elemento.

> [!NOTE]
> Cuando el **condición** elemento se usa dentro de un elemento FunctionImportMapping, sólo el **nombre** atributo no es aplicable.

El **condición** elemento puede ser un elemento secundario de los siguientes elementos:

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

El **condición** elemento no puede tener ningún elemento secundario.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se aplican a la **condición** elemento:

| Nombre de atributo | Es necesario | Valor                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ColumnName** | No          | El nombre de la columna de la tabla cuyo valor se utiliza para evaluar la condición.                                                                                                                                                                                                                   |
| **IsNull**     | No          | **True** o **False**. Si el valor es **True** y el valor de columna es **null**, o si el valor es **False** y el valor de columna no es **null**, la condición es true . De lo contrario, la condición es falsa. <br/> El **IsNull** y **valor** atributos no se puede usar al mismo tiempo. |
| **Valor**      | No          | El valor con el que se compara el valor de la columna. Si los valores son los mismos, la condición es verdadera. De lo contrario, la condición es falsa. <br/> El **IsNull** y **valor** atributos no se puede usar al mismo tiempo.                                                                       |
| **Name**       | No          | El nombre de la propiedad de entidad del modelo conceptual cuyo valor se utiliza para evaluar la condición. <br/> Este atributo no es aplicable si la **condición** elemento se usa dentro de un elemento FunctionImportMapping.                                                                           |

### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra **condición** elementos como elementos secundarios de **MappingFragment** elementos. Cuando **HireDate** no es null y **EnrollmentDate** es null, se asignan los datos entre el **SchoolModel.Instructor** tipo y el **PersonID**y **HireDate** columnas de la **persona** tabla. Cuando **EnrollmentDate** no es null y **HireDate** es null, se asignan los datos entre el **SchoolModel.Student** tipo y el **PersonID** y **inscripción** columnas de la **persona** tabla.

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

## <a name="deletefunction-element-msl"></a>DeleteFunction (Elemento) (MSL)

El **DeleteFunction** elemento en el lenguaje de especificación de (asignaciones MSL) asigna la función de eliminación de un tipo de entidad o asociación en el modelo conceptual a un procedimiento almacenado en la base de datos subyacente. Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento. Para obtener más información, vea el elemento de función de almacenamiento (SSDL).

> [!NOTE]
> Si no asigna las tres de la inserción, actualización o eliminación de las operaciones de un tipo de entidad a procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecuta en tiempo de ejecución y se produce un UpdateException.

### <a name="deletefunction-applied-to-entitytypemapping"></a>DeleteFunction aplicado a EntityTypeMapping

Cuando se aplica al elemento EntityTypeMapping, el **DeleteFunction** elemento la función de eliminación de un tipo de entidad en el modelo conceptual asigna a un procedimiento almacenado.

El **DeleteFunction** elemento puede tener los elementos secundarios siguientes cuando se aplica a un **EntityTypeMapping** elemento:

-   AssociationEnd (cero o más)
-   ComplexProperty (cero o más)
-   ScalarProperty (cero o más)

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **DeleteFunction** elemento cuando se aplica a un **EntityTypeMapping** elemento.

| Nombre de atributo            | Es necesario | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sí         | El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de eliminación está asignada. El procedimiento almacenado se debe declarar en el modelo de almacenamiento. |
| **RowsAffectedParameter** | No          | El nombre del parámetro de salida que devuelve el número de filas afectadas.                                                                               |

#### <a name="example"></a>Ejemplo

El siguiente ejemplo se basa en el modelo School y se muestra el **DeleteFunction** elemento de asignación de la función de eliminación de la **persona** tipo de entidad a la **DeletePerson** procedimiento almacenado. El **DeletePerson** se declara un procedimiento almacenado en el modelo de almacenamiento.

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

### <a name="deletefunction-applied-to-associationsetmapping"></a>DeleteFunction aplicado a AssociationSetMapping

Cuando se aplica al elemento AssociationSetMapping, el **DeleteFunction** elemento la función de eliminación de una asociación en el modelo conceptual asigna a un procedimiento almacenado.

El **DeleteFunction** elemento puede tener los elementos secundarios siguientes cuando se aplica a la **AssociationSetMapping** elemento:

-   EndProperty

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **DeleteFunction** elemento cuando se aplica a la **AssociationSetMapping** elemento.

| Nombre de atributo            | Es necesario | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sí         | El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de eliminación está asignada. El procedimiento almacenado se debe declarar en el modelo de almacenamiento. |
| **RowsAffectedParameter** | No          | El nombre del parámetro de salida que devuelve el número de filas afectadas.                                                                               |

#### <a name="example"></a>Ejemplo

El siguiente ejemplo se basa en el modelo School y se muestra el **DeleteFunction** elemento que se usa para asignar la función de eliminación de la **CourseInstructor** asociación a la  **DeleteCourseInstructor** procedimiento almacenado. El **DeleteCourseInstructor** se declara un procedimiento almacenado en el modelo de almacenamiento.

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

## <a name="endproperty-element-msl"></a>EndProperty (Elemento) (MSL)

El **EndProperty** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre un extremo o una función de modificación de una asociación del modelo conceptual y la base de datos subyacente. La asignación de columnas de la propiedad se especifica en un elemento de ScalarProperty secundario.

Cuando un **EndProperty** elemento se usa para definir la asignación para el extremo de asociación de un modelo conceptual, es un elemento secundario de un elemento AssociationSetMapping. Cuando el **EndProperty** elemento se usa para definir la asignación para una función de modificación de una asociación del modelo conceptual, es un elemento secundario de un elemento InsertFunction o DeleteFunction (elemento).

El **EndProperty** elemento puede tener los elementos secundarios siguientes:

-   ScalarProperty (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se aplican a la **EndProperty** elemento:

| Nombre de atributo | Es necesario | Valor                                                 |
|:---------------|:------------|:------------------------------------------------------|
| nombre           | Sí         | El nombre del extremo de la asociación que se está asignando. |

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **AssociationSetMapping** elemento en el que el **FK\_curso\_departamento** asociación del modelo conceptual se asigna a la **Curso** tabla en la base de datos. Las asignaciones entre columnas de tabla y las propiedades de tipo de asociación se especifican en el secundario **EndProperty** elementos.

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

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra el **EndProperty** las funciones de inserción y eliminación de una asociación de asignación de elemento (**CourseInstructor**) a los procedimientos almacenados en la base de datos subyacente. Las funciones asignadas se declaran en el modelo de almacenamiento.

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

## <a name="entitycontainermapping-element-msl"></a>EntityContainerMapping (Elemento) (MSL)

El **EntityContainerMapping** elemento en el lenguaje de especificación de (asignaciones MSL) asigna el contenedor de entidades en el modelo conceptual al contenedor de entidades del modelo de almacenamiento. El **EntityContainerMapping** es un elemento secundario del elemento de asignación.

El **EntityContainerMapping** elemento puede tener los elementos secundarios siguientes (en el orden mostrado):

-   EntitySetMapping (cero o más)
-   AssociationSetMapping (cero o más)
-   FunctionImportMapping (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntityContainerMapping** elemento.

| Nombre de atributo            | Es necesario | Valor                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Sí         | El nombre del contenedor de entidades del modelo de almacenamiento que se está asignando.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Sí         | El nombre del contenedor de entidades del modelo conceptual que se está asignando.                                                                                                                                                                                  |
| **Generateupdateviews como**   | No          | **True** o **False**. Si **False**, no hay ninguna vista de actualización se genera. Este atributo debe establecerse en **False** cuando haya una asignación de solo lectura que podría no ser válida porque los datos pueden no ida y vuelta correctamente. <br/> El valor predeterminado es **True**. |

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntityContainerMapping** elemento que se asigna el **SchoolModelEntities** contenedor (el contenedor de entidades del modelo conceptual) a la  **SchoolModelStoreContainer** contenedor (el contenedor de entidades del modelo de almacenamiento):

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

## <a name="entitysetmapping-element-msl"></a>EntitySetMapping (Elemento) (MSL)

El **EntitySetMapping** establece el elemento en la especificación de lenguaje (MSL) asigna todos los tipos de entidades del modelo conceptual se establecen en la entidad del modelo de almacenamiento. Un conjunto de entidades en el modelo conceptual es un contenedor lógico para instancias de entidades del mismo tipo (y los tipos derivados). Un conjunto de entidades en el modelo de almacenamiento representa una tabla o vista en la base de datos subyacente. El conjunto de entidades del modelo conceptual especificado por el valor de la **nombre** atributo de la **EntitySetMapping** elemento. La asigna a la tabla o vista es especificada por el **StoreEntitySet** atributo en cada MappingFragment, elemento secundario o en el **EntitySetMapping** propio elemento.

El **EntitySetMapping** elemento puede tener los elementos secundarios siguientes:

-   EntityTypeMapping (cero o más)
-   QueryView (cero o uno)
-   MappingFragment (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntitySetMapping** elemento.

| Nombre de atributo           | Es necesario | Valor                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                 | Sí         | El nombre del conjunto de entidades del modelo conceptual que se está asignando.                                                                                                                                                             |
| **TypeName** **1**       | No          | El nombre del tipo de entidad del modelo conceptual que se está asignando.                                                                                                                                                            |
| **StoreEntitySet** **1** | No          | El nombre del conjunto de entidades del modelo de almacenamiento al que se está asignando.                                                                                                                                                             |
| **MakeColumnsDistinct**  | No          | **True** o **False** dependiendo de si se devuelven filas distintas solo. <br/> Si este atributo se establece en **True**, **generateupdateviews como** atributo del elemento EntityContainerMapping debe establecerse en **False**. |

 

**1** el **TypeName** y **StoreEntitySet** atributos se pueden usar en lugar de los elementos secundarios EntityTypeMapping y MappingFragment para asignar un tipo de entidad única a una sola tabla.

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **EntitySetMapping** elemento que asigna tres tipos (un tipo base y dos tipos derivados) en el **cursos** conjunto de entidades del modelo conceptual a tres tablas diferentes en el base de datos subyacente. Las tablas se especifican mediante el **StoreEntitySet** atributo en cada **MappingFragment** elemento.

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

## <a name="entitytypemapping-element-msl"></a>EntityTypeMapping (Elemento) (MSL)

El **EntityTypeMapping** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre un tipo de entidad en el modelo conceptual y las tablas o vistas en la base de datos subyacente. Para obtener información sobre los tipos de entidad del modelo conceptual y las tablas de base de datos o vistas subyacente, vea el elemento EntityType (CSDL) y el elemento EntitySet (SSDL). El tipo de entidad del modelo conceptual que se está asignando especificado por el **TypeName** atributo de la **EntityTypeMapping** elemento. Especifica la tabla o vista que se está asignando el **StoreEntitySet** atributo del elemento MappingFragment secundario.

El ModificationFunctionMapping elemento secundario se puede usar para asignar la inserción, actualización o eliminación de funciones de tipos de entidad a procedimientos almacenados de la base de datos.

El **EntityTypeMapping** elemento puede tener los elementos secundarios siguientes:

-   MappingFragment (cero o más)
-   ModificationFunctionMapping (cero o uno)
-   ScalarProperty
-   Condición

> [!NOTE]
> **MappingFragment** y **ModificationFunctionMapping** elementos no pueden ser elementos secundarios de la **EntityTypeMapping** elemento al mismo tiempo.


> [!NOTE]
> El **ScalarProperty** y **condición** elementos sólo pueden ser elementos secundarios de la **EntityTypeMapping** elemento cuando se utiliza dentro de un elemento FunctionImportMapping.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **EntityTypeMapping** elemento.

| Nombre de atributo | Es necesario | Valor                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **TypeName**   | Sí         | El nombre completo, calificado con el espacio de nombres, del tipo de entidad del modelo conceptual que se está asignando. <br/> Si el tipo es abstracto o un tipo derivado, el valor debe ser `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra un elemento EntitySetMapping con dos secundarios **EntityTypeMapping** elementos. En la primera **EntityTypeMapping** elemento, el **SchoolModel.Person** tipo de entidad se asigna a la **persona** tabla. En el segundo **EntityTypeMapping** elemento, la funcionalidad de actualización de la **SchoolModel.Person** tipo se asigna a un procedimiento almacenado, **UpdatePerson**, en la base de datos .

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

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra la asignación de una jerarquía de tipos en la que el tipo raíz es abstracto. Tenga en cuenta el uso de la `IsOfType` sintaxis para el **TypeName** atributos.

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

## <a name="functionimportmapping-element-msl"></a>FunctionImportMapping (Elemento) (MSL)

El **FunctionImportMapping** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre una importación de función en el modelo conceptual y un procedimiento almacenado o función en la base de datos subyacente. Las importaciones de función se deben declarar en el modelo conceptual, mientras que los procedimientos almacenados se deben declarar en el modelo de almacenamiento. Para obtener más información, vea el elemento FunctionImport (CSDL) y el elemento de función de almacenamiento (SSDL).

> [!NOTE]
> De forma predeterminada, si una importación de función devuelve un tipo de entidad del modelo conceptual o un tipo complejo, entonces los nombres de las columnas devueltas por el procedimiento almacenado subyacente deben coincidir exactamente con los nombres de las propiedades del tipo del modelo conceptual. Si los nombres de columna no coincidan con los nombres de propiedad, la asignación debe definirse en un elemento ResultMapping.

El **FunctionImportMapping** elemento puede tener los elementos secundarios siguientes:

-   ResultMapping (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se aplican a la **FunctionImportMapping** elemento:

| Nombre de atributo         | Es necesario | Valor                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | Sí         | El nombre de la importación de función del modelo conceptual que se está asignando.           |
| **FunctionName**       | Sí         | El nombre, calificado con el espacio de nombres, de la función del modelo de almacenamiento que se está asignando. |

### <a name="example"></a>Ejemplo

En el siguiente ejemplo se basa en el modelo School. Considere la siguiente función en el modelo de almacenamiento:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

También considere esta importación de función en el modelo conceptual:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

El siguiente ejemplo muestra un **FunctionImportMapping** elemento que se usa para asignar la función y la importación de función anteriores entre sí:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>InsertFunction (Elemento) (MSL)

El **InsertFunction** elemento en el lenguaje de especificación de (asignaciones MSL) asigna la función de inserción de un tipo de entidad o asociación en el modelo conceptual a un procedimiento almacenado en la base de datos subyacente. Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento. Para obtener más información, vea el elemento de función de almacenamiento (SSDL).

> [!NOTE]
> Si no asigna las tres de la inserción, actualización o eliminación de las operaciones de un tipo de entidad a procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecuta en tiempo de ejecución y se produce un UpdateException.

El **InsertFunction** elemento puede ser un elemento secundario del elemento ModificationFunctionMapping y aplicado a EntityTypeMapping (elemento) o AssociationSetMapping (elemento).

### <a name="insertfunction-applied-to-entitytypemapping"></a>InsertFunction aplicado a EntityTypeMapping

Cuando se aplica al elemento EntityTypeMapping, el **InsertFunction** elemento la función de inserción de un tipo de entidad en el modelo conceptual asigna a un procedimiento almacenado.

El **InsertFunction** elemento puede tener los elementos secundarios siguientes cuando se aplica a un **EntityTypeMapping** elemento:

-   AssociationEnd (cero o más)
-   ComplexProperty (cero o más)
-   ResultBinding (cero o uno)
-   ScalarProperty (cero o más)

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **InsertFunction** elemento cuando se aplica a un **EntityTypeMapping** elemento.

| Nombre de atributo            | Es necesario | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sí         | El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de inserción está asignada. El procedimiento almacenado se debe declarar en el modelo de almacenamiento. |
| **RowsAffectedParameter** | No          | El nombre del parámetro de salida que devuelve el número de filas afectadas.                                                                               |

#### <a name="example"></a>Ejemplo

El siguiente ejemplo se basa en el modelo School y se muestra el **InsertFunction** elemento que se usa para asignar la función de inserción del tipo de entidad Person a la **InsertPerson** procedimiento almacenado. El **InsertPerson** se declara un procedimiento almacenado en el modelo de almacenamiento.

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
### <a name="insertfunction-applied-to-associationsetmapping"></a>InsertFunction aplicado a AssociationSetMapping

Cuando se aplica al elemento AssociationSetMapping, el **InsertFunction** elemento la función de inserción de una asociación en el modelo conceptual asigna a un procedimiento almacenado.

El **InsertFunction** elemento puede tener los elementos secundarios siguientes cuando se aplica a la **AssociationSetMapping** elemento:

-   EndProperty

#### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **InsertFunction** elemento cuando se aplica a la **AssociationSetMapping** elemento.

| Nombre de atributo            | Es necesario | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sí         | El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de inserción está asignada. El procedimiento almacenado se debe declarar en el modelo de almacenamiento. |
| **RowsAffectedParameter** | No          | El nombre del parámetro de salida que devuelve el número de filas afectadas.                                                                               |

#### <a name="example"></a>Ejemplo

El siguiente ejemplo se basa en el modelo School y se muestra el **InsertFunction** elemento que se usa para asignar la función de inserción de la **CourseInstructor** asociación a la  **InsertCourseInstructor** procedimiento almacenado. El **InsertCourseInstructor** se declara un procedimiento almacenado en el modelo de almacenamiento.

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

## <a name="mapping-element-msl"></a>Mapping (Elemento) (MSL)

El **asignación** elemento en el lenguaje de especificación de (asignaciones MSL) contiene información para la asignación de objetos que se definen en un modelo conceptual a una base de datos (como se describe en un modelo de almacenamiento). Para obtener más información, vea la especificación de CSDL y SSDL especificación.

El **asignación** es el elemento raíz de una especificación de asignaciones. El espacio de nombres XML para la asignación de las especificaciones http://schemas.microsoft.com/ado/2009/11/mapping/cs.

El elemento de asignación puede tener los elementos secundarios siguientes (en el orden mostrado):

-   Alias (cero o más)
-   EntityContainerMapping (exactamente uno)

Los nombres de los tipos de modelos conceptuales y de almacenamiento a los que se hace referencia en MSL deben estar calificados con sus respectivos nombres de espacios de nombres. Para obtener información sobre el nombre de espacio de nombres del modelo conceptual, vea el elemento de esquema (CSDL). Para obtener información sobre el nombre de espacio de nombres del modelo de almacenamiento, vea el elemento de esquema de almacenamiento (SSDL). Alias de espacios de nombres que se utilizan en MSL se pueden definir con el elemento de Alias.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **asignación** elemento.

| Nombre de atributo | Es necesario | Valor                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **Espacio**      | Sí         | **C-S**. Este es un valor fijo y no se puede cambiar. |

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **asignación** elemento que se basa en parte del modelo School. Para obtener más información sobre el modelo School, vea la Guía de inicio rápido (Entity Framework):

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

## <a name="mappingfragment-element-msl"></a>MappingFragment (Elemento) (MSL)

El **MappingFragment** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre las propiedades de un tipo de entidad del modelo conceptual y una tabla o vista en la base de datos. Para obtener información sobre los tipos de entidad del modelo conceptual y las tablas de base de datos o vistas subyacente, vea el elemento EntityType (CSDL) y el elemento EntitySet (SSDL). El **MappingFragment** puede ser un elemento secundario de EntityTypeMapping (elemento) o el elemento EntitySetMapping.

El **MappingFragment** elemento puede tener los elementos secundarios siguientes:

-   ComplexType (cero o más)
-   ScalarProperty (cero o más)
-   Condición (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **MappingFragment** elemento.

| Nombre de atributo          | Es necesario | Valor                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Sí         | El nombre de la tabla o vista que se está asociando.                                                                                                                                                                           |
| **MakeColumnsDistinct** | No          | **True** o **False** dependiendo de si se devuelven filas distintas solo. <br/> Si este atributo se establece en **True**, **generateupdateviews como** atributo del elemento EntityContainerMapping debe establecerse en **False**. |

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **MappingFragment** elemento como elemento secundario de un **EntityTypeMapping** elemento. En este ejemplo, las propiedades de la **curso** tipo en el modelo conceptual se asignan a columnas de la **curso** tabla en la base de datos.

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

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra un **MappingFragment** elemento como elemento secundario de un **EntitySetMapping** elemento. Como se muestra en el ejemplo anterior, las propiedades de la **curso** tipo en el modelo conceptual se asignan a columnas de la **curso** tabla en la base de datos.

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

## <a name="modificationfunctionmapping-element-msl"></a>ModificationFunctionMapping (Elemento) (MSL)

El **ModificationFunctionMapping** elemento en el lenguaje de especificación de (asignaciones MSL) asigna la inserción, actualización y eliminación de las funciones de un tipo de entidad del modelo conceptual a procedimientos almacenados de la base de datos subyacente. El **ModificationFunctionMapping** elemento también puede asignar la inserción y eliminación de funciones para las asociaciones de varios a varios en el modelo conceptual a procedimientos almacenados de la base de datos subyacente. Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento. Para obtener más información, vea el elemento de función de almacenamiento (SSDL).

> [!NOTE]
> Si no asigna las tres de la inserción, actualización o eliminación de las operaciones de un tipo de entidad a procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecuta en tiempo de ejecución y se produce un UpdateException.


> [!NOTE]
> Si las funciones de modificación para una entidad de una jerarquía de herencia están asignadas a procedimientos almacenados, entonces las funciones de modificación para todos los tipos de la jerarquía deben estar asignadas a procedimientos almacenados.

El **ModificationFunctionMapping** elemento puede ser un elemento secundario de EntityTypeMapping (elemento) o AssociationSetMapping (elemento).

El **ModificationFunctionMapping** elemento puede tener los elementos secundarios siguientes:

-   DeleteFunction (cero o uno)
-   InsertFunction (cero o uno)
-   UpdateFunction (cero o uno)

No hay atributos son aplicables a la **ModificationFunctionMapping** elemento.

### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra la entidad de asignación del conjunto de la **personas** conjunto de entidades en el modelo School. Además de la asignación de columna para el **persona** tipo de entidad, la asignación de la inserción, actualización y eliminación de las funciones de la **persona** se muestran el tipo. Las funciones asignadas se declaran en el modelo de almacenamiento.

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

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra la asociación de establecer la asignación de la **CourseInstructor** conjunto de asociaciones en el modelo School. Además de la asignación de columna para el **CourseInstructor** asociación, la asignación de las funciones de inserción y eliminación de la **CourseInstructor** asociación se muestran. Las funciones asignadas se declaran en el modelo de almacenamiento.

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
 

 

## <a name="queryview-element-msl"></a>QueryView (Elemento) (MSL)

El **QueryView** elemento en el lenguaje de especificación de (asignaciones MSL) define una asignación entre un tipo de entidad o asociación de solo lectura en el modelo conceptual y una tabla en la base de datos subyacente. La asignación se define con una consulta de Entity SQL que se evalúa con el modelo de almacenamiento, y expresar el resultado establecido en términos de una entidad o asociación en el modelo conceptual. Dado que las vistas de consulta son de solo lectura, no puede utilizar los comandos de actualización estándar para actualizar los tipos que se definen mediante vistas de consulta. Puede realizar las actualizaciones de estos tipos utilizando funciones de modificación. Para obtener más información, consulte Cómo: asignación de funciones de modificación a procedimientos almacenados.

> [!NOTE]
> En el **QueryView** elemento, las expresiones de Entity SQL que contienen **GroupBy**, no se admiten los agregados de grupo o las propiedades de navegación.

 

El **QueryView** elemento puede ser un elemento secundario del elemento EntitySetMapping o AssociationSetMapping (elemento). En el primer caso, la vista de consulta define una asignación de solo lectura para una entidad del modelo conceptual. En el último caso, la vista de consulta define una asignación de solo lectura para una asociación del modelo conceptual.

> [!NOTE]
> Si el **AssociationSetMapping** elemento se utiliza en una asociación con una restricción referencial, el **AssociationSetMapping** se omite el elemento. Para obtener más información, vea el elemento ReferentialConstraint (CSDL).

El **QueryView** elemento no puede tener elementos secundarios.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **QueryView** elemento.

| Nombre de atributo | Es necesario | Valor                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **TypeName**   | No          | El nombre del tipo de modelo conceptual que se está asignando mediante la vista de consulta. |

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra el **QueryView** como elemento secundario de la **EntitySetMapping** elemento y define una asignación de la vista de consulta para el **departamento** tipo de entidad en el Modelo School.

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

Dado que la consulta devuelve solo un subconjunto de los miembros de la **departamento** tipo en el modelo de almacenamiento, el **departamento** tipo en el modelo School se ha modificado según esta asignación como sigue:

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

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra el **QueryView** elemento como elemento secundario de un **AssociationSetMapping** elemento y define una asignación de solo lectura para el `FK_Course_Department` asociación del modelo School.

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
 
### <a name="comments"></a>Comentarios

Puede definir vistas de consulta para habilitar los escenarios siguientes:

-   Defina una entidad en el modelo conceptual que no incluya todas las propiedades de la entidad en el modelo de almacenamiento. Esto incluye las propiedades que no tienen valores predeterminados y no admiten **null** valores.
-   Asigne las columnas calculadas del modelo de almacenamiento a las propiedades de los tipos de entidad del modelo conceptual.
-   Defina una asignación en la que las condiciones utilizadas para dividir las entidades del modelo conceptual no se basen en la igualdad. Al especificar una asignación condicional mediante el **condición** elemento, la condición proporcionada debe igualar el valor especificado. Para obtener más información, vea el elemento Condition (MSL).
-   Asigne la misma columna en el modelo de almacenamiento a varios tipos en el modelo conceptual.
-   Asigne varios tipos a la misma tabla.
-   Defina asociaciones en el modelo conceptual que no se basen en claves externas en el esquema relacional.
-   Utilice la lógica de negocios personalizada para establecer el valor de las propiedades del modelo conceptual. Por ejemplo, podría asignar el valor de cadena "T" en el origen de datos en un valor de **true**, un valor booleano, en el modelo conceptual.
-   Defina filtros condicionales para los resultados de la consulta.
-   Aplique menos restricciones en los datos del modelo conceptual que en el modelo de almacenamiento. Por ejemplo, puede crear una propiedad en el modelo conceptual que acepta valores NULL incluso si no es compatible con la columna a la que está asignada **null**valores.

Las consideraciones siguientes se aplican al definir las vistas de consulta para las entidades:

-   Las vistas de consulta son de solo lectura. Solo puede realizar las actualizaciones a las entidades utilizando las funciones de modificación.
-   Al definir un tipo de entidad mediante una vista de consulta, también debe definir todas las entidades relacionadas mediante vistas de consulta.
-   Al asignar una asociación varios a varios a una entidad en el modelo de almacenamiento que representa una tabla de vínculos en el esquema relacional, debe definir un **QueryView** elemento en el **AssociationSetMapping** elemento para esta tabla de vínculos.
-   Las vistas de consulta se deben definir para todos los tipos de una jerarquía de tipos. Puede hacer esto de las siguientes maneras:
-   -   Con una sola **QueryView** elemento que especifica una única consulta de Entity SQL que devuelve una unión de todos los tipos de entidad en la jerarquía.
    -   Con una sola **QueryView** elemento que especifica una única consulta de Entity SQL que utiliza el operador CASE para devolver un tipo de entidad concreto de la jerarquía según una condición específica.
    -   Con más **QueryView** (elemento) para un tipo específico de la jerarquía. En este caso, utilice el **TypeName** atributo de la **QueryView** elemento para especificar el tipo de entidad para cada vista.
-   Cuando se define una vista de consulta, no puede especificar el **StorageSetName** atributo el **EntitySetMapping** elemento.
-   Cuando se define una vista de consulta, el **EntitySetMapping**elemento no puede contener también **propiedad** asignaciones.

## <a name="resultbinding-element-msl"></a>ResultBinding (Elemento) (MSL)

El **ResultBinding** elemento en el lenguaje de especificación de (asignaciones MSL) asigna valores de columna que se devuelven los procedimientos almacenados las propiedades de entidad en el modelo conceptual cuando las funciones de modificación del tipo de entidad se asignan a almacenado procedimientos de la base de datos subyacente. Por ejemplo, cuando se devuelve el valor de una columna de identidad, insertar un procedimiento almacenado, el **ResultBinding** elemento el valor devuelto asigna a una propiedad de tipo de entidad en el modelo conceptual.

El **ResultBinding** elemento puede ser secundario del elemento InsertFunction o UpdateFunction (elemento).

El **ResultBinding** elemento no puede tener elementos secundarios.

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se aplican a la **ResultBinding** elemento:

| Nombre de atributo | Es necesario | Valor                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Name**       | Sí         | El nombre de la propiedad de entidad del modelo conceptual que se está asignando. |
| **ColumnName** | Sí         | El nombre de la columna que se está asignando.                                          |

### <a name="example"></a>Ejemplo

El siguiente ejemplo se basa en el modelo School y se muestra un **InsertFunction** elemento que se usa para asignar la función de inserción de la **persona** tipo de entidad a la **InsertPerson** procedimiento almacenado. (El **InsertPerson** procedimiento almacenado se muestra a continuación y se declara en el modelo de almacenamiento.) Un **ResultBinding** elemento se usa para asignar un valor de columna devuelto por el procedimiento almacenado (**NewPersonID**) a una propiedad de tipo de entidad (**PersonID**).

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

La instrucción Transact-SQL siguiente se describe la **InsertPerson** procedimiento almacenado:

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

## <a name="resultmapping-element-msl"></a>ResultMapping (Elemento) (MSL)

El **ResultMapping** elemento en el lenguaje de especificación de (asignaciones MSL) define la asignación entre una importación de función en el modelo conceptual y un procedimiento almacenado en la base de datos subyacente cuando se cumple lo siguiente:

-   La importación de función devuelve un tipo de entidad del modelo conceptual o un tipo complejo.
-   Los nombres de las columnas devueltas por el procedimiento almacenado no coinciden exactamente con los nombres de las propiedades del tipo de entidad o el tipo complejo.

De forma predeterminada, la asignación entre las columnas devueltas por un procedimiento almacenado y un tipo de entidad o un tipo complejo se basa en los nombres de las propiedades y de las columnas. Si los nombres de columna no coincidan con los nombres de propiedad, se debe utilizar el **ResultMapping** elemento para definir la asignación. Para obtener un ejemplo de la asignación predeterminada, vea el elemento FunctionImportMapping (MSL).

El **ResultMapping** elemento es un elemento secundario del elemento FunctionImportMapping.

El **ResultMapping** elemento puede tener los elementos secundarios siguientes:

-   EntityTypeMapping (cero o más)
-   ComplexTypeMapping

No hay atributos son aplicables a la **ResultMapping** elemento.

### <a name="example"></a>Ejemplo

Considere el siguiente procedimiento almacenado:

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

Considere también el siguiente tipo de entidad del modelo conceptual:

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

Para crear una importación de función que devuelva instancias del tipo de entidad anterior, la asignación entre las columnas devueltas por el procedimiento almacenado y el tipo de entidad debe definirse en un **ResultMapping** elemento:

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

## <a name="scalarproperty-element-msl"></a>ScalarProperty (Elemento) (MSL)

El **ScalarProperty** elemento en el lenguaje de especificación de (asignaciones MSL) asigna una propiedad de un tipo de entidad del modelo conceptual, un tipo complejo o una asociación a una columna de tabla o un parámetro de procedimiento almacenado en la base de datos subyacente.

> [!NOTE]
> Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento. Para obtener más información, vea el elemento de función de almacenamiento (SSDL).

El **ScalarProperty** elemento puede ser un elemento secundario de los siguientes elementos:

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

Como elemento secundario de la **MappingFragment**, **ComplexProperty**, o **EndProperty** elemento, el **ScalarProperty** elemento asigna una propiedad en el modelo conceptual a una columna de la base de datos. Como elemento secundario de la **InsertFunction**, **UpdateFunction**, o **DeleteFunction** elemento, el **ScalarProperty** elemento asigna una propiedad en el modelo conceptual a un parámetro de procedimiento almacenado.

El **ScalarProperty** elemento no puede tener elementos secundarios.

### <a name="applicable-attributes"></a>Atributos aplicables

Los atributos que se aplican a la **ScalarProperty** elemento varían según el rol del elemento.

En la tabla siguiente se describe los atributos que son aplicables cuando el **ScalarProperty** elemento se usa para asignar una propiedad de modelo conceptual a una columna en la base de datos:

| Nombre de atributo | Es necesario | Valor                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Sí         | El nombre de la propiedad del modelo conceptual que se está asignando. |
| **ColumnName** | Sí         | El nombre de la columna de tabla que se está asociando.              |

En la tabla siguiente se describe los atributos que se aplican a la **ScalarProperty** elemento cuando se usa para asignar una propiedad de modelo conceptual a un parámetro de procedimiento almacenado:

| Nombre de atributo    | Es necesario | Valor                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**          | Sí         | El nombre de la propiedad del modelo conceptual que se está asignando.                                                                                 |
| **ParameterName** | Sí         | El nombre del parámetro que se está asignando.                                                                                                 |
| **Versión**       | No          | **Actual** o **Original** dependiendo de si se debe usar el valor actual o el valor original de la propiedad para las comprobaciones de simultaneidad. |

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra el **ScalarProperty** elemento que se utiliza de dos maneras:

-   Para asignar las propiedades de la **persona** tipo de entidad a las columnas de la **persona**tabla.
-   Para asignar las propiedades de la **persona** los parámetros de tipo de entidad del **UpdatePerson** procedimiento almacenado. Los procedimientos almacenados se declaran en el modelo de almacenamiento.

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

### <a name="example"></a>Ejemplo

El ejemplo siguiente se muestra el **ScalarProperty** elemento que se usa para asignar la inserción y eliminación de funciones de una asociación del modelo conceptual a procedimientos almacenados de la base de datos. Los procedimientos almacenados se declaran en el modelo de almacenamiento.

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

## <a name="updatefunction-element-msl"></a>UpdateFunction (Elemento) (MSL)

El **UpdateFunction** elemento en el lenguaje de especificación de (asignaciones MSL) asigna la función de actualización de un tipo de entidad en el modelo conceptual a un procedimiento almacenado en la base de datos subyacente. Los procedimientos almacenados a los que están asignados las funciones de modificación se deben declarar en el modelo de almacenamiento. Para obtener más información, vea el elemento de función de almacenamiento (SSDL).

> [!NOTE]
>  Si no asigna las tres de la inserción, actualización o eliminación de las operaciones de un tipo de entidad a procedimientos almacenados, se producirá un error en las operaciones no asignadas si se ejecuta en tiempo de ejecución y se produce un UpdateException.

El **UpdateFunction** elemento puede ser un elemento secundario del elemento ModificationFunctionMapping y aplicado a EntityTypeMapping (elemento).

El **UpdateFunction** elemento puede tener los elementos secundarios siguientes:

-   AssociationEnd (cero o más)
-   ComplexProperty (cero o más)
-   ResultBinding (cero o uno)
-   ScalarProperty (cero o más)

### <a name="applicable-attributes"></a>Atributos aplicables

En la tabla siguiente se describe los atributos que se pueden aplicar a la **UpdateFunction** elemento.

| Nombre de atributo            | Es necesario | Valor                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Sí         | El nombre completo, calificado con el espacio de nombres, del procedimiento almacenado al que la función de actualización está asignada. El procedimiento almacenado se debe declarar en el modelo de almacenamiento. |
| **RowsAffectedParameter** | No          | El nombre del parámetro de salida que devuelve el número de filas afectadas.                                                                               |

### <a name="example"></a>Ejemplo

El siguiente ejemplo se basa en el modelo School y se muestra el **UpdateFunction** elemento que se usa para asignar la función de actualización de la **persona** tipo de entidad a la **UpdatePerson** procedimiento almacenado. El **UpdatePerson** se declara un procedimiento almacenado en el modelo de almacenamiento.

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
