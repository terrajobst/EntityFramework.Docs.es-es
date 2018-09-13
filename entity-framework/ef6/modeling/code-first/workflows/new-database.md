---
title: 'Code First en una base de datos: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 6572574ad36094ac0960c429cfa8606b6aebb492
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490758"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="270b7-102">Code First en una base de datos</span><span class="sxs-lookup"><span data-stu-id="270b7-102">Code First to a New Database</span></span>
<span data-ttu-id="270b7-103">En este tutorial de vídeo y paso a paso proporcionan una introducción al desarrollo de Code First destinadas a una base de datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="270b7-104">Este escenario incluye destinadas a una base de datos que no existe y Code First creará o una base de datos vacía que Code First agregará nuevas tablas a.</span><span class="sxs-lookup"><span data-stu-id="270b7-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="270b7-105">Código primero le permite definir su modelo mediante C\# o clases de VB.Net.</span><span class="sxs-lookup"><span data-stu-id="270b7-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="270b7-106">Configuración adicional si lo desea puede realizarse mediante los atributos en las clases y propiedades o mediante una API fluida.</span><span class="sxs-lookup"><span data-stu-id="270b7-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="270b7-107">Vea el vídeo</span><span class="sxs-lookup"><span data-stu-id="270b7-107">Watch the video</span></span>
<span data-ttu-id="270b7-108">Este vídeo ofrece una introducción al desarrollo de Code First destinadas a una base de datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="270b7-109">Este escenario incluye destinadas a una base de datos que no existe y Code First creará o una base de datos vacía que Code First agregará nuevas tablas a.</span><span class="sxs-lookup"><span data-stu-id="270b7-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="270b7-110">Código primero permite definir su modelo mediante clases de C# o VB.Net.</span><span class="sxs-lookup"><span data-stu-id="270b7-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="270b7-111">Configuración adicional si lo desea puede realizarse mediante los atributos en las clases y propiedades o mediante una API fluida.</span><span class="sxs-lookup"><span data-stu-id="270b7-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="270b7-112">**Presentado por**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="270b7-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="270b7-113">**Vídeo**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="270b7-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="270b7-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="270b7-114">Pre-Requisites</span></span>

<span data-ttu-id="270b7-115">Debe tener al menos Visual Studio 2010 o Visual Studio 2012 instalado para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="270b7-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="270b7-116">Si utiliza Visual Studio 2010, también necesitará tener [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.</span><span class="sxs-lookup"><span data-stu-id="270b7-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="270b7-117">1. Crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="270b7-117">1. Create the Application</span></span>

<span data-ttu-id="270b7-118">Para simplificar las cosas que vamos a crear una aplicación de consola básica que usa Code First para tener acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="270b7-119">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="270b7-119">Open Visual Studio</span></span>
-   <span data-ttu-id="270b7-120">**Archivo -&gt; nuevo:&gt; proyecto...**</span><span class="sxs-lookup"><span data-stu-id="270b7-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="270b7-121">Seleccione **Windows** en el menú izquierdo y **aplicación de consola**</span><span class="sxs-lookup"><span data-stu-id="270b7-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="270b7-122">Escriba **CodeFirstNewDatabaseSample** como el nombre</span><span class="sxs-lookup"><span data-stu-id="270b7-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="270b7-123">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="270b7-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="270b7-124">2. Crear el modelo</span><span class="sxs-lookup"><span data-stu-id="270b7-124">2. Create the Model</span></span>

<span data-ttu-id="270b7-125">Vamos a definir un modelo muy simple mediante clases.</span><span class="sxs-lookup"><span data-stu-id="270b7-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="270b7-126">Simplemente nos estamos definiendo en el archivo Program.cs, pero en una aplicación real que dividiría las clases de salida en archivos independientes y, potencialmente, un proyecto independiente.</span><span class="sxs-lookup"><span data-stu-id="270b7-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="270b7-127">Debajo de la definición de clase de programa en Program.cs, agregue las dos clases siguientes.</span><span class="sxs-lookup"><span data-stu-id="270b7-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

