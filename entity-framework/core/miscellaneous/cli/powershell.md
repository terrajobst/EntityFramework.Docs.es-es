---
title: Referencia de herramientas de EF Core (consola del administrador de paquetes)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 45370a82131da9db8b724fe395d41b1e3641fcf8
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181331"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="6bcbc-102">Referencia de herramientas de Entity Framework Core: consola del administrador de paquetes en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6bcbc-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="6bcbc-103">Las herramientas de la consola del administrador de paquetes (PMC) para Entity Framework Core realizar tareas de desarrollo en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="6bcbc-104">Por ejemplo, se crean [migraciones](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), se aplican migraciones y se genera código para un modelo basado en una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="6bcbc-105">Los comandos se ejecutan dentro de Visual Studio mediante la [consola del administrador de paquetes](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="6bcbc-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="6bcbc-106">Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="6bcbc-107">Si no usa Visual Studio, se recomienda usar en su lugar las [herramientas de línea de comandos de EF Core](dotnet.md) .</span><span class="sxs-lookup"><span data-stu-id="6bcbc-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="6bcbc-108">Las herramientas de la CLI son multiplataforma y se ejecutan en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="6bcbc-109">Instalación de las herramientas</span><span class="sxs-lookup"><span data-stu-id="6bcbc-109">Installing the tools</span></span>

<span data-ttu-id="6bcbc-110">Los procedimientos para instalar y actualizar las herramientas difieren entre ASP.NET Core 2.1 + y versiones anteriores u otros tipos de proyecto.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="6bcbc-111">ASP.NET Core versión 2,1 y versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="6bcbc-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="6bcbc-112">Las herramientas se incluyen automáticamente en un proyecto ASP.NET Core 2.1 + porque el paquete `Microsoft.EntityFrameworkCore.Tools` se incluye en el [metapaquete Microsoft. AspNetCore. app](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6bcbc-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6bcbc-113">Por lo tanto, no tiene que hacer nada para instalar las herramientas, pero tiene que:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="6bcbc-114">Restaure los paquetes antes de usar las herramientas en un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="6bcbc-115">Instale un paquete para actualizar las herramientas a una versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="6bcbc-116">Para asegurarse de que está obteniendo la versión más reciente de las herramientas, se recomienda que realice también el siguiente paso:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="6bcbc-117">Edite el archivo *. csproj* y agregue una línea que especifique la versión más reciente del paquete [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) .</span><span class="sxs-lookup"><span data-stu-id="6bcbc-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="6bcbc-118">Por ejemplo, el archivo *. csproj* podría incluir un `ItemGroup` similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="6bcbc-119">Actualice las herramientas cuando reciba un mensaje similar al del ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="6bcbc-120">La versión de herramientas de EF Core ' 2.1.1-RTM-30846 ' es anterior a la del runtime ' 2.1.3-RTM-32065 '.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="6bcbc-121">Actualice las herramientas para las últimas características y correcciones de errores.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="6bcbc-122">Para actualizar las herramientas:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-122">To update the tools:</span></span>
* <span data-ttu-id="6bcbc-123">Instale la SDK de .NET Core más reciente.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="6bcbc-124">Actualice Visual Studio a la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="6bcbc-125">Edite el archivo *. csproj* para que incluya una referencia de paquete al paquete de herramientas más reciente, como se mostró anteriormente.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="6bcbc-126">Otras versiones y tipos de proyecto</span><span class="sxs-lookup"><span data-stu-id="6bcbc-126">Other versions and project types</span></span>

<span data-ttu-id="6bcbc-127">Instale las herramientas de la consola del administrador de paquetes ejecutando el siguiente comando en la **consola del administrador de paquetes**:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="6bcbc-128">Actualice las herramientas ejecutando el siguiente comando en la **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="6bcbc-129">Comprobar la instalación</span><span class="sxs-lookup"><span data-stu-id="6bcbc-129">Verify the installation</span></span>

