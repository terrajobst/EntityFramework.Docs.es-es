---
title: Enlace de datos con WinForms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 071172810f7dac45f42aca0efa7f329bac31e9cd
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251198"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="91c8b-102">Enlace de datos con formularios Windows Forms</span><span class="sxs-lookup"><span data-stu-id="91c8b-102">Databinding with WinForms</span></span>
<span data-ttu-id="91c8b-103">En este tutorial paso a paso muestra cómo enlazar tipos POCO a controles de Windows Forms (WinForms) en forma de "master-detail".</span><span class="sxs-lookup"><span data-stu-id="91c8b-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="91c8b-104">La aplicación usa Entity Framework para rellenar los objetos con los datos de la base de datos, realizar un seguimiento de cambios y conservar los datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="91c8b-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="91c8b-105">El modelo define dos tipos que participan en la relación de uno a varios: categoría (entidad de seguridad\\maestra) y el producto (dependientes\\detalle).</span><span class="sxs-lookup"><span data-stu-id="91c8b-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="91c8b-106">A continuación, se usan las herramientas de Visual Studio para enlazar los tipos definidos en el modelo a los controles de WinForms.</span><span class="sxs-lookup"><span data-stu-id="91c8b-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="91c8b-107">El marco de enlace de datos de formularios Windows Forms permite la navegación entre los objetos relacionados: selección de filas en la vista principal hace que la vista de detalle para actualizar con los datos secundarios correspondientes.</span><span class="sxs-lookup"><span data-stu-id="91c8b-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="91c8b-108">Las capturas de pantalla y los listados de código de este tutorial se toman de Visual Studio 2013, pero se puede completar este tutorial con Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="91c8b-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="91c8b-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="91c8b-109">Pre-Requisites</span></span>

