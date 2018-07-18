---
title: Enlace de datos con WPF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
caps.latest.revision: 3
ms.openlocfilehash: 1756ec14fe83d80199b6040bd345dc2fe6294281
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122304"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="47de9-102">Enlace de datos con WPF</span><span class="sxs-lookup"><span data-stu-id="47de9-102">Databinding with WPF</span></span>
<span data-ttu-id="47de9-103">En este tutorial paso a paso muestra cómo enlazar tipos POCO a controles de WPF en un formulario de "master-detail".</span><span class="sxs-lookup"><span data-stu-id="47de9-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="47de9-104">La aplicación utiliza la API de Entity Framework para rellenar los objetos con los datos de la base de datos, realizar un seguimiento de cambios y conservar los datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47de9-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="47de9-105">El modelo define dos tipos que participan en la relación de uno a varios: **categoría** (entidad de seguridad\\maestra) y **producto** (dependientes\\detalle).</span><span class="sxs-lookup"><span data-stu-id="47de9-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="47de9-106">A continuación, se usan las herramientas de Visual Studio para enlazar los tipos definidos en el modelo a los controles WPF.</span><span class="sxs-lookup"><span data-stu-id="47de9-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="47de9-107">El marco de enlace de datos WPF permite la navegación entre los objetos relacionados: selección de filas en la vista principal hace que la vista de detalle para actualizar con los datos secundarios correspondientes.</span><span class="sxs-lookup"><span data-stu-id="47de9-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="47de9-108">Las capturas de pantalla y los listados de código de este tutorial se toman de Visual Studio 2013, pero se puede completar este tutorial con Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="47de9-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="47de9-109">Utilice la opción de 'Object' para la creación de orígenes de datos WPF</span><span class="sxs-lookup"><span data-stu-id="47de9-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="47de9-110">Con una versión anterior de Entity Framework se usa para recomienda el uso de la **base de datos** opción al crear un nuevo origen de datos basado en un modelo creado con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="47de9-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="47de9-111">Esto era porque el diseñador generaría un contexto que deriva de ObjectContext y clases de entidad que se derivan de EntityObject.</span><span class="sxs-lookup"><span data-stu-id="47de9-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="47de9-112">Con la opción de base de datos le ayudarán a escribir el código mejor para interactuar con esta superficie de API.</span><span class="sxs-lookup"><span data-stu-id="47de9-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="47de9-113">Los diseñadores de EF para Visual Studio 2012 y Visual Studio 2013 generar un contexto que se deriva de DbContext, junto con clases de entidad POCO simple.</span><span class="sxs-lookup"><span data-stu-id="47de9-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="47de9-114">Se recomienda usarlo en una plantilla de generación de código que use DbContext, tal como se describe más adelante en este tutorial con Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="47de9-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="47de9-115">Si usa la superficie de API de DbContext se debe usar el **objeto** opción al crear un nuevo origen de datos, como se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="47de9-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="47de9-116">Si es necesario, puede [revertir a la generación de código en función de ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) para los modelos creados con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="47de9-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="47de9-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="47de9-117">Pre-Requisites</span></span>