<span data-ttu-id="6bcbc-130">Ejecute este comando para comprobar que las herramientas están instaladas:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="6bcbc-131">La salida tiene el siguiente aspecto (no indica qué versión de las herramientas está usando):</span><span class="sxs-lookup"><span data-stu-id="6bcbc-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="6bcbc-132">Uso de las herramientas</span><span class="sxs-lookup"><span data-stu-id="6bcbc-132">Using the tools</span></span>

<span data-ttu-id="6bcbc-133">Antes de usar las herramientas:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-133">Before using the tools:</span></span>
* <span data-ttu-id="6bcbc-134">Comprenda la diferencia entre el proyecto de destino y el de inicio.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="6bcbc-135">Aprenda a usar las herramientas con .NET Standard bibliotecas de clases.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="6bcbc-136">En el caso de los proyectos de ASP.NET Core, establezca el entorno.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="6bcbc-137">Proyecto de destino e inicio</span><span class="sxs-lookup"><span data-stu-id="6bcbc-137">Target and startup project</span></span>

<span data-ttu-id="6bcbc-138">Los comandos hacen referencia a un *proyecto* y un *proyecto de inicio*.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="6bcbc-139">El *proyecto* también se conoce como *proyecto de destino* porque es donde los comandos agregan o quitan archivos.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="6bcbc-140">De forma predeterminada, el proyecto **predeterminado** seleccionado en la **consola del administrador de paquetes** es el proyecto de destino.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="6bcbc-141">Puede especificar otro proyecto como proyecto de destino mediante la opción <nobr>`--project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="6bcbc-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="6bcbc-142">El *proyecto de inicio* es el que las herramientas compilan y ejecutan.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="6bcbc-143">Las herramientas tienen que ejecutar código de aplicación en tiempo de diseño para obtener información sobre el proyecto, como la cadena de conexión a la base de datos y la configuración del modelo.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="6bcbc-144">De forma predeterminada, el **proyecto de inicio** en **Explorador de soluciones** es el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="6bcbc-145">Puede especificar otro proyecto como proyecto de inicio mediante la opción <nobr>`--startup-project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="6bcbc-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="6bcbc-146">El proyecto de inicio y el proyecto de destino suelen ser el mismo proyecto.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="6bcbc-147">Un escenario típico en el que se trata de proyectos independientes es cuando:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="6bcbc-148">El contexto de EF Core y las clases de entidad se encuentran en una biblioteca de clases de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="6bcbc-149">Una aplicación de consola de .NET Core o una aplicación web hace referencia a la biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="6bcbc-150">También es posible [colocar el código de las migraciones en una biblioteca de clases independiente del contexto de EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="6bcbc-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="6bcbc-151">Otras plataformas de destino</span><span class="sxs-lookup"><span data-stu-id="6bcbc-151">Other target frameworks</span></span>

<span data-ttu-id="6bcbc-152">Las herramientas de la consola del administrador de paquetes funcionan con proyectos de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="6bcbc-153">Es posible que las aplicaciones que tienen el modelo de EF Core en una biblioteca de clases .NET Standard no tengan un proyecto de .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="6bcbc-154">Por ejemplo, esto es cierto para las aplicaciones Xamarin y Plataforma universal de Windows.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="6bcbc-155">En tales casos, puede crear un proyecto de aplicación de consola de .NET Core o .NET Framework cuyo único propósito es actuar como proyecto de inicio para las herramientas.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="6bcbc-156">El proyecto puede ser un proyecto ficticio sin código real &mdash; solo es necesario para proporcionar un destino para las herramientas.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="6bcbc-157">¿Por qué es necesario un proyecto ficticio?</span><span class="sxs-lookup"><span data-stu-id="6bcbc-157">Why is a dummy project required?</span></span> <span data-ttu-id="6bcbc-158">Como se mencionó anteriormente, las herramientas tienen que ejecutar código de aplicación en tiempo de diseño.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="6bcbc-159">Para ello, deben usar .NET Core o .NET Framework Runtime.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="6bcbc-160">Cuando el modelo de EF Core está en un proyecto que tiene como destino .NET Core o .NET Framework, las herramientas de EF Core toman prestado el tiempo de ejecución del proyecto.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="6bcbc-161">No pueden hacerlo si el modelo de EF Core está en una biblioteca de clases .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="6bcbc-162">El .NET Standard no es una implementación real de .NET; es una especificación de un conjunto de API que las implementaciones de .NET deben admitir.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="6bcbc-163">Por lo tanto .NET Standard no es suficiente para que las herramientas de EF Core ejecuten código de aplicación.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="6bcbc-164">El proyecto ficticio que cree para usarlo como proyecto de inicio proporciona una plataforma de destino concreta en la que las herramientas pueden cargar la biblioteca de clases de .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="6bcbc-165">Entorno de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6bcbc-165">ASP.NET Core environment</span></span>

