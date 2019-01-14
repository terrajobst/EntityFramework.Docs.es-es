---
title: 'Introducción a ASP.NET Core: base de datos nueva - EF Core'
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 4734586adc89e9c1d866a1b4accd8b5e51fe2bb0
ms.sourcegitcommit: ebf661025d2ad2b62466fa7bf0e0772a7811cbe7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2019
ms.locfileid: "54211171"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Introducción a EF Core en ASP.NET Core con una base de datos nueva

En este tutorial compilará una aplicación de MVC de ASP.NET Core que realiza el acceso a datos básicos mediante Entity Framework Core. El tutorial usa migraciones para crear la base de datos a partir del modelo de datos.

Puede seguir el tutorial con Visual Studio 2017 en Windows o mediante la CLI de .NET Core en Windows, macOS o Linux.

Vea el ejemplo de este artículo en GitHub:
* [Visual Studio 2017 con SQL Server](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* [CLI de .NET Core con SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).

## <a name="prerequisites"></a>Requisitos previos

Instale el software siguiente:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 versión 15.7 o posterior](https://www.visualstudio.com/downloads/) con estas cargas de trabajo:
  * **Desarrollo de ASP.NET y web** (en **Web y nube**)
  * **Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core).

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core).

---

## <a name="create-a-new-project"></a>Crear un proyecto nuevo

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Abra Visual Studio 2017
* **Archivo > Nuevo > Proyecto**
* En el menú izquierdo, seleccione **Instalado > Visual C# > .NET Core**.
* Seleccione **Aplicación web de ASP.NET Core**.
* Escriba **EFGetStarted.AspNetCore.NewDb** como el nombre y haga clic en **Aceptar**.
* En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:
  * Asegúrese de que **.NET Core** y **ASP.NET Core 2.1** estén seleccionados en las listas desplegables
  * Seleccione la plantilla de proyecto **Aplicación web (Model-View-Controller)**.
  * Asegúrese de que **Autenticación** esté establecido en **Sin autenticación**
  * Haga clic en **Aceptar**.

Advertencia: Si usa **Cuentas de usuario individuales** en lugar de **Ninguno** para **Autenticación**, se agrega un modelo de Entity Framework Core al proyecto en `Models\IdentityModel.cs`. Con las técnicas que aprenderá en este tutorial podrá elegir agregar un segundo modelo o extender este modelo existente para contener las clases de entidad.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

* Ejecute el siguiente comando para crear un proyecto de MVC:

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* Cambie al directorio del proyecto. Los siguientes comandos que escriba deben ejecutarse en el nuevo proyecto.

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a>Instalación de Entity Framework Core

Para instalar EF Core, instale el paquete de los proveedores de bases de datos de EF Core que quiera establecer como destino. Para obtener una lista de proveedores disponibles, vea [Proveedores de bases de datos](../../providers/index.md).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Para este tutorial no tendrá que instalar ningún paquete de proveedor porque se usa SQL Server. El paquete de proveedor de SQL Server está incluido en el metapaquete [Microsoft.AspNetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Este tutorial usa SQLite porque se ejecuta en todas las plataformas compatibles con .NET Core.

* Ejecute el siguiente comando para instalar el proveedor de SQLite:

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a>Creación del modelo

Defina una clase de contexto y clases de entidad que conformen el modelo.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Haga clic con el botón derecho en la carpeta **Modelos** y seleccione **Agregar > Clase**.
* Escriba **Model.cs** como el nombre y haga clic en **Aceptar**.
* Reemplace el contenido del archivo por el código siguiente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

* En la carpeta **Modelos**, cree **Model.cs** con el código siguiente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

Una aplicación de producción normalmente colocaría cada clase en un archivo independiente. Por simplicidad, este tutorial coloca estas clases en un archivo.

## <a name="register-the-context-with-dependency-injection"></a>Registro del contexto con inserción de dependencias

Los servicios (como `BloggingContext`) se registran con [inserción de dependencias](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante el inicio de la aplicación. A los componentes que requieren estos servicios (por ejemplo, los controladores MVC), se les proporcionan a través de parámetros o propiedades de constructor.

Para que `BloggingContext` esté disponible para los controladores MVC, debe registrarlo como servicio.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En **Startup.cs**, agregue las siguientes instrucciones `using`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* Agregue el siguiente código resaltado al método `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

* En **Startup.cs**, agregue las siguientes instrucciones `using`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* Agregue el siguiente código resaltado al método `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

Una aplicación de producción normalmente colocaría la cadena de conexión en una variable de entorno o un archivo de configuración. Por simplicidad, en este tutorial se define en código. Para más información, consulte [Cadenas de conexión](../../miscellaneous/connection-strings.md).

## <a name="create-the-database"></a>Creación de la base de datos

Los pasos siguientes usan [migraciones](xref:core/managing-schemas/migrations/index) para crear una base de datos.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**
* Ejecute los comandos siguientes:

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  Si se produce un error que indica `The term 'add-migration' is not recognized as the name of a cmdlet`, cierre y vuelva a abrir Visual Studio.

  El comando `Add-Migration` aplica la técnica scaffolding a una migración para crear el conjunto inicial de tablas para el modelo. El comando `Update-Database` crea la base de datos y le aplica la nueva migración.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

* Ejecute los comandos siguientes:

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  El comando `migrations` aplica la técnica scaffolding a una migración para crear el conjunto inicial de tablas para el modelo. El comando `database update` crea la base de datos y le aplica la nueva migración.

---

## <a name="create-a-controller"></a>Creación de un controlador

Aplique scaffolding a un controlador y a las vistas de la entidad `Blog`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Haga clic con el botón derecho en la carpeta **Controladores** del **Explorador de soluciones** y seleccione **Agregar > Controlador**.
* Seleccione **Controlador de MVC con vistas que usan Entity Framework** y haga clic en **Agregar**.
* Establezca la **clase de modelo** en **Blog** y **Clase de contexto de datos** en **BloggingContext**.
* Haga clic en **Agregar**.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

* Ejecute los comandos siguientes:

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  Los comandos `tool install` y `add package` instalan las herramientas que pueden aplicar la técnica scaffolding a controladores y vistas. El comando `restore` garantiza que se descarguen todos los paquetes del proyecto, mientras que el comando `aspnet-codegenerator` aplica la técnica scaffolding.
---

El motor de scaffolding crea los siguientes archivos:

* Un controlador (*Controllers/BlogsController.cs*)
* Vistas de Razor para las páginas Create, Delete, Details, Edit e Index (_Views/Blogs/*.cshtml_)

## <a name="run-the-application"></a>Ejecutar la aplicación

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Depurar** > **Iniciar sin depurar**

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```cli
dotnet run
```
---

* Navegue a `/Blogs`.

* Use el vínculo **Crear nuevo** para crear algunas entradas de blog.

  ![Página Crear](_static/create.png)

* Pruebe los vínculos **Detalles**, **Editar** y **Eliminar**.

  ![Página de índice](_static/index-new-db.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Tutorial: Introducción a EF Core en la aplicación de consola de .NET Core con una base de datos nueva](xref:core/get-started/netcore/new-db-sqlite)
* [Tutorial: Introducción a Razor Pages en ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Tutorial: Razor Pages con Entity Framework Core en ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
