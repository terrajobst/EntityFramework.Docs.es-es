---
title: 'Procedimientos almacenados del diseñador CUD: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813563"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="9d7e5-102">Procedimientos almacenados CUD del diseñador</span><span class="sxs-lookup"><span data-stu-id="9d7e5-102">Designer CUD Stored Procedures</span></span>

<span data-ttu-id="9d7e5-103">Este tutorial paso a paso muestra cómo asignar las operaciones de creación\\inserción, actualización y eliminación (CUD) de un tipo de entidad a procedimientos almacenados mediante el Entity Framework Designer (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="9d7e5-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="9d7e5-104"> De forma predeterminada, el Entity Framework genera automáticamente las instrucciones SQL para las operaciones CUD, pero también puede asignar procedimientos almacenados a estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-104"> By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="9d7e5-105">Tenga en cuenta que Code First no admite la asignación a funciones o procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="9d7e5-106">Sin embargo, puede llamar a procedimientos almacenados o funciones mediante el método System. Data. Entity. DbSet. SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="9d7e5-107">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9d7e5-107">For example:</span></span>

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="9d7e5-108">Consideraciones al asignar las operaciones CUD a procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="9d7e5-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="9d7e5-109">Al asignar las operaciones CUD a los procedimientos almacenados, se aplican las consideraciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="9d7e5-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span>

- <span data-ttu-id="9d7e5-110">Si va a asignar una de las operaciones de CUD a un procedimiento almacenado, asígnela todas.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="9d7e5-111">Si no asigna los tres, se producirá un error en las operaciones no asignadas si se ejecutan y se producirá una  **UpdateException** .</span><span class="sxs-lookup"><span data-stu-id="9d7e5-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
- <span data-ttu-id="9d7e5-112">Debe asignar todos los parámetros del procedimiento almacenado a las propiedades de la entidad.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-112">You must map every parameter of the stored procedure to entity properties.</span></span>
- <span data-ttu-id="9d7e5-113">Si el servidor genera el valor de clave principal para la fila insertada, debe volver a asignar este valor a la propiedad clave de la entidad.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="9d7e5-114">En el ejemplo siguiente, el procedimiento almacenado **InsertPerson** devuelve la clave principal recién creada como parte del conjunto de resultados del procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="9d7e5-115">La clave principal se asigna a la clave de entidad (**PersonID**) mediante el **&lt;agregar enlaces de resultados&gt;** característica del diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
- <span data-ttu-id="9d7e5-116">Las llamadas a procedimientos almacenados se asignan 1:1 con las entidades del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="9d7e5-117">Por ejemplo, si implementa una jerarquía de herencia en el modelo conceptual y, a continuación, asigna los procedimientos almacenados CUD para las entidades **primaria** (base) y **secundaria** (derivada), al guardar los cambios **secundarios** solo se llamará a los procedimientos almacenados del **elemento secundario**, no se desencadenarán las llamadas a procedimientos almacenados del **elemento primario**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d7e5-118">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="9d7e5-118">Prerequisites</span></span>

<span data-ttu-id="9d7e5-119">Para completar este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="9d7e5-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="9d7e5-120">Una versión reciente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="9d7e5-121">La [base de datos de ejemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="9d7e5-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="9d7e5-122">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="9d7e5-122">Set up the Project</span></span>

- <span data-ttu-id="9d7e5-123">Abra Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-123">Open Visual Studio 2012.</span></span>
- <span data-ttu-id="9d7e5-124">Seleccionar **archivo-&gt; nuevo-&gt; proyecto**</span><span class="sxs-lookup"><span data-stu-id="9d7e5-124">Select **File-&gt; New -&gt; Project**</span></span>
- <span data-ttu-id="9d7e5-125">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione la plantilla de **consola** .</span><span class="sxs-lookup"><span data-stu-id="9d7e5-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
- <span data-ttu-id="9d7e5-126">Escriba **CUDSProcsSample** como nombre.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-126">Enter **CUDSProcsSample** as the name.</span></span>
- <span data-ttu-id="9d7e5-127">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="9d7e5-128">Crear un modelo</span><span class="sxs-lookup"><span data-stu-id="9d7e5-128">Create a Model</span></span>

- <span data-ttu-id="9d7e5-129">Haga clic con el botón derecho en el nombre del proyecto en Explorador de soluciones y seleccione **agregar&gt; nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
- <span data-ttu-id="9d7e5-130">Seleccione **datos** en el menú de la izquierda y, a continuación, seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
- <span data-ttu-id="9d7e5-131">Escriba **CUDSProcs. edmx** como nombre de archivo y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
- <span data-ttu-id="9d7e5-132">En el cuadro de diálogo elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
- <span data-ttu-id="9d7e5-133">Haga clic en **nueva conexión**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-133">Click **New Connection**.</span></span> <span data-ttu-id="9d7e5-134">En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **LocalDB)\\mssqllocaldb**), seleccione el método de autenticación, escriba **School** para el nombre de la base de datos y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="9d7e5-135">El cuadro de diálogo elegir la conexión de datos se actualiza con la configuración de conexión de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
- <span data-ttu-id="9d7e5-136">En el cuadro de diálogo elija los objetos de base de datos, en el nodo **tablas** , seleccione la tabla **Person** .</span><span class="sxs-lookup"><span data-stu-id="9d7e5-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
- <span data-ttu-id="9d7e5-137">Además, seleccione los siguientes procedimientos almacenados en el nodo **procedimientos almacenados y funciones** : **DeletePerson**, **InsertPerson**y **UpdatePerson**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span>
- <span data-ttu-id="9d7e5-138">A partir de Visual Studio 2012, el diseñador de EF admite la importación masiva de procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="9d7e5-139">La **importación de funciones y procedimientos almacenados seleccionados en el modelo de entidad** está activada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="9d7e5-140">Como en este ejemplo tenemos procedimientos almacenados que insertan, actualizan y eliminan tipos de entidad, no queremos importarlos y desactivarán esta casilla.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span>

    ![Importar procedimientos de S](~/ef6/media/importsprocs.jpg)

- <span data-ttu-id="9d7e5-142">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-142">Click **Finish**.</span></span>
    <span data-ttu-id="9d7e5-143">Se muestra el diseñador de EF, que proporciona una superficie de diseño para editar el modelo.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="9d7e5-144">Asignar la entidad Person a procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="9d7e5-144">Map the Person Entity to Stored Procedures</span></span>

- <span data-ttu-id="9d7e5-145">Haga clic con el botón secundario en la **persona** tipo de entidad y seleccione **asignación de procedimiento almacenado**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
- <span data-ttu-id="9d7e5-146">Las asignaciones de procedimientos almacenados aparecen en la ventana detalles de la **asignación** .</span><span class="sxs-lookup"><span data-stu-id="9d7e5-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
- <span data-ttu-id="9d7e5-147">Haga clic en **&lt;seleccionar Insertar función&gt;** .</span><span class="sxs-lookup"><span data-stu-id="9d7e5-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="9d7e5-148">El campo se convierte en una lista desplegable de los procedimientos almacenados del modelo de almacenamiento que se puede asignar a tipos de entidad del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="9d7e5-149">Seleccione **InsertPerson** de la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-149">Select **InsertPerson** from the drop-down list.</span></span>
- <span data-ttu-id="9d7e5-150">Aparecen las asignaciones predeterminadas entre los parámetros de procedimiento almacenado y las propiedades de entidad.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="9d7e5-151">Observe que las flechas indican la dirección de la asignación: se proporcionan valores de propiedad como parámetros de procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
- <span data-ttu-id="9d7e5-152">Haga clic en \*\*&lt;agregar&gt;de enlace de resultados \*\*.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-152">Click **&lt;Add Result Binding&gt;**.</span></span>
- <span data-ttu-id="9d7e5-153">Escriba **NewPersonID**, el nombre del parámetro devuelto por el procedimiento almacenado **InsertPerson** .</span><span class="sxs-lookup"><span data-stu-id="9d7e5-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="9d7e5-154">Asegúrese de no escribir espacios iniciales ni finales.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-154">Make sure not to type leading or trailing spaces.</span></span>
- <span data-ttu-id="9d7e5-155">Presione **entrar**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-155">Press **Enter**.</span></span>
- <span data-ttu-id="9d7e5-156">De forma predeterminada, **NewPersonID** se asigna a la clave de entidad **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="9d7e5-157">Observe que una flecha indica la dirección de la asignación: el valor de la columna de resultado se proporciona para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Detalles de la asignación](~/ef6/media/mappingdetails.png)

