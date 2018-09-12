---
title: Instalación de Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250327"
---
# <a name="installing-entity-framework-core"></a>Instalación de Entity Framework Core

## <a name="prerequisites"></a>Requisitos previos

* Para desarrollar aplicaciones que tengan como destino .NET Core 2.1, instale [el SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core). El SDK debe estar instalado incluso si dispone de la versión más reciente de Visual Studio 2017.

* Para poder usar Visual Studio para el desarrollo de aplicaciones que tengan como destino .NET Core 2.1, instale Visual Studio 2017 15.7 o versiones posteriores.

* Para usar Entity Framework 2.1 en aplicaciones ASP.NET Core, use ASP.NET Core 2.1. Las aplicaciones que usen versiones anteriores de ASP.NET Core deberán actualizarse a la versión 2.1.

* Puede usar Visual Studio 2015 para aplicaciones que tengan como destino .NET Framework 4.6.1 o versiones posteriores. Para ello, necesitará una versión de NuGet que reconozca .NET Standard 2.0 y sus marcos compatibles. Para obtenerla en Visual Studio 2015, [actualice el cliente de NuGet a la versión 3.6.0](https://www.nuget.org/downloads).

## <a name="get-the-entity-framework-core-runtime"></a>Obtención del runtime de Entity Framework Core

Para agregar las bibliotecas del runtime de EF Core a una aplicación, instale el paquete NuGet para el proveedor de bases de datos que quiera usar. Para obtener una lista de los proveedores admitidos y los nombres de sus paquetes NuGet, vea [Proveedores de la base de datos](../../providers/index.md).

Para instalar o actualizar paquetes NuGet, use la CLI de .NET Core, o bien el cuadro de diálogo o la consola del Administrador de paquetes de Visual Studio.

En el caso de las aplicaciones ASP.NET Core 2.1, los proveedores de SQL Server y en memoria se incluyen automáticamente, por lo que no es necesario instalarlos por separado.

> [!TIP]  
> Si necesita actualizar una aplicación que usa un proveedor de base de datos de terceros, busque siempre una actualización del proveedor que sea compatible con la versión de EF Core que quiere usar. Por ejemplo, los proveedores de bases de datos de las versiones anteriores no son compatibles con la versión 2.1 del runtime de EF Core.  

### <a name="net-core-cli"></a>CLI de .NET Core

El siguiente comando de la CLI de .NET Core instala o actualiza el proveedor de SQL Server:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Puede indicar una versión específica en el comando `dotnet add package` usando el modificador `-v`. Por ejemplo, para instalar paquetes de EF Core 2.1.0, anexe `-v 2.1.0` al comando.

### <a name="visual-studio-nuget-package-manager-dialog"></a>Cuadro de diálogo Administrador de paquetes NuGet en Visual Studio

* En el menú, seleccione **Proyecto > Administrar paquetes NuGet**

* Haga clic en la pestaña **Examinar** o **Actualizaciones**

* Para instalar o actualizar el proveedor de SQL Server, seleccione el paquete `Microsoft.EntityFrameworkCore.SqlServer` y confirme la acción.

Para obtener más información, vea [Diálogo del Administrador de paquetes NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Consola del Administrador de paquetes NuGet de Visual Studio

* En el menú, seleccione **Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**.

* Para instalar el proveedor de SQL Server, ejecute el comando siguiente en la consola del Administrador de paquetes:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Para actualizar el proveedor, use el comando `Update-Package`.

* Para especificar una versión, use el modificador `-Version`. Por ejemplo, para instalar paquetes de EF Core 2.1.0, anexe `-Version 2.1.0` a los comandos.

Para obtener más información, vea [Consola del Administrador de paquetes](https://docs.microsoft.com/nuget/tools/package-manager-console).

## <a name="get-entity-framework-core-tools"></a>Obtención de herramientas de Entity Framework Core

Además de las bibliotecas del runtime, puede instalar herramientas que son capaces de realizar algunas tareas relacionadas con EF Core en el proyecto en tiempo de diseño. Por ejemplo, puede crear y aplicar migraciones, así como crear un modelo según una base de datos existente.

Existen dos conjuntos de herramientas:
* Las herramientas de [interfaz de la línea de comandos (CLI)](../../miscellaneous/cli/dotnet.md) de .NET Core pueden usarse en Windows, Linux y macOS. Estos comandos comienzan por `dotnet ef`. 
* Las [herramientas de la consola del Administrador de paquetes](../../miscellaneous/cli/powershell.md) se ejecutan en Visual Studio 2017 (Windows). Estos comandos empiezan por un verbo. Por ejemplo: `Add-Migration`, `Update-Database`.

Aunque puede usar los comandos de `dotnet ef` desde la consola del Administrador de paquetes, usar las herramientas de la consola del Administrador de paquetes en Visual Studio resulta más práctico:
* Estos comandos trabajan automáticamente con el proyecto actualmente seleccionado en la consola del Administrador de paquetes, sin necesidad de cambiar manualmente entre directorios.  
* Abren automáticamente los archivos generados por los comandos de Visual Studio una vez completado el comando.

<a name="cli"></a>

### <a name="get-the-cli-tools"></a>Obtención de las herramientas de la CLI

Los comandos de `dotnet ef` están incluidos en el SDK de .NET Core. Pero es necesario instalar el paquete `Microsoft.EntityFrameworkCore.Design` para habilitarlos:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Para las aplicaciones de ASP.NET Core 2.1, este paquete se incluye automáticamente.

Tal como se explicó anteriormente en [Requisitos previos](#prerequisites), también deberá instalar el SDK de .NET Core 2.1.

> [!IMPORTANT]      
> Use siempre la versión del paquete de herramientas que coincida con la versión principal de los paquetes en tiempo de ejecución.

### <a name="get-the-package-manager-console-tools"></a>Obtención de las herramientas de la consola del Administrador de paquetes

Para obtener las herramientas de la consola del Administrador de paquetes para EF Core, instale el paquete `Microsoft.EntityFrameworkCore.Tools`:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

Para las aplicaciones de ASP.NET Core 2.1, este paquete se incluye automáticamente.

## <a name="upgrading-to-ef-core-21"></a>Actualización a EF Core 2.1

Si va a actualizar una aplicación existente a EF Core 2.1, es posible que algunas referencias a los paquetes más antiguos de EF Core tengan que quitarse manualmente:

* Los paquetes en tiempo de diseño del proveedor de base de datos, como `Microsoft.EntityFrameworkCore.SqlServer.Design`, ya no son necesarios ni se admiten en EF Core 2.1, pero no se quitan automáticamente al actualizar los demás paquetes.

* Las herramientas de la CLI de .NET ahora están incluidas en el SDK de .NET, por lo que se puede quitar la referencia a ese paquete desde el archivo *.csproj*:

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

En el caso de las aplicaciones que tengan como destino .NET Framework y se hayan creado con versiones anteriores de Visual Studio, asegúrese de que sean compatibles con las bibliotecas de .NET Standard 2.0:

  * Edite el archivo de proyecto y asegúrese de que la siguiente entrada aparece en el grupo de propiedades inicial:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * En los proyectos de prueba, asegúrese también de que la entrada siguiente está presente:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
