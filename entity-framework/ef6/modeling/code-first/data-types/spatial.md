---
title: Espacial - Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: b8f2485a7002dcb4079f4045432f3224123fdb2f
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283607"
---
# <a name="spatial---code-first"></a><span data-ttu-id="629f0-102">Espacial - Code First</span><span class="sxs-lookup"><span data-stu-id="629f0-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="629f0-103">**EF5 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="629f0-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="629f0-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="629f0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="629f0-105">El tutorial de vídeo y paso a paso muestra cómo asignar tipos espaciales con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="629f0-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="629f0-106">También se muestra cómo usar una consulta LINQ para buscar una distancia entre dos ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="629f0-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="629f0-107">En este tutorial utilizará Code First para crear una nueva base de datos, pero también puede usar [Code First para una base de datos](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="629f0-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="629f0-108">Compatibilidad con tipos espaciales se introdujo en Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="629f0-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="629f0-109">Tenga en cuenta que para utilizar las nuevas características, como el tipo espacial, enumeraciones y funciones con valores de tabla, debe tener como destino .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="629f0-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="629f0-110">Visual Studio 2012 tiene como destino .NET 4.5 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="629f0-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="629f0-111">Para utilizar tipos de datos espaciales también debe usar un proveedor de Entity Framework que tiene compatibilidad espacial.</span><span class="sxs-lookup"><span data-stu-id="629f0-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="629f0-112">Consulte [compatibilidad del proveedor con tipos espaciales](~/ef6/fundamentals/providers/spatial-support.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="629f0-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="629f0-113">Hay dos tipos principales de los datos espaciales: geography y geometry.</span><span class="sxs-lookup"><span data-stu-id="629f0-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="629f0-114">El tipo de datos geography almacena datos elípticos (por ejemplo, coordenadas GPS latitud y longitud).</span><span class="sxs-lookup"><span data-stu-id="629f0-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="629f0-115">El tipo de datos geometry representa el sistema de coordenadas euclidiano (plano).</span><span class="sxs-lookup"><span data-stu-id="629f0-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="629f0-116">Vea el vídeo</span><span class="sxs-lookup"><span data-stu-id="629f0-116">Watch the video</span></span>
<span data-ttu-id="629f0-117">Este vídeo muestra cómo asignar tipos espaciales con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="629f0-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="629f0-118">También se muestra cómo usar una consulta LINQ para buscar una distancia entre dos ubicaciones.</span><span class="sxs-lookup"><span data-stu-id="629f0-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="629f0-119">**Presentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="629f0-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="629f0-120">**Vídeo**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="629f0-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="629f0-121">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="629f0-121">Pre-Requisites</span></span>

<span data-ttu-id="629f0-122">Necesitará tener Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition instalado para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="629f0-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="629f0-123">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="629f0-123">Set up the Project</span></span>

1.  <span data-ttu-id="629f0-124">Abra Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="629f0-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="629f0-125">En el **archivo** menú, elija **New**y, a continuación, haga clic en **proyecto**</span><span class="sxs-lookup"><span data-stu-id="629f0-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="629f0-126">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla</span><span class="sxs-lookup"><span data-stu-id="629f0-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="629f0-127">Escriba **SpatialCodeFirst** como el nombre del proyecto y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="629f0-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="629f0-128">Definir un modelo nuevo mediante Code First</span><span class="sxs-lookup"><span data-stu-id="629f0-128">Define a New Model using Code First</span></span>

<span data-ttu-id="629f0-129">Cuando usando desarrollo Code First suele comenzar mediante la escritura de las clases de .NET Framework que definen el modelo conceptual (dominio).</span><span class="sxs-lookup"><span data-stu-id="629f0-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="629f0-130">El código siguiente define la clase de la universidad.</span><span class="sxs-lookup"><span data-stu-id="629f0-130">The code below defines the University class.</span></span>

<span data-ttu-id="629f0-131">La Universidad tiene la propiedad de ubicación del tipo de DbGeography.</span><span class="sxs-lookup"><span data-stu-id="629f0-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="629f0-132">Para usar el tipo de DbGeography, debe agregar una referencia al ensamblado System.Data.Entity y también agregar la instrucción using de System.Data.Spatial.</span><span class="sxs-lookup"><span data-stu-id="629f0-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="629f0-133">Abra el archivo Program.cs y pegue lo siguiente mediante instrucciones en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="629f0-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="629f0-134">En el archivo Program.cs, agregue la siguiente definición de clase de la universidad.</span><span class="sxs-lookup"><span data-stu-id="629f0-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="629f0-135">Definir la clase DbContext tipo derivado</span><span class="sxs-lookup"><span data-stu-id="629f0-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="629f0-136">Además de definir las entidades, debe definir una clase que deriva de DbContext y expone DbSet&lt;TEntity&gt; propiedades.</span><span class="sxs-lookup"><span data-stu-id="629f0-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="629f0-137">La clase DbSet&lt;TEntity&gt; propiedades que el contexto sepan qué tipos van a incluir en el modelo.</span><span class="sxs-lookup"><span data-stu-id="629f0-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="629f0-138">Una instancia del tipo derivado de DbContext administra los objetos entidad durante el tiempo de ejecución, que incluye rellenar objetos con los datos de una base de datos, cambie el seguimiento y almacenar datos a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="629f0-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="629f0-139">Los tipos de DbContext y DbSet se definen en el ensamblado de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="629f0-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="629f0-140">Se agregará una referencia a este archivo DLL mediante el paquete EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="629f0-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="629f0-141">En el Explorador de soluciones, haga doble clic en el nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="629f0-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="629f0-142">Seleccione **administrar paquetes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="629f0-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="629f0-143">En el cuadro de diálogo Administrar paquetes de NuGet, seleccione el **Online** pestaña y elija el **EntityFramework** paquete.</span><span class="sxs-lookup"><span data-stu-id="629f0-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="629f0-144">Haga clic en **instalar**</span><span class="sxs-lookup"><span data-stu-id="629f0-144">Click **Install**</span></span>

