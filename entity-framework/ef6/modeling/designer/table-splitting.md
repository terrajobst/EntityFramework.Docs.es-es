---
title: La división de tablas diseñador - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: 87b6e1bd0374f77dfffab342c659cf4e16c8a337
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994508"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="7c624-102">División de tabla del diseñador</span><span class="sxs-lookup"><span data-stu-id="7c624-102">Designer Table Splitting</span></span>
<span data-ttu-id="7c624-103">En este tutorial se muestra cómo asignar varios tipos de entidad a una única tabla mediante la modificación de un modelo con Entity Framework Designer (Diseñador de EF).</span><span class="sxs-lookup"><span data-stu-id="7c624-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="7c624-104">Una razón que desea utilizar la división de tablas está retrasando la carga de algunas propiedades cuando se usa la carga para los objetos de la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="7c624-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span> <span data-ttu-id="7c624-105">Puede separar las propiedades que es posible que contienen gran cantidad de datos en una entidad independiente y la carga solo cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="7c624-105">You can separate the properties that might contain very large amount of data into a seperate entity and only load it when required.</span></span>

<span data-ttu-id="7c624-106">La siguiente imagen muestra las ventanas principales que se usan al trabajar con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="7c624-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="7c624-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7c624-108">Prerequisites</span></span>

<span data-ttu-id="7c624-109">Para completar este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="7c624-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="7c624-110">Una versión reciente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7c624-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="7c624-111">El [base de datos de ejemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="7c624-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="7c624-112">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="7c624-112">Set up the Project</span></span>

<span data-ttu-id="7c624-113">En este tutorial se usa Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="7c624-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="7c624-114">Abra Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="7c624-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="7c624-115">En el menú **Archivo** , elija **Nuevo**y haga clic en **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="7c624-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="7c624-116">En el panel izquierdo, haga clic en Visual C\#y, a continuación, seleccione la plantilla de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="7c624-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="7c624-117">Escriba **TableSplittingSample** como el nombre del proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7c624-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="7c624-118">Crear un modelo basado en la base de datos School</span><span class="sxs-lookup"><span data-stu-id="7c624-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="7c624-119">Haga clic en el nombre del proyecto en el Explorador de soluciones, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="7c624-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="7c624-120">Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="7c624-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="7c624-121">Escriba **TableSplittingModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="7c624-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="7c624-122">En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente.**</span><span class="sxs-lookup"><span data-stu-id="7c624-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="7c624-123">Haga clic en nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="7c624-123">Click New Connection.</span></span> <span data-ttu-id="7c624-124">En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7c624-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="7c624-125">El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7c624-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="7c624-126">En el cuadro de diálogo Elija los objetos de base de datos, expandir el **tablas** nodo y compruebe el **persona** tabla.</span><span class="sxs-lookup"><span data-stu-id="7c624-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="7c624-127">Esto agregará la tabla especificada en el **School** modelo.</span><span class="sxs-lookup"><span data-stu-id="7c624-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="7c624-128">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="7c624-128">Click **Finish**.</span></span>

<span data-ttu-id="7c624-129">Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.</span><span class="sxs-lookup"><span data-stu-id="7c624-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="7c624-130">Todos los objetos que seleccionó en el **elija los objetos de base de datos** cuadro de diálogo se agregan al modelo.</span><span class="sxs-lookup"><span data-stu-id="7c624-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="7c624-131">Dos entidades se asignan a una sola tabla</span><span class="sxs-lookup"><span data-stu-id="7c624-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="7c624-132">En esta sección dividirá el **persona** entidad a dos entidades y, a continuación, se asignan a una única tabla.</span><span class="sxs-lookup"><span data-stu-id="7c624-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="7c624-133">El **persona** entidad no contiene todas las propiedades que pueden contener grandes cantidades de datos; simplemente sirve como ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7c624-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="7c624-134">Haga clic en un área vacía de la superficie de diseño, seleccione **Agregar nuevo**y haga clic en **entidad**.</span><span class="sxs-lookup"><span data-stu-id="7c624-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="7c624-135">El **nueva entidad** aparece el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="7c624-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="7c624-136">Tipo **HireInfo** para el **nombre de entidad** y **PersonID** para el **propiedad Key** nombre.</span><span class="sxs-lookup"><span data-stu-id="7c624-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="7c624-137">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7c624-137">Click **OK**.</span></span>
-   <span data-ttu-id="7c624-138">Se crea y se muestra un nuevo tipo de entidad en la superficie de diseño.</span><span class="sxs-lookup"><span data-stu-id="7c624-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="7c624-139">Seleccione el **HireDate** propiedad de la **persona** tipo de entidad y presione **CTRL+x** claves.</span><span class="sxs-lookup"><span data-stu-id="7c624-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="7c624-140">Seleccione el **HireInfo** entidad y presione **CTRL+v** claves.</span><span class="sxs-lookup"><span data-stu-id="7c624-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="7c624-141">Crear una asociación entre **persona** y **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="7c624-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="7c624-142">Para ello, haga clic en un área vacía de la superficie de diseño, seleccione **Agregar nuevo**y haga clic en **asociación**.</span><span class="sxs-lookup"><span data-stu-id="7c624-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="7c624-143">El **Agregar asociación** aparece el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="7c624-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="7c624-144">El **PersonHireInfo** se asigna el nombre de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7c624-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="7c624-145">Especificar la multiplicidad **1(One)** en ambos extremos de la relación.</span><span class="sxs-lookup"><span data-stu-id="7c624-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="7c624-146">Presione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7c624-146">Press **OK**.</span></span>

