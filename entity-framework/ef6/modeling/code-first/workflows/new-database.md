---
title: 'Code First en una base de datos: EF6'
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 8ed1bfbc3536acc0d83b9c8ecdd180aeb44eff83
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251055"
---
# <a name="code-first-to-a-new-database"></a>Code First en una base de datos
En este tutorial de vídeo y paso a paso proporcionan una introducción al desarrollo de Code First destinadas a una base de datos. Este escenario incluye destinadas a una base de datos que no existe y Code First creará o una base de datos vacía que Code First agregará nuevas tablas a. Código primero le permite definir su modelo mediante C\# o clases de VB.Net. Configuración adicional si lo desea puede realizarse mediante los atributos en las clases y propiedades o mediante una API fluida.

## <a name="watch-the-video"></a>Vea el vídeo
Este vídeo ofrece una introducción al desarrollo de Code First destinadas a una base de datos. Este escenario incluye destinadas a una base de datos que no existe y Code First creará o una base de datos vacía que Code First agregará nuevas tablas a. Código primero permite definir su modelo mediante clases de C# o VB.Net. Configuración adicional si lo desea puede realizarse mediante los atributos en las clases y propiedades o mediante una API fluida.

**Presentado por**: [Rowan Miller](http://romiller.com/)

**Vídeo**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

# <a name="pre-requisites"></a>Requisitos previos

Debe tener al menos Visual Studio 2010 o Visual Studio 2012 instalado para completar este tutorial.

Si utiliza Visual Studio 2010, también necesitará tener [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.

## <a name="1-create-the-application"></a>1. Crear la aplicación

Para simplificar las cosas que vamos a crear una aplicación de consola básica que usa Code First para tener acceso a datos.

-   Apertura de Visual Studio
-   **Archivo -&gt; nuevo:&gt; proyecto...**
-   Seleccione **Windows** en el menú izquierdo y **aplicación de consola**
-   Escriba **CodeFirstNewDatabaseSample** como el nombre
-   Seleccione **Aceptar**.

## <a name="2-create-the-model"></a>2. Crear el modelo

Vamos a definir un modelo muy simple mediante clases. Simplemente nos estamos definiendo en el archivo Program.cs, pero en una aplicación real que dividiría las clases de salida en archivos independientes y, potencialmente, un proyecto independiente.

Debajo de la definición de clase de programa en Program.cs, agregue las dos clases siguientes.

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

Observará que estamos haciendo las dos propiedades de navegación (Blog.Posts y Post.Blog) virtual. Esto habilita la característica de carga diferida de Entity Framework. La carga diferida significa que el contenido de estas propiedades se cargarán automáticamente desde la base de datos cuando se intenta tener acceso a ellos.

## <a name="3-create-a-context"></a>3. Crear un contexto

Ahora es momento de definir un contexto derivado que representa una sesión con la base de datos, lo que nos permite consultar y guardar los datos. Definimos un contexto que se deriva de System.Data.Entity.DbContext y expone una clase DbSet con tipo&lt;TEntity&gt; para cada clase en nuestro modelo.

Ahora comienza a usar tipos de Entity Framework, por lo que necesitamos agregar el paquete EntityFramework NuGet.

-   **Proyecto –&gt; administrar paquetes NuGet...**
    Nota: Si no tienes el **administrar paquetes NuGet...** opción que debe instalar la [versión más reciente de NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Seleccione el **Online** ficha
-   Seleccione el **EntityFramework** paquete
-   Haga clic en **instalar**

Agregue una mediante declaración para System.Data.Entity en la parte superior de Program.cs.

``` csharp
using System.Data.Entity;
```

Debajo de la clase Post en Program.cs, agregue el siguiente contexto derivado.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Le presentamos una lista completa de lo que debería contener Program.cs.

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

Eso es todo el código que se necesita para empezar a almacenar y recuperar datos. Obviamente hay bastantes sucede segundo plano y le echamos un vistazo a que en un momento, pero primero vamos a verlo en acción.

## <a name="4-reading--writing-data"></a>4. Leer y escribir datos

Como se muestra a continuación, implemente el método Main en Program.cs. Este código crea una nueva instancia de nuestro contexto y, a continuación, usa para insertar un nuevo Blog. A continuación, usa una consulta LINQ para recuperar todos los Blogs de la base de datos ordenado alfabéticamente por título.

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

Ahora puede ejecutar la aplicación y probarlo.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>¿Dónde están mis datos?

Por convención DbContext ha creado una base de datos por usted.

-   Si una instancia local de SQL Express está disponible (instalado con Visual Studio 2010 de forma predeterminada), a continuación, Code First ha creado la base de datos en esa instancia
-   Si SQL Express no está disponible, a continuación, Code First se probará ni usará [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (instalado de forma predeterminada con Visual Studio 2012)
-   La base de datos se denomina con el nombre completo del contexto derivado, en nuestro caso es **CodeFirstNewDatabaseSample.BloggingContext**

Estas son simplemente las convenciones predeterminadas y hay varias maneras de cambiar la base de datos que usa Code First, obtener más información está disponible en el **cómo DbContext detecta el modelo y la conexión de base de datos** tema.
Puede conectarse a esta base de datos mediante el Explorador de servidores en Visual Studio

-   **Vista -&gt; Explorador de servidores**
-   Haga clic con el botón derecho en **conexiones de datos** y seleccione **Agregar conexión...**
-   Si aún no lo ha conectado a una base de datos del explorador de servidores antes de que tendrá que seleccionar Microsoft SQL Server como origen de datos

    ![Seleccionar origen de datos](~/ef6/media/selectdatasource.png)

-   Conectarse a LocalDB o Express de SQL, según lo que se ha instalado

Ahora podemos inspeccionar el esquema que Code First crea.

![Esquema inicial](~/ef6/media/schemainitial.png)

DbContext puede averiguar qué clases debe incluir en el modelo echando un vistazo a las propiedades DbSet que hemos definido. A continuación, se usa el conjunto predeterminado de las convenciones de Code First para determinar los nombres de tabla y columna, determinar los tipos de datos, buscar las claves principales, etcetera. Más adelante en este tutorial, buscaremos en cómo puede invalidar estas convenciones.

## <a name="5-dealing-with-model-changes"></a>5. Trabajar con los cambios del modelo

Ahora es momento de realizar algunos cambios en nuestro modelo, cuando se realizan estos cambios que también es necesario para actualizar el esquema de base de datos. Para ello, vamos a usar una característica denominada migraciones o migraciones de Code First para abreviar.

Las migraciones nos permite tener un conjunto ordenado de los pasos que describen cómo actualizar el esquema de base de datos (y cambiar). Cada uno de estos pasos, que se conoce como una migración, contiene código que se describe los cambios que se aplicará. 

El primer paso es habilitar migraciones de Code First para nuestro BloggingContext.

-   **Herramientas -&gt; Administrador de paquetes de biblioteca -&gt; consola de administrador de paquetes**
-   Ejecute el comando **Enable-Migrations** en la consola del Administrador de paquetes
-   Se ha agregado una nueva carpeta migraciones a nuestro proyecto que contiene dos elementos:
    -   **Configuration.cs** – este archivo contiene la configuración que se va a usar para migrar las migraciones BloggingContext. No es necesario cambiar nada en este tutorial, pero aquí es donde puede especificar datos de inicialización, los proveedores de registro para otras bases de datos, cambian el espacio de nombres que se generan las migraciones en etc.
    -   **&lt;marca de tiempo&gt;\_InitialCreate.cs** : se trata de la primera migración, representa los cambios que ya se han aplicado a la base de datos a partir de la que se va a una base de datos vacío a uno que incluye las tablas de Blogs y publicaciones . Aunque nos dejó que Code First crea automáticamente estas tablas para nosotros, ahora que nos hemos suscrito a las migraciones se han convertido en una migración. En primer lugar código también se registra en nuestra base de datos local que se ha aplicado esta migración. La marca de tiempo en el nombre de archivo se usa para fines de ordenación.

    Ahora vamos a realizar un cambio en nuestro modelo, agregue una propiedad de dirección Url a la clase de Blog:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Ejecute el **Add-Migration AddUrl** comando en la consola de administrador de paquetes.
    El comando Add-Migration comprueba los cambios desde la última migración y aplica la técnica scaffolding una migración nueva con los cambios que se encuentran. Podemos poner un nombre; las migraciones en este caso estamos llamando a la migración 'AddUrl'.
    El código con scaffolding está diciendo que tenemos que agregar una columna de dirección Url, que puede contener datos de cadena, el dbo. Tabla de blogs. Si es necesario, nos podríamos modificar el código con scaffolding, pero que no es necesario en este caso.

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

-   Ejecute el **Update-Database** comando en la consola de administrador de paquetes. Este comando aplicará cualquier migración pendiente a la base de datos. Ya se ha aplicado la migración InitialCreate por lo que las migraciones solo aplicará nuestra nueva migración AddUrl.
    Sugerencia: Puede usar el **– Verbose** cambiar cuando se llama a Update-Database para ver el código SQL que se está ejecutando en la base de datos.

Ahora, la nueva columna de dirección Url se agrega a la tabla Blogs en la base de datos:

![Esquema de dirección Url](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. Anotaciones de datos

Hasta ahora hemos solo permitimos que EF descubre el modelo mediante las convenciones de forma predeterminada, pero se va a haber ocasiones nuestras clases no siguen las convenciones y deberá ser capaz de realizar la configuración. Hay dos opciones para este fin; veremos las anotaciones de datos en esta sección y, a continuación, la API fluida en la sección siguiente.

-   Vamos a agregar una clase de usuario a nuestro modelo

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   También es necesario agregar un conjunto a nuestro contexto derivado

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Si se ha intentado agregar una migración, se obtendría un error que dice que "*EntityType 'User' no tiene ninguna clave definida. Definir la clave para este EntityType."* Dado que EF no tiene ninguna manera de saber que el nombre de usuario debe ser la clave principal de usuario.
-   Vamos a usar ahora las anotaciones de datos por lo que necesitamos agregar un mediante declaración en la parte superior de Program.cs

```
using System.ComponentModel.DataAnnotations;
```

-   Ahora anotar la propiedad de nombre de usuario para identificar que es la clave principal

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Use la **Add-Migration AddUser** comando para aplicar scaffolding a una migración para aplicar estos cambios en la base de datos
-   Ejecute el **Update-Database** comando para aplicar la nueva migración a la base de datos

La nueva tabla se agrega ahora a la base de datos:

![Esquema con los usuarios](~/ef6/media/schemawithusers.png)

La lista completa de anotaciones compatibles con EF es:

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. API fluida

En la sección anterior, analizamos usando anotaciones de datos para complementar o reemplazar lo que se detectó por convención. La otra forma de configurar el modelo es a través de la API fluida de Code First.

La mayoría de modelo de configuración se puede realizar mediante las anotaciones de datos simple. La API fluida es una manera de especificar la configuración del modelo que cubre todo lo que las anotaciones de datos pueden hacer además a alguna configuración más avanzada que no son posible con las anotaciones de datos más avanzada. Las anotaciones de datos y la API fluida se pueden usar conjuntamente.

Para obtener acceso a la API fluida de invalidar el método OnModelCreating en DbContext. Supongamos que deseamos cambiar el nombre de la columna que User.DisplayName se almacena en Mostrar\_nombre.

-   Invalide el método OnModelCreating en BloggingContext con el código siguiente

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

-   Use la **Add-Migration ChangeDisplayName** comando para aplicar scaffolding a una migración para aplicar estos cambios en la base de datos.
-   Ejecute el **Update-Database** comando para aplicar la nueva migración a la base de datos.

La columna DisplayName ahora se cambia el nombre para mostrar\_nombre:

![Esquema con nombre cambiado el nombre para mostrar](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Resumen

En este tutorial analizamos desarrollo Code First con una base de datos. Se ha definido un modelo mediante clases, a continuación, usa ese modelo para crear una base de datos y almacenar y recuperar datos. Una vez que se creó la base de datos se usan migraciones de Code First para cambiar el esquema como nuestro modelo evolucionado. También hemos visto cómo configurar un modelo mediante las anotaciones de datos y la API Fluent.