<span data-ttu-id="6bcbc-166">Para especificar el entorno de ASP.NET Core proyectos, establezca **env: ASPNETCORE_ENVIRONMENT** antes de ejecutar los comandos.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="6bcbc-167">Parámetros comunes</span><span class="sxs-lookup"><span data-stu-id="6bcbc-167">Common parameters</span></span>

<span data-ttu-id="6bcbc-168">En la tabla siguiente se muestran los parámetros que son comunes a todos los comandos EF Core:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="6bcbc-169">Parámetro</span><span class="sxs-lookup"><span data-stu-id="6bcbc-169">Parameter</span></span>                 | <span data-ttu-id="6bcbc-170">Descripción</span><span class="sxs-lookup"><span data-stu-id="6bcbc-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6bcbc-171">-Context \<String ></span><span class="sxs-lookup"><span data-stu-id="6bcbc-171">-Context \<String></span></span>        | <span data-ttu-id="6bcbc-172">La clase `DbContext` que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-172">The `DbContext` class to use.</span></span> <span data-ttu-id="6bcbc-173">Nombre de clase solo o completo con espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="6bcbc-174">Si se omite este parámetro, EF Core encuentra la clase de contexto.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="6bcbc-175">Si hay varias clases de contexto, este parámetro es obligatorio.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="6bcbc-176">-@No__t de proyecto-0String ></span><span class="sxs-lookup"><span data-stu-id="6bcbc-176">-Project \<String></span></span>        | <span data-ttu-id="6bcbc-177">Proyecto de destino.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-177">The target project.</span></span> <span data-ttu-id="6bcbc-178">Si se omite este parámetro, el **proyecto predeterminado** de la **consola del administrador de paquetes** se utiliza como proyecto de destino.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="6bcbc-179">-Proyecto \<String ></span><span class="sxs-lookup"><span data-stu-id="6bcbc-179">-StartupProject \<String></span></span> | <span data-ttu-id="6bcbc-180">Proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-180">The startup project.</span></span> <span data-ttu-id="6bcbc-181">Si se omite este parámetro, el **proyecto de inicio** de **las propiedades** de la solución se usa como proyecto de destino.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="6bcbc-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="6bcbc-182">-Verbose</span></span>                  | <span data-ttu-id="6bcbc-183">Mostrar resultado detallado.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="6bcbc-184">Para mostrar información de ayuda sobre un comando, use el comando `Get-Help` de PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="6bcbc-185">Los parámetros context, Project y proyecto admiten la expansión de pestañas.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="6bcbc-186">Agregar-migración</span><span class="sxs-lookup"><span data-stu-id="6bcbc-186">Add-Migration</span></span>

<span data-ttu-id="6bcbc-187">Agrega una nueva migración.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-187">Adds a new migration.</span></span>

<span data-ttu-id="6bcbc-188">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-188">Parameters:</span></span>

