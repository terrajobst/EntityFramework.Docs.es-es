---
title: 'Introducción a UWP: base de datos nueva - EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2017
ms.locfileid: "26049838"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Introducción a EF Core en Plataforma universal de Windows (UWP) con una base de datos nueva

> [!NOTE]
> En este tutorial se usa EF Core 2.0.1 (que se publicó junto con ASP.NET Core y el SDK 2.0.3 de .NET Core). EF Core 2.0.0 carece de algunas correcciones de errores fundamentales necesarias para una buena experiencia con UWP.

En este tutorial, compilará una aplicación de Plataforma universal de Windows que realiza el acceso a datos básicos en una base de datos SQLite local mediante Entity Framework.

> [!IMPORTANT]
> **Considere la posibilidad de evitar los tipos anónimos en las consultas LINQ en UWP**. Para implementar una aplicación de UWP en la tienda de aplicaciones, la aplicación debe estar compilada con .NET Native. Las consultas con tipos anónimos tienen un menor rendimiento en .NET Native.

> [!TIP]
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) de este artículo en GitHub.

## <a name="prerequisites"></a>Requisitos previos

Necesitará estos elementos para llevar a cabo este tutorial:

* [Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)

* [SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) versión 15.4 o posterior con la carga de trabajo **Desarrollo de la Plataforma universal de Windows**.

## <a name="create-a-new-model-project"></a>Creación de un proyecto de modelo nuevo

> [!WARNING]
> Debido a las limitaciones en el modo en que las herramientas de .NET Core interactúan con los proyectos de UWP, el modelo debe ubicarse en un proyecto no de UWP para poder ejecutar los comandos de migraciones en la consola del Administrador de paquetes

* Apertura de Visual Studio

* Archivo > Nuevo> Proyecto...

* En el menú de la izquierda, seleccione Plantillas > Visual C#

* Seleccione la plantilla del proyecto **Biblioteca de clases (.NET Standard)**

* Asigne un nombre al proyecto y haga clic en **Aceptar**

## <a name="install-entity-framework"></a>Instalación de Entity Framework

Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse. Este tutorial usa SQLite. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).

* Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.

Más adelante en este tutorial también usaremos parte de Entity Framework Tools para mantener la base de datos. Por lo tanto, también instalaremos el paquete de herramientas.

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.

* Edite el archivo .csproj y reemplace `<TargetFramework>netstandard2.0</TargetFramework>` por `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`

## <a name="create-your-model"></a>Creación del modelo

Es momento de definir un contexto y clases de entidad que constituyan el modelo.

* Proyecto > Agregar clase...

* Escriba *Model.cs* como el nombre y haga clic en **Aceptar**

* Reemplace el contenido del archivo por el código siguiente

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Creación de un proyecto de UWP nuevo

* Apertura de Visual Studio

* Archivo > Nuevo> Proyecto...

* En el menú de la izquierda, seleccione Plantillas > Visual C# > Windows Universal

* Seleccione la plantilla de proyecto **Aplicación en blanco (Universal Windows)**

* Asigne un nombre al proyecto y haga clic en **Aceptar**

* Establezca la versión de destino y la versión mínima al menos en `Windows 10 Fall Creators Update (10.0; build 16299.0)`

## <a name="create-your-database"></a>Creación de la base de datos

Ahora que ya tiene un modelo, puede usar las migraciones para crear una base de datos para usted.

* Herramientas –> Administrador de paquetes NuGet –> Consola del Administrador de paquetes

* Seleccione el proyecto de modelo como el proyecto predeterminado y establézcalo como el proyecto de inicio

* Ejecute `Add-Migration MyFirstMigration` para aplicar scaffolding a una migración para crear el conjunto inicial de tablas para el modelo.

Como deseamos crear la base de datos en el dispositivo donde se ejecuta la aplicación, agregaremos código para aplicar cualquier migración pendiente a la base de datos local en el inicio de la aplicación. La primera vez que se ejecute la aplicación, se encargará de crear la base de datos local.

* Haga clic con el botón derecho en **App.xaml** en el **Explorador de soluciones** y seleccione **Ver código**.

* Agregue el texto resaltado en el inicio del archivo.

* Agregue el código resaltado para aplicar cualquier migración pendiente.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> Si hace cambios en el modelo en el futuro, puede usar el comando `Add-Migration` para aplicar scaffolding a una migración nueva con el fin de aplicar los cambios correspondientes a la base de datos. Toda migración pendiente se aplicará a la base de datos local de cada dispositivo cuando se inicie la aplicación.
>
>EF usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.

## <a name="use-your-model"></a>Uso del modelo

Ahora puede usar el modelo para realizar el acceso a los datos.

* Abra *MainPage.xaml*

* Agregue el controlador de carga de página y el contenido de UI que aparece resaltado a continuación

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

Ahora agregaremos código para conectar la interfaz de usuario con la base de datos

* Haga clic con el botón derecho en **MainPage.xaml** en el **Explorador de soluciones** y seleccione **Ver código**.

* Agregue el código resaltado de la lista siguiente.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

Ahora puede ejecutar la aplicación para verla en acción.

* Depurar > Iniciar sin depuración

* La aplicación se compilará e iniciará

* Escriba una dirección URL y haga clic en el botón **Agregar**

![imagen](_static/create.png)

![imagen](_static/list.png)

## <a name="next-steps"></a>Pasos siguientes

> [!TIP]
> Es posible mejorar el rendimiento de `SaveChanges()` mediante la implementación de [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) en los tipos de entidad y mediante el uso de `ChangeTrackingStrategy.ChangingAndChangedNotifications`.

¡Y listo! Ahora tiene una aplicación de UWP sencilla que ejecuta Entity Framework.

Consulte otros artículos de esta documentación para más información sobre las características de Entity Framework.
