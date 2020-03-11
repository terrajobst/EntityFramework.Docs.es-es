---
title: Enlace de enlaces con WPF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414102"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="7e43a-102">Enlace de enlaces con WPF</span><span class="sxs-lookup"><span data-stu-id="7e43a-102">Databinding with WPF</span></span>
<span data-ttu-id="7e43a-103">En este tutorial paso a paso se muestra cómo enlazar tipos POCO a controles de WPF en un formulario "maestro-detalles".</span><span class="sxs-lookup"><span data-stu-id="7e43a-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="7e43a-104">La aplicación utiliza las API de Entity Framework para rellenar los objetos con datos de la base de datos, realizar un seguimiento de los cambios y conservar los datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7e43a-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="7e43a-105">El modelo define dos tipos que participan en una relación de uno a varios: **Category** (principal\\Master) y **Product** (Dependent\\Detail).</span><span class="sxs-lookup"><span data-stu-id="7e43a-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="7e43a-106">A continuación, se usan las herramientas de Visual Studio para enlazar los tipos definidos en el modelo a los controles de WPF.</span><span class="sxs-lookup"><span data-stu-id="7e43a-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="7e43a-107">El marco de enlace de datos de WPF permite la navegación entre objetos relacionados: la selección de filas en la vista maestra hace que la vista de detalles se actualice con los datos secundarios correspondientes.</span><span class="sxs-lookup"><span data-stu-id="7e43a-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="7e43a-108">Las capturas de pantalla y las listas de código de este tutorial se han tomado de Visual Studio 2013 pero puede completar este tutorial con Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="7e43a-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="7e43a-109">Usar la opción ' Object ' para crear orígenes de datos de WPF</span><span class="sxs-lookup"><span data-stu-id="7e43a-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="7e43a-110">Con la versión anterior de Entity Framework usamos para recomendar el uso de la opción de **base** de datos al crear un nuevo origen de datos basado en un modelo creado con el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="7e43a-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="7e43a-111">Esto se debe a que el diseñador genera un contexto que se deriva de las clases de ObjectContext y de entidad derivadas de EntityObject.</span><span class="sxs-lookup"><span data-stu-id="7e43a-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="7e43a-112">El uso de la opción de base de datos le ayudará a escribir el mejor código para interactuar con esta superficie de API.</span><span class="sxs-lookup"><span data-stu-id="7e43a-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="7e43a-113">Los diseñadores de EF para Visual Studio 2012 y Visual Studio 2013 generan un contexto que se deriva de DbContext junto con clases de entidad POCO simples.</span><span class="sxs-lookup"><span data-stu-id="7e43a-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="7e43a-114">Con Visual Studio 2010, se recomienda cambiar a una plantilla de generación de código que use DbContext, tal y como se describe más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="7e43a-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="7e43a-115">Al usar la superficie de API de DbContext, debe usar la opción de **objeto** al crear un nuevo origen de datos, como se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="7e43a-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="7e43a-116">Si es necesario, puede [revertir a la generación de código basada en ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) para los modelos creados con el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="7e43a-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="7e43a-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7e43a-117">Pre-Requisites</span></span>

