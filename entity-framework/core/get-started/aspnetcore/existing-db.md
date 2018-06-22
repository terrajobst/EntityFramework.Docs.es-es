---
title: 'Introducción a ASP.NET Core: base de datos existente - EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: db2469d0badd428734425c1f568667f00bef2f4f
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2018
ms.locfileid: "30151019"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Introducción a EF Core en ASP.NET Core con una base de datos existente

En este tutorial, compilará una aplicación de MVC de ASP.NET Core que realiza el acceso a datos básicos mediante Entity Framework. Usará técnicas de ingeniería inversa para crear un modelo de Entity Framework en función de una base de datos existente.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) de este artículo en GitHub.

## <a name="prerequisites"></a>Requisitos previos

Deberá cumplir los requisitos previos siguientes para completar este tutorial:

* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) con estas cargas de trabajo:
  * **Desarrollo de ASP.NET y web** (en **Web y nube**)
  * **Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)
* [SDK de .NET Core 2.0](https://www.microsoft.com/net/download/core).
* [Base de datos para blogs](#blogging-database)

### <a name="blogging-database"></a>Base de datos para blogs

En este tutorial se usa una base de datos para **blogs** en la instancia de LocalDB como la base de datos existente.

> [!TIP]  
> Si ya creó la base de datos de **blogs** como parte de otro tutorial, puede omitir estos pasos.

* Apertura de Visual Studio
* **Herramientas -> Conectar con base de datos...**
* Seleccione **Microsoft SQL Server** y haga clic en **Continuar**
* Escriba **(localdb)\mssqllocaldb** como el **nombre del servidor**
* Escriba **maestra** como el **nombre de la base de datos** y haga clic en **Aceptar**
* La base de datos maestra ahora aparece en **Conexiones de base de datos** del **Explorador de servidores**
* Haga clic con el botón derecho en la base de datos en el **Explorador de servidores** y seleccione **Nueva consulta**
* En el editor de consultas, copie el script que aparece a continuación.
* Haga clic con el botón derecho en el editor de consultas y seleccione **Ejecutar**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Crear un proyecto nuevo

* Abra Visual Studio 2017
* **Archivo -> Nuevo -> Proyecto...**
* En el menú de la izquierda, seleccione **Instalado -> Plantillas -> Visual C# -> Web**
* Seleccione la plantilla de proyecto **Aplicación web de ASP.NET Core (.NET Core)**
* Escriba **EFGetStarted.AspNetCore.ExistingDb** como el nombre y haga clic en **Aceptar**
* Espere que aparezca el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**
* En **ASP.NET Core Templates 2.0** (Plantillas 2.0 de ASP.NET Core), seleccione la **Aplicación web (Model-View-Controller)**
* Asegúrese de que la **autenticación** esté establecida en **Sin autenticación**.
* Haga clic en **Aceptar**.

## <a name="install-entity-framework"></a>Instalación de Entity Framework

Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse. Este tutorial usa SQL Server. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).

* **Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Usaremos parte de Entity Framework Tools para crear un modelo desde la base de datos. Por lo tanto, también instalaremos el paquete de herramientas:

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.

Usaremos las herramientas de scaffolding de ASP.NET Core para crear más adelante controladores y vistas. Por lo tanto, también instalaremos el paquete de diseño:

* Ejecute `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`.

## <a name="reverse-engineer-your-model"></a>Utilización de técnicas de ingeniería inversa para el modelo

Ya es momento de crear el modelo de EF en función de la base de datos existente.

* **Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**
* Ejecute el comando siguiente para crear un modelo a partir de la base de datos existente:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Si recibe un error que indica `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, cierre Visual Studio y vuelva a abrirlo.

> [!TIP]  
> Para especificar las tablas para las cuales desea generar entidades, agregue el argumento `-Tables` al comando anterior. P. ej., `-Tables Blog,Post`.

El proceso de ingeniería inversa creó clases de entidad (`Blog.cs` & `Post.cs`) y un contexto derivado (`BloggingContext.cs`) en función del esquema de la base de datos existente.

 Las clases de entidad son simples objetos de C# que representan los datos que va a consultar y guardar.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 El contexto representa una sesión con la base de datos y le permite consultar y guardar instancias de las clases de entidad.

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a>Registro del contenido con inserción de dependencias

El concepto de la inserción de dependencias es fundamental para ASP.NET Core. Los servicios (como `BloggingContext`) se registran con inserción de dependencias durante el inicio de la aplicación. Luego se proporcionan estos servicios a los componentes que los necesitan (como los controladores MVC) a través de propiedades o parámetros de constructor. Para más información sobre la inserción de dependencias, consulte el artículo sobre la [inserción de dependencias](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) en el sitio de ASP.NET.

### <a name="remove-inline-context-configuration"></a>Eliminación de la configuración en contexto alineada

En ASP.NET Core, la configuración se realiza generalmente en **Startup.cs**. Para cumplir con este patrón, migraremos la configuración del proveedor de bases de datos a **Startup.cs**.

* Abrir `Models\BloggingContext.cs`
* Eliminación del método `OnConfiguring(...)`

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Agregue el constructor siguiente, lo que permitirá que la inserción de dependencias pase la configuración al contexto

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Registro y configuración del contexto en Startup.cs

Con el fin de que los controladores MVC usen `BloggingContext`, lo registraremos como servicio.

* Abra **Startup.cs**
* Agregue las instrucciones `using` siguientes en el comienzo del archivo

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Ahora podemos usar el método `AddDbContext(...)` para registrarlo como servicio.
* Ubique el método `ConfigureServices(...)`
* Agregue el código siguiente para registrar el contexto como servicio

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> En una aplicación real, habitualmente colocaría la cadena de conexión en un archivo de configuración. Por simplicidad, se define en el código. Para más información, consulte [Cadenas de conexión](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller"></a>Creación de un controlador

A continuación, habilitaremos scaffolding en el proyecto.

* Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar -> Controlador...**
* Seleccione **Dependencias completas** y haga clic en **Agregar**
* Puede omitir las instrucciones que aparecen en el archivo `ScaffoldingReadMe.txt` que se abre

Ahora que se habilitó scaffolding, podemos aplicarlo a un controlador de la entidad `Blog`.

* Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar -> Controlador...**
* Seleccione **Controlador de MVC con vistas que usan Entity Framework** y haga clic en **Aceptar**
* Establezca la **clase de modelo** en **Blog** y **Clase de contexto de datos** en **BloggingContext**
* Haga clic en **Agregar**

## <a name="run-the-application"></a>Ejecutar la aplicación

Ahora puede ejecutar la aplicación para verla en acción.

* **Depurar -> Iniciar sin depuración**
* La aplicación se compilará y abrirá en un explorador web
* Navegue a `/Blogs`
* Haga clic en **Crear nuevo**
* Escriba una **dirección URL** para el blog nuevo y haga clic en **Crear**

![imagen](_static/create.png)

![imagen](_static/index-existing-db.png)
