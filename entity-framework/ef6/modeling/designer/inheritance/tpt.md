---
title: Herencia de TPT diseñador - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
caps.latest.revision: 3
ms.openlocfilehash: c3ccb44f931b830a96a553d5af1e722a9ca4bbf0
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122376"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="9e0d7-102">Herencia de TPT Diseñador</span><span class="sxs-lookup"><span data-stu-id="9e0d7-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="9e0d7-103">Este tutorial paso a paso muestra cómo implementar la herencia de tabla por tipo (TPT) en el modelo usando el Diseñador de Entity Framework (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="9e0d7-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="9e0d7-104">La herencia de tabla por tipo utiliza una tabla independiente de la base de datos para mantener los datos de las propiedades no heredadas y de las propiedades de clave para cada tipo de la jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="9e0d7-105">En este tutorial se asignará el **curso** (tipo base), **OnsiteCourse** (se deriva de curso), y **OnsiteCourse** (se deriva de **curso**) entidades a tablas con los mismos nombres.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="9e0d7-106">Crearemos un modelo de la base de datos y, a continuación, modificar el modelo para implementar la herencia de TPT.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="9e0d7-107">Puede también iniciar con Model First y, a continuación, generar la base de datos del modelo.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="9e0d7-108">EF Designer usa la estrategia de TPT de forma predeterminada y, por lo que cualquier herencia en el modelo se asignan a tablas independientes.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="9e0d7-109">Otras opciones de herencia</span><span class="sxs-lookup"><span data-stu-id="9e0d7-109">Other Inheritance Options</span></span>

<span data-ttu-id="9e0d7-110">Tabla por jerarquía (TPH) es otro tipo de herencia en la base de datos de una tabla se utiliza para mantener los datos de todos los tipos de entidad en una jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="9e0d7-111">Para obtener información acerca de cómo asignar la herencia de tabla por jerarquía con Entity Designer, vea [la herencia de TPH de EF Designer](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="9e0d7-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="9e0d7-112">Tenga en cuenta que, la tabla concreta por tipo de herencia (TPC) y herencia mixta modelos son compatibles con el tiempo de ejecución de Entity Framework, pero no son compatibles con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="9e0d7-113">Si desea usar TPC o herencia mixta, tiene dos opciones: usar Code First o editar manualmente el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="9e0d7-114">Si decide trabajar con el archivo EDMX, la ventana de detalles de asignación se colocará en "modo seguro" y no podrá usar el diseñador para cambiar las asignaciones.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e0d7-115">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="9e0d7-115">Prerequisites</span></span>

<span data-ttu-id="9e0d7-116">Para completar este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="9e0d7-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="9e0d7-117">Una versión reciente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="9e0d7-118">El [base de datos de ejemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="9e0d7-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="9e0d7-119">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="9e0d7-119">Set up the Project</span></span>

-   <span data-ttu-id="9e0d7-120">Abra Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="9e0d7-121">Seleccione **archivo -&gt; nuevo:&gt; proyecto**</span><span class="sxs-lookup"><span data-stu-id="9e0d7-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="9e0d7-122">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="9e0d7-123">Escriba **TPTDBFirstSample** como el nombre.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="9e0d7-124">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="9e0d7-125">Crear un modelo</span><span class="sxs-lookup"><span data-stu-id="9e0d7-125">Create a Model</span></span>

-   <span data-ttu-id="9e0d7-126">Haga clic en el proyecto en el Explorador de soluciones y seleccione **Add -&gt; nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="9e0d7-127">Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="9e0d7-128">Escriba **TPTModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="9e0d7-129">En el cuadro de diálogo Elegir contenido del modelo, seleccione ** Generar desde la base de datos ** y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="9e0d7-130">Haga clic en **nueva conexión**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-130">Click **New Connection**.</span></span>
    <span data-ttu-id="9e0d7-131">En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="9e0d7-132">El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="9e0d7-133">En el cuadro de diálogo Elija los objetos de base de datos en el nodo tablas, seleccione el **departamento**, **curso OnsiteCourse y OnsiteCourse** tablas.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="9e0d7-134">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-134">Click **Finish**.</span></span>

<span data-ttu-id="9e0d7-135">Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="9e0d7-136">Todos los objetos que seleccionó en el cuadro de diálogo Elija los objetos de base de datos se agregan al modelo.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="9e0d7-137">Implementar la herencia de tabla por tipo</span><span class="sxs-lookup"><span data-stu-id="9e0d7-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="9e0d7-138">En la superficie de diseño, haga clic en el **OnsiteCourse** tipo de entidad y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="9e0d7-139">En el **propiedades** ventana, establezca la propiedad de tipo Base en **curso**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="9e0d7-140">Haga clic en el **OnsiteCourse** tipo de entidad y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="9e0d7-141">En el **propiedades** ventana, establezca la propiedad de tipo Base en **curso**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="9e0d7-142">Haga clic en la asociación (la línea) entre el **OnsiteCourse** y **curso** tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="9e0d7-143">Seleccione **eliminar del modelo**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="9e0d7-144">Haga clic en la asociación entre el **OnsiteCourse** y **curso** tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="9e0d7-145">Seleccione **eliminar del modelo**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="9e0d7-146">Ahora eliminaremos el **CourseID** propiedad desde **OnsiteCourse** y **OnsiteCourse** porque estas clases heredan **CourseID** desde el **curso** tipo base.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="9e0d7-147">Haga clic en el **CourseID** propiedad de la **OnsiteCourse** tipo de entidad y, a continuación, seleccione **eliminar del modelo**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="9e0d7-148">Haga clic en el **CourseID** propiedad de la **OnsiteCourse** tipo de entidad y, a continuación, seleccione **eliminar del modelo**</span><span class="sxs-lookup"><span data-stu-id="9e0d7-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="9e0d7-149">Ya está implementada la herencia de tabla por tipo.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="9e0d7-151">Usar el modelo</span><span class="sxs-lookup"><span data-stu-id="9e0d7-151">Use the Model</span></span>

<span data-ttu-id="9e0d7-152">Abra el **Program.cs** donde el **Main** se define el método.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="9e0d7-153">Pegue el código siguiente en el **Main** función.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="9e0d7-154">El código ejecuta tres consultas.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-154">The code executes three queries.</span></span> <span data-ttu-id="9e0d7-155">La primera consulta devuelve todos los **cursos** relacionadas con el departamento especificado.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="9e0d7-156">La segunda consulta utiliza la **OfType** método devuelva **Online** relacionadas con el departamento especificado.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="9e0d7-157">Devuelve la tercera consulta **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="9e0d7-157">The third query returns **OnsiteCourses**.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
