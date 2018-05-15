---
title: 'Introducción a ASP.NET Core: base de datos nueva - EF Core'
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 80477ca57b8b3df6de8ba3595c9056c6b8412040
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Introducción a EF Core en ASP.NET Core con una base de datos nueva

En este tutorial, compilará una aplicación de MVC de ASP.NET Core que realiza el acceso a datos básicos mediante Entity Framework Core. Usará las migraciones para crear la base de datos a partir del modelo de EF Core. Consulte [Recursos adicionales](#additional-resources) para más tutoriales de Entity Framework Core.

En este tutorial se necesita:
* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) con estas cargas de trabajo:
  * **Desarrollo de ASP.NET y web** (en **Web y nube**)
  * **Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)
* [SDK de .NET Core 2.0](https://www.microsoft.com/net/download/core).

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) de este artículo en GitHub.

## <a name="create-a-new-project-in-visual-studio-2017"></a>Creación de un proyecto nuevo en Visual Studio 2017

* **Archivo > Nuevo > Proyecto**
* En el menú de la izquierda, seleccione **Instalado > Plantillas > Visual C# > .NET Core**.
* Seleccione **Aplicación web de ASP.NET Core**.
* Escriba **EFGetStarted.AspNetCore.NewDb** como el nombre y haga clic en **Aceptar**.
* En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:
  * Asegúrese de que las opciones **.NET Core** y **ASP.NET Core 2.0** estén seleccionadas en las listas desplegables.
  * Seleccione la plantilla de proyecto **Aplicación web (Model-View-Controller)**.
  * Asegúrese de que la **autenticación** esté establecida en **Sin autenticación**.
  * Haga clic en **Aceptar**.

Advertencia: Si usa **Cuentas de usuario individuales** en lugar de **Ninguna** para **Autenticación**, se agregará un modelo de Entity Framework Core al proyecto en `Models\IdentityModel.cs`. Con las técnicas que aprenderá en este tutorial podrá elegir agregar un segundo modelo o extender este modelo existente para contener las clases de entidad.

## <a name="install-entity-framework-core"></a>Instalación de Entity Framework Core

Instale el paquete para los proveedores de bases de datos de EF Core a los que desea dirigirse. Este tutorial usa SQL Server. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).

* **Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Usaremos parte de Entity Framework Tools para crear una base de datos desde el modelo de EF Core. Por lo tanto, también instalaremos el paquete de herramientas:

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.

Usaremos las herramientas de scaffolding de ASP.NET Core para crear más adelante controladores y vistas. Por lo tanto, también instalaremos el paquete de diseño:

* Ejecute `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`.

## <a name="create-the-model"></a>Creación del modelo

Defina un contexto y clases de entidad que constituirán el modelo:

* Haga clic con el botón derecho en la carpeta **Modelos** y seleccione **Agregar > Clase**.
* Escriba **Model.cs** como el nombre y haga clic en **Aceptar**.
* Reemplace el contenido del archivo por el código siguiente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

Nota: En una aplicación real, habitualmente colocaría cada clase del modelo en un archivo independiente. Por simplicidad, en este tutorial colocamos todas las clases en un solo archivo.

## <a name="register-your-context-with-dependency-injection"></a>Registro del contenido con inserción de dependencias

Los servicios (como `BloggingContext`) se registran con [inserción de dependencias](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante el inicio de la aplicación. Luego se proporcionan estos servicios a los componentes que los necesitan (como los controladores MVC) a través de propiedades o parámetros de constructor.

Con el fin de que los controladores MVC usen `BloggingContext`, lo registraremos como servicio.

* Abra **Startup.cs**
* Agregue las instrucciones `using` siguientes:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Agregue el método `AddDbContext` para registrarlo como servicio:

* Agregue el código siguiente al método `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

Nota: Una aplicación real habitualmente colocaría la cadena de conexión en un archivo de configuración. Por simplicidad, se define en el código. Para más información, consulte [Cadenas de conexión](../../miscellaneous/connection-strings.md).

## <a name="create-your-database"></a>Creación de la base de datos

Una vez que ya tiene un modelo, puede usar las [migraciones](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) para crear una base de datos.

* Abra la PMC:

  **Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**
* Ejecute `Add-Migration InitialCreate` para aplicar scaffolding a una migración para crear el conjunto inicial de tablas para el modelo. Si recibe un error que indica `The term 'add-migration' is not recognized as the name of a cmdlet`, cierre Visual Studio y vuelva a abrirlo.
* Ejecute `Update-Database` para aplicar la migración nueva a la base de datos. Este comando crea la base de datos antes de aplicar las migraciones.

## <a name="create-a-controller"></a>Creación de un controlador

Habilitación de scaffolding en el proyecto:

* Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar > Controlador**.
* Seleccione **Dependencias mínimas** y haga clic en **Agregar**.
* Puede omitir o eliminar el archivo *ScaffoldingReadMe.txt*.

Ahora que se habilitó scaffolding, podemos aplicarlo a un controlador de la entidad `Blog`.

* Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar > Controlador**.
* Seleccione **Controlador de MVC con vistas que usan Entity Framework** y haga clic en **Aceptar**.
* Establezca la **clase de modelo** en **Blog** y **Clase de contexto de datos** en **BloggingContext**.
* Haga clic en **Agregar**.


## <a name="run-the-application"></a>Ejecutar la aplicación

Presione F5 para ejecutar y probar la aplicación.

* Navegue a `/Blogs`.
* Use el vínculo de creación para crear algunas entradas de blog. Pruebe los detalles y elimine los vínculos.

![imagen](_static/create.png)

![imagen](_static/index-new-db.png)

## <a name="additional-resources"></a>Recursos adicionales

* [EF: base de datos nueva con SQLite](xref:core/get-started/netcore/new-db-sqlite), un tutorial de EF para la consola multiplataforma.
* [Introducción a ASP.NET Core MVC en Mac o Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Introducción a ASP.NET Core MVC con Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Introducción a ASP.NET Core y Entity Framework Core con Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
