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
<a name="entity-framework-core-tools"></a><span data-ttu-id="46412-102">Herramientas de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="46412-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="46412-103">Las herramientas de Entity Framework Core ayudan durante el desarrollo de aplicaciones de EF Core.</span><span class="sxs-lookup"><span data-stu-id="46412-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="46412-104">Se usan principalmente para aplicar scaffolding a DbContext y a tipos de entidad mediante la aplicación de ingeniería inversa al esquema de una base de datos y para administrar migraciones.</span><span class="sxs-lookup"><span data-stu-id="46412-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="46412-105">Las [herramientas de la Consola del administrador de paquetes (PMC) de EF Core][1] proporcionan una experiencia de nivel superior dentro de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46412-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="46412-106">Ejecútelas con la [Consola del administrador de paquetes][2] de NuGet.</span><span class="sxs-lookup"><span data-stu-id="46412-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="46412-107">Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="46412-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="46412-108">Las [herramientas de línea de comandos de .NET de EF Core][3] son una extensión de las [herramientas de la interfaz de la línea de comandos (CLI) de .NET Core][4], que son multiplataforma y pueden ejecutarse fuera de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46412-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="46412-109">Estas herramientas necesitan un proyecto de SDK de .NET Core (uno con `Sdk="Microsoft.NET.Sdk"` o similar en el archivo de proyecto).</span><span class="sxs-lookup"><span data-stu-id="46412-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="46412-110">Ambas herramientas exponen la misma funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="46412-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="46412-111">Si está desarrollando en Visual Studio, se recomienda usar las herramientas de la PMC, ya que proporcionan una experiencia más integrada.</span><span class="sxs-lookup"><span data-stu-id="46412-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="46412-112">Marcos de trabajo</span><span class="sxs-lookup"><span data-stu-id="46412-112">Frameworks</span></span>
----------
<span data-ttu-id="46412-113">Las herramientas admiten proyectos para .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="46412-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="46412-114">Si el proyecto tiene como destino otro marco de trabajo (por ejemplo, Universal Windows o Xamarin), se recomienda crear un proyecto independiente de .NET Standard y hacer que uno de los marcos admitidos sea multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="46412-114">If your project targets another framework (for example, Universal Windows or Xamarin), we recommend creating a separate .NET Standard project and cross-targeting one of the supported frameworks.</span></span>

<span data-ttu-id="46412-115">Para hacer a .NET Core multiplataforma, por ejemplo, haga clic con el botón derecho en el proyecto y seleccione **Editar \*.csproj**.</span><span class="sxs-lookup"><span data-stu-id="46412-115">To cross-target .NET Core, for example, right-click on the project and select **Edit \*.csproj**.</span></span> <span data-ttu-id="46412-116">Actualice la propiedad `TargetFramework` como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="46412-116">Update the `TargetFramework` property as follows.</span></span> <span data-ttu-id="46412-117">(Tenga en cuenta que el nombre de la propiedad se convierte en plural).</span><span class="sxs-lookup"><span data-stu-id="46412-117">(Note, the property name becomes plural.)</span></span>

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

<span data-ttu-id="46412-118">Si usa una biblioteca de clases .NET Standard, no es necesario convertir en multiplataforma si el proyecto de inicio tiene como destino .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="46412-118">If you're using a .NET Standard class library, you don't need to cross-target if your startup project targets .NET Framework or .NET Core.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="46412-119">Proyectos de inicio y destino</span><span class="sxs-lookup"><span data-stu-id="46412-119">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="46412-120">Cada vez que se invoca un comando, intervienen dos proyectos: el proyecto de destino y el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="46412-120">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="46412-121">El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).</span><span class="sxs-lookup"><span data-stu-id="46412-121">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="46412-122">El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.</span><span class="sxs-lookup"><span data-stu-id="46412-122">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="46412-123">El proyecto de destino y el proyecto de inicio pueden ser el mismo.</span><span class="sxs-lookup"><span data-stu-id="46412-123">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