<span data-ttu-id="91c8b-110">Debe tener Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 instalado para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="91c8b-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="91c8b-111">Si utiliza Visual Studio 2010, también debe instalar NuGet.</span><span class="sxs-lookup"><span data-stu-id="91c8b-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="91c8b-112">Para obtener más información, consulte [instalar NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="91c8b-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="91c8b-113">Crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="91c8b-113">Create the Application</span></span>

-   <span data-ttu-id="91c8b-114">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91c8b-114">Open Visual Studio</span></span>
-   <span data-ttu-id="91c8b-115">**Archivo -&gt; nuevo:&gt; proyecto...**</span><span class="sxs-lookup"><span data-stu-id="91c8b-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="91c8b-116">Seleccione **Windows** en el panel izquierdo y **FormsApplication Windows** en el panel derecho</span><span class="sxs-lookup"><span data-stu-id="91c8b-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="91c8b-117">Escriba **WinFormswithEFSample** como el nombre</span><span class="sxs-lookup"><span data-stu-id="91c8b-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="91c8b-118">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="91c8b-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="91c8b-119">Instale el paquete NuGet de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="91c8b-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="91c8b-120">En el Explorador de soluciones, haga doble clic en el **WinFormswithEFSample** proyecto</span><span class="sxs-lookup"><span data-stu-id="91c8b-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="91c8b-121">Seleccione **administrar paquetes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="91c8b-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="91c8b-122">En el cuadro de diálogo Administrar paquetes de NuGet, seleccione el **Online** pestaña y elija el **EntityFramework** paquete</span><span class="sxs-lookup"><span data-stu-id="91c8b-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="91c8b-123">Haga clic en **instalar**</span><span class="sxs-lookup"><span data-stu-id="91c8b-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="91c8b-124">Además del ensamblado de Entity Framework también se agrega una referencia a System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="91c8b-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="91c8b-125">Si el proyecto tiene una referencia a System.Data.Entity, a continuación, se eliminará cuando se instala el paquete de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="91c8b-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="91c8b-126">El ensamblado System.Data.Entity ya no se usa para las aplicaciones de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="91c8b-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="91c8b-127">Implementación de IListSource para colecciones</span><span class="sxs-lookup"><span data-stu-id="91c8b-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="91c8b-128">Propiedades de la colección deben implementar la interfaz IListSource para habilitar el enlace de datos bidireccional con la ordenación cuando se usa Windows Forms.</span><span class="sxs-lookup"><span data-stu-id="91c8b-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="91c8b-129">Para ello, vamos a ampliar ObservableCollection para agregar la funcionalidad de IListSource.</span><span class="sxs-lookup"><span data-stu-id="91c8b-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="91c8b-130">Agregar un **ObservableListSource** clase al proyecto:</span><span class="sxs-lookup"><span data-stu-id="91c8b-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="91c8b-131">Haga doble clic en el nombre del proyecto</span><span class="sxs-lookup"><span data-stu-id="91c8b-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="91c8b-132">Seleccione **Add -&gt; nuevo elemento**</span><span class="sxs-lookup"><span data-stu-id="91c8b-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="91c8b-133">Seleccione **clase** y escriba **ObservableListSource** para el nombre de clase</span><span class="sxs-lookup"><span data-stu-id="91c8b-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="91c8b-134">Reemplace el código generado de forma predeterminada con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="91c8b-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="91c8b-135">*Esta clase permite datos bidireccional de enlace así como la ordenación. La clase se deriva de ObservableCollection&lt;T&gt; y agrega una implementación explícita de IListSource. El método GetList() de IListSource se implementa para devolver una implementación de IBindingList que permanece sincronizada con ObservableCollection. La implementación de IBindingList generada por ToBindingList admite la ordenación. El método de extensión ToBindingList se define en el ensamblado de Entity Framework.*</span><span class="sxs-lookup"><span data-stu-id="91c8b-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extention method is defined in the EntityFramework assembly.*</span></span>

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList  (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="91c8b-136">Definir un modelo</span><span class="sxs-lookup"><span data-stu-id="91c8b-136">Define a Model</span></span>

<span data-ttu-id="91c8b-137">En este tutorial, puede decidió implementar un modelo con Code First o EF Designer.</span><span class="sxs-lookup"><span data-stu-id="91c8b-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="91c8b-138">Realice una de las dos secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="91c8b-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="91c8b-139">Opción 1: Definir un modelo con Code First</span><span class="sxs-lookup"><span data-stu-id="91c8b-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="91c8b-140">En esta sección se muestra cómo crear un modelo y su base de datos asociada mediante Code First.</span><span class="sxs-lookup"><span data-stu-id="91c8b-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="91c8b-141">Saltar a la siguiente sección (**opción 2: definir un modelo con Database First)** si prefiere usar Database First para invertir diseñar el modelo desde una base de datos mediante el diseñador EF</span><span class="sxs-lookup"><span data-stu-id="91c8b-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="91c8b-142">Cuando usando desarrollo Code First suele comenzar mediante la escritura de las clases de .NET Framework que definen el modelo conceptual (dominio).</span><span class="sxs-lookup"><span data-stu-id="91c8b-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="91c8b-143">Agregue un nuevo **producto** clase al proyecto</span><span class="sxs-lookup"><span data-stu-id="91c8b-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="91c8b-144">Reemplace el código generado de forma predeterminada con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="91c8b-144">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   <span data-ttu-id="91c8b-145">Agregar un **categoría** clase al proyecto.</span><span class="sxs-lookup"><span data-stu-id="91c8b-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="91c8b-146">Reemplace el código generado de forma predeterminada con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="91c8b-146">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

<span data-ttu-id="91c8b-147">Además de definir las entidades, debe definir una clase que derive de **DbContext** y expone **DbSet&lt;TEntity&gt;**  propiedades.</span><span class="sxs-lookup"><span data-stu-id="91c8b-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="91c8b-148">El **DbSet** propiedades que el contexto sepan qué tipos van a incluir en el modelo.</span><span class="sxs-lookup"><span data-stu-id="91c8b-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="91c8b-149">El **DbContext** y **DbSet** tipos se definen en el ensamblado de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="91c8b-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="91c8b-150">Una instancia del tipo derivado de DbContext administra los objetos entidad durante el tiempo de ejecución, que incluye rellenar objetos con los datos de una base de datos, cambie el seguimiento y almacenar datos a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="91c8b-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="91c8b-151">Agregue un nuevo **ProductContext** clase al proyecto.</span><span class="sxs-lookup"><span data-stu-id="91c8b-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="91c8b-152">Reemplace el código generado de forma predeterminada con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="91c8b-152">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="91c8b-153">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="91c8b-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="91c8b-154">Opción 2: Definir un modelo con Database First</span><span class="sxs-lookup"><span data-stu-id="91c8b-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="91c8b-155">Esta sección muestra cómo usar la primera base de datos para aplicar ingeniería inversa a su modelo de una base de datos con EF designer.</span><span class="sxs-lookup"><span data-stu-id="91c8b-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="91c8b-156">Si ha completado la sección anterior (**opción 1: definir un modelo con Code First)**, omita esta sección e ir directamente a la **la carga diferida** sección.</span><span class="sxs-lookup"><span data-stu-id="91c8b-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="91c8b-157">Crear una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="91c8b-157">Create an Existing Database</span></span>

<span data-ttu-id="91c8b-158">Normalmente, cuando tiene como destino una base de datos existente que ya se creará, pero para este tutorial es necesario crear una base de datos para tener acceso a.</span><span class="sxs-lookup"><span data-stu-id="91c8b-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="91c8b-159">El servidor de base de datos que se instala con Visual Studio es diferente según la versión de Visual Studio que ha instalado:</span><span class="sxs-lookup"><span data-stu-id="91c8b-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="91c8b-160">Si usa Visual Studio 2010 se creará una base de datos de SQL Express.</span><span class="sxs-lookup"><span data-stu-id="91c8b-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="91c8b-161">Si usa Visual Studio 2012, a continuación, se creará un [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) base de datos.</span><span class="sxs-lookup"><span data-stu-id="91c8b-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="91c8b-162">Sigamos adelante y generar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="91c8b-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="91c8b-163">**Vista -&gt; Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="91c8b-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="91c8b-164">Haga clic con el botón derecho en **conexiones de datos -&gt; Agregar conexión...**</span><span class="sxs-lookup"><span data-stu-id="91c8b-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="91c8b-165">Si aún no lo ha conectado a una base de datos del explorador de servidores antes de que tendrá que seleccionar Microsoft SQL Server como origen de datos</span><span class="sxs-lookup"><span data-stu-id="91c8b-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Cambiar origen de datos](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="91c8b-167">Conectarse a LocalDB o Express de SQL, según lo que se ha instalado y escriba **productos** como el nombre de la base de datos</span><span class="sxs-lookup"><span data-stu-id="91c8b-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Agregar conexión LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Agregar conexión rápida](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="91c8b-170">Seleccione **Aceptar** y se le solicitará si desea crear una nueva base de datos, seleccione **sí**</span><span class="sxs-lookup"><span data-stu-id="91c8b-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Crear base de datos](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="91c8b-172">La nueva base de datos aparecerá ahora en el Explorador de servidores, haga doble clic en él y seleccione **nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="91c8b-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="91c8b-173">Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **Execute**</span><span class="sxs-lookup"><span data-stu-id="91c8b-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="91c8b-174">Modelo de ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="91c8b-174">Reverse Engineer Model</span></span>

<span data-ttu-id="91c8b-175">Vamos a usar Entity Framework Designer, que se incluye como parte de Visual Studio, para crear nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="91c8b-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="91c8b-176">**Proyecto -&gt; Agregar nuevo elemento...**</span><span class="sxs-lookup"><span data-stu-id="91c8b-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="91c8b-177">Seleccione **datos** en el menú izquierdo y, a continuación, **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="91c8b-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="91c8b-178">Escriba **ProductModel** como el nombre y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="91c8b-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="91c8b-179">Esto inicia el **Asistente para Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="91c8b-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="91c8b-180">Seleccione **generar desde la base de datos** y haga clic en **siguiente**</span><span class="sxs-lookup"><span data-stu-id="91c8b-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="91c8b-182">Seleccione la conexión a la base de datos que creó en la primera sección, escriba **ProductContext** como el nombre de la cadena de conexión y haga clic en **siguiente**</span><span class="sxs-lookup"><span data-stu-id="91c8b-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Elegir la conexión](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="91c8b-184">Haga clic en la casilla de verificación situada junto a 'Tablas' para importar todas las tablas y haga clic en 'Finalizar'</span><span class="sxs-lookup"><span data-stu-id="91c8b-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Elija los objetos](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="91c8b-186">Una vez completado el proceso de ingeniería inversa el nuevo modelo se agrega al proyecto y se abrirá para que pueda ver en el Diseñador de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="91c8b-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="91c8b-187">También se agregó un archivo App.config al proyecto con los detalles de conexión para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="91c8b-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="91c8b-188">Pasos adicionales en Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="91c8b-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="91c8b-189">Si está trabajando en Visual Studio 2010, a continuación, deberá actualizar el diseñador EF para usar la generación de código de EF6.</span><span class="sxs-lookup"><span data-stu-id="91c8b-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="91c8b-190">Haga doble clic en una zona vacía del modelo de EF Designer y seleccione **Add Code Generation Item...**</span><span class="sxs-lookup"><span data-stu-id="91c8b-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="91c8b-191">Seleccione **plantillas en línea** desde el menú izquierdo y busque **DbContext**</span><span class="sxs-lookup"><span data-stu-id="91c8b-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="91c8b-192">Seleccione el **EF 6.x generador de DbContext para C\#,** escriba **ProductsModel** como el nombre y haga clic en Agregar</span><span class="sxs-lookup"><span data-stu-id="91c8b-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="91c8b-193">Actualizando la generación de código para el enlace de datos</span><span class="sxs-lookup"><span data-stu-id="91c8b-193">Updating code generation for data binding</span></span>

<span data-ttu-id="91c8b-194">EF genera código desde el modelo usando plantillas T4.</span><span class="sxs-lookup"><span data-stu-id="91c8b-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="91c8b-195">Las plantillas se incluye con Visual Studio o se descarga desde la Galería de Visual Studio están diseñadas para su uso de propósito general.</span><span class="sxs-lookup"><span data-stu-id="91c8b-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="91c8b-196">Esto significa que las entidades generadas a partir de estas plantillas tienen ICollection simple&lt;T&gt; propiedades.</span><span class="sxs-lookup"><span data-stu-id="91c8b-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="91c8b-197">Sin embargo, al realizar el enlace de datos es recomendable tener las propiedades de colección que implementan IListSource.</span><span class="sxs-lookup"><span data-stu-id="91c8b-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="91c8b-198">Por eso hemos creado la clase ObservableListSource anterior y ahora vamos a modificar las plantillas para hacer uso de esta clase.</span><span class="sxs-lookup"><span data-stu-id="91c8b-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="91c8b-199">Abra el **el Explorador de soluciones** y encontrar **ProductModel.edmx** archivo</span><span class="sxs-lookup"><span data-stu-id="91c8b-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="91c8b-200">Buscar el **ProductModel.tt** archivo que se anidará en el archivo ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="91c8b-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Plantilla de modelo de producto](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="91c8b-202">Haga doble clic en el archivo ProductModel.tt para abrirlo en el editor de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="91c8b-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="91c8b-203">Buscar y reemplazar las dos repeticiones de "**ICollection**"con"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="91c8b-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="91c8b-204">Estos se encuentran en aproximadamente líneas 296 y 484.</span><span class="sxs-lookup"><span data-stu-id="91c8b-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="91c8b-205">Buscar y reemplazar la primera aparición de "**HashSet**"con"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="91c8b-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="91c8b-206">Esta falta de disponibilidad se encuentra en línea aproximadamente 50.</span><span class="sxs-lookup"><span data-stu-id="91c8b-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="91c8b-207">**No** reemplazar la segunda aparición de HashSet encuentra más adelante en el código.</span><span class="sxs-lookup"><span data-stu-id="91c8b-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="91c8b-208">Guarde el archivo ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="91c8b-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="91c8b-209">Esto debería producir el código para las entidades a generarse.</span><span class="sxs-lookup"><span data-stu-id="91c8b-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="91c8b-210">Si el código no se pueden regenerar automáticamente, a continuación, haga clic con el botón derecho en ProductModel.tt y elija "Ejecutar herramienta personalizada".</span><span class="sxs-lookup"><span data-stu-id="91c8b-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="91c8b-211">Si ahora abra el archivo Category.cs (que está anidado bajo ProductModel.tt), a continuación, debería ver que la colección de productos tiene el tipo **ObservableListSource&lt;producto&gt;**.</span><span class="sxs-lookup"><span data-stu-id="91c8b-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="91c8b-212">Compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="91c8b-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="91c8b-213">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="91c8b-213">Lazy Loading</span></span>

<span data-ttu-id="91c8b-214">El **productos** propiedad en el **categoría** clase y **categoría** propiedad en el **producto** clase son las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="91c8b-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="91c8b-215">En Entity Framework, las propiedades de navegación proporcionan una manera de navegar por una relación entre dos tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="91c8b-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="91c8b-216">EF ofrece una opción de cargar entidades relacionadas de la base de datos automáticamente la primera vez que se tener acceso a la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="91c8b-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="91c8b-217">Con este tipo de carga (denominada la carga diferida), tenga en cuenta que la primera vez que se tiene acceso a cada propiedad de navegación una consulta independiente se ejecutará en la base de datos si el contenido no está en el contexto.</span><span class="sxs-lookup"><span data-stu-id="91c8b-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="91c8b-218">Al utilizar tipos de entidad POCO, EF logra la carga diferida al crear instancias de tipos de proxy derivado en tiempo de ejecución y, a continuación, reemplazar propiedades virtuales en las clases para agregar el enlace de carga.</span><span class="sxs-lookup"><span data-stu-id="91c8b-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="91c8b-219">Para obtener la carga diferida de objetos relacionados, debe declarar navegación captadores de propiedades como **pública** y **virtual** (**Overridable** en Visual Basic), y la clase no debe ser **sealed** (**NotOverridable** en Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="91c8b-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="91c8b-220">Cuando la base de datos utilizando las propiedades de navegación primera se realizan automáticamente virtuales para habilitar la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="91c8b-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="91c8b-221">En la sección Code First hemos optado por hacer que las propiedades de navegación virtual por la misma razón</span><span class="sxs-lookup"><span data-stu-id="91c8b-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="91c8b-222">Enlazar objetos a controles</span><span class="sxs-lookup"><span data-stu-id="91c8b-222">Bind Object to Controls</span></span>

<span data-ttu-id="91c8b-223">Agregue las clases que se definen en el modelo como orígenes de datos para esta aplicación de WinForms.</span><span class="sxs-lookup"><span data-stu-id="91c8b-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="91c8b-224">En el menú principal, seleccione **proyecto -&gt; Agregar nuevo origen de datos...**</span><span class="sxs-lookup"><span data-stu-id="91c8b-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="91c8b-225">(en Visual Studio 2010, deberá seleccionar **datos -&gt; Agregar nuevo origen de datos...** )</span><span class="sxs-lookup"><span data-stu-id="91c8b-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="91c8b-226">En la ventana de un tipo de origen de datos elija, seleccione **objeto** y haga clic en **siguiente**</span><span class="sxs-lookup"><span data-stu-id="91c8b-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="91c8b-227">En el cuadro de diálogo de objetos de datos, seleccione expandir la **WinFormswithEFSample** dos veces y seleccione **categoría** no es necesario para seleccionar el origen de datos del producto, porque se producirá a él a través del producto propiedad en el origen de datos de categoría.</span><span class="sxs-lookup"><span data-stu-id="91c8b-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![Origen de datos](~/ef6/media/datasource.png)

-   <span data-ttu-id="91c8b-229">Haga clic en **Finalizar.** 
     *Si la ventana de orígenes de datos no aparece, seleccione \*\*\* Vista -&gt; otras Windows -&gt; orígenes de datos*\*</span><span class="sxs-lookup"><span data-stu-id="91c8b-229">Click **Finish.**
*If the Data Sources window is not showing up, select\*\*\*View -&gt; Other Windows-&gt; Data Sources*\*</span></span>
-   <span data-ttu-id="91c8b-230">Presione el icono de anclaje, por lo que la ventana de orígenes de datos no auto ocultar.</span><span class="sxs-lookup"><span data-stu-id="91c8b-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="91c8b-231">Deberá presionar el botón de actualización si la ventana que ya estaba visible.</span><span class="sxs-lookup"><span data-stu-id="91c8b-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Origen de datos 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="91c8b-233">En el Explorador de soluciones, haga doble clic en el **Form1.cs** archivo para abrir el formulario principal en el diseñador.</span><span class="sxs-lookup"><span data-stu-id="91c8b-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="91c8b-234">Seleccione el **categoría** origen de datos y arrastrarlo en el formulario.</span><span class="sxs-lookup"><span data-stu-id="91c8b-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="91c8b-235">De forma predeterminada, una DataGridView nuevo (**categoryDataGridView**) y controles de barra de herramientas de exploración se agregan al diseñador.</span><span class="sxs-lookup"><span data-stu-id="91c8b-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="91c8b-236">Estos controles se enlazan a BindingSource (**categoryBindingSource**) y enlace Navigator (**categoryBindingNavigator**) los componentes que se crean así.</span><span class="sxs-lookup"><span data-stu-id="91c8b-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="91c8b-237">Editar las columnas en el **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="91c8b-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="91c8b-238">Queremos establecer la **CategoryId** columna como de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="91c8b-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="91c8b-239">El valor de la **CategoryId** propiedad es generada por la base de datos después de que guardemos los datos.</span><span class="sxs-lookup"><span data-stu-id="91c8b-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="91c8b-240">Haga clic en el control DataGridView de formularios y seleccione Editar columnas...</span><span class="sxs-lookup"><span data-stu-id="91c8b-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="91c8b-241">Seleccione la columna CategoryId y establece ReadOnly en True</span><span class="sxs-lookup"><span data-stu-id="91c8b-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="91c8b-242">Haga clic en Aceptar</span><span class="sxs-lookup"><span data-stu-id="91c8b-242">Press OK</span></span>
-   <span data-ttu-id="91c8b-243">Seleccione los productos desde el origen de datos de categoría y arrastrarlo en el formulario.</span><span class="sxs-lookup"><span data-stu-id="91c8b-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="91c8b-244">El productDataGridView y productBindingSource se agregan al formulario.</span><span class="sxs-lookup"><span data-stu-id="91c8b-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="91c8b-245">Editar las columnas en el productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="91c8b-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="91c8b-246">Queremos ocultar las columnas CategoryId y categoría y establezca el ProductId de sólo lectura.</span><span class="sxs-lookup"><span data-stu-id="91c8b-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="91c8b-247">El valor para la propiedad ProductId es generado por la base de datos después de que guardemos los datos.</span><span class="sxs-lookup"><span data-stu-id="91c8b-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="91c8b-248">Haga clic en el control DataGridView de formularios y seleccione **Editar columnas...** .</span><span class="sxs-lookup"><span data-stu-id="91c8b-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="91c8b-249">Seleccione el **ProductId** columna y establezca **ReadOnly** a **True**.</span><span class="sxs-lookup"><span data-stu-id="91c8b-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="91c8b-250">Seleccione el **CategoryId** columna y presione la **quitar** botón.</span><span class="sxs-lookup"><span data-stu-id="91c8b-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="91c8b-251">Haga lo mismo con el **categoría** columna.</span><span class="sxs-lookup"><span data-stu-id="91c8b-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="91c8b-252">Presione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="91c8b-252">Press **OK**.</span></span>

    <span data-ttu-id="91c8b-253">Hasta ahora, hemos asociado nuestros controles DataGridView de formularios componentes BindingSource en el diseñador.</span><span class="sxs-lookup"><span data-stu-id="91c8b-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="91c8b-254">En la siguiente sección se agregará código para el código subyacente para establecer categoryBindingSource.DataSource a la colección de entidades que realiza el seguimiento DbContext actualmente.</span><span class="sxs-lookup"><span data-stu-id="91c8b-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="91c8b-255">Cuando se arrastra y coloca los productos desde la categoría, los formularios Windows Forms encargó de la configuración de la propiedad productsBindingSource.DataSource a la propiedad categoryBindingSource y productsBindingSource.DataMember para productos.</span><span class="sxs-lookup"><span data-stu-id="91c8b-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="91c8b-256">Debido a este enlace, se mostrará solo los productos que pertenecen a la categoría actualmente seleccionada en el productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="91c8b-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="91c8b-257">Habilitar la **guardar** botón en la barra de herramientas de navegación, haga clic en el botón secundario del mouse y seleccione **habilitado**.</span><span class="sxs-lookup"><span data-stu-id="91c8b-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Diseñador de formularios 1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="91c8b-259">Agregue el controlador de eventos para la operación de guardar botón haciendo doble clic en el botón.</span><span class="sxs-lookup"><span data-stu-id="91c8b-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="91c8b-260">Esto agregará el controlador de eventos y proporcionan el código subyacente para el formulario.</span><span class="sxs-lookup"><span data-stu-id="91c8b-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="91c8b-261">El código para el **categoryBindingNavigatorSaveItem\_haga clic en** se agregará el controlador de eventos en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="91c8b-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="91c8b-262">Agregue el código que administra la interacción de datos</span><span class="sxs-lookup"><span data-stu-id="91c8b-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="91c8b-263">Ahora vamos a agregar el código para usar el ProductContext para tener acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="91c8b-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="91c8b-264">Como se muestra a continuación, actualice el código de la ventana del formulario principal.</span><span class="sxs-lookup"><span data-stu-id="91c8b-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="91c8b-265">El código declara una instancia de ejecución prolongada de ProductContext.</span><span class="sxs-lookup"><span data-stu-id="91c8b-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="91c8b-266">El objeto ProductContext se usa para consultar y guardar los datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="91c8b-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="91c8b-267">El método Dispose() en la instancia de ProductContext, a continuación, se llama desde el método OnClosing invalidado.</span><span class="sxs-lookup"><span data-stu-id="91c8b-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="91c8b-268">Los comentarios del código proporcionan detalles sobre lo que hace el código.</span><span class="sxs-lookup"><span data-stu-id="91c8b-268">The code comments provide details about what the code does.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="91c8b-269">Probar la aplicación de Windows Forms</span><span class="sxs-lookup"><span data-stu-id="91c8b-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="91c8b-270">La aplicación y puede probar la funcionalidad de compilación y ejecución.</span><span class="sxs-lookup"><span data-stu-id="91c8b-270">Compile and run the application and you can test out the functionality.</span></span>

    ![Formulario 1 antes de guardar](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="91c8b-272">Después de guardar las claves del almacén generado se muestran en la pantalla.</span><span class="sxs-lookup"><span data-stu-id="91c8b-272">After saving the store generated keys are shown on the screen.</span></span>

    ![Formulario 1 guardar después](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="91c8b-274">Si utilizó Code First, también verá que una **WinFormswithEFSample.ProductContext** base de datos se crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="91c8b-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Explorador de objetos de servidor](~/ef6/media/serverobjexplorer.png)