| <span data-ttu-id="6bcbc-189">Parámetro</span><span class="sxs-lookup"><span data-stu-id="6bcbc-189">Parameter</span></span>                         | <span data-ttu-id="6bcbc-190">Descripción</span><span class="sxs-lookup"><span data-stu-id="6bcbc-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6bcbc-191">@no__t -0 \<String > <nobr></span><span class="sxs-lookup"><span data-stu-id="6bcbc-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="6bcbc-192">El nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-192">The name of the migration.</span></span> <span data-ttu-id="6bcbc-193">Este es un parámetro posicional y es obligatorio.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="6bcbc-194"><nobr>-OutputDir \<String ></nobr></span><span class="sxs-lookup"><span data-stu-id="6bcbc-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="6bcbc-195">Directorio (y subespacio de nombres) que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="6bcbc-196">Las rutas de acceso son relativas al directorio del proyecto de destino.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="6bcbc-197">El valor predeterminado es "migraciones".</span><span class="sxs-lookup"><span data-stu-id="6bcbc-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="6bcbc-198">Drop-Database</span><span class="sxs-lookup"><span data-stu-id="6bcbc-198">Drop-Database</span></span>

<span data-ttu-id="6bcbc-199">Quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-199">Drops the database.</span></span>

<span data-ttu-id="6bcbc-200">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-200">Parameters:</span></span>

| <span data-ttu-id="6bcbc-201">Parámetro</span><span class="sxs-lookup"><span data-stu-id="6bcbc-201">Parameter</span></span> | <span data-ttu-id="6bcbc-202">Descripción</span><span class="sxs-lookup"><span data-stu-id="6bcbc-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="6bcbc-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="6bcbc-203">-WhatIf</span></span>   | <span data-ttu-id="6bcbc-204">Mostrar la base de datos que se va a quitar, pero no quitarla.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="6bcbc-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="6bcbc-205">Get-DbContext</span></span>

