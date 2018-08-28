---
title: Migraciones de Code automática First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: f358a4df04b03399e9e54ffdf0389e43d715af1c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996100"
---
# <a name="automatic-code-first-migrations"></a>Migraciones automáticas de Code First
Las migraciones automáticas permite usar migraciones de Code First sin necesidad de un archivo de código en el proyecto para cada cambio realizado. No todos los cambios se pueden aplicar automáticamente: por ejemplo cambia el nombre de columna exige el uso de una migración basada en código.

> [!NOTE]
> En este artículo se supone que sabe cómo usar migraciones de Code First en escenarios básicos. Si no lo hace, necesitará leer [migraciones de Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="recommendation-for-team-environments"></a>Recomendación para entornos de equipo

Puede intercalar las migraciones automáticas y basado en código pero no se recomienda en escenarios de desarrollo en equipo. Si forma parte de un equipo de desarrolladores que usan el control de código fuente se deben usar las migraciones automáticas puramente o las migraciones basadas puramente en código. Dado que las limitaciones de las migraciones automáticas se recomienda usar las migraciones basadas en el código en entornos de equipo.

## <a name="building-an-initial-model--database"></a>Creación de un modelo inicial y una base de datos

Antes de empezar a usar Migraciones, se necesitan un proyecto y un modelo de Code First con los que trabajar. En este tutorial se va a usar el modelo canónico **Blog** y **Post**.

-   Cree un nuevo **MigrationsAutomaticDemo** aplicación de consola
-   Agregue la versión más reciente del paquete NuGet de **EntityFramework** al proyecto
    -   **Herramientas –&gt; Administrador de paquetes de biblioteca–&gt; Consola del Administrador de paquetes**
    -   Ejecute el comando **Install-Package EntityFramework**
-   Agregue un archivo **Model.cs** con el código que se muestra a continuación. Este código define una sola clase **Blog** que conforma el modelo de dominio y una clase **BlogContext** que es el contexto de EF Code First

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

-   Ahora que tenemos un modelo, es hora de usarlo para el acceso a los datos. Actualice el archivo **Program.cs** con el código que se muestra a continuación.

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

-   Ejecute la aplicación y verá que una **MigrationsAutomaticCodeDemo.BlogContext** base de datos se crea automáticamente.

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Habilitación de migraciones

Es hora de realizar más cambios en el modelo.

-   Vamos a introducir una propiedad Url en la clase Blog.

``` csharp
    public string Url { get; set; }
```

Si volviera a ejecutar la aplicación de nuevo, aparecería una excepción InvalidOperationException con el texto *El modelo que respalda el contexto 'BlogContext' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar Migraciones de Code First para actualizar la base de datos (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*

Como sugiere la excepción, es hora de empezar a usar Migraciones de Code First. Puesto que deseamos usar las migraciones automáticas, vamos a especificar la **– EnableAutomaticMigrations** cambie.

-   Ejecutar el **EnableAutomaticMigrations: Enable-Migrations** comando en la consola de administrador de paquetes este comando se ha agregado un **migraciones** carpeta a nuestro proyecto. Esta nueva carpeta contiene un archivo:

-   **La clase Configuration.** Esta clase permite configurar el comportamiento de Migraciones en el contexto. En este tutorial se usa la configuración predeterminada.
    *Puesto que hay un solo contexto de Code First en el proyecto, Enable-Migrations ha rellenado automáticamente el tipo de contexto al que se aplica esta configuración.*

 

## <a name="your-first-automatic-migration"></a>La primera migración automática

Migraciones de Code First tiene dos comandos principales con los que se va a familiarizar.

-   **Add-Migration** aplica la técnica scaffolding a la siguiente migración en función de los cambios realizados en el modelo desde la creación de la última migración
-   **Update-Database** aplica las migraciones pendientes a la base de datos

Vamos a evitar mediante Add-Migration (a menos que realmente debemos) y se centran en lo que permite migraciones de Code First automáticamente calcular y aplicar los cambios. Vamos a usar **Update-Database** para obtener las migraciones de Code First para insertar los cambios en nuestro modelo (el nuevo **Blog.Ur**propiedad l) para la base de datos.

-   Ejecute el **Update-Database** comando en la consola de administrador de paquetes.

El **MigrationsAutomaticDemo.BlogContext** base de datos se ha actualizado para incluir la **Url** columna en el **Blogs** tabla.

 

## <a name="your-second-automatic-migration"></a>La segunda migración automática

Vamos a hacer otra cambiar y dejar que las migraciones de Code First a insertar automáticamente los cambios en la base de datos para nosotros.

-   También vamos a agregar una nueva clase **Post**

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

-   Además agregaremos una colección **Posts** a la clase **Blog** para formar el otro extremo de la relación entre **Blog** y **Post**

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Ahora use **Update-Database** para actualizar la base de datos. Esta vez vamos a especificar la marca **-Verbose** para que pueda ver el SQL que ejecuta Migraciones de Code First.

-   Ejecute el comando **Update-Database –Verbose** en la consola del Administrador de paquetes.

## <a name="adding-a-code-based-migration"></a>Agregar un código de migración basada en

Ahora Echemos un vistazo a algo que podemos desear usar una migración basada en código para.

-   Vamos a agregar un **clasificación** propiedad a la **Blog** clase

``` csharp
    public int Rating { get; set; }
```

Simplemente podríamos ejecutar **Update-Database** para insertar estos cambios en la base de datos. Sin embargo, vamos a agregar un distinto de NULL **Blogs.Rating** columna, si no hay ningún dato existente en la tabla se asignará el valor predeterminado CLR del tipo de datos para la nueva columna (clasificación es entero, por lo que sería **0**). Pero queremos especificar un valor predeterminado de **3**, para que las filas existentes en la tabla **Blogs** comiencen con una clasificación decente.
Vamos a usar el comando Add-Migration para escribir este cambio horizontal en una migración basada en el código de modo que podemos modificarlo. El **Add-Migration** comando nos permite dar un nombre de estas migraciones, pongámosle nuestra **AddBlogRating**.

-   Ejecute el **Add-Migration AddBlogRating** comando en la consola de administrador de paquetes.
-   En el **migraciones** carpeta ahora tenemos un nuevo **AddBlogRating** migración. Previamente se ha corregido el nombre de archivo de migración con una marca de tiempo para ayudar a con la ordenación. Vamos a editar el código generado para especificar un valor predeterminado de 3 para Blog.Rating (10 de línea en el código siguiente)

*La migración también tiene un archivo de código subyacente que captura algunos metadatos. Estos metadatos permitirá migraciones de Code First replicar las migraciones automáticas que se ha realizado antes de esta migración basada en código. Esto es importante si otro programador desea ejecutar nuestro migraciones o cuando llega el momento de implementar nuestra aplicación.*

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

La migración editada tiene buen aspecto, así que vamos a usar **Update-Database** para actualizar la base de datos.

-   Ejecute el **Update-Database** comando en la consola de administrador de paquetes.

## <a name="back-to-automatic-migrations"></a>A las migraciones automáticas

Ahora estamos libres para volver a las migraciones automáticas para los cambios más sencillos. Migraciones de Code First se encargará de llevar a cabo las migraciones automáticas y basados en el código en el orden correcto basándose en los metadatos que se almacena en el archivo de código subyacente para cada migración basada en código.

-   Vamos a agregar una propiedad Post.Abstract a nuestro modelo

``` csharp
    public string Abstract { get; set; }
```

Ahora podemos usar **Update-Database** para obtener las migraciones de Code First para insertar este cambio en la base de datos mediante la migración automática.

-   Ejecute el **Update-Database** comando en la consola de administrador de paquetes.

## <a name="summary"></a>Resumen

En este tutorial que se ha explicado cómo usar las migraciones automáticas para insertar modelo cambia a la base de datos. También vimos cómo aplicar la técnica scaffolding y ejecutar las migraciones basadas en código entre las migraciones automáticas cuando necesite más control.
