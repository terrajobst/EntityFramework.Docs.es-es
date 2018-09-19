---
title: Migraciones de Code automática First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283919"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="0830e-102">Migraciones automáticas de Code First</span><span class="sxs-lookup"><span data-stu-id="0830e-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="0830e-103">Las migraciones automáticas permite usar migraciones de Code First sin necesidad de un archivo de código en el proyecto para cada cambio realizado.</span><span class="sxs-lookup"><span data-stu-id="0830e-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="0830e-104">No todos los cambios se pueden aplicar automáticamente: por ejemplo cambia el nombre de columna exige el uso de una migración basada en código.</span><span class="sxs-lookup"><span data-stu-id="0830e-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="0830e-105">En este artículo se supone que sabe cómo usar migraciones de Code First en escenarios básicos.</span><span class="sxs-lookup"><span data-stu-id="0830e-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="0830e-106">Si no lo hace, necesitará leer [migraciones de Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="0830e-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="0830e-107">Recomendación para entornos de equipo</span><span class="sxs-lookup"><span data-stu-id="0830e-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="0830e-108">Puede intercalar las migraciones automáticas y basado en código pero no se recomienda en escenarios de desarrollo en equipo.</span><span class="sxs-lookup"><span data-stu-id="0830e-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="0830e-109">Si forma parte de un equipo de desarrolladores que usan el control de código fuente se deben usar las migraciones automáticas puramente o las migraciones basadas puramente en código.</span><span class="sxs-lookup"><span data-stu-id="0830e-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="0830e-110">Dado que las limitaciones de las migraciones automáticas se recomienda usar las migraciones basadas en el código en entornos de equipo.</span><span class="sxs-lookup"><span data-stu-id="0830e-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="0830e-111">Creación de un modelo inicial y una base de datos</span><span class="sxs-lookup"><span data-stu-id="0830e-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="0830e-112">Antes de empezar a usar Migraciones, se necesitan un proyecto y un modelo de Code First con los que trabajar.</span><span class="sxs-lookup"><span data-stu-id="0830e-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="0830e-113">En este tutorial se va a usar el modelo canónico **Blog** y **Post**.</span><span class="sxs-lookup"><span data-stu-id="0830e-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="0830e-114">Cree un nuevo **MigrationsAutomaticDemo** aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="0830e-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="0830e-115">Agregue la versión más reciente del paquete NuGet de **EntityFramework** al proyecto</span><span class="sxs-lookup"><span data-stu-id="0830e-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="0830e-116">**Herramientas –&gt; Administrador de paquetes de biblioteca–&gt; Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="0830e-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="0830e-117">Ejecute el comando **Install-Package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="0830e-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="0830e-118">Agregue un archivo **Model.cs** con el código que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="0830e-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="0830e-119">Este código define una sola clase **Blog** que conforma el modelo de dominio y una clase **BlogContext** que es el contexto de EF Code First</span><span class="sxs-lookup"><span data-stu-id="0830e-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   <span data-ttu-id="0830e-120">Ahora que tenemos un modelo, es hora de usarlo para el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="0830e-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="0830e-121">Actualice el archivo **Program.cs** con el código que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="0830e-121">Update the **Program.cs** file with the code shown below.</span></span>

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsAutomaticDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   <span data-ttu-id="0830e-122">Ejecute la aplicación y verá que una **MigrationsAutomaticCodeDemo.BlogContext** base de datos se crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="0830e-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![Base de datos LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="0830e-124">Habilitación de migraciones</span><span class="sxs-lookup"><span data-stu-id="0830e-124">Enabling Migrations</span></span>

