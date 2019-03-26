---
title: Referencia de las herramientas EF Core (consola de administrador de paquetes) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: cb05e3fb66adf96f8a6778711a76520d0be24c71
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419775"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="fc001-102">Referencia: consola de administrador de paquetes en Visual Studio de las herramientas de Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="fc001-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="fc001-103">Las herramientas de la consola de administrador de paquetes (PMC) para Entity Framework Core realizan tareas de desarrollo de tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="fc001-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="fc001-104">Por ejemplo, que crean [migraciones](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), aplicar las migraciones y generar código para un modelo basado en una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="fc001-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="fc001-105">Los comandos se ejecutan dentro de Visual Studio mediante el [Package Manager Console](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="fc001-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="fc001-106">Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fc001-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="fc001-107">Si no usa Visual Studio, se recomienda la [herramientas de línea de comandos de EF Core](dotnet.md) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="fc001-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="fc001-108">Las herramientas de CLI son multiplataforma y se ejecutan dentro de un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="fc001-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="fc001-109">Instalación de las herramientas</span><span class="sxs-lookup"><span data-stu-id="fc001-109">Installing the tools</span></span>

<span data-ttu-id="fc001-110">Los procedimientos para instalar y actualizar las herramientas difieren entre ASP.NET Core 2.1 + y versiones anteriores u otros tipos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="fc001-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="fc001-111">ASP.NET Core 2.1 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="fc001-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="fc001-112">Las herramientas se incluyen automáticamente en un proyecto de ASP.NET Core 2.1 + porque el `Microsoft.EntityFrameworkCore.Tools` paquete se incluye en el [Microsoft.AspNetCore.App metapaquete](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="fc001-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="fc001-113">Por lo tanto, no tiene que hacer nada para instalar las herramientas, pero es necesario:</span><span class="sxs-lookup"><span data-stu-id="fc001-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="fc001-114">Restaurar los paquetes antes de usar las herramientas en un proyecto nuevo.</span><span class="sxs-lookup"><span data-stu-id="fc001-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="fc001-115">Instalar un paquete para actualizar las herramientas a una versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="fc001-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="fc001-116">Para asegurarse de que está obteniendo la versión más reciente de las herramientas, se recomienda que también realice el paso siguiente:</span><span class="sxs-lookup"><span data-stu-id="fc001-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="fc001-117">Editar su *.csproj* archivo y agregue una línea que especifica la versión más reciente de la [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) paquete.</span><span class="sxs-lookup"><span data-stu-id="fc001-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="fc001-118">Por ejemplo, el *.csproj* archivo puede incluir un `ItemGroup` similar a este:</span><span class="sxs-lookup"><span data-stu-id="fc001-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="fc001-119">Actualizar las herramientas cuando reciba un mensaje similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fc001-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="fc001-120">La versión de herramientas de EF Core '2.1.1-rtm-30846' es mayor que el tiempo de ejecución '2.1.3-rtm-32065'.</span><span class="sxs-lookup"><span data-stu-id="fc001-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="fc001-121">Actualizar las herramientas para las últimas características y correcciones de errores.</span><span class="sxs-lookup"><span data-stu-id="fc001-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="fc001-122">Para actualizar las herramientas:</span><span class="sxs-lookup"><span data-stu-id="fc001-122">To update the tools:</span></span>
* <span data-ttu-id="fc001-123">Instale el SDK de .NET Core más reciente.</span><span class="sxs-lookup"><span data-stu-id="fc001-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="fc001-124">Actualización a la versión más reciente de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fc001-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="fc001-125">Editar el *.csproj* de archivos para que incluya una referencia de paquete para el paquete de herramientas más reciente, como se mostró anteriormente.</span><span class="sxs-lookup"><span data-stu-id="fc001-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="fc001-126">Otras versiones y tipos de proyecto</span><span class="sxs-lookup"><span data-stu-id="fc001-126">Other versions and project types</span></span>

<span data-ttu-id="fc001-127">Instalar las herramientas de la consola de administrador de paquetes con el comando siguiente **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="fc001-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="fc001-128">Actualizar las herramientas ejecutando el siguiente comando en **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="fc001-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="fc001-129">Comprobar la instalación</span><span class="sxs-lookup"><span data-stu-id="fc001-129">Verify the installation</span></span>

<span data-ttu-id="fc001-130">Compruebe que las herramientas están instaladas, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="fc001-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="fc001-131">El resultado será similar al siguiente (no le diga qué versión de las herramientas que se usa):</span><span class="sxs-lookup"><span data-stu-id="fc001-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a><span data-ttu-id="fc001-132">Uso de las herramientas</span><span class="sxs-lookup"><span data-stu-id="fc001-132">Using the tools</span></span>

<span data-ttu-id="fc001-133">Antes de usar las herramientas:</span><span class="sxs-lookup"><span data-stu-id="fc001-133">Before using the tools:</span></span>
* <span data-ttu-id="fc001-134">Comprender la diferencia entre el proyecto de inicio y de destino.</span><span class="sxs-lookup"><span data-stu-id="fc001-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="fc001-135">Obtenga información sobre cómo usar las herramientas con las bibliotecas de clases .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="fc001-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="fc001-136">Para los proyectos de ASP.NET Core, establezca el entorno.</span><span class="sxs-lookup"><span data-stu-id="fc001-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="fc001-137">Proyecto de inicio y de destino</span><span class="sxs-lookup"><span data-stu-id="fc001-137">Target and startup project</span></span>

<span data-ttu-id="fc001-138">Los comandos que hacen referencia a un *proyecto* y un *proyecto de inicio*.</span><span class="sxs-lookup"><span data-stu-id="fc001-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="fc001-139">El *proyecto* también es conocido como el *proyecto de destino* porque es donde los comandos Agregar o quitar archivos.</span><span class="sxs-lookup"><span data-stu-id="fc001-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="fc001-140">De forma predeterminada, el **proyecto predeterminado** seleccionado en **Package Manager Console** es el proyecto de destino.</span><span class="sxs-lookup"><span data-stu-id="fc001-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="fc001-141">Puede especificar otro proyecto como proyecto de destino mediante el <nobr> `--project` </nobr> opción.</span><span class="sxs-lookup"><span data-stu-id="fc001-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="fc001-142">El *proyecto de inicio* es lo que las herramientas de compilación y ejecutan.</span><span class="sxs-lookup"><span data-stu-id="fc001-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="fc001-143">Tienen las herramientas ejecutar código de la aplicación en tiempo de diseño para obtener información sobre el proyecto, como la cadena de conexión de base de datos y la configuración del modelo.</span><span class="sxs-lookup"><span data-stu-id="fc001-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="fc001-144">De forma predeterminada, el **proyecto de inicio** en **el Explorador de soluciones** es el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="fc001-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="fc001-145">Puede especificar otro proyecto como proyecto de inicio mediante el <nobr> `--startup-project` </nobr> opción.</span><span class="sxs-lookup"><span data-stu-id="fc001-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="fc001-146">El proyecto de inicio y el proyecto de destino suelen ser el mismo proyecto.</span><span class="sxs-lookup"><span data-stu-id="fc001-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="fc001-147">Un escenario típico, donde son proyectos independientes es cuando:</span><span class="sxs-lookup"><span data-stu-id="fc001-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="fc001-148">Las clases de entidad y el contexto de EF Core están en una biblioteca de clases de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fc001-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="fc001-149">Una aplicación de consola .NET Core o una aplicación web hace referencia a la biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="fc001-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="fc001-150">También es posible [colocar código de migraciones en una biblioteca de clases independiente desde el contexto de EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="fc001-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="fc001-151">Otros marcos de destino</span><span class="sxs-lookup"><span data-stu-id="fc001-151">Other target frameworks</span></span>

<span data-ttu-id="fc001-152">Las herramientas de la consola de administrador de paquetes de trabajar con proyectos de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="fc001-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="fc001-153">Las aplicaciones que tienen el modelo de EF Core en una biblioteca de clases .NET Standard que no tenga un proyecto de .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="fc001-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="fc001-154">Por ejemplo, esto es cierto de aplicaciones de Xamarin y plataforma Universal de Windows.</span><span class="sxs-lookup"><span data-stu-id="fc001-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="fc001-155">En tales casos, puede crear un proyecto de aplicación de consola .NET Core o .NET Framework cuyo único propósito es actuar como proyecto de inicio para las herramientas.</span><span class="sxs-lookup"><span data-stu-id="fc001-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="fc001-156">El proyecto puede ser un proyecto ficticio sin código real &mdash; solo es necesario proporcionar un destino para las herramientas.</span><span class="sxs-lookup"><span data-stu-id="fc001-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="fc001-157">¿Por qué es un proyecto ficticio necesario?</span><span class="sxs-lookup"><span data-stu-id="fc001-157">Why is a dummy project required?</span></span> <span data-ttu-id="fc001-158">Como se mencionó anteriormente, tienen las herramientas ejecutar código de la aplicación en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="fc001-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="fc001-159">Para ello, deben usar el runtime de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="fc001-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="fc001-160">Cuando el modelo de EF Core está en un proyecto destinado a .NET Core o .NET Framework, las herramientas de EF Core pedir prestado el tiempo de ejecución del proyecto.</span><span class="sxs-lookup"><span data-stu-id="fc001-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="fc001-161">No puede hacerlo si el modelo de EF Core está en una biblioteca de clases .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="fc001-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="fc001-162">.NET Standard no es una implementación real. NET; es una especificación de un conjunto de API que las implementaciones de .NET deben admitir.</span><span class="sxs-lookup"><span data-stu-id="fc001-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="fc001-163">Por lo tanto, .NET Standard no es suficiente para las herramientas de EF Core ejecutar código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fc001-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="fc001-164">El proyecto ficticio creado para usar como proyecto de inicio proporciona una plataforma de destino concreta en la que las herramientas pueden cargar la biblioteca de clases .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="fc001-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="fc001-165">Entorno de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc001-165">ASP.NET Core environment</span></span>

<span data-ttu-id="fc001-166">Para especificar el entorno para proyectos de ASP.NET Core, establezca **env:ASPNETCORE_ENVIRONMENT** antes de ejecutar comandos.</span><span class="sxs-lookup"><span data-stu-id="fc001-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="fc001-167">Parámetros comunes</span><span class="sxs-lookup"><span data-stu-id="fc001-167">Common parameters</span></span>

<span data-ttu-id="fc001-168">La siguiente tabla muestra los parámetros que son comunes a todos los comandos de EF Core:</span><span class="sxs-lookup"><span data-stu-id="fc001-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="fc001-169">Parámetro</span><span class="sxs-lookup"><span data-stu-id="fc001-169">Parameter</span></span>                 | <span data-ttu-id="fc001-170">Descripción</span><span class="sxs-lookup"><span data-stu-id="fc001-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fc001-171">-Contexto \<cadena ></span><span class="sxs-lookup"><span data-stu-id="fc001-171">-Context \<String></span></span>        | <span data-ttu-id="fc001-172">La clase `DbContext` que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="fc001-172">The `DbContext` class to use.</span></span> <span data-ttu-id="fc001-173">Nombre de clase completa con espacios de nombres o única.</span><span class="sxs-lookup"><span data-stu-id="fc001-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="fc001-174">Si se omite este parámetro, EF Core busca la clase de contexto.</span><span class="sxs-lookup"><span data-stu-id="fc001-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="fc001-175">Si hay varias clases de contexto, este parámetro es necesario.</span><span class="sxs-lookup"><span data-stu-id="fc001-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="fc001-176">-Proyecto \<cadena ></span><span class="sxs-lookup"><span data-stu-id="fc001-176">-Project \<String></span></span>        | <span data-ttu-id="fc001-177">El proyecto de destino.</span><span class="sxs-lookup"><span data-stu-id="fc001-177">The target project.</span></span> <span data-ttu-id="fc001-178">Si se omite este parámetro, el **proyecto predeterminado** para **Package Manager Console** se utiliza como el proyecto de destino.</span><span class="sxs-lookup"><span data-stu-id="fc001-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="fc001-179">-StartupProject \<String></span><span class="sxs-lookup"><span data-stu-id="fc001-179">-StartupProject \<String></span></span> | <span data-ttu-id="fc001-180">El proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="fc001-180">The startup project.</span></span> <span data-ttu-id="fc001-181">Si se omite este parámetro, el **proyecto de inicio** en **propiedades de la solución** se utiliza como el proyecto de destino.</span><span class="sxs-lookup"><span data-stu-id="fc001-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="fc001-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="fc001-182">-Verbose</span></span>                  | <span data-ttu-id="fc001-183">Mostrar resultados detallados.</span><span class="sxs-lookup"><span data-stu-id="fc001-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="fc001-184">Para mostrar información de ayuda sobre un comando, use PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="fc001-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="fc001-185">Los parámetros de contexto, el proyecto y el proyecto de inicio admiten la expansión de ficha.</span><span class="sxs-lookup"><span data-stu-id="fc001-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="fc001-186">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="fc001-186">Add-Migration</span></span>

<span data-ttu-id="fc001-187">Agrega una nueva migración.</span><span class="sxs-lookup"><span data-stu-id="fc001-187">Adds a new migration.</span></span>

<span data-ttu-id="fc001-188">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="fc001-188">Parameters:</span></span>

| <span data-ttu-id="fc001-189">Parámetro</span><span class="sxs-lookup"><span data-stu-id="fc001-189">Parameter</span></span>                         | <span data-ttu-id="fc001-190">Descripción</span><span class="sxs-lookup"><span data-stu-id="fc001-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fc001-191"><nobr>-Nombre \<cadena ><nobr></span><span class="sxs-lookup"><span data-stu-id="fc001-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="fc001-192">El nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="fc001-192">The name of the migration.</span></span> <span data-ttu-id="fc001-193">Esto es un parámetro posicional y es necesario.</span><span class="sxs-lookup"><span data-stu-id="fc001-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="fc001-194"><nobr>-OutputDir \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="fc001-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="fc001-195">El directorio (y subespacio de nombres) para usar.</span><span class="sxs-lookup"><span data-stu-id="fc001-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="fc001-196">Son las rutas de acceso relativa al directorio del proyecto de destino.</span><span class="sxs-lookup"><span data-stu-id="fc001-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="fc001-197">El valor predeterminado es "Migraciones".</span><span class="sxs-lookup"><span data-stu-id="fc001-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="fc001-198">Quitar la base de datos</span><span class="sxs-lookup"><span data-stu-id="fc001-198">Drop-Database</span></span>

<span data-ttu-id="fc001-199">Quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc001-199">Drops the database.</span></span>

<span data-ttu-id="fc001-200">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="fc001-200">Parameters:</span></span>

| <span data-ttu-id="fc001-201">Parámetro</span><span class="sxs-lookup"><span data-stu-id="fc001-201">Parameter</span></span> | <span data-ttu-id="fc001-202">Descripción</span><span class="sxs-lookup"><span data-stu-id="fc001-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="fc001-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="fc001-203">-WhatIf</span></span>   | <span data-ttu-id="fc001-204">Mostrar qué base de datos se quitarían, pero no colóquelo.</span><span class="sxs-lookup"><span data-stu-id="fc001-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="fc001-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="fc001-205">Get-DbContext</span></span>

<span data-ttu-id="fc001-206">Obtiene información sobre un `DbContext` tipo.</span><span class="sxs-lookup"><span data-stu-id="fc001-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="fc001-207">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="fc001-207">Remove-Migration</span></span>

<span data-ttu-id="fc001-208">Quita la última migración (revierte los cambios de código que se realizaron para la migración).</span><span class="sxs-lookup"><span data-stu-id="fc001-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="fc001-209">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="fc001-209">Parameters:</span></span>

| <span data-ttu-id="fc001-210">Parámetro</span><span class="sxs-lookup"><span data-stu-id="fc001-210">Parameter</span></span> | <span data-ttu-id="fc001-211">Descripción</span><span class="sxs-lookup"><span data-stu-id="fc001-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="fc001-212">-Force</span><span class="sxs-lookup"><span data-stu-id="fc001-212">-Force</span></span>    | <span data-ttu-id="fc001-213">Revertir la migración (revertir los cambios que se aplicaron a la base de datos).</span><span class="sxs-lookup"><span data-stu-id="fc001-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="fc001-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="fc001-214">Scaffold-DbContext</span></span>

<span data-ttu-id="fc001-215">Genera código para un `DbContext` y tipos de entidad para una base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc001-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="fc001-216">En orden para `Scaffold-DbContext` para generar un tipo de entidad, la tabla de base de datos debe tener una clave principal.</span><span class="sxs-lookup"><span data-stu-id="fc001-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="fc001-217">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="fc001-217">Parameters:</span></span>

| <span data-ttu-id="fc001-218">Parámetro</span><span class="sxs-lookup"><span data-stu-id="fc001-218">Parameter</span></span>                          | <span data-ttu-id="fc001-219">Descripción</span><span class="sxs-lookup"><span data-stu-id="fc001-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fc001-220"><nobr>-Connection \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="fc001-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="fc001-221">La cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc001-221">The connection string to the database.</span></span> <span data-ttu-id="fc001-222">Para los proyectos de ASP.NET Core 2.x, el valor puede ser *nombre =\<nombre de cadena de conexión >*.</span><span class="sxs-lookup"><span data-stu-id="fc001-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="fc001-223">En ese caso, el nombre procede de los orígenes de configuración que se configuran para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="fc001-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="fc001-224">Esto es un parámetro posicional y es necesario.</span><span class="sxs-lookup"><span data-stu-id="fc001-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="fc001-225"><nobr>-Proveedor \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="fc001-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="fc001-226">El proveedor debe usar.</span><span class="sxs-lookup"><span data-stu-id="fc001-226">The provider to use.</span></span> <span data-ttu-id="fc001-227">Normalmente, esto es el nombre del paquete de NuGet, por ejemplo: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="fc001-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="fc001-228">Esto es un parámetro posicional y es necesario.</span><span class="sxs-lookup"><span data-stu-id="fc001-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="fc001-229">-OutputDir \<cadena ></span><span class="sxs-lookup"><span data-stu-id="fc001-229">-OutputDir \<String></span></span>               | <span data-ttu-id="fc001-230">Para poner los archivos en el directorio.</span><span class="sxs-lookup"><span data-stu-id="fc001-230">The directory to put files in.</span></span> <span data-ttu-id="fc001-231">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="fc001-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="fc001-232">-ContextDir \<cadena ></span><span class="sxs-lookup"><span data-stu-id="fc001-232">-ContextDir \<String></span></span>              | <span data-ttu-id="fc001-233">El directorio para colocar el `DbContext` archivo.</span><span class="sxs-lookup"><span data-stu-id="fc001-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="fc001-234">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="fc001-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="fc001-235">-Contexto \<cadena ></span><span class="sxs-lookup"><span data-stu-id="fc001-235">-Context \<String></span></span>                 | <span data-ttu-id="fc001-236">El nombre de la `DbContext` clase genere.</span><span class="sxs-lookup"><span data-stu-id="fc001-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="fc001-237">-Schemas \<String[]></span><span class="sxs-lookup"><span data-stu-id="fc001-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="fc001-238">Los esquemas de tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="fc001-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="fc001-239">Si se omite este parámetro, se incluyen todos los esquemas.</span><span class="sxs-lookup"><span data-stu-id="fc001-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="fc001-240">-Tablas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="fc001-240">-Tables \<String[]></span></span>                | <span data-ttu-id="fc001-241">Para generar tipos de entidad para las tablas.</span><span class="sxs-lookup"><span data-stu-id="fc001-241">The tables to generate entity types for.</span></span> <span data-ttu-id="fc001-242">Si se omite este parámetro, se incluyen todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="fc001-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="fc001-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="fc001-243">-DataAnnotations</span></span>                   | <span data-ttu-id="fc001-244">Usar atributos para configurar el modelo (donde sea posible).</span><span class="sxs-lookup"><span data-stu-id="fc001-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="fc001-245">Si se omite este parámetro, se usa solo la API fluida.</span><span class="sxs-lookup"><span data-stu-id="fc001-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="fc001-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="fc001-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="fc001-247">Usar nombres de tabla y columna exactamente como aparecen en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc001-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="fc001-248">Si se omite este parámetro, se cambian los nombres de base de datos que mejor se ajuste a convenciones de estilo de nombre de C#.</span><span class="sxs-lookup"><span data-stu-id="fc001-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="fc001-249">-Force</span><span class="sxs-lookup"><span data-stu-id="fc001-249">-Force</span></span>                             | <span data-ttu-id="fc001-250">Sobrescribir archivos existentes.</span><span class="sxs-lookup"><span data-stu-id="fc001-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="fc001-251">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fc001-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="fc001-252">Ejemplo que se aplica la técnica scaffolding únicamente las tablas seleccionadas y crea el contexto en una carpeta independiente con un nombre especificado:</span><span class="sxs-lookup"><span data-stu-id="fc001-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="fc001-253">Script-Migration</span><span class="sxs-lookup"><span data-stu-id="fc001-253">Script-Migration</span></span>

<span data-ttu-id="fc001-254">Genera un script SQL que se aplica a todos los cambios de una migración seleccionada a otra migración seleccionado.</span><span class="sxs-lookup"><span data-stu-id="fc001-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="fc001-255">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="fc001-255">Parameters:</span></span>

| <span data-ttu-id="fc001-256">Parámetro</span><span class="sxs-lookup"><span data-stu-id="fc001-256">Parameter</span></span>                | <span data-ttu-id="fc001-257">Descripción</span><span class="sxs-lookup"><span data-stu-id="fc001-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fc001-258">*-From* \<String></span><span class="sxs-lookup"><span data-stu-id="fc001-258">*-From* \<String></span></span>        | <span data-ttu-id="fc001-259">La migración inicial.</span><span class="sxs-lookup"><span data-stu-id="fc001-259">The starting migration.</span></span> <span data-ttu-id="fc001-260">Las migraciones pueden identificarse por su nombre o identificador.</span><span class="sxs-lookup"><span data-stu-id="fc001-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="fc001-261">El número 0 es un caso especial que significa *antes de la primera migración*.</span><span class="sxs-lookup"><span data-stu-id="fc001-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="fc001-262">El valor predeterminado es 0.</span><span class="sxs-lookup"><span data-stu-id="fc001-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="fc001-263">*-To* \<String></span><span class="sxs-lookup"><span data-stu-id="fc001-263">*-To* \<String></span></span>          | <span data-ttu-id="fc001-264">La migración final.</span><span class="sxs-lookup"><span data-stu-id="fc001-264">The ending migration.</span></span> <span data-ttu-id="fc001-265">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="fc001-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="fc001-266"><nobr>-Idempotent</nobr></span><span class="sxs-lookup"><span data-stu-id="fc001-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="fc001-267">Generar un script que se puede usar en una base de datos en cualquier migración.</span><span class="sxs-lookup"><span data-stu-id="fc001-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="fc001-268">-Output \<cadena ></span><span class="sxs-lookup"><span data-stu-id="fc001-268">-Output \<String></span></span>        | <span data-ttu-id="fc001-269">Para escribir el resultado en el archivo.</span><span class="sxs-lookup"><span data-stu-id="fc001-269">The file to write the result to.</span></span> <span data-ttu-id="fc001-270">Si se omite este parámetro, se crea el archivo con un nombre generado en la misma carpeta cuando se crean archivos de tiempo de ejecución de la aplicación, por ejemplo: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span><span class="sxs-lookup"><span data-stu-id="fc001-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="fc001-271">El To, From, y los parámetros de salida admiten la expansión de ficha.</span><span class="sxs-lookup"><span data-stu-id="fc001-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="fc001-272">El ejemplo siguiente crea una secuencia de comandos para la migración de InitialCreate, usando el nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="fc001-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="fc001-273">En el ejemplo siguiente se crea una secuencia de comandos para todas las migraciones tras la migración InitialCreate, utilizando el identificador de la migración.</span><span class="sxs-lookup"><span data-stu-id="fc001-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="fc001-274">Actualizar base de datos</span><span class="sxs-lookup"><span data-stu-id="fc001-274">Update-Database</span></span>

<span data-ttu-id="fc001-275">Actualiza la base de datos a la última migración o a una migración especificada.</span><span class="sxs-lookup"><span data-stu-id="fc001-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="fc001-276">Parámetro</span><span class="sxs-lookup"><span data-stu-id="fc001-276">Parameter</span></span>                           | <span data-ttu-id="fc001-277">Descripción</span><span class="sxs-lookup"><span data-stu-id="fc001-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fc001-278"><nobr>*-Migración* \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="fc001-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="fc001-279">La migración de destino.</span><span class="sxs-lookup"><span data-stu-id="fc001-279">The target migration.</span></span> <span data-ttu-id="fc001-280">Las migraciones pueden identificarse por su nombre o identificador.</span><span class="sxs-lookup"><span data-stu-id="fc001-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="fc001-281">El número 0 es un caso especial que significa *antes de la primera migración* y hace que todas las migraciones se puede revertir.</span><span class="sxs-lookup"><span data-stu-id="fc001-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="fc001-282">Si no se especifica ninguna migración, lo valores predeterminados del comando a la última migración.</span><span class="sxs-lookup"><span data-stu-id="fc001-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="fc001-283">El parámetro de la migración es compatible con la expansión de ficha.</span><span class="sxs-lookup"><span data-stu-id="fc001-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="fc001-284">El ejemplo siguiente devuelve todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="fc001-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="fc001-285">Los ejemplos siguientes actualización la base de datos para una migración especificada.</span><span class="sxs-lookup"><span data-stu-id="fc001-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="fc001-286">El primero usa el nombre de la migración y la segunda usa el identificador de la migración:</span><span class="sxs-lookup"><span data-stu-id="fc001-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="fc001-287">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fc001-287">Additional resources</span></span>

* [<span data-ttu-id="fc001-288">Migraciones</span><span class="sxs-lookup"><span data-stu-id="fc001-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="fc001-289">Ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="fc001-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
