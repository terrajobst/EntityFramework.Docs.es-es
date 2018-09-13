---
title: Separación de entidad diseñador - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490861"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="7e701-102">División de entidades de diseñador</span><span class="sxs-lookup"><span data-stu-id="7e701-102">Designer Entity Splitting</span></span>
<span data-ttu-id="7e701-103">En este tutorial se muestra cómo asignar un tipo de entidad a dos tablas modificando un modelo con Entity Framework Designer (Diseñador de EF).</span><span class="sxs-lookup"><span data-stu-id="7e701-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="7e701-104">Puede asignar una entidad a varias tablas cuando estas comparten una clave común.</span><span class="sxs-lookup"><span data-stu-id="7e701-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="7e701-105">Los conceptos que se aplican en la asignación de un tipo de entidad a dos tablas se extienden con facilidad a la asignación de un tipo de entidad a más de dos tablas.</span><span class="sxs-lookup"><span data-stu-id="7e701-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="7e701-106">La siguiente imagen muestra las ventanas principales que se usan al trabajar con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="7e701-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="7e701-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7e701-108">Prerequisites</span></span>

<span data-ttu-id="7e701-109">Edición visual de Studio 2012 o Visual Studio 2010, Ultimate, Premium, Professional o Web Express.</span><span class="sxs-lookup"><span data-stu-id="7e701-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="7e701-110">Crear la base de datos</span><span class="sxs-lookup"><span data-stu-id="7e701-110">Create the Database</span></span>

<span data-ttu-id="7e701-111">El servidor de base de datos que se instala con Visual Studio es diferente según la versión de Visual Studio que ha instalado:</span><span class="sxs-lookup"><span data-stu-id="7e701-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="7e701-112">Si usa Visual Studio 2012, a continuación, se creará una base de datos LocalDB.</span><span class="sxs-lookup"><span data-stu-id="7e701-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="7e701-113">Si usa Visual Studio 2010 se creará una base de datos de SQL Express.</span><span class="sxs-lookup"><span data-stu-id="7e701-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="7e701-114">En primer lugar, vamos a crear una base de datos con dos tablas que vamos a combinar en una única entidad.</span><span class="sxs-lookup"><span data-stu-id="7e701-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="7e701-115">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e701-115">Open Visual Studio</span></span>
-   <span data-ttu-id="7e701-116">**Vista -&gt; Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="7e701-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="7e701-117">Haga clic con el botón derecho en **conexiones de datos -&gt; Agregar conexión...**</span><span class="sxs-lookup"><span data-stu-id="7e701-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="7e701-118">Si aún no lo ha conectado a una base de datos del explorador de servidores antes de que tendrá que seleccionar **Microsoft SQL Server** como origen de datos</span><span class="sxs-lookup"><span data-stu-id="7e701-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="7e701-119">Conectarse a LocalDB o Express de SQL, según lo que se ha instalado</span><span class="sxs-lookup"><span data-stu-id="7e701-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="7e701-120">Escriba **EntitySplitting** como el nombre de la base de datos</span><span class="sxs-lookup"><span data-stu-id="7e701-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="7e701-121">Seleccione **Aceptar** y se le solicitará si desea crear una nueva base de datos, seleccione **sí**</span><span class="sxs-lookup"><span data-stu-id="7e701-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="7e701-122">La nueva base de datos aparecerán en el Explorador de servidores</span><span class="sxs-lookup"><span data-stu-id="7e701-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="7e701-123">Si usa Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7e701-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="7e701-124">Haga doble clic en la base de datos en el Explorador de servidores y seleccione **nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="7e701-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="7e701-125">Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **Execute**</span><span class="sxs-lookup"><span data-stu-id="7e701-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="7e701-126">Si usa Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="7e701-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="7e701-127">Seleccione **datos -&gt; Transact SQL Editor -&gt; nueva conexión de consulta...**</span><span class="sxs-lookup"><span data-stu-id="7e701-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="7e701-128">Escriba **.\\ SQLEXPRESS** como el nombre del servidor y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="7e701-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="7e701-129">Seleccione el **EntitySplitting** de base de datos en la lista desplegable en la parte superior del editor de consultas</span><span class="sxs-lookup"><span data-stu-id="7e701-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="7e701-130">Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **ejecutar SQL**</span><span class="sxs-lookup"><span data-stu-id="7e701-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a><span data-ttu-id="7e701-131">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="7e701-131">Create the Project</span></span>

