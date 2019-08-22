---
title: 'Introducción a ASP.NET Core: base de datos existente - EF Core'
author: rowanmiller
description: Introducción a EF Core en ASP.NET Core con una base de datos existente
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: eeebd75bebe85994c6439e06243e113f2bda814c
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565232"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Introducción a EF Core en ASP.NET Core con una base de datos existente

En este tutorial compilará una aplicación de MVC de ASP.NET Core que realiza el acceso a datos básicos mediante Entity Framework Core. Para crear un modelo de Entity Framework, usará ingeniería inversa en una base de datos existente.

[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Requisitos previos

Instale el software siguiente:

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) con estas cargas de trabajo:
  * **Desarrollo de ASP.NET y web** (en **Web y nube**)
  * **Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core).

## <a name="create-blogging-database"></a>Creación de una base de datos para blogs

En este tutorial se usa una base de datos para **blogs** en la instancia de LocalDB como la base de datos existente. Si ya creó la base de datos de **blogs** como parte de otro tutorial, omita estos pasos.

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
* **Archivo > Nuevo > Proyecto...**
* En el menú de la izquierda, seleccione **Instalado > Visual C# > Web**.
* Seleccione la plantilla de proyecto **Aplicación web ASP.NET Core**.
* Escriba **EFGetStarted.AspNetCore.ExistingDb** como nombre (tiene que coincidir con exactamente el espacio de nombres que se usa más adelante en el código) y haga clic en **Aceptar** 
* Espere que aparezca el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**
* Asegúrese de que el menú desplegable de la plataforma de destino esté establecido en **.NET Core** y de que el menú desplegable de versión esté establecido en **ASP.NET Core 2.1**.
* Seleccione la plantilla **Aplicación web (Model-View-Controller)** .
* Asegúrese de que la **autenticación** esté establecida en **Sin autenticación**.
* Haga clic en **Aceptar**.

## <a name="install-entity-framework-core"></a>Instalación de Entity Framework Core

Para instalar EF Core, instale el paquete de los proveedores de bases de datos de EF Core que quiera establecer como destino. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md). 

Para este tutorial no tendrá que instalar ningún paquete de proveedor porque se usa SQL Server. El paquete de proveedor de SQL Server está incluido en el metapaquete [Microsoft.AspNetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="reverse-engineer-your-model"></a>Utilización de técnicas de ingeniería inversa para el modelo

Ya es momento de crear el modelo de EF en función de la base de datos existente.

* **Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**
* Ejecute el comando siguiente para crear un modelo a partir de la base de datos existente:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Si recibe un error que indica `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, cierre Visual Studio y vuelva a abrirlo.

> [!TIP]  
> Para especificar las tablas para las cuales desea generar entidades, agregue el argumento `-Tables` al comando anterior. Por ejemplo: `-Tables Blog,Post`.

El proceso de ingeniería inversa creó clases de entidad (`Blog.cs` & `Post.cs`) y un contexto derivado (`BloggingContext.cs`) en función del esquema de la base de datos existente.

 Las clases de entidad son simples objetos de C# que representan los datos que va a consultar y guardar. Estas son las clases de entidad `Blog` y `Post`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Para habilitar la carga diferida, puede usar las propiedades de navegación `virtual` (Blog.Post y Post.Blog).

 El contexto representa una sesión con la base de datos y le permite consultar y guardar instancias de las clases de entidad.

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

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

### <a name="register-and-configure-your-context-in-startupcs"></a>Registro y configuración del contexto en Startup.cs

Para que `BloggingContext` esté disponible para los controladores MVC, debe registrarlo como servicio.

* Abra **Startup.cs**
* Agregue las instrucciones `using` siguientes en el comienzo del archivo

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Ahora puede usar el método `AddDbContext(...)` para registrarlo como servicio.
* Ubique el método `ConfigureServices(...)`
* Agregue el código siguiente resaltado para registrar el contexto como servicio.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> En una aplicación real, lo habitual sería colocar la cadena de conexión en un archivo de configuración o una variable de entorno. Por simplicidad, en este tutorial se define en código. Para más información, consulte [Cadenas de conexión](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller-and-views"></a>Crear un controlador y vistas

* Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar -> Controlador...**
* Seleccione **Controlador de MVC con vistas que usan Entity Framework** y haga clic en **Aceptar**
* Establezca la **clase de modelo** en **Blog** y **Clase de contexto de datos** en **BloggingContext**
* Haga clic en **Agregar**

## <a name="run-the-application"></a>Ejecutar la aplicación

Ahora puede ejecutar la aplicación para verla en acción.

* **Depurar -> Iniciar sin depuración**
* La aplicación se compila y se abre en un explorador web.
* Navegue a `/Blogs`.
* Haga clic en **Crear nuevo**
* Escriba una **dirección URL** para el blog nuevo y haga clic en **Crear**

  ![Página Crear](_static/create.png)

  ![Página de índice](_static/index-existing-db.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo aplicar scaffolding a un contexto y a clases de entidad, vea los siguientes artículos:
* [Ingeniería inversa](xref:core/managing-schemas/scaffolding)
* [Referencia sobre las herramientas de Entity Framework Core (CLI de .NET)](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Referencia sobre las herramientas de Entity Framework Core (Consola del Administrador de paquetes)](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
