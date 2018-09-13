---
title: La base de datos en primer lugar - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: b499dea02cbeaa64f6ef87bf89cc739110c8b560
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490927"
---
# <a name="database-first"></a><span data-ttu-id="6f449-102">En primer lugar la base de datos</span><span class="sxs-lookup"><span data-stu-id="6f449-102">Database First</span></span>
<span data-ttu-id="6f449-103">En este tutorial de vídeo y paso a paso proporcionan una introducción al desarrollo de primera base de datos mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6f449-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="6f449-104">Base de datos en primer lugar le permite aplicar ingeniería inversa a un modelo a partir de una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="6f449-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="6f449-105">El modelo se almacena en un archivo EDMX (extensión edmx) y se puede ver y editar en el Diseñador de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6f449-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="6f449-106">Las clases que interactúan en la aplicación se generan automáticamente desde el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="6f449-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="6f449-107">Vea el vídeo</span><span class="sxs-lookup"><span data-stu-id="6f449-107">Watch the video</span></span>
<span data-ttu-id="6f449-108">Este vídeo ofrece una introducción al desarrollo de primera base de datos mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6f449-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="6f449-109">Base de datos en primer lugar le permite aplicar ingeniería inversa a un modelo a partir de una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="6f449-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="6f449-110">El modelo se almacena en un archivo EDMX (extensión edmx) y se puede ver y editar en el Diseñador de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6f449-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="6f449-111">Las clases que interactúan en la aplicación se generan automáticamente desde el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="6f449-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="6f449-112">**Presentado por**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="6f449-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="6f449-113">**Vídeo**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="6f449-113">**Video**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="6f449-114">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="6f449-114">Pre-Requisites</span></span>

<span data-ttu-id="6f449-115">Debe tener al menos Visual Studio 2010 o Visual Studio 2012 instalado para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="6f449-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="6f449-116">Si utiliza Visual Studio 2010, también necesitará tener [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.</span><span class="sxs-lookup"><span data-stu-id="6f449-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="6f449-117">1. Crear una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="6f449-117">1. Create an Existing Database</span></span>

