---
title: Referencia de la línea de comandos - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: d43b01fc61bb1c9b678e12e41c27d7efe9a59fa5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490368"
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

Si quiere usar una biblioteca de clases, considere la posibilidad de usar una biblioteca de clases de .NET Core o .NET Framework si es posible. De este modo se reducirán al mínimo los problemas relacionados con las herramientas de .NET. Si en su lugar quiere usar una biblioteca de clases de .NET Standard, tendrá que usar un proyecto de inicio que tenga como destino .NET Framework o .NET Core, de modo que las herramientas dispongan de una plataforma de destino concreta en la que puedan cargar la biblioteca de clases. Este proyecto de inicio puede ser un proyecto ficticio sin código real, ya que solo es necesario para proporcionar un destino para las herramientas.

Si el proyecto tiene como destino otro marco de trabajo (por ejemplo, Windows universal o Xamarin), deberá crear una biblioteca de clases de .NET Standard independiente. En este caso, siga las instrucciones anteriores para crear un proyecto de inicio que puedan usar las herramientas.

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
