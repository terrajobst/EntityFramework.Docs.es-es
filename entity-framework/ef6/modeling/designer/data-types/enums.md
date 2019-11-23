---
title: 'Compatibilidad con la enumeración: EF Designer-EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182520"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="74fdf-102">Compatibilidad con la enumeración: EF Designer</span><span class="sxs-lookup"><span data-stu-id="74fdf-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="74fdf-103">**EF5** y versiones posteriores: las características, las API, etc. que se describen en esta página se introdujeron en Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="74fdf-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="74fdf-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="74fdf-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="74fdf-105">Este tutorial de vídeo y paso a paso muestra cómo utilizar tipos de enumeración con el Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="74fdf-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="74fdf-106">También se muestra cómo usar las enumeraciones en una consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="74fdf-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="74fdf-107">En este tutorial se utilizará Model First para crear una nueva base de datos, pero el diseñador de EF también puede usarse con el flujo de trabajo de [Database First](~/ef6/modeling/designer/workflows/database-first.md) para asignar a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="74fdf-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="74fdf-108">La compatibilidad con la enumeración se presentó en Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="74fdf-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="74fdf-109">Para usar las nuevas características, como las enumeraciones, los tipos de datos espaciales y las funciones con valores de tabla, debe tener como destino .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="74fdf-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="74fdf-110">Visual Studio 2012 tiene como destino .NET 4,5 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="74fdf-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="74fdf-111">En Entity Framework, una enumeración puede tener los siguientes tipos subyacentes: **byte**, **Int16**, **Int32**, **Int64** o **SByte**.</span><span class="sxs-lookup"><span data-stu-id="74fdf-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="74fdf-112">Ver el vídeo</span><span class="sxs-lookup"><span data-stu-id="74fdf-112">Watch the Video</span></span>
<span data-ttu-id="74fdf-113">En este vídeo se muestra cómo utilizar tipos de enumeración con el Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="74fdf-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="74fdf-114">También se muestra cómo usar las enumeraciones en una consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="74fdf-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="74fdf-115">**Presentada por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="74fdf-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="74fdf-116">**Vídeo**: [wmv](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (zip)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="74fdf-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="74fdf-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="74fdf-117">Pre-Requisites</span></span>

<span data-ttu-id="74fdf-118">Deberá tener instalado Visual Studio 2012, Ultimate, Premium, Professional o Web Express Edition para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="74fdf-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="74fdf-119">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="74fdf-119">Set up the Project</span></span>

1.  <span data-ttu-id="74fdf-120">Abra Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="74fdf-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="74fdf-121">En el menú **archivo** , seleccione **nuevo**y, a continuación, haga clic en **proyecto** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="74fdf-122">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione la plantilla de **consola** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="74fdf-123">Escriba **EnumEFDesigner** como nombre del proyecto y haga clic en **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="74fdf-124">Crear un nuevo modelo con EF Designer</span><span class="sxs-lookup"><span data-stu-id="74fdf-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="74fdf-125">Haga clic con el botón secundario en el nombre del proyecto en Explorador de soluciones, seleccione **Agregar**y, a continuación, haga clic en **nuevo elemento** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="74fdf-126">Seleccione **datos** en el menú de la izquierda y, a continuación, seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="74fdf-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="74fdf-127">Escriba **EnumTestModel. edmx** como nombre de archivo y, a continuación, haga clic en **Agregar** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="74fdf-128">En la página del Asistente para Entity Data Model, seleccione **modelo vacío** en el cuadro de diálogo elegir contenido del modelo.</span><span class="sxs-lookup"><span data-stu-id="74fdf-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="74fdf-129">Haga clic en **Finalizar**</span><span class="sxs-lookup"><span data-stu-id="74fdf-129">Click **Finish**</span></span>

