---
title: Obtener Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
caps.latest.revision: 4
ms.openlocfilehash: 400bf1428e6754a88dbc1264c346bb66282725a0
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122755"
---
# <a name="get-entity-framework"></a><span data-ttu-id="6a0ce-102">Obtener Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6a0ce-102">Get Entity Framework</span></span>
<span data-ttu-id="6a0ce-103">Entity Framework está formado por las herramientas de EF para Visual Studio y el tiempo de ejecución de EF.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="6a0ce-104">Herramientas EF para Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a0ce-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="6a0ce-105">Entity Framework Tools para Visual Studio incluyen EF Designer y el Asistente para modelo de EF y son necesarios para la base de datos en primer lugar y modelar flujos de trabajo primeros.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="6a0ce-106">Herramientas EF se incluyen en todas las versiones recientes de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="6a0ce-107">Si lleva a cabo una instalación personalizada de Visual Studio, deberá asegurarse de que el elemento se selecciona "Herramientas de Entity Framework 6" eligiendo una carga de trabajo que lo incluye o seleccionando como un componente individual.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="6a0ce-108">Para algunas versiones anteriores de Visual Studio, herramientas actualizadas de EF están disponibles como descarga.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="6a0ce-109">Consulte [versiones de Visual Studio](~/ef6/what-is-new/visual-studio.md) para obtener instrucciones sobre cómo obtener la versión más reciente de EF herramientas disponibles para su versión de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="6a0ce-110">En tiempo de ejecución EF</span><span class="sxs-lookup"><span data-stu-id="6a0ce-110">EF Runtime</span></span>

<span data-ttu-id="6a0ce-111">La versión más reciente de Entity Framework está disponible como la [paquete EntityFramework NuGet](http://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="6a0ce-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="6a0ce-112">Si no está familiarizado con el Administrador de paquetes de NuGet, le recomendamos que lea el [información general sobre NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="6a0ce-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="6a0ce-113">Instalar el paquete de NuGet EF</span><span class="sxs-lookup"><span data-stu-id="6a0ce-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="6a0ce-114">Puede instalar el paquete de Entity Framework con el botón secundario en el **referencias** carpeta del proyecto y seleccionando **administrar paquetes NuGet...**</span><span class="sxs-lookup"><span data-stu-id="6a0ce-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="6a0ce-116">Instalación desde la consola de administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="6a0ce-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="6a0ce-117">Como alternativa, puede instalar EntityFramework ejecutando el siguiente comando en el [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="6a0ce-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="6a0ce-118">Instalar una versión concreta de EF</span><span class="sxs-lookup"><span data-stu-id="6a0ce-118">Installing a specific version of EF</span></span>

<span data-ttu-id="6a0ce-119">De EF 4.1 y versiones posteriores, se publican nuevas versiones del tiempo de ejecución EF como el [paquete NuGet de EntityFramework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="6a0ce-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Pacakge](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="6a0ce-120">Cualquiera de estas versiones se pueden agregar a un proyecto basado en .NET Framework ejecutando el siguiente comando en Visual Studio [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span><span class="sxs-lookup"><span data-stu-id="6a0ce-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="6a0ce-121">Tenga en cuenta que `<number>` representa la versión específica de EF para instalar.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="6a0ce-122">Por ejemplo, la 6.2.0 es la versión del número de EF 6.2.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="6a0ce-123">Tiempos de ejecución EF antes 4.1 formaban parte de .NET Framework y no se puede instalar por separado.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="6a0ce-124">Instalar la última versión preliminar</span><span class="sxs-lookup"><span data-stu-id="6a0ce-124">Installing the Latest Preview</span></span>

<span data-ttu-id="6a0ce-125">Los métodos anteriores le proporcionará la versión más reciente totalmente compatible con la versión de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="6a0ce-126">A menudo hay versiones preliminares de disponible que nos encantaría que probar y envíenos sus comentarios acerca de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="6a0ce-127">Para instalar la versión preliminar más reciente de Entity Framework puede seleccionar **incluir versión preliminar** en la ventana Administrar paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="6a0ce-128">Si no hay ninguna versión preliminar está disponible automáticamente obtendrá la última versión compatible totalmente de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6a0ce-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![IncludePreRelease](~/ef6/media/includeprerelease.png)

<span data-ttu-id="6a0ce-130">Como alternativa, puede ejecutar el comando siguiente el [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="6a0ce-130">Alternatively, you can run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
