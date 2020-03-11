---
title: Code First espaciales-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415788"
---
# <a name="spatial---code-first"></a><span data-ttu-id="e361d-102">Code First espacial</span><span class="sxs-lookup"><span data-stu-id="e361d-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="e361d-103">**EF5** y versiones posteriores: las características, las API, etc. que se describen en esta página se introdujeron en Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="e361d-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="e361d-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="e361d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="e361d-105">En el tutorial de vídeo y paso a paso se muestra cómo asignar tipos espaciales con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="e361d-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="e361d-106">También se muestra cómo usar una consulta LINQ para buscar una distancia entre dos ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="e361d-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="e361d-107">En este tutorial se utilizará Code First para crear una nueva base de datos, pero también puede usar [code First a una base de datos existente](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="e361d-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="e361d-108">La compatibilidad con tipos espaciales se presentó en Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="e361d-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="e361d-109">Tenga en cuenta que para usar las nuevas características, como el tipo espacial, las enumeraciones y las funciones con valores de tabla, debe tener como destino .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="e361d-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="e361d-110">Visual Studio 2012 tiene como destino .NET 4,5 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="e361d-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="e361d-111">Para usar los tipos de datos espaciales, también debe usar un proveedor de Entity Framework que tenga compatibilidad espacial.</span><span class="sxs-lookup"><span data-stu-id="e361d-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="e361d-112">Consulte [compatibilidad con proveedores para tipos espaciales](~/ef6/fundamentals/providers/spatial-support.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="e361d-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="e361d-113">Hay dos tipos de datos espaciales principales: Geography y Geometry.</span><span class="sxs-lookup"><span data-stu-id="e361d-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="e361d-114">El tipo de datos Geography almacena los datos de datos elipsoidales (por ejemplo, las coordenadas de latitud y longitud de GPS).</span><span class="sxs-lookup"><span data-stu-id="e361d-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="e361d-115">El tipo de datos Geometry representa el sistema de coordenadas euclidiana (plano).</span><span class="sxs-lookup"><span data-stu-id="e361d-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="e361d-116">Visualización del vídeo</span><span class="sxs-lookup"><span data-stu-id="e361d-116">Watch the video</span></span>
<span data-ttu-id="e361d-117">Este vídeo muestra cómo asignar tipos espaciales con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="e361d-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="e361d-118">También se muestra cómo usar una consulta LINQ para buscar una distancia entre dos ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="e361d-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="e361d-119">**Presentada por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="e361d-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="e361d-120">**Vídeo**: [wmv](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="e361d-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="e361d-121">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="e361d-121">Pre-Requisites</span></span>

<span data-ttu-id="e361d-122">Deberá tener instalado Visual Studio 2012, Ultimate, Premium, Professional o Web Express Edition para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="e361d-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="e361d-123">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="e361d-123">Set up the Project</span></span>

1.  <span data-ttu-id="e361d-124">Abra Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e361d-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="e361d-125">En el menú **archivo** , seleccione **nuevo**y, a continuación, haga clic en **proyecto** .</span><span class="sxs-lookup"><span data-stu-id="e361d-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="e361d-126">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione la plantilla de **consola** .</span><span class="sxs-lookup"><span data-stu-id="e361d-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="e361d-127">Escriba **SpatialCodeFirst** como nombre del proyecto y haga clic en **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="e361d-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="e361d-128">Definir un nuevo modelo mediante Code First</span><span class="sxs-lookup"><span data-stu-id="e361d-128">Define a New Model using Code First</span></span>

<span data-ttu-id="e361d-129">Al usar Code First desarrollo, normalmente comienza por escribir .NET Framework clases que definen el modelo conceptual (de dominio).</span><span class="sxs-lookup"><span data-stu-id="e361d-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="e361d-130">El código siguiente define la clase University.</span><span class="sxs-lookup"><span data-stu-id="e361d-130">The code below defines the University class.</span></span>

<span data-ttu-id="e361d-131">La Universidad tiene la propiedad Location del tipo DbGeography.</span><span class="sxs-lookup"><span data-stu-id="e361d-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="e361d-132">Para usar el tipo DbGeography, debe agregar una referencia al ensamblado System. Data. Entity y también agregar la instrucción using System. Data. Spatial.</span><span class="sxs-lookup"><span data-stu-id="e361d-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="e361d-133">Abra el archivo Program.cs y pegue las siguientes instrucciones Using en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="e361d-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="e361d-134">Agregue la siguiente definición de clase University al archivo Program.cs.</span><span class="sxs-lookup"><span data-stu-id="e361d-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="e361d-135">Definir el tipo derivado de DbContext</span><span class="sxs-lookup"><span data-stu-id="e361d-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="e361d-136">Además de definir entidades, debe definir una clase que derive de DbContext y exponga las propiedades de DbSet&lt;de la carpa&gt;.</span><span class="sxs-lookup"><span data-stu-id="e361d-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="e361d-137">Las propiedades de DbSet&lt;la&gt; de la carpa permiten que el contexto sepa qué tipos desea incluir en el modelo.</span><span class="sxs-lookup"><span data-stu-id="e361d-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="e361d-138">Una instancia del tipo derivado de DbContext administra los objetos de entidad durante el tiempo de ejecución, lo que incluye el rellenado de objetos con datos de una base de datos, el seguimiento de cambios y la persistencia de datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e361d-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="e361d-139">Los tipos DbContext y DbSet se definen en el ensamblado EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="e361d-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="e361d-140">Se agregará una referencia a este archivo DLL mediante el paquete NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="e361d-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="e361d-141">En Explorador de soluciones, haga clic con el botón derecho en el nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="e361d-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="e361d-142">Seleccione **administrar paquetes NuGet..** .</span><span class="sxs-lookup"><span data-stu-id="e361d-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="e361d-143">En el cuadro de diálogo administrar paquetes NuGet, seleccione la pestaña **en línea** y elija el paquete **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="e361d-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="e361d-144">Haga clic en **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="e361d-144">Click **Install**</span></span>

