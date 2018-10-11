---
title: 'Introducción a .NET Framework: base de datos existente - EF Core'
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: b9e079f88dd35016407b19bb627f8bd46edb3d4c
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447162"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Introducción a EF Core en .NET Framework con una base de datos existente

En este tutorial, compilará una aplicación de consola que realiza el acceso a datos básicos en una base de datos de Microsoft SQL Server mediante Entity Framework. Para crear un modelo de Entity Framework, usará ingeniería inversa en una base de datos existente.

[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017 versión 15.7 o posterior](https://www.visualstudio.com/downloads/)

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

* En el menú izquierdo, seleccione **Instalado > Visual C# > Escritorio de Windows**.

* Seleccione la plantilla del proyecto **Aplicación de consola (.NET Framework)**

* Asegúrese de que el proyecto tenga **.NET Framework 4.6.1** o posterior como destino.

* Asigne el nombre *ConsoleApp.ExistingDb* al proyecto y haga clic en **Aceptar**.

## <a name="install-entity-framework"></a>Instalación de Entity Framework

Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse. Este tutorial usa SQL Server. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).

* **Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

En el paso siguiente, usará parte de Entity Framework Tools para aplicar ingeniería inversa en la base de datos. Por lo tanto, instale el paquete de herramientas.

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.

## <a name="reverse-engineer-the-model"></a>Aplicación de ingeniería inversa en el modelo

Ahora hay que crear el modelo de EF en función de una base de datos existente.

* **Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**

* Ejecute el comando siguiente para crear un modelo a partir de la base de datos existente

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> Para especificar las tablas para generar entidades, agregue el argumento `-Tables` al comando anterior. Por ejemplo: `-Tables Blog,Post`.

El proceso de ingeniería inversa creó clases de entidad (`Blog` y `Post`) y un contexto derivado (`BloggingContext`) en función del esquema de la base de datos existente.

Las clases de entidad son simples objetos de C# que representan los datos que va a consultar y guardar. Estas son las clases de entidad `Blog` y `Post`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> Para habilitar la carga diferida, puede usar las propiedades de navegación `virtual` (Blog.Post y Post.Blog).

El contexto representa una sesión con la base de datos. Tiene métodos que puede usar para consultar y guardar instancias de las clases de entidad.

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a>Uso del modelo

Ahora puede usar el modelo para realizar el acceso a los datos.

* Abra *Program.cs*.

* Reemplace el contenido del archivo por el código siguiente

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* Depurar > Iniciar sin depuración

  Verá que un blog se guarda en la base de datos y, luego, los detalles de todos los blogs se imprimen en la consola.

  ![imagen](_static/output-existing-db.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo aplicar scaffolding a un contexto y a clases de entidad, vea los siguientes artículos:
* [Referencia sobre las herramientas de Entity Framework Core (CLI de .NET)](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Referencia sobre las herramientas de Entity Framework Core (Consola del Administrador de paquetes)](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)