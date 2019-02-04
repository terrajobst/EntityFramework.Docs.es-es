---
title: Async consultar y guardar - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 89c7b9d533d37b4c9e123f37d8ab27c67ba26cc8
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668718"
---
# <a name="async-query-and-save"></a>Async consultar y guardar
> [!NOTE]
> **Solo EF6 y versiones posteriores**: las características, las API, etc. que se tratan en esta página se han incluido a partir de Entity Framework 6. Si usa una versión anterior, no se aplica parte o la totalidad de la información.

EF6 introdujo la compatibilidad para consulta asincrónica y guárdelo con el [async y await palabras clave](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) que se introdujeron en .NET 4.5. Aunque no todas las aplicaciones pueden beneficiarse de la asincronía, puede usar para mejorar la escalabilidad de la capacidad de respuesta y el servidor de cliente cuando se administran las tareas dependientes de E/s, red o larga ejecución.

## <a name="when-to-really-use-async"></a>Cuándo se debe realmente usar async

El propósito de este tutorial es introducir los conceptos de async en una forma que resulte fácil de observar la diferencia entre la ejecución del programa sincrónicas y asincrónicas. En este tutorial no está pensado para mostrar cualquiera de los escenarios clave donde la programación asincrónica proporciona ventajas.

Programación asincrónica se centra principalmente en lo que libera el subproceso administrado actual (código de .NET ejecución de subprocesos) para realizar otro trabajo mientras se espera una operación que no requiere ningún tiempo de proceso desde un subproceso administrado. Por ejemplo, mientras que el motor de base de datos está procesando una consulta no hay nada que se realiza mediante código de .NET.

En las aplicaciones cliente (WinForms, WPF, etc.) puede utilizarse el subproceso actual para que la interfaz de usuario siga respondiendo mientras se realiza la operación asincrónica. En aplicaciones de servidor (ASP.NET, etc.) se puede usar el subproceso para procesar otras solicitudes entrantes: Esto puede reducir el uso de memoria o aumentar el rendimiento del servidor.

En la mayoría de las aplicaciones usar async no tendrán ninguna ventaja apreciable e incluso podría ser perjudicial. Use las pruebas, la generación de perfiles y sentido común para medir el impacto de async en su escenario concreto antes de confirmar.

Estos son algunos recursos más que aprender sobre async:

-   [Información general de Brandon Bray de async y await en .NET 4.5](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   [Programación asincrónica](https://msdn.microsoft.com/library/hh191443.aspx) páginas en la biblioteca de MSDN
-   [Cómo crear aplicaciones Web ASP.NET utilizando Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (incluye una demostración de rendimiento del servidor aumento de)

## <a name="create-the-model"></a>Creación del modelo

Vamos a usar el [flujo de trabajo Code First](~/ef6/modeling/code-first/workflows/new-database.md) para crear nuestro modelo y generar la base de datos, sin embargo, la funcionalidad asincrónica funcionará con todos los modelos EF, incluidos aquellos creados con EF Designer.

-   Crear una aplicación de consola y llámelo **AsyncDemo**
-   Agregue el paquete EntityFramework NuGet
    -   En el Explorador de soluciones, haga doble clic en el **AsyncDemo** proyecto
    -   Seleccione **administrar paquetes NuGet...**
    -   En el cuadro de diálogo Administrar paquetes de NuGet, seleccione el **Online** pestaña y elija el **EntityFramework** paquete
    -   Haga clic en **instalar**
-   Agregar un **Model.cs** clase con la siguiente implementación

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

 

## <a name="create-a-synchronous-program"></a>Crear un programa sincrónico

Ahora que tenemos un modelo de EF, vamos a escribir algún código que lo utiliza para realizar algún acceso a los datos.

-   Reemplace el contenido de **Program.cs** con el código siguiente

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
                    Console.WriteLine("Calling SaveChanges.");
                    db.SaveChanges();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

Este código llama a la **PerformDatabaseOperations** método que se guarda un nuevo **Blog** a la base de datos y, a continuación, recupera todos los **Blogs** desde la base de datos y los imprime en la **Consola**. Una vez hecho esto, el programa escribe una cita del día a la **consola**.

Puesto que el código es sincrónico, nos podemos observar el siguiente flujo de ejecución cuando se ejecuta el programa:

1.  **SaveChanges** comienza a insertar el nuevo **Blog** a la base de datos
2.  **SaveChanges** completa
3.  Consulta para todos los **Blogs** se envía a la base de datos
4.  Devuelve la consulta y los resultados se escriben en **consola**
5.  Cita del día se escribe en **consola**

![Salida de sincronización](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Convertirla en asincrónica

Ahora que tenemos nuestro programa ponerse en marcha, podremos hacer uso del nuevas async y await palabras clave. Nos hemos realizado los cambios siguientes en Program.cs

1.  Línea 2: El mediante declaración para el **System.Data.Entity** espacio de nombres nos da acceso a los métodos de extensión de async EF.
2.  Línea 4: El mediante declaración para el **System.Threading.Tasks** espacio de nombres nos permite utilizar el **tarea** tipo.
3.  Línea 12 y 18: Hemos recopilado como tarea que supervisa el progreso de **PerformSomeDatabaseOperations** (línea 12) y, a continuación, bloquear la ejecución de programa para esta tarea a una vez completada todo el trabajo para el programa se realiza (línea 18).
4.  Línea 25: Hemos actualización **PerformSomeDatabaseOperations** se marquen como **async** y devolver un **tarea**.
5.  Línea 35: Ahora estamos llamando a la versión asincrónica de SaveChanges y espera su finalización.
6.  Línea 42: Ahora estamos llamando a la versión asincrónica de ToList y se espera en el resultado.

Para obtener una lista completa de métodos de extensión disponibles en el espacio de nombres System.Data.Entity, consulte la clase QueryableExtensions. *También deberá agregar "using System.Data.Entity" para el uso de instrucciones.*

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

Ahora que el código es asincrónica, nos podemos observar un flujo de ejecución diferente cuando se ejecuta el programa:

1.  **SaveChanges** comienza a insertar el nuevo **Blog** a la base de datos *una vez que el comando se envía a la base de datos no más de proceso se necesita tiempo en el subproceso administrado actual. El **PerformDatabaseOperations** método devuelve (incluso si no ha terminado de ejecutarse) y continúa el flujo del programa en el método Main.*
2.  **Cita del día se escribe en la consola**
    *ya que no hay nada más trabajo que hacer en el método Main, se bloquea el subproceso administrado en la espera llamar hasta que se complete la operación de base de datos. Cuando haya terminado, el resto de nuestro **PerformDatabaseOperations** se ejecutará.*
3.  **SaveChanges** completa
4.  Consulta para todos los **Blogs** se envía a la base de datos *de nuevo, el subproceso administrado es libre para realizar otro trabajo mientras se procesa la consulta en la base de datos. Puesto que se haya completado toda la ejecución de otra, el subproceso simplemente detendrá en la llamada espera aunque.*
5.  Devuelve la consulta y los resultados se escriben en **consola**

![Salida de Async](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>La conclusión

Ahora hemos visto lo fácil que es hacer uso de métodos asincrónicos de EF. Aunque las ventajas de async no pueden ser muy evidentes con una aplicación de consola simple, estas mismas estrategias se pueden aplicar en situaciones en las actividades relacionadas con la red o de larga ejecución podrían en caso contrario, bloquea la aplicación, o provocar un gran número de subprocesos aumentar el consumo de memoria.
