---
title: 'Espacial EF6 - EF Designer:'
author: divega
ms.date: 2016-10-23
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: 39fe54ffe9d0ffd1753844f7bffcd1832d82eec5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994372"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="7b500-102">Espacial - EF Designer</span><span class="sxs-lookup"><span data-stu-id="7b500-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="7b500-103">**EF5 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="7b500-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="7b500-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="7b500-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="7b500-105">El tutorial de vídeo y paso a paso muestra cómo asignar tipos espaciales con Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="7b500-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="7b500-106">También se muestra cómo usar una consulta LINQ para buscar una distancia entre dos ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="7b500-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="7b500-107">En este tutorial usará Model First para crear una nueva base de datos, pero también se puede usar el Diseñador de EF con el [Database First](~/ef6/modeling/designer/workflows/database-first.md) flujo de trabajo para asignar a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="7b500-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="7b500-108">Compatibilidad con tipos espaciales se introdujo en Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="7b500-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="7b500-109">Tenga en cuenta que para utilizar las nuevas características, como el tipo espacial, enumeraciones y funciones con valores de tabla, debe tener como destino .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="7b500-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="7b500-110">Visual Studio 2012 tiene como destino .NET 4.5 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7b500-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="7b500-111">Para utilizar tipos de datos espaciales también debe usar un proveedor de Entity Framework que tiene compatibilidad espacial.</span><span class="sxs-lookup"><span data-stu-id="7b500-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="7b500-112">Consulte [compatibilidad del proveedor con tipos espaciales](~/ef6/fundamentals/providers/spatial-support.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="7b500-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="7b500-113">Hay dos tipos principales de los datos espaciales: geography y geometry.</span><span class="sxs-lookup"><span data-stu-id="7b500-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="7b500-114">El tipo de datos geography almacena datos elípticos (por ejemplo, coordenadas GPS latitud y longitud).</span><span class="sxs-lookup"><span data-stu-id="7b500-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="7b500-115">El tipo de datos geometry representa el sistema de coordenadas euclidiano (plano).</span><span class="sxs-lookup"><span data-stu-id="7b500-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="7b500-116">Vea el vídeo</span><span class="sxs-lookup"><span data-stu-id="7b500-116">Watch the video</span></span>
<span data-ttu-id="7b500-117">Este vídeo muestra cómo asignar tipos espaciales con Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="7b500-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="7b500-118">También se muestra cómo usar una consulta LINQ para buscar una distancia entre dos ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="7b500-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="7b500-119">**Presentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="7b500-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="7b500-120">**Vídeo**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="7b500-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="7b500-121">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7b500-121">Pre-Requisites</span></span>

<span data-ttu-id="7b500-122">Necesitará tener Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition instalado para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="7b500-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="7b500-123">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="7b500-123">Set up the Project</span></span>

1.  <span data-ttu-id="7b500-124">Abra Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7b500-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="7b500-125">En el **archivo** menú, elija **New**y, a continuación, haga clic en **proyecto**</span><span class="sxs-lookup"><span data-stu-id="7b500-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="7b500-126">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla</span><span class="sxs-lookup"><span data-stu-id="7b500-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="7b500-127">Escriba **SpatialEFDesigner** como el nombre del proyecto y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="7b500-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="7b500-128">Crear un nuevo modelo con EF Designer</span><span class="sxs-lookup"><span data-stu-id="7b500-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="7b500-129">Haga clic en el nombre del proyecto en el Explorador de soluciones, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**</span><span class="sxs-lookup"><span data-stu-id="7b500-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="7b500-130">Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel de plantillas</span><span class="sxs-lookup"><span data-stu-id="7b500-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="7b500-131">Escriba **UniversityModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**</span><span class="sxs-lookup"><span data-stu-id="7b500-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="7b500-132">En la página del Asistente para Entity Data Model, seleccione **modelo vacío** en el cuadro de diálogo Elegir contenido del modelo</span><span class="sxs-lookup"><span data-stu-id="7b500-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="7b500-133">Haga clic en **finalizar**</span><span class="sxs-lookup"><span data-stu-id="7b500-133">Click **Finish**</span></span>

<span data-ttu-id="7b500-134">Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.</span><span class="sxs-lookup"><span data-stu-id="7b500-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="7b500-135">El asistente realiza las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="7b500-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="7b500-136">Genera el archivo EnumTestModel.edmx que define el modelo conceptual, el modelo de almacenamiento y la asignación entre ellos.</span><span class="sxs-lookup"><span data-stu-id="7b500-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="7b500-137">Establece la propiedad Metadata Artifact Processing del archivo .edmx para insertar en el ensamblado de salida para que los archivos de metadatos generados se incrustan en el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="7b500-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="7b500-138">Agrega una referencia a los ensamblados siguientes: Entity Framework, System.ComponentModel.DataAnnotations y System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="7b500-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="7b500-139">Crea los archivos UniversityModel.tt y UniversityModel.Context.tt y los agrega en el archivo .edmx.</span><span class="sxs-lookup"><span data-stu-id="7b500-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="7b500-140">Estos archivos de plantilla T4 generarán el código que define el tipo de DbContext derivado y tipos POCO que se asignan a las entidades en el modelo edmx</span><span class="sxs-lookup"><span data-stu-id="7b500-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="7b500-141">Agregar un nuevo tipo de entidad</span><span class="sxs-lookup"><span data-stu-id="7b500-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="7b500-142">Haga clic en un área vacía de la superficie de diseño, seleccione **Add -&gt; entidad**, aparece el cuadro de diálogo New Entity</span><span class="sxs-lookup"><span data-stu-id="7b500-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="7b500-143">Especificar **Universidad** para el tipo de nombre y especificar **ID** para el nombre de propiedad de clave, deje el tipo como **Int32**</span><span class="sxs-lookup"><span data-stu-id="7b500-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="7b500-144">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7b500-144">Click **OK**</span></span>
4.  <span data-ttu-id="7b500-145">Haga clic en la entidad y seleccione **Add New -&gt; propiedad escalar**</span><span class="sxs-lookup"><span data-stu-id="7b500-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="7b500-146">Cambiar el nombre de la nueva propiedad a **nombre**</span><span class="sxs-lookup"><span data-stu-id="7b500-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="7b500-147">Agregar otra propiedad escalar y cambie su nombre a **ubicación** abrir la ventana Propiedades y cambie el tipo de la nueva propiedad a **Geography**</span><span class="sxs-lookup"><span data-stu-id="7b500-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="7b500-148">Guardar el modelo y compile el proyecto</span><span class="sxs-lookup"><span data-stu-id="7b500-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="7b500-149">Cuando se compila, las advertencias sobre sin asignar entidades y asociaciones pueden aparecer en la lista de errores.</span><span class="sxs-lookup"><span data-stu-id="7b500-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="7b500-150">Puede omitir estas advertencias porque una vez que se elija Generar la base de datos del modelo, los errores desaparecerán.</span><span class="sxs-lookup"><span data-stu-id="7b500-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="7b500-151">Generar la base de datos de modelo</span><span class="sxs-lookup"><span data-stu-id="7b500-151">Generate Database from Model</span></span>