<span data-ttu-id="270b7-128">Observará que estamos haciendo las dos propiedades de navegación (Blog.Posts y Post.Blog) virtual.</span><span class="sxs-lookup"><span data-stu-id="270b7-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="270b7-129">Esto habilita la característica de carga diferida de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="270b7-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="270b7-130">La carga diferida significa que el contenido de estas propiedades se cargarán automáticamente desde la base de datos cuando se intenta tener acceso a ellos.</span><span class="sxs-lookup"><span data-stu-id="270b7-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="270b7-131">3. Crear un contexto</span><span class="sxs-lookup"><span data-stu-id="270b7-131">3. Create a Context</span></span>

<span data-ttu-id="270b7-132">Ahora es momento de definir un contexto derivado que representa una sesión con la base de datos, lo que nos permite consultar y guardar los datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="270b7-133">Definimos un contexto que se deriva de System.Data.Entity.DbContext y expone una clase DbSet con tipo&lt;TEntity&gt; para cada clase en nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="270b7-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="270b7-134">Ahora comienza a usar tipos de Entity Framework, por lo que necesitamos agregar el paquete EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="270b7-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="270b7-135">**Proyecto –&gt; administrar paquetes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="270b7-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="270b7-136">Nota: Si no tienes el **administrar paquetes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="270b7-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="270b7-137">opción que debe instalar la [versión más reciente de NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="270b7-137">option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="270b7-138">Seleccione el **Online** ficha</span><span class="sxs-lookup"><span data-stu-id="270b7-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="270b7-139">Seleccione el **EntityFramework** paquete</span><span class="sxs-lookup"><span data-stu-id="270b7-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="270b7-140">Haga clic en **instalar**</span><span class="sxs-lookup"><span data-stu-id="270b7-140">Click **Install**</span></span>

<span data-ttu-id="270b7-141">Agregue una mediante declaración para System.Data.Entity en la parte superior de Program.cs.</span><span class="sxs-lookup"><span data-stu-id="270b7-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="270b7-142">Debajo de la clase Post en Program.cs, agregue el siguiente contexto derivado.</span><span class="sxs-lookup"><span data-stu-id="270b7-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="270b7-143">Le presentamos una lista completa de lo que debería contener Program.cs.</span><span class="sxs-lookup"><span data-stu-id="270b7-143">Here is a complete listing of what Program.cs should now contain.</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

<span data-ttu-id="270b7-144">Eso es todo el código que se necesita para empezar a almacenar y recuperar datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="270b7-145">Obviamente hay bastantes sucede segundo plano y le echamos un vistazo a que en un momento, pero primero vamos a verlo en acción.</span><span class="sxs-lookup"><span data-stu-id="270b7-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="270b7-146">4. Leer y escribir datos</span><span class="sxs-lookup"><span data-stu-id="270b7-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="270b7-147">Como se muestra a continuación, implemente el método Main en Program.cs.</span><span class="sxs-lookup"><span data-stu-id="270b7-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="270b7-148">Este código crea una nueva instancia de nuestro contexto y, a continuación, usa para insertar un nuevo Blog.</span><span class="sxs-lookup"><span data-stu-id="270b7-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="270b7-149">A continuación, usa una consulta LINQ para recuperar todos los Blogs de la base de datos ordenado alfabéticamente por título.</span><span class="sxs-lookup"><span data-stu-id="270b7-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="270b7-150">Ahora puede ejecutar la aplicación y probarlo.</span><span class="sxs-lookup"><span data-stu-id="270b7-150">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="270b7-151">¿Dónde están mis datos?</span><span class="sxs-lookup"><span data-stu-id="270b7-151">Where’s My Data?</span></span>

<span data-ttu-id="270b7-152">Por convención DbContext ha creado una base de datos por usted.</span><span class="sxs-lookup"><span data-stu-id="270b7-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="270b7-153">Si una instancia local de SQL Express está disponible (instalado con Visual Studio 2010 de forma predeterminada), a continuación, Code First ha creado la base de datos en esa instancia</span><span class="sxs-lookup"><span data-stu-id="270b7-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="270b7-154">Si SQL Express no está disponible, a continuación, Code First se probará ni usará [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (instalado de forma predeterminada con Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="270b7-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="270b7-155">La base de datos se denomina con el nombre completo del contexto derivado, en nuestro caso es **CodeFirstNewDatabaseSample.BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="270b7-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="270b7-156">Estas son simplemente las convenciones predeterminadas y hay varias maneras de cambiar la base de datos que usa Code First, obtener más información está disponible en el **cómo DbContext detecta el modelo y la conexión de base de datos** tema.</span><span class="sxs-lookup"><span data-stu-id="270b7-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="270b7-157">Puede conectarse a esta base de datos mediante el Explorador de servidores en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="270b7-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="270b7-158">**Vista -&gt; Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="270b7-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="270b7-159">Haga clic con el botón derecho en **conexiones de datos** y seleccione **Agregar conexión...**</span><span class="sxs-lookup"><span data-stu-id="270b7-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="270b7-160">Si aún no lo ha conectado a una base de datos del explorador de servidores antes de que tendrá que seleccionar Microsoft SQL Server como origen de datos</span><span class="sxs-lookup"><span data-stu-id="270b7-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Seleccionar origen de datos](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="270b7-162">Conectarse a LocalDB o Express de SQL, según lo que se ha instalado</span><span class="sxs-lookup"><span data-stu-id="270b7-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="270b7-163">Ahora podemos inspeccionar el esquema que Code First crea.</span><span class="sxs-lookup"><span data-stu-id="270b7-163">We can now inspect the schema that Code First created.</span></span>

![Esquema inicial](~/ef6/media/schemainitial.png)

<span data-ttu-id="270b7-165">DbContext puede averiguar qué clases debe incluir en el modelo echando un vistazo a las propiedades DbSet que hemos definido.</span><span class="sxs-lookup"><span data-stu-id="270b7-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="270b7-166">A continuación, se usa el conjunto predeterminado de las convenciones de Code First para determinar los nombres de tabla y columna, determinar los tipos de datos, buscar las claves principales, etcetera.</span><span class="sxs-lookup"><span data-stu-id="270b7-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="270b7-167">Más adelante en este tutorial, buscaremos en cómo puede invalidar estas convenciones.</span><span class="sxs-lookup"><span data-stu-id="270b7-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="270b7-168">5. Trabajar con los cambios del modelo</span><span class="sxs-lookup"><span data-stu-id="270b7-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="270b7-169">Ahora es momento de realizar algunos cambios en nuestro modelo, cuando se realizan estos cambios que también es necesario para actualizar el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="270b7-170">Para ello, vamos a usar una característica denominada migraciones o migraciones de Code First para abreviar.</span><span class="sxs-lookup"><span data-stu-id="270b7-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="270b7-171">Las migraciones nos permite tener un conjunto ordenado de los pasos que describen cómo actualizar el esquema de base de datos (y cambiar).</span><span class="sxs-lookup"><span data-stu-id="270b7-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="270b7-172">Cada uno de estos pasos, que se conoce como una migración, contiene código que se describe los cambios que se aplicará.</span><span class="sxs-lookup"><span data-stu-id="270b7-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="270b7-173">El primer paso es habilitar migraciones de Code First para nuestro BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="270b7-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="270b7-174">**Herramientas -&gt; Administrador de paquetes de biblioteca -&gt; consola de administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="270b7-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="270b7-175">Ejecute el comando **Enable-Migrations** en la consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="270b7-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="270b7-176">Se ha agregado una nueva carpeta migraciones a nuestro proyecto que contiene dos elementos:</span><span class="sxs-lookup"><span data-stu-id="270b7-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="270b7-177">**Configuration.cs** – este archivo contiene la configuración que se va a usar para migrar las migraciones BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="270b7-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="270b7-178">No es necesario cambiar nada en este tutorial, pero aquí es donde puede especificar datos de inicialización, los proveedores de registro para otras bases de datos, cambian el espacio de nombres que se generan las migraciones en etc.</span><span class="sxs-lookup"><span data-stu-id="270b7-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="270b7-179">**&lt;marca de tiempo&gt;\_InitialCreate.cs** : se trata de la primera migración, representa los cambios que ya se han aplicado a la base de datos a partir de la que se va a una base de datos vacío a uno que incluye las tablas de Blogs y publicaciones .</span><span class="sxs-lookup"><span data-stu-id="270b7-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="270b7-180">Aunque nos dejó que Code First crea automáticamente estas tablas para nosotros, ahora que nos hemos suscrito a las migraciones se han convertido en una migración.</span><span class="sxs-lookup"><span data-stu-id="270b7-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="270b7-181">En primer lugar código también se registra en nuestra base de datos local que se ha aplicado esta migración.</span><span class="sxs-lookup"><span data-stu-id="270b7-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="270b7-182">La marca de tiempo en el nombre de archivo se usa para fines de ordenación.</span><span class="sxs-lookup"><span data-stu-id="270b7-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="270b7-183">Ahora vamos a realizar un cambio en nuestro modelo, agregue una propiedad de dirección Url a la clase de Blog:</span><span class="sxs-lookup"><span data-stu-id="270b7-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="270b7-184">Ejecute el **Add-Migration AddUrl** comando en la consola de administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="270b7-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="270b7-185">El comando Add-Migration comprueba los cambios desde la última migración y aplica la técnica scaffolding una migración nueva con los cambios que se encuentran.</span><span class="sxs-lookup"><span data-stu-id="270b7-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="270b7-186">Podemos poner un nombre; las migraciones en este caso estamos llamando a la migración 'AddUrl'.</span><span class="sxs-lookup"><span data-stu-id="270b7-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="270b7-187">El código con scaffolding está diciendo que tenemos que agregar una columna de dirección Url, que puede contener datos de cadena, el dbo. Tabla de blogs.</span><span class="sxs-lookup"><span data-stu-id="270b7-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="270b7-188">Si es necesario, nos podríamos modificar el código con scaffolding, pero que no es necesario en este caso.</span><span class="sxs-lookup"><span data-stu-id="270b7-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   <span data-ttu-id="270b7-189">Ejecute el **Update-Database** comando en la consola de administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="270b7-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="270b7-190">Este comando aplicará cualquier migración pendiente a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="270b7-191">Ya se ha aplicado la migración InitialCreate por lo que las migraciones solo aplicará nuestra nueva migración AddUrl.</span><span class="sxs-lookup"><span data-stu-id="270b7-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="270b7-192">Sugerencia: Puede usar el **– Verbose** cambiar cuando se llama a Update-Database para ver el código SQL que se está ejecutando en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="270b7-193">Ahora, la nueva columna de dirección Url se agrega a la tabla Blogs en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="270b7-193">The new Url column is now added to the Blogs table in the database:</span></span>

![Esquema de dirección Url](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="270b7-195">6. Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="270b7-195">6. Data Annotations</span></span>

<span data-ttu-id="270b7-196">Hasta ahora hemos solo permitimos que EF descubre el modelo mediante las convenciones de forma predeterminada, pero se va a haber ocasiones nuestras clases no siguen las convenciones y deberá ser capaz de realizar la configuración.</span><span class="sxs-lookup"><span data-stu-id="270b7-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="270b7-197">Hay dos opciones para este fin; veremos las anotaciones de datos en esta sección y, a continuación, la API fluida en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="270b7-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="270b7-198">Vamos a agregar una clase de usuario a nuestro modelo</span><span class="sxs-lookup"><span data-stu-id="270b7-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="270b7-199">También es necesario agregar un conjunto a nuestro contexto derivado</span><span class="sxs-lookup"><span data-stu-id="270b7-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="270b7-200">Si se ha intentado agregar una migración, se obtendría un error que dice que "*EntityType 'User' no tiene ninguna clave definida. Definir la clave para este EntityType."*</span><span class="sxs-lookup"><span data-stu-id="270b7-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="270b7-201">Dado que EF no tiene ninguna manera de saber que el nombre de usuario debe ser la clave principal de usuario.</span><span class="sxs-lookup"><span data-stu-id="270b7-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="270b7-202">Vamos a usar ahora las anotaciones de datos por lo que necesitamos agregar un mediante declaración en la parte superior de Program.cs</span><span class="sxs-lookup"><span data-stu-id="270b7-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="270b7-203">Ahora anotar la propiedad de nombre de usuario para identificar que es la clave principal</span><span class="sxs-lookup"><span data-stu-id="270b7-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="270b7-204">Use la **Add-Migration AddUser** comando para aplicar scaffolding a una migración para aplicar estos cambios en la base de datos</span><span class="sxs-lookup"><span data-stu-id="270b7-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="270b7-205">Ejecute el **Update-Database** comando para aplicar la nueva migración a la base de datos</span><span class="sxs-lookup"><span data-stu-id="270b7-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="270b7-206">La nueva tabla se agrega ahora a la base de datos:</span><span class="sxs-lookup"><span data-stu-id="270b7-206">The new table is now added to the database:</span></span>

![Esquema con los usuarios](~/ef6/media/schemawithusers.png)

<span data-ttu-id="270b7-208">La lista completa de anotaciones compatibles con EF es:</span><span class="sxs-lookup"><span data-stu-id="270b7-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="270b7-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="270b7-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="270b7-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="270b7-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="270b7-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="270b7-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="270b7-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="270b7-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="270b7-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="270b7-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="270b7-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="270b7-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="270b7-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="270b7-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="270b7-222">7. API fluida</span><span class="sxs-lookup"><span data-stu-id="270b7-222">7. Fluent API</span></span>

<span data-ttu-id="270b7-223">En la sección anterior, analizamos usando anotaciones de datos para complementar o reemplazar lo que se detectó por convención.</span><span class="sxs-lookup"><span data-stu-id="270b7-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="270b7-224">La otra forma de configurar el modelo es a través de la API fluida de Code First.</span><span class="sxs-lookup"><span data-stu-id="270b7-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="270b7-225">La mayoría de modelo de configuración se puede realizar mediante las anotaciones de datos simple.</span><span class="sxs-lookup"><span data-stu-id="270b7-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="270b7-226">La API fluida es una manera de especificar la configuración del modelo que cubre todo lo que las anotaciones de datos pueden hacer además a alguna configuración más avanzada que no son posible con las anotaciones de datos más avanzada.</span><span class="sxs-lookup"><span data-stu-id="270b7-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="270b7-227">Las anotaciones de datos y la API fluida se pueden usar conjuntamente.</span><span class="sxs-lookup"><span data-stu-id="270b7-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="270b7-228">Para obtener acceso a la API fluida de invalidar el método OnModelCreating en DbContext.</span><span class="sxs-lookup"><span data-stu-id="270b7-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="270b7-229">Supongamos que deseamos cambiar el nombre de la columna que User.DisplayName se almacena en Mostrar\_nombre.</span><span class="sxs-lookup"><span data-stu-id="270b7-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="270b7-230">Invalide el método OnModelCreating en BloggingContext con el código siguiente</span><span class="sxs-lookup"><span data-stu-id="270b7-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   <span data-ttu-id="270b7-231">Use la **Add-Migration ChangeDisplayName** comando para aplicar scaffolding a una migración para aplicar estos cambios en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="270b7-232">Ejecute el **Update-Database** comando para aplicar la nueva migración a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="270b7-233">La columna DisplayName ahora se cambia el nombre para mostrar\_nombre:</span><span class="sxs-lookup"><span data-stu-id="270b7-233">The DisplayName column is now renamed to display\_name:</span></span>

![Esquema con nombre cambiado el nombre para mostrar](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="270b7-235">Resumen</span><span class="sxs-lookup"><span data-stu-id="270b7-235">Summary</span></span>

<span data-ttu-id="270b7-236">En este tutorial analizamos desarrollo Code First con una base de datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="270b7-237">Se ha definido un modelo mediante clases, a continuación, usa ese modelo para crear una base de datos y almacenar y recuperar datos.</span><span class="sxs-lookup"><span data-stu-id="270b7-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="270b7-238">Una vez que se creó la base de datos se usan migraciones de Code First para cambiar el esquema como nuestro modelo evolucionado.</span><span class="sxs-lookup"><span data-stu-id="270b7-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="270b7-239">También hemos visto cómo configurar un modelo mediante las anotaciones de datos y la API Fluent.</span><span class="sxs-lookup"><span data-stu-id="270b7-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
