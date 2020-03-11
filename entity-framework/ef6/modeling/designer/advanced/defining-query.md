---
title: Definición de Query-EF Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415536"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="33d7a-102">Definir el diseñador de consultas-EF</span><span class="sxs-lookup"><span data-stu-id="33d7a-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="33d7a-103">En este tutorial se muestra cómo agregar una consulta de definición y un tipo de entidad correspondiente a un modelo mediante el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="33d7a-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="33d7a-104">Una consulta de definición se usa normalmente para proporcionar una funcionalidad similar a la que proporciona una vista de base de datos, pero la vista se define en el modelo, no en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="33d7a-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="33d7a-105">Una consulta de definición permite ejecutar una instrucción SQL que se especifica en el elemento **DefiningQuery** de un archivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="33d7a-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="33d7a-106">Para obtener más información, vea **DefiningQuery** en la [especificación de SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="33d7a-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="33d7a-107">Al utilizar las consultas de definición, también debe definir un tipo de entidad en el modelo.</span><span class="sxs-lookup"><span data-stu-id="33d7a-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="33d7a-108">El tipo de entidad se utiliza para exponer los datos expuestos por la consulta de definición.</span><span class="sxs-lookup"><span data-stu-id="33d7a-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="33d7a-109">Tenga en cuenta que los datos que se exponen a través de este tipo de entidad son de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="33d7a-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="33d7a-110">Las consultas con parámetros no se pueden ejecutar como consultas de definición.</span><span class="sxs-lookup"><span data-stu-id="33d7a-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="33d7a-111">Sin embargo, los datos se pueden actualizar asignando las funciones de inserción, actualización y eliminación del tipo de entidad que los muestra a los procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="33d7a-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="33d7a-112">Para obtener más información, vea [Insert, Update y DELETE con procedimientos almacenados](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="33d7a-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="33d7a-113">En este tema se muestra cómo realizar las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="33d7a-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="33d7a-114">Agregar una consulta de definición</span><span class="sxs-lookup"><span data-stu-id="33d7a-114">Add a Defining Query</span></span>
-   <span data-ttu-id="33d7a-115">Agregar un tipo de entidad al modelo</span><span class="sxs-lookup"><span data-stu-id="33d7a-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="33d7a-116">Asignar la consulta de definición al tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="33d7a-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33d7a-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="33d7a-117">Prerequisites</span></span>

<span data-ttu-id="33d7a-118">Para completar este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="33d7a-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="33d7a-119">Una versión reciente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33d7a-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="33d7a-120">La [base de datos de ejemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="33d7a-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="33d7a-121">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="33d7a-121">Set up the Project</span></span>