<span data-ttu-id="7b500-152">Ahora podemos generar una base de datos que se basa en el modelo.</span><span class="sxs-lookup"><span data-stu-id="7b500-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="7b500-153">Haga clic en un espacio vacío en el Entity Designer superficie y seleccione **generar base de datos de modelo**</span><span class="sxs-lookup"><span data-stu-id="7b500-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="7b500-154">Haga clic en mostrará la elija el cuadro de diálogo de conexión de datos del Asistente para generar base de datos es el **nueva conexión** botón especifique **(localdb)\\mssqllocaldb** para el nombre del servidor y  **Universidad** para la base de datos y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="7b500-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="7b500-155">Se abrirá, haga clic en un cuadro de diálogo que le pregunta si desea crear una nueva base de datos **Sí**.</span><span class="sxs-lookup"><span data-stu-id="7b500-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="7b500-156">Haga clic en **siguiente** y Asistente para crear base de datos genera el lenguaje de definición de datos (DDL) para crear una base de datos de la DDL generada se muestra en el resumen y una nota de cuadro de diálogo Configuración, el DDL no contiene una definición para un tabla que se asigna al tipo de enumeración</span><span class="sxs-lookup"><span data-stu-id="7b500-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="7b500-157">Haga clic en **finalizar** no ejecuta el script DDL al hacer clic en Finalizar.</span><span class="sxs-lookup"><span data-stu-id="7b500-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="7b500-158">El Asistente para crear base de datos hace lo siguiente: abre el **UniversityModel.edmx.sql** en T-SQL, Editor genera las secciones de esquema y asignación de almacén de EDMX archivo agrega información de la cadena de conexión al archivo App.config</span><span class="sxs-lookup"><span data-stu-id="7b500-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="7b500-159">Haga clic en el botón secundario del mouse en el Editor de Transact-SQL y seleccione **Execute** aparece el diálogo Conectar a servidor, escriba la información de conexión del paso 2 y haga clic en **Connect**</span><span class="sxs-lookup"><span data-stu-id="7b500-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="7b500-160">Para ver el esquema generado, haga doble clic en el nombre de base de datos en el Explorador de objetos de SQL Server y seleccione **actualizar**</span><span class="sxs-lookup"><span data-stu-id="7b500-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="7b500-161">Conservar y recuperar datos</span><span class="sxs-lookup"><span data-stu-id="7b500-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="7b500-162">Abra el archivo Program.cs, donde se define el método Main.</span><span class="sxs-lookup"><span data-stu-id="7b500-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="7b500-163">Agregue el código siguiente en la función Main.</span><span class="sxs-lookup"><span data-stu-id="7b500-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="7b500-164">El código agrega dos nuevos objetos de la Universidad del contexto.</span><span class="sxs-lookup"><span data-stu-id="7b500-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="7b500-165">Propiedades espaciales se inicializan mediante el método DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="7b500-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="7b500-166">Punto de geografía que se representan como WellKnownText se pasa al método.</span><span class="sxs-lookup"><span data-stu-id="7b500-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="7b500-167">El código, a continuación, guarda los datos.</span><span class="sxs-lookup"><span data-stu-id="7b500-167">The code then saves the data.</span></span> <span data-ttu-id="7b500-168">A continuación, la consulta LINQ que devuelve un objeto de la Universidad donde su ubicación es más cercana a la ubicación especificada, se genera y ejecuta.</span><span class="sxs-lookup"><span data-stu-id="7b500-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="7b500-169">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7b500-169">Compile and run the application.</span></span> <span data-ttu-id="7b500-170">El programa produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="7b500-170">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="7b500-171">Para ver los datos en la base de datos, haga doble clic en el nombre de base de datos en el Explorador de objetos de SQL Server y seleccione **actualizar**.</span><span class="sxs-lookup"><span data-stu-id="7b500-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="7b500-172">A continuación, haga clic en el botón secundario del mouse en la tabla y seleccione **ver datos**.</span><span class="sxs-lookup"><span data-stu-id="7b500-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="7b500-173">Resumen</span><span class="sxs-lookup"><span data-stu-id="7b500-173">Summary</span></span>

<span data-ttu-id="7b500-174">En este tutorial explicamos cómo asignar tipos espaciales mediante el Diseñador de Entity Framework y cómo usar tipos espaciales en el código.</span><span class="sxs-lookup"><span data-stu-id="7b500-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
