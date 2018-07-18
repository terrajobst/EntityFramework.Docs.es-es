---
title: Funciones con valores de tabla (TVF) - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
caps.latest.revision: 3
ms.openlocfilehash: 7d652725a2655b691b03aa3f43103753fe72ede7
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122623"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="080c3-102">Funciones con valores de tabla (TVF)</span><span class="sxs-lookup"><span data-stu-id="080c3-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="080c3-103">**EF5 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="080c3-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="080c3-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="080c3-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="080c3-105">El tutorial de vídeo y paso a paso muestra cómo asignar funciones con valores de tabla (TVF) mediante el Diseñador de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="080c3-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="080c3-106">También se muestra cómo llamar a una función TVF desde una consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="080c3-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="080c3-107">TVF están actualmente solo se admite en la base de datos primer flujo de trabajo.</span><span class="sxs-lookup"><span data-stu-id="080c3-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="080c3-108">Compatibilidad TVF se introdujo en Entity Framework versión 5.</span><span class="sxs-lookup"><span data-stu-id="080c3-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="080c3-109">Tenga en cuenta que para usar las nuevas características, como funciones con valores de tabla, enumeraciones y tipos espaciales debe tener como destino .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="080c3-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="080c3-110">Visual Studio 2012 tiene como destino .NET 4.5 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="080c3-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="080c3-111">TVF son muy similares a los procedimientos almacenados con una diferencia importante: el resultado de una función TVF es ajustable.</span><span class="sxs-lookup"><span data-stu-id="080c3-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="080c3-112">Esto significa que los resultados de una función TVF pueden usarse en una consulta LINQ, mientras que los resultados de un procedimiento almacenado no.</span><span class="sxs-lookup"><span data-stu-id="080c3-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="080c3-113">Vea el vídeo</span><span class="sxs-lookup"><span data-stu-id="080c3-113">Watch the video</span></span>

<span data-ttu-id="080c3-114">**Presentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="080c3-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="080c3-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="080c3-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="080c3-116">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="080c3-116">Pre-Requisites</span></span>

<span data-ttu-id="080c3-117">Para completar este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="080c3-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="080c3-118">Instalar el [base de datos School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="080c3-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="080c3-119">Tiene una versión reciente de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="080c3-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="080c3-120">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="080c3-120">Set up the Project</span></span>

1.  <span data-ttu-id="080c3-121">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="080c3-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="080c3-122">En el **archivo** menú, elija **New**y, a continuación, haga clic en **proyecto**</span><span class="sxs-lookup"><span data-stu-id="080c3-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="080c3-123">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla</span><span class="sxs-lookup"><span data-stu-id="080c3-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="080c3-124">Escriba **TVF** como el nombre del proyecto y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="080c3-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="080c3-125">Agregar una función TVF a la base de datos</span><span class="sxs-lookup"><span data-stu-id="080c3-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="080c3-126">Seleccione **vista -&gt; Explorador de objetos SQL Server**</span><span class="sxs-lookup"><span data-stu-id="080c3-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="080c3-127">Si LocalDB no está en la lista de servidores: haga doble clic en **SQL Server** y seleccione **agregar SQL Server** Use el valor predeterminado **Windows autenticación** para conectarse al servidor de LocalDB</span><span class="sxs-lookup"><span data-stu-id="080c3-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="080c3-128">Expanda el nodo de LocalDB</span><span class="sxs-lookup"><span data-stu-id="080c3-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="080c3-129">En el nodo bases de datos, haga clic en el nodo de base de datos School y seleccione **nueva consulta...**</span><span class="sxs-lookup"><span data-stu-id="080c3-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="080c3-130">En el Editor de Transact-SQL, pegue la siguiente definición de función TVF</span><span class="sxs-lookup"><span data-stu-id="080c3-130">In T-SQL Editor, paste the following TVF definition</span></span>

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   <span data-ttu-id="080c3-131">Haga clic en el botón secundario del mouse en el editor de Transact-SQL y seleccione **Execute**</span><span class="sxs-lookup"><span data-stu-id="080c3-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="080c3-132">La función GetStudentGradesForCourse se agrega a la base de datos School</span><span class="sxs-lookup"><span data-stu-id="080c3-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="080c3-133">Crear un modelo</span><span class="sxs-lookup"><span data-stu-id="080c3-133">Create a Model</span></span>

