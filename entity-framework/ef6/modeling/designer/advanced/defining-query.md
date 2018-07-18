---
title: Definir consulta - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
caps.latest.revision: 3
ms.openlocfilehash: 593fb9925a7a0b59a69b8c8dc4846640627756aa
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122280"
---
# <a name="defining-query---ef-designer"></a>Definir consulta - EF Designer
Este tutorial muestra cómo agregar una definición de tipo de consulta y una entidad correspondiente a un modelo con EF Designer. Una consulta de definición se usa normalmente para proporcionar funcionalidad similar a la que ofrece una vista de base de datos, pero la vista se define en el modelo, no en la base de datos. Una consulta de definición permite ejecutar una instrucción SQL que se especifica en el **DefiningQuery** elemento de un archivo .edmx. Para obtener más información, consulte **DefiningQuery** en el [especificación de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Cuando se usan consultas de definición, también debe definir un tipo de entidad en el modelo. El tipo de entidad se usa para mostrar los datos expuestos por la consulta de definición. Tenga en cuenta que los datos mostrados a través de este tipo de entidad es de solo lectura.

Las consultas con parámetros no se pueden ejecutar como consultas de definición. Sin embargo, los datos se pueden actualizar asignando las funciones de inserción, actualización y eliminación del tipo de entidad que los muestra a los procedimientos almacenados. Para obtener más información, consulte [Insert, Update y Delete con procedimientos almacenados](~/ef6/modeling/designer/stored-procedures/cud.md).

En este tema se muestra cómo realizar las siguientes tareas.

-   Agregar una consulta de definición
-   Agregar un tipo de entidad al modelo
-   La consulta de definición se asignan al tipo de entidad

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará:

- Una versión reciente de Visual Studio.
- El [base de datos de ejemplo School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurar el proyecto

En este tutorial usa Visual Studio 2012 o posterior.

-   Abra Visual Studio.
-   En el menú **Archivo** , elija **Nuevo**y haga clic en **Proyecto**.
-   En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **aplicación de consola** plantilla.
-   Escriba **DefiningQuerySample** como el nombre del proyecto y haga clic en **Aceptar**.

 

## <a name="create-a-model-based-on-the-school-database"></a>Crear un modelo basado en la base de datos School

-   Haga clic en el nombre del proyecto en el Explorador de soluciones, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**.
-   Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.
-   Escriba **DefiningQueryModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.
-   En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**.
-   Haga clic en nueva conexión. En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.
    El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.
-   En el cuadro de diálogo Elija los objetos de base de datos, compruebe el **tablas** nodo. Esta acción agregará todas las tablas a la **School** modelo.
-   Haga clic en **Finalizar**.
-   En el Explorador de soluciones, haga clic en el **DefiningQueryModel.edmx** de archivo y seleccione **abrir con...** .
-   Seleccione **Editor XML (texto)**.

    ![XMLEditor](~/ef6/media/xmleditor.png)

-   Haga clic en **Sí** si aparece el mensaje siguiente:

    ![Warning2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Agregar una consulta de definición

En este paso se usará el Editor XML para agregar una definición de consulta y un tipo de entidad a la sección SSDL del archivo .edmx. 

-   Agregar un **EntitySet** elemento a la sección SSDL del archivo .edmx (línea 5 a 13). Especifique lo siguiente:
    -   Solo el **nombre** y **EntityType** los atributos de la **EntitySet** se especifican el elemento.
    -   El nombre completo del tipo de entidad se usa en el **EntityType** atributo.
    -   La instrucción SQL que se ejecutará se especifica en el **DefiningQuery** elemento.

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   Agregar el **EntityType** elemento a la sección SSDL del archivo. archivo tal como se muestra a continuación. Tenga en cuenta lo siguiente:
    -   El valor de la **nombre** atributo corresponde al valor de la **EntityType** atributo en el **EntitySet** elemento anterior, aunque el nombre completo de la tipo de entidad se usa en el **EntityType** atributo.
    -   Los nombres de propiedad corresponden a los nombres de columna devueltos por la instrucción SQL en el **DefiningQuery** elemento (arriba).
    -   En este ejemplo, la clave de entidad consta de tres propiedades para garantizar un valor de clave único.

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> Si más adelante se ejecuta el **Asistente para actualizar modelo** cuadro de diálogo, los cambios realizados en el modelo de almacenamiento, incluida la definición de las consultas, se sobrescribirán.

 

## <a name="add-an-entity-type-to-the-model"></a>Agregar un tipo de entidad al modelo

En este paso se agregará el tipo de entidad al modelo conceptual con EF Designer.  Tenga en cuenta lo siguiente:

-   El **nombre** de la entidad que corresponde al valor de la **EntityType** atributo en el **EntitySet** elemento anterior.
-   Los nombres de propiedad corresponden a los nombres de columna devueltos por la instrucción SQL en el **DefiningQuery** elemento anterior.
-   En este ejemplo, la clave de entidad consta de tres propiedades para garantizar un valor de clave único.

Abra el modelo de EF Designer.

-   Haga doble clic en el DefiningQueryModel.edmx.
-   Por ejemplo **Sí** en el siguiente mensaje:

    ![Warning2](~/ef6/media/warning2.png)

 

Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.

-   Haga clic en la superficie designer y seleccione **Agregar nuevo**-&gt;**entidad...** .
-   Especificar **GradeReport** para el nombre de entidad y **CourseID** para el **propiedad Key**.
-   Haga clic en el **GradeReport** entidad y seleccione **Agregar nuevo** - &gt; **propiedad escalar**.
-   Cambiar el nombre predeterminado de la propiedad a **FirstName**.
-   Agregar otra propiedad escalar y especifique **LastName** para el nombre.
-   Agregar otra propiedad escalar y especifique **grado** para el nombre.
-   En el **propiedades** ventana, cambie el **grado**del **tipo** propiedad **Decimal**.
-   Seleccione el **FirstName** y **LastName** propiedades.
-   En el **propiedades** ventana, cambie el **EntityKey** valor de propiedad **True**.

Como resultado, los siguientes elementos se agregaron a la **CSDL** sección del archivo .edmx.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>La consulta de definición se asignan al tipo de entidad

En este paso, se usará la ventana de detalles de la asignación para asignar conceptual y los tipos de entidad de almacenamiento.

-   Haga clic en el **GradeReport** entidad en la superficie de diseño y seleccione **asignación de tabla**.  
    El **detalles de Mapping** se muestra la ventana.
-   Seleccione **GradeReport** desde el **&lt;agregar una tabla o vista&gt;** lista desplegable (ubicado en **tabla**s).  
    Asignaciones entre conceptual predeterminado y el almacenamiento **GradeReport** aparecen el tipo de entidad.  
    ![MappingDetails3](~/ef6/media/mappingdetails.png)

Como resultado, el **EntitySetMapping** elemento se agrega a la sección de asignación del archivo .edmx. 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   Compile la aplicación.

 

## <a name="call-the-defining-query-in-your-code"></a>Llame a la consulta de definición en el código

Ahora puede ejecutar la consulta de definición mediante la **GradeReport** tipo de entidad. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