<span data-ttu-id="6bcbc-206">Obtiene información sobre un tipo `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="6bcbc-207">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="6bcbc-207">Remove-Migration</span></span>

<span data-ttu-id="6bcbc-208">Quita la última migración (revierte los cambios de código que se realizaron para la migración).</span><span class="sxs-lookup"><span data-stu-id="6bcbc-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="6bcbc-209">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-209">Parameters:</span></span>

| <span data-ttu-id="6bcbc-210">Parámetro</span><span class="sxs-lookup"><span data-stu-id="6bcbc-210">Parameter</span></span> | <span data-ttu-id="6bcbc-211">Descripción</span><span class="sxs-lookup"><span data-stu-id="6bcbc-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="6bcbc-212">-Force</span><span class="sxs-lookup"><span data-stu-id="6bcbc-212">-Force</span></span>    | <span data-ttu-id="6bcbc-213">Revertir la migración (revertir los cambios que se aplicaron a la base de datos).</span><span class="sxs-lookup"><span data-stu-id="6bcbc-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="6bcbc-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="6bcbc-214">Scaffold-DbContext</span></span>

<span data-ttu-id="6bcbc-215">Genera código para un `DbContext` y tipos de entidad para una base de datos.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="6bcbc-216">Para que `Scaffold-DbContext` genere un tipo de entidad, la tabla de base de datos debe tener una clave principal.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="6bcbc-217">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-217">Parameters:</span></span>

| <span data-ttu-id="6bcbc-218">Parámetro</span><span class="sxs-lookup"><span data-stu-id="6bcbc-218">Parameter</span></span>                          | <span data-ttu-id="6bcbc-219">Descripción</span><span class="sxs-lookup"><span data-stu-id="6bcbc-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6bcbc-220"><nobr>-@No__t de conexión-1String ></nobr></span><span class="sxs-lookup"><span data-stu-id="6bcbc-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="6bcbc-221">Cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-221">The connection string to the database.</span></span> <span data-ttu-id="6bcbc-222">En el caso de los proyectos de ASP.NET Core 2. x, el valor puede ser *Name = \<name de la cadena de conexión >* .</span><span class="sxs-lookup"><span data-stu-id="6bcbc-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="6bcbc-223">En ese caso, el nombre procede de los orígenes de configuración que se configuran para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="6bcbc-224">Este es un parámetro posicional y es obligatorio.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="6bcbc-225"><nobr>-Proveedor \<String ></nobr></span><span class="sxs-lookup"><span data-stu-id="6bcbc-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="6bcbc-226">Proveedor que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-226">The provider to use.</span></span> <span data-ttu-id="6bcbc-227">Normalmente, es el nombre del paquete de NuGet, por ejemplo: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="6bcbc-228">Este es un parámetro posicional y es obligatorio.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="6bcbc-229">-OutputDir \<String ></span><span class="sxs-lookup"><span data-stu-id="6bcbc-229">-OutputDir \<String></span></span>               | <span data-ttu-id="6bcbc-230">Directorio en el que se colocarán los archivos.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-230">The directory to put files in.</span></span> <span data-ttu-id="6bcbc-231">Las rutas de acceso son relativas al directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="6bcbc-232">-ContextDir \<String ></span><span class="sxs-lookup"><span data-stu-id="6bcbc-232">-ContextDir \<String></span></span>              | <span data-ttu-id="6bcbc-233">Directorio en el que se va a colocar el archivo `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="6bcbc-234">Las rutas de acceso son relativas al directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="6bcbc-235">-Context \<String ></span><span class="sxs-lookup"><span data-stu-id="6bcbc-235">-Context \<String></span></span>                 | <span data-ttu-id="6bcbc-236">Nombre de la clase `DbContext` que se va a generar.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="6bcbc-237">-Schemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="6bcbc-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="6bcbc-238">Esquemas de las tablas para las que se van a generar tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="6bcbc-239">Si se omite este parámetro, se incluyen todos los esquemas.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="6bcbc-240">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="6bcbc-240">-Tables \<String[]></span></span>                | <span data-ttu-id="6bcbc-241">Tablas para las que se van a generar tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-241">The tables to generate entity types for.</span></span> <span data-ttu-id="6bcbc-242">Si se omite este parámetro, se incluyen todas las tablas.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="6bcbc-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="6bcbc-243">-DataAnnotations</span></span>                   | <span data-ttu-id="6bcbc-244">Use los atributos para configurar el modelo (siempre que sea posible).</span><span class="sxs-lookup"><span data-stu-id="6bcbc-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="6bcbc-245">Si se omite este parámetro, solo se usa la API fluida.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="6bcbc-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="6bcbc-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="6bcbc-247">Utilice nombres de tabla y columna exactamente como aparecen en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="6bcbc-248">Si se omite este parámetro, los nombres de base de datos se cambian para C# ajustarse mejor a las convenciones de estilo de nombre.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="6bcbc-249">-Force</span><span class="sxs-lookup"><span data-stu-id="6bcbc-249">-Force</span></span>                             | <span data-ttu-id="6bcbc-250">Sobrescribe los archivos existentes.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="6bcbc-251">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="6bcbc-252">Ejemplo que scaffolding solo selecciona tablas y crea el contexto en una carpeta independiente con un nombre especificado:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="6bcbc-253">Script: migración</span><span class="sxs-lookup"><span data-stu-id="6bcbc-253">Script-Migration</span></span>

<span data-ttu-id="6bcbc-254">Genera un script SQL que aplica todos los cambios de una migración seleccionada a otra migración seleccionada.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="6bcbc-255">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-255">Parameters:</span></span>