<span data-ttu-id="6f449-118">Normalmente, cuando tiene como destino una base de datos existente que ya se creará, pero para este tutorial es necesario crear una base de datos para tener acceso a.</span><span class="sxs-lookup"><span data-stu-id="6f449-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="6f449-119">El servidor de base de datos que se instala con Visual Studio es diferente según la versión de Visual Studio que ha instalado:</span><span class="sxs-lookup"><span data-stu-id="6f449-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="6f449-120">Si usa Visual Studio 2010 se creará una base de datos de SQL Express.</span><span class="sxs-lookup"><span data-stu-id="6f449-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="6f449-121">Si usa Visual Studio 2012, a continuación, se creará un [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f449-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="6f449-122">Sigamos adelante y generar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f449-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="6f449-123">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f449-123">Open Visual Studio</span></span>
-   <span data-ttu-id="6f449-124">**Vista -&gt; Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="6f449-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="6f449-125">Haga clic con el botón derecho en **conexiones de datos -&gt; Agregar conexión...**</span><span class="sxs-lookup"><span data-stu-id="6f449-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="6f449-126">Si aún no lo ha conectado a una base de datos del explorador de servidores antes de que tendrá que seleccionar Microsoft SQL Server como origen de datos</span><span class="sxs-lookup"><span data-stu-id="6f449-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Seleccionar origen de datos](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="6f449-128">Conectarse a LocalDB o Express de SQL, según lo que se ha instalado y escriba **DatabaseFirst.Blogging** como el nombre de la base de datos</span><span class="sxs-lookup"><span data-stu-id="6f449-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![Conexión de SQL Express DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![Conexión de LocalDB DF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="6f449-131">Seleccione **Aceptar** y se le solicitará si desea crear una nueva base de datos, seleccione **sí**</span><span class="sxs-lookup"><span data-stu-id="6f449-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Crear el cuadro de diálogo de base de datos](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="6f449-133">La nueva base de datos aparecerá ahora en el Explorador de servidores, haga doble clic en él y seleccione **nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="6f449-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="6f449-134">Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **Execute**</span><span class="sxs-lookup"><span data-stu-id="6f449-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);
```

## <a name="2-create-the-application"></a><span data-ttu-id="6f449-135">2. Crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="6f449-135">2. Create the Application</span></span>

<span data-ttu-id="6f449-136">Para simplificar las cosas que vamos a crear una aplicación de consola básica que usa la primera base de datos para tener acceso a datos:</span><span class="sxs-lookup"><span data-stu-id="6f449-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="6f449-137">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f449-137">Open Visual Studio</span></span>
-   <span data-ttu-id="6f449-138">**Archivo -&gt; nuevo:&gt; proyecto...**</span><span class="sxs-lookup"><span data-stu-id="6f449-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="6f449-139">Seleccione **Windows** en el menú izquierdo y **aplicación de consola**</span><span class="sxs-lookup"><span data-stu-id="6f449-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="6f449-140">Escriba **DatabaseFirstSample** como el nombre</span><span class="sxs-lookup"><span data-stu-id="6f449-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="6f449-141">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6f449-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="6f449-142">3. Modelo de ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="6f449-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="6f449-143">Vamos a usar Entity Framework Designer, que se incluye como parte de Visual Studio, para crear nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="6f449-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="6f449-144">**Proyecto -&gt; Agregar nuevo elemento...**</span><span class="sxs-lookup"><span data-stu-id="6f449-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="6f449-145">Seleccione **datos** en el menú izquierdo y, a continuación, **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="6f449-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="6f449-146">Escriba **BloggingModel** como el nombre y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="6f449-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="6f449-147">Esto inicia el **Asistente para Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="6f449-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="6f449-148">Seleccione **generar desde la base de datos** y haga clic en **siguiente**</span><span class="sxs-lookup"><span data-stu-id="6f449-148">Select **Generate from Database** and click **Next**</span></span>

    ![Paso 1 del Asistente](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="6f449-150">Seleccione la conexión a la base de datos que creó en la primera sección, escriba **BloggingContext** como el nombre de la cadena de conexión y haga clic en **siguiente**</span><span class="sxs-lookup"><span data-stu-id="6f449-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![Paso 2 del Asistente](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="6f449-152">Haga clic en la casilla de verificación situada junto a 'Tablas' para importar todas las tablas y haga clic en 'Finalizar'</span><span class="sxs-lookup"><span data-stu-id="6f449-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Paso 3 del Asistente](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="6f449-154">Una vez completado el proceso de ingeniería inversa el nuevo modelo se agrega al proyecto y se abrirá para que pueda ver en el Diseñador de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6f449-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="6f449-155">También se agregó un archivo App.config al proyecto con los detalles de conexión para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f449-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![Modelo inicial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="6f449-157">Pasos adicionales en Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="6f449-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="6f449-158">Si está trabajando en Visual Studio 2010, hay algunos pasos adicionales que debe seguir para actualizar a la versión más reciente de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6f449-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="6f449-159">Actualizar es importante porque proporciona acceso a una superficie de API mejorada, que es mucho más fácil de usar, así como las correcciones de errores más recientes.</span><span class="sxs-lookup"><span data-stu-id="6f449-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="6f449-160">En primer lugar, necesitamos obtener la versión más reciente de Entity Framework en NuGet.</span><span class="sxs-lookup"><span data-stu-id="6f449-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="6f449-161">\*\*Proyecto –&gt; administrar paquetes NuGet... \*\* 
     \*Si no tiene el \**administrar paquetes NuGet... \*\* opción, debe instalar la [versión más reciente de NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="6f449-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="6f449-162">Seleccione el **Online** ficha</span><span class="sxs-lookup"><span data-stu-id="6f449-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="6f449-163">Seleccione el **EntityFramework** paquete</span><span class="sxs-lookup"><span data-stu-id="6f449-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="6f449-164">Haga clic en **instalar**</span><span class="sxs-lookup"><span data-stu-id="6f449-164">Click **Install**</span></span>

<span data-ttu-id="6f449-165">A continuación, se debe intercambiar nuestro modelo para generar código que hace uso de la API de DbContext, que se introdujo en versiones posteriores de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6f449-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="6f449-166">Haga doble clic en una zona vacía del modelo de EF Designer y seleccione **Add Code Generation Item...**</span><span class="sxs-lookup"><span data-stu-id="6f449-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="6f449-167">Seleccione **plantillas en línea** desde el menú izquierdo y busque **DbContext**</span><span class="sxs-lookup"><span data-stu-id="6f449-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="6f449-168">Seleccione el EF **5.x generador de DbContext para C\#**, escriba **BloggingModel** como el nombre y haga clic en **agregar**</span><span class="sxs-lookup"><span data-stu-id="6f449-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Plantilla de DbContext](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="6f449-170">4. Leer y escribir datos</span><span class="sxs-lookup"><span data-stu-id="6f449-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="6f449-171">Ahora que tenemos un modelo es el momento de usarlo para acceder a algunos datos.</span><span class="sxs-lookup"><span data-stu-id="6f449-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="6f449-172">Las clases que vamos a usar para acceder a los datos se generan automáticamente para usted según el archivo EDMX.</span><span class="sxs-lookup"><span data-stu-id="6f449-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="6f449-173">*Esta captura de pantalla es de Visual Studio 2012, si usa Visual Studio 2010 el BloggingModel.tt y BloggingModel.Context.tt archivos estarán directamente en el proyecto en lugar de anidan en el archivo EDMX.*</span><span class="sxs-lookup"><span data-stu-id="6f449-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Clases generadas DF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="6f449-175">Como se muestra a continuación, implemente el método Main en Program.cs.</span><span class="sxs-lookup"><span data-stu-id="6f449-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="6f449-176">Este código crea una nueva instancia de nuestro contexto y, a continuación, usa para insertar un nuevo Blog.</span><span class="sxs-lookup"><span data-stu-id="6f449-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="6f449-177">A continuación, usa una consulta LINQ para recuperar todos los Blogs de la base de datos ordenado alfabéticamente por título.</span><span class="sxs-lookup"><span data-stu-id="6f449-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="6f449-178">Ahora puede ejecutar la aplicación y probarlo.</span><span class="sxs-lookup"><span data-stu-id="6f449-178">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="6f449-179">5. Trabajar con los cambios de la base de datos</span><span class="sxs-lookup"><span data-stu-id="6f449-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="6f449-180">Ahora es momento de realizar algunos cambios en el esquema de base de datos, cuando se realizan estos cambios que también se debe actualizar nuestro modelo para reflejar dichos cambios.</span><span class="sxs-lookup"><span data-stu-id="6f449-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="6f449-181">El primer paso es realizar algunos cambios en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f449-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="6f449-182">Vamos a agregar una tabla de usuarios en el esquema.</span><span class="sxs-lookup"><span data-stu-id="6f449-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="6f449-183">Haga doble clic en el **DatabaseFirst.Blogging** en el Explorador de servidores de base de datos y seleccione **nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="6f449-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="6f449-184">Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **Execute**</span><span class="sxs-lookup"><span data-stu-id="6f449-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="6f449-185">Ahora que se actualiza el esquema, es momento de actualizar el modelo con esos cambios.</span><span class="sxs-lookup"><span data-stu-id="6f449-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="6f449-186">Haga doble clic en una zona vacía de su modelo de EF Designer y seleccione "Actualizar modelo desde base de datos...', se iniciará el Asistente para actualizar</span><span class="sxs-lookup"><span data-stu-id="6f449-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="6f449-187">En la pestaña agregar de la comprobación del Asistente para actualizar la casilla situada junto a las tablas, esto indica que deseamos agregar una tabla nueva desde el esquema.</span><span class="sxs-lookup"><span data-stu-id="6f449-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="6f449-188">*La pestaña de actualización muestra las tablas existentes en el modelo que se comprobará los cambios durante la actualización. Las pestañas de eliminación muestran todas las tablas que se han quitado del esquema y también se quitará del modelo como parte de la actualización. La información de estos dos fichas se detecta automáticamente y se proporciona con propósito informativo únicamente, no se puede cambiar cualquier configuración.*</span><span class="sxs-lookup"><span data-stu-id="6f449-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![Asistente de actualización](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="6f449-190">Haga clic en Finalizar en el Asistente para actualizar</span><span class="sxs-lookup"><span data-stu-id="6f449-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="6f449-191">El modelo se ha actualizado para incluir una nueva entidad de usuario que se asigna a la tabla de usuarios que se agregan a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f449-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![Modelo actualizado](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="6f449-193">Resumen</span><span class="sxs-lookup"><span data-stu-id="6f449-193">Summary</span></span>

<span data-ttu-id="6f449-194">En este tutorial que analizamos el desarrollo de Database First, que nos permitió crear un modelo en el Diseñador de EF en función de una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="6f449-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="6f449-195">A continuación, se utiliza ese modelo para leer y escribir algunos datos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f449-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="6f449-196">Por último, se actualiza el modelo para reflejar los cambios realizados en el esquema de base de datos.</span><span class="sxs-lookup"><span data-stu-id="6f449-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