<span data-ttu-id="e361d-145">Tenga en cuenta que, además del ensamblado EntityFramework, también se agrega una referencia al ensamblado System. ComponentModel. DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="e361d-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="e361d-146">En la parte superior del archivo Program.cs, agregue la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="e361d-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="e361d-147">En Program.cs, agregue la definición de contexto.</span><span class="sxs-lookup"><span data-stu-id="e361d-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="e361d-148">Conservar y recuperar datos</span><span class="sxs-lookup"><span data-stu-id="e361d-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="e361d-149">Abra el archivo Program.cs en el que se define el método Main.</span><span class="sxs-lookup"><span data-stu-id="e361d-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="e361d-150">Agregue el código siguiente a la función main.</span><span class="sxs-lookup"><span data-stu-id="e361d-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="e361d-151">El código agrega dos nuevos objetos universitarios al contexto.</span><span class="sxs-lookup"><span data-stu-id="e361d-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="e361d-152">Las propiedades espaciales se inicializan mediante el método DbGeography. FromText.</span><span class="sxs-lookup"><span data-stu-id="e361d-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="e361d-153">El punto de geografía representado como WellKnownText se pasa al método.</span><span class="sxs-lookup"><span data-stu-id="e361d-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="e361d-154">Después, el código guarda los datos.</span><span class="sxs-lookup"><span data-stu-id="e361d-154">The code then saves the data.</span></span> <span data-ttu-id="e361d-155">A continuación, se crea y se ejecuta la consulta LINQ que devuelve un objeto University donde su ubicación es más cercana a la ubicación especificada.</span><span class="sxs-lookup"><span data-stu-id="e361d-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="e361d-156">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e361d-156">Compile and run the application.</span></span> <span data-ttu-id="e361d-157">El programa produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="e361d-157">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="e361d-158">Ver la base de datos generada</span><span class="sxs-lookup"><span data-stu-id="e361d-158">View the Generated Database</span></span>

<span data-ttu-id="e361d-159">Al ejecutar la aplicación por primera vez, el Entity Framework crea una base de datos automáticamente.</span><span class="sxs-lookup"><span data-stu-id="e361d-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="e361d-160">Dado que tenemos instalado Visual Studio 2012, la base de datos se creará en la instancia de LocalDB.</span><span class="sxs-lookup"><span data-stu-id="e361d-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="e361d-161">De forma predeterminada, el Entity Framework nombra la base de datos después del nombre completo del contexto derivado (en este ejemplo, que es **SpatialCodeFirst. UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="e361d-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="e361d-162">Las veces posteriores en las que se utilizará la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="e361d-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="e361d-163">Tenga en cuenta que si realiza cambios en el modelo una vez creada la base de datos, debe utilizar Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e361d-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="e361d-164">Vea [code First en una nueva base de datos](~/ef6/modeling/code-first/workflows/new-database.md) para obtener un ejemplo del uso de las migraciones.</span><span class="sxs-lookup"><span data-stu-id="e361d-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="e361d-165">Para ver la base de datos y los datos, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e361d-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="e361d-166">En el menú principal de Visual Studio 2012, seleccione **ver** -&gt; **Explorador de objetos de SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e361d-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="e361d-167">Si LocalDB no está en la lista de servidores, haga clic con el botón secundario del mouse en **SQL Server** y seleccione **Agregar SQL Server** usar la **autenticación de Windows** predeterminada para conectarse a la instancia de LocalDB.</span><span class="sxs-lookup"><span data-stu-id="e361d-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="e361d-168">Expandir el nodo LocalDB</span><span class="sxs-lookup"><span data-stu-id="e361d-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="e361d-169">Desdoblar la carpeta **bases** de datos para ver la nueva base de datos y examinar la tabla **universidades**</span><span class="sxs-lookup"><span data-stu-id="e361d-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="e361d-170">Para ver los datos, haga clic con el botón derecho en la tabla y seleccione **ver datos** .</span><span class="sxs-lookup"><span data-stu-id="e361d-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="e361d-171">Resumen</span><span class="sxs-lookup"><span data-stu-id="e361d-171">Summary</span></span>

<span data-ttu-id="e361d-172">En este tutorial, hemos visto cómo usar los tipos espaciales con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="e361d-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