| <span data-ttu-id="6bcbc-256">Parámetro</span><span class="sxs-lookup"><span data-stu-id="6bcbc-256">Parameter</span></span>                | <span data-ttu-id="6bcbc-257">Descripción</span><span class="sxs-lookup"><span data-stu-id="6bcbc-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6bcbc-258">*-Desde* \<String ></span><span class="sxs-lookup"><span data-stu-id="6bcbc-258">*-From* \<String></span></span>        | <span data-ttu-id="6bcbc-259">La migración inicial.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-259">The starting migration.</span></span> <span data-ttu-id="6bcbc-260">Las migraciones pueden identificarse por nombre o por identificador.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="6bcbc-261">El número 0 es un caso especial que significa *antes de la primera migración*.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="6bcbc-262">El valor predeterminado es 0.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="6bcbc-263">*-To* \<String ></span><span class="sxs-lookup"><span data-stu-id="6bcbc-263">*-To* \<String></span></span>          | <span data-ttu-id="6bcbc-264">La migración final.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-264">The ending migration.</span></span> <span data-ttu-id="6bcbc-265">Tiene como valor predeterminado la última migración.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="6bcbc-266"><nobr>-Idempotent</nobr></span><span class="sxs-lookup"><span data-stu-id="6bcbc-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="6bcbc-267">Generar un script que se puede usar en una base de datos en cualquier migración.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="6bcbc-268">-@No__t de salida-0String ></span><span class="sxs-lookup"><span data-stu-id="6bcbc-268">-Output \<String></span></span>        | <span data-ttu-id="6bcbc-269">Archivo en el que se va a escribir el resultado.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-269">The file to write the result to.</span></span> <span data-ttu-id="6bcbc-270">Si se omite este parámetro, el archivo se crea con un nombre generado en la misma carpeta en que se crean los archivos en tiempo de ejecución de la aplicación, por ejemplo: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/* .</span><span class="sxs-lookup"><span data-stu-id="6bcbc-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="6bcbc-271">Los parámetros para, de y de salida admiten la expansión de pestañas.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="6bcbc-272">En el ejemplo siguiente se crea un script para la migración de InitialCreate con el nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="6bcbc-273">En el ejemplo siguiente se crea un script para todas las migraciones después de la migración de InitialCreate con el identificador de migración.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="6bcbc-274">Actualizar base de datos</span><span class="sxs-lookup"><span data-stu-id="6bcbc-274">Update-Database</span></span>

<span data-ttu-id="6bcbc-275">Actualiza la base de datos a la última migración o a una migración especificada.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="6bcbc-276">Parámetro</span><span class="sxs-lookup"><span data-stu-id="6bcbc-276">Parameter</span></span>                           | <span data-ttu-id="6bcbc-277">Descripción</span><span class="sxs-lookup"><span data-stu-id="6bcbc-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6bcbc-278"><nobr> *-* @No__t de migración-2String ></nobr></span><span class="sxs-lookup"><span data-stu-id="6bcbc-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="6bcbc-279">La migración de destino.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-279">The target migration.</span></span> <span data-ttu-id="6bcbc-280">Las migraciones pueden identificarse por nombre o por identificador.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="6bcbc-281">El número 0 es un caso especial que significa *antes de la primera migración* y hace que se reviertan todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="6bcbc-282">Si no se especifica ninguna migración, el comando toma como valor predeterminado la última migración.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="6bcbc-283">El parámetro Migration admite la expansión de pestañas.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="6bcbc-284">En el ejemplo siguiente se revierten todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="6bcbc-285">En los siguientes ejemplos se actualiza la base de datos a una migración especificada.</span><span class="sxs-lookup"><span data-stu-id="6bcbc-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="6bcbc-286">El primero usa el nombre de la migración y el segundo usa el identificador de migración:</span><span class="sxs-lookup"><span data-stu-id="6bcbc-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="6bcbc-287">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6bcbc-287">Additional resources</span></span>

* [<span data-ttu-id="6bcbc-288">Migraciones</span><span class="sxs-lookup"><span data-stu-id="6bcbc-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="6bcbc-289">Ingeniería inversa</span><span class="sxs-lookup"><span data-stu-id="6bcbc-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
