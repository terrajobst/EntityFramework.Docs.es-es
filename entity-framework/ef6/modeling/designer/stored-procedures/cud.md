---
title: 'Diseñador CUD procedimientos almacenados: EF6'
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
caps.latest.revision: 3
ms.openlocfilehash: 6b6a1f843142713153fa86309ef55f9d6e804766
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121928"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="ac692-102">Diseñador CUD procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="ac692-102">Designer CUD Stored Procedures</span></span>
<span data-ttu-id="ac692-103">En este tutorial paso a paso muestran cómo asignar el crear\\Insertar, actualizar y eliminar operaciones (CUD) de un tipo de entidad a procedimientos almacenados mediante el Diseñador de Entity Framework (EF Designer).</span><span class="sxs-lookup"><span data-stu-id="ac692-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span>  <span data-ttu-id="ac692-104">De forma predeterminada, Entity Framework genera automáticamente las instrucciones SQL para las operaciones CUD, pero también se pueden asignar procedimientos almacenados a estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="ac692-104">By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="ac692-105">Tenga en cuenta que Code First no admite la asignación a procedimientos almacenados o funciones.</span><span class="sxs-lookup"><span data-stu-id="ac692-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="ac692-106">Sin embargo, puede llamar a procedimientos almacenados o funciones mediante el método System.Data.Entity.DbSet.SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="ac692-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="ac692-107">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ac692-107">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="ac692-108">Consideraciones al asignar las operaciones CUD en procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="ac692-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="ac692-109">Al asignar las operaciones CUD a procedimientos almacenados, se aplican las consideraciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="ac692-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span> 

-   <span data-ttu-id="ac692-110">Si una de las operaciones CUD va a asignar a un procedimiento almacenado, se asignan todas ellas.</span><span class="sxs-lookup"><span data-stu-id="ac692-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="ac692-111">Si no asigna las tres, las operaciones no asignadas se producirá un error si se ejecuta y un **UpdateException** se iniciará.</span><span class="sxs-lookup"><span data-stu-id="ac692-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
-   <span data-ttu-id="ac692-112">Todos los parámetros del procedimiento almacenado deben asignar las propiedades de entidad.</span><span class="sxs-lookup"><span data-stu-id="ac692-112">You must map every parameter of the stored procedure to entity properties.</span></span>
-   <span data-ttu-id="ac692-113">Si el servidor genera el valor de clave principal para la fila insertada, debe asignar este valor a la propiedad de clave de la entidad.</span><span class="sxs-lookup"><span data-stu-id="ac692-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="ac692-114">En el ejemplo siguiente, la **InsertPerson** procedimiento almacenado devuelve la clave principal recién creada como parte del conjunto de resultados del procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="ac692-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="ac692-115">La clave principal se asigna a la clave de entidad (**PersonID**) mediante el **&lt;Agregar Result Binding&gt;** característica de EF Designer.</span><span class="sxs-lookup"><span data-stu-id="ac692-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
-   <span data-ttu-id="ac692-116">Las llamadas de procedimiento almacenado son asignado 1:1 con las entidades del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="ac692-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="ac692-117">Por ejemplo, si implementa una jerarquía de herencia en el modelo conceptual y después asigne el CUD procedimientos almacenados para la **primario** (base) y el **secundarios** entidades (derivadas), guardar el **Secundarios** cambios solo se llamará el **secundarios**de procedimientos almacenados, no se desencadenará la **primario**de llamadas a procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="ac692-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ac692-118">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ac692-118">Prerequisites</span></span>

<span data-ttu-id="ac692-119">Para completar este tutorial, necesitará:</span><span class="sxs-lookup"><span data-stu-id="ac692-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="ac692-120">Una versión reciente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac692-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="ac692-121">El [base de datos de ejemplo School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="ac692-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="ac692-122">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="ac692-122">Set up the Project</span></span>

-   <span data-ttu-id="ac692-123">Abra Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="ac692-123">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="ac692-124">Seleccione **archivo -&gt; nuevo:&gt; proyecto**</span><span class="sxs-lookup"><span data-stu-id="ac692-124">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="ac692-125">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla.</span><span class="sxs-lookup"><span data-stu-id="ac692-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="ac692-126">Escriba **CUDSProcsSample** como el nombre.</span><span class="sxs-lookup"><span data-stu-id="ac692-126">Enter **CUDSProcsSample** as the name.</span></span>
-   <span data-ttu-id="ac692-127">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ac692-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="ac692-128">Crear un modelo</span><span class="sxs-lookup"><span data-stu-id="ac692-128">Create a Model</span></span>

