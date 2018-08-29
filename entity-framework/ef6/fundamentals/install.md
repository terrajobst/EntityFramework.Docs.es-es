---
title: Obtener Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 91b78e56f60edf7ebc8769b1c385f8547f63cd3d
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152393"
---
# <a name="get-entity-framework"></a>Obtener Entity Framework
Entity Framework está formado por las herramientas de EF para Visual Studio y el tiempo de ejecución de EF.

## <a name="ef-tools-for-visual-studio"></a>Herramientas EF para Visual Studio

Entity Framework Tools para Visual Studio incluyen EF Designer y el Asistente para modelo de EF y son necesarios para la base de datos en primer lugar y modelar flujos de trabajo primeros. Herramientas EF se incluyen en todas las versiones recientes de Visual Studio. Si lleva a cabo una instalación personalizada de Visual Studio, deberá asegurarse de que el elemento se selecciona "Herramientas de Entity Framework 6" eligiendo una carga de trabajo que lo incluye o seleccionando como un componente individual.

Para algunas versiones anteriores de Visual Studio, herramientas actualizadas de EF están disponibles como descarga. Consulte [versiones de Visual Studio](~/ef6/what-is-new/visual-studio.md) para obtener instrucciones sobre cómo obtener la versión más reciente de EF herramientas disponibles para su versión de Visual Studio.

## <a name="ef-runtime"></a>En tiempo de ejecución EF

La versión más reciente de Entity Framework está disponible como la [paquete EntityFramework NuGet](http://nuget.org/packages/EntityFramework/). Si no está familiarizado con el Administrador de paquetes de NuGet, le recomendamos que lea el [información general sobre NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Instalar el paquete de NuGet EF

Puede instalar el paquete de Entity Framework con el botón secundario en el **referencias** carpeta del proyecto y seleccionando **administrar paquetes NuGet...**

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Instalación desde la consola de administrador de paquetes

Como alternativa, puede instalar EntityFramework ejecutando el siguiente comando en el [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Instalar una versión concreta de EF

De EF 4.1 y versiones posteriores, se publican nuevas versiones del tiempo de ejecución EF como el [paquete NuGet de EntityFramework](https://www.nuget.org/packages/EntityFramework/). Cualquiera de estas versiones se pueden agregar a un proyecto basado en .NET Framework ejecutando el siguiente comando en Visual Studio [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

``` powershell
Install-Package EntityFramework -Version <number>
```

Tenga en cuenta que `<number>` representa la versión específica de EF para instalar. Por ejemplo, la 6.2.0 es la versión del número de EF 6.2.   

Tiempos de ejecución EF antes 4.1 formaban parte de .NET Framework y no se puede instalar por separado.

### <a name="installing-the-latest-preview"></a>Instalar la última versión preliminar

Los métodos anteriores le proporcionará la versión más reciente totalmente compatible con la versión de Entity Framework. A menudo hay versiones preliminares de disponible que nos encantaría que probar y envíenos sus comentarios acerca de Entity Framework.

Para instalar la versión preliminar más reciente de Entity Framework puede seleccionar **incluir versión preliminar** en la ventana Administrar paquetes de NuGet. Si no hay ninguna versión preliminar está disponible automáticamente obtendrá la última versión compatible totalmente de Entity Framework.

![IncludePreRelease](~/ef6/media/includeprerelease.png)

Como alternativa, puede ejecutar el comando siguiente el [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