<span data-ttu-id="0830e-125">Es hora de realizar más cambios en el modelo.</span><span class="sxs-lookup"><span data-stu-id="0830e-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="0830e-126">Vamos a introducir una propiedad Url en la clase Blog.</span><span class="sxs-lookup"><span data-stu-id="0830e-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="0830e-127">Si volviera a ejecutar la aplicación de nuevo, aparecería una excepción InvalidOperationException con el texto *El modelo que respalda el contexto 'BlogContext' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar Migraciones de Code First para actualizar la base de datos (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span><span class="sxs-lookup"><span data-stu-id="0830e-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="0830e-128">Como sugiere la excepción, es hora de empezar a usar Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="0830e-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="0830e-129">Puesto que deseamos usar las migraciones automáticas, vamos a especificar la **– EnableAutomaticMigrations** cambie.</span><span class="sxs-lookup"><span data-stu-id="0830e-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="0830e-130">Ejecutar el **EnableAutomaticMigrations: Enable-Migrations** comando en la consola de administrador de paquetes este comando se ha agregado un **migraciones** carpeta a nuestro proyecto.</span><span class="sxs-lookup"><span data-stu-id="0830e-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="0830e-131">Esta nueva carpeta contiene un archivo:</span><span class="sxs-lookup"><span data-stu-id="0830e-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="0830e-132">**La clase Configuration.**</span><span class="sxs-lookup"><span data-stu-id="0830e-132">**The Configuration class.**</span></span> <span data-ttu-id="0830e-133">Esta clase permite configurar el comportamiento de Migraciones en el contexto.</span><span class="sxs-lookup"><span data-stu-id="0830e-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="0830e-134">En este tutorial se usa la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0830e-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="0830e-135">*Puesto que hay un solo contexto de Code First en el proyecto, Enable-Migrations ha rellenado automáticamente el tipo de contexto al que se aplica esta configuración.*</span><span class="sxs-lookup"><span data-stu-id="0830e-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="0830e-136">La primera migración automática</span><span class="sxs-lookup"><span data-stu-id="0830e-136">Your First Automatic Migration</span></span>

<span data-ttu-id="0830e-137">Migraciones de Code First tiene dos comandos principales con los que se va a familiarizar.</span><span class="sxs-lookup"><span data-stu-id="0830e-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="0830e-138">**Add-Migration** aplica la técnica scaffolding a la siguiente migración en función de los cambios realizados en el modelo desde la creación de la última migración</span><span class="sxs-lookup"><span data-stu-id="0830e-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="0830e-139">**Update-Database** aplica las migraciones pendientes a la base de datos</span><span class="sxs-lookup"><span data-stu-id="0830e-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="0830e-140">Vamos a evitar mediante Add-Migration (a menos que realmente debemos) y se centran en lo que permite migraciones de Code First automáticamente calcular y aplicar los cambios.</span><span class="sxs-lookup"><span data-stu-id="0830e-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="0830e-141">Vamos a usar **Update-Database** para obtener las migraciones de Code First para insertar los cambios en nuestro modelo (el nuevo **Blog.Ur**propiedad l) para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0830e-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="0830e-142">Ejecute el **Update-Database** comando en la consola de administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="0830e-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="0830e-143">El **MigrationsAutomaticDemo.BlogContext** base de datos se ha actualizado para incluir la **Url** columna en el **Blogs** tabla.</span><span class="sxs-lookup"><span data-stu-id="0830e-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="0830e-144">La segunda migración automática</span><span class="sxs-lookup"><span data-stu-id="0830e-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="0830e-145">Vamos a hacer otra cambiar y dejar que las migraciones de Code First a insertar automáticamente los cambios en la base de datos para nosotros.</span><span class="sxs-lookup"><span data-stu-id="0830e-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="0830e-146">También vamos a agregar una nueva clase **Post**</span><span class="sxs-lookup"><span data-stu-id="0830e-146">Let's also add a new **Post** class</span></span>

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   <span data-ttu-id="0830e-147">Además agregaremos una colección **Posts** a la clase **Blog** para formar el otro extremo de la relación entre **Blog** y **Post**</span><span class="sxs-lookup"><span data-stu-id="0830e-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="0830e-148">Ahora use **Update-Database** para actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0830e-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="0830e-149">Esta vez vamos a especificar la marca **-Verbose** para que pueda ver el SQL que ejecuta Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="0830e-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="0830e-150">Ejecute el comando **Update-Database –Verbose** en la consola del Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="0830e-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="0830e-151">Agregar un código de migración basada en</span><span class="sxs-lookup"><span data-stu-id="0830e-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="0830e-152">Ahora Echemos un vistazo a algo que podemos desear usar una migración basada en código para.</span><span class="sxs-lookup"><span data-stu-id="0830e-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="0830e-153">Vamos a agregar un **clasificación** propiedad a la **Blog** clase</span><span class="sxs-lookup"><span data-stu-id="0830e-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="0830e-154">Simplemente podríamos ejecutar **Update-Database** para insertar estos cambios en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0830e-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="0830e-155">Sin embargo, vamos a agregar un distinto de NULL **Blogs.Rating** columna, si no hay ningún dato existente en la tabla se asignará el valor predeterminado CLR del tipo de datos para la nueva columna (clasificación es entero, por lo que sería **0**).</span><span class="sxs-lookup"><span data-stu-id="0830e-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="0830e-156">Pero queremos especificar un valor predeterminado de **3**, para que las filas existentes en la tabla **Blogs** comiencen con una clasificación decente.</span><span class="sxs-lookup"><span data-stu-id="0830e-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="0830e-157">Vamos a usar el comando Add-Migration para escribir este cambio horizontal en una migración basada en el código de modo que podemos modificarlo.</span><span class="sxs-lookup"><span data-stu-id="0830e-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="0830e-158">El **Add-Migration** comando nos permite dar un nombre de estas migraciones, pongámosle nuestra **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="0830e-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="0830e-159">Ejecute el **Add-Migration AddBlogRating** comando en la consola de administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="0830e-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="0830e-160">En el **migraciones** carpeta ahora tenemos un nuevo **AddBlogRating** migración.</span><span class="sxs-lookup"><span data-stu-id="0830e-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="0830e-161">Previamente se ha corregido el nombre de archivo de migración con una marca de tiempo para ayudar a con la ordenación.</span><span class="sxs-lookup"><span data-stu-id="0830e-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="0830e-162">Vamos a editar el código generado para especificar un valor predeterminado de 3 para Blog.Rating (10 de línea en el código siguiente)</span><span class="sxs-lookup"><span data-stu-id="0830e-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="0830e-163">*La migración también tiene un archivo de código subyacente que captura algunos metadatos. Estos metadatos permitirá migraciones de Code First replicar las migraciones automáticas que se ha realizado antes de esta migración basada en código. Esto es importante si otro programador desea ejecutar nuestro migraciones o cuando llega el momento de implementar nuestra aplicación.*</span><span class="sxs-lookup"><span data-stu-id="0830e-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

<span data-ttu-id="0830e-164">La migración editada tiene buen aspecto, así que vamos a usar **Update-Database** para actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0830e-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="0830e-165">Ejecute el **Update-Database** comando en la consola de administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="0830e-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="0830e-166">A las migraciones automáticas</span><span class="sxs-lookup"><span data-stu-id="0830e-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="0830e-167">Ahora estamos libres para volver a las migraciones automáticas para los cambios más sencillos.</span><span class="sxs-lookup"><span data-stu-id="0830e-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="0830e-168">Migraciones de Code First se encargará de llevar a cabo las migraciones automáticas y basados en el código en el orden correcto basándose en los metadatos que se almacena en el archivo de código subyacente para cada migración basada en código.</span><span class="sxs-lookup"><span data-stu-id="0830e-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="0830e-169">Vamos a agregar una propiedad Post.Abstract a nuestro modelo</span><span class="sxs-lookup"><span data-stu-id="0830e-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="0830e-170">Ahora podemos usar **Update-Database** para obtener las migraciones de Code First para insertar este cambio en la base de datos mediante la migración automática.</span><span class="sxs-lookup"><span data-stu-id="0830e-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="0830e-171">Ejecute el **Update-Database** comando en la consola de administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="0830e-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="0830e-172">Resumen</span><span class="sxs-lookup"><span data-stu-id="0830e-172">Summary</span></span>

<span data-ttu-id="0830e-173">En este tutorial que se ha explicado cómo usar las migraciones automáticas para insertar modelo cambia a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0830e-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="0830e-174">También vimos cómo aplicar la técnica scaffolding y ejecutar las migraciones basadas en código entre las migraciones automáticas cuando necesite más control.</span><span class="sxs-lookup"><span data-stu-id="0830e-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