- <span data-ttu-id="9d7e5-159">Haga clic en **&lt;seleccione Actualizar función&gt;**  y seleccione **UpdatePerson** en la lista desplegable resultante.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="9d7e5-160">Aparecen las asignaciones predeterminadas entre los parámetros de procedimiento almacenado y las propiedades de entidad.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
- <span data-ttu-id="9d7e5-161">Haga clic en **&lt;seleccionar eliminar función&gt;**  y seleccione **DeletePerson** en la lista desplegable resultante.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="9d7e5-162">Aparecen las asignaciones predeterminadas entre los parámetros de procedimiento almacenado y las propiedades de entidad.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="9d7e5-163">Las operaciones de inserción, actualización y eliminación de la **persona** tipo de entidad ahora están asignadas a procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="9d7e5-164">Si desea habilitar la comprobación de simultaneidad al actualizar o eliminar una entidad con procedimientos almacenados, use una de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="9d7e5-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

- <span data-ttu-id="9d7e5-165">Use un parámetro de **salida** para devolver el número de filas afectadas del procedimiento almacenado y active las casillas de los **parámetros afectados** de las filas junto al nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="9d7e5-166">Si el valor devuelto es cero cuando se llama a la operación, se producirá una   [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) .</span><span class="sxs-lookup"><span data-stu-id="9d7e5-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
- <span data-ttu-id="9d7e5-167">Active la casilla **usar valor original** junto a una propiedad que quiera usar para la comprobación de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="9d7e5-168">Cuando se intenta realizar una actualización, se usará el valor de la propiedad que se leyó originalmente de la base de datos al escribir datos de nuevo en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="9d7e5-169">Si el valor no coincide con el valor de la base de datos, se producirá una **OptimisticConcurrencyException** .</span><span class="sxs-lookup"><span data-stu-id="9d7e5-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="9d7e5-170">Usar el modelo</span><span class="sxs-lookup"><span data-stu-id="9d7e5-170">Use the Model</span></span>

<span data-ttu-id="9d7e5-171">Abra el archivo **Program.CS** en el que se define el método **Main** .</span><span class="sxs-lookup"><span data-stu-id="9d7e5-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="9d7e5-172">Agregue el código siguiente a la función main.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="9d7e5-173">El código crea un nuevo objeto **Person** y, a continuación, actualiza el objeto y, por último, elimina el objeto.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

- <span data-ttu-id="9d7e5-174">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-174">Compile and run the application.</span></span> <span data-ttu-id="9d7e5-175">El programa genera el siguiente resultado \*</span><span class="sxs-lookup"><span data-stu-id="9d7e5-175">The program produces the following output \*</span></span>

> [!NOTE]
> <span data-ttu-id="9d7e5-176">El servidor genera automáticamente PersonID, por lo que probablemente verá un número diferente \*</span><span class="sxs-lookup"><span data-stu-id="9d7e5-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="9d7e5-177">Si está trabajando con la versión Ultimate de Visual Studio, puede usar IntelliTrace con el depurador para ver las instrucciones SQL que se ejecutan.</span><span class="sxs-lookup"><span data-stu-id="9d7e5-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![Solamente](~/ef6/media/intellitrace.png)
