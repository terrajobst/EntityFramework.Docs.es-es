---
title: 'Herencia de diseñador TPT: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415050"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="fbb00-102">Herencia de diseñador TPT</span><span class="sxs-lookup"><span data-stu-id="fbb00-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="fbb00-103">En este tutorial paso a paso se muestra cómo implementar la herencia de tabla por tipo (TPT) en el modelo mediante el Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="fbb00-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="fbb00-104">La herencia de tabla por tipo utiliza una tabla independiente de la base de datos para mantener los datos de las propiedades no heredadas y de las propiedades de clave para cada tipo de la jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="fbb00-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="fbb00-105">En este tutorial, asignaremos las entidades **Course** (tipo base), **OnlineCourse** (se deriva de course) y **OnsiteCourse** (se deriva de **Course**) a tablas con los mismos nombres.</span><span class="sxs-lookup"><span data-stu-id="fbb00-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="fbb00-106">Vamos a crear un modelo a partir de la base de datos y, a continuación, modificaremos el modelo para implementar la herencia de TPT.</span><span class="sxs-lookup"><span data-stu-id="fbb00-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="fbb00-107">También puede empezar con el Model First y, a continuación, generar la base de datos a partir del modelo.</span><span class="sxs-lookup"><span data-stu-id="fbb00-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="fbb00-108">El diseñador de EF usa la estrategia TPT de forma predeterminada, por lo que cualquier herencia del modelo se asignará a tablas independientes.</span><span class="sxs-lookup"><span data-stu-id="fbb00-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="fbb00-109">Otras opciones de herencia</span><span class="sxs-lookup"><span data-stu-id="fbb00-109">Other Inheritance Options</span></span>

<span data-ttu-id="fbb00-110">Tabla por jerarquía (TPH) es otro tipo de herencia en la que se utiliza una tabla de base de datos para mantener los datos de todos los tipos de entidad en una jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="fbb00-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span><span data-ttu-id="fbb00-111">  Para obtener información sobre cómo asignar la herencia de tabla por jerarquía con Entity Designer, vea la [herencia TPH del diseñador de EF](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="fbb00-111">  For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="fbb00-112">Tenga en cuenta que, la herencia de tipos por hormigón (TPC) y los modelos de herencia mixtos son compatibles con el tiempo de ejecución de Entity Framework pero no son compatibles con el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="fbb00-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="fbb00-113">Si desea utilizar la herencia de TPC o mixta, tiene dos opciones: usar Code First o editar manualmente el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="fbb00-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="fbb00-114">Si decide trabajar con el archivo EDMX, la ventana detalles de la asignación se colocará en "modo seguro" y no podrá usar el diseñador para cambiar las asignaciones.</span><span class="sxs-lookup"><span data-stu-id="fbb00-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fbb00-115">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="fbb00-115">Prerequisites</span></span>

<span data-ttu-id="fbb00-116">Para completar este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="fbb00-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="fbb00-117">Una versión reciente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fbb00-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="fbb00-118">La [base de datos de ejemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="fbb00-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="fbb00-119">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="fbb00-119">Set up the Project</span></span>

-   <span data-ttu-id="fbb00-120">Abra Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="fbb00-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="fbb00-121">Seleccionar **archivo-&gt; nuevo-&gt; proyecto**</span><span class="sxs-lookup"><span data-stu-id="fbb00-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="fbb00-122">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione la plantilla de **consola** .</span><span class="sxs-lookup"><span data-stu-id="fbb00-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="fbb00-123">Escriba **TPTDBFirstSample** como nombre.</span><span class="sxs-lookup"><span data-stu-id="fbb00-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="fbb00-124">Seleccione  **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="fbb00-125">Creación de un modelo</span><span class="sxs-lookup"><span data-stu-id="fbb00-125">Create a Model</span></span>

