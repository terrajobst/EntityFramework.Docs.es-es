---
title: Model First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 3dd0eba29619f09995d7009dd29462c14bde98c4
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251146"
---
# <a name="model-first"></a>En primer lugar del modelo
En este tutorial de vídeo y paso a paso proporcionan una introducción al desarrollo de Model First mediante Entity Framework. Modelo primero le permite crear un nuevo modelo con Entity Framework Designer y, a continuación, generar un esquema de base de datos del modelo. El modelo se almacena en un archivo EDMX (extensión edmx) y se puede ver y editar en el Diseñador de Entity Framework. Las clases que interactúan en la aplicación se generan automáticamente desde el archivo EDMX.

## <a name="watch-the-video"></a>Vea el vídeo
En este tutorial de vídeo y paso a paso proporcionan una introducción al desarrollo de Model First mediante Entity Framework. Modelo primero le permite crear un nuevo modelo con Entity Framework Designer y, a continuación, generar un esquema de base de datos del modelo. El modelo se almacena en un archivo EDMX (extensión edmx) y se puede ver y editar en el Diseñador de Entity Framework. Las clases que interactúan en la aplicación se generan automáticamente desde el archivo EDMX.

**Presentado por**: [Rowan Miller](http://romiller.com/)

**Vídeo**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Requisitos previos

Necesitará tener Visual Studio 2010 o Visual Studio 2012 instalado para completar este tutorial.

Si utiliza Visual Studio 2010, también necesitará tener [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) instalado.

## <a name="1-create-the-application"></a>1. Crear la aplicación

Para simplificar las cosas que vamos a crear una aplicación de consola básica que usa Model First para tener acceso a datos:

-   Apertura de Visual Studio
-   **Archivo -&gt; nuevo:&gt; proyecto...**
-   Seleccione **Windows** en el menú izquierdo y **aplicación de consola**
-   Escriba **ModelFirstSample** como el nombre
-   Seleccione **Aceptar**.

## <a name="2-create-model"></a>2. Crear modelo

Vamos a usar Entity Framework Designer, que se incluye como parte de Visual Studio, para crear nuestro modelo.

-   **Proyecto -&gt; Agregar nuevo elemento...**
-   Seleccione **datos** en el menú izquierdo y, a continuación, **ADO.NET Entity Data Model**
-   Escriba **BloggingModel** como el nombre y haga clic en **Aceptar**, esto inicia el Asistente para Entity Data Model
-   Seleccione **modelo vacío** y haga clic en **finalizar**

    ![Crear un modelo vacío](~/ef6/media/createemptymodel.png)

Se abre el Diseñador de Entity Framework con un modelo en blanco. Ahora podemos comenzar a agregar entidades, propiedades y asociaciones en el modelo.

-   Haga doble clic en la superficie de diseño y seleccione **propiedades**
-   En el cambio de la ventana de propiedades el **Entity Container Name** a **BloggingContext**
    *es el nombre del contexto derivado que se generarán para usted, el contexto Representa una sesión con la base de datos, lo que nos permite consultar y guardar los datos*
-   Haga doble clic en la superficie de diseño y seleccione **Add New -&gt; entidad...**
-   Escriba **Blog** como el nombre de entidad y **BlogId** como el nombre de clave y haga clic en **Aceptar**

    ![Agregar entidad Blog](~/ef6/media/addblogentity.png)

-   Haga doble clic en la nueva entidad en la superficie de diseño y seleccione **Add New -&gt; propiedad escalar**, escriba **nombre** como el nombre de la propiedad.
-   Repita este proceso para agregar un **Url** propiedad.
-   Haga doble clic en el **Url** propiedad en la superficie de diseño y seleccione **propiedades**, en el cambio de la ventana de propiedades el **Nullable** si se establece en **True** 
     *Esto nos permite guardar un Blog en la base de datos sin asignarle una dirección Url*
-   Mediante las técnicas que acaba de aprender, agregue un **Post** entidad con un **PostId** propiedad clave
-   Agregar **título** y **contenido** las propiedades escalares del **Post** entidad

Ahora que tenemos un par de entidades, es momento de agregar una asociación (o una relación) entre ellos.

-   Haga doble clic en la superficie de diseño y seleccione **Add New -&gt; asociación...**
-   Hacer que apunte a un extremo de la relación **Blog** con una multiplicidad de **una** y el otro punto final para **Post** con una multiplicidad de **muchos** 
     *Esto significa que un Blog tiene todas las entradas y una entrada pertenezca a un Blog*
-   Asegúrese del **agregar propiedades de clave externa a la entidad 'Post'** está activada y haga clic en **Aceptar**

    ![Agregar asociación MF](~/ef6/media/addassociationmf.png)

Ahora tenemos un modelo simple que podemos usar para leer y escribir datos y generar una base de datos.

![Modelo inicial](~/ef6/media/modelinitial.png)

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

    ![Plantilla de DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. Generar la base de datos

Dado que nuestro modelo, Entity Framework puede calcular un esquema de base de datos que nos permitirá almacenar y recuperar datos mediante el modelo.

El servidor de base de datos que se instala con Visual Studio es diferente según la versión de Visual Studio que ha instalado:

-   Si usa Visual Studio 2010 se creará una base de datos de SQL Express.
-   Si usa Visual Studio 2012, a continuación, se creará un [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) base de datos.

Sigamos adelante y generar la base de datos.

-   Haga doble clic en la superficie de diseño y seleccione **generar base de datos de modelo...**
-   Haga clic en **nueva conexión...** y especifique LocalDB o Express de SQL, según la versión de Visual Studio que está utilizando, escriba **ModelFirst.Blogging** como el nombre de la base de datos.

    ![Conexión de LocalDB MF](~/ef6/media/localdbconnectionmf.png)

    ![Conexión de SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   Seleccione **Aceptar** y se le solicitará si desea crear una nueva base de datos, seleccione **sí**
-   Seleccione **siguiente** y Entity Framework Designer calculará una secuencia de comandos para crear el esquema de base de datos
-   Una vez que se muestra la secuencia de comandos, haga clic en **finalizar** y se agregará al proyecto y abrir la secuencia de comandos
-   Haga doble clic en el script y seleccione **Execute**, se le pedirá que especifique la base de datos para conectarse a, especifique LocalDB o Express de SQL Server, según la versión de Visual Studio que está utilizando

## <a name="4-reading--writing-data"></a>4. Leer y escribir datos

Ahora que tenemos un modelo es el momento de usarlo para acceder a algunos datos. Las clases que vamos a usar para acceder a los datos se generan automáticamente para usted según el archivo EDMX.

*Esta captura de pantalla es de Visual Studio 2012, si usa Visual Studio 2010 el BloggingModel.tt y BloggingModel.Context.tt archivos estarán directamente en el proyecto en lugar de anidan en el archivo EDMX.*

![Clases generadas](~/ef6/media/generatedclasses.png)

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

## <a name="5-dealing-with-model-changes"></a>5. Trabajar con los cambios del modelo

Ahora es momento de realizar algunos cambios en nuestro modelo, cuando se realizan estos cambios que también es necesario para actualizar el esquema de base de datos.

Comenzaremos por agregar una nueva entidad de usuario a nuestro modelo.

-   Agregue un nuevo **usuario** nombre de la entidad con **Username** como el nombre de clave y **cadena** como el tipo de propiedad para la clave

    ![Agregar entidad de usuario](~/ef6/media/adduserentity.png)

-   Haga doble clic en el **Username** propiedad en la superficie de diseño y seleccione **propiedades**, cambio de las propiedades en la ventana la **MaxLength** si se establece en **50 ** 
     *Esto restringe los datos que se pueden almacenar en el nombre de usuario y 50 caracteres*
-   Agregar un **DisplayName** propiedad escalar a la **usuario** entidad

Ahora tenemos un modelo actualizado y estamos preparados actualizar la base de datos para dar cabida a nuestro nuevo tipo de entidad de usuario.

-   Haga doble clic en la superficie de diseño y seleccione **generar base de datos de modelo...** , Entity Framework calculará una secuencia de comandos para volver a crear un esquema basado en el modelo actualizado.
-   Haga clic en **finalizar**
-   Puede recibir advertencias sobre la sobrescritura de la secuencia de comandos DDL existente y las partes de asignación y almacenamiento del modelo, haga clic en **Sí** tanto estas advertencias
-   Se abre el script actualizado de SQL para crear la base de datos de  
    *El script generado quitará todas las tablas existentes y, a continuación, volver a crear el esquema desde el principio. Esto puede funcionar para el desarrollo local, pero no es un viable para insertar los cambios en una base de datos que ya se ha implementado. Si necesita publicar los cambios en una base de datos que ya se ha implementado, deberá modificar la secuencia de comandos o usar una herramienta de comparación de esquemas para calcular un script de migración.*
-   Haga doble clic en el script y seleccione **Execute**, se le pedirá que especifique la base de datos para conectarse a, especifique LocalDB o Express de SQL Server, según la versión de Visual Studio que está utilizando

## <a name="summary"></a>Resumen

En este tutorial que analizamos el desarrollo de Model First, que nos permitió crear un modelo en el Diseñador de EF y, a continuación, generar una base de datos de ese modelo. A continuación, se usa el modelo para leer y escribir algunos datos de la base de datos. Por último, se actualizó el modelo y, a continuación, volver a crear el esquema de base de datos para que coincida con el modelo.
