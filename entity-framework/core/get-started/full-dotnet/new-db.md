---
title: 'Introducción a .NET Framework: base de datos nueva - EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812526"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Introducción a EF Core en .NET Framework con una base de datos nueva

En este tutorial, compilará una aplicación de consola que realiza el acceso a datos básicos en una base de datos de Microsoft SQL Server mediante Entity Framework. Usará las migraciones para crear la base de datos a partir del modelo.

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) de este artículo en GitHub.

## <a name="prerequisites"></a>Requisitos previos

Deberá cumplir los requisitos previos siguientes para completar este tutorial:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [La versión más reciente del Administrador de paquetes de NuGet](https://dist.nuget.org/index.html)

* [La versión más reciente de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a>Crear un proyecto nuevo

* Apertura de Visual Studio

* Archivo > Nuevo > Proyecto...

* En el menú de la izquierda, seleccione Plantillas > Visual C# > Escritorio clásico de Windows

* Seleccione la plantilla del proyecto **Aplicación de consola (.NET Framework)**

* Asegúrese de tener como destino **.NET Framework 4.5.1** o una versión posterior

* Asigne un nombre al proyecto y haga clic en **Aceptar**

## <a name="install-entity-framework"></a>Instalación de Entity Framework

Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse. Este tutorial usa SQL Server. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).

* Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Más adelante en este tutorial también usaremos parte de Entity Framework Tools para mantener la base de datos. Por lo tanto, también instalaremos el paquete de herramientas.

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.

## <a name="create-your-model"></a>Creación del modelo

Es momento de definir un contexto y clases de entidad que constituyan el modelo.

* Proyecto > Agregar clase...

* Escriba *Model.cs* como el nombre y haga clic en **Aceptar**

* Reemplace el contenido del archivo por el código siguiente

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> En una aplicación real, colocaría cada clase en un archivo independiente y la cadena de conexión en el archivo `App.Config` y léala con `ConfigurationManager`. Por simplicidad, en este tutorial colocamos todos estos elementos en un solo archivo de código.

## <a name="create-your-database"></a>Creación de la base de datos

Ahora que ya tiene un modelo, puede usar las migraciones para crear una base de datos para usted.

* Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes

* Ejecute `Add-Migration MyFirstMigration` para aplicar scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.

* Ejecute `Update-Database` para aplicar la migración nueva a la base de datos. Como la base de datos todavía no existe, se creará antes de que se aplique la migración.

> [!TIP]  
> Si hace cambios en el modelo a futuro, puede usar el comando `Add-Migration` para aplicar scaffolding a una migración nueva con el fin de hacer los cambios de esquema correspondientes a la base de datos. Una vez que compruebe el código con scaffold (y haya hecho los cambios necesarios), puede usar el comando `Update-Database` para aplicar los cambios a la base de datos.
>
>EF usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.

## <a name="use-your-model"></a>Uso del modelo

Ahora puede usar el modelo para realizar el acceso a los datos.

* Abra *Program.cs*.

* Reemplace el contenido del archivo por el código siguiente

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
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

![imagen](_static/output-new-db.png)
