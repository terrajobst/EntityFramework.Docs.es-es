---
title: Instalación de EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 7bb2ee11940a4fd5736c7a23c16533ef53018f7b
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949197"
---
# <a name="installing-ef-core"></a>Instalación de EF Core

## <a name="prerequisites"></a>Requisitos previos

Para desarrollar aplicaciones de .NET Core 2.0 (incluidas aplicaciones de ASP.NET Core 2.0 para .NET Core) debe descargar e instalar una versión del [SDK de .NET Core 2.0](https://www.microsoft.com/net/download/core) que sea apropiada para la plataforma. **Debe hacerlo aunque haya instalado Visual Studio 2017 versión 15.3.**

Para usar EF Core 2.0 o cualquier otra biblioteca de .NET Standard 2.0 con una plataforma .NET además de .NET Core 2.0 (por ejemplo, con .NET Framework 4.6.1 o superior), necesita una versión de NuGet compatible con .NET Standard 2.0 y sus marcos de trabajo compatibles. Estas son algunas formas de conseguirlo:

* Instalar Visual Studio 2017 versión 15.3
* Si usa Visual Studio 2015, [descargue y actualice el cliente de NuGet a la versión 3.6.0](https://www.nuget.org/downloads)

Los proyectos creados con versiones anteriores de Visual Studio para .NET Framework pueden necesitar modificaciones adicionales para ser compatibles con las bibliotecas de .NET Standard 2.0:

* Edite el archivo de proyecto y asegúrese de que la siguiente entrada aparece en el grupo de propiedades inicial:
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* En los proyectos de prueba, asegúrese también de que la entrada siguiente está presente:
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>Recopilación de las partes
La manera recomendada de agregar las bibliotecas en tiempo de ejecución de EF Core a una aplicación es instalar un proveedor de base de datos de EF Core desde NuGet.

Además de las bibliotecas en tiempo de ejecución, puede instalar herramientas que faciliten la ejecución de varias tareas relacionadas con EF Core en el proyecto en tiempo de diseño, como crear y aplicar migraciones y crear un modelo basado en una base de datos existente.

> [!TIP]  
> Si necesita actualizar una aplicación que usa un proveedor de base de datos de terceros, busque siempre una actualización del proveedor que sea compatible con la versión de EF Core que quiere usar. Por ejemplo, los proveedores de bases de datos de las versiones anteriores no son compatibles con la versión 2.0 del runtime de EF Core.  

> [!TIP]  
> Las aplicaciones para ASP.NET Core 2.0 pueden usar EF Core 2.0 sin dependencias adicionales además de proveedores de bases de datos de terceros. Las aplicaciones destinadas a versiones anteriores de ASP.NET Core deben actualizarse a ASP.NET Core 2.0 para usar EF Core 2.0.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>Desarrollo multiplataforma con la interfaz de la línea de comandos (CLI) de .NET Core

Para desarrollar aplicaciones para [.NET Core](https://www.microsoft.com/net/download/core), puede usar los [comandos de CLI `dotnet`](https://docs.microsoft.com/dotnet/core/tools/) en combinación con el editor de texto que prefiera o un entorno de desarrollo integrado (IDE) como Visual Studio, Visual Studio para Mac o Visual Studio Code.

> [!IMPORTANT]  
> Las aplicaciones destinadas a .NET Core requieren versiones específicas de Visual Studio. Por ejemplo, el desarrollo de .NET Core 1.x requiere Visual Studio 2017, mientras que el desarrollo de .NET Core 2.0 requiere Visual Studio 2017, versión 15.3.

Para instalar o actualizar el proveedor de SQL Server en una aplicación de .NET Core multiplataforma, vaya al directorio de la aplicación y ejecute lo siguiente en una línea de comandos:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Puede indicar una instalación de una versión específica en el comando `dotnet add package`, con el modificador `-v`. Por ejemplo, para instalar paquetes de EF Core 2.0, anexe `-v 2.0.0` al comando.

EF Core incluye un conjunto de [comandos adicionales para la CLI `dotnet`](../../miscellaneous/cli/dotnet.md) que empiezan por `dotnet ef`. Para usar los comandos de CLI `dotnet ef`, el archivo `.csproj` de la aplicación debe contener la entrada siguiente:

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

Las herramientas de CLI de .NET Core para EF Core también requieren un paquete independiente denominado Microsoft.EntityFrameworkCore.Design. Puede agregarlo simplemente al proyecto mediante:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> Use siempre versiones de los paquetes de herramientas que coincidan con la versión principal de los paquetes en tiempo de ejecución.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Desarrollo de Visual Studio

Puede desarrollar muchos tipos diferentes de aplicaciones para .NET Core, .NET Framework u otras plataformas compatibles con EF Core mediante Visual Studio.

Hay dos maneras de instalar un proveedor de base de datos de EF Core en la aplicación desde Visual Studio:

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>Con la [interfaz de usuario del administrador de paquetes](https://docs.microsoft.com/nuget/tools/package-manager-ui) de NuGet

* Seleccione en el menú **Proyecto > Administrar paquetes NuGet**

* Haga clic en la pestaña **Examinar** o **Actualizaciones**

* Seleccione el paquete `Microsoft.EntityFrameworkCore.SqlServer` y la versión deseada y confirme

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>Con la [Consola del administrador de paquetes (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console) de NuGet

* Seleccione en el menú **Herramientas > Administrador de paquetes NuGet > Consola del administrador de paquetes**

* Escriba y ejecute el comando siguiente en la PMC:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Puede usar el comando `Update-Package` en su lugar para actualizar un paquete que ya está instalado a una versión más reciente

* Para especificar una versión específica, puede usar el modificador `-Version`. Por ejemplo, para instalar paquetes de EF Core 2.0, agregue `-Version 2.0.0` a los comandos

#### <a name="tools"></a>Herramientas

También hay una versión de PowerShell de los [comandos de EF Core que se ejecutan dentro de la PMC](../../miscellaneous/cli/powershell.md) en Visual Studio, con capacidades similares a las de los comandos `dotnet ef`. Para usarlos, instale el paquete `Microsoft.EntityFrameworkCore.Tools` con la UI del administrador de paquetes o la PMC.

> [!IMPORTANT]  
> Use siempre versiones de los paquetes de herramientas que coincidan con la versión principal de los paquetes en tiempo de ejecución.

> [!TIP]  
> Aunque es posible usar los comandos `dotnet ef` desde la PMC en Visual Studio, es mucho más conveniente usar la versión de PowerShell:
> * Trabajan automáticamente con el proyecto actual seleccionado en la PMC sin necesidad de cambiar manualmente entre directorios.  
> * Abren automáticamente los archivos generados por los comandos de Visual Studio una vez completado el comando.

> [!IMPORTANT]  
> **Paquetes en desuso de EF Core 2.0:** si va a actualizar una aplicación existente a EF Core 2.0, es posible que algunas referencias a los paquetes más antiguos de EF Core tengan que quitarse manualmente. En concreto, los paquetes en tiempo de diseño del proveedor de base de datos, como `Microsoft.EntityFrameworkCore.SqlServer.Design`, ya no son necesarios ni se admiten en EF Core 2.0, pero no se quitan automáticamente al actualizar los demás paquetes.