-   <span data-ttu-id="ac692-129">Haga clic en el nombre del proyecto en el Explorador de soluciones y seleccione **Add -&gt; nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="ac692-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="ac692-130">Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="ac692-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="ac692-131">Escriba **CUDSProcs.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="ac692-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="ac692-132">En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ac692-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="ac692-133">Haga clic en **nueva conexión**.</span><span class="sxs-lookup"><span data-stu-id="ac692-133">Click **New Connection**.</span></span> <span data-ttu-id="ac692-134">En el cuadro de diálogo Propiedades de conexión, escriba el nombre del servidor (por ejemplo, **(localdb)\\mssqllocaldb**), seleccione el método de autenticación, el tipo **School** para el nombre de la base de datos y, a continuación, Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ac692-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="ac692-135">El cuadro de diálogo Elegir la conexión de datos se actualiza con la configuración de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="ac692-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="ac692-136">En el elija los objetos de base de datos cuadro de diálogo el **tablas** nodo, seleccione el **persona** tabla.</span><span class="sxs-lookup"><span data-stu-id="ac692-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
-   <span data-ttu-id="ac692-137">Además, seleccione los siguientes procedimientos almacenados en el **procedimientos almacenados y funciones** nodo: **DeletePerson**, **InsertPerson**, y **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="ac692-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span> 
-   <span data-ttu-id="ac692-138">A partir de Visual Studio 2012, EF Designer admite la importación masiva de los procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="ac692-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="ac692-139">El **Importar seleccionada procedimientos almacenados y funciones en el modelo de entidad** está activada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ac692-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="ac692-140">Puesto que en este ejemplo nos hemos procedimientos almacenados que insertarán, actualización y eliminación tipos de entidad, quiere importarlos y se desactiva esta casilla de verificación.</span><span class="sxs-lookup"><span data-stu-id="ac692-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span> 

    ![ImportSProcs](~/ef6/media/importsprocs.jpg)

-   <span data-ttu-id="ac692-142">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="ac692-142">Click **Finish**.</span></span>
    <span data-ttu-id="ac692-143">EF Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.</span><span class="sxs-lookup"><span data-stu-id="ac692-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="ac692-144">Asignar la entidad Person a procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="ac692-144">Map the Person Entity to Stored Procedures</span></span>

-   <span data-ttu-id="ac692-145">Haga clic en el **persona** tipo de entidad y seleccione **asignación de procedimientos almacenados**.</span><span class="sxs-lookup"><span data-stu-id="ac692-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
-   <span data-ttu-id="ac692-146">Las asignaciones de procedimiento almacenado que aparecen en la **detalles de Mapping** ventana.</span><span class="sxs-lookup"><span data-stu-id="ac692-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="ac692-147">Haga clic en  **&lt;seleccione Insertar función&gt;**.</span><span class="sxs-lookup"><span data-stu-id="ac692-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="ac692-148">El campo se convierte en una lista desplegable de los procedimientos almacenados del modelo de almacenamiento que se puede asignar a tipos de entidad del modelo conceptual.</span><span class="sxs-lookup"><span data-stu-id="ac692-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="ac692-149">Seleccione **InsertPerson** en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="ac692-149">Select **InsertPerson** from the drop-down list.</span></span>
-   <span data-ttu-id="ac692-150">Aparecen las asignaciones predeterminadas entre los parámetros de procedimiento almacenado y las propiedades de entidad.</span><span class="sxs-lookup"><span data-stu-id="ac692-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="ac692-151">Observe que las flechas indican la dirección de la asignación: se proporcionan valores de propiedad como parámetros de procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="ac692-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
-   <span data-ttu-id="ac692-152">Haga clic en  **&lt;Agregar Result Binding&gt;**.</span><span class="sxs-lookup"><span data-stu-id="ac692-152">Click **&lt;Add Result Binding&gt;**.</span></span>
-   <span data-ttu-id="ac692-153">Tipo **NewPersonID**, el nombre del parámetro devuelto por la **InsertPerson** procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="ac692-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="ac692-154">Asegúrese de no escriba espacios iniciales o finales.</span><span class="sxs-lookup"><span data-stu-id="ac692-154">Make sure not to type leading or trailing spaces.</span></span>
-   <span data-ttu-id="ac692-155">Presione **ENTRAR**.</span><span class="sxs-lookup"><span data-stu-id="ac692-155">Press **Enter**.</span></span>
-   <span data-ttu-id="ac692-156">De forma predeterminada, **NewPersonID** se asigna a la clave de entidad **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="ac692-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="ac692-157">Observe que una flecha indica la dirección de la asignación: el valor de la columna de resultado se proporciona para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="ac692-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![MappingDetails](~/ef6/media/mappingdetails.png)

