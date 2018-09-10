---
title: Herencia de TPH diseñador - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 1eb935414b20d6e93e9d470ccc845bc13626ed3a
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250849"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="f331c-102">Herencia de TPH Diseñador</span><span class="sxs-lookup"><span data-stu-id="f331c-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="f331c-103">En este tutorial paso a paso muestra cómo implementar la herencia de tabla por jerarquía (TPH) en el modelo conceptual con Entity Framework Designer (Diseñador de EF).</span><span class="sxs-lookup"><span data-stu-id="f331c-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="f331c-104">Herencia de TPH emplea una tabla de base de datos para mantener los datos para todos los tipos de entidad en una jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="f331c-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="f331c-105">En este tutorial se asignará la tabla Person a tres tipos de entidad: persona (el tipo base), Student (que se deriva de persona) e Instructor (que se deriva de persona).</span><span class="sxs-lookup"><span data-stu-id="f331c-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="f331c-106">Crearemos un modelo conceptual de la base de datos (Database First) y, a continuación, modificar el modelo para implementar la herencia de TPH con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="f331c-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="f331c-107">Es posible asignar a una herencia de TPH con Model First, pero tendría que escribir su propio flujo de trabajo de generación de base de datos que es compleja.</span><span class="sxs-lookup"><span data-stu-id="f331c-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="f331c-108">A continuación, podría asignar este flujo de trabajo la **flujo de trabajo de generación de base de datos** propiedad en EF Designer.</span><span class="sxs-lookup"><span data-stu-id="f331c-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="f331c-109">Una alternativa más sencilla es usar Code First.</span><span class="sxs-lookup"><span data-stu-id="f331c-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="f331c-110">Otras opciones de herencia</span><span class="sxs-lookup"><span data-stu-id="f331c-110">Other Inheritance Options</span></span>

<span data-ttu-id="f331c-111">Tabla por tipo (TPT) es otro tipo de herencia en el que las tablas independientes en la base de datos se asignan a entidades que participan en la herencia.</span><span class="sxs-lookup"><span data-stu-id="f331c-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span>  <span data-ttu-id="f331c-112">Para obtener información acerca de cómo asignar la herencia de tabla por tipo con EF Designer, vea [herencia de TPT de EF Designer](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="f331c-112">For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="f331c-113">Herencia de tipo de tabla por concreto (TPC) y modelos de herencia mixto son compatibles con el tiempo de ejecución de Entity Framework, pero no son compatibles con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="f331c-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="f331c-114">Si desea usar TPC o herencia mixta, tiene dos opciones: usar Code First o editar manualmente el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="f331c-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="f331c-115">Si decide trabajar con el archivo EDMX, la ventana de detalles de asignación se colocará en "modo seguro" y no podrá usar el diseñador para cambiar las asignaciones.</span><span class="sxs-lookup"><span data-stu-id="f331c-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f331c-116">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="f331c-116">Prerequisites</span></span>

<span data-ttu-id="f331c-117">Para completar este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="f331c-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="f331c-118">Una versión reciente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f331c-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="f331c-119">El [base de datos de ejemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="f331c-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="f331c-120">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="f331c-120">Set up the Project</span></span>

-   <span data-ttu-id="f331c-121">Abra Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f331c-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="f331c-122">Seleccione **archivo -&gt; nuevo:&gt; proyecto**</span><span class="sxs-lookup"><span data-stu-id="f331c-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="f331c-123">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla.</span><span class="sxs-lookup"><span data-stu-id="f331c-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="f331c-124">Escriba **TPHDBFirstSample** como el nombre.</span><span class="sxs-lookup"><span data-stu-id="f331c-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="f331c-125">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f331c-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="f331c-126">Crear un modelo</span><span class="sxs-lookup"><span data-stu-id="f331c-126">Create a Model</span></span>

-   <span data-ttu-id="f331c-127">Haga clic en el nombre del proyecto en el Explorador de soluciones y seleccione **Add -&gt; nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="f331c-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="f331c-128">Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="f331c-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="f331c-129">Escriba **TPHModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="f331c-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="f331c-130">En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="f331c-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="f331c-131">Haga clic en **nueva conexión**.</span><span class="sxs-lookup"><span data-stu-id="f331c-131">Click **New Connection**.</span></span>
    <span data-ttu-id="f331c-132">En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f331c-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="f331c-133">El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f331c-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="f331c-134">En el cuadro de diálogo Elija los objetos de base de datos en el nodo tablas, seleccione el **persona** tabla.</span><span class="sxs-lookup"><span data-stu-id="f331c-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="f331c-135">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="f331c-135">Click **Finish**.</span></span>

