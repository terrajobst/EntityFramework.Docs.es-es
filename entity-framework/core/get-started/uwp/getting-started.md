---
title: 'Introducción a UWP: base de datos nueva - EF Core'
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996915"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Introducción a EF Core en Plataforma universal de Windows (UWP) con una base de datos nueva

En este tutorial, compilará una aplicación de Plataforma universal de Windows (UWP) que accede a datos básicos en una base de datos SQLite local mediante Entity Framework Core.

[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Requisitos previos

* [Windows 10 Fall Creators Update (10.0; compilación 16299) o versiones posteriores](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 versión 15.7 o versiones posteriores](https://www.visualstudio.com/downloads/) con la carga de trabajo **Desarrollo de la Plataforma universal de Windows**.

* [SDK de .NET Core 2.1 o versiones posteriores](https://www.microsoft.com/net/core).

## <a name="create-a-model-project"></a>Creación de un proyecto de modelo

> [!IMPORTANT]
> Debido a las limitaciones en el modo en que las herramientas de .NET Core interactúan con los proyectos de UWP, el modelo debe ubicarse en un proyecto no de UWP para poder ejecutar los comandos de migraciones en la **consola del Administrador de paquetes** (PMC).

* Apertura de Visual Studio

* **Archivo > Nuevo > Proyecto**

* En el menú de la izquierda, seleccione **Instalado > Visual C# > .NET Standard**.

* Seleccione la plantilla **Biblioteca de clases (.NET Standard)**.

* Asigne el nombre *Blogging.Model* al proyecto.

* Asigne el nombre *Blogging* a la solución.

* Haga clic en **Aceptar**.

## <a name="install-entity-framework-core"></a>Instalación de Entity Framework Core

Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse. En este tutorial se usa SQLite. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).

* **Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes**.

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.

Más adelante en este tutorial, usará parte de las herramientas de Entity Framework Core para mantener la base de datos. Por lo tanto, instale el paquete de herramientas.

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.

## <a name="create-the-model"></a>Creación del modelo

Ahora hay que definir un contexto y clases de entidad que constituyan el modelo.

* Elimine *Class1.cs*.

* Cree *Model.cs* con el código siguiente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Creación de un proyecto de UWP nuevo

* En el **Explorador de soluciones**, haga clic con el botón derecho en la solución y elija **Agregar > Nuevo proyecto**.

* En el menú izquierdo, seleccione **Instalado > Visual C# > Universal de Windows**.

* Seleccione la plantilla de proyecto **Aplicación en blanco (Universal de Windows)**.

* Asigne el nombre *Blogging.UWP* al proyecto y haga clic en **Aceptar**.

* Establezca la versión de destino y la versión mínima al menos en **Windows 10 Fall Creators Update (10.0; compilación 16299.0)**.

## <a name="create-the-initial-migration"></a>Creación de la migración inicial

Ahora que tiene un modelo, configure la aplicación para que se cree una base de datos la primera vez que se ejecute. En esta sección, creará la migración inicial. En la sección siguiente, agregue código que se aplique a esta migración cuando se inicie la aplicación.

Las herramientas de migración requieren un proyecto de inicio que no sea de UWP, por lo que primero debe crearlo.

* En el **Explorador de soluciones**, haga clic con el botón derecho en la solución y elija **Agregar > Nuevo proyecto**.

* En el menú de la izquierda, seleccione **Instalado > Visual C# > .NET Core**.

* Seleccione la plantilla del proyecto **Aplicación de consola (.NET Core)**.

* Asigne el nombre *Blogging.Migrations.Startup* al proyecto y haga clic en **Aceptar**.

* Agregue una referencia al proyecto *Blogging.Model* al proyecto *Blogging.Migrations.Startup*.

Ya puede crear su migración inicial.

* **Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**

* Seleccione el proyecto *Blogging.Model* como **proyecto predeterminado**.

* En el **Explorador de soluciones**, establezca el proyecto *Blogging.Migrations.Startup* como proyecto de inicio.

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Design`

  Este paquete es requerido por el paquete de herramientas Microsoft.EntityFrameworkCore.Tools.

* Ejecute `Add-Migration InitialCreate`.

  Este comando aplica el scaffolding a una migración que crea el conjunto inicial de tablas para el modelo.

## <a name="create-the-database-on-app-startup"></a>Creación de la base de datos al iniciar la aplicación

Puesto que quiere crear la base de datos en el dispositivo donde se ejecuta la aplicación, debe agregar código para aplicar cualquier migración pendiente a la base de datos local en el inicio de la aplicación. La primera vez que se ejecute la aplicación, esta se encargará de crear la base de datos local.

* Agregue una referencia del proyecto *Blogging.UWP* al proyecto *Blogging.Model*.

* Abra *App.xaml.cs*.

* Agregue el código resaltado para aplicar cualquier migración pendiente.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Si modifica el modelo, use el comando `Add-Migration` para aplicar el scaffolding a una migración nueva con el fin de aplicar los cambios correspondientes a la base de datos. Toda migración pendiente se aplicará a la base de datos local de cada dispositivo cuando se inicie la aplicación.
>
>EF usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.

## <a name="use-the-model"></a>Uso del modelo

Ahora puede usar el modelo para realizar el acceso a los datos.

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
