---
title: Obtener Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416100"
---
# <a name="get-entity-framework"></a>Obtener Entity Framework
Entity Framework está formado por EF Tools para Visual Studio y EF runtime.

## <a name="ef-tools-for-visual-studio"></a>EF Tools para Visual Studio

Entity Framework Tools para Visual Studio incluyen EF Designer y el EF Model Wizard y son necesarios para los flujos de trabajo _database first_ y _model first_. EF Tools es includo en todas las versiones recientes de Visual Studio. Si lleva a cabo una instalación personalizada de Visual Studio, deberá asegurarse el elemento "Entity Frameworks 6 Tools" está seleccionado eligiendo un _workload_ que lo incluya o seleccionandolo como un componente individual.

Para algunas versiones anteriores de Visual Studio, EF Tools actualizadas están disponibles como descarga. Consulte [versiones de Visual Studio](~/ef6/what-is-new/visual-studio.md) para obtener instrucciones sobre cómo obtener la versión más reciente de las herramientas de EF disponibles para su versión de Visual Studio.

## <a name="ef-runtime"></a>EF runtime

La versión más reciente de Entity Framework está disponible como el [paquete NuGet EntityFramework](https://nuget.org/packages/EntityFramework/). Si no está familiarizado con el administrador de paquetes NuGet, le recomendamos que lea la [información general de Nuget](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Instalar EF NuGet package

Para instalar el paquete EntityFramework, haga clic con el botón derecho en la carpeta **referencias** del proyecto y seleccione **administrar paquetes NuGet..** .

![Administración de paquetes de NuGet](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Instalación desde Package Manager Console

Como alternativa, puede instalar EntityFramework ejecutando el siguiente comando en la consola del [Administrador de paquetes](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Instalación de una versión específica de EF

Desde EF 4,1 en adelante, se han publicado nuevas versiones del tiempo de ejecución de EF como el [paquete NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/). Cualquiera de esas versiones se puede Agregar a un proyecto basado en .NET Framework ejecutando el siguiente comando en la [consola del administrador de paquetes](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)de Visual Studio:

``` powershell
Install-Package EntityFramework -Version <number>
```

Tenga en cuenta que `<number>` representa la versión específica de EF que se va a instalar. Por ejemplo, 6.2.0 es la versión del número para EF 6,2.   

Los EF runtimes anteriores a 4.1 formaban parte de .NET Framework y no se puede instalar por separado.

### <a name="installing-the-latest-preview"></a>Instalación de la versión preliminar más reciente

Los métodos anteriores le proporcionarán la versión más reciente totalmente compatible con la versión de Entity Framework. A menudo hay versiones preliminares disponibles de Entity Framework que nos encantaría que probara y de las que puede enviarnos sus comentarios.

Para instalar la versión preliminar más reciente de EntityFramework, puede seleccionar **incluir versión** preliminar en la ventana administrar paquetes NuGet. Si no hay ninguna versión preliminar disponible automáticamente obtendrá la última versión compatible de Entity Framework.

![Incluir versión preliminar](~/ef6/media/includeprerelease.png)

Como alternativa, puede ejecutar el siguiente comando en la [consola del administrador de paquetes](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
