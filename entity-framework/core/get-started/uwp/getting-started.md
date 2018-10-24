---
title: 'Introducción a UWP: base de datos nueva - EF Core'
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 48d26adbe17e4734753a7ada547b9c13317bef0d
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315625"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Introducción a EF Core en Plataforma universal de Windows (UWP) con una base de datos nueva

En este tutorial, compilará una aplicación de Plataforma universal de Windows (UWP) que accede a datos básicos en una base de datos SQLite local mediante Entity Framework Core.

[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Requisitos previos

* [Windows 10 Fall Creators Update (10.0; compilación 16299) o versiones posteriores](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 versión 15.7 o versiones posteriores](https://www.visualstudio.com/downloads/) con la carga de trabajo **Desarrollo de la Plataforma universal de Windows**.

* [SDK de .NET Core 2.1 o versiones posteriores](https://www.microsoft.com/net/core).

> [!IMPORTANT]
> En este tutorial se usan comandos de [migraciones](xref:core/managing-schemas/migrations/index) de Entity Framework Core para crear y actualizar el esquema de la base de datos.
> Estos comandos no funcionan directamente con proyectos de UWP.
> Por este motivo, el modelo de datos de la aplicación se coloca en un proyecto de biblioteca compartida y se usa una aplicación de consola .NET Core independiente para ejecutar los comandos.

## <a name="create-a-library-project-to-hold-the-data-model"></a>Crear un proyecto de biblioteca que contenga el modelo de datos

* Apertura de Visual Studio

* **Archivo > Nuevo > Proyecto**

* En el menú de la izquierda, seleccione **Instalado > Visual C# > .NET Standard**.

* Seleccione la plantilla **Biblioteca de clases (.NET Standard)**.

* Asigne el nombre *Blogging.Model* al proyecto.

* Asigne el nombre *Blogging* a la solución.

* Haga clic en **Aceptar**.

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a>Instalar el runtime de Entity Framework Core en el proyecto de modelo de datos

Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse. En este tutorial se usa SQLite. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).

* **Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**.

* Asegúrese de que el proyecto de biblioteca *Blogging.Model* esté seleccionado como **Proyecto predeterminado** en la consola del administrador de paquetes.

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.

## <a name="create-the-data-model"></a>Crear el modelo de datos

Ahora hay que definir el elemento *DbContext* y las clases de entidad que conforman el modelo.

* Elimine *Class1.cs*.

* Cree *Model.cs* con el código siguiente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a>Crear un nuevo proyecto de consola para ejecutar comandos de migraciones

* En el **Explorador de soluciones**, haga clic con el botón derecho en la solución y elija **Agregar > Nuevo proyecto**.

* En el menú de la izquierda, seleccione **Instalado > Visual C# > .NET Core**.

* Seleccione la plantilla del proyecto **Aplicación de consola (.NET Core)**.

* Asigne el nombre *Blogging.Migrations.Startup* al proyecto y haga clic en **Aceptar**.

* Agregue una referencia al proyecto *Blogging.Model* al proyecto *Blogging.Migrations.Startup*.

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a>Instalar las herramientas de Entity Framework Core en el proyecto de inicio de migraciones

Para habilitar los comandos de migraciones de EF Core en la consola del administrador de paquetes, instale el paquete de herramientas de EF Core en la aplicación de consola.

* **Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`.

## <a name="create-the-initial-migration"></a>Creación de la migración inicial

 Cree la migración inicial al especificar la aplicación de consola como proyecto de inicio.

* Ejecute `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`.

Este comando aplica scaffolding a una migración que crea el conjunto inicial de tablas de base de datos para el modelo de datos.

## <a name="create-the-uwp-project"></a>Crear el proyecto de UWP

* En el **Explorador de soluciones**, haga clic con el botón derecho en la solución y elija **Agregar > Nuevo proyecto**.

* En el menú izquierdo, seleccione **Instalado > Visual C# > Universal de Windows**.

* Seleccione la plantilla de proyecto **Aplicación en blanco (Universal de Windows)**.

* Asigne el nombre *Blogging.UWP* al proyecto y haga clic en **Aceptar**.

> [!IMPORTANT]
> Establezca la versión de destino y la versión mínima al menos en **Windows 10 Fall Creators Update (10.0; compilación 16299.0)**.
> Las versiones anteriores de Windows 10 no admiten .NET Standard 2.0, que Entity Framework Core necesita.

## <a name="add-code-to-create-the-database-on-application-startup"></a>Agregar código para crear la base de datos al inicio de la aplicación

Puesto que quiere crear la base de datos en el dispositivo donde se ejecuta la aplicación, debe agregar código para aplicar cualquier migración pendiente a la base de datos local en el inicio de la aplicación. La primera vez que se ejecute la aplicación, esta se encargará de crear la base de datos local.

* Agregue una referencia del proyecto *Blogging.UWP* al proyecto *Blogging.Model*.

* Abra *App.xaml.cs*.

* Agregue el código resaltado para aplicar cualquier migración pendiente.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Si modifica el modelo, use el comando `Add-Migration` para aplicar el scaffolding a una migración nueva con el fin de aplicar los cambios correspondientes a la base de datos. Toda migración pendiente se aplicará a la base de datos local de cada dispositivo cuando se inicie la aplicación.
>
>EF Core usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.

## <a name="use-the-data-model"></a>Usar el modelo de datos

Ahora puede usar EF Core para realizar el acceso a los datos.

* Abra *MainPage.xaml*.

* Agregue el controlador de carga de página y el contenido de UI que aparece resaltado a continuación

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

Ahora, agregue código para conectar la interfaz de usuario con la base de datos.

* Abra *MainPage.xaml.cs*.

* Agregue el código resaltado de la lista siguiente:

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

Ahora puede ejecutar la aplicación para verla en acción.

* En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto *Blogging.UWP* y seleccione **Implementar**.

* Establezca *Blogging.UWP* como el proyecto de inicio.

* **Depurar > Iniciar sin depuración**

  La aplicación se compilará y ejecutará.

* Escriba una dirección URL y haga clic en el botón **Agregar**

  ![imagen](_static/create.png)

  ![imagen](_static/list.png)

  ¡Y listo! Ahora tiene una aplicación de UWP sencilla que ejecuta Entity Framework Core.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre la compatibilidad y el rendimiento que debe conocer al usar EF Core con UWP, vea [Implementaciones de .NET compatibles con EF Core](../../platforms/index.md#universal-windows-platform).

Consulte otros artículos de esta documentación para obtener más información sobre las características de Entity Framework Core.
