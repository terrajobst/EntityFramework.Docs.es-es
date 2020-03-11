---
title: 'Code First a una base de datos existente: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 0a51f826422d7e2bff33b968605eace1e754c425
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415602"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="6072f-102">Code First a una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="6072f-102">Code First to an Existing Database</span></span>
<span data-ttu-id="6072f-103">Este vídeo y el tutorial paso a paso proporcionan una introducción al desarrollo de Code First que tiene como destino una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="6072f-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="6072f-104">Code First permite definir el modelo mediante clases de C\# o VB.Net.</span><span class="sxs-lookup"><span data-stu-id="6072f-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="6072f-105">Opcionalmente, se puede realizar una configuración adicional mediante atributos en las clases y propiedades o mediante una API fluida.</span><span class="sxs-lookup"><span data-stu-id="6072f-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="6072f-106">Visualización del vídeo</span><span class="sxs-lookup"><span data-stu-id="6072f-106">Watch the video</span></span>
<span data-ttu-id="6072f-107">Este vídeo [ahora está disponible en Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="6072f-107">This video is [now available on Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="6072f-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="6072f-108">Pre-Requisites</span></span>

<span data-ttu-id="6072f-109">Para completar este tutorial, necesitará tener **Visual Studio 2012** o **Visual Studio 2013** instalado.</span><span class="sxs-lookup"><span data-stu-id="6072f-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="6072f-110">También necesitará la versión **6,1** (o posterior) de la **Entity Framework Tools para Visual Studio** instalado.</span><span class="sxs-lookup"><span data-stu-id="6072f-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="6072f-111">Consulte [obtener Entity Framework](~/ef6/fundamentals/install.md) para obtener información sobre la instalación de la versión más reciente del Entity Framework Tools.</span><span class="sxs-lookup"><span data-stu-id="6072f-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="6072f-112">1. crear una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="6072f-112">1. Create an Existing Database</span></span>

<span data-ttu-id="6072f-113">Normalmente, cuando el destino es una base de datos existente, ya se creará, pero para este tutorial es necesario crear una base de datos para tener acceso a.</span><span class="sxs-lookup"><span data-stu-id="6072f-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="6072f-114">Vamos a generar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6072f-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="6072f-115">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6072f-115">Open Visual Studio</span></span>
-   <span data-ttu-id="6072f-116">**Explorador de servidores de&gt; de vista**</span><span class="sxs-lookup"><span data-stu-id="6072f-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="6072f-117">Haga clic con el botón derecho en **conexiones de datos:&gt; agregar conexión...**</span><span class="sxs-lookup"><span data-stu-id="6072f-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="6072f-118">Si no se ha conectado a una base de datos desde **Explorador de servidores** antes de que tenga que seleccionar **Microsoft SQL Server** como origen de datos</span><span class="sxs-lookup"><span data-stu-id="6072f-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Seleccionar origen de datos](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="6072f-120">Conéctese a la instancia de LocalDB y escriba **blog** como nombre de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6072f-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![Conexión de LocalDB](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="6072f-122">Seleccione **Aceptar** y se le preguntará si desea crear una nueva base de datos, seleccione **sí** .</span><span class="sxs-lookup"><span data-stu-id="6072f-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Cuadro de diálogo crear base de datos](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="6072f-124">La nueva base de datos aparecerá ahora en Explorador de servidores, haga clic con el botón derecho en ella y seleccione **nueva consulta** .</span><span class="sxs-lookup"><span data-stu-id="6072f-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="6072f-125">Copie el siguiente código SQL en la nueva consulta, haga clic con el botón derecho en la consulta y seleccione **Ejecutar** .</span><span class="sxs-lookup"><span data-stu-id="6072f-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="6072f-126">2. crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="6072f-126">2. Create the Application</span></span>

<span data-ttu-id="6072f-127">Para simplificar las cosas, crearemos una aplicación de consola básica que use Code First para realizar el acceso a los datos:</span><span class="sxs-lookup"><span data-stu-id="6072f-127">To keep things simple we will build a basic console application that uses Code First to do the data access:</span></span>

-   <span data-ttu-id="6072f-128">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6072f-128">Open Visual Studio</span></span>
-   <span data-ttu-id="6072f-129">**Archivo-&gt; nuevo proyecto de&gt;...**</span><span class="sxs-lookup"><span data-stu-id="6072f-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="6072f-130">Seleccionar **ventanas** en el menú izquierdo y en la **aplicación de consola**</span><span class="sxs-lookup"><span data-stu-id="6072f-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="6072f-131">Escriba **CodeFirstExistingDatabaseSample** como nombre</span><span class="sxs-lookup"><span data-stu-id="6072f-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="6072f-132">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="6072f-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="6072f-133">3. modelo de ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="6072f-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="6072f-134">Usaremos el Entity Framework Tools para Visual Studio, que nos ayudará a generar código inicial para asignarlo a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6072f-134">We will use the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="6072f-135">Estas herramientas solo generan código que también podría escribir manualmente si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="6072f-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="6072f-136">**Proyecto-&gt; agregar nuevo elemento...**</span><span class="sxs-lookup"><span data-stu-id="6072f-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="6072f-137">Seleccione **datos** en el menú de la izquierda y, a continuación, **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="6072f-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="6072f-138">Escriba **BloggingContext** como nombre y haga clic en **Aceptar** .</span><span class="sxs-lookup"><span data-stu-id="6072f-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="6072f-139">Se iniciará el **Asistente para Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="6072f-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="6072f-140">Seleccione **code First desde la base de datos** y haga clic en **siguiente** .</span><span class="sxs-lookup"><span data-stu-id="6072f-140">Select **Code First from Database** and click **Next**</span></span>

    ![Asistente One CFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="6072f-142">Seleccione la conexión a la base de datos creada en la primera sección y haga clic en **siguiente** .</span><span class="sxs-lookup"><span data-stu-id="6072f-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![Asistente dos CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="6072f-144">Haga clic en la casilla situada junto a **tablas** para importar todas las tablas y haga clic en **Finalizar** .</span><span class="sxs-lookup"><span data-stu-id="6072f-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![Asistente tres CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="6072f-146">Una vez que se complete el proceso de ingeniería inversa, se agregará al proyecto un número de elementos, echemos un vistazo a lo que se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="6072f-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="6072f-147">Archivo de configuración</span><span class="sxs-lookup"><span data-stu-id="6072f-147">Configuration file</span></span>

<span data-ttu-id="6072f-148">Se ha agregado un archivo app. config al proyecto, este archivo contiene la cadena de conexión a la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="6072f-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="6072f-149">*Observará también algunas otras opciones del archivo de configuración, que son valores de EF predeterminados que indican Code First dónde crear las bases de datos. Dado que estamos asignando a una base de datos existente, esta configuración se omitirá en nuestra aplicación.*</span><span class="sxs-lookup"><span data-stu-id="6072f-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="6072f-150">Contexto derivado</span><span class="sxs-lookup"><span data-stu-id="6072f-150">Derived Context</span></span>

<span data-ttu-id="6072f-151">Se ha agregado una clase **BloggingContext** al proyecto.</span><span class="sxs-lookup"><span data-stu-id="6072f-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="6072f-152">El contexto representa una sesión con la base de datos, lo que nos permite consultar y guardar datos.</span><span class="sxs-lookup"><span data-stu-id="6072f-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="6072f-153">El contexto expone un **&gt;de&lt;de DbSet** para cada tipo de nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="6072f-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="6072f-154">También observará que el constructor predeterminado llama a un constructor base mediante la sintaxis **Name =** .</span><span class="sxs-lookup"><span data-stu-id="6072f-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="6072f-155">Esto indica Code First que la cadena de conexión que se va a utilizar para este contexto se debe cargar desde el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="6072f-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

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

<span data-ttu-id="6072f-156">*Siempre debe usar la sintaxis **Name =** cuando se usa una cadena de conexión en el archivo de configuración. Esto garantiza que, si la cadena de conexión no está presente, se producirá Entity Framework en lugar de crear una nueva base de datos por Convención.*</span><span class="sxs-lookup"><span data-stu-id="6072f-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="6072f-157">Clases de modelo</span><span class="sxs-lookup"><span data-stu-id="6072f-157">Model classes</span></span>

<span data-ttu-id="6072f-158">Por último, también se han agregado al proyecto una clase de **blog** y **publicación** .</span><span class="sxs-lookup"><span data-stu-id="6072f-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="6072f-159">Estas son las clases de dominio que componen nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="6072f-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="6072f-160">Verá que las anotaciones de datos se aplican a las clases para especificar la configuración en la que las convenciones de Code First no se alinearán con la estructura de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="6072f-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="6072f-161">Por ejemplo, verá la anotación **StringLength** en **blog.Name** y **blog. URL** , ya que tienen una longitud máxima de **200** en la base de datos (la Code First predeterminada es usar la longitud máximo que admite el proveedor de base de datos- **nvarchar (Max)** en SQL Server).</span><span class="sxs-lookup"><span data-stu-id="6072f-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

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

## <a name="4-reading--writing-data"></a><span data-ttu-id="6072f-162">4. leer & escribir datos</span><span class="sxs-lookup"><span data-stu-id="6072f-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="6072f-163">Ahora que tenemos un modelo, es el momento de usarlo para tener acceso a algunos datos.</span><span class="sxs-lookup"><span data-stu-id="6072f-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="6072f-164">Implemente el método **Main** en **Program.CS** como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="6072f-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="6072f-165">Este código crea una nueva instancia de nuestro contexto y, a continuación, la usa para insertar un nuevo **blog**.</span><span class="sxs-lookup"><span data-stu-id="6072f-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="6072f-166">A continuación, usa una consulta LINQ para recuperar todos los **blogs** de la base de datos ordenados alfabéticamente por **título**.</span><span class="sxs-lookup"><span data-stu-id="6072f-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="6072f-167">Ahora puede ejecutar la aplicación y probarla.</span><span class="sxs-lookup"><span data-stu-id="6072f-167">You can now run the application and test it.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="6072f-168">¿Qué ocurre si mi base de datos cambia?</span><span class="sxs-lookup"><span data-stu-id="6072f-168">What if My Database Changes?</span></span>

<span data-ttu-id="6072f-169">El Asistente para Code First a base de datos está diseñado para generar un conjunto de puntos de partida de clases que se pueden retocar y modificar.</span><span class="sxs-lookup"><span data-stu-id="6072f-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="6072f-170">Si cambia el esquema de la base de datos, puede editar manualmente las clases o realizar otro ingeniero inverso para sobrescribir las clases.</span><span class="sxs-lookup"><span data-stu-id="6072f-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="6072f-171">Usar Migraciones de Code First en una base de datos existente</span><span class="sxs-lookup"><span data-stu-id="6072f-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="6072f-172">Si desea usar Migraciones de Code First con una base de datos existente, vea [migraciones de Code First a una base de datos existente](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="6072f-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="6072f-173">Resumen</span><span class="sxs-lookup"><span data-stu-id="6072f-173">Summary</span></span>

<span data-ttu-id="6072f-174">En este tutorial, hemos examinado Code First desarrollo con una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="6072f-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="6072f-175">Usamos el Entity Framework Tools de Visual Studio para aplicar ingeniería inversa a un conjunto de clases que se asignan a la base de datos y que podrían usarse para almacenar y recuperar datos.</span><span class="sxs-lookup"><span data-stu-id="6072f-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