-   <span data-ttu-id="ac692-159">Haga clic en **&lt;seleccionar Update Function&gt;** y seleccione **UpdatePerson** en la lista desplegable resultante.</span><span class="sxs-lookup"><span data-stu-id="ac692-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="ac692-160">Aparecen las asignaciones predeterminadas entre los parámetros de procedimiento almacenado y las propiedades de entidad.</span><span class="sxs-lookup"><span data-stu-id="ac692-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
-   <span data-ttu-id="ac692-161">Haga clic en **&lt;Seleccionar Delete Function&gt;** y seleccione **DeletePerson** en la lista desplegable resultante.</span><span class="sxs-lookup"><span data-stu-id="ac692-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="ac692-162">Aparecen las asignaciones predeterminadas entre los parámetros de procedimiento almacenado y las propiedades de entidad.</span><span class="sxs-lookup"><span data-stu-id="ac692-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="ac692-163">La inserción, actualizar y eliminar operaciones de la **persona** tipo de entidad están asignadas ahora a procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="ac692-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="ac692-164">Si desea habilitar la comprobación de concurrencia al actualizar o eliminar una entidad con procedimientos almacenados, utilice una de las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="ac692-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

-   <span data-ttu-id="ac692-165">Use un **salida** afectados de parámetro para devolver el número de filas desde el procedimiento almacenado y compruebe el **Rowsaffectedparameter** casilla de verificación junto al nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="ac692-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="ac692-166">Si el valor devuelto es cero cuando se llama a la operación, un [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) se iniciará.</span><span class="sxs-lookup"><span data-stu-id="ac692-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
-   <span data-ttu-id="ac692-167">Compruebe el **usar Original Value** casilla de verificación junto a una propiedad que se va a usar para la comprobación de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="ac692-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="ac692-168">Cuando se intenta realizar una actualización, se usará el valor de la propiedad que se leyó originalmente de la base de datos cuando realiza una escritura de datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ac692-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="ac692-169">Si el valor no coincide con el valor de la base de datos, un **OptimisticConcurrencyException** se iniciará.</span><span class="sxs-lookup"><span data-stu-id="ac692-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="ac692-170">Usar el modelo</span><span class="sxs-lookup"><span data-stu-id="ac692-170">Use the Model</span></span>

<span data-ttu-id="ac692-171">Abra el **Program.cs** donde el **Main** se define el método.</span><span class="sxs-lookup"><span data-stu-id="ac692-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="ac692-172">Agregue el código siguiente en la función Main.</span><span class="sxs-lookup"><span data-stu-id="ac692-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="ac692-173">El código crea un nuevo **persona** objeto, a continuación, actualiza el objeto y finalmente elimina el objeto.</span><span class="sxs-lookup"><span data-stu-id="ac692-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>         

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

-   <span data-ttu-id="ac692-174">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ac692-174">Compile and run the application.</span></span> <span data-ttu-id="ac692-175">El programa produce el siguiente resultado \*</span><span class="sxs-lookup"><span data-stu-id="ac692-175">The program produces the following output \*</span></span>
    >[!NOTE]
> <span data-ttu-id="ac692-176">PersonID es generado automáticamente por el servidor, por lo que verá más probable es que un número diferente \*</span><span class="sxs-lookup"><span data-stu-id="ac692-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="ac692-177">Si está trabajando con la versión de Visual Studio Ultimate, puede utilizar Intellitrace con el depurador para ver las instrucciones SQL que se ejecutan.</span><span class="sxs-lookup"><span data-stu-id="ac692-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
