---
title: Code First en una base de datos existente - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: f05420beb3dff2d632151fcbf48986b0d9cd18ff
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490615"
---
# <a name="code-first-to-an-existing-database"></a>Code First en una base de datos existente
En este tutorial de vídeo y paso a paso proporcionan una introducción al desarrollo de Code First destinadas a una base de datos existente. Código primero le permite definir su modelo mediante C\# o clases de VB.Net. Configuración adicional si lo desea puede realizarse mediante los atributos en las clases y propiedades o mediante una API fluida.

## <a name="watch-the-video"></a>Vea el vídeo
Este vídeo es [ya está disponible en Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Requisitos previos

Deberá tener **Visual Studio 2012** o **Visual Studio 2013** instalado para completar este tutorial.

También necesitará versión **6.1** (o posterior) de la **Entity Framework Tools para Visual Studio** instalado. Consulte [obtener Entity Framework](~/ef6/fundamentals/install.md) para obtener información acerca de cómo instalar la versión más reciente de las herramientas de Entity Framework.

## <a name="1-create-an-existing-database"></a>1. Crear una base de datos existente

Normalmente, cuando tiene como destino una base de datos existente que ya se creará, pero para este tutorial es necesario crear una base de datos para tener acceso a.

Sigamos adelante y generar la base de datos.

-   Apertura de Visual Studio
-   **Vista -&gt; Explorador de servidores**
-   Haga clic con el botón derecho en **conexiones de datos -&gt; Agregar conexión...**
-   Si aún no lo ha conectado a una base de datos **Explorador de servidores** antes de que tendrá que seleccionar **Microsoft SQL Server** como origen de datos

    ![Seleccionar origen de datos](~/ef6/media/selectdatasource.png)

-   Conectarse a la instancia de LocalDB y escriba **blogs** como el nombre de la base de datos

    ![Conexión de LocalDB](~/ef6/media/localdbconnection.png)

-   Seleccione **Aceptar** y se le solicitará si desea crear una nueva base de datos, seleccione **sí**

    ![Crear el cuadro de diálogo de base de datos](~/ef6/media/createdatabasedialog.png)

-   La nueva base de datos aparecerá ahora en el Explorador de servidores, haga doble clic en él y seleccione **nueva consulta**
-   Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **Execute**

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

## <a name="2-create-the-application"></a>2. Crear la aplicación

Para simplificar las cosas que vamos a crear una aplicación de consola básica que usa Code First para tener acceso a datos:

-   Apertura de Visual Studio
-   **Archivo -&gt; nuevo:&gt; proyecto...**
-   Seleccione **Windows** en el menú izquierdo y **aplicación de consola**
-   Escriba **CodeFirstExistingDatabaseSample** como el nombre
-   Seleccione **Aceptar**.

 

## <a name="3-reverse-engineer-model"></a>3. Modelo de ingeniería inversa

Vamos a usar Entity Framework Tools para Visual Studio para ayudarnos a generar código para asignar a la base de datos inicial. Estas herramientas solo genera código que también podría escribir a mano si lo prefiere.

-   **Proyecto -&gt; Agregar nuevo elemento...**
-   Seleccione **datos** en el menú izquierdo y, a continuación, **ADO.NET Entity Data Model**
-   Escriba **BloggingContext** como el nombre y haga clic en **Aceptar**
-   Esto inicia el **Asistente para Entity Data Model**
-   Seleccione **Code First de base de datos** y haga clic en **siguiente**

    ![Un CFE Asistente](~/ef6/media/wizardonecfe.png)

-   Seleccione la conexión a la base de datos que creó en la primera sección y haga clic en **siguiente**

    ![Asistente para dos CFE](~/ef6/media/wizardtwocfe.png)

-   Haga clic en la casilla de verificación junto a **tablas** para importar todas las tablas y haga clic en **finalizar**

    ![Asistente para tres CFE](~/ef6/media/wizardthreecfe.png)

Una vez que un número de elementos de completa el proceso de ingeniería inversa habrá agregado al proyecto, vamos a Eche un vistazo a lo que se ha agregado.

### <a name="configuration-file"></a>Archivo de configuración

Se agregó un archivo App.config al proyecto, este archivo contiene la cadena de conexión a la base de datos existente.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Observará algunos otros valores en el archivo de configuración demasiado, se trata de configuración de EF predeterminada que indican a Code First dónde crear bases de datos. Puesto que vamos a asignar a una base de datos existente, esta configuración se omitirá en nuestra aplicación.*

### <a name="derived-context"></a>Contexto derivado

Un **BloggingContext** ha agregado la clase al proyecto. El contexto representa una sesión con la base de datos, lo que nos permite consultar y guardar los datos.
Expone el contexto de un **DbSet&lt;TEntity&gt;**  para cada tipo de nuestro modelo. También podrá observar que el constructor predeterminado llama a un constructor base con el **nombre =** sintaxis. Code First indica que la cadena de conexión que se utilizará para este contexto se debe cargar desde el archivo de configuración.

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

*Siempre debe usar el **nombre =** sintaxis cuando se usa una cadena de conexión en el archivo de configuración. Esto garantiza que si la cadena de conexión no está presente, a continuación, Entity Framework arrojará en lugar de crear una nueva base de datos por convención.*

### <a name="model-classes"></a>Clases de modelo

Por último, un **Blog** y **Post** clase también se han agregado al proyecto. Estas son las clases de dominio que conforman nuestro modelo. Podrá ver las anotaciones de datos se aplica a las clases para especificar la configuración donde las convenciones de Code First no estaría en consonancia con la estructura de la base de datos existente. Por ejemplo, verá el **StringLength** anotación en **Blog.Name** y **Blog.Url** porque tienen una longitud máxima de **200** en el base de datos (el valor de Code First predeterminado consiste en utilizar la longitud máximo admitida por el proveedor de base de datos - **nvarchar (max)** en SQL Server).

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

## <a name="4-reading--writing-data"></a>4. Leer y escribir datos

Ahora que tenemos un modelo es el momento de usarlo para acceder a algunos datos. Implemente el **Main** método **Program.cs** tal como se muestra a continuación. Este código crea una nueva instancia de nuestro contexto y, a continuación, se usa para insertar un nuevo **Blog**. A continuación, usa una consulta LINQ para recuperar todos **Blogs** desde la base de datos ordenado alfabéticamente por **título**.

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
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>¿Qué ocurre si mi base de datos los cambios?

Code First al Asistente para la base de datos está diseñada para generar un conjunto de punto de partida de las clases que puede, a continuación, ajustar y modificar. Si cambia el esquema de base de datos puede manualmente editar las clases o realizar otra ingeniería inversa para sobrescribir las clases.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Uso de migraciones de Code First para una base de datos existente

Si desea usar migraciones de Code First con una base de datos existente, vea [migraciones de Code First para una base de datos](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Resumen

En este tutorial analizamos desarrollo Code First con una base de datos existente. Se usan Entity Framework Tools para Visual Studio para un conjunto de clases que asignan a la base de datos y podría utilizarse para almacenar y recuperar datos de ingeniería inversa.