<span data-ttu-id="629f0-145">Tenga en cuenta que el ensamblado de Entity Framework, además de una referencia al ensamblado System.ComponentModel.DataAnnotations también se agrega.</span><span class="sxs-lookup"><span data-stu-id="629f0-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="629f0-146">En la parte superior del archivo Program.cs, agregue la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="629f0-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="629f0-147">En el archivo Program.cs, agregue la definición del contexto.</span><span class="sxs-lookup"><span data-stu-id="629f0-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="629f0-148">Conservar y recuperar datos</span><span class="sxs-lookup"><span data-stu-id="629f0-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="629f0-149">Abra el archivo Program.cs, donde se define el método Main.</span><span class="sxs-lookup"><span data-stu-id="629f0-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="629f0-150">Agregue el código siguiente en la función Main.</span><span class="sxs-lookup"><span data-stu-id="629f0-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="629f0-151">El código agrega dos nuevos objetos de la Universidad del contexto.</span><span class="sxs-lookup"><span data-stu-id="629f0-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="629f0-152">Propiedades espaciales se inicializan mediante el método DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="629f0-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="629f0-153">Punto de geografía que se representan como WellKnownText se pasa al método.</span><span class="sxs-lookup"><span data-stu-id="629f0-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="629f0-154">El código, a continuación, guarda los datos.</span><span class="sxs-lookup"><span data-stu-id="629f0-154">The code then saves the data.</span></span> <span data-ttu-id="629f0-155">A continuación, la consulta LINQ que devuelve un objeto de la Universidad donde su ubicación es más cercana a la ubicación especificada, se genera y ejecuta.</span><span class="sxs-lookup"><span data-stu-id="629f0-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="629f0-156">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="629f0-156">Compile and run the application.</span></span> <span data-ttu-id="629f0-157">El programa produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="629f0-157">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="629f0-158">Vista de la base de datos generado</span><span class="sxs-lookup"><span data-stu-id="629f0-158">View the Generated Database</span></span>

<span data-ttu-id="629f0-159">Al ejecutar la aplicación por primera vez, Entity Framework crea una base de datos.</span><span class="sxs-lookup"><span data-stu-id="629f0-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="629f0-160">Dado que tenemos instalado Visual Studio 2012, se creará la base de datos en la instancia de LocalDB.</span><span class="sxs-lookup"><span data-stu-id="629f0-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="629f0-161">De forma predeterminada, Entity Framework los nombres de la base de datos después del nombre completo del contexto derivado (en este ejemplo es **SpatialCodeFirst.UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="629f0-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="629f0-162">Las siguientes veces que se usará la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="629f0-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="629f0-163">Tenga en cuenta que si realiza cualquier cambio en el modelo una vez creada la base de datos, debe usar migraciones de Code First para actualizar el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="629f0-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="629f0-164">Consulte [Code First para una base de datos](~/ef6/modeling/code-first/workflows/new-database.md) para obtener un ejemplo del uso de las migraciones.</span><span class="sxs-lookup"><span data-stu-id="629f0-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="629f0-165">Para ver la base de datos y los datos, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="629f0-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="629f0-166">En el menú principal de Visual Studio 2012, seleccione **vista**  - &gt; **Explorador de objetos de SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="629f0-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="629f0-167">Si LocalDB no está en la lista de servidores, haga clic en el botón secundario del mouse en **SQL Server** y seleccione **agregar SQL Server** Use el valor predeterminado **Windows autenticación** para conectarse a la la instancia de LocalDB</span><span class="sxs-lookup"><span data-stu-id="629f0-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="629f0-168">Expanda el nodo de LocalDB</span><span class="sxs-lookup"><span data-stu-id="629f0-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="629f0-169">Expandir el **bases de datos** carpeta para ver la nueva base de datos y busque el **universidades** tabla</span><span class="sxs-lookup"><span data-stu-id="629f0-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="629f0-170">Para ver los datos, haga doble clic en la tabla y seleccione **ver datos**</span><span class="sxs-lookup"><span data-stu-id="629f0-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="629f0-171">Resumen</span><span class="sxs-lookup"><span data-stu-id="629f0-171">Summary</span></span>

<span data-ttu-id="629f0-172">En este tutorial analizamos cómo usar tipos espaciales con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="629f0-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