-   <span data-ttu-id="fbb00-126">Haga clic con el botón derecho en el proyecto en Explorador de soluciones y seleccione **agregar&gt; nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="fbb00-127">Seleccione **datos** en el menú de la izquierda y, a continuación, seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="fbb00-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="fbb00-128">Escriba **TPTModel. edmx** como nombre de archivo y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="fbb00-129">En el cuadro de diálogo elegir contenido del modelo, seleccione ** generar desde la base de datos**y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="fbb00-130">Haga clic en **nueva conexión**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-130">Click **New Connection**.</span></span>
    <span data-ttu-id="fbb00-131">En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **LocalDB)\\mssqllocaldb**), seleccione el método de autenticación, escriba **School** para el nombre de la base de datos y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="fbb00-132">El cuadro de diálogo elegir la conexión de datos se actualiza con la configuración de conexión de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fbb00-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="fbb00-133">En el cuadro de diálogo elija los objetos de base de datos, en el nodo tablas, seleccione las tablas **Department**, **Course, OnlineCourse y OnsiteCourse** .</span><span class="sxs-lookup"><span data-stu-id="fbb00-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="fbb00-134">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-134">Click **Finish**.</span></span>

<span data-ttu-id="fbb00-135">Se muestra el diseñador de entidades, que proporciona una superficie de diseño para editar el modelo.</span><span class="sxs-lookup"><span data-stu-id="fbb00-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="fbb00-136">Todos los objetos que seleccionó en el cuadro de diálogo elija los objetos de base de datos se agregan al modelo.</span><span class="sxs-lookup"><span data-stu-id="fbb00-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="fbb00-137">Implementar la herencia de tabla por tipo</span><span class="sxs-lookup"><span data-stu-id="fbb00-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="fbb00-138">En la superficie de diseño, haga clic con el botón secundario en el tipo de entidad **OnlineCourse** y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="fbb00-139">En la ventana **propiedades** , establezca la propiedad tipo base en **Course**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="fbb00-140">Haga clic con el botón secundario en el tipo de entidad **OnsiteCourse** y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="fbb00-141">En la ventana **propiedades** , establezca la propiedad tipo base en **Course**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="fbb00-142">Haga clic con el botón secundario en la Asociación (la línea) entre los tipos de entidad **OnlineCourse** y **Course** .</span><span class="sxs-lookup"><span data-stu-id="fbb00-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="fbb00-143">Seleccione **eliminar del modelo**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="fbb00-144">Haga clic con el botón secundario en la asociación entre los tipos de entidad **OnsiteCourse** y **Course** .</span><span class="sxs-lookup"><span data-stu-id="fbb00-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="fbb00-145">Seleccione **eliminar del modelo**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="fbb00-146">Ahora eliminaremos la propiedad **CourseID** de **OnlineCourse** y **OnsiteCourse** , ya que estas clases heredan **courseid** del tipo base **Course** .</span><span class="sxs-lookup"><span data-stu-id="fbb00-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="fbb00-147">Haga clic con el botón secundario en la propiedad **CourseID** del tipo de entidad **OnlineCourse** y, a continuación, seleccione **eliminar del modelo**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="fbb00-148">Haga clic con el botón secundario en la propiedad **CourseID** del tipo de entidad **OnsiteCourse** y, a continuación, seleccione **eliminar del modelo** .</span><span class="sxs-lookup"><span data-stu-id="fbb00-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="fbb00-149">Ya está implementada la herencia de tabla por tipo.</span><span class="sxs-lookup"><span data-stu-id="fbb00-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="fbb00-151">Usar el modelo</span><span class="sxs-lookup"><span data-stu-id="fbb00-151">Use the Model</span></span>

<span data-ttu-id="fbb00-152">Abra el archivo **Program.CS** en el que se define el método **Main** .</span><span class="sxs-lookup"><span data-stu-id="fbb00-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="fbb00-153">Pegue el código siguiente en la función **Main** .</span><span class="sxs-lookup"><span data-stu-id="fbb00-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="fbb00-154">El código ejecuta tres consultas.</span><span class="sxs-lookup"><span data-stu-id="fbb00-154">The code executes three queries.</span></span> <span data-ttu-id="fbb00-155">La primera consulta devuelve todos los **cursos** relacionados con el Departamento especificado.</span><span class="sxs-lookup"><span data-stu-id="fbb00-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="fbb00-156">La segunda consulta utiliza el método **Intype** para devolver **OnlineCourses** relacionado con el Departamento especificado.</span><span class="sxs-lookup"><span data-stu-id="fbb00-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="fbb00-157">La tercera consulta devuelve **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="fbb00-157">The third query returns **OnsiteCourses**.</span></span>

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
