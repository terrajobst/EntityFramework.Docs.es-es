---
title: Enlace de datos con WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639141"
---
> [!IMPORTANT]
> <span data-ttu-id="66945-102">**Este documento es válido para WPF en .NET Framework**</span><span class="sxs-lookup"><span data-stu-id="66945-102">**This document is valid for WPF on the .NET Framework only**</span></span>
>
> <span data-ttu-id="66945-103">En este documento se describe el enlace de datos para WPF en .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="66945-103">This document describes databinding for WPF on the .NET Framework.</span></span> <span data-ttu-id="66945-104">Para los nuevos proyectos de .NET Core, se recomienda usar [EF Core](/ef/core) en lugar de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="66945-104">For new .NET Core projects, we recommend you use [EF Core](/ef/core) instead of Entity Framework 6.</span></span> <span data-ttu-id="66945-105">Se realiza un seguimiento de la documentación para el enlace de datos en EF Core en [el #778.](https://github.com/dotnet/EntityFramework.Docs/issues/778)</span><span class="sxs-lookup"><span data-stu-id="66945-105">The documentation for databinding in EF Core is tracked in [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span></span>

# <a name="databinding-with-wpf"></a><span data-ttu-id="66945-106">Databinding with WPF (Enlace de datos con WPF)</span><span class="sxs-lookup"><span data-stu-id="66945-106">Databinding with WPF</span></span>
<span data-ttu-id="66945-107">En este tutorial paso a paso se muestra cómo enlazar tipos POCO a controles WPFWPF en un formulario "maestro-detalle".</span><span class="sxs-lookup"><span data-stu-id="66945-107">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="66945-108">La aplicación usa las API de Entity Framework para rellenar objetos con datos de la base de datos, realizar un seguimiento de los cambios y conservar los datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="66945-108">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="66945-109">El modelo define dos tipos que participan en la\\relación uno a\\varios: **Categoría** (principal maestro) y **Producto** (detalle dependiente).</span><span class="sxs-lookup"><span data-stu-id="66945-109">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="66945-110">A continuación, las herramientas de Visual Studio se usan para enlazar los tipos definidos en el modelo a los controles WPFWPF.</span><span class="sxs-lookup"><span data-stu-id="66945-110">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="66945-111">El marco de enlace de datos WPFWPF permite la navegación entre objetos relacionados: la selección de filas en la vista maestra hace que la vista de detalle se actualice con los datos secundarios correspondientes.</span><span class="sxs-lookup"><span data-stu-id="66945-111">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="66945-112">Las capturas de pantalla y las listas de código de este tutorial se toman de Visual Studio 2013, pero puede completar este tutorial con Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="66945-112">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="66945-113">Use la opción 'Objeto' para crear orígenes de datos WPF</span><span class="sxs-lookup"><span data-stu-id="66945-113">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="66945-114">Con la versión anterior de Entity Framework se usa el uso de la base de **datos** opción al crear un nuevo origen de datos basado en un modelo creado con el Diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="66945-114">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="66945-115">Esto se debió a que el diseñador generaría un contexto derivado de ObjectContext y clases de entidad derivadas de EntityObject.</span><span class="sxs-lookup"><span data-stu-id="66945-115">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="66945-116">El uso de la opción Base de datos le ayudaría a escribir el mejor código para interactuar con esta superficie de API.</span><span class="sxs-lookup"><span data-stu-id="66945-116">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="66945-117">Los diseñadores de EF para Visual Studio 2012 y Visual Studio 2013 generan un contexto que deriva de DbContext junto con clases de entidad POCO simples.</span><span class="sxs-lookup"><span data-stu-id="66945-117">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="66945-118">Con Visual Studio 2010 se recomienda intercambiar a una plantilla de generación de código que usa DbContext como se describe más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="66945-118">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="66945-119">Al usar la superficie de la API de DbContext, debe usar la opción **Object** al crear un nuevo origen de datos, como se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="66945-119">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="66945-120">Si es necesario, puede volver a la generación de [código basada en ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) para los modelos creados con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="66945-120">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="66945-121">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="66945-121">Pre-Requisites</span></span>

<span data-ttu-id="66945-122">Debe tener Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 instalado para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="66945-122">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="66945-123">Si usa Visual Studio 2010, también tiene que instalar NuGet.</span><span class="sxs-lookup"><span data-stu-id="66945-123">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="66945-124">Para obtener más información, vea [Instalar NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="66945-124">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="66945-125">Crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="66945-125">Create the Application</span></span>

-   <span data-ttu-id="66945-126">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66945-126">Open Visual Studio</span></span>
-   <span data-ttu-id="66945-127">**Archivo&gt; -&gt; Nuevo - Proyecto....**</span><span class="sxs-lookup"><span data-stu-id="66945-127">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="66945-128">Seleccione **Windows** en el panel izquierdo y **WPFApplication** en el panel derecho</span><span class="sxs-lookup"><span data-stu-id="66945-128">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="66945-129">Escriba **WPFwithEFSample** como nombre</span><span class="sxs-lookup"><span data-stu-id="66945-129">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="66945-130">Seleccione **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="66945-130">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="66945-131">Instale el paquete NuGet de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="66945-131">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="66945-132">En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="66945-132">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="66945-133">Seleccione **Administrar paquetes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="66945-133">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="66945-134">En el cuadro de diálogo Administrar paquetes NuGet, seleccione la pestaña **Online** y elija el paquete **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="66945-134">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="66945-135">Haga clic en **Instalar**</span><span class="sxs-lookup"><span data-stu-id="66945-135">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="66945-136">Además del ensamblado EntityFramework, también se agrega una referencia a System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="66945-136">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="66945-137">Si el proyecto tiene una referencia a System.Data.Entity, se quitará cuando se instale el paquete EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="66945-137">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="66945-138">El ensamblado System.Data.Entity ya no se utiliza para aplicaciones de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="66945-138">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="66945-139">Definir un modelo</span><span class="sxs-lookup"><span data-stu-id="66945-139">Define a Model</span></span>

<span data-ttu-id="66945-140">En este tutorial puede optar por implementar un modelo mediante Code First o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="66945-140">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="66945-141">Complete una de las dos secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="66945-141">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="66945-142">Opción 1: Definir un modelo utilizando Code First</span><span class="sxs-lookup"><span data-stu-id="66945-142">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="66945-143">En esta sección se muestra cómo crear un modelo y su base de datos asociada mediante Code First.</span><span class="sxs-lookup"><span data-stu-id="66945-143">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="66945-144">Vaya a la siguiente sección **(Opción 2: definir un modelo con Database First)** si prefiere usar Database First para realizar ingeniería inversa del modelo desde una base de datos mediante el diseñador de EF</span><span class="sxs-lookup"><span data-stu-id="66945-144">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="66945-145">Cuando se usa el desarrollo de Code First, normalmente se empieza escribiendo clases de .NET Framework que definen el modelo conceptual (dominio).</span><span class="sxs-lookup"><span data-stu-id="66945-145">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="66945-146">Agregue una nueva clase a **la WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="66945-146">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="66945-147">Haga clic con el botón derecho en el nombre del proyecto</span><span class="sxs-lookup"><span data-stu-id="66945-147">Right-click on the project name</span></span>
    -   <span data-ttu-id="66945-148">Seleccione **Agregar**y, a continuación, **Nuevo elemento**</span><span class="sxs-lookup"><span data-stu-id="66945-148">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="66945-149">Seleccione **Clase** e introduzca **Producto** para el nombre de clase</span><span class="sxs-lookup"><span data-stu-id="66945-149">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="66945-150">Reemplace la definición de clase **Product** por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="66945-150">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="66945-151">El **Products** propiedad en el **Category** clase y **Category** propiedad en el **Product** clase son propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="66945-151">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="66945-152">En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="66945-152">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="66945-153">Además de definir entidades, debe definir una clase que derive de&lt;DbContext y expone las propiedades de DbSet TEntity.&gt;</span><span class="sxs-lookup"><span data-stu-id="66945-153">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="66945-154">Las propiedades&lt;DbSet TEntity&gt; permiten al contexto saber qué tipos desea incluir en el modelo.</span><span class="sxs-lookup"><span data-stu-id="66945-154">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="66945-155">Una instancia del tipo derivado DbContext administra los objetos de entidad durante el tiempo de ejecución, lo que incluye rellenar objetos con datos de una base de datos, realizar el seguimiento de cambios y conservar los datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="66945-155">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="66945-156">Agregue una nueva clase **ProductContext** al proyecto con la siguiente definición:</span><span class="sxs-lookup"><span data-stu-id="66945-156">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="66945-157">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="66945-157">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="66945-158">Opción 2: Definir un modelo utilizando Database First</span><span class="sxs-lookup"><span data-stu-id="66945-158">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="66945-159">En esta sección se muestra cómo usar Database First para realizar ingeniería inversa del modelo desde una base de datos mediante el diseñador de EF.</span><span class="sxs-lookup"><span data-stu-id="66945-159">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="66945-160">Si ha completado la sección anterior (**Opción 1: Definir un modelo mediante Code First),** omita esta sección y vaya directamente a la sección **Carga diferida.**</span><span class="sxs-lookup"><span data-stu-id="66945-160">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="66945-161">Crear una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="66945-161">Create an Existing Database</span></span>

<span data-ttu-id="66945-162">Normalmente, cuando se dirige a una base de datos existente, ya se creará, pero para este tutorial necesitamos crear una base de datos para tener acceso.</span><span class="sxs-lookup"><span data-stu-id="66945-162">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="66945-163">El servidor de base de datos que se instala con Visual Studio es diferente en función de la versión de Visual Studio que haya instalado:</span><span class="sxs-lookup"><span data-stu-id="66945-163">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="66945-164">Si usa Visual Studio 2010, va a crear una base de datos de SQL Express.</span><span class="sxs-lookup"><span data-stu-id="66945-164">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="66945-165">Si usa Visual Studio 2012, va a crear una base de datos [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)</span><span class="sxs-lookup"><span data-stu-id="66945-165">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="66945-166">Vamos a seguir adelante y generar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="66945-166">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="66945-167">**Ver&gt; - Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="66945-167">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="66945-168">Haga clic con el botón derecho en Conexiones de **datos -&gt; Agregar conexión...**</span><span class="sxs-lookup"><span data-stu-id="66945-168">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="66945-169">Si no se ha conectado a una base de datos desde el Explorador de servidores antes de que deba seleccionar Microsoft SQL Server como origen de datos</span><span class="sxs-lookup"><span data-stu-id="66945-169">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Cambiar origen de datos](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="66945-171">Conéctese a LocalDB o SQL Express, dependiendo del que haya instalado, e introduzca **Products** como nombre de la base de datos</span><span class="sxs-lookup"><span data-stu-id="66945-171">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Agregar conexión LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Añadir conexión exprés](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="66945-174">Seleccione **Aceptar** y se le preguntará si desea crear una nueva base de datos, seleccione **Sí**</span><span class="sxs-lookup"><span data-stu-id="66945-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Creación de una base de datos](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="66945-176">La nueva base de datos aparecerá ahora en el Explorador de servidores, haga clic con el botón derecho en ella y seleccione **Nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="66945-176">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="66945-177">Copie el siguiente SQL en la nueva consulta, haga clic con el botón derecho en la consulta y seleccione **Ejecutar**</span><span class="sxs-lookup"><span data-stu-id="66945-177">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="66945-178">Modelo de ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="66945-178">Reverse Engineer Model</span></span>

<span data-ttu-id="66945-179">Vamos a hacer uso de Entity Framework Designer, que se incluye como parte de Visual Studio, para crear nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="66945-179">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="66945-180">**Proyecto&gt; - Añadir nuevo elemento...**</span><span class="sxs-lookup"><span data-stu-id="66945-180">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="66945-181">Seleccione **Datos** en el menú de la izquierda y, a continuación, **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="66945-181">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="66945-182">Escriba **ProductModel** como nombre y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="66945-182">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="66945-183">Esto inicia el **Asistente para Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="66945-183">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="66945-184">Seleccione **Generar a partir de la base de datos** y haga clic en **Siguiente**</span><span class="sxs-lookup"><span data-stu-id="66945-184">Select **Generate from Database** and click **Next**</span></span>

    ![Elegir contenido de Model](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="66945-186">Seleccione la conexión a la base de datos que creó en la primera sección, escriba **ProductContext** como nombre de la cadena de conexión y haga clic en **Siguiente**</span><span class="sxs-lookup"><span data-stu-id="66945-186">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Elija su conexión](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="66945-188">Haga clic en la casilla de verificación situada junto a 'Tablas' para importar todas las tablas y haga clic en 'Finalizar'</span><span class="sxs-lookup"><span data-stu-id="66945-188">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Elija sus objetos](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="66945-190">Una vez completado el proceso de ingeniería inversa, el nuevo modelo se agrega al proyecto y se abre para que lo vea en Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="66945-190">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="66945-191">También se ha agregado un archivo App.config al proyecto con los detalles de conexión de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="66945-191">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="66945-192">Pasos adicionales en Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="66945-192">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="66945-193">Si está trabajando en Visual Studio 2010, deberá actualizar el diseñador de EF para usar la generación de código EF6.</span><span class="sxs-lookup"><span data-stu-id="66945-193">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="66945-194">Haga clic con el botón derecho en un punto vacío del modelo en EF Designer y seleccione Agregar elemento de generación de **código...**</span><span class="sxs-lookup"><span data-stu-id="66945-194">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="66945-195">Seleccione **Plantillas en línea** en el menú de la izquierda y busque **DbContext**</span><span class="sxs-lookup"><span data-stu-id="66945-195">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="66945-196">Seleccione **EF 6.x DbContext\#Generator para C ,** escriba **ProductsModel** como nombre y haga clic en Agregar</span><span class="sxs-lookup"><span data-stu-id="66945-196">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="66945-197">Actualización de la generación de código para el enlace de datos</span><span class="sxs-lookup"><span data-stu-id="66945-197">Updating code generation for data binding</span></span>

<span data-ttu-id="66945-198">EF genera código a partir del modelo mediante plantillas T4.</span><span class="sxs-lookup"><span data-stu-id="66945-198">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="66945-199">Las plantillas incluidas con Visual Studio o descargadas de la galería de Visual Studio están diseñadas para uso general.</span><span class="sxs-lookup"><span data-stu-id="66945-199">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="66945-200">Esto significa que las entidades generadas a&lt;&gt; partir de estas plantillas tienen propiedades ICollection T simples.</span><span class="sxs-lookup"><span data-stu-id="66945-200">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="66945-201">Sin embargo, al realizar el enlace de datos mediante WPFWPF es deseable usar **ObservableCollection** para las propiedades de colección para que WPFWPF pueda realizar un seguimiento de los cambios realizados en las colecciones.</span><span class="sxs-lookup"><span data-stu-id="66945-201">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="66945-202">Para ello vamos a modificar las plantillas para utilizar ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="66945-202">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="66945-203">Abra el **Explorador de soluciones** y busque el archivo **ProductModel.edmx**</span><span class="sxs-lookup"><span data-stu-id="66945-203">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="66945-204">Busque el archivo **ProductModel.tt** que se anidará en el archivo ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="66945-204">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Plantilla de modelo de producto WPF](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="66945-206">Haga doble clic en el archivo ProductModel.tt para abrirlo en el editor de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66945-206">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="66945-207">Busque y reemplace las dos apariciones de "**ICollection"** por "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="66945-207">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="66945-208">Estos se encuentran aproximadamente en las líneas 296 y 484.</span><span class="sxs-lookup"><span data-stu-id="66945-208">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="66945-209">Busque y reemplace la primera aparición de "**HashSet**" por "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="66945-209">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="66945-210">Esta ocurrencia se encuentra aproximadamente en la línea 50.</span><span class="sxs-lookup"><span data-stu-id="66945-210">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="66945-211">**No** reemplace la segunda aparición de HashSet que se encuentra más adelante en el código.</span><span class="sxs-lookup"><span data-stu-id="66945-211">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="66945-212">Busque y reemplace la única aparición de "**System.Collections.Generic**" por "**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="66945-212">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="66945-213">Se encuentra aproximadamente en la línea 424.</span><span class="sxs-lookup"><span data-stu-id="66945-213">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="66945-214">Guarde el archivo ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="66945-214">Save the ProductModel.tt file.</span></span> <span data-ttu-id="66945-215">Esto debería hacer que se regenere el código de las entidades.</span><span class="sxs-lookup"><span data-stu-id="66945-215">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="66945-216">Si el código no se regenera automáticamente, haga clic con el botón derecho en ProductModel.tt y elija "Ejecutar herramienta personalizada".</span><span class="sxs-lookup"><span data-stu-id="66945-216">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="66945-217">Si ahora abre el archivo de Category.cs (que está anidado en ProductModel.tt), debería ver que la colección Products tiene el tipo **ObservableCollection&lt;Product&gt;**.</span><span class="sxs-lookup"><span data-stu-id="66945-217">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="66945-218">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="66945-218">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="66945-219">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="66945-219">Lazy Loading</span></span>

<span data-ttu-id="66945-220">El **Products** propiedad en el **Category** clase y **Category** propiedad en el **Product** clase son propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="66945-220">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="66945-221">En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="66945-221">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="66945-222">EF le ofrece la opción de cargar entidades relacionadas desde la base de datos automáticamente la primera vez que tenga acceso a la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="66945-222">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="66945-223">Con este tipo de carga (denominada carga diferida), tenga en cuenta que la primera vez que acceda a cada propiedad de navegación se ejecutará una consulta independiente en la base de datos si el contenido no está ya en el contexto.</span><span class="sxs-lookup"><span data-stu-id="66945-223">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="66945-224">Cuando se usan tipos de entidad POCO, EF logra la carga diferida mediante la creación de instancias de tipos de proxy derivados durante el tiempo de ejecución y, a continuación, invalidar las propiedades virtuales en las clases para agregar el enlace de carga.</span><span class="sxs-lookup"><span data-stu-id="66945-224">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="66945-225">Para obtener la carga diferida de objetos relacionados, debe declarar los captadores de propiedades de navegación como **públicos** y **virtuales** **(Overridable** en Visual Basic) y la clase no debe estar **sellada** **(NotOverridable** en Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="66945-225">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="66945-226">Cuando se utilizan las propiedades de navegación de Database First se hacen virtuales automáticamente para habilitar la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="66945-226">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="66945-227">En la sección Code First elegimos hacer que las propiedades de navegación sean virtuales por la misma razón</span><span class="sxs-lookup"><span data-stu-id="66945-227">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="66945-228">Enlazar objeto a controles</span><span class="sxs-lookup"><span data-stu-id="66945-228">Bind Object to Controls</span></span>

<span data-ttu-id="66945-229">Agregue las clases que se definen en el modelo como orígenes de datos para esta aplicación WPFWPF.</span><span class="sxs-lookup"><span data-stu-id="66945-229">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="66945-230">Haga doble clic en **MainWindow.xaml** en el Explorador de soluciones para abrir el formulario principal</span><span class="sxs-lookup"><span data-stu-id="66945-230">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="66945-231">En el menú principal, seleccione **Proyecto -&gt; Agregar nuevo origen de datos ...**</span><span class="sxs-lookup"><span data-stu-id="66945-231">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="66945-232">(en Visual Studio 2010, debe seleccionar **Datos -&gt; Agregar nuevo origen**de datos... )</span><span class="sxs-lookup"><span data-stu-id="66945-232">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="66945-233">En la ventana Elegir un tipo de origen de datos, seleccione **Objeto** y haga clic en **Siguiente**</span><span class="sxs-lookup"><span data-stu-id="66945-233">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="66945-234">En el cuadro de diálogo Seleccionar los objetos de datos, desdoble el **ARCHIVO WPFwithEFSample** dos veces y seleccione **Categoría**</span><span class="sxs-lookup"><span data-stu-id="66945-234">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="66945-235">*No es necesario seleccionar el origen de datos **Del producto,** porque llegaremos a él a través de la propiedad **Producto**en el origen de datos **Categoría***</span><span class="sxs-lookup"><span data-stu-id="66945-235">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Seleccionar objetos de datos](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="66945-237">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="66945-237">Click **Finish.**</span></span>
-   <span data-ttu-id="66945-238">La ventana Orígenes de datos se abre junto a la ventana MainWindow.xaml Si la ventana Orígenes de \*datos no se muestra, seleccione Ver **-&gt; Otros orígenes de&gt; datos** \* de Windows</span><span class="sxs-lookup"><span data-stu-id="66945-238">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="66945-239">Pulse el icono de pasador para que la ventana Orígenes de datos no se oculte automáticamente.</span><span class="sxs-lookup"><span data-stu-id="66945-239">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="66945-240">Es posible que deba pulsar el botón de actualización si la ventana ya estaba visible.</span><span class="sxs-lookup"><span data-stu-id="66945-240">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Orígenes de datos](~/ef6/media/datasources.png)

-   <span data-ttu-id="66945-242">Seleccione el origen de datos **Categoría** y arrástrelo al formulario.</span><span class="sxs-lookup"><span data-stu-id="66945-242">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="66945-243">Lo siguiente ocurrió cuando arrastramos esta fuente:</span><span class="sxs-lookup"><span data-stu-id="66945-243">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="66945-244">El recurso **categoryViewSource** y el control **categoryDataGrid** se agregaron a XAML</span><span class="sxs-lookup"><span data-stu-id="66945-244">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="66945-245">El DataContext propiedad en el elemento Grid primario se estableció en "'StaticResource **categoryViewSource** '".</span><span class="sxs-lookup"><span data-stu-id="66945-245">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="66945-246">El **recurso categoryViewSource** sirve como origen\\de enlace para el elemento Grid primario externo.</span><span class="sxs-lookup"><span data-stu-id="66945-246"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="66945-247">A continuación, los elementos Grid internos heredan el valor DataContext de la cuadrícula primaria (la propiedad ItemsSource de categoryDataGrid se establece en "-Binding-")</span><span class="sxs-lookup"><span data-stu-id="66945-247">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="66945-248">Adición de una cuadrícula de detalles</span><span class="sxs-lookup"><span data-stu-id="66945-248">Adding a Details Grid</span></span>

<span data-ttu-id="66945-249">Ahora que tenemos una cuadrícula para mostrar Categorías vamos a agregar una cuadrícula de detalles para mostrar los Productos asociados.</span><span class="sxs-lookup"><span data-stu-id="66945-249">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="66945-250">Seleccione la propiedad **Products** en el origen de datos **Category** y arrástrela al formulario.</span><span class="sxs-lookup"><span data-stu-id="66945-250">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="66945-251">El recurso **categoryProductsViewSource** y la cuadrícula **productDataGrid** se agregan a XAML</span><span class="sxs-lookup"><span data-stu-id="66945-251">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="66945-252">La ruta de enlace para este recurso se establece en Products</span><span class="sxs-lookup"><span data-stu-id="66945-252">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="66945-253">WPFWPF marco de enlace de datos garantiza que sólo los productos relacionados con la categoría seleccionada se muestran en **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="66945-253">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="66945-254">Desde el cuadro de herramientas, arrastre **Button** al formulario.</span><span class="sxs-lookup"><span data-stu-id="66945-254">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="66945-255">Establezca la propiedad **Name** en **buttonSave** y la propiedad **Content** en **Save**.</span><span class="sxs-lookup"><span data-stu-id="66945-255">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="66945-256">El formulario debe ser similar a esto:</span><span class="sxs-lookup"><span data-stu-id="66945-256">The form should look similar to this:</span></span>

![Diseñador](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="66945-258">Agregar código que controla la interacción de datos</span><span class="sxs-lookup"><span data-stu-id="66945-258">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="66945-259">Es hora de agregar algunos controladores de eventos a la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="66945-259">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="66945-260">En la ventana XAML, \*\* &lt;\*\* haga clic en el elemento Window, esto selecciona la ventana principal</span><span class="sxs-lookup"><span data-stu-id="66945-260">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="66945-261">En la ventana **Propiedades** elija **Eventos** en la parte superior derecha y, a continuación, haga doble clic en el cuadro de texto situado a la derecha de la etiqueta **Cargado**</span><span class="sxs-lookup"><span data-stu-id="66945-261">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Propiedades de la ventana principal](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="66945-263">Agregue también el evento **Click** para el botón **Guardar** haciendo doble clic en el botón Guardar del diseñador.</span><span class="sxs-lookup"><span data-stu-id="66945-263">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="66945-264">Esto le lleva al código subyacente para el formulario, ahora vamos a editar el código para usar el ProductContext para realizar el acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="66945-264">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="66945-265">Actualice el código de MainWindow como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="66945-265">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="66945-266">El código declara una instancia de ejecución prolongada de **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="66945-266">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="66945-267">El objeto **ProductContext** se utiliza para consultar y guardar datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="66945-267">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="66945-268">A continuación, se llama a **Dispose()** en la instancia **de ProductContext** desde el método **OnClosing** invalidado.</span><span class="sxs-lookup"><span data-stu-id="66945-268">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="66945-269">Los comentarios de código proporcionan detalles sobre lo que hace el código.</span><span class="sxs-lookup"><span data-stu-id="66945-269"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="66945-270">Pruebe la aplicación WPF</span><span class="sxs-lookup"><span data-stu-id="66945-270">Test the WPF Application</span></span>

-   <span data-ttu-id="66945-271">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="66945-271">Compile and run the application.</span></span> <span data-ttu-id="66945-272">Si usó Code First, verá que se crea una base de datos **WPFwithEFSample.ProductContext** automáticamente.</span><span class="sxs-lookup"><span data-stu-id="66945-272">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="66945-273">Escriba un nombre de categoría en la cuadrícula superior y los nombres de productos en la cuadrícula inferior No escriba nada en las *columnas de ID, porque la clave principal la genera la base* de datos</span><span class="sxs-lookup"><span data-stu-id="66945-273">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Ventana principal con nuevas categorías y productos](~/ef6/media/screen1.png)

-   <span data-ttu-id="66945-275">Pulse el botón **Guardar** para guardar los datos en la base de datos</span><span class="sxs-lookup"><span data-stu-id="66945-275">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="66945-276">Después de la llamada a **SaveChanges()** de DbContext, los identificadores se rellenan con los valores generados por la base de datos.</span><span class="sxs-lookup"><span data-stu-id="66945-276">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="66945-277">Porque llamamos **a Refresh()** después de **SaveChanges()** los controles **DataGrid** también se actualizan con los nuevos valores.</span><span class="sxs-lookup"><span data-stu-id="66945-277">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Ventana principal con iDE rellenados](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="66945-279">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="66945-279">Additional Resources</span></span>

<span data-ttu-id="66945-280">Para obtener más información sobre el enlace de datos a colecciones mediante WPFWPF, vea [este tema](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) en la documentación de WPFWPF.</span><span class="sxs-lookup"><span data-stu-id="66945-280">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
