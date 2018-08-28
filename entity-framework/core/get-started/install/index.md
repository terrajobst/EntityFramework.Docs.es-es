---
title: Instalación de EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 30ca81a0ede65506a6684d2322d31332115b1ed3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996933"
---
# <a name="installing-ef-core"></a><span data-ttu-id="573ab-102">Instalación de EF Core</span><span class="sxs-lookup"><span data-stu-id="573ab-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="573ab-103">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="573ab-103">Prerequisites</span></span>

<span data-ttu-id="573ab-104">Para desarrollar aplicaciones de .NET Core 2.1 (incluidas aplicaciones de ASP.NET Core 2.1 para .NET Core) debe descargar e instalar una versión del [SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core) que sea apropiada para la plataforma.</span><span class="sxs-lookup"><span data-stu-id="573ab-104">In order to develop .NET Core 2.1 applications (including ASP.NET Core 2.1 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="573ab-105">**Debe hacerlo aunque haya instalado Visual Studio 2017 versión 15.7.**</span><span class="sxs-lookup"><span data-stu-id="573ab-105">**This is true even if you have installed Visual Studio 2017 version 15.7.**</span></span>

<span data-ttu-id="573ab-106">Para usar EF Core 2.1 o cualquier otra biblioteca de .NET Standard 2.0 con una plataforma .NET además de .NET Core 2.1 (por ejemplo, con .NET Framework 4.6.1 o superior), necesita una versión de NuGet compatible con .NET Standard 2.0 y sus marcos de trabajo compatibles.</span><span class="sxs-lookup"><span data-stu-id="573ab-106">In order to use EF Core 2.1 or any other .NET Standard 2.0 library with a .NET platform besides .NET Core 2.1 (for example, with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="573ab-107">Estas son algunas formas de conseguirlo:</span><span class="sxs-lookup"><span data-stu-id="573ab-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="573ab-108">Instalar Visual Studio 2017 versión 15.7</span><span class="sxs-lookup"><span data-stu-id="573ab-108">Install Visual Studio 2017 version 15.7</span></span>
* <span data-ttu-id="573ab-109">Si usa Visual Studio 2015, [descargue y actualice el cliente de NuGet a la versión 3.6.0](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="573ab-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="573ab-110">Los proyectos creados con versiones anteriores de Visual Studio para .NET Framework pueden necesitar modificaciones adicionales para ser compatibles con las bibliotecas de .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="573ab-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="573ab-111">Edite el archivo de proyecto y asegúrese de que la siguiente entrada aparece en el grupo de propiedades inicial:</span><span class="sxs-lookup"><span data-stu-id="573ab-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="573ab-112">En los proyectos de prueba, asegúrese también de que la entrada siguiente está presente:</span><span class="sxs-lookup"><span data-stu-id="573ab-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="573ab-113">Recopilación de las partes</span><span class="sxs-lookup"><span data-stu-id="573ab-113">Getting the bits</span></span>
<span data-ttu-id="573ab-114">La manera recomendada de agregar las bibliotecas en tiempo de ejecución de EF Core a una aplicación es instalar un proveedor de base de datos de EF Core desde NuGet.</span><span class="sxs-lookup"><span data-stu-id="573ab-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="573ab-115">Además de las bibliotecas en tiempo de ejecución, puede instalar herramientas que faciliten la ejecución de varias tareas relacionadas con EF Core en el proyecto en tiempo de diseño, como crear y aplicar migraciones y crear un modelo basado en una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="573ab-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="573ab-116">Si necesita actualizar una aplicación que usa un proveedor de base de datos de terceros, busque siempre una actualización del proveedor que sea compatible con la versión de EF Core que quiere usar.</span><span class="sxs-lookup"><span data-stu-id="573ab-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="573ab-117">Por ejemplo, los proveedores de bases de datos de las versiones anteriores no son compatibles con la versión 2.1 del runtime de EF Core.</span><span class="sxs-lookup"><span data-stu-id="573ab-117">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="573ab-118">Las aplicaciones para ASP.NET Core 2.1 pueden usar EF Core 2.1 sin dependencias adicionales además de proveedores de bases de datos de terceros.</span><span class="sxs-lookup"><span data-stu-id="573ab-118">Applications targeting ASP.NET Core 2.1 can use EF Core 2.1 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="573ab-119">Las aplicaciones destinadas a versiones anteriores de ASP.NET Core deben actualizarse a ASP.NET Core 2.1 para usar EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="573ab-119">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.1 in order to use EF Core 2.1.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="573ab-120">Desarrollo multiplataforma con la interfaz de la línea de comandos (CLI) de .NET Core</span><span class="sxs-lookup"><span data-stu-id="573ab-120">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="573ab-121">Para desarrollar aplicaciones para [.NET Core](https://www.microsoft.com/net/download/core), puede usar los [comandos de la CLI `dotnet`](https://docs.microsoft.com/dotnet/core/tools/) en combinación con el editor de texto que prefiera o un entorno de desarrollo integrado (IDE) como Visual Studio, Visual Studio para Mac o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="573ab-121">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="573ab-122">Las aplicaciones destinadas a .NET Core requieren versiones específicas de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="573ab-122">Applications that target .NET Core require specific versions of Visual Studio.</span></span> <span data-ttu-id="573ab-123">Por ejemplo, el desarrollo de .NET Core 1.x requiere Visual Studio 2017, mientras que el desarrollo de .NET Core 2.1 requiere Visual Studio 2017, versión 15.7.</span><span class="sxs-lookup"><span data-stu-id="573ab-123">For example, .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.1 development requires Visual Studio 2017 version 15.7.</span></span>

<span data-ttu-id="573ab-124">Para instalar o actualizar el proveedor de SQL Server en una aplicación de .NET Core multiplataforma, vaya al directorio de la aplicación y ejecute lo siguiente en una línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="573ab-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="573ab-125">Puede indicar una instalación de una versión específica en el comando `dotnet add package`, con el modificador `-v`.</span><span class="sxs-lookup"><span data-stu-id="573ab-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="573ab-126">Por ejemplo, para instalar paquetes de EF Core 2.1, anexe `-v 2.1.0` al comando.</span><span class="sxs-lookup"><span data-stu-id="573ab-126">For example, to install EF Core 2.1 packages, append `-v 2.1.0` to the command.</span></span>

<span data-ttu-id="573ab-127">EF Core incluye un conjunto de [comandos adicionales para la CLI `dotnet`](../../miscellaneous/cli/dotnet.md) que empiezan por `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="573ab-127">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="573ab-128">Las herramientas de la CLI de .NET Core para EF Core requieren un paquete denominado `Microsoft.EntityFrameworkCore.Design`.</span><span class="sxs-lookup"><span data-stu-id="573ab-128">The .NET Core CLI tools for EF Core require a package called `Microsoft.EntityFrameworkCore.Design`.</span></span> <span data-ttu-id="573ab-129">Puede agregarlo al proyecto mediante:</span><span class="sxs-lookup"><span data-stu-id="573ab-129">You can add it to the project using:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> <span data-ttu-id="573ab-130">Use siempre la versión del paquete de herramientas que coincida con la versión principal de los paquetes en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="573ab-130">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="573ab-131">Desarrollo de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="573ab-131">Visual Studio development</span></span>

<span data-ttu-id="573ab-132">Puede desarrollar muchos tipos diferentes de aplicaciones para .NET Core, .NET Framework u otras plataformas compatibles con EF Core mediante Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="573ab-132">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="573ab-133">Hay dos maneras de instalar un proveedor de base de datos de EF Core en la aplicación desde Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="573ab-133">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="573ab-134">Con la [interfaz de usuario del administrador de paquetes](https://docs.microsoft.com/nuget/tools/package-manager-ui) de NuGet</span><span class="sxs-lookup"><span data-stu-id="573ab-134">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="573ab-135">Seleccione en el menú **Proyecto > Administrar paquetes NuGet**</span><span class="sxs-lookup"><span data-stu-id="573ab-135">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="573ab-136">Haga clic en la pestaña **Examinar** o **Actualizaciones**</span><span class="sxs-lookup"><span data-stu-id="573ab-136">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="573ab-137">Seleccione el paquete `Microsoft.EntityFrameworkCore.SqlServer` y la versión deseada y confirme</span><span class="sxs-lookup"><span data-stu-id="573ab-137">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="573ab-138">Con la [Consola del administrador de paquetes (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console) de NuGet</span><span class="sxs-lookup"><span data-stu-id="573ab-138">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="573ab-139">Seleccione en el menú **Herramientas > Administrador de paquetes NuGet > Consola del administrador de paquetes**</span><span class="sxs-lookup"><span data-stu-id="573ab-139">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="573ab-140">Escriba y ejecute el comando siguiente en la PMC:</span><span class="sxs-lookup"><span data-stu-id="573ab-140">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="573ab-141">Puede usar el comando `Update-Package` en su lugar para actualizar un paquete que ya está instalado a una versión más reciente</span><span class="sxs-lookup"><span data-stu-id="573ab-141">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="573ab-142">Para especificar una versión específica, puede usar el modificador `-Version`.</span><span class="sxs-lookup"><span data-stu-id="573ab-142">To specify a specific version, you can use the `-Version` modifier.</span></span> <span data-ttu-id="573ab-143">Por ejemplo, para instalar paquetes de EF Core 2.1, agregue `-Version 2.1.0` a los comandos</span><span class="sxs-lookup"><span data-stu-id="573ab-143">For example, to install EF Core 2.1 packages, append `-Version 2.1.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="573ab-144">Herramientas</span><span class="sxs-lookup"><span data-stu-id="573ab-144">Tools</span></span>

<span data-ttu-id="573ab-145">También hay una versión de PowerShell de los [comandos de EF Core que se ejecutan dentro de la PMC](../../miscellaneous/cli/powershell.md) en Visual Studio, con capacidades similares a las de los comandos `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="573ab-145">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> 

> [!TIP]  
> <span data-ttu-id="573ab-146">Aunque es posible usar los comandos `dotnet ef` desde la PMC en Visual Studio, es mucho más conveniente usar la versión de PowerShell:</span><span class="sxs-lookup"><span data-stu-id="573ab-146">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="573ab-147">Trabajan automáticamente con el proyecto actual seleccionado en la PMC sin necesidad de cambiar manualmente entre directorios.</span><span class="sxs-lookup"><span data-stu-id="573ab-147">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="573ab-148">Abren automáticamente los archivos generados por los comandos de Visual Studio una vez completado el comando.</span><span class="sxs-lookup"><span data-stu-id="573ab-148">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="573ab-149">**Paquetes en desuso de EF Core 2.1:** si va a actualizar una aplicación existente a EF Core 2.1, es posible que algunas referencias a los paquetes más antiguos de EF Core tengan que quitarse manualmente:</span><span class="sxs-lookup"><span data-stu-id="573ab-149">**Deprecated packages in EF Core 2.1:** If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>
> * <span data-ttu-id="573ab-150">Los paquetes en tiempo de diseño del proveedor de base de datos, como `Microsoft.EntityFrameworkCore.SqlServer.Design`, ya no son necesarios ni se admiten en EF Core 2.1, pero no se quitan automáticamente al actualizar los demás paquetes.</span><span class="sxs-lookup"><span data-stu-id="573ab-150">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but will not be automatically removed when upgrading the other packages.</span></span>
> * <span data-ttu-id="573ab-151">Las herramientas de la CLI de .NET ahora están incluidas en el SDK de .NET, por lo que se puede quitar la referencia a ese paquete desde el archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="573ab-151">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
