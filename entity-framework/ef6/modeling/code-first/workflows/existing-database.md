---
title: Code First en una base de datos existente - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: fedfb921919582e2cdb5f3bc497f11889b972ad6
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251081"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="94fb9-102">Code First en una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="94fb9-102">Code First to an Existing Database</span></span>
<span data-ttu-id="94fb9-103">En este tutorial de vídeo y paso a paso proporcionan una introducción al desarrollo de Code First destinadas a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="94fb9-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="94fb9-104">Código primero le permite definir su modelo mediante C\# o clases de VB.Net.</span><span class="sxs-lookup"><span data-stu-id="94fb9-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="94fb9-105">Configuración adicional si lo desea puede realizarse mediante los atributos en las clases y propiedades o mediante una API fluida.</span><span class="sxs-lookup"><span data-stu-id="94fb9-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="94fb9-106">Vea el vídeo</span><span class="sxs-lookup"><span data-stu-id="94fb9-106">Watch the video</span></span>
<span data-ttu-id="94fb9-107">Este vídeo es [ya está disponible en Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="94fb9-107">This video is [now available on Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="94fb9-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="94fb9-108">Pre-Requisites</span></span>

<span data-ttu-id="94fb9-109">Deberá tener **Visual Studio 2012** o **Visual Studio 2013** instalado para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="94fb9-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="94fb9-110">También necesitará versión **6.1** (o posterior) de la **Entity Framework Tools para Visual Studio** instalado.</span><span class="sxs-lookup"><span data-stu-id="94fb9-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="94fb9-111">Consulte [obtener Entity Framework](~/ef6/fundamentals/install.md) para obtener información acerca de cómo instalar la versión más reciente de las herramientas de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="94fb9-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="94fb9-112">1. Crear una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="94fb9-112">1. Create an Existing Database</span></span>

<span data-ttu-id="94fb9-113">Normalmente, cuando tiene como destino una base de datos existente que ya se creará, pero para este tutorial es necesario crear una base de datos para tener acceso a.</span><span class="sxs-lookup"><span data-stu-id="94fb9-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="94fb9-114">Sigamos adelante y generar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="94fb9-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="94fb9-115">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94fb9-115">Open Visual Studio</span></span>
-   <span data-ttu-id="94fb9-116">**Vista -&gt; Explorador de servidores**</span><span class="sxs-lookup"><span data-stu-id="94fb9-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="94fb9-117">Haga clic con el botón derecho en **conexiones de datos -&gt; Agregar conexión...**</span><span class="sxs-lookup"><span data-stu-id="94fb9-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="94fb9-118">Si aún no lo ha conectado a una base de datos **Explorador de servidores** antes de que tendrá que seleccionar **Microsoft SQL Server** como origen de datos</span><span class="sxs-lookup"><span data-stu-id="94fb9-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Seleccionar origen de datos](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="94fb9-120">Conectarse a la instancia de LocalDB y escriba **blogs** como el nombre de la base de datos</span><span class="sxs-lookup"><span data-stu-id="94fb9-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![Conexión de LocalDB](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="94fb9-122">Seleccione **Aceptar** y se le solicitará si desea crear una nueva base de datos, seleccione **sí**</span><span class="sxs-lookup"><span data-stu-id="94fb9-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Crear el cuadro de diálogo de base de datos](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="94fb9-124">La nueva base de datos aparecerá ahora en el Explorador de servidores, haga doble clic en él y seleccione **nueva consulta**</span><span class="sxs-lookup"><span data-stu-id="94fb9-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="94fb9-125">Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **Execute**</span><span class="sxs-lookup"><span data-stu-id="94fb9-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a><span data-ttu-id="94fb9-126">2. Crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="94fb9-126">2. Create the Application</span></span>

<span data-ttu-id="94fb9-127">Para simplificar las cosas que vamos a crear una aplicación de consola básica que usa Code First para tener acceso a datos:</span><span class="sxs-lookup"><span data-stu-id="94fb9-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="94fb9-128">Apertura de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="94fb9-128">Open Visual Studio</span></span>
-   <span data-ttu-id="94fb9-129">**Archivo -&gt; nuevo:&gt; proyecto...**</span><span class="sxs-lookup"><span data-stu-id="94fb9-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="94fb9-130">Seleccione **Windows** en el menú izquierdo y **aplicación de consola**</span><span class="sxs-lookup"><span data-stu-id="94fb9-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="94fb9-131">Escriba **CodeFirstExistingDatabaseSample** como el nombre</span><span class="sxs-lookup"><span data-stu-id="94fb9-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="94fb9-132">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="94fb9-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="94fb9-133">3. Modelo de ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="94fb9-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="94fb9-134">Vamos a usar Entity Framework Tools para Visual Studio para ayudarnos a generar código para asignar a la base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="94fb9-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="94fb9-135">Estas herramientas solo genera código que también podría escribir a mano si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="94fb9-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="94fb9-136">**Proyecto -&gt; Agregar nuevo elemento...**</span><span class="sxs-lookup"><span data-stu-id="94fb9-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="94fb9-137">Seleccione **datos** en el menú izquierdo y, a continuación, **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="94fb9-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="94fb9-138">Escriba **BloggingContext** como el nombre y haga clic en **Aceptar**</span><span class="sxs-lookup"><span data-stu-id="94fb9-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="94fb9-139">Esto inicia el **Asistente para Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="94fb9-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="94fb9-140">Seleccione **Code First de base de datos** y haga clic en **siguiente**</span><span class="sxs-lookup"><span data-stu-id="94fb9-140">Select **Code First from Database** and click **Next**</span></span>

    ![Un CFE Asistente](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="94fb9-142">Seleccione la conexión a la base de datos que creó en la primera sección y haga clic en **siguiente**</span><span class="sxs-lookup"><span data-stu-id="94fb9-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![Asistente para dos CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="94fb9-144">Haga clic en la casilla de verificación junto a **tablas** para importar todas las tablas y haga clic en **finalizar**</span><span class="sxs-lookup"><span data-stu-id="94fb9-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![Asistente para tres CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="94fb9-146">Una vez que un número de elementos de completa el proceso de ingeniería inversa habrá agregado al proyecto, vamos a Eche un vistazo a lo que se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="94fb9-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="94fb9-147">Archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="94fb9-147">Configuration file</span></span>

<span data-ttu-id="94fb9-148">Se agregó un archivo App.config al proyecto, este archivo contiene la cadena de conexión a la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="94fb9-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="94fb9-149">*Observará algunos otros valores en el archivo de configuración demasiado, se trata de configuración de EF predeterminada que indican a Code First dónde crear bases de datos. Puesto que vamos a asignar a una base de datos existente, esta configuración se omitirá en nuestra aplicación.*</span><span class="sxs-lookup"><span data-stu-id="94fb9-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="94fb9-150">Contexto derivado</span><span class="sxs-lookup"><span data-stu-id="94fb9-150">Derived Context</span></span>

<span data-ttu-id="94fb9-151">Un **BloggingContext** ha agregado la clase al proyecto.</span><span class="sxs-lookup"><span data-stu-id="94fb9-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="94fb9-152">El contexto representa una sesión con la base de datos, lo que nos permite consultar y guardar los datos.</span><span class="sxs-lookup"><span data-stu-id="94fb9-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="94fb9-153">Expone el contexto de un **DbSet&lt;TEntity&gt;**  para cada tipo de nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="94fb9-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="94fb9-154">También podrá observar que el constructor predeterminado llama a un constructor base con el **nombre =** sintaxis.</span><span class="sxs-lookup"><span data-stu-id="94fb9-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="94fb9-155">Code First indica que la cadena de conexión que se utilizará para este contexto se debe cargar desde el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="94fb9-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

<span data-ttu-id="94fb9-156">*Siempre debe usar el **nombre =** sintaxis cuando se usa una cadena de conexión en el archivo de configuración. Esto garantiza que si la cadena de conexión no está presente, a continuación, Entity Framework arrojará en lugar de crear una nueva base de datos por convención.*</span><span class="sxs-lookup"><span data-stu-id="94fb9-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="94fb9-157">Clases de modelo</span><span class="sxs-lookup"><span data-stu-id="94fb9-157">Model classes</span></span>

<span data-ttu-id="94fb9-158">Por último, un **Blog** y **Post** clase también se han agregado al proyecto.</span><span class="sxs-lookup"><span data-stu-id="94fb9-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="94fb9-159">Estas son las clases de dominio que conforman nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="94fb9-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="94fb9-160">Podrá ver las anotaciones de datos se aplica a las clases para especificar la configuración donde las convenciones de Code First no estaría en consonancia con la estructura de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="94fb9-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="94fb9-161">Por ejemplo, verá el **StringLength** anotación en **Blog.Name** y **Blog.Url** porque tienen una longitud máxima de **200** en el base de datos (el valor de Code First predeterminado consiste en utilizar la longitud máximo admitida por el proveedor de base de datos - **nvarchar (max)** en SQL Server).</span><span class="sxs-lookup"><span data-stu-id="94fb9-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a><span data-ttu-id="94fb9-162">4. Leer y escribir datos</span><span class="sxs-lookup"><span data-stu-id="94fb9-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="94fb9-163">Ahora que tenemos un modelo es el momento de usarlo para acceder a algunos datos.</span><span class="sxs-lookup"><span data-stu-id="94fb9-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="94fb9-164">Implemente el **Main** método **Program.cs** tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="94fb9-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="94fb9-165">Este código crea una nueva instancia de nuestro contexto y, a continuación, se usa para insertar un nuevo **Blog**.</span><span class="sxs-lookup"><span data-stu-id="94fb9-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="94fb9-166">A continuación, usa una consulta LINQ para recuperar todos **Blogs** desde la base de datos ordenado alfabéticamente por **título**.</span><span class="sxs-lookup"><span data-stu-id="94fb9-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="94fb9-167">Ahora puede ejecutar la aplicación y probarlo.</span><span class="sxs-lookup"><span data-stu-id="94fb9-167">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="94fb9-168">¿Qué ocurre si mi base de datos los cambios?</span><span class="sxs-lookup"><span data-stu-id="94fb9-168">What if My Database Changes?</span></span>

<span data-ttu-id="94fb9-169">Code First al Asistente para la base de datos está diseñada para generar un conjunto de punto de partida de las clases que puede, a continuación, ajustar y modificar.</span><span class="sxs-lookup"><span data-stu-id="94fb9-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="94fb9-170">Si cambia el esquema de base de datos puede manualmente editar las clases o realizar otra ingeniería inversa para sobrescribir las clases.</span><span class="sxs-lookup"><span data-stu-id="94fb9-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="94fb9-171">Uso de migraciones de Code First para una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="94fb9-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="94fb9-172">Si desea usar migraciones de Code First con una base de datos existente, vea [migraciones de Code First para una base de datos](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="94fb9-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="94fb9-173">Resumen</span><span class="sxs-lookup"><span data-stu-id="94fb9-173">Summary</span></span>

<span data-ttu-id="94fb9-174">En este tutorial analizamos desarrollo Code First con una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="94fb9-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="94fb9-175">Se usan Entity Framework Tools para Visual Studio para un conjunto de clases que asignan a la base de datos y podría utilizarse para almacenar y recuperar datos de ingeniería inversa.</span><span class="sxs-lookup"><span data-stu-id="94fb9-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
