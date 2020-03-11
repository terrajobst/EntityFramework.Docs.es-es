---
title: 'Migraciones de Code First: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: e5a91af73bab9d45b0f1f4242ce503c6b6f407f6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413310"
---
# <a name="code-first-migrations"></a><span data-ttu-id="b8aa6-102">Migraciones de Code First</span><span class="sxs-lookup"><span data-stu-id="b8aa6-102">Code First Migrations</span></span>
<span data-ttu-id="b8aa6-103">Migraciones de Code First es la manera recomendada de desarrollar el esquema de base de datos de la aplicación si usa el flujo de trabajo de Code First.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-103">Code First Migrations is the recommended way to evolve your application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="b8aa6-104">Migraciones proporciona un conjunto de herramientas que permiten:</span><span class="sxs-lookup"><span data-stu-id="b8aa6-104">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="b8aa6-105">Crear una base de datos inicial que funciona con el modelo de EF</span><span class="sxs-lookup"><span data-stu-id="b8aa6-105">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="b8aa6-106">Generar migraciones para realizar un seguimiento de los cambios realizados en el modelo de EF</span><span class="sxs-lookup"><span data-stu-id="b8aa6-106">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="b8aa6-107">Mantener actualizada la base de datos con esos cambios</span><span class="sxs-lookup"><span data-stu-id="b8aa6-107">Keep your database up to date with those changes</span></span>

<span data-ttu-id="b8aa6-108">En el tutorial siguiente se proporciona información general sobre Migraciones de Code First en Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-108">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="b8aa6-109">Puede realizar el tutorial completo o ir al tema que le interesa.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-109">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="b8aa6-110">Se tratan los siguientes temas:</span><span class="sxs-lookup"><span data-stu-id="b8aa6-110">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="b8aa6-111">Creación de un modelo inicial y una base de datos</span><span class="sxs-lookup"><span data-stu-id="b8aa6-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="b8aa6-112">Antes de empezar a usar Migraciones, se necesitan un proyecto y un modelo de Code First con los que trabajar.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="b8aa6-113">En este tutorial se va a usar el modelo canónico **Blog** y **Post**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="b8aa6-114">Cree una nueva aplicación de consola **MigrationsDemo**</span><span class="sxs-lookup"><span data-stu-id="b8aa6-114">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="b8aa6-115">Agregue la versión más reciente del paquete NuGet de **EntityFramework** al proyecto</span><span class="sxs-lookup"><span data-stu-id="b8aa6-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="b8aa6-116">**Herramientas –&gt; Administrador de paquetes de biblioteca–&gt; Consola del Administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="b8aa6-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="b8aa6-117">Ejecute el comando **Install-Package EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="b8aa6-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="b8aa6-118">Agregue un archivo **Model.cs** con el código que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="b8aa6-119">Este código define una sola clase **Blog** que conforma el modelo de dominio y una clase **BlogContext** que es el contexto de EF Code First</span><span class="sxs-lookup"><span data-stu-id="b8aa6-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsDemo
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

-   <span data-ttu-id="b8aa6-120">Ahora que tenemos un modelo, es hora de usarlo para el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="b8aa6-121">Actualice el archivo **Program.cs** con el código que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-121">Update the **Program.cs** file with the code shown below.</span></span>

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsDemo
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

-   <span data-ttu-id="b8aa6-122">Ejecute la aplicación y verá que se ha creado automáticamente una base de datos **MigrationsCodeDemo.BlogContext**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-122">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![Base de datos LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="b8aa6-124">Habilitación de migraciones</span><span class="sxs-lookup"><span data-stu-id="b8aa6-124">Enabling Migrations</span></span>

