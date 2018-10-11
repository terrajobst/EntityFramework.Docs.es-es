---
title: Referencia sobre las herramientas de Entity Framework Core (EF Core)
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 9fcb452c2798a3d07e39cbcc3c34629dca4394ff
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447123"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="cafae-102">Referencia sobre las herramientas de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="cafae-102">Entity Framework Core tools reference</span></span>

<span data-ttu-id="cafae-103">Las herramientas de Entity Framework Core ayudan con las tareas de desarrollo en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="cafae-103">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="cafae-104">Se usan principalmente para administrar migraciones y para aplicar scaffolding a `DbContext` y a tipos de entidad mediante utilización de técnicas de ingeniería inversa en el esquema de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="cafae-104">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

* <span data-ttu-id="cafae-105">Las [herramientas de la Consola del Administrador de paquetes de EF Core](powershell.md) se ejecutan en la [Consola del Administrador de paquetes](https://docs.microsoft.com/nuget/tools/package-manager-console) de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cafae-105">The [EF Core Package Manager Console tools](powershell.md)) run in the [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio.</span></span> <span data-ttu-id="cafae-106">Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cafae-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

* <span data-ttu-id="cafae-107">Las [herramientas de la interfaz de la línea de comandos (CLI) de EF Core .NET](dotnet.md) son una extensión de las [herramientas de la CLI de .NET Core](https://docs.microsoft.com/dotnet/core/tools/) multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="cafae-107">The [EF Core .NET command-line interface (CLI) tools](dotnet.md) are an extension to the cross-platform [.NET Core CLI tools](https://docs.microsoft.com/dotnet/core/tools/).</span></span> <span data-ttu-id="cafae-108">Estas herramientas necesitan un proyecto de SDK de .NET Core (uno con `Sdk="Microsoft.NET.Sdk"` o similar en el archivo de proyecto).</span><span class="sxs-lookup"><span data-stu-id="cafae-108">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="cafae-109">Ambas herramientas exponen la misma funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="cafae-109">Both tools expose the same functionality.</span></span> <span data-ttu-id="cafae-110">Si está desarrollando en Visual Studio, se recomienda usar las herramientas de la **Consola del Administrador de paquetes**, ya que proporcionan una experiencia más integrada.</span><span class="sxs-lookup"><span data-stu-id="cafae-110">If you're developing in Visual Studio, we recommend using the **Package Manager Console** tools since they provide a more integrated experience.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cafae-111">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="cafae-111">Next steps</span></span>

* [<span data-ttu-id="cafae-112">Referencia sobre las herramientas de la Consola del Administrador de paquetes de EF Core</span><span class="sxs-lookup"><span data-stu-id="cafae-112">EF Core Package Manager Console tools reference</span></span>](powershell.md)
* [<span data-ttu-id="cafae-113">Referencia sobre las herramientas de la CLI de EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="cafae-113">EF Core .NET CLI tools reference</span></span>](dotnet.md)