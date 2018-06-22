---
title: 'Introducción a .NET Framework: base de datos existente - EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812630"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Introducción a EF Core en .NET Framework con una base de datos existente

En este tutorial, compilará una aplicación de consola que realiza el acceso a datos básicos en una base de datos de Microsoft SQL Server mediante Entity Framework. Usará técnicas de ingeniería inversa para crear un modelo de Entity Framework en función de una base de datos existente.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) de este artículo en GitHub.

## <a name="prerequisites"></a>Requisitos previos

Deberá cumplir los requisitos previos siguientes para completar este tutorial:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [La versión más reciente del Administrador de paquetes de NuGet](https://dist.nuget.org/index.html)

* [La versión más reciente de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Base de datos para blogs](#blogging-database)

### <a name="blogging-database"></a>Base de datos para blogs

En este tutorial se usa una base de datos para **blogs** en la instancia de LocalDB como la base de datos existente.

> [!TIP]  
> Si ya creó la base de datos de **blogs** como parte de otro tutorial, puede omitir estos pasos.

* Apertura de Visual Studio

* Herramientas -> Conectar con base de datos...

* Seleccione **Microsoft SQL Server** y haga clic en **Continuar**

* Escriba **(localdb)\mssqllocaldb** como el **nombre del servidor**

* Escriba **maestra** como el **nombre de la base de datos** y haga clic en **Aceptar**

* La base de datos maestra ahora aparece en **Conexiones de base de datos** del **Explorador de servidores**

* Haga clic con el botón derecho en la base de datos en el **Explorador de servidores** y seleccione **Nueva consulta**

* En el editor de consultas, copie el script que aparece a continuación.

* Haga clic con el botón derecho en el editor de consultas y seleccione **Ejecutar**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Crear un proyecto nuevo

* Apertura de Visual Studio

* Archivo > Nuevo > Proyecto...

* En el menú de la izquierda, seleccione Plantillas > Visual C# > Windows

* Seleccione la plantilla de proyecto **Aplicación de consola**.

* Asegúrese de tener como destino **.NET Framework 4.5.1** o una versión posterior

* Asigne un nombre al proyecto y haga clic en **Aceptar**

## <a name="install-entity-framework"></a>Instalación de Entity Framework

Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse. Este tutorial usa SQL Server. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).

* Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Para habilitar técnicas de ingeniería inversa desde una base de datos existente, es necesario instalar también un par de otros paquetes.

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.

## <a name="reverse-engineer-your-model"></a>Utilización de técnicas de ingeniería inversa para el modelo

Ya es momento de crear el modelo de EF en función de la base de datos existente.

* Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes

* Ejecute el comando siguiente para crear un modelo a partir de la base de datos existente

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

El proceso de ingeniería inversa creó clases de entidad y un contexto derivado en función del esquema de la base de datos existente. Las clases de entidad son simples objetos de C# que representan los datos que va a consultar y guardar.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

El contexto representa una sesión con la base de datos y le permite consultar y guardar instancias de las clases de entidad.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
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

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a>Uso del modelo

Ahora puede usar el modelo para realizar el acceso a los datos.

* Abra *Program.cs*.

* Reemplace el contenido del archivo por el código siguiente

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* Depurar > Iniciar sin depuración

Verá que un blog se guarda en la base de datos y luego los detalles de todos los blogs se imprimen en la consola.

![imagen](_static/output-existing-db.png)
