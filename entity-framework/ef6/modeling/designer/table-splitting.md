---
title: 'División de la tabla del diseñador: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921777"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="3eed6-102">División de tablas del diseñador</span><span class="sxs-lookup"><span data-stu-id="3eed6-102">Designer Table Splitting</span></span>
<span data-ttu-id="3eed6-103">En este tutorial se muestra cómo asignar varios tipos de entidad a una sola tabla modificando un modelo con el Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="3eed6-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="3eed6-104">Uno de los motivos por los que puede querer usar la división de tablas es retrasar la carga de algunas propiedades al usar la carga diferida para cargar los objetos.</span><span class="sxs-lookup"><span data-stu-id="3eed6-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span><span data-ttu-id="3eed6-105"> Puede separar las propiedades que pueden contener una gran cantidad de datos en una entidad independiente y solo cargarlos cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="3eed6-105"> You can separate the properties that might contain very large amount of data into a separate entity and only load it when required.</span></span>

<span data-ttu-id="3eed6-106">En la imagen siguiente se muestran las ventanas principales que se usan al trabajar con el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="3eed6-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="3eed6-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3eed6-108">Prerequisites</span></span>

<span data-ttu-id="3eed6-109">Para completar este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="3eed6-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="3eed6-110">Una versión reciente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3eed6-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="3eed6-111">La [base de datos de ejemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="3eed6-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="3eed6-112">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="3eed6-112">Set up the Project</span></span>

<span data-ttu-id="3eed6-113">En este tutorial se usa Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="3eed6-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="3eed6-114">Abra Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="3eed6-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="3eed6-115">En el menú **Archivo** , elija **Nuevo**y haga clic en **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="3eed6-116">En el panel izquierdo, haga clic en\#Visual C y, a continuación, seleccione la plantilla aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="3eed6-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="3eed6-117">Escriba **TableSplittingSample** como nombre del proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="3eed6-118">Creación de un modelo basado en la base de datos School</span><span class="sxs-lookup"><span data-stu-id="3eed6-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="3eed6-119">Haga clic con el botón secundario en el nombre del proyecto en Explorador de soluciones, seleccione **Agregar**y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="3eed6-120">Seleccione **datos** en el menú de la izquierda y, a continuación, seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="3eed6-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="3eed6-121">Escriba **TableSplittingModel. edmx** como nombre de archivo y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="3eed6-122">En el cuadro de diálogo elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente.**</span><span class="sxs-lookup"><span data-stu-id="3eed6-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="3eed6-123">Haga clic en nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="3eed6-123">Click New Connection.</span></span> <span data-ttu-id="3eed6-124">En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(LocalDB)\\mssqllocaldb**), seleccione el método de autenticación, escriba **School** como nombre de la base de datos y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="3eed6-125">El cuadro de diálogo elegir la conexión de datos se actualiza con la configuración de conexión de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3eed6-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="3eed6-126">En el cuadro de diálogo elija los objetos de base de datos, desactive el nodo **tablas** y Compruebe la tabla **Person** .</span><span class="sxs-lookup"><span data-stu-id="3eed6-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="3eed6-127">Esto agregará la tabla especificada al modelo **School** .</span><span class="sxs-lookup"><span data-stu-id="3eed6-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="3eed6-128">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-128">Click **Finish**.</span></span>