1.  <span data-ttu-id="080c3-134">Haga clic en el nombre del proyecto en el Explorador de soluciones, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**</span><span class="sxs-lookup"><span data-stu-id="080c3-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="080c3-135">Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el **plantillas** panel</span><span class="sxs-lookup"><span data-stu-id="080c3-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="080c3-136">Escriba **TVFModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**</span><span class="sxs-lookup"><span data-stu-id="080c3-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="080c3-137">En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**</span><span class="sxs-lookup"><span data-stu-id="080c3-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="080c3-138">Haga clic en **nueva conexión** ENTRAR **(localdb)\\mssqllocaldb** en el texto de nombre de servidor cuadro ENTRAR **School** haga clic en el nombre de la base de datos **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="080c3-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="080c3-139">En el elija los objetos de base de datos cuadro de diálogo el **tablas** nodo, seleccione el **persona**, **StudentGrade**, y **curso** tablas</span><span class="sxs-lookup"><span data-stu-id="080c3-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="080c3-140">Seleccione el **GetStudentGradesForCourse** función ubicado en el **procedimientos almacenados y funciones** nodo tenga en cuenta que a partir de Visual Studio 2012, Entity Designer le permite importar por lotes los procedimientos almacenados y funciones</span><span class="sxs-lookup"><span data-stu-id="080c3-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="080c3-141">Haga clic en **finalizar**</span><span class="sxs-lookup"><span data-stu-id="080c3-141">Click **Finish**</span></span>
9.  <span data-ttu-id="080c3-142">Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.</span><span class="sxs-lookup"><span data-stu-id="080c3-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="080c3-143">Todos los objetos que seleccionó en el **elija los objetos de base de datos** cuadro de diálogo se agregan al modelo.</span><span class="sxs-lookup"><span data-stu-id="080c3-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="080c3-144">De forma predeterminada, la forma del resultado de cada procedimiento almacenado importada o la función se convertirá automáticamente en un nuevo tipo complejo en el modelo de entidad.</span><span class="sxs-lookup"><span data-stu-id="080c3-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="080c3-145">Pero queremos asignar los resultados de la función GetStudentGradesForCourse a la entidad StudentGrade: haga clic en la superficie de diseño y seleccione **Explorador de modelos** en el Explorador de modelos, seleccione **Function Imports**y, a continuación, haga doble clic en el **GetStudentGradesForCourse** función en el Editar función cuadro de diálogo Importar, seleccione **entidades** y elija **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="080c3-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="080c3-146">Conservar y recuperar datos</span><span class="sxs-lookup"><span data-stu-id="080c3-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="080c3-147">Abra el archivo donde se define el método Main.</span><span class="sxs-lookup"><span data-stu-id="080c3-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="080c3-148">Agregue el código siguiente en la función Main.</span><span class="sxs-lookup"><span data-stu-id="080c3-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="080c3-149">El código siguiente muestra cómo crear una consulta que utiliza una función con valores de tabla.</span><span class="sxs-lookup"><span data-stu-id="080c3-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="080c3-150">La consulta proyecta los resultados en un tipo anónimo que contiene el título del curso relacionado y los alumnos relacionados con un grado mayor o igual a la versión 3.5.</span><span class="sxs-lookup"><span data-stu-id="080c3-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

<span data-ttu-id="080c3-151">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="080c3-151">Compile and run the application.</span></span> <span data-ttu-id="080c3-152">El programa produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="080c3-152">The program produces the following output:</span></span>

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="080c3-153">Resumen</span><span class="sxs-lookup"><span data-stu-id="080c3-153">Summary</span></span>

<span data-ttu-id="080c3-154">En este tutorial hemos examinado cómo asignar funciones con valores de tabla (TVF) mediante el Diseñador de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="080c3-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="080c3-155">También muestra cómo llamar a una función TVF desde una consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="080c3-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
