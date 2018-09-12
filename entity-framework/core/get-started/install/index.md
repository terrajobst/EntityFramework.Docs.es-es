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
# <a name="installing-entity-framework-core"></a><span data-ttu-id="73a21-102">Instalación de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="73a21-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73a21-103">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="73a21-103">Prerequisites</span></span>

* <span data-ttu-id="73a21-104">Para desarrollar aplicaciones que tengan como destino .NET Core 2.1, instale [el SDK de .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="73a21-104">To develop apps that target .NET Core 2.1, install [the .NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="73a21-105">El SDK debe estar instalado incluso si dispone de la versión más reciente de Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="73a21-105">The SDK has to be installed even if you have the latest version of Visual Studio 2017.</span></span>

* <span data-ttu-id="73a21-106">Para poder usar Visual Studio para el desarrollo de aplicaciones que tengan como destino .NET Core 2.1, instale Visual Studio 2017 15.7 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="73a21-106">To use Visual Studio for development of apps that target .NET Core 2.1, install Visual Studio 2017 version 15.7 or later.</span></span>

* <span data-ttu-id="73a21-107">Para usar Entity Framework 2.1 en aplicaciones ASP.NET Core, use ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="73a21-107">To use Entity Framework 2.1 in ASP.NET Core applications, use ASP.NET Core 2.1.</span></span> <span data-ttu-id="73a21-108">Las aplicaciones que usen versiones anteriores de ASP.NET Core deberán actualizarse a la versión 2.1.</span><span class="sxs-lookup"><span data-stu-id="73a21-108">Applications that use earlier versions of ASP.NET Core must be updated to 2.1.</span></span>

* <span data-ttu-id="73a21-109">Puede usar Visual Studio 2015 para aplicaciones que tengan como destino .NET Framework 4.6.1 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="73a21-109">You can use Visual Studio 2015 for apps that target the .NET Framework 4.6.1 or later.</span></span> <span data-ttu-id="73a21-110">Para ello, necesitará una versión de NuGet que reconozca .NET Standard 2.0 y sus marcos compatibles.</span><span class="sxs-lookup"><span data-stu-id="73a21-110">But you need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="73a21-111">Para obtenerla en Visual Studio 2015, [actualice el cliente de NuGet a la versión 3.6.0](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="73a21-111">To get that in Visual Studio 2015, [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads).</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="73a21-112">Obtención del runtime de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="73a21-112">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="73a21-113">Para agregar las bibliotecas del runtime de EF Core a una aplicación, instale el paquete NuGet para el proveedor de bases de datos que quiera usar.</span><span class="sxs-lookup"><span data-stu-id="73a21-113">To add EF Core runtime libraries to an application, install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="73a21-114">Para obtener una lista de los proveedores admitidos y los nombres de sus paquetes NuGet, vea [Proveedores de la base de datos](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="73a21-114">For a list of supported providers and their NuGet package names, see [Database providers](../../providers/index.md).</span></span>

<span data-ttu-id="73a21-115">Para instalar o actualizar paquetes NuGet, use la CLI de .NET Core, o bien el cuadro de diálogo o la consola del Administrador de paquetes de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73a21-115">To install or update NuGet packages, use the .NET Core CLI, the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

<span data-ttu-id="73a21-116">En el caso de las aplicaciones ASP.NET Core 2.1, los proveedores de SQL Server y en memoria se incluyen automáticamente, por lo que no es necesario instalarlos por separado.</span><span class="sxs-lookup"><span data-stu-id="73a21-116">For ASP.NET Core 2.1 applications, the in-memory and SQL Server providers are automatically included, so there's no need to install them separately.</span></span>

> [!TIP]  
> <span data-ttu-id="73a21-117">Si necesita actualizar una aplicación que usa un proveedor de base de datos de terceros, busque siempre una actualización del proveedor que sea compatible con la versión de EF Core que quiere usar.</span><span class="sxs-lookup"><span data-stu-id="73a21-117">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="73a21-118">Por ejemplo, los proveedores de bases de datos de las versiones anteriores no son compatibles con la versión 2.1 del runtime de EF Core.</span><span class="sxs-lookup"><span data-stu-id="73a21-118">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

### <a name="net-core-cli"></a><span data-ttu-id="73a21-119">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="73a21-119">.NET Core CLI</span></span>

<span data-ttu-id="73a21-120">El siguiente comando de la CLI de .NET Core instala o actualiza el proveedor de SQL Server:</span><span class="sxs-lookup"><span data-stu-id="73a21-120">The following .NET Core CLI command installs or updates the SQL Server provider:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="73a21-121">Puede indicar una versión específica en el comando `dotnet add package` usando el modificador `-v`.</span><span class="sxs-lookup"><span data-stu-id="73a21-121">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="73a21-122">Por ejemplo, para instalar paquetes de EF Core 2.1.0, anexe `-v 2.1.0` al comando.</span><span class="sxs-lookup"><span data-stu-id="73a21-122">For example, to install EF Core 2.1.0 packages, append `-v 2.1.0` to the command.</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="73a21-123">Cuadro de diálogo Administrador de paquetes NuGet en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73a21-123">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="73a21-124">En el menú, seleccione **Proyecto > Administrar paquetes NuGet**</span><span class="sxs-lookup"><span data-stu-id="73a21-124">From the menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="73a21-125">Haga clic en la pestaña **Examinar** o **Actualizaciones**</span><span class="sxs-lookup"><span data-stu-id="73a21-125">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="73a21-126">Para instalar o actualizar el proveedor de SQL Server, seleccione el paquete `Microsoft.EntityFrameworkCore.SqlServer` y confirme la acción.</span><span class="sxs-lookup"><span data-stu-id="73a21-126">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="73a21-127">Para obtener más información, vea [Diálogo del Administrador de paquetes NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="73a21-127">For more information, see [NuGet Package Manager Dialog](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="73a21-128">Consola del Administrador de paquetes NuGet de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73a21-128">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="73a21-129">En el menú, seleccione **Herramientas > Administrador de paquetes NuGet > Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="73a21-129">From the menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="73a21-130">Para instalar el proveedor de SQL Server, ejecute el comando siguiente en la consola del Administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="73a21-130">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="73a21-131">Para actualizar el proveedor, use el comando `Update-Package`.</span><span class="sxs-lookup"><span data-stu-id="73a21-131">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="73a21-132">Para especificar una versión, use el modificador `-Version`.</span><span class="sxs-lookup"><span data-stu-id="73a21-132">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="73a21-133">Por ejemplo, para instalar paquetes de EF Core 2.1.0, anexe `-Version 2.1.0` a los comandos.</span><span class="sxs-lookup"><span data-stu-id="73a21-133">For example, to install EF Core 2.1.0 packages, append `-Version 2.1.0` to the commands</span></span>

<span data-ttu-id="73a21-134">Para obtener más información, vea [Consola del Administrador de paquetes](https://docs.microsoft.com/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="73a21-134">For more information, see [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console).</span></span>

## <a name="get-entity-framework-core-tools"></a><span data-ttu-id="73a21-135">Obtención de herramientas de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="73a21-135">Get Entity Framework Core tools</span></span>

<span data-ttu-id="73a21-136">Además de las bibliotecas del runtime, puede instalar herramientas que son capaces de realizar algunas tareas relacionadas con EF Core en el proyecto en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="73a21-136">Besides the runtime libraries, you can install tools that can perform some EF Core-related tasks in your project at design time.</span></span> <span data-ttu-id="73a21-137">Por ejemplo, puede crear y aplicar migraciones, así como crear un modelo según una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="73a21-137">For example, you can create migrations, apply migrations, and create a model based on an existing database.</span></span>

<span data-ttu-id="73a21-138">Existen dos conjuntos de herramientas:</span><span class="sxs-lookup"><span data-stu-id="73a21-138">Two sets of tools are available:</span></span>
* <span data-ttu-id="73a21-139">Las herramientas de [interfaz de la línea de comandos (CLI)](../../miscellaneous/cli/dotnet.md) de .NET Core pueden usarse en Windows, Linux y macOS.</span><span class="sxs-lookup"><span data-stu-id="73a21-139">The .NET Core [command-line interface (CLI) tools](../../miscellaneous/cli/dotnet.md) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="73a21-140">Estos comandos comienzan por `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="73a21-140">These commands begin with `dotnet ef`.</span></span> 
* <span data-ttu-id="73a21-141">Las [herramientas de la consola del Administrador de paquetes](../../miscellaneous/cli/powershell.md) se ejecutan en Visual Studio 2017 (Windows).</span><span class="sxs-lookup"><span data-stu-id="73a21-141">The [Package Manager Console tools](../../miscellaneous/cli/powershell.md)  run in Visual Studio 2017 on Windows.</span></span> <span data-ttu-id="73a21-142">Estos comandos empiezan por un verbo. Por ejemplo: `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="73a21-142">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="73a21-143">Aunque puede usar los comandos de `dotnet ef` desde la consola del Administrador de paquetes, usar las herramientas de la consola del Administrador de paquetes en Visual Studio resulta más práctico:</span><span class="sxs-lookup"><span data-stu-id="73a21-143">Although you can use the `dotnet ef` commands from the Package Manager Console, it's more convenient to use the Package Manager Console tools when you're using Visual Studio:</span></span>
* <span data-ttu-id="73a21-144">Estos comandos trabajan automáticamente con el proyecto actualmente seleccionado en la consola del Administrador de paquetes, sin necesidad de cambiar manualmente entre directorios.</span><span class="sxs-lookup"><span data-stu-id="73a21-144">They automatically work with the current project selected in the Package Manager Console without requiring manually switching directories.</span></span>  
* <span data-ttu-id="73a21-145">Abren automáticamente los archivos generados por los comandos de Visual Studio una vez completado el comando.</span><span class="sxs-lookup"><span data-stu-id="73a21-145">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-cli-tools"></a><span data-ttu-id="73a21-146">Obtención de las herramientas de la CLI</span><span class="sxs-lookup"><span data-stu-id="73a21-146">Get the CLI tools</span></span>

<span data-ttu-id="73a21-147">Los comandos de `dotnet ef` están incluidos en el SDK de .NET Core. Pero es necesario instalar el paquete `Microsoft.EntityFrameworkCore.Design` para habilitarlos:</span><span class="sxs-lookup"><span data-stu-id="73a21-147">The `dotnet ef` commands are included in the .NET Core SDK, but to enable the commands you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="73a21-148">Para las aplicaciones de ASP.NET Core 2.1, este paquete se incluye automáticamente.</span><span class="sxs-lookup"><span data-stu-id="73a21-148">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

<span data-ttu-id="73a21-149">Tal como se explicó anteriormente en [Requisitos previos](#prerequisites), también deberá instalar el SDK de .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="73a21-149">As explained earlier in [Prerequisites](#prerequisites), you also need to install the .NET Core 2.1 SDK.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="73a21-150">Use siempre la versión del paquete de herramientas que coincida con la versión principal de los paquetes en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="73a21-150">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="73a21-151">Obtención de las herramientas de la consola del Administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="73a21-151">Get the Package Manager Console tools</span></span>

<span data-ttu-id="73a21-152">Para obtener las herramientas de la consola del Administrador de paquetes para EF Core, instale el paquete `Microsoft.EntityFrameworkCore.Tools`:</span><span class="sxs-lookup"><span data-stu-id="73a21-152">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="73a21-153">Para las aplicaciones de ASP.NET Core 2.1, este paquete se incluye automáticamente.</span><span class="sxs-lookup"><span data-stu-id="73a21-153">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

## <a name="upgrading-to-ef-core-21"></a><span data-ttu-id="73a21-154">Actualización a EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="73a21-154">Upgrading to EF Core 2.1</span></span>

<span data-ttu-id="73a21-155">Si va a actualizar una aplicación existente a EF Core 2.1, es posible que algunas referencias a los paquetes más antiguos de EF Core tengan que quitarse manualmente:</span><span class="sxs-lookup"><span data-stu-id="73a21-155">If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>

* <span data-ttu-id="73a21-156">Los paquetes en tiempo de diseño del proveedor de base de datos, como `Microsoft.EntityFrameworkCore.SqlServer.Design`, ya no son necesarios ni se admiten en EF Core 2.1, pero no se quitan automáticamente al actualizar los demás paquetes.</span><span class="sxs-lookup"><span data-stu-id="73a21-156">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but aren't automatically removed when upgrading the other packages.</span></span>

* <span data-ttu-id="73a21-157">Las herramientas de la CLI de .NET ahora están incluidas en el SDK de .NET, por lo que se puede quitar la referencia a ese paquete desde el archivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="73a21-157">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

<span data-ttu-id="73a21-158">En el caso de las aplicaciones que tengan como destino .NET Framework y se hayan creado con versiones anteriores de Visual Studio, asegúrese de que sean compatibles con las bibliotecas de .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="73a21-158">For applications that target the .NET Framework and were created by earlier versions of Visual Studio, make sure that they are compatible with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="73a21-159">Edite el archivo de proyecto y asegúrese de que la siguiente entrada aparece en el grupo de propiedades inicial:</span><span class="sxs-lookup"><span data-stu-id="73a21-159">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="73a21-160">En los proyectos de prueba, asegúrese también de que la entrada siguiente está presente:</span><span class="sxs-lookup"><span data-stu-id="73a21-160">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