-   <span data-ttu-id="7e701-132">En el menú **Archivo** , elija **Nuevo**y haga clic en **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="7e701-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="7e701-133">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **aplicación de consola** plantilla.</span><span class="sxs-lookup"><span data-stu-id="7e701-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="7e701-134">Escriba **MapEntityToTablesSample** como el nombre del proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7e701-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="7e701-135">Haga clic en **n** si se le pide que guarde la consulta SQL que creó en la primera sección.</span><span class="sxs-lookup"><span data-stu-id="7e701-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="7e701-136">Crear un modelo basado en la base de datos</span><span class="sxs-lookup"><span data-stu-id="7e701-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="7e701-137">Haga clic en el nombre del proyecto en el Explorador de soluciones, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="7e701-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="7e701-138">Seleccione **datos** desde el menú de la izquierda y seleccione **ADO.NET Entity Data Model** en el panel Plantillas.</span><span class="sxs-lookup"><span data-stu-id="7e701-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="7e701-139">Escriba **MapEntityToTablesModel.edmx** para el nombre de archivo y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="7e701-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="7e701-140">En el cuadro de diálogo Elegir contenido del modelo, seleccione **generar desde la base de datos**y, a continuación, haga clic en **siguiente.**</span><span class="sxs-lookup"><span data-stu-id="7e701-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="7e701-141">Seleccione el **EntitySplitting** conexión en la lista desplegable y haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="7e701-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="7e701-142">En el cuadro de diálogo Elija los objetos de base de datos, active la casilla junto a la **tablas** nodo.</span><span class="sxs-lookup"><span data-stu-id="7e701-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="7e701-143">Esta acción agregará todas las tablas de la **EntitySplitting** base de datos para el modelo.</span><span class="sxs-lookup"><span data-stu-id="7e701-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="7e701-144">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="7e701-144">Click **Finish**.</span></span>

<span data-ttu-id="7e701-145">Entity Designer, que proporciona una superficie de diseño para modificar el modelo, se muestra.</span><span class="sxs-lookup"><span data-stu-id="7e701-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="7e701-146">Asignar una entidad a dos tablas</span><span class="sxs-lookup"><span data-stu-id="7e701-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="7e701-147">En este paso se actualizará el **persona** tipo de entidad para combinar datos de la **persona** y **PersonInfo** tablas.</span><span class="sxs-lookup"><span data-stu-id="7e701-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="7e701-148">Seleccione el **correo electrónico** y **teléfono** propiedades de la \*\* PersonInfo \*\* entidad y presione **CTRL+x** claves.</span><span class="sxs-lookup"><span data-stu-id="7e701-148">Select the **Email** and **Phone** properties of the \*\*PersonInfo \*\*entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="7e701-149">Seleccione el \*\* persona \*\* entidad y presione **CTRL+v** claves.</span><span class="sxs-lookup"><span data-stu-id="7e701-149">Select the \*\*Person \*\*entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="7e701-150">En la superficie de diseño, seleccione el **PersonInfo** entidad y presione **eliminar** botón en el teclado.</span><span class="sxs-lookup"><span data-stu-id="7e701-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="7e701-151">Haga clic en **No** cuando se le pregunte si desea quitar el **PersonInfo** tabla del modelo, que se van a asignar a la **persona** entidad.</span><span class="sxs-lookup"><span data-stu-id="7e701-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Eliminar tablas](~/ef6/media/deletetables.png)

<span data-ttu-id="7e701-153">Los pasos siguientes requieren la **detalles de Mapping** ventana.</span><span class="sxs-lookup"><span data-stu-id="7e701-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="7e701-154">Si no puede ver esta ventana, haga clic en la superficie de diseño y seleccione **detalles de Mapping**.</span><span class="sxs-lookup"><span data-stu-id="7e701-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="7e701-155">Seleccione el **persona** tipo de entidad y haga clic en **&lt;agregar una tabla o vista&gt;** en el **detalles de Mapping** ventana.</span><span class="sxs-lookup"><span data-stu-id="7e701-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="7e701-156">Seleccione \*\* PersonInfo \*\* en la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="7e701-156">Select \*\*PersonInfo \*\* from the drop-down list.</span></span>
    <span data-ttu-id="7e701-157">El **detalles de Mapping** ventana se actualiza con las asignaciones de columnas de forma predeterminada, estos están bien para nuestro escenario.</span><span class="sxs-lookup"><span data-stu-id="7e701-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="7e701-158">El **persona** tipo de entidad está asignado ahora a la **persona** y **PersonInfo** tablas.</span><span class="sxs-lookup"><span data-stu-id="7e701-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Asignación de 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="7e701-160">Usar el modelo</span><span class="sxs-lookup"><span data-stu-id="7e701-160">Use the Model</span></span>

-   <span data-ttu-id="7e701-161">Pegue el código siguiente en el método Main.</span><span class="sxs-lookup"><span data-stu-id="7e701-161">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   <span data-ttu-id="7e701-162">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7e701-162">Compile and run the application.</span></span>

<span data-ttu-id="7e701-163">Las siguientes instrucciones de T-SQL se ejecutaron en la base de datos como resultado de ejecutar esta aplicación.</span><span class="sxs-lookup"><span data-stu-id="7e701-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="7e701-164">Las dos siguientes **insertar** las instrucciones que se ejecutaron como resultado de ejecutar el contexto. SaveChanges().</span><span class="sxs-lookup"><span data-stu-id="7e701-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="7e701-165">Obtienen los datos el **persona** entidad y dividirla entre el **persona** y **PersonInfo** tablas.</span><span class="sxs-lookup"><span data-stu-id="7e701-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Insertar 1](~/ef6/media/insert1.png)

    ![Insertar 2](~/ef6/media/insert2.png)
-   <span data-ttu-id="7e701-168">La siguiente **seleccione** se ejecutó como resultado de la enumeración de las personas de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7e701-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="7e701-169">Combina los datos de la **persona** y **PersonInfo** tabla.</span><span class="sxs-lookup"><span data-stu-id="7e701-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Seleccionar](~/ef6/media/select.png)