<span data-ttu-id="b8aa6-125">Es hora de realizar más cambios en el modelo.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="b8aa6-126">Vamos a introducir una propiedad Url en la clase Blog.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="b8aa6-127">Si volviera a ejecutar la aplicación de nuevo, aparecería una excepción InvalidOperationException con el texto *El modelo que respalda el contexto 'BlogContext' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar Migraciones de Code First para actualizar la base de datos (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="b8aa6-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="b8aa6-128">Como sugiere la excepción, es hora de empezar a usar Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="b8aa6-129">El primer paso es habilitar las migraciones para el contexto.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-129">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="b8aa6-130">Ejecute el comando **Enable-Migrations** en la consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="b8aa6-130">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="b8aa6-131">Este comando ha agregado una carpeta **Migraciones** al proyecto.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-131">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="b8aa6-132">Esta nueva carpeta contiene dos archivos:</span><span class="sxs-lookup"><span data-stu-id="b8aa6-132">This new folder contains two files:</span></span>

-   <span data-ttu-id="b8aa6-133">**La clase Configuration.**</span><span class="sxs-lookup"><span data-stu-id="b8aa6-133">**The Configuration class.**</span></span> <span data-ttu-id="b8aa6-134">Esta clase permite configurar el comportamiento de Migraciones en el contexto.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="b8aa6-135">En este tutorial se usa la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="b8aa6-136">*Puesto que hay un solo contexto de Code First en el proyecto, Enable-Migrations ha rellenado automáticamente el tipo de contexto al que se aplica esta configuración.*</span><span class="sxs-lookup"><span data-stu-id="b8aa6-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="b8aa6-137">**Una migración InitialCreate**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-137">**An InitialCreate migration**.</span></span> <span data-ttu-id="b8aa6-138">Esta migración se ha generado porque Code First ya había creado automáticamente una base de datos antes de que se habilitaran las migraciones.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-138">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="b8aa6-139">El código de esta migración con scaffolding representa los objetos que ya se han creado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-139">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="b8aa6-140">En nuestro caso, es la tabla **Blog** con las columnas **BlogId** y **Name**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-140">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="b8aa6-141">El nombre de archivo incluye una marca de tiempo para ayudar con el orden.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-141">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="b8aa6-142">*Si la base de datos aún no se hubiera creado, esta migración InitialCreate no se hubiera agregado al proyecto. Por el contrario, la primera vez que se llamara a Add-Migration, al código para crear estas tablas se le aplicaría scaffolding para una nueva migración.*</span><span class="sxs-lookup"><span data-stu-id="b8aa6-142">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="b8aa6-143">Varios modelos con la misma base de datos como destino</span><span class="sxs-lookup"><span data-stu-id="b8aa6-143">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="b8aa6-144">Al usar versiones anteriores a EF6, solo se puede usar un modelo de Code First para generar o administrar el esquema de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-144">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="b8aa6-145">Este es el resultado de una sola tabla **\_\_MigrationsHistory** por base de datos sin forma de identificar qué entradas pertenecen a qué modelo.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-145">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="b8aa6-146">A partir de EF6, la clase **Configuration** incluye una propiedad **ContextKey**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-146">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="b8aa6-147">Esta actúa como identificador único para cada modelo de Code First.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-147">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="b8aa6-148">Una columna correspondiente de la tabla **\_\_MigrationsHistory** permite que entradas de varios modelos compartan la tabla.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-148">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="b8aa6-149">De forma predeterminada, esta propiedad se establece en el nombre completo del contexto.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-149">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="b8aa6-150">Generación y ejecución de migraciones</span><span class="sxs-lookup"><span data-stu-id="b8aa6-150">Generating & Running Migrations</span></span>

<span data-ttu-id="b8aa6-151">Migraciones de Code First tiene dos comandos principales con los que se va a familiarizar.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-151">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="b8aa6-152">**Add-Migration** aplica la técnica scaffolding a la siguiente migración en función de los cambios realizados en el modelo desde la creación de la última migración</span><span class="sxs-lookup"><span data-stu-id="b8aa6-152">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="b8aa6-153">**Update-Database** aplica las migraciones pendientes a la base de datos</span><span class="sxs-lookup"><span data-stu-id="b8aa6-153">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="b8aa6-154">Es necesario aplicar scaffolding a una migración para encargarse de la nueva propiedad Url que se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-154">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="b8aa6-155">El comando **Add-Migration** permite poner un nombre a estas migraciones; a la nuestra la llamaremos **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-155">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="b8aa6-156">Ejecute el comando **Add-Migration AddBlogUrl** en la consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="b8aa6-156">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="b8aa6-157">En la carpeta **Migraciones** ahora tenemos una nueva migración **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-157">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="b8aa6-158">El nombre de archivo de la migración lleva una marca de tiempo para ayudar con el orden</span><span class="sxs-lookup"><span data-stu-id="b8aa6-158">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogUrl : DbMigration
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

<span data-ttu-id="b8aa6-159">Ahora se podría editar esta migración o agregarle elementos, pero todo tiene bastante buen aspecto.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-159">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="b8aa6-160">Vamos a usar **Update-Database** para aplicar esta migración a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-160">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="b8aa6-161">Ejecute el comando **Update-Database** en la consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="b8aa6-161">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="b8aa6-162">Migraciones de Code First compara las migraciones de la carpeta **Migraciones** con las que se han aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-162">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="b8aa6-163">Verá que es necesario aplicar la migración **AddBlogUrl** y ejecutarla.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-163">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="b8aa6-164">La base de datos **MigrationsDemo.BlogContext** se ha actualizado para incluir la columna **Url** en la tabla **Blogs**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-164">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="b8aa6-165">Personalización de migraciones</span><span class="sxs-lookup"><span data-stu-id="b8aa6-165">Customizing Migrations</span></span>

<span data-ttu-id="b8aa6-166">Hasta ahora hemos generado y ejecutado una migración sin realizar ningún cambio.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-166">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="b8aa6-167">Ahora vamos a editar el código que se genera de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-167">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="b8aa6-168">Es hora de realizar algunos cambios más en el modelo: vamos a agregar una nueva propiedad **Rating** a la clase **Blog**</span><span class="sxs-lookup"><span data-stu-id="b8aa6-168">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="b8aa6-169">También vamos a agregar una nueva clase **Post**</span><span class="sxs-lookup"><span data-stu-id="b8aa6-169">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="b8aa6-170">Además agregaremos una colección **Posts** a la clase **Blog** para formar el otro extremo de la relación entre **Blog** y **Post**</span><span class="sxs-lookup"><span data-stu-id="b8aa6-170">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="b8aa6-171">Vamos a usar el comando **Add-Migration** para permitir que Migraciones de Code First aplique scaffolding de su mejor estimación en la migración.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-171">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="b8aa6-172">Vamos a llamar a esta migración **AddPostClass**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-172">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="b8aa6-173">Ejecute el comando **Add-Migration AddPostClass** en la consola del Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-173">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="b8aa6-174">Migraciones de Code First hizo un muy buen trabajo de scaffolding de estos cambios, pero hay algunas cosas que es posible que queramos cambiar:</span><span class="sxs-lookup"><span data-stu-id="b8aa6-174">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="b8aa6-175">En primer lugar, vamos a agregar un índice único a la columna **Posts.Title** (adición en la línea 22 y 29 del código siguiente).</span><span class="sxs-lookup"><span data-stu-id="b8aa6-175">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="b8aa6-176">También vamos a agregar una columna **Blogs.Rating** que no admite valores null.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-176">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="b8aa6-177">Si hay datos existentes en la tabla, se les asigna el valor predeterminado CLR del tipo de datos para la nueva columna (Rating es entero, por lo que sería **0**).</span><span class="sxs-lookup"><span data-stu-id="b8aa6-177">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="b8aa6-178">Pero queremos especificar un valor predeterminado de **3**, para que las filas existentes en la tabla **Blogs** comiencen con una clasificación decente.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-178">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="b8aa6-179">(Puede ver el valor predeterminado especificado en la línea 24 del código siguiente)</span><span class="sxs-lookup"><span data-stu-id="b8aa6-179">(You can see the default value specified on line 24 of the code below)</span></span>

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostClass : DbMigration
        {
            public override void Up()
            {
                CreateTable(
                    "dbo.Posts",
                    c => new
                        {
                            PostId = c.Int(nullable: false, identity: true),
                            Title = c.String(maxLength: 200),
                            Content = c.String(),
                            BlogId = c.Int(nullable: false),
                        })
                    .PrimaryKey(t => t.PostId)
                    .ForeignKey("dbo.Blogs", t => t.BlogId, cascadeDelete: true)
                    .Index(t => t.BlogId)
                    .Index(p => p.Title, unique: true);

                AddColumn("dbo.Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropIndex("dbo.Posts", new[] { "Title" });
                DropIndex("dbo.Posts", new[] { "BlogId" });
                DropForeignKey("dbo.Posts", "BlogId", "dbo.Blogs");
                DropColumn("dbo.Blogs", "Rating");
                DropTable("dbo.Posts");
            }
        }
    }
```

<span data-ttu-id="b8aa6-180">La migración editada ya está lista, así que vamos a usar **Update-Database** para actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-180">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="b8aa6-181">Esta vez vamos a especificar la marca **-Verbose** para que pueda ver el SQL que ejecuta Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-181">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="b8aa6-182">Ejecute el comando **Update-Database –Verbose** en la consola del Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-182">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="b8aa6-183">Movimiento de datos o SQL personalizado</span><span class="sxs-lookup"><span data-stu-id="b8aa6-183">Data Motion / Custom SQL</span></span>

<span data-ttu-id="b8aa6-184">Hasta ahora hemos examinado las operaciones de migración que no cambian ni mueven datos, y ahora vamos a ver algo que necesita mover algunos datos.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-184">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="b8aa6-185">Todavía no hay compatibilidad nativa con el movimiento de datos, pero podemos ejecutar algunos comandos SQL arbitrarios en cualquier punto del script.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-185">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="b8aa6-186">Vamos a agregar una propiedad **Post.Abstract** al modelo.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-186">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="b8aa6-187">Luego, vamos a rellenar previamente el elemento **Abstract** de publicaciones existentes con algún texto del inicio de la columna **Content**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-187">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="b8aa6-188">Vamos a usar el comando **Add-Migration** para permitir que Migraciones de Code First aplique scaffolding de su mejor estimación en la migración.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-188">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="b8aa6-189">Ejecute el comando **Add-Migration AddPostAbstract** en la consola del Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-189">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="b8aa6-190">La migración generada se encarga de los cambios de esquema, pero además queremos rellenar previamente la columna **Abstract** con los 100 primeros caracteres del contenido de cada publicación.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-190">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="b8aa6-191">Lo podemos hacer si recurrimos a SQL y ejecutamos una instrucción **UPDATE** después de agregar la columna.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-191">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="b8aa6-192">(Adición en la línea 12 del código siguiente)</span><span class="sxs-lookup"><span data-stu-id="b8aa6-192">(Adding in line 12 in the code below)</span></span>

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostAbstract : DbMigration
        {
            public override void Up()
            {
                AddColumn("dbo.Posts", "Abstract", c => c.String());

                Sql("UPDATE dbo.Posts SET Abstract = LEFT(Content, 100) WHERE Abstract IS NULL");
            }

            public override void Down()
            {
                DropColumn("dbo.Posts", "Abstract");
            }
        }
    }
```

<span data-ttu-id="b8aa6-193">La migración editada tiene buen aspecto, así que vamos a usar **Update-Database** para actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-193">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="b8aa6-194">Especificamos la marca **–Verbose** para poder ver el SQL que se ejecuta en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-194">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="b8aa6-195">Ejecute el comando **Update-Database –Verbose** en la consola del Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-195">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="b8aa6-196">Migrar a una versión determinada (incluido un cambio a una versión anterior)</span><span class="sxs-lookup"><span data-stu-id="b8aa6-196">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="b8aa6-197">Hasta ahora siempre hemos actualizado a la migración más reciente, pero puede haber ocasiones en que quiera cambiar a una migración anterior o posterior.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-197">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="b8aa6-198">Supongamos que queremos migrar la base de datos al estado en que estaba después de ejecutar la migración **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-198">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="b8aa6-199">Podemos usar el modificador **–TargetMigration** para cambiar a esta migración anterior.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-199">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="b8aa6-200">Ejecute el comando **Update-Database –TargetMigration: AddBlogUrl** en la consola del Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-200">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="b8aa6-201">Este comando ejecuta el script Down de las migraciones **AddBlogAbstract** y **AddPostClass**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-201">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="b8aa6-202">Si quiere revertir a una base de datos vacía, puede usar el comando **Update-Database –TargetMigration: $InitialDatabase**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-202">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="b8aa6-203">Obtención de un script SQL</span><span class="sxs-lookup"><span data-stu-id="b8aa6-203">Getting a SQL Script</span></span>

<span data-ttu-id="b8aa6-204">Si otro desarrollador quiere estos cambios en su equipo, puede sincronizar una vez que se protejan los cambios en el control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-204">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="b8aa6-205">Cuando tenga las nuevas migraciones, puede ejecutar el comando Update-Database para que los cambios se apliquen localmente.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-205">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="b8aa6-206">Pero si queremos enviar estos cambios a un servidor de prueba y, finalmente, a producción, probablemente querremos un script SQL que podamos pasar al DBA.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-206">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="b8aa6-207">Ejecute el comando **Update-Database**, pero esta vez especifique la marca **–Script** para que los cambios se escriban en un script en lugar de aplicarse.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-207">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="b8aa6-208">También se especifican una migración de origen y de destino para las que generar el script.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-208">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="b8aa6-209">Queremos un script que abarque desde una base de datos vacía ( **$InitialDatabase**) a la versión más reciente (migración **AddPostAbstract**).</span><span class="sxs-lookup"><span data-stu-id="b8aa6-209">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="b8aa6-210">*Si no se especifica una migración de destino, Migraciones usa la última migración como destino. Si no se especifica una migración de origen, Migraciones usa el estado actual de la base de datos.*</span><span class="sxs-lookup"><span data-stu-id="b8aa6-210">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="b8aa6-211">Ejecute el comando **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** en la consola del Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-211">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="b8aa6-212">Migraciones de Code First ejecuta la canalización de migración, pero en lugar de aplicar los cambios, los escribe en un archivo .sql.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-212">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="b8aa6-213">Una vez que se ha generado el script, se abre automáticamente en Visual Studio, listo para verse o guardarse.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-213">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="b8aa6-214">Generación de scripts idempotentes</span><span class="sxs-lookup"><span data-stu-id="b8aa6-214">Generating Idempotent Scripts</span></span>

<span data-ttu-id="b8aa6-215">A partir de EF6, si especifica **–SourceMigration $InitialDatabase**, el script generado será "idempotente".</span><span class="sxs-lookup"><span data-stu-id="b8aa6-215">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="b8aa6-216">Los scripts idempotentes pueden actualizar una base de datos de cualquier versión a la versión más reciente (o la versión especificada si se usa **–TargetMigration**).</span><span class="sxs-lookup"><span data-stu-id="b8aa6-216">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="b8aa6-217">El script generado incluye lógica para comprobar la tabla **\_\_MigrationsHistory** y aplicar solo los cambios que no se han aplicado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-217">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="b8aa6-218">Actualización automática al iniciar la aplicación (inicializador MigrateDatabaseToLatestVersion)</span><span class="sxs-lookup"><span data-stu-id="b8aa6-218">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="b8aa6-219">Si va a implementar la aplicación, es posible que quiera que actualice la base de datos automáticamente (al aplicar las migraciones pendientes) cuando se inicie.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-219">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="b8aa6-220">Puede hacerlo si registra el inicializador de base de datos **MigrateDatabaseToLatestVersion**.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-220">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="b8aa6-221">Un inicializador de base de datos simplemente contiene alguna lógica que se usa para asegurarse de que la base de datos se ha configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-221">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="b8aa6-222">Esta lógica se ejecuta la primera vez que se usa el contexto dentro del proceso de aplicación (**AppDomain**).</span><span class="sxs-lookup"><span data-stu-id="b8aa6-222">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="b8aa6-223">Se puede actualizar el archivo **Program.cs**, como se muestra a continuación, para establecer el inicializador **MigrateDatabaseToLatestVersion** para BlogContext antes de usar el contexto (línea 14).</span><span class="sxs-lookup"><span data-stu-id="b8aa6-223">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="b8aa6-224">Tenga en cuenta que también debe agregar una instrucción using para el espacio de nombres **System.Data.Entity** (línea 5).</span><span class="sxs-lookup"><span data-stu-id="b8aa6-224">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="b8aa6-225">*Cuando se crea una instancia de este inicializador, se debe especificar el tipo de contexto (**BlogContext**) y la configuración de las migraciones (**Configuration**): la configuración de las migraciones es la clase que se ha agregado a la carpeta **Migraciones** al habilitar Migraciones.*</span><span class="sxs-lookup"><span data-stu-id="b8aa6-225">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Data.Entity;
    using MigrationsDemo.Migrations;

    namespace MigrationsDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                Database.SetInitializer(new MigrateDatabaseToLatestVersion<BlogContext, Configuration>());

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

<span data-ttu-id="b8aa6-226">Ahora, siempre que se ejecute la aplicación, en primer lugar comprobará si la base de datos de destino está actualizada y, si no lo está, aplicará las migraciones pendientes.</span><span class="sxs-lookup"><span data-stu-id="b8aa6-226">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>