<span data-ttu-id="47de9-118">Debe tener Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 instalado para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="47de9-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="47de9-119">Si utiliza Visual Studio 2010, también debe instalar NuGet.</span><span class="sxs-lookup"><span data-stu-id="47de9-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="47de9-120">Para obtener más información, consulte [instalar NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="47de9-120">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="47de9-121">Crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="47de9-121">Create the Application</span></span>

-   <span data-ttu-id="47de9-122">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47de9-122">Open Visual Studio</span></span>
-   <span data-ttu-id="47de9-123">**Archivo -&gt; nuevo:&gt; proyecto...**</span><span class="sxs-lookup"><span data-stu-id="47de9-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="47de9-124">Seleccione **Windows** en el panel izquierdo y **WPFApplication** en el panel derecho</span><span class="sxs-lookup"><span data-stu-id="47de9-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="47de9-125">Escriba **WPFwithEFSample** como el nombre</span><span class="sxs-lookup"><span data-stu-id="47de9-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="47de9-126">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="47de9-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="47de9-127">Instale el paquete NuGet de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="47de9-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="47de9-128">En el Explorador de soluciones, haga doble clic en el **WinFormswithEFSample** proyecto</span><span class="sxs-lookup"><span data-stu-id="47de9-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="47de9-129">Seleccione **administrar paquetes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="47de9-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="47de9-130">En el cuadro de diálogo Administrar paquetes de NuGet, seleccione el **Online** pestaña y elija el **EntityFramework** paquete</span><span class="sxs-lookup"><span data-stu-id="47de9-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="47de9-131">Haga clic en **instalar**</span><span class="sxs-lookup"><span data-stu-id="47de9-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="47de9-132">Además del ensamblado de Entity Framework también se agrega una referencia a System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="47de9-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="47de9-133">Si el proyecto tiene una referencia a System.Data.Entity, a continuación, se eliminará cuando se instala el paquete de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="47de9-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="47de9-134">El ensamblado System.Data.Entity ya no se usa para las aplicaciones de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="47de9-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="47de9-135">Definir un modelo</span><span class="sxs-lookup"><span data-stu-id="47de9-135">Define a Model</span></span>

<span data-ttu-id="47de9-136">En este tutorial, puede decidió implementar un modelo con Code First o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="47de9-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="47de9-137">Realice una de las dos secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="47de9-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="47de9-138">Opción 1: Definir un modelo con Code First</span><span class="sxs-lookup"><span data-stu-id="47de9-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="47de9-139">En esta sección se muestra cómo crear un modelo y su base de datos asociada mediante Code First.</span><span class="sxs-lookup"><span data-stu-id="47de9-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="47de9-140">Saltar a la siguiente sección (**opción 2: definir un modelo con Database First)** si prefiere usar Database First para invertir diseñar el modelo desde una base de datos mediante el diseñador EF</span><span class="sxs-lookup"><span data-stu-id="47de9-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="47de9-141">Cuando usando desarrollo Code First suele comenzar mediante la escritura de las clases de .NET Framework que definen el modelo conceptual (dominio).</span><span class="sxs-lookup"><span data-stu-id="47de9-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="47de9-142">Agregue una nueva clase a la **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="47de9-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="47de9-143">Haga doble clic en el nombre del proyecto</span><span class="sxs-lookup"><span data-stu-id="47de9-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="47de9-144">Seleccione **agregar**, a continuación, **nuevo elemento**</span><span class="sxs-lookup"><span data-stu-id="47de9-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="47de9-145">Seleccione **clase** y escriba **producto** para el nombre de clase</span><span class="sxs-lookup"><span data-stu-id="47de9-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="47de9-146">Reemplace el **producto** definición con el siguiente código de clase:</span><span class="sxs-lookup"><span data-stu-id="47de9-146">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="47de9-147">El **productos** propiedad en el **categoría** clase y **categoría** propiedad en el **producto** clase son las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="47de9-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="47de9-148">En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="47de9-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="47de9-149">Además de definir las entidades, debe definir una clase que deriva de DbContext y expone DbSet&lt;TEntity&gt; propiedades.</span><span class="sxs-lookup"><span data-stu-id="47de9-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="47de9-150">La clase DbSet&lt;TEntity&gt; propiedades que el contexto sepan qué tipos van a incluir en el modelo.</span><span class="sxs-lookup"><span data-stu-id="47de9-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="47de9-151">Una instancia del tipo derivado de DbContext administra los objetos entidad durante el tiempo de ejecución, que incluye rellenar objetos con los datos de una base de datos, cambie el seguimiento y almacenar datos a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47de9-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="47de9-152">Agregue un nuevo **ProductContext** clase al proyecto con la siguiente definición:</span><span class="sxs-lookup"><span data-stu-id="47de9-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="47de9-153">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="47de9-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="47de9-154">Opción 2: Definir un modelo con Database First</span><span class="sxs-lookup"><span data-stu-id="47de9-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="47de9-155">Esta sección muestra cómo usar la primera base de datos para aplicar ingeniería inversa a su modelo de una base de datos con EF designer.</span><span class="sxs-lookup"><span data-stu-id="47de9-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="47de9-156">Si ha completado la sección anterior (**opción 1: definir un modelo con Code First)**, omita esta sección e ir directamente a la **la carga diferida** sección.</span><span class="sxs-lookup"><span data-stu-id="47de9-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="47de9-157">Crear una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="47de9-157">Create an Existing Database</span></span>

<span data-ttu-id="47de9-158">Normalmente, cuando tiene como destino una base de datos existente que ya se creará, pero para este tutorial es necesario crear una base de datos para tener acceso a.</span><span class="sxs-lookup"><span data-stu-id="47de9-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="47de9-159">El servidor de base de datos que se instala con Visual Studio es diferente según la versión de Visual Studio que ha instalado:</span><span class="sxs-lookup"><span data-stu-id="47de9-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="47de9-160">Si usa Visual Studio 2010 se creará una base de datos de SQL Express.</span><span class="sxs-lookup"><span data-stu-id="47de9-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="47de9-161">Si usa Visual Studio 2012, a continuación, se creará un [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) base de datos.</span><span class="sxs-lookup"><span data-stu-id="47de9-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="47de9-162">Sigamos adelante y generar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47de9-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="47de9-163">**Vista -&gt; Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="47de9-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="47de9-164">Haga clic con el botón derecho en **conexiones de datos -&gt; Agregar conexión...**</span><span class="sxs-lookup"><span data-stu-id="47de9-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="47de9-165">Si aún no lo ha conectado a una base de datos del explorador de servidores antes de que tendrá que seleccionar Microsoft SQL Server como origen de datos</span><span class="sxs-lookup"><span data-stu-id="47de9-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="47de9-167">Conectarse a LocalDB o Express de SQL, según lo que se ha instalado y escriba **productos** como el nombre de la base de datos</span><span class="sxs-lookup"><span data-stu-id="47de9-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="47de9-170">Seleccione **Aceptar** y se le solicitará si desea crear una nueva base de datos, seleccione **sí**</span><span class="sxs-lookup"><span data-stu-id="47de9-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="47de9-172">La nueva base de datos aparecerá ahora en el Explorador de servidores, haga doble clic en él y seleccione **nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="47de9-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="47de9-173">Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **Execute**</span><span class="sxs-lookup"><span data-stu-id="47de9-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="47de9-174">Modelo de ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="47de9-174">Reverse Engineer Model</span></span>

<span data-ttu-id="47de9-175">Vamos a usar Entity Framework Designer, que se incluye como parte de Visual Studio, para crear nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="47de9-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="47de9-176">**Proyecto -&gt; Agregar nuevo elemento...**</span><span class="sxs-lookup"><span data-stu-id="47de9-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="47de9-177">Seleccione **datos** en el menú izquierdo y, a continuación, **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="47de9-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="47de9-178">Escriba **ProductModel** como el nombre y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="47de9-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="47de9-179">Esto inicia el **Asistente para Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="47de9-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="47de9-180">Seleccione **generar desde la base de datos** y haga clic en **siguiente**</span><span class="sxs-lookup"><span data-stu-id="47de9-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="47de9-182">Seleccione la conexión a la base de datos que creó en la primera sección, escriba **ProductContext** como el nombre de la cadena de conexión y haga clic en **siguiente**</span><span class="sxs-lookup"><span data-stu-id="47de9-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="47de9-184">Haga clic en la casilla de verificación situada junto a 'Tablas' para importar todas las tablas y haga clic en 'Finalizar'</span><span class="sxs-lookup"><span data-stu-id="47de9-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="47de9-186">Una vez completado el proceso de ingeniería inversa el nuevo modelo se agrega al proyecto y se abrirá para que pueda ver en el Diseñador de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="47de9-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="47de9-187">También se agregó un archivo App.config al proyecto con los detalles de conexión para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47de9-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="47de9-188">Pasos adicionales en Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="47de9-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="47de9-189">Si está trabajando en Visual Studio 2010, a continuación, deberá actualizar el diseñador EF para usar la generación de código de EF6.</span><span class="sxs-lookup"><span data-stu-id="47de9-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="47de9-190">Haga doble clic en una zona vacía del modelo de EF Designer y seleccione **Add Code Generation Item...**</span><span class="sxs-lookup"><span data-stu-id="47de9-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="47de9-191">Seleccione **plantillas en línea** desde el menú izquierdo y busque **DbContext**</span><span class="sxs-lookup"><span data-stu-id="47de9-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="47de9-192">Seleccione el **EF 6.x generador de DbContext para C\#,** escriba **ProductsModel** como el nombre y haga clic en Agregar</span><span class="sxs-lookup"><span data-stu-id="47de9-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="47de9-193">Actualizando la generación de código para el enlace de datos</span><span class="sxs-lookup"><span data-stu-id="47de9-193">Updating code generation for data binding</span></span>

<span data-ttu-id="47de9-194">EF genera código desde el modelo usando plantillas T4.</span><span class="sxs-lookup"><span data-stu-id="47de9-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="47de9-195">Las plantillas se incluye con Visual Studio o se descarga desde la Galería de Visual Studio están diseñadas para su uso de propósito general.</span><span class="sxs-lookup"><span data-stu-id="47de9-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="47de9-196">Esto significa que las entidades generadas a partir de estas plantillas tienen ICollection simple&lt;T&gt; propiedades.</span><span class="sxs-lookup"><span data-stu-id="47de9-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="47de9-197">Sin embargo, al realizar datos enlace con WPF es deseable utilizar **ObservableCollection** para las propiedades de colección, por lo que WPF puede realizar un seguimiento de los cambios realizados en las colecciones.</span><span class="sxs-lookup"><span data-stu-id="47de9-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="47de9-198">Para ello, vamos a modificar las plantillas para usar ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="47de9-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="47de9-199">Abra el **el Explorador de soluciones** y encontrar **ProductModel.edmx** archivo</span><span class="sxs-lookup"><span data-stu-id="47de9-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="47de9-200">Buscar el **ProductModel.tt** archivo que se anidará en el archivo ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="47de9-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![WpfProductModelTemplate](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="47de9-202">Haga doble clic en el archivo ProductModel.tt para abrirlo en el editor de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47de9-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="47de9-203">Buscar y reemplazar las dos repeticiones de "**ICollection**"con"**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="47de9-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="47de9-204">Estos se encuentran aproximadamente en las líneas 296 y 484.</span><span class="sxs-lookup"><span data-stu-id="47de9-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="47de9-205">Buscar y reemplazar la primera aparición de "**HashSet**"con"**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="47de9-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="47de9-206">Esta falta de disponibilidad se encuentra aproximadamente en línea 50.</span><span class="sxs-lookup"><span data-stu-id="47de9-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="47de9-207">**No** reemplazar la segunda aparición de HashSet encuentra más adelante en el código.</span><span class="sxs-lookup"><span data-stu-id="47de9-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="47de9-208">Buscar y reemplazar la aparición única de "**System.Collections.Generic**"con"**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="47de9-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="47de9-209">Esto se encuentra aproximadamente en línea 424.</span><span class="sxs-lookup"><span data-stu-id="47de9-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="47de9-210">Guarde el archivo ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="47de9-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="47de9-211">Esto debería producir el código para las entidades a generarse.</span><span class="sxs-lookup"><span data-stu-id="47de9-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="47de9-212">Si el código no se pueden regenerar automáticamente, a continuación, haga clic con el botón derecho en ProductModel.tt y elija "Ejecutar herramienta personalizada".</span><span class="sxs-lookup"><span data-stu-id="47de9-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="47de9-213">Si ahora abra el archivo Category.cs (que está anidado bajo ProductModel.tt), a continuación, debería ver que la colección de productos tiene el tipo **ObservableCollection&lt;producto&gt;**.</span><span class="sxs-lookup"><span data-stu-id="47de9-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="47de9-214">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="47de9-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="47de9-215">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="47de9-215">Lazy Loading</span></span>

<span data-ttu-id="47de9-216">El **productos** propiedad en el **categoría** clase y **categoría** propiedad en el **producto** clase son las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="47de9-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="47de9-217">En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="47de9-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="47de9-218">EF ofrece una opción de cargar entidades relacionadas de la base de datos automáticamente la primera vez que se tener acceso a la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="47de9-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="47de9-219">Con este tipo de carga (denominada la carga diferida), tenga en cuenta que la primera vez que se tiene acceso a cada propiedad de navegación una consulta independiente se ejecutará en la base de datos si el contenido no está en el contexto.</span><span class="sxs-lookup"><span data-stu-id="47de9-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="47de9-220">Al utilizar tipos de entidad POCO, EF logra la carga diferida al crear instancias de tipos de proxy derivado en tiempo de ejecución y, a continuación, reemplazar propiedades virtuales en las clases para agregar el enlace de carga.</span><span class="sxs-lookup"><span data-stu-id="47de9-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="47de9-221">Para obtener la carga diferida de objetos relacionados, debe declarar navegación captadores de propiedades como **pública** y **virtual** (**Overridable** en Visual Basic), y la clase no debe ser **sealed** (**NotOverridable** en Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="47de9-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="47de9-222">Cuando la base de datos utilizando las propiedades de navegación primera se realizan automáticamente virtuales para habilitar la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="47de9-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="47de9-223">En la sección Code First hemos optado por hacer que las propiedades de navegación virtual por la misma razón</span><span class="sxs-lookup"><span data-stu-id="47de9-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="47de9-224">Enlazar objetos a controles</span><span class="sxs-lookup"><span data-stu-id="47de9-224">Bind Object to Controls</span></span>

<span data-ttu-id="47de9-225">Agregue las clases que se definen en el modelo como orígenes de datos para esta aplicación de WPF.</span><span class="sxs-lookup"><span data-stu-id="47de9-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="47de9-226">Haga doble clic en **MainWindow.xaml** en el Explorador de soluciones para abrir el formulario principal</span><span class="sxs-lookup"><span data-stu-id="47de9-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="47de9-227">En el menú principal, seleccione **proyecto -&gt; Agregar nuevo origen de datos...**</span><span class="sxs-lookup"><span data-stu-id="47de9-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="47de9-228">(en Visual Studio 2010, deberá seleccionar **datos -&gt; Agregar nuevo origen de datos...** )</span><span class="sxs-lookup"><span data-stu-id="47de9-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="47de9-229">En la elegir un Typewindow del origen de datos, seleccione **objeto** y haga clic en **siguiente**</span><span class="sxs-lookup"><span data-stu-id="47de9-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="47de9-230">En el cuadro de diálogo de objetos de datos, seleccione expandir la **WPFwithEFSample** dos veces y seleccione **categoría**</span><span class="sxs-lookup"><span data-stu-id="47de9-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="47de9-231">*No es necesario para seleccionar la **producto** del origen de datos, ya que obtenemos a él a través de la **producto**de propiedad en el **categoría** origen de datos*</span><span class="sxs-lookup"><span data-stu-id="47de9-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![SelectDataObjects](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="47de9-233">Haga clic en **Finalizar.**</span><span class="sxs-lookup"><span data-stu-id="47de9-233">Click **Finish.**</span></span>
-   <span data-ttu-id="47de9-234">Se abre la ventana de orígenes de datos junto a la ventana de MainWindow.xaml *si la ventana de orígenes de datos no aparece, seleccione **vista -&gt; otras Windows -&gt; orígenes de datos***</span><span class="sxs-lookup"><span data-stu-id="47de9-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="47de9-235">Presione el icono de anclaje, por lo que la ventana de orígenes de datos no auto ocultar.</span><span class="sxs-lookup"><span data-stu-id="47de9-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="47de9-236">Deberá presionar el botón de actualización si la ventana que ya estaba visible.</span><span class="sxs-lookup"><span data-stu-id="47de9-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![DataSources](~/ef6/media/datasources.png)

-   <span data-ttu-id="47de9-238">Seleccione el ** categoría ** origen de datos y arrastrarlo en el formulario.</span><span class="sxs-lookup"><span data-stu-id="47de9-238">Select the **Category **data source and drag it on the form.</span></span>

<span data-ttu-id="47de9-239">El siguiente, se produjo cuando se arrastra este origen:</span><span class="sxs-lookup"><span data-stu-id="47de9-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="47de9-240">El **categoryViewSource** recursos y la ** categoryDataGrid ** se agregó a XAML.</span><span class="sxs-lookup"><span data-stu-id="47de9-240">The **categoryViewSource** resource and the** categoryDataGrid** control were added to XAML.</span></span> <span data-ttu-id="47de9-241">Para obtener más información sobre DataViewSources, consulte http://bea.stollnitz.com/blog/?p=387.</span><span class="sxs-lookup"><span data-stu-id="47de9-241">For more information about DataViewSources, see http://bea.stollnitz.com/blog/?p=387.</span></span>
-   <span data-ttu-id="47de9-242">La propiedad DataContext en el elemento de cuadrícula principal se estableció en "{StaticResource **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="47de9-242">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span>  <span data-ttu-id="47de9-243">El **categoryViewSource** recursos actúa como un origen de enlace de la actividad\\elemento de cuadrícula principal.</span><span class="sxs-lookup"><span data-stu-id="47de9-243">The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="47de9-244">Los elementos internos de la cuadrícula, a continuación, heredan el valor de DataContext de la cuadrícula (la propiedad ItemsSource del categoryDataGrid se establece en "{Binding}") primario.</span><span class="sxs-lookup"><span data-stu-id="47de9-244">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}").</span></span> 

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="47de9-245">Agregar una cuadrícula de detalles</span><span class="sxs-lookup"><span data-stu-id="47de9-245">Adding a Details Grid</span></span>