<span data-ttu-id="33d7a-122">En este tutorial se usa Visual Studio 2012 o una versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="33d7a-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="33d7a-123">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33d7a-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="33d7a-124">En el menú **Archivo** , seleccione **Nuevo**y haga clic en **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="33d7a-125">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione la plantilla **aplicación de consola** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="33d7a-126">Escriba **DefiningQuerySample** como nombre del proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="33d7a-127">Creación de un modelo basado en la base de datos School</span><span class="sxs-lookup"><span data-stu-id="33d7a-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="33d7a-128">Haga clic con el botón secundario en el nombre del proyecto en Explorador de soluciones, seleccione **Agregar**y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="33d7a-129">Seleccione **datos** en el menú de la izquierda y, a continuación, seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="33d7a-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="33d7a-130">Escriba **DefiningQueryModel. edmx** como nombre de archivo y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="33d7a-131">En el cuadro de diálogo elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="33d7a-132">Haga clic en nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="33d7a-132">Click New Connection.</span></span> <span data-ttu-id="33d7a-133">En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **LocalDB)\\mssqllocaldb**), seleccione el método de autenticación, escriba **School** para el nombre de la base de datos y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="33d7a-134">El cuadro de diálogo elegir la conexión de datos se actualiza con la configuración de conexión de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="33d7a-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="33d7a-135">En el cuadro de diálogo elija los objetos de base de datos, compruebe el nodo **tablas** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="33d7a-136">Se agregarán todas las tablas al modelo **School** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="33d7a-137">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-137">Click **Finish**.</span></span>
-   <span data-ttu-id="33d7a-138">En Explorador de soluciones, haga clic con el botón secundario en el archivo **DefiningQueryModel. edmx** y seleccione **abrir con.** ...</span><span class="sxs-lookup"><span data-stu-id="33d7a-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="33d7a-139">Seleccione **Editor XML (texto)** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-139">Select **XML (Text) Editor**.</span></span>

    ![Editor XML](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="33d7a-141">Haga clic en **sí** si se le solicita el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="33d7a-141">Click **Yes** if prompted with the following message:</span></span>

    ![ADVERTENCIA 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="33d7a-143">Agregar una consulta de definición</span><span class="sxs-lookup"><span data-stu-id="33d7a-143">Add a Defining Query</span></span>

<span data-ttu-id="33d7a-144">En este paso, usaremos el editor XML para agregar una consulta de definición y un tipo de entidad a la sección SSDL del archivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="33d7a-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="33d7a-145">Agregue un elemento de **EntitySet** a la sección SSDL del archivo. edmx (línea 5 a 13).</span><span class="sxs-lookup"><span data-stu-id="33d7a-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="33d7a-146">Especifique lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="33d7a-146">Specify the following:</span></span>
    -   <span data-ttu-id="33d7a-147">Solo se especifican los atributos **Name** y **EntityType** del elemento de **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="33d7a-148">El nombre completo del tipo de entidad se usa en el atributo **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="33d7a-149">La instrucción SQL que se va a ejecutar se especifica en el elemento **DefiningQuery** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

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

-   <span data-ttu-id="33d7a-150">Agregue el elemento **EntityType** a la sección SSDL del archivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="33d7a-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="33d7a-151">como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="33d7a-151">file as shown below.</span></span> <span data-ttu-id="33d7a-152">Tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="33d7a-152">Note the following:</span></span>
    -   <span data-ttu-id="33d7a-153">El valor del atributo **Name** corresponde al valor del atributo **EntityType** en el elemento **EntitySet** anterior, aunque el nombre completo del tipo de entidad se usa en el atributo **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="33d7a-154">Los nombres de propiedad corresponden a los nombres de columna devueltos por la instrucción SQL en el elemento **DefiningQuery** (arriba).</span><span class="sxs-lookup"><span data-stu-id="33d7a-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="33d7a-155">En este ejemplo, la clave de entidad consta de tres propiedades para garantizar un valor de clave único.</span><span class="sxs-lookup"><span data-stu-id="33d7a-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

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
> <span data-ttu-id="33d7a-156">Si posteriormente ejecuta el cuadro de diálogo **Asistente para actualizar modelo** , se sobrescribirán los cambios realizados en el modelo de almacenamiento, incluida la definición de las consultas.</span><span class="sxs-lookup"><span data-stu-id="33d7a-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="33d7a-157">Agregar un tipo de entidad al modelo</span><span class="sxs-lookup"><span data-stu-id="33d7a-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="33d7a-158">En este paso se agregará el tipo de entidad al modelo conceptual mediante el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="33d7a-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span> <span data-ttu-id="33d7a-159"> Tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="33d7a-159"> Note the following:</span></span>

-   <span data-ttu-id="33d7a-160">El **nombre** de la entidad corresponde al valor del atributo **EntityType** en el elemento **EntitySet** anterior.</span><span class="sxs-lookup"><span data-stu-id="33d7a-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="33d7a-161">Los nombres de propiedad corresponden a los nombres de columna devueltos por la instrucción SQL en el elemento **DefiningQuery** anterior.</span><span class="sxs-lookup"><span data-stu-id="33d7a-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="33d7a-162">En este ejemplo, la clave de entidad consta de tres propiedades para garantizar un valor de clave único.</span><span class="sxs-lookup"><span data-stu-id="33d7a-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="33d7a-163">Abra el modelo en el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="33d7a-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="33d7a-164">Haga doble clic en DefiningQueryModel. edmx.</span><span class="sxs-lookup"><span data-stu-id="33d7a-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="33d7a-165">Por **ejemplo** , en el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="33d7a-165">Say **Yes** to the following message:</span></span>

    ![ADVERTENCIA 2](~/ef6/media/warning2.png)

 

<span data-ttu-id="33d7a-167">Se muestra el diseñador de entidades, que proporciona una superficie de diseño para editar el modelo.</span><span class="sxs-lookup"><span data-stu-id="33d7a-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="33d7a-168">Haga clic con el botón secundario en la superficie del diseñador y seleccione **Agregar nuevo**-&gt;**entidad.** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="33d7a-169">Especifique **GradeReport** para el nombre de entidad y el **CourseID** para la **propiedad de clave**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="33d7a-170">Haga clic con el botón secundario en la entidad **GradeReport** y seleccione **agregar nuevo**-&gt; **propiedad escalar**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="33d7a-171">Cambie el nombre predeterminado de la propiedad a **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="33d7a-172">Agregue otra propiedad escalar y especifique **LastName** como nombre.</span><span class="sxs-lookup"><span data-stu-id="33d7a-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="33d7a-173">Agregue otra propiedad escalar y especifique **grade** como el nombre.</span><span class="sxs-lookup"><span data-stu-id="33d7a-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="33d7a-174">En la ventana **propiedades** , cambie la propiedad **tipo** de **grado**a **decimal**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="33d7a-175">Seleccione las propiedades **FirstName** y **LastName** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="33d7a-176">En la ventana **propiedades** , cambie el valor de la propiedad **EntityKey** a **true**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="33d7a-177">Como resultado, se han agregado los siguientes elementos a la sección **CSDL** del archivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="33d7a-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="33d7a-178">Asignar la consulta de definición al tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="33d7a-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="33d7a-179">En este paso, usaremos la ventana detalles de la asignación para asignar los tipos de entidad conceptual y de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="33d7a-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="33d7a-180">Haga clic con el botón secundario en la entidad **GradeReport** en la superficie de diseño y seleccione **asignación de tabla**.</span><span class="sxs-lookup"><span data-stu-id="33d7a-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="33d7a-181">Se muestra la ventana detalles de la **asignación** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="33d7a-182">Seleccione **GradeReport** en la lista desplegable **&lt;agregar una tabla o vista&gt;** (que se encuentra en la **tabla**s).</span><span class="sxs-lookup"><span data-stu-id="33d7a-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="33d7a-183">Aparecen las asignaciones predeterminadas entre el tipo de entidad conceptual y Storage **GradeReport** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="33d7a-184">Asignación de ![Details3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="33d7a-184">![Mapping Details3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="33d7a-185">Como resultado, el elemento **EntitySetMapping** se agrega a la sección de asignación del archivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="33d7a-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

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

-   <span data-ttu-id="33d7a-186">Realice la compilación de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="33d7a-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="33d7a-187">Llamar a la consulta de definición en el código</span><span class="sxs-lookup"><span data-stu-id="33d7a-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="33d7a-188">Ahora puede ejecutar la consulta de definición mediante el tipo de entidad **GradeReport** .</span><span class="sxs-lookup"><span data-stu-id="33d7a-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