<span data-ttu-id="f331c-136">Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.</span><span class="sxs-lookup"><span data-stu-id="f331c-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="f331c-137">Todos los objetos que seleccionó en el cuadro de diálogo Elija los objetos de base de datos se agregan al modelo.</span><span class="sxs-lookup"><span data-stu-id="f331c-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="f331c-138">Que es el modo **persona** tabla busca en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f331c-138">That is how the **Person** table looks in the database.</span></span>

![Tabla Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="f331c-140">Implementar la herencia de tabla por jerarquía</span><span class="sxs-lookup"><span data-stu-id="f331c-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="f331c-141">El **persona** tabla tiene el **discriminador** columna, que puede tener uno de dos valores: "Student" e "Instructor".</span><span class="sxs-lookup"><span data-stu-id="f331c-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="f331c-142">Dependiendo del valor el **persona** tabla se asignará a la **estudiante** entidad o la **Instructor** entidad.</span><span class="sxs-lookup"><span data-stu-id="f331c-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="f331c-143">El **persona** tabla también tiene dos columnas, **HireDate** y **EnrollmentDate**, que debe ser **que acepta valores NULL** porque no puede ser una persona un Student e instructor al mismo tiempo (al menos no en este tutorial).</span><span class="sxs-lookup"><span data-stu-id="f331c-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="f331c-144">Agregar nuevas entidades</span><span class="sxs-lookup"><span data-stu-id="f331c-144">Add new Entities</span></span>

-   <span data-ttu-id="f331c-145">Agregar una nueva entidad.</span><span class="sxs-lookup"><span data-stu-id="f331c-145">Add a new entity.</span></span>
    <span data-ttu-id="f331c-146">Para ello, haga doble clic en un área vacía de la superficie de diseño de Entity Framework Designer y seleccione **Add -&gt;entidad**.</span><span class="sxs-lookup"><span data-stu-id="f331c-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="f331c-147">Tipo **Instructor** para el **nombre de entidad** y seleccione **persona** en la lista desplegable para la **tipo Base**.</span><span class="sxs-lookup"><span data-stu-id="f331c-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="f331c-148">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f331c-148">Click **OK**.</span></span>
-   <span data-ttu-id="f331c-149">Agregue otra nueva entidad.</span><span class="sxs-lookup"><span data-stu-id="f331c-149">Add another new entity.</span></span> <span data-ttu-id="f331c-150">Tipo **estudiante** para el **nombre de entidad** y seleccione **persona** en la lista desplegable para la **tipo Base**.</span><span class="sxs-lookup"><span data-stu-id="f331c-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="f331c-151">Se agregaron dos nuevos tipos de entidad a la superficie de diseño.</span><span class="sxs-lookup"><span data-stu-id="f331c-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="f331c-152">Una flecha señala desde los nuevos tipos de entidad a la **persona** tipo de entidad; esto indica que **persona** es el tipo base para los nuevos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="f331c-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="f331c-153">Haga clic en el **HireDate** propiedad de la **persona** entidad.</span><span class="sxs-lookup"><span data-stu-id="f331c-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="f331c-154">Seleccione **cortar** (o utilizar la tecla Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="f331c-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="f331c-155">Haga clic en el **Instructor** entidad y seleccione **pegar** (o utilizar la tecla Ctrl-V).</span><span class="sxs-lookup"><span data-stu-id="f331c-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="f331c-156">Haga clic en el **HireDate** propiedad y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="f331c-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="f331c-157">En el **propiedades** ventana, establezca el **Nullable** propiedad **false**.</span><span class="sxs-lookup"><span data-stu-id="f331c-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="f331c-158">Haga clic en el **EnrollmentDate** propiedad de la **persona** entidad.</span><span class="sxs-lookup"><span data-stu-id="f331c-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="f331c-159">Seleccione **cortar** (o utilizar la tecla Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="f331c-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="f331c-160">Haga clic en el **estudiante** entidad y seleccione **pegar (o clave de uso de la combinación Ctrl-V).**</span><span class="sxs-lookup"><span data-stu-id="f331c-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="f331c-161">Seleccione el **EnrollmentDate** propiedad y establezca el **Nullable** propiedad **false**.</span><span class="sxs-lookup"><span data-stu-id="f331c-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="f331c-162">Seleccione el **persona** tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="f331c-162">Select the **Person** entity type.</span></span> <span data-ttu-id="f331c-163">En el **propiedades** ventana, establezca su **abstracta** propiedad **true**.</span><span class="sxs-lookup"><span data-stu-id="f331c-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="f331c-164">Eliminar el **discriminador** propiedad desde **persona**.</span><span class="sxs-lookup"><span data-stu-id="f331c-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="f331c-165">En la sección siguiente, se explica el motivo debe eliminarse.</span><span class="sxs-lookup"><span data-stu-id="f331c-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="f331c-166">Asigne las entidades</span><span class="sxs-lookup"><span data-stu-id="f331c-166">Map the entities</span></span>

-   <span data-ttu-id="f331c-167">Haga clic en el **Instructor** y seleccione **asignación de tabla.**</span><span class="sxs-lookup"><span data-stu-id="f331c-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="f331c-168">La entidad Instructor está seleccionada en la ventana Detalles de la asignación.</span><span class="sxs-lookup"><span data-stu-id="f331c-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="f331c-169">Haga clic en **&lt;agregar una tabla o vista&gt;** en el **detalles de Mapping** ventana.</span><span class="sxs-lookup"><span data-stu-id="f331c-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="f331c-170">El **&lt;agregar una tabla o vista&gt;** campo se convierte en una lista desplegable de tablas o vistas a que se puede asignar la entidad seleccionada.</span><span class="sxs-lookup"><span data-stu-id="f331c-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="f331c-171">Seleccione **persona** en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="f331c-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="f331c-172">El **detalles de Mapping** ventana se actualiza con las asignaciones de columna predeterminadas y una opción para agregar una condición.</span><span class="sxs-lookup"><span data-stu-id="f331c-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="f331c-173">Haga clic en  **&lt;agregar una condición&gt;**.</span><span class="sxs-lookup"><span data-stu-id="f331c-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="f331c-174">El **&lt;agregar una condición&gt;** campo se convierte en una lista desplegable de columnas para el que se pueden establecer condiciones.</span><span class="sxs-lookup"><span data-stu-id="f331c-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="f331c-175">Seleccione **discriminador** en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="f331c-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="f331c-176">En el **operador** columna de la **detalles de Mapping** ventana, seleccione = de la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="f331c-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="f331c-177">En el **valor/propiedad** columna, escriba **Instructor**.</span><span class="sxs-lookup"><span data-stu-id="f331c-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="f331c-178">El resultado final debe tener este aspecto:</span><span class="sxs-lookup"><span data-stu-id="f331c-178">The end result should look like this:</span></span>

    ![Detalles de la asignación](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="f331c-180">Repita estos pasos para la **estudiante** tipo de entidad, pero asegúrese de la condición igual a **estudiante** valor.</span><span class="sxs-lookup"><span data-stu-id="f331c-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="f331c-181">*La razón por la que desea quitar el **discriminador** propiedad, es porque no se puede asignar una columna de tabla de más de una vez. Esta columna se utilizará para la asignación condicional, por lo que no se puede usar para la asignación de propiedad así. La única manera que puede usarse en ambos casos, si utiliza una condición de un **Is Null** o **Is Not Null** comparación.*</span><span class="sxs-lookup"><span data-stu-id="f331c-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="f331c-182">Ahora se implementa la herencia de tabla por jerarquía.</span><span class="sxs-lookup"><span data-stu-id="f331c-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![TPH final](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="f331c-184">Usar el modelo</span><span class="sxs-lookup"><span data-stu-id="f331c-184">Use the Model</span></span>

<span data-ttu-id="f331c-185">Abra el **Program.cs** donde el **Main** se define el método.</span><span class="sxs-lookup"><span data-stu-id="f331c-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="f331c-186">Pegue el código siguiente en el **Main** función.</span><span class="sxs-lookup"><span data-stu-id="f331c-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="f331c-187">El código ejecuta tres consultas.</span><span class="sxs-lookup"><span data-stu-id="f331c-187">The code executes three queries.</span></span> <span data-ttu-id="f331c-188">La primera consulta devuelve todos los **persona** objetos.</span><span class="sxs-lookup"><span data-stu-id="f331c-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="f331c-189">La segunda consulta utiliza la **OfType** método devuelva **Instructor** objetos.</span><span class="sxs-lookup"><span data-stu-id="f331c-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="f331c-190">La tercera consulta utiliza la **OfType** método devuelva **estudiante** objetos.</span><span class="sxs-lookup"><span data-stu-id="f331c-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
