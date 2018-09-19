---
title: Compatibilidad con enum - Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283725"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="73bec-102">Compatibilidad con enum - Code First</span><span class="sxs-lookup"><span data-stu-id="73bec-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="73bec-103">**EF5 y versiones posteriores solo** -las características, las API, etc. se describe en esta página se introdujeron en Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="73bec-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="73bec-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="73bec-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="73bec-105">En este tutorial de vídeo y paso a paso muestra cómo usar los tipos de enumeración con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="73bec-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="73bec-106">También se muestra cómo usar las enumeraciones en una consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="73bec-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="73bec-107">En este tutorial utilizará Code First para crear una nueva base de datos, pero también puede usar [Code First para asignar a una base de datos](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="73bec-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="73bec-108">Compatibilidad de la enumeración se introdujo en Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="73bec-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="73bec-109">Para usar las nuevas características como enumeraciones, tipos de datos espaciales y funciones con valores de tabla, debe tener como destino .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="73bec-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="73bec-110">Visual Studio 2012 tiene como destino .NET 4.5 de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="73bec-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="73bec-111">En Entity Framework, una enumeración puede tener los siguientes tipos subyacentes: **bytes**, **Int16**, **Int32**, **Int64** , o **SByte**.</span><span class="sxs-lookup"><span data-stu-id="73bec-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="73bec-112">Vea el vídeo</span><span class="sxs-lookup"><span data-stu-id="73bec-112">Watch the video</span></span>
<span data-ttu-id="73bec-113">Este vídeo muestra cómo usar los tipos de enumeración con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="73bec-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="73bec-114">También se muestra cómo usar las enumeraciones en una consulta LINQ.</span><span class="sxs-lookup"><span data-stu-id="73bec-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="73bec-115">**Presentado por**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="73bec-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="73bec-116">**Vídeo**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="73bec-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="73bec-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="73bec-117">Pre-Requisites</span></span>

<span data-ttu-id="73bec-118">Necesitará tener Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition instalado para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="73bec-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="73bec-119">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="73bec-119">Set up the Project</span></span>

1.  <span data-ttu-id="73bec-120">Abra Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="73bec-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="73bec-121">En el **archivo** menú, elija **New**y, a continuación, haga clic en **proyecto**</span><span class="sxs-lookup"><span data-stu-id="73bec-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="73bec-122">En el panel izquierdo, haga clic en **Visual C\#** y, a continuación, seleccione el **consola** plantilla</span><span class="sxs-lookup"><span data-stu-id="73bec-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="73bec-123">Escriba **EnumCodeFirst** como el nombre del proyecto y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="73bec-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="73bec-124">Definir un modelo nuevo mediante Code First</span><span class="sxs-lookup"><span data-stu-id="73bec-124">Define a New Model using Code First</span></span>

<span data-ttu-id="73bec-125">Cuando usando desarrollo Code First suele comenzar mediante la escritura de las clases de .NET Framework que definen el modelo conceptual (dominio).</span><span class="sxs-lookup"><span data-stu-id="73bec-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="73bec-126">El código siguiente define la clase de departamento.</span><span class="sxs-lookup"><span data-stu-id="73bec-126">The code below defines the Department class.</span></span>

<span data-ttu-id="73bec-127">El código también define la enumeración DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="73bec-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="73bec-128">De forma predeterminada, la enumeración es de **int** tipo.</span><span class="sxs-lookup"><span data-stu-id="73bec-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="73bec-129">La propiedad Name de la clase departamento es del tipo DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="73bec-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="73bec-130">Abra el archivo Program.cs y pegue las siguientes definiciones de clase.</span><span class="sxs-lookup"><span data-stu-id="73bec-130">Open the Program.cs file and paste the following class definitions.</span></span>

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="73bec-131">Definir la clase DbContext tipo derivado</span><span class="sxs-lookup"><span data-stu-id="73bec-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="73bec-132">Además de definir las entidades, debe definir una clase que deriva de DbContext y expone DbSet&lt;TEntity&gt; propiedades.</span><span class="sxs-lookup"><span data-stu-id="73bec-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="73bec-133">La clase DbSet&lt;TEntity&gt; propiedades que el contexto sepan qué tipos van a incluir en el modelo.</span><span class="sxs-lookup"><span data-stu-id="73bec-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="73bec-134">Una instancia del tipo derivado de DbContext administra los objetos entidad durante el tiempo de ejecución, que incluye rellenar objetos con los datos de una base de datos, cambie el seguimiento y almacenar datos a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="73bec-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="73bec-135">Los tipos de DbContext y DbSet se definen en el ensamblado de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="73bec-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="73bec-136">Se agregará una referencia a este archivo DLL mediante el paquete EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="73bec-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="73bec-137">En el Explorador de soluciones, haga doble clic en el nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="73bec-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="73bec-138">Seleccione **administrar paquetes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="73bec-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="73bec-139">En el cuadro de diálogo Administrar paquetes de NuGet, seleccione el **Online** pestaña y elija el **EntityFramework** paquete.</span><span class="sxs-lookup"><span data-stu-id="73bec-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="73bec-140">Haga clic en **instalar**</span><span class="sxs-lookup"><span data-stu-id="73bec-140">Click **Install**</span></span>