<span data-ttu-id="7c624-147">El paso siguiente requiere la **detalles de Mapping** ventana.</span><span class="sxs-lookup"><span data-stu-id="7c624-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="7c624-148">Si no puede ver esta ventana, haga clic en la superficie de diseño y seleccione **detalles de Mapping**.</span><span class="sxs-lookup"><span data-stu-id="7c624-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="7c624-149">Seleccione el **HireInfo** tipo de entidad y haga clic en **&lt;agregar una tabla o vista&gt;** en el **detalles de Mapping** ventana.</span><span class="sxs-lookup"><span data-stu-id="7c624-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="7c624-150">Seleccione **persona** desde el **&lt;agregar una tabla o vista&gt;** lista de campos de lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="7c624-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="7c624-151">La lista contiene las tablas o vistas a la que se puede asignar la entidad seleccionada.</span><span class="sxs-lookup"><span data-stu-id="7c624-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="7c624-152">Las propiedades adecuadas deben asignarse de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7c624-152">The appropriate properties should be mapped by default.</span></span>

    ![Asignación](~/ef6/media/mapping.png)

-   <span data-ttu-id="7c624-154">Seleccione el **PersonHireInfo** asociación en la superficie de diseño.</span><span class="sxs-lookup"><span data-stu-id="7c624-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="7c624-155">Haga clic en la asociación en la superficie de diseño y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="7c624-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="7c624-156">En el **propiedades** ventana, seleccione el **las restricciones referenciales** propiedad y haga clic en el botón de puntos suspensivos.</span><span class="sxs-lookup"><span data-stu-id="7c624-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="7c624-157">Seleccione **persona** desde el **Principal** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="7c624-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="7c624-158">Presione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7c624-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="7c624-159">Usar el modelo</span><span class="sxs-lookup"><span data-stu-id="7c624-159">Use the Model</span></span>

-   <span data-ttu-id="7c624-160">Pegue el código siguiente en el método Main.</span><span class="sxs-lookup"><span data-stu-id="7c624-160">Paste the following code in the Main method.</span></span>

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
-   <span data-ttu-id="7c624-161">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c624-161">Compile and run the application.</span></span>

<span data-ttu-id="7c624-162">Las siguientes instrucciones de T-SQL se ejecutaron en el **School** base de datos como resultado de ejecutar esta aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c624-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="7c624-163">La siguiente **insertar** se ejecutó como resultado de ejecutar el contexto. SaveChanges() y combina datos de la **persona** y **HireInfo** entidades</span><span class="sxs-lookup"><span data-stu-id="7c624-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insertar](~/ef6/media/insert.png)

-   <span data-ttu-id="7c624-165">La siguiente **seleccione** se ejecutó como resultado de ejecutar el contexto. People.FirstOrDefault() y selecciona solo las columnas asignadas a **persona**</span><span class="sxs-lookup"><span data-stu-id="7c624-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Select1](~/ef6/media/select1.png)

-   <span data-ttu-id="7c624-167">La siguiente **seleccione** se ejecutó como consecuencia de tener acceso a la existingPerson.Instructor de propiedad de navegación y selecciona solo las columnas asignadas a **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="7c624-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Select2](~/ef6/media/select2.png)
