---
title: "Referencia de la línea de comandos - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a>Herramientas de Entity Framework Core
===========================
Las herramientas de Entity Framework Core ayudan durante el desarrollo de aplicaciones de EF Core. Se usan principalmente para aplicar scaffolding a DbContext y a tipos de entidad mediante la aplicación de ingeniería inversa al esquema de una base de datos y para administrar migraciones.

Las [herramientas de la Consola del administrador de paquetes (PMC) de EF Core][1] proporcionan una experiencia de nivel superior dentro de Visual Studio. Ejecútelas con la [Consola del administrador de paquetes][2] de NuGet. Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.

Las [herramientas de línea de comandos de .NET de EF Core][3] son una extensión de las [herramientas de la interfaz de la línea de comandos (CLI) de .NET Core][4], que son multiplataforma y pueden ejecutarse fuera de Visual Studio. Estas herramientas necesitan un proyecto de SDK de .NET Core (uno con `Sdk="Microsoft.NET.Sdk"` o similar en el archivo de proyecto).

Ambas herramientas exponen la misma funcionalidad. Si está desarrollando en Visual Studio, se recomienda usar las herramientas de la PMC, ya que proporcionan una experiencia más integrada.

<a name="frameworks"></a>Marcos de trabajo
----------
Las herramientas admiten proyectos para .NET Framework o .NET Core.

Si el proyecto tiene como destino otro marco de trabajo (por ejemplo, Universal Windows o Xamarin), se recomienda crear un proyecto independiente de .NET Standard y hacer que uno de los marcos admitidos sea multiplataforma.

Para hacer a .NET Core multiplataforma, por ejemplo, haga clic con el botón derecho en el proyecto y seleccione **Editar \*.csproj**. Actualice la propiedad `TargetFramework` como se indica a continuación. (Tenga en cuenta que el nombre de la propiedad se convierte en plural).

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

Si usa una biblioteca de clases .NET Standard, no es necesario convertir en multiplataforma si el proyecto de inicio tiene como destino .NET Framework o .NET Core.

<a name="startup-and-target-projects"></a>Proyectos de inicio y destino
---------------------------
Cada vez que se invoca un comando, intervienen dos proyectos: el proyecto de destino y el proyecto de inicio.

El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).

El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.

El proyecto de destino y el proyecto de inicio pueden ser el mismo.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
