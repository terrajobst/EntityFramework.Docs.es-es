---
title: Async consultar y guardar - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 4ed4f5c13341f33ccff8325a5ddacd8f7b195a76
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283828"
---
# <a name="async-query-and-save"></a><span data-ttu-id="dc3e6-102">Async consultar y guardar</span><span class="sxs-lookup"><span data-stu-id="dc3e6-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="dc3e6-103">**Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="dc3e6-104">Si usa una versión anterior, no se aplica parte o la totalidad de la información.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="dc3e6-105">EF6 introdujo la compatibilidad para consulta asincrónica y guárdelo con el [async y await palabras clave](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) que se introdujeron en .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="dc3e6-106">Aunque no todas las aplicaciones pueden beneficiarse de la asincronía, puede usar para mejorar la escalabilidad de la capacidad de respuesta y el servidor de cliente cuando se administran las tareas dependientes de E/s, red o larga ejecución.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="dc3e6-107">Cuándo se debe realmente usar async</span><span class="sxs-lookup"><span data-stu-id="dc3e6-107">When to really use async</span></span>

<span data-ttu-id="dc3e6-108">El propósito de este tutorial es introducir los conceptos de async en una forma que resulte fácil de observar la diferencia entre la ejecución del programa sincrónicas y asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="dc3e6-109">En este tutorial no está pensado para mostrar cualquiera de los escenarios clave donde la programación asincrónica proporciona ventajas.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="dc3e6-110">Programación asincrónica se centra principalmente en lo que libera el subproceso administrado actual (código de .NET ejecución de subprocesos) para realizar otro trabajo mientras se espera una operación que no requiere ningún tiempo de proceso desde un subproceso administrado.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="dc3e6-111">Por ejemplo, mientras que el motor de base de datos está procesando una consulta no hay nada que se realiza mediante código de .NET.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="dc3e6-112">En las aplicaciones cliente (WinForms, WPF, etc.) puede utilizarse el subproceso actual para que la interfaz de usuario siga respondiendo mientras se realiza la operación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="dc3e6-113">En aplicaciones de servidor (ASP.NET, etc.) se puede usar el subproceso para procesar otras solicitudes entrantes: Esto puede reducir el uso de memoria o aumentar el rendimiento del servidor.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="dc3e6-114">En la mayoría de las aplicaciones usar async no tendrán ninguna ventaja apreciable e incluso podría ser perjudicial.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="dc3e6-115">Use las pruebas, la generación de perfiles y sentido común para medir el impacto de async en su escenario concreto antes de confirmar.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="dc3e6-116">Estos son algunos recursos más que aprender sobre async:</span><span class="sxs-lookup"><span data-stu-id="dc3e6-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="dc3e6-117">Información general de Brandon Bray de async y await en .NET 4.5</span><span class="sxs-lookup"><span data-stu-id="dc3e6-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="dc3e6-118">[Programación asincrónica](https://msdn.microsoft.com/library/hh191443.aspx) páginas en la biblioteca de MSDN</span><span class="sxs-lookup"><span data-stu-id="dc3e6-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="dc3e6-119">[Cómo crear aplicaciones Web ASP.NET utilizando Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (incluye una demostración de rendimiento del servidor aumento de)</span><span class="sxs-lookup"><span data-stu-id="dc3e6-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="dc3e6-120">Creación del modelo</span><span class="sxs-lookup"><span data-stu-id="dc3e6-120">Create the model</span></span>

<span data-ttu-id="dc3e6-121">Vamos a usar el [flujo de trabajo Code First](~/ef6/modeling/code-first/workflows/new-database.md) para crear nuestro modelo y generar la base de datos, sin embargo, la funcionalidad asincrónica funcionará con todos los modelos EF, incluidos aquellos creados con EF Designer.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="dc3e6-122">Crear una aplicación de consola y llámelo **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="dc3e6-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="dc3e6-123">Agregue el paquete EntityFramework NuGet</span><span class="sxs-lookup"><span data-stu-id="dc3e6-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="dc3e6-124">En el Explorador de soluciones, haga doble clic en el **AsyncDemo** proyecto</span><span class="sxs-lookup"><span data-stu-id="dc3e6-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="dc3e6-125">Seleccione **administrar paquetes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="dc3e6-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="dc3e6-126">En el cuadro de diálogo Administrar paquetes de NuGet, seleccione el **Online** pestaña y elija el **EntityFramework** paquete</span><span class="sxs-lookup"><span data-stu-id="dc3e6-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="dc3e6-127">Haga clic en **instalar**</span><span class="sxs-lookup"><span data-stu-id="dc3e6-127">Click **Install**</span></span>
-   <span data-ttu-id="dc3e6-128">Agregar un **Model.cs** clase con la siguiente implementación</span><span class="sxs-lookup"><span data-stu-id="dc3e6-128">Add a **Model.cs** class with the following implementation</span></span>

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
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
    }