<span data-ttu-id="73bec-141">Tenga en cuenta que el ensamblado de Entity Framework, además de las referencias a ensamblados System.ComponentModel.DataAnnotations y System.Data.Entity se agregan también.</span><span class="sxs-lookup"><span data-stu-id="73bec-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="73bec-142">En la parte superior del archivo Program.cs, agregue la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="73bec-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="73bec-143">En el archivo Program.cs, agregue la definición del contexto.</span><span class="sxs-lookup"><span data-stu-id="73bec-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="73bec-144">Conservar y recuperar datos</span><span class="sxs-lookup"><span data-stu-id="73bec-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="73bec-145">Abra el archivo Program.cs, donde se define el método Main.</span><span class="sxs-lookup"><span data-stu-id="73bec-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="73bec-146">Agregue el código siguiente en la función Main.</span><span class="sxs-lookup"><span data-stu-id="73bec-146">Add the following code into the Main function.</span></span> <span data-ttu-id="73bec-147">El código agrega un nuevo objeto de departamento en el contexto.</span><span class="sxs-lookup"><span data-stu-id="73bec-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="73bec-148">A continuación, guarda los datos.</span><span class="sxs-lookup"><span data-stu-id="73bec-148">It then saves the data.</span></span> <span data-ttu-id="73bec-149">El código también ejecuta una consulta LINQ que devuelve un departamento donde el nombre es DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="73bec-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="73bec-150">Compile y ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="73bec-150">Compile and run the application.</span></span> <span data-ttu-id="73bec-151">El programa produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="73bec-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="73bec-152">Vista de la base de datos generado</span><span class="sxs-lookup"><span data-stu-id="73bec-152">View the Generated Database</span></span>

<span data-ttu-id="73bec-153">Al ejecutar la aplicación por primera vez, Entity Framework crea una base de datos.</span><span class="sxs-lookup"><span data-stu-id="73bec-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="73bec-154">Dado que tenemos instalado Visual Studio 2012, se creará la base de datos en la instancia de LocalDB.</span><span class="sxs-lookup"><span data-stu-id="73bec-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="73bec-155">De forma predeterminada, Entity Framework los nombres de la base de datos después del nombre completo del contexto derivado (para este ejemplo, es **EnumCodeFirst.EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="73bec-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="73bec-156">Las siguientes veces que se usará la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="73bec-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="73bec-157">Tenga en cuenta que si realiza cualquier cambio en el modelo una vez creada la base de datos, debe usar migraciones de Code First para actualizar el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="73bec-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="73bec-158">Consulte [Code First para una base de datos](~/ef6/modeling/code-first/workflows/new-database.md) para obtener un ejemplo del uso de las migraciones.</span><span class="sxs-lookup"><span data-stu-id="73bec-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="73bec-159">Para ver la base de datos y los datos, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="73bec-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="73bec-160">En el menú principal de Visual Studio 2012, seleccione **vista**  - &gt; **Explorador de objetos de SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="73bec-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="73bec-161">Si LocalDB no está en la lista de servidores, haga clic en el botón secundario del mouse en **SQL Server** y seleccione **agregar SQL Server** Use el valor predeterminado **Windows autenticación** para conectarse a la Instancia de LocalDB</span><span class="sxs-lookup"><span data-stu-id="73bec-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="73bec-162">Expanda el nodo de LocalDB</span><span class="sxs-lookup"><span data-stu-id="73bec-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="73bec-163">Expandir el **bases de datos** carpeta para ver la nueva base de datos y busque el **departamento** tabla tenga en cuenta que Code First no crea una tabla que asigna al tipo de enumeración</span><span class="sxs-lookup"><span data-stu-id="73bec-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="73bec-164">Para ver los datos, haga doble clic en la tabla y seleccione **ver datos**</span><span class="sxs-lookup"><span data-stu-id="73bec-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="73bec-165">Resumen</span><span class="sxs-lookup"><span data-stu-id="73bec-165">Summary</span></span>

<span data-ttu-id="73bec-166">En este tutorial analizamos cómo usar los tipos de enumeración con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="73bec-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