<span data-ttu-id="7e43a-118">Debe tener Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 instalado para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="7e43a-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="7e43a-119">Si usa Visual Studio 2010, también tiene que instalar NuGet.</span><span class="sxs-lookup"><span data-stu-id="7e43a-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="7e43a-120">Para obtener más información, consulte [instalación de NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="7e43a-120">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="7e43a-121">Crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="7e43a-121">Create the Application</span></span>

-   <span data-ttu-id="7e43a-122">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e43a-122">Open Visual Studio</span></span>
-   <span data-ttu-id="7e43a-123">**Archivo-&gt; nuevo proyecto de&gt;....**</span><span class="sxs-lookup"><span data-stu-id="7e43a-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="7e43a-124">Seleccione **Windows** en el panel izquierdo y **WPFApplication** en el panel derecho.</span><span class="sxs-lookup"><span data-stu-id="7e43a-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="7e43a-125">Escriba **WPFwithEFSample** como nombre</span><span class="sxs-lookup"><span data-stu-id="7e43a-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="7e43a-126">Seleccione **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="7e43a-127">Instalar el paquete NuGet de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="7e43a-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="7e43a-128">En Explorador de soluciones, haga clic con el botón derecho en el proyecto **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="7e43a-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="7e43a-129">Seleccione **administrar paquetes NuGet.** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="7e43a-130">En el cuadro de diálogo administrar paquetes NuGet, seleccione la pestaña **en línea** y elija el paquete **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="7e43a-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="7e43a-131">Haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="7e43a-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="7e43a-132">Además del ensamblado EntityFramework, también se agrega una referencia a System. ComponentModel. DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="7e43a-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="7e43a-133">Si el proyecto tiene una referencia a System. Data. Entity, se quitará cuando se instale el paquete EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="7e43a-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="7e43a-134">El ensamblado System. Data. Entity ya no se utiliza para las aplicaciones de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="7e43a-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="7e43a-135">Definir un modelo</span><span class="sxs-lookup"><span data-stu-id="7e43a-135">Define a Model</span></span>

<span data-ttu-id="7e43a-136">En este tutorial, puede elegir implementar un modelo mediante Code First o el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="7e43a-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="7e43a-137">Complete una de las dos secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="7e43a-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="7e43a-138">Opción 1: definir un modelo con Code First</span><span class="sxs-lookup"><span data-stu-id="7e43a-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="7e43a-139">En esta sección se muestra cómo crear un modelo y su base de datos asociada mediante Code First.</span><span class="sxs-lookup"><span data-stu-id="7e43a-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="7e43a-140">Vaya a la sección siguiente (**opción 2: definir un modelo con Database First)** si prefiere usar Database First para aplicar ingeniería inversa al modelo desde una base de datos mediante el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="7e43a-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="7e43a-141">Al usar Code First desarrollo, normalmente comienza por escribir .NET Framework clases que definen el modelo conceptual (de dominio).</span><span class="sxs-lookup"><span data-stu-id="7e43a-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="7e43a-142">Agregue una nueva clase a **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="7e43a-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="7e43a-143">Haga clic con el botón derecho en el nombre del proyecto</span><span class="sxs-lookup"><span data-stu-id="7e43a-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="7e43a-144">Seleccione **Agregar**y, a continuación, **nuevo elemento** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="7e43a-145">Seleccione **clase** y escriba del **producto** para el nombre de clase.</span><span class="sxs-lookup"><span data-stu-id="7e43a-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="7e43a-146">Reemplace la definición de clase del **producto** por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7e43a-146">Replace the **Product** class definition with the following code:</span></span>

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

<span data-ttu-id="7e43a-147">La propiedad **Products** de la clase **categoría** y la propiedad **categoría** de la clase **Product** son propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="7e43a-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="7e43a-148">En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="7e43a-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="7e43a-149">Además de definir entidades, debe definir una clase que derive de DbContext y exponga las propiedades de DbSet&lt;de la carpa&gt;.</span><span class="sxs-lookup"><span data-stu-id="7e43a-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="7e43a-150">Las propiedades de DbSet&lt;la&gt; de la carpa permiten que el contexto sepa qué tipos desea incluir en el modelo.</span><span class="sxs-lookup"><span data-stu-id="7e43a-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="7e43a-151">Una instancia del tipo derivado de DbContext administra los objetos de entidad durante el tiempo de ejecución, lo que incluye el rellenado de objetos con datos de una base de datos, el seguimiento de cambios y la persistencia de datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7e43a-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="7e43a-152">Agregue una nueva clase **ProductContext** al proyecto con la siguiente definición:</span><span class="sxs-lookup"><span data-stu-id="7e43a-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="7e43a-153">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7e43a-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="7e43a-154">Opción 2: definir un modelo con Database First</span><span class="sxs-lookup"><span data-stu-id="7e43a-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="7e43a-155">En esta sección se muestra cómo usar Database First para aplicar ingeniería inversa al modelo desde una base de datos mediante el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="7e43a-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="7e43a-156">Si ha completado la sección anterior (**opción 1: definir un modelo con Code First)** , omita esta sección y vaya directamente a la sección de **carga diferida** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="7e43a-157">Crear una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="7e43a-157">Create an Existing Database</span></span>

<span data-ttu-id="7e43a-158">Normalmente, cuando el destino es una base de datos existente, ya se creará, pero para este tutorial es necesario crear una base de datos para tener acceso a.</span><span class="sxs-lookup"><span data-stu-id="7e43a-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="7e43a-159">El servidor de base de datos que se instala con Visual Studio es diferente en función de la versión de Visual Studio que haya instalado:</span><span class="sxs-lookup"><span data-stu-id="7e43a-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="7e43a-160">Si usa Visual Studio 2010, va a crear una base de datos de SQL Express.</span><span class="sxs-lookup"><span data-stu-id="7e43a-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="7e43a-161">Si usa Visual Studio 2012, va a crear una base de datos de [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) .</span><span class="sxs-lookup"><span data-stu-id="7e43a-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="7e43a-162">Vamos a generar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7e43a-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="7e43a-163">**Explorador de servidores de&gt; de vista**</span><span class="sxs-lookup"><span data-stu-id="7e43a-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="7e43a-164">Haga clic con el botón derecho en **conexiones de datos:&gt; agregar conexión...**</span><span class="sxs-lookup"><span data-stu-id="7e43a-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="7e43a-165">Si no se ha conectado a una base de datos desde Explorador de servidores antes de que tenga que seleccionar Microsoft SQL Server como origen de datos</span><span class="sxs-lookup"><span data-stu-id="7e43a-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Cambiar origen de datos](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="7e43a-167">Conéctese a LocalDB o a SQL Express, en función de la que haya instalado y escriba **Products** como nombre de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7e43a-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Agregar conexión LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Agregar conexión Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="7e43a-170">Seleccione **Aceptar** y se le preguntará si desea crear una nueva base de datos, seleccione **sí** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Creación de una base de datos](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="7e43a-172">La nueva base de datos aparecerá ahora en Explorador de servidores, haga clic con el botón derecho en ella y seleccione **nueva consulta** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="7e43a-173">Copie el siguiente código SQL en la nueva consulta, haga clic con el botón derecho en la consulta y seleccione **Ejecutar** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="7e43a-174">Modelo de ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="7e43a-174">Reverse Engineer Model</span></span>

<span data-ttu-id="7e43a-175">Vamos a hacer uso de Entity Framework Designer, que se incluye como parte de Visual Studio, para crear nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="7e43a-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="7e43a-176">**Proyecto-&gt; agregar nuevo elemento...**</span><span class="sxs-lookup"><span data-stu-id="7e43a-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="7e43a-177">Seleccione **datos** en el menú de la izquierda y, a continuación, **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="7e43a-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="7e43a-178">Escriba **ProductModel** como nombre y haga clic en **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="7e43a-179">Se iniciará el **Asistente para Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="7e43a-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="7e43a-180">Seleccione **generar desde la base de datos** y haga clic en **siguiente** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-180">Select **Generate from Database** and click **Next**</span></span>

    ![Elegir contenido de Model](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="7e43a-182">Seleccione la conexión a la base de datos creada en la primera sección, escriba **ProductContext** como nombre de la cadena de conexión y haga clic en **siguiente** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Elegir la conexión](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="7e43a-184">Haga clic en la casilla situada junto a "tablas" para importar todas las tablas y haga clic en "finalizar".</span><span class="sxs-lookup"><span data-stu-id="7e43a-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Elección de los objetos](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="7e43a-186">Una vez que se completa el proceso de ingeniería inversa, el nuevo modelo se agrega al proyecto y se abre para que pueda verlo en el Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="7e43a-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="7e43a-187">También se ha agregado un archivo app. config al proyecto con los detalles de conexión de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7e43a-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="7e43a-188">Pasos adicionales en Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="7e43a-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="7e43a-189">Si está trabajando en Visual Studio 2010, tendrá que actualizar el diseñador de EF para usar la generación de código EF6.</span><span class="sxs-lookup"><span data-stu-id="7e43a-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="7e43a-190">Haga clic con el botón derecho en una zona vacía del modelo en el diseñador de EF y seleccione **Agregar elemento de generación de código.** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="7e43a-191">Seleccione **plantillas en línea** en el menú de la izquierda y busque **DbContext**</span><span class="sxs-lookup"><span data-stu-id="7e43a-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="7e43a-192">Seleccione el **generador de DbContext de EF 6. x para C\#,** escriba **ProductsModel** como nombre y haga clic en Agregar.</span><span class="sxs-lookup"><span data-stu-id="7e43a-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="7e43a-193">Actualizar la generación de código para el enlace de datos</span><span class="sxs-lookup"><span data-stu-id="7e43a-193">Updating code generation for data binding</span></span>

<span data-ttu-id="7e43a-194">EF genera código a partir del modelo mediante plantillas T4.</span><span class="sxs-lookup"><span data-stu-id="7e43a-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="7e43a-195">Las plantillas que se incluyen con Visual Studio o que se descargan de la galería de Visual Studio están pensadas para uso general.</span><span class="sxs-lookup"><span data-stu-id="7e43a-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="7e43a-196">Esto significa que las entidades generadas a partir de estas plantillas tienen propiedades simples de ICollection&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="7e43a-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="7e43a-197">Sin embargo, al realizar el enlace de datos mediante WPF, es conveniente usar **ObservableCollection** para las propiedades de colección para que WPF pueda realizar un seguimiento de los cambios realizados en las colecciones.</span><span class="sxs-lookup"><span data-stu-id="7e43a-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="7e43a-198">A este fin, modificaremos las plantillas para usar ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="7e43a-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="7e43a-199">Abra el **Explorador de soluciones** y busque el archivo **ProductModel. edmx.**</span><span class="sxs-lookup"><span data-stu-id="7e43a-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="7e43a-200">Busque el archivo **ProductModel.TT** que se anidará en el archivo ProductModel. edmx.</span><span class="sxs-lookup"><span data-stu-id="7e43a-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Plantilla de modelo de producto de WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="7e43a-202">Haga doble clic en el archivo ProductModel.tt para abrirlo en el editor de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7e43a-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="7e43a-203">Busque y reemplace las dos apariciones de "**ICollection**" por "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="7e43a-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="7e43a-204">Se encuentran aproximadamente en las líneas 296 y 484.</span><span class="sxs-lookup"><span data-stu-id="7e43a-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="7e43a-205">Busque y reemplace la primera aparición de "**HashSet**" con "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="7e43a-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="7e43a-206">Esta repetición se encuentra aproximadamente en la línea 50.</span><span class="sxs-lookup"><span data-stu-id="7e43a-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="7e43a-207">**No** Reemplace la segunda aparición de HashSet que se encuentra más adelante en el código.</span><span class="sxs-lookup"><span data-stu-id="7e43a-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="7e43a-208">Busque y reemplace la única aparición de "**System. Collections. Generic**" por "**System. Collections. ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="7e43a-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="7e43a-209">Esto se encuentra aproximadamente en la línea 424.</span><span class="sxs-lookup"><span data-stu-id="7e43a-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="7e43a-210">Guarde el archivo ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="7e43a-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="7e43a-211">Esto debería hacer que se vuelva a generar el código para las entidades.</span><span class="sxs-lookup"><span data-stu-id="7e43a-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="7e43a-212">Si el código no se vuelve a generar automáticamente, haga clic con el botón derecho en ProductModel.tt y elija "ejecutar herramienta personalizada".</span><span class="sxs-lookup"><span data-stu-id="7e43a-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="7e43a-213">Si ahora abre el archivo Category.cs (que está anidado en ProductModel.tt), debería ver que la colección Products tiene el tipo **ObservableCollection&lt;&gt;de producto** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="7e43a-214">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7e43a-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="7e43a-215">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="7e43a-215">Lazy Loading</span></span>

<span data-ttu-id="7e43a-216">La propiedad **Products** de la clase **categoría** y la propiedad **categoría** de la clase **Product** son propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="7e43a-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="7e43a-217">En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="7e43a-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="7e43a-218">EF ofrece una opción para cargar automáticamente entidades relacionadas desde la base de datos la primera vez que se tiene acceso a la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="7e43a-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="7e43a-219">Con este tipo de carga (denominado carga diferida), tenga en cuenta que la primera vez que se tiene acceso a cada propiedad de navegación se ejecutará una consulta independiente en la base de datos si el contenido no está ya en el contexto.</span><span class="sxs-lookup"><span data-stu-id="7e43a-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="7e43a-220">Cuando se usan tipos de entidad POCO, EF consigue la carga diferida mediante la creación de instancias de tipos de proxy derivados durante el tiempo de ejecución y, a continuación, la invalidación de las propiedades virtuales en las clases para agregar el enlace de carga.</span><span class="sxs-lookup"><span data-stu-id="7e43a-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="7e43a-221">Para obtener la carga diferida de los objetos relacionados, debe declarar captadores de propiedades de navegación como **Public** y **virtual** (**reemplazable** en Visual Basic) y la clase no debe estar **sellada** (**NotOverridable** en Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="7e43a-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="7e43a-222">Cuando se usa Database First propiedades de navegación se convierten automáticamente en virtual para habilitar la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="7e43a-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="7e43a-223">En la sección Code First hemos optado por que las propiedades de navegación sean virtuales por la misma razón</span><span class="sxs-lookup"><span data-stu-id="7e43a-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="7e43a-224">Enlazar un objeto a controles</span><span class="sxs-lookup"><span data-stu-id="7e43a-224">Bind Object to Controls</span></span>

<span data-ttu-id="7e43a-225">Agregue las clases que se definen en el modelo como orígenes de datos para esta aplicación WPF.</span><span class="sxs-lookup"><span data-stu-id="7e43a-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="7e43a-226">Haga doble clic en **MainWindow. Xaml** en explorador de soluciones para abrir el formulario principal.</span><span class="sxs-lookup"><span data-stu-id="7e43a-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="7e43a-227">En el menú principal, seleccione **proyecto-&gt; agregar nuevo origen de datos.** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="7e43a-228">(en Visual Studio 2010, debe seleccionar **datos-&gt; agregar nuevo origen de datos...** )</span><span class="sxs-lookup"><span data-stu-id="7e43a-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="7e43a-229">En elegir un origen de datos Typewindow, seleccione **objeto** y haga clic en **siguiente** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="7e43a-230">En el cuadro de diálogo Seleccionar los objetos de datos, **WPFwithEFSample** dos veces la y seleccione **categoría** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="7e43a-231">*No es necesario seleccionar el origen de datos del **producto** , porque lo haremos a través de la propiedad del **producto**en el origen de datos de la **categoría** .*</span><span class="sxs-lookup"><span data-stu-id="7e43a-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Seleccionar objetos de datos](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="7e43a-233">Haga clic en **Finish** (Finalizar).</span><span class="sxs-lookup"><span data-stu-id="7e43a-233">Click **Finish.**</span></span>
-   <span data-ttu-id="7e43a-234">La ventana orígenes de datos se abre junto a la ventana MainWindow. XAML *si la ventana orígenes de datos no aparece, seleccione **ver-&gt; otros orígenes de datos&gt; de Windows***  .</span><span class="sxs-lookup"><span data-stu-id="7e43a-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="7e43a-235">Presione el icono de anclaje para que la ventana orígenes de datos no se oculte automáticamente.</span><span class="sxs-lookup"><span data-stu-id="7e43a-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="7e43a-236">Es posible que tenga que presionar el botón actualizar si la ventana ya estaba visible.</span><span class="sxs-lookup"><span data-stu-id="7e43a-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Orígenes de datos](~/ef6/media/datasources.png)

-   <span data-ttu-id="7e43a-238">Seleccione el origen de datos de la **categoría** y arrástrelo en el formulario.</span><span class="sxs-lookup"><span data-stu-id="7e43a-238">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="7e43a-239">Al arrastrar este origen, ocurre lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7e43a-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="7e43a-240">El recurso **categoryViewSource** y el control **categoryDataGrid** se agregaron a XAML</span><span class="sxs-lookup"><span data-stu-id="7e43a-240">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="7e43a-241">La propiedad DataContext del elemento Grid primario se estableció en "{StaticResource **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="7e43a-241">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="7e43a-242"> El recurso **categoryViewSource** actúa como origen de enlace para el elemento primario de la cuadrícula externa\\.</span><span class="sxs-lookup"><span data-stu-id="7e43a-242"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="7e43a-243">Los elementos de la cuadrícula interna heredan entonces el valor DataContext de la cuadrícula primaria (la propiedad ItemsSource de categoryDataGrid se establece en "{Binding}")</span><span class="sxs-lookup"><span data-stu-id="7e43a-243">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a><span data-ttu-id="7e43a-244">Agregar una cuadrícula de detalles</span><span class="sxs-lookup"><span data-stu-id="7e43a-244">Adding a Details Grid</span></span>

<span data-ttu-id="7e43a-245">Ahora que tenemos una cuadrícula para mostrar las categorías, vamos a agregar una cuadrícula de detalles para mostrar los productos asociados.</span><span class="sxs-lookup"><span data-stu-id="7e43a-245">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="7e43a-246">Seleccione la propiedad **Products** en el origen de datos **Category** y arrástrela en el formulario.</span><span class="sxs-lookup"><span data-stu-id="7e43a-246">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="7e43a-247">El recurso **categoryProductsViewSource** y la cuadrícula **productDataGrid** se agregan a XAML</span><span class="sxs-lookup"><span data-stu-id="7e43a-247">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="7e43a-248">La ruta de acceso de enlace de este recurso se establece en Products</span><span class="sxs-lookup"><span data-stu-id="7e43a-248">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="7e43a-249">El marco de enlace de datos de WPF garantiza que solo los productos relacionados con la categoría seleccionada se muestran en **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="7e43a-249">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="7e43a-250">En el cuadro de herramientas, arrastre el **botón** al formulario.</span><span class="sxs-lookup"><span data-stu-id="7e43a-250">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="7e43a-251">Establezca la propiedad **nombre** en **buttonSave** y la propiedad **contenido** en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="7e43a-251">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="7e43a-252">El formulario debe tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="7e43a-252">The form should look similar to this:</span></span>

![Diseñador](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="7e43a-254">Agregar código que controla la interacción de datos</span><span class="sxs-lookup"><span data-stu-id="7e43a-254">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="7e43a-255">Es el momento de agregar algunos controladores de eventos a la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="7e43a-255">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="7e43a-256">En la ventana XAML, haga clic en el elemento **&lt;ventana** , que selecciona la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="7e43a-256">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="7e43a-257">En la ventana **propiedades** , elija **eventos** en la parte superior derecha y, a continuación, haga doble clic en el cuadro de texto a la derecha de la etiqueta **cargada** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-257">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Propiedades de la ventana principal](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="7e43a-259">Agregue también el evento **click** del botón **Guardar** haciendo doble clic en el botón Guardar en el diseñador.</span><span class="sxs-lookup"><span data-stu-id="7e43a-259">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="7e43a-260">Esto le lleva al código subyacente para el formulario. ahora editaremos el código para usar el ProductContext para realizar el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="7e43a-260">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="7e43a-261">Actualice el código para MainWindow, tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="7e43a-261">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="7e43a-262">El código declara una instancia de ejecución prolongada de **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="7e43a-262">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="7e43a-263">El objeto **ProductContext** se usa para consultar y guardar datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7e43a-263">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="7e43a-264">A continuación, se llama a **Dispose ()** en la instancia de **ProductContext** desde el método de **cierre** de sesión reemplazado.</span><span class="sxs-lookup"><span data-stu-id="7e43a-264">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="7e43a-265"> Los comentarios de código proporcionan detalles sobre lo que hace el código.</span><span class="sxs-lookup"><span data-stu-id="7e43a-265"> The code comments provide details about what the code does.</span></span>

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a><span data-ttu-id="7e43a-266">Prueba de la aplicación WPF</span><span class="sxs-lookup"><span data-stu-id="7e43a-266">Test the WPF Application</span></span>

-   <span data-ttu-id="7e43a-267">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7e43a-267">Compile and run the application.</span></span> <span data-ttu-id="7e43a-268">Si ha usado Code First, verá que se ha creado una base de datos **WPFwithEFSample. ProductContext** .</span><span class="sxs-lookup"><span data-stu-id="7e43a-268">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="7e43a-269">Escriba un nombre de categoría en la cuadrícula superior y los nombres de producto en la cuadrícula inferior *no escriba nada en las columnas de ID., porque la base de datos genera la clave principal.*</span><span class="sxs-lookup"><span data-stu-id="7e43a-269">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Ventana principal con nuevas categorías y productos](~/ef6/media/screen1.png)

-   <span data-ttu-id="7e43a-271">Presione el botón **Guardar** para guardar los datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7e43a-271">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="7e43a-272">Después de la llamada a **SaveChanges ()** de DbContext, los identificadores se rellenan con los valores generados por la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7e43a-272">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="7e43a-273">Dado que llamamos a **Refresh ()** después de **SaveChanges ()** , los controles **DataGrid** también se actualizan con los nuevos valores.</span><span class="sxs-lookup"><span data-stu-id="7e43a-273">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Ventana principal con los identificadores rellenados](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="7e43a-275">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7e43a-275">Additional Resources</span></span>

<span data-ttu-id="7e43a-276">Para obtener más información sobre el enlace de datos a colecciones mediante WPF, vea [este tema](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) en la documentación de WPF.</span><span class="sxs-lookup"><span data-stu-id="7e43a-276">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