<span data-ttu-id="3eed6-129">Se muestra el diseñador de entidades, que proporciona una superficie de diseño para editar el modelo.</span><span class="sxs-lookup"><span data-stu-id="3eed6-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="3eed6-130">Todos los objetos que seleccionó en el cuadro de diálogo **Elija los objetos** de base de datos se agregan al modelo.</span><span class="sxs-lookup"><span data-stu-id="3eed6-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="3eed6-131">Asignación de dos entidades a una sola tabla</span><span class="sxs-lookup"><span data-stu-id="3eed6-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="3eed6-132">En esta sección dividirá la entidad **Person** en dos entidades y, a continuación, las asignará a una sola tabla.</span><span class="sxs-lookup"><span data-stu-id="3eed6-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="3eed6-133">La entidad **Person** no contiene propiedades que puedan contener grandes cantidades de datos. simplemente se usa como ejemplo.</span><span class="sxs-lookup"><span data-stu-id="3eed6-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="3eed6-134">Haga clic con el botón secundario en un área vacía de la superficie de diseño, seleccione **Agregar nuevo**y haga clic en **entidad**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="3eed6-135">Aparecerá el cuadro de diálogo **nueva entidad** .</span><span class="sxs-lookup"><span data-stu-id="3eed6-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="3eed6-136">Escriba **HireInfo** para el **nombre de entidad** y **PersonID** para el nombre de la **propiedad de clave** .</span><span class="sxs-lookup"><span data-stu-id="3eed6-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="3eed6-137">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-137">Click **OK**.</span></span>
-   <span data-ttu-id="3eed6-138">Se crea y se muestra un nuevo tipo de entidad en la superficie de diseño.</span><span class="sxs-lookup"><span data-stu-id="3eed6-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="3eed6-139">Seleccione la propiedad **HireDate** del tipo de entidad **Person** y presione las teclas **Ctrl + X** .</span><span class="sxs-lookup"><span data-stu-id="3eed6-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="3eed6-140">Seleccione la entidad **HireInfo** y presione las teclas **Ctrl + V** .</span><span class="sxs-lookup"><span data-stu-id="3eed6-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="3eed6-141">Cree una asociación entre **Person** y **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="3eed6-142">Para ello, haga clic con el botón secundario en un área vacía de la superficie de diseño, seleccione **Agregar nuevo**y haga clic en **Asociación**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="3eed6-143">Aparece el cuadro de diálogo **Agregar Asociación** .</span><span class="sxs-lookup"><span data-stu-id="3eed6-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="3eed6-144">El nombre de **PersonHireInfo** se proporciona de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="3eed6-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="3eed6-145">Especifique Multiplicity **1 (uno)** en ambos extremos de la relación.</span><span class="sxs-lookup"><span data-stu-id="3eed6-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="3eed6-146">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-146">Press **OK**.</span></span>

<span data-ttu-id="3eed6-147">El paso siguiente requiere la ventana **detalles** de la asignación.</span><span class="sxs-lookup"><span data-stu-id="3eed6-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="3eed6-148">Si no puede ver esta ventana, haga clic con el botón secundario en la superficie de diseño y seleccione detalles de la **asignación**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="3eed6-149">Seleccione el tipo de entidad **HireInfo** y haga clic **&lt;en agregar una&gt;tabla o vista** en la ventana **detalles** de la asignación.</span><span class="sxs-lookup"><span data-stu-id="3eed6-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="3eed6-150">Seleccione **persona** en la  **&lt;lista desplegable agregar una&gt;tabla o vista**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="3eed6-151">La lista contiene tablas o vistas a las que se puede asignar la entidad seleccionada.</span><span class="sxs-lookup"><span data-stu-id="3eed6-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="3eed6-152">Las propiedades adecuadas deben estar asignadas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="3eed6-152">The appropriate properties should be mapped by default.</span></span>

    ![Asignación](~/ef6/media/mapping.png)

-   <span data-ttu-id="3eed6-154">Seleccione la Asociación **PersonHireInfo** en la superficie de diseño.</span><span class="sxs-lookup"><span data-stu-id="3eed6-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="3eed6-155">Haga clic con el botón secundario en la asociación en la superficie de diseño y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="3eed6-156">En la ventana **propiedades** , seleccione la propiedad **restricciones referenciales** y haga clic en el botón de puntos suspensivos.</span><span class="sxs-lookup"><span data-stu-id="3eed6-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="3eed6-157">Seleccione **persona** en la lista desplegable **principal** .</span><span class="sxs-lookup"><span data-stu-id="3eed6-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="3eed6-158">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3eed6-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="3eed6-159">Usar el modelo</span><span class="sxs-lookup"><span data-stu-id="3eed6-159">Use the Model</span></span>

-   <span data-ttu-id="3eed6-160">Pegue el código siguiente en el método Main.</span><span class="sxs-lookup"><span data-stu-id="3eed6-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="3eed6-161">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3eed6-161">Compile and run the application.</span></span>

<span data-ttu-id="3eed6-162">Las siguientes instrucciones T-SQL se ejecutaron en la base de datos **School** como resultado de la ejecución de esta aplicación.</span><span class="sxs-lookup"><span data-stu-id="3eed6-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="3eed6-163">La siguiente **inserción** se ejecutó como resultado de la ejecución del contexto. SaveChanges () y combina datos de las entidades **Person** y **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="3eed6-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insertar](~/ef6/media/insert.png)

-   <span data-ttu-id="3eed6-165">La siguiente **selección** se ejecutó como resultado de la ejecución del contexto. People. FirstOrDefault () y selecciona solo las columnas asignadas a **Person**</span><span class="sxs-lookup"><span data-stu-id="3eed6-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Seleccione 1](~/ef6/media/select1.png)

-   <span data-ttu-id="3eed6-167">La siguiente **selección** se ejecutó como resultado del acceso a la propiedad de navegación ExistingPerson. instructor y selecciona solo las columnas asignadas a **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="3eed6-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Seleccione 2](~/ef6/media/select2.png)
