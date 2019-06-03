---
title: Referencia sobre las herramientas de Entity Framework Core (EF Core)
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 13e80f740bc5ce3404e8dba40b65ec872c5e3f90
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452253"
---
# <a name="entity-framework-core-tools-reference"></a>Referencia sobre las herramientas de Entity Framework Core

Las herramientas de Entity Framework Core ayudan con las tareas de desarrollo en tiempo de diseño. Se usan principalmente para administrar migraciones y para aplicar scaffolding a `DbContext` y a tipos de entidad mediante utilización de técnicas de ingeniería inversa en el esquema de una base de datos.

* Las [herramientas de la Consola del Administrador de paquetes de EF Core](powershell.md) se ejecutan en la [Consola del Administrador de paquetes](https://docs.microsoft.com/nuget/tools/package-manager-console) de Visual Studio. Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.

* Las [herramientas de la interfaz de la línea de comandos (CLI) de EF Core .NET](dotnet.md) son una extensión de las [herramientas de la CLI de .NET Core](https://docs.microsoft.com/dotnet/core/tools/) multiplataforma. Estas herramientas necesitan un proyecto de SDK de .NET Core (uno con `Sdk="Microsoft.NET.Sdk"` o similar en el archivo de proyecto).

Ambas herramientas exponen la misma funcionalidad. Si está desarrollando en Visual Studio, se recomienda usar las herramientas de la **Consola del Administrador de paquetes**, ya que proporcionan una experiencia más integrada.

## <a name="next-steps"></a>Pasos siguientes

* [Referencia sobre las herramientas de la Consola del Administrador de paquetes de EF Core](powershell.md)
* [Referencia sobre las herramientas de la CLI de EF Core .NET](dotnet.md)
