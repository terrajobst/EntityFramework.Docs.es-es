---
title: Definir consulta - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: 8415a265cdbe078422e0467ee97da955a81b873d
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250977"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="0d0f3-102">Definir consulta - EF Designer</span><span class="sxs-lookup"><span data-stu-id="0d0f3-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="0d0f3-103">Este tutorial muestra cómo agregar una definición de tipo de consulta y una entidad correspondiente a un modelo con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="0d0f3-104">Una consulta de definición se usa normalmente para proporcionar funcionalidad similar a la que ofrece una vista de base de datos, pero la vista se define en el modelo, no en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="0d0f3-105">Una consulta de definición permite ejecutar una instrucción SQL que se especifica en el **DefiningQuery** elemento de un archivo .edmx.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="0d0f3-106">Para obtener más información, consulte **DefiningQuery** en el [especificación de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="0d0f3-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="0d0f3-107">Cuando se usan consultas de definición, también debe definir un tipo de entidad en el modelo.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="0d0f3-108">El tipo de entidad se usa para mostrar los datos expuestos por la consulta de definición.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="0d0f3-109">Tenga en cuenta que los datos mostrados a través de este tipo de entidad es de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="0d0f3-110">Las consultas con parámetros no se pueden ejecutar como consultas de definición.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="0d0f3-111">Sin embargo, los datos se pueden actualizar asignando las funciones de inserción, actualización y eliminación del tipo de entidad que los muestra a los procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="0d0f3-112">Para obtener más información, consulte [Insert, Update y Delete con procedimientos almacenados](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="0d0f3-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="0d0f3-113">En este tema se muestra cómo realizar las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="0d0f3-114">Agregar una consulta de definición</span><span class="sxs-lookup"><span data-stu-id="0d0f3-114">Add a Defining Query</span></span>
-   <span data-ttu-id="0d0f3-115">Agregar un tipo de entidad al modelo</span><span class="sxs-lookup"><span data-stu-id="0d0f3-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="0d0f3-116">La consulta de definición se asignan al tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="0d0f3-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d0f3-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="0d0f3-117">Prerequisites</span></span>

<span data-ttu-id="0d0f3-118">Para completar este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="0d0f3-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="0d0f3-119">Una versión reciente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="0d0f3-120">El [base de datos de ejemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="0d0f3-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="0d0f3-121">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="0d0f3-121">Set up the Project</span></span>

<span data-ttu-id="0d0f3-122">En este tutorial usa Visual Studio 2012 o posterior.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="0d0f3-123">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="0d0f3-124">En el menú **Archivo** , elija **Nuevo**y haga clic en **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="0d0f3-125">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **aplicación de consola** plantilla.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="0d0f3-126">Escriba **DefiningQuerySample** como el nombre del proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="0d0f3-127">Crear un modelo basado en la base de datos School</span><span class="sxs-lookup"><span data-stu-id="0d0f3-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="0d0f3-128">Haga clic en el nombre del proyecto en el Explorador de soluciones, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="0d0f3-129">Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="0d0f3-130">Escriba **DefiningQueryModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="0d0f3-131">En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="0d0f3-132">Haga clic en nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-132">Click New Connection.</span></span> <span data-ttu-id="0d0f3-133">En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="0d0f3-134">El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="0d0f3-135">En el cuadro de diálogo Elija los objetos de base de datos, compruebe el **tablas** nodo.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="0d0f3-136">Esta acción agregará todas las tablas a la **School** modelo.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="0d0f3-137">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-137">Click **Finish**.</span></span>
-   <span data-ttu-id="0d0f3-138">En el Explorador de soluciones, haga clic en el **DefiningQueryModel.edmx** de archivo y seleccione **abrir con...** .</span><span class="sxs-lookup"><span data-stu-id="0d0f3-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="0d0f3-139">Seleccione **Editor XML (texto)**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-139">Select **XML (Text) Editor**.</span></span>

    ![Editor XML](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="0d0f3-141">Haga clic en **Sí** si aparece el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="0d0f3-141">Click **Yes** if prompted with the following message:</span></span>

    ![Advertencia 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="0d0f3-143">Agregar una consulta de definición</span><span class="sxs-lookup"><span data-stu-id="0d0f3-143">Add a Defining Query</span></span>

<span data-ttu-id="0d0f3-144">En este paso se usará el Editor XML para agregar una definición de consulta y un tipo de entidad a la sección SSDL del archivo .edmx.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="0d0f3-145">Agregar un **EntitySet** elemento a la sección SSDL del archivo .edmx (línea 5 a 13).</span><span class="sxs-lookup"><span data-stu-id="0d0f3-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="0d0f3-146">Especifique lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0d0f3-146">Specify the following:</span></span>
    -   <span data-ttu-id="0d0f3-147">Solo el **nombre** y **EntityType** los atributos de la **EntitySet** se especifican el elemento.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="0d0f3-148">El nombre completo del tipo de entidad se usa en el **EntityType** atributo.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="0d0f3-149">La instrucción SQL que se ejecutará se especifica en el **DefiningQuery** elemento.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

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

-   <span data-ttu-id="0d0f3-150">Agregar el **EntityType** elemento a la sección SSDL del archivo.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="0d0f3-151">archivo tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-151">file as shown below.</span></span> <span data-ttu-id="0d0f3-152">Tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0d0f3-152">Note the following:</span></span>
    -   <span data-ttu-id="0d0f3-153">El valor de la **nombre** atributo corresponde al valor de la **EntityType** atributo en el **EntitySet** elemento anterior, aunque el nombre completo de la tipo de entidad se usa en el **EntityType** atributo.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="0d0f3-154">Los nombres de propiedad corresponden a los nombres de columna devueltos por la instrucción SQL en el **DefiningQuery** elemento (arriba).</span><span class="sxs-lookup"><span data-stu-id="0d0f3-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="0d0f3-155">En este ejemplo, la clave de entidad consta de tres propiedades para garantizar un valor de clave único.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

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
> <span data-ttu-id="0d0f3-156">Si más adelante se ejecuta el **Asistente para actualizar modelo** cuadro de diálogo, los cambios realizados en el modelo de almacenamiento, incluida la definición de las consultas, se sobrescribirán.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="0d0f3-157">Agregar un tipo de entidad al modelo</span><span class="sxs-lookup"><span data-stu-id="0d0f3-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="0d0f3-158">En este paso se agregará el tipo de entidad al modelo conceptual con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span>  <span data-ttu-id="0d0f3-159">Tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0d0f3-159">Note the following:</span></span>

-   <span data-ttu-id="0d0f3-160">El **nombre** de la entidad que corresponde al valor de la **EntityType** atributo en el **EntitySet** elemento anterior.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="0d0f3-161">Los nombres de propiedad corresponden a los nombres de columna devueltos por la instrucción SQL en el **DefiningQuery** elemento anterior.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="0d0f3-162">En este ejemplo, la clave de entidad consta de tres propiedades para garantizar un valor de clave único.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="0d0f3-163">Abra el modelo de EF Designer.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="0d0f3-164">Haga doble clic en el DefiningQueryModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="0d0f3-165">Por ejemplo **Sí** en el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="0d0f3-165">Say **Yes** to the following message:</span></span>

    ![Advertencia 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="0d0f3-167">Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="0d0f3-168">Haga clic en la superficie designer y seleccione **Agregar nuevo**-&gt;**entidad...** .</span><span class="sxs-lookup"><span data-stu-id="0d0f3-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="0d0f3-169">Especificar **GradeReport** para el nombre de entidad y **CourseID** para el **propiedad Key**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="0d0f3-170">Haga clic en el **GradeReport** entidad y seleccione **Agregar nuevo** - &gt; **propiedad escalar**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="0d0f3-171">Cambiar el nombre predeterminado de la propiedad a **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="0d0f3-172">Agregar otra propiedad escalar y especifique **LastName** para el nombre.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="0d0f3-173">Agregar otra propiedad escalar y especifique **grado** para el nombre.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="0d0f3-174">En el **propiedades** ventana, cambie el **grado**del **tipo** propiedad **Decimal**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="0d0f3-175">Seleccione el **FirstName** y **LastName** propiedades.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="0d0f3-176">En el **propiedades** ventana, cambie el **EntityKey** valor de propiedad **True**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="0d0f3-177">Como resultado, los siguientes elementos se agregaron a la **CSDL** sección del archivo .edmx.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="0d0f3-178">La consulta de definición se asignan al tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="0d0f3-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="0d0f3-179">En este paso, se usará la ventana de detalles de la asignación para asignar conceptual y los tipos de entidad de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="0d0f3-180">Haga clic en el **GradeReport** entidad en la superficie de diseño y seleccione **asignación de tabla**.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="0d0f3-181">El **detalles de Mapping** se muestra la ventana.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="0d0f3-182">Seleccione **GradeReport** desde el **&lt;agregar una tabla o vista&gt;** lista desplegable (ubicado en **tabla**s).</span><span class="sxs-lookup"><span data-stu-id="0d0f3-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="0d0f3-183">Asignaciones entre conceptual predeterminado y el almacenamiento **GradeReport** aparecen el tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="0d0f3-184">![Asignación detalles3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="0d0f3-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="0d0f3-185">Como resultado, el **EntitySetMapping** elemento se agrega a la sección de asignación del archivo .edmx.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

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

-   <span data-ttu-id="0d0f3-186">Compile la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="0d0f3-187">Llame a la consulta de definición en el código</span><span class="sxs-lookup"><span data-stu-id="0d0f3-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="0d0f3-188">Ahora puede ejecutar la consulta de definición mediante la **GradeReport** tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="0d0f3-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
