---
title: 'Consulta del diseñador procedimientos almacenados: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 6284b10261e6f3b9bf69d1c15e121988e4976d48
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489952"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="5e436-102">Consulta del Diseñador de procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="5e436-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="5e436-103">En este tutorial paso a paso muestra cómo usar el Diseñador de Entity Framework (EF Designer) para importar los procedimientos almacenados en un modelo y, a continuación, llamar a los procedimientos almacenados importados para recuperar los resultados.</span><span class="sxs-lookup"><span data-stu-id="5e436-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="5e436-104">Tenga en cuenta que Code First no admite la asignación a procedimientos almacenados o funciones.</span><span class="sxs-lookup"><span data-stu-id="5e436-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="5e436-105">Sin embargo, puede llamar a procedimientos almacenados o funciones mediante el método System.Data.Entity.DbSet.SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="5e436-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="5e436-106">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5e436-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="5e436-107">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="5e436-107">Prerequisites</span></span>

<span data-ttu-id="5e436-108">Para completar este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="5e436-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="5e436-109">Una versión reciente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e436-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="5e436-110">El [base de datos de ejemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="5e436-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="5e436-111">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="5e436-111">Set up the Project</span></span>

-   <span data-ttu-id="5e436-112">Abra Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="5e436-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="5e436-113">Seleccione **archivo -&gt; nuevo:&gt; proyecto**</span><span class="sxs-lookup"><span data-stu-id="5e436-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="5e436-114">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla.</span><span class="sxs-lookup"><span data-stu-id="5e436-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="5e436-115">Escriba **EFwithSProcsSample** como el nombre.</span><span class="sxs-lookup"><span data-stu-id="5e436-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="5e436-116">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5e436-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="5e436-117">Crear un modelo</span><span class="sxs-lookup"><span data-stu-id="5e436-117">Create a Model</span></span>

-   <span data-ttu-id="5e436-118">Haga clic en el proyecto en el Explorador de soluciones y seleccione **Add -&gt; nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="5e436-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="5e436-119">Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="5e436-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="5e436-120">Escriba **EFwithSProcsModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="5e436-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="5e436-121">En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="5e436-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="5e436-122">Haga clic en **nueva conexión**.</span><span class="sxs-lookup"><span data-stu-id="5e436-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="5e436-123">En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5e436-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="5e436-124">El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="5e436-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="5e436-125">En el cuadro de diálogo Elija los objetos de base de datos, compruebe el **tablas** casilla de verificación para seleccionar todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="5e436-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="5e436-126">Además, seleccione los siguientes procedimientos almacenados en el **procedimientos almacenados y funciones** nodo: **GetStudentGrades** y **GetDepartmentName**.</span><span class="sxs-lookup"><span data-stu-id="5e436-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Importar](~/ef6/media/import.jpg)

    <span data-ttu-id="5e436-128">*A partir de Visual Studio 2012, EF Designer admite la importación masiva de los procedimientos almacenados. El **Importar seleccionada procedimientos almacenados y funciones en el modelo de theentity** está activada de forma predeterminada.*</span><span class="sxs-lookup"><span data-stu-id="5e436-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="5e436-129">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="5e436-129">Click **Finish**.</span></span>

<span data-ttu-id="5e436-130">De forma predeterminada, la forma del resultado de cada procedimiento almacenado importada o la función que devuelve más de una columna se convertirá automáticamente en un nuevo tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="5e436-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="5e436-131">En este ejemplo desea asignar los resultados de la **GetStudentGrades** función a la **StudentGrade** entidad y los resultados de la **GetDepartmentName** a **ninguno** (**ninguno** es el valor predeterminado).</span><span class="sxs-lookup"><span data-stu-id="5e436-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="5e436-132">Para que una importación de función devolver un tipo de entidad, las columnas devueltas por el procedimiento almacenado correspondiente deben coincidir exactamente con las propiedades escalares del tipo de entidad devuelta.</span><span class="sxs-lookup"><span data-stu-id="5e436-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="5e436-133">Una importación de función también puede devolver colecciones de tipos simples, tipos complejos o ningún valor.</span><span class="sxs-lookup"><span data-stu-id="5e436-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="5e436-134">Haga clic en la superficie de diseño y seleccione **Explorador de modelos**.</span><span class="sxs-lookup"><span data-stu-id="5e436-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="5e436-135">En **Explorador de modelos**, seleccione **Function Imports**y, a continuación, haga doble clic en el **GetStudentGrades** función.</span><span class="sxs-lookup"><span data-stu-id="5e436-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="5e436-136">En el cuadro de diálogo Editar importación de función, seleccione **entidades** y elija **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="5e436-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="5e436-137">*El **importación de función es ajustable** casilla en la parte superior de la **Function Imports** cuadro de diálogo le permitirá asignar a las funciones que admite composición. Si activa esta casilla, solo las funciones que se pueden componer (funciones con valores de tabla) se mostrarán en el **Stored Procedure / nombre de la función** lista desplegable. Si no activa esta casilla, se mostrará solo las funciones que no se pueden componer en la lista.*</span><span class="sxs-lookup"><span data-stu-id="5e436-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="5e436-138">Usar el modelo</span><span class="sxs-lookup"><span data-stu-id="5e436-138">Use the Model</span></span>

<span data-ttu-id="5e436-139">Abra el **Program.cs** donde el **Main** se define el método.</span><span class="sxs-lookup"><span data-stu-id="5e436-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="5e436-140">Agregue el código siguiente en la función Main.</span><span class="sxs-lookup"><span data-stu-id="5e436-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="5e436-141">El código llama a dos procedimientos almacenados: **GetStudentGrades** (devuelve **StudentGrades** especificado *StudentId*) y **GetDepartmentName** (devuelve el nombre del departamento en el parámetro de salida).</span><span class="sxs-lookup"><span data-stu-id="5e436-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

<span data-ttu-id="5e436-142">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e436-142">Compile and run the application.</span></span> <span data-ttu-id="5e436-143">El programa produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="5e436-143">The program produces the following output:</span></span>

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="5e436-144">Parámetros de salida</span><span class="sxs-lookup"><span data-stu-id="5e436-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="5e436-145">Si se usan parámetros de salida, sus valores no estará disponibles hasta que se han leído completamente los resultados.</span><span class="sxs-lookup"><span data-stu-id="5e436-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="5e436-146">Esto es debido al comportamiento subyacente de DbDataReader, vea [recuperar datos mediante DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="5e436-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