<span data-ttu-id="47de9-246">Ahora que tenemos una cuadrícula para mostrar las categorías vamos a agregar una cuadrícula de detalles para mostrar los productos asociados.</span><span class="sxs-lookup"><span data-stu-id="47de9-246">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="47de9-247">Seleccione el ** productos ** propiedad en la ** categoría ** origen de datos y arrastrarlo en el formulario.</span><span class="sxs-lookup"><span data-stu-id="47de9-247">Select the **Products **property from under the **Category **data source and drag it on the form.</span></span>
    -   <span data-ttu-id="47de9-248">El **categoryProductsViewSource** recursos y **productDataGrid** cuadrícula se agregan a XAML</span><span class="sxs-lookup"><span data-stu-id="47de9-248">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="47de9-249">La ruta de acceso de enlace para este recurso está establecido en productos</span><span class="sxs-lookup"><span data-stu-id="47de9-249">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="47de9-250">Marco de enlace de datos WPF garantiza que solo productos relacionados con la categoría seleccionada se mostrarán en **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="47de9-250">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="47de9-251">En el cuadro de herramientas, arrastre **botón** sesión en el formulario.</span><span class="sxs-lookup"><span data-stu-id="47de9-251">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="47de9-252">Establecer el **nombre** propiedad **buttonSave** y el **contenido** propiedad **guardar**.</span><span class="sxs-lookup"><span data-stu-id="47de9-252">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="47de9-253">El formato debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="47de9-253">The form should look similar to this:</span></span>

