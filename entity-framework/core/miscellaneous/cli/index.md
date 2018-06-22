---
title: Referencia de la línea de comandos - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: db25ed55e3724ee71743e563f39a6e4b16c17589
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769426"
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="2beb4-102">Herramientas de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2beb4-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="2beb4-103">Las herramientas de Entity Framework Core ayudan durante el desarrollo de aplicaciones de EF Core.</span><span class="sxs-lookup"><span data-stu-id="2beb4-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="2beb4-104">Se usan principalmente para aplicar scaffolding a DbContext y a tipos de entidad mediante la aplicación de ingeniería inversa al esquema de una base de datos y para administrar migraciones.</span><span class="sxs-lookup"><span data-stu-id="2beb4-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="2beb4-105">Las [herramientas de la Consola del administrador de paquetes (PMC) de EF Core][1] proporcionan una experiencia de nivel superior dentro de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2beb4-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="2beb4-106">Ejecútelas con la [Consola del administrador de paquetes][2] de NuGet.</span><span class="sxs-lookup"><span data-stu-id="2beb4-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="2beb4-107">Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2beb4-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="2beb4-108">Las [herramientas de línea de comandos de .NET de EF Core][3] son una extensión de las [herramientas de la interfaz de la línea de comandos (CLI) de .NET Core][4], que son multiplataforma y pueden ejecutarse fuera de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2beb4-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="2beb4-109">Estas herramientas necesitan un proyecto de SDK de .NET Core (uno con `Sdk="Microsoft.NET.Sdk"` o similar en el archivo de proyecto).</span><span class="sxs-lookup"><span data-stu-id="2beb4-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="2beb4-110">Ambas herramientas exponen la misma funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="2beb4-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="2beb4-111">Si está desarrollando en Visual Studio, se recomienda usar las herramientas de la PMC, ya que proporcionan una experiencia más integrada.</span><span class="sxs-lookup"><span data-stu-id="2beb4-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="2beb4-112">Marcos de trabajo</span><span class="sxs-lookup"><span data-stu-id="2beb4-112">Frameworks</span></span>
----------
<span data-ttu-id="2beb4-113">Las herramientas admiten proyectos para .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2beb4-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="2beb4-114">Si quiere usar una biblioteca de clases, considere la posibilidad de usar una biblioteca de clases de .NET Core o .NET Framework si es posible.</span><span class="sxs-lookup"><span data-stu-id="2beb4-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="2beb4-115">De este modo se reducirán al mínimo los problemas relacionados con las herramientas de .NET.</span><span class="sxs-lookup"><span data-stu-id="2beb4-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="2beb4-116">Si en su lugar quiere usar una biblioteca de clases de .NET Standard, tendrá que usar un proyecto de inicio que tenga como destino .NET Framework o .NET Core, de modo que las herramientas dispongan de una plataforma de destino concreta en la que puedan cargar la biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="2beb4-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="2beb4-117">Este proyecto de inicio puede ser un proyecto ficticio sin código real, ya que solo es necesario para proporcionar un destino para las herramientas.</span><span class="sxs-lookup"><span data-stu-id="2beb4-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="2beb4-118">Si el proyecto tiene como destino otro marco de trabajo (por ejemplo, Windows universal o Xamarin), deberá crear una biblioteca de clases de .NET Standard independiente.</span><span class="sxs-lookup"><span data-stu-id="2beb4-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="2beb4-119">En este caso, siga las instrucciones anteriores para crear un proyecto de inicio que puedan usar las herramientas.</span><span class="sxs-lookup"><span data-stu-id="2beb4-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="2beb4-120">Proyectos de inicio y destino</span><span class="sxs-lookup"><span data-stu-id="2beb4-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="2beb4-121">Cada vez que se invoca un comando, intervienen dos proyectos: el proyecto de destino y el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="2beb4-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="2beb4-122">El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).</span><span class="sxs-lookup"><span data-stu-id="2beb4-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="2beb4-123">El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.</span><span class="sxs-lookup"><span data-stu-id="2beb4-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="2beb4-124">El proyecto de destino y el proyecto de inicio pueden ser el mismo.</span><span class="sxs-lookup"><span data-stu-id="2beb4-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