<span data-ttu-id="74fdf-130">Se muestra el diseñador de entidades, que proporciona una superficie de diseño para editar el modelo.</span><span class="sxs-lookup"><span data-stu-id="74fdf-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="74fdf-131">El asistente realiza las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="74fdf-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="74fdf-132">Genera el archivo EnumTestModel. edmx que define el modelo conceptual, el modelo de almacenamiento y la asignación entre ellos.</span><span class="sxs-lookup"><span data-stu-id="74fdf-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="74fdf-133">Establece la propiedad de procesamiento de artefactos de metadatos del archivo. edmx que se va a incrustar en el ensamblado de salida para incrustar los archivos de metadatos generados en el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="74fdf-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="74fdf-134">Agrega una referencia a los siguientes ensamblados: EntityFramework, System. ComponentModel. DataAnnotations y System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="74fdf-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="74fdf-135">Crea archivos EnumTestModel.tt y EnumTestModel.Context.tt y los agrega en el archivo. edmx.</span><span class="sxs-lookup"><span data-stu-id="74fdf-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="74fdf-136">Estos archivos de plantilla T4 generan el código que define el tipo derivado de DbContext y los tipos POCO que se asignan a las entidades en el modelo. edmx.</span><span class="sxs-lookup"><span data-stu-id="74fdf-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="74fdf-137">Agregar un nuevo tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="74fdf-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="74fdf-138">Haga clic con el botón secundario en un área vacía de la superficie de diseño, seleccione **agregar&gt; entidad**, aparecerá el cuadro de diálogo nueva entidad.</span><span class="sxs-lookup"><span data-stu-id="74fdf-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="74fdf-139">Especifique **Department** como nombre de tipo y especifique **departmentId** como nombre de la propiedad de clave y deje el tipo como **Int32** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="74fdf-140">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="74fdf-140">Click **OK**</span></span>
4.  <span data-ttu-id="74fdf-141">Haga clic con el botón derecho en la entidad y seleccione **Agregar nueva&gt; propiedad escalar** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="74fdf-142">Cambiar el nombre de la nueva propiedad a **nombre**</span><span class="sxs-lookup"><span data-stu-id="74fdf-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="74fdf-143">Cambie el tipo de la nueva propiedad a **Int32** (de forma predeterminada, la nueva propiedad es de tipo cadena) para cambiar el tipo, abra el ventana Propiedades y cambie la propiedad Type a **Int32** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="74fdf-144">Agregue otra propiedad escalar y cambie su nombre a **presupuesto**, cambie el tipo a **decimal** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="74fdf-145">Agregar un tipo de enumeración</span><span class="sxs-lookup"><span data-stu-id="74fdf-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="74fdf-146">En el Entity Framework Designer, haga clic con el botón secundario en la propiedad nombre, seleccione **convertir en enumeración** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![Convertir en enumeración](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="74fdf-148">En el cuadro de diálogo **Agregar enumeración** , escriba **DepartmentNames** para el nombre del tipo de enumeración, cambie el tipo subyacente a **Int32**y, a continuación, agregue los siguientes miembros al tipo: Inglés, matemáticas y economía.</span><span class="sxs-lookup"><span data-stu-id="74fdf-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![Agregar tipo de enumeración](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="74fdf-150">Presione **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="74fdf-150">Press **OK**</span></span>
4.  <span data-ttu-id="74fdf-151">Guardar el modelo y compilar el proyecto</span><span class="sxs-lookup"><span data-stu-id="74fdf-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="74fdf-152">Al compilar, las advertencias sobre entidades y asociaciones no asignadas pueden aparecer en el Lista de errores.</span><span class="sxs-lookup"><span data-stu-id="74fdf-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="74fdf-153">Puede omitir estas advertencias porque, después de elegir generar la base de datos a partir del modelo, los errores desaparecerán.</span><span class="sxs-lookup"><span data-stu-id="74fdf-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="74fdf-154">Si observa el ventana Propiedades, observará que el tipo de la propiedad Name se ha cambiado a **DepartmentNames** y que el tipo de enumeración recién agregado se ha agregado a la lista de tipos.</span><span class="sxs-lookup"><span data-stu-id="74fdf-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="74fdf-155">Si cambia a la ventana Explorador de modelos, verá que el tipo también se agregó al nodo tipos de enumeración.</span><span class="sxs-lookup"><span data-stu-id="74fdf-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Explorador de modelos](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="74fdf-157">También puede agregar nuevos tipos de enumeración desde esta ventana haciendo clic con el botón secundario del mouse y seleccionando **Agregar tipo de enumeración**.</span><span class="sxs-lookup"><span data-stu-id="74fdf-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="74fdf-158">Una vez creado el tipo, aparecerá en la lista de tipos y podrá asociarlo a una propiedad.</span><span class="sxs-lookup"><span data-stu-id="74fdf-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="74fdf-159">Generar base de datos a partir del modelo</span><span class="sxs-lookup"><span data-stu-id="74fdf-159">Generate Database from Model</span></span>

<span data-ttu-id="74fdf-160">Ahora se puede generar una base de datos basada en el modelo.</span><span class="sxs-lookup"><span data-stu-id="74fdf-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="74fdf-161">Haga clic con el botón secundario en un espacio vacío en la superficie de Entity Designer y seleccione **generar base de datos a partir del modelo** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="74fdf-162">Aparece el cuadro de diálogo elegir la conexión de datos del Asistente para generar base de datos, haga clic en el botón **nueva conexión** especifique **(LocalDB)\\mssqllocaldb** para el nombre del servidor y **EnumTest** para la base de datos y haga clic en **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="74fdf-163">Aparecerá un cuadro de diálogo en el que se le pregunta si desea crear una nueva base de datos, haga clic en **sí**.</span><span class="sxs-lookup"><span data-stu-id="74fdf-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="74fdf-164">Haga clic en **siguiente** y el Asistente para crear bases de datos genera el lenguaje de definición de datos (DDL) para crear una base de datos. la DDL generada se muestra en el cuadro de diálogo Resumen y configuración. tenga en cuenta que el DDL no contiene una definición para una tabla que se asigna al tipo de enumeración</span><span class="sxs-lookup"><span data-stu-id="74fdf-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="74fdf-165">Haga clic en **Finalizar** al hacer clic en finalizar no se ejecuta el script DDL.</span><span class="sxs-lookup"><span data-stu-id="74fdf-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="74fdf-166">El Asistente para crear bases de datos hace lo siguiente: abre **EnumTest. edmx. SQL** en el editor de T-SQL genera las secciones de asignación y esquema de almacenamiento del archivo edmx agrega información de la cadena de conexión al archivo app. config.</span><span class="sxs-lookup"><span data-stu-id="74fdf-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="74fdf-167">Haga clic con el botón secundario del mouse en el editor de T-SQL y seleccione **Ejecutar** el cuadro de diálogo conectar con el servidor, escriba la información de conexión del paso 2 y haga clic en **conectar** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="74fdf-168">Para ver el esquema generado, haga clic con el botón derecho en el nombre de la base de datos en Explorador de objetos de SQL Server y seleccione **Actualizar** .</span><span class="sxs-lookup"><span data-stu-id="74fdf-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="74fdf-169">Conservar y recuperar datos</span><span class="sxs-lookup"><span data-stu-id="74fdf-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="74fdf-170">Abra el archivo Program.cs en el que se define el método Main.</span><span class="sxs-lookup"><span data-stu-id="74fdf-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="74fdf-171">Agregue el código siguiente a la función main.</span><span class="sxs-lookup"><span data-stu-id="74fdf-171">Add the following code into the Main function.</span></span> <span data-ttu-id="74fdf-172">El código agrega un nuevo objeto Department al contexto.</span><span class="sxs-lookup"><span data-stu-id="74fdf-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="74fdf-173">Después guarda los datos.</span><span class="sxs-lookup"><span data-stu-id="74fdf-173">It then saves the data.</span></span> <span data-ttu-id="74fdf-174">El código también ejecuta una consulta LINQ que devuelve un departamento en el que el nombre es DepartmentNames. English.</span><span class="sxs-lookup"><span data-stu-id="74fdf-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="74fdf-175">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="74fdf-175">Compile and run the application.</span></span> <span data-ttu-id="74fdf-176">El programa produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="74fdf-176">The program produces the following output:</span></span>

```console
DepartmentID: 1 Name: English
```

<span data-ttu-id="74fdf-177">Para ver los datos de la base de datos, haga clic con el botón derecho en el nombre de la base de datos en Explorador de objetos de SQL Server y seleccione **Actualizar**.</span><span class="sxs-lookup"><span data-stu-id="74fdf-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="74fdf-178">A continuación, haga clic con el botón secundario del mouse en la tabla y seleccione **ver datos**.</span><span class="sxs-lookup"><span data-stu-id="74fdf-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="74fdf-179">Resumen</span><span class="sxs-lookup"><span data-stu-id="74fdf-179">Summary</span></span>

<span data-ttu-id="74fdf-180">En este tutorial, hemos visto cómo asignar tipos de enumeración mediante el Entity Framework Designer y cómo usar las enumeraciones en el código.</span><span class="sxs-lookup"><span data-stu-id="74fdf-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