![Diseñador](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="47de9-255">Agregue código que administra la interacción de datos</span><span class="sxs-lookup"><span data-stu-id="47de9-255">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="47de9-256">Es el momento de agregar algunos controladores de eventos a la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="47de9-256">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="47de9-257">En la ventana XAML, haga clic en el  **&lt;ventana** elemento, este comando selecciona la ventana principal</span><span class="sxs-lookup"><span data-stu-id="47de9-257">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="47de9-258">En el **propiedades** ventana Elegir **eventos** en la esquina superior derecha, a continuación, haga doble clic en el cuadro de texto a la derecha de la **Loaded** etiqueta</span><span class="sxs-lookup"><span data-stu-id="47de9-258">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![MainWindowProperties](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="47de9-260">Agregue también el **haga clic en** eventos para el **guardar** botón haciendo doble clic en el botón Guardar en el diseñador.</span><span class="sxs-lookup"><span data-stu-id="47de9-260">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="47de9-261">Aparecerá el código subyacente del formulario, ahora se modificará el código para usar el ProductContext para tener acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="47de9-261">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="47de9-262">Actualice el código para MainWindow tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="47de9-262">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="47de9-263">El código declara una instancia de ejecución prolongada de **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="47de9-263">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="47de9-264">El **ProductContext** objeto se usa para consultar y guardar datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47de9-264">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="47de9-265">El **Dispose**() en el **ProductContext** , a continuación, se denomina instancia de invalidado **OnClosing** método.</span><span class="sxs-lookup"><span data-stu-id="47de9-265">The **Dispose**() on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span> <span data-ttu-id="47de9-266">Los comentarios del código proporcionan detalles sobre lo que hace el código.</span><span class="sxs-lookup"><span data-stu-id="47de9-266">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="47de9-267">Probar la aplicación de WPF</span><span class="sxs-lookup"><span data-stu-id="47de9-267">Test the WPF Application</span></span>

-   <span data-ttu-id="47de9-268">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="47de9-268">Compile and run the application.</span></span> <span data-ttu-id="47de9-269">Si utilizó Code First, verá que una **WPFwithEFSample.ProductContext** base de datos se crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="47de9-269">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="47de9-270">Escriba un nombre de categoría en los nombres de producto y de cuadrícula principales en la cuadrícula inferior *no escribir nada en las columnas de identificador, ya que la clave principal se genera la base de datos*</span><span class="sxs-lookup"><span data-stu-id="47de9-270">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Screen1](~/ef6/media/screen1.png)

-   <span data-ttu-id="47de9-272">Presione el **guardar** botón para guardar los datos en la base de datos</span><span class="sxs-lookup"><span data-stu-id="47de9-272">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="47de9-273">Después de llamar a la clase DbContext **SaveChanges**(), los identificadores se rellenan con los valores de la base de datos generada.</span><span class="sxs-lookup"><span data-stu-id="47de9-273">After the call to DbContext’s **SaveChanges**(), the IDs are populated with the database generated values.</span></span> <span data-ttu-id="47de9-274">Dado que se llama **actualizar**() después de **SaveChanges**() la **DataGrid** controles se actualizan con los nuevos valores.</span><span class="sxs-lookup"><span data-stu-id="47de9-274">Because we called **Refresh**() after **SaveChanges**() the **DataGrid** controls are updated with the new values as well.</span></span>

![Screen2](~/ef6/media/screen2.png)
