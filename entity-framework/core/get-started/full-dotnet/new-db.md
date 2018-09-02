---
title: 'Introducción a .NET Framework: base de datos nueva - EF Core'
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 66d9011a5978fc3d8253a963bdb9df27848e9ff9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997592"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Introducción a EF Core en .NET Framework con una base de datos nueva

En este tutorial, compilará una aplicación de consola que realiza el acceso a datos básicos en una base de datos de Microsoft SQL Server mediante Entity Framework. Se usan migraciones para crear la base de datos a partir del modelo.

[Vea un ejemplo de este artículo en GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017 versión 15.7 o posterior](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a>Crear un proyecto nuevo

* Abra Visual Studio 2017

* **Archivo > Nuevo > Proyecto...**

* En el menú izquierdo, seleccione **Instalado > Visual C# > Escritorio de Windows**.

* Seleccione la plantilla del proyecto **Aplicación de consola (.NET Framework)**

* Asegúrese de que el proyecto tenga **.NET Framework 4.6.1** o posterior como destino.

* Asigne el nombre *ConsoleApp.NewDb* al proyecto y haga clic en **Aceptar**.

## <a name="install-entity-framework"></a>Instalación de Entity Framework

Para usar EF Core, instale el paquete correspondiente a los proveedores de bases de datos a los que desea dirigirse. Este tutorial usa SQL Server. Para una lista de los proveedores disponibles, consulte [Proveedores de bases de datos](../../providers/index.md).

* Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Más adelante en este tutorial usará parte de Entity Framework Tools para mantener la base de datos. Por lo tanto, instale el paquete de herramientas.

* Ejecute `Install-Package Microsoft.EntityFrameworkCore.Tools`.

## <a name="create-the-model"></a>Creación del modelo

Ahora hay que definir un contexto y clases de entidad que constituyan el modelo.

* **Proyecto > Agregar clase...**

* Escriba *Model.cs* como el nombre y haga clic en **Aceptar**

* Reemplace el contenido del archivo por el código siguiente

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> En una aplicación real, lo habitual sería colocar cada clase en un archivo independiente, y la cadena de conexión, en un archivo de configuración o una variable de entorno. Por simplicidad, en este tutorial todos estos elementos están en un solo archivo de código.

## <a name="create-the-database"></a>Creación de la base de datos

Ahora que ya tiene un modelo, puede usar las migraciones para crear una base de datos.

* **Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**

* Ejecute `Add-Migration InitialCreate` para aplicar scaffolding a una migración y, así, crear el conjunto inicial de tablas para el modelo.

* Ejecute `Update-Database` para aplicar la migración nueva a la base de datos. Como la base de datos todavía no existe, se creará antes de aplicar la migración.

> [!TIP]  
> Si hace cambios en el modelo, puede usar el comando `Add-Migration` para aplicar scaffolding a una migración nueva con el fin de hacer los cambios de esquema correspondientes a la base de datos. Una vez que compruebe el código con scaffold (y haya hecho los cambios necesarios), puede usar el comando `Update-Database` para aplicar los cambios a la base de datos.
>
> EF usa una tabla `__EFMigrationsHistory` en la base de datos para realizar un seguimiento de cuáles son las migraciones que ya se aplicaron a la base de datos.

## <a name="use-the-model"></a>Uso del modelo

Ahora puede usar el modelo para realizar el acceso a los datos.

* Abra *Program.cs*.

* Reemplace el contenido del archivo por el código siguiente

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* **Depurar > Iniciar sin depuración**

  Verá que un blog se guarda en la base de datos y, luego, los detalles de todos los blogs se imprimen en la consola.

  ![imagen](_static/output-new-db.png)

## <a name="additional-resources"></a>Recursos adicionales

* [EF Core en .NET Framework con una base de datos existente](xref:core/get-started/full-dotnet/existing-db)
* [EF Core en .NET Core con una base de datos nueva (SQLite)](xref:core/get-started/netcore/new-db-sqlite), un tutorial de EF para la consola multiplataforma.