```

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="dc3e6-129">Crear un programa sincrónico</span><span class="sxs-lookup"><span data-stu-id="dc3e6-129">Create a synchronous program</span></span>

<span data-ttu-id="dc3e6-130">Ahora que tenemos un modelo de EF, vamos a escribir algún código que lo utiliza para realizar algún acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="dc3e6-131">Reemplace el contenido de **Program.cs** con el código siguiente</span><span class="sxs-lookup"><span data-stu-id="dc3e6-131">Replace the contents of **Program.cs** with the following code</span></span>

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine();
                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    db.SaveChanges();

                    // Query for all blogs ordered by name
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine();
                    Console.WriteLine("All blogs:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="dc3e6-132">Este código llama a la **PerformDatabaseOperations** método que se guarda un nuevo **Blog** a la base de datos y, a continuación, recupera todos los **Blogs** desde la base de datos y los imprime en la **Consola**.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="dc3e6-133">Una vez hecho esto, el programa escribe una cita del día a la **consola**.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="dc3e6-134">Puesto que el código es sincrónico, nos podemos observar el siguiente flujo de ejecución cuando se ejecuta el programa:</span><span class="sxs-lookup"><span data-stu-id="dc3e6-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="dc3e6-135">**SaveChanges** comienza a insertar el nuevo **Blog** a la base de datos</span><span class="sxs-lookup"><span data-stu-id="dc3e6-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="dc3e6-136">**SaveChanges** completa</span><span class="sxs-lookup"><span data-stu-id="dc3e6-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="dc3e6-137">Consulta para todos los **Blogs** se envía a la base de datos</span><span class="sxs-lookup"><span data-stu-id="dc3e6-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="dc3e6-138">Devuelve la consulta y los resultados se escriben en **consola**</span><span class="sxs-lookup"><span data-stu-id="dc3e6-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="dc3e6-139">Cita del día se escribe en **consola**</span><span class="sxs-lookup"><span data-stu-id="dc3e6-139">Quote of the day is written to **Console**</span></span>

![Salida de sincronización](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="dc3e6-141">Convertirla en asincrónica</span><span class="sxs-lookup"><span data-stu-id="dc3e6-141">Making it asynchronous</span></span>

<span data-ttu-id="dc3e6-142">Ahora que tenemos nuestro programa ponerse en marcha, podremos hacer uso del nuevas async y await palabras clave.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="dc3e6-143">Nos hemos realizado los cambios siguientes en Program.cs</span><span class="sxs-lookup"><span data-stu-id="dc3e6-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="dc3e6-144">La línea 2: El uso de la instrucción para el **System.Data.Entity** espacio de nombres nos da acceso a los métodos de extensión de async EF.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="dc3e6-145">La línea 4: El uso de la instrucción para el **System.Threading.Tasks** espacio de nombres nos permite utilizar el **tarea** tipo.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="dc3e6-146">Línea 12 y 18: vamos a capturar como tarea que supervisa el progreso de **PerformSomeDatabaseOperations** (línea 12) y, a continuación, bloquear la ejecución de programa para esta tarea a una vez completada todo el trabajo para el programa se realiza (línea 18).</span><span class="sxs-lookup"><span data-stu-id="dc3e6-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="dc3e6-147">Línea 25: Hemos actualización **PerformSomeDatabaseOperations** se marquen como **async** y devolver un **tarea**.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="dc3e6-148">Línea 35: Ahora estamos llamando a la versión asincrónica de SaveChanges y espera su finalización.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="dc3e6-149">Línea 42: Ahora estamos llamando a la versión asincrónica de ToList y se espera en el resultado.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-149">Line 42: We're now calling hte Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="dc3e6-150">Para obtener una lista completa de métodos de extensión disponibles en el espacio de nombres System.Data.Entity, consulte la clase QueryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="dc3e6-151">*También deberá agregar "using System.Data.Entity" para el uso de instrucciones.*</span><span class="sxs-lookup"><span data-stu-id="dc3e6-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="dc3e6-152">Ahora que el código es asincrónica, nos podemos observar un flujo de ejecución diferente cuando se ejecuta el programa:</span><span class="sxs-lookup"><span data-stu-id="dc3e6-152">Now that the code is asyncronous, we can observe a different execution flow when we run the program:</span></span>

1.  <span data-ttu-id="dc3e6-153">**SaveChanges** comienza a insertar el nuevo **Blog** a la base de datos *una vez que el comando se envía a la base de datos no más de proceso se necesita tiempo en el subproceso administrado actual. El **PerformDatabaseOperations** método devuelve (incluso si no ha terminado de ejecutarse) y continúa el flujo del programa en el método Main.*</span><span class="sxs-lookup"><span data-stu-id="dc3e6-153">**SaveChanges** begins to push the new **Blog** to the database *Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2.  <span data-ttu-id="dc3e6-154">**Cita del día se escribe en la consola**
    \*ya que no hay nada más trabajo que hacer en el método Main, se bloquea el subproceso administrado en la espera llamar hasta que se complete la operación de base de datos. Cuando haya terminado, el resto de nuestro **PerformDatabaseOperations** \* se ejecutará.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-154">**Quote of the day is written to Console**
*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations*** will be executed.</span></span>
3.  <span data-ttu-id="dc3e6-155">**SaveChanges** completa</span><span class="sxs-lookup"><span data-stu-id="dc3e6-155">**SaveChanges** completes</span></span>
4.  <span data-ttu-id="dc3e6-156">Consulta para todos los **Blogs** se envía a la base de datos *de nuevo, el subproceso administrado es libre para realizar otro trabajo mientras se procesa la consulta en la base de datos. Puesto que se haya completado toda la ejecución de otra, el subproceso simplemente detendrá en la llamada espera aunque.*</span><span class="sxs-lookup"><span data-stu-id="dc3e6-156">Query for all **Blogs** is sent to the database *Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="dc3e6-157">Devuelve la consulta y los resultados se escriben en **consola**</span><span class="sxs-lookup"><span data-stu-id="dc3e6-157">Query returns and results are written to **Console**</span></span>

![Salida de Async](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="dc3e6-159">La conclusión</span><span class="sxs-lookup"><span data-stu-id="dc3e6-159">The takeaway</span></span>

<span data-ttu-id="dc3e6-160">Ahora hemos visto lo fácil que es hacer uso de métodos asincrónicos de EF.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-160">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="dc3e6-161">Aunque las ventajas de async no pueden ser muy evidentes con una aplicación de consola simple, estas mismas estrategias se pueden aplicar en situaciones en las actividades relacionadas con la red o de larga ejecución podrían en caso contrario, bloquea la aplicación, o provocar un gran número de subprocesos aumentar el consumo de memoria.</span><span class="sxs-lookup"><span data-stu-id="dc3e6-161">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
