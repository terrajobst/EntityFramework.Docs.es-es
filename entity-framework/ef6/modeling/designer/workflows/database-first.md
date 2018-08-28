---
title: La base de datos en primer lugar - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: c60108c09fcbaaa1f86e77fa52cb13fe018975e1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995915"
---
# <a name="database-first"></a>En primer lugar la base de datos
En este tutorial de vídeo y paso a paso proporcionan una introducción al desarrollo de primera base de datos mediante Entity Framework. Base de datos en primer lugar le permite aplicar ingeniería inversa a un modelo a partir de una base de datos existente. El modelo se almacena en un archivo EDMX (extensión edmx) y se puede ver y editar en el Diseñador de Entity Framework. Las clases que interactúan en la aplicación se generan automáticamente desde el archivo EDMX.

## <a name="watch-the-video"></a>Vea el vídeo
Este vídeo ofrece una introducción al desarrollo de primera base de datos mediante Entity Framework. Base de datos en primer lugar le permite aplicar ingeniería inversa a un modelo a partir de una base de datos existente. El modelo se almacena en un archivo EDMX (extensión edmx) y se puede ver y editar en el Diseñador de Entity Framework. Las clases que interactúan en la aplicación se generan automáticamente desde el archivo EDMX.

**Presentado por**: [Rowan Miller](http://romiller.com/)

**Vídeo**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Requisitos previos

Debe tener al menos Visual Studio 2010 o Visual Studio 2012 instalado para completar este tutorial.

Si utiliza Visual Studio 2010, también necesitará tener [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.

 

## <a name="1-create-an-existing-database"></a>1. Crear una base de datos existente

Normalmente, cuando tiene como destino una base de datos existente que ya se creará, pero para este tutorial es necesario crear una base de datos para tener acceso a.

El servidor de base de datos que se instala con Visual Studio es diferente según la versión de Visual Studio que ha instalado:

-   Si usa Visual Studio 2010 se creará una base de datos de SQL Express.
-   Si usa Visual Studio 2012, a continuación, se creará un [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) base de datos.

 

Sigamos adelante y generar la base de datos.

-   Apertura de Visual Studio
-   **Vista -&gt; Explorador de servidores**
-   Haga clic con el botón derecho en **conexiones de datos -&gt; Agregar conexión...**
-   Si aún no lo ha conectado a una base de datos del explorador de servidores antes de que tendrá que seleccionar Microsoft SQL Server como origen de datos

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   Conectarse a LocalDB o Express de SQL, según lo que se ha instalado y escriba **DatabaseFirst.Blogging** como el nombre de la base de datos

    ![SqlExpressConnectionDF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDBConnectionDF](~/ef6/media/localdbconnectiondf.png)

-   Seleccione **Aceptar** y se le solicitará si desea crear una nueva base de datos, seleccione **sí**

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

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
```

## <a name="2-create-the-application"></a>2. Crear la aplicación

Para simplificar las cosas que vamos a crear una aplicación de consola básica que usa la primera base de datos para tener acceso a datos:

-   Apertura de Visual Studio
-   **Archivo -&gt; nuevo:&gt; proyecto...**
-   Seleccione **Windows** en el menú izquierdo y **aplicación de consola**
-   Escriba **DatabaseFirstSample** como el nombre
-   Seleccione **Aceptar**.

 

## <a name="3-reverse-engineer-model"></a>3. Modelo de ingeniería inversa

Vamos a usar Entity Framework Designer, que se incluye como parte de Visual Studio, para crear nuestro modelo.

-   **Proyecto -&gt; Agregar nuevo elemento...**
-   Seleccione **datos** en el menú izquierdo y, a continuación, **ADO.NET Entity Data Model**
-   Escriba **BloggingModel** como el nombre y haga clic en **Aceptar**
-   Esto inicia el **Asistente para Entity Data Model**
-   Seleccione **generar desde la base de datos** y haga clic en **siguiente**

    ![WizardStep1](~/ef6/media/wizardstep1.png)

-   Seleccione la conexión a la base de datos que creó en la primera sección, escriba **BloggingContext** como el nombre de la cadena de conexión y haga clic en **siguiente**

    ![WizardStep2](~/ef6/media/wizardstep2.png)

-   Haga clic en la casilla de verificación situada junto a 'Tablas' para importar todas las tablas y haga clic en 'Finalizar'

    ![WizardStep3](~/ef6/media/wizardstep3.png)

 

Una vez completado el proceso de ingeniería inversa el nuevo modelo se agrega al proyecto y se abrirá para que pueda ver en el Diseñador de Entity Framework. También se agregó un archivo App.config al proyecto con los detalles de conexión para la base de datos.

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Pasos adicionales en Visual Studio 2010

Si está trabajando en Visual Studio 2010, hay algunos pasos adicionales que debe seguir para actualizar a la versión más reciente de Entity Framework. Actualizar es importante porque proporciona acceso a una superficie de API mejorada, que es mucho más fácil de usar, así como las correcciones de errores más recientes.

En primer lugar, necesitamos obtener la versión más reciente de Entity Framework en NuGet.

-   **Proyecto –&gt; administrar paquetes NuGet... ** 
     *Si no tiene el **administrar paquetes NuGet... ** opción, debe instalar la [versión más reciente de NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Seleccione el **Online** ficha
-   Seleccione el **EntityFramework** paquete
-   Haga clic en **instalar**

A continuación, se debe intercambiar nuestro modelo para generar código que hace uso de la API de DbContext, que se introdujo en versiones posteriores de Entity Framework.

-   Haga doble clic en una zona vacía del modelo de EF Designer y seleccione **Add Code Generation Item...**
-   Seleccione **plantillas en línea** desde el menú izquierdo y busque **DbContext**
-   Seleccione el EF **5.x generador de DbContext para C\#**, escriba **BloggingModel** como el nombre y haga clic en **agregar**

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. Leer y escribir datos

Ahora que tenemos un modelo es el momento de usarlo para acceder a algunos datos. Las clases que vamos a usar para acceder a los datos se generan automáticamente para usted según el archivo EDMX.

*Esta captura de pantalla es de Visual Studio 2012, si usa Visual Studio 2010 el BloggingModel.tt y BloggingModel.Context.tt archivos estarán directamente en el proyecto en lugar de anidan en el archivo EDMX.*

![GeneratedClassesDF](~/ef6/media/generatedclassesdf.png)

 

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
 

## <a name="5-dealing-with-database-changes"></a>5. Trabajar con los cambios de la base de datos

Ahora es momento de realizar algunos cambios en el esquema de base de datos, cuando se realizan estos cambios que también se debe actualizar nuestro modelo para reflejar dichos cambios.

El primer paso es realizar algunos cambios en el esquema de base de datos. Vamos a agregar una tabla de usuarios en el esquema.

-   Haga doble clic en el **DatabaseFirst.Blogging** en el Explorador de servidores de base de datos y seleccione **nueva consulta**
-   Copie el siguiente código SQL en la consulta nueva y, después, haga doble clic en la consulta y seleccione **Execute**

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Ahora que se actualiza el esquema, es momento de actualizar el modelo con esos cambios.

-   Haga doble clic en una zona vacía de su modelo de EF Designer y seleccione "Actualizar modelo desde base de datos...', se iniciará el Asistente para actualizar
-   En la pestaña agregar de la comprobación del Asistente para actualizar la casilla situada junto a las tablas, esto indica que deseamos agregar una tabla nueva desde el esquema.
    *La pestaña de actualización muestra las tablas existentes en el modelo que se comprobará los cambios durante la actualización. Las pestañas de eliminación muestran todas las tablas que se han quitado del esquema y también se quitará del modelo como parte de la actualización. La información de estos dos fichas se detecta automáticamente y se proporciona con propósito informativo únicamente, no se puede cambiar cualquier configuración.*

    ![RefreshWizard](~/ef6/media/refreshwizard.png)

-   Haga clic en Finalizar en el Asistente para actualizar

 

El modelo se ha actualizado para incluir una nueva entidad de usuario que se asigna a la tabla de usuarios que se agregan a la base de datos.

![ModelUpdated](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Resumen

En este tutorial que analizamos el desarrollo de Database First, que nos permitió crear un modelo en el Diseñador de EF en función de una base de datos existente. A continuación, se utiliza ese modelo para leer y escribir algunos datos de la base de datos. Por último, se actualiza el modelo para reflejar los cambios realizados en el esquema de base de datos.
