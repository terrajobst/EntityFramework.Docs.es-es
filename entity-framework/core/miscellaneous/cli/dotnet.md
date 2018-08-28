---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995302"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="3cf2a-102">Herramientas de línea de comandos de EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="3cf2a-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="3cf2a-103">Las herramientas de línea de comandos de Entity Framework Core .NET son una extensión multiplataforma **dotnet** comando, que forma parte de la [SDK de .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="3cf2a-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="3cf2a-104">Si usa Visual Studio, se recomienda [las herramientas de PMC] [ 1] en su lugar ya que proporcionan una experiencia más integrada.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="3cf2a-105">Instalación de las herramientas</span><span class="sxs-lookup"><span data-stu-id="3cf2a-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="3cf2a-106">El SDK de .NET Core 2.1.300 versión más reciente incluye **dotnet ef** comandos que son compatibles con EF Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="3cf2a-107">Por lo tanto, si usa las versiones recientes de .NET Core SDK y el tiempo de ejecución de EF Core, se requiere ninguna instalación y puede omitir el resto de esta sección.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="3cf2a-108">Por otro lado, el **dotnet ef** herramienta contenida en la versión del SDK de .NET Core 2.1.300 y versiones más reciente no es compatible con la versión de EF Core 1.0 y 1.1.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="3cf2a-109">Antes de que puede trabajar con un proyecto que usa estas versiones anteriores de EF Core en un equipo que tiene el SDK de .NET Core 2.1.300 o posterior, instalado, también debe instalar la versión 2.1.200 o una versión anterior del SDK y configurar la aplicación para que use esa versión anterior mediante la modificación de sus  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) archivo.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="3cf2a-110">Este archivo normalmente se incluye en el directorio de la solución (uno por encima del proyecto).</span><span class="sxs-lookup"><span data-stu-id="3cf2a-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="3cf2a-111">A continuación, puede continuar con las instrucciones de instalación siguientes.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="3cf2a-112">Las versiones anteriores del SDK de .NET Core, puede instalar las herramientas de línea de comandos de .NET de EF Core siguiendo estos pasos:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="3cf2a-113">Edite el archivo de proyecto y agregue Microsoft.EntityFrameworkCore.Tools.DotNet como un elemento DotNetCliToolReference (ver abajo)</span><span class="sxs-lookup"><span data-stu-id="3cf2a-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="3cf2a-114">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="3cf2a-115">El proyecto resultante debe tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-115">The resulting project should look something like this:</span></span>

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> <span data-ttu-id="3cf2a-116">Una referencia de paquete con `PrivateAssets="All"` significa que no se expone a los proyectos que hacen referencia a este proyecto, que es especialmente útil para los paquetes que normalmente se usan solo durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="3cf2a-117">Si hizo todo correctamente, debería poder ejecutar correctamente el siguiente comando en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="3cf2a-118">Uso de las herramientas</span><span class="sxs-lookup"><span data-stu-id="3cf2a-118">Using the tools</span></span>
---------------
<span data-ttu-id="3cf2a-119">Cada vez que se invoca un comando, intervienen dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="3cf2a-120">El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).</span><span class="sxs-lookup"><span data-stu-id="3cf2a-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="3cf2a-121">El proyecto de destino tiene como valor predeterminado para el proyecto en el directorio actual, pero se puede cambiar mediante la <nobr> **--proyecto** </nobr> opción.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="3cf2a-122">El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="3cf2a-123">También tiene como valor predeterminado para el proyecto en el directorio actual, pero se puede cambiar mediante la **: proyecto de inicio** opción.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="3cf2a-124">Por ejemplo, actualizando la base de datos de la aplicación web que tenga instalado en un proyecto diferente de EF Core tendría este aspecto: `dotnet ef database update --project {project-path}` (desde el directorio de aplicación web)</span><span class="sxs-lookup"><span data-stu-id="3cf2a-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="3cf2a-125">Opciones comunes:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="3cf2a-126">--json</span><span class="sxs-lookup"><span data-stu-id="3cf2a-126">--json</span></span>                           | <span data-ttu-id="3cf2a-127">Mostrar la salida JSON.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-127">Show JSON output.</span></span>           |
| <span data-ttu-id="3cf2a-128">-c</span><span class="sxs-lookup"><span data-stu-id="3cf2a-128">-c</span></span> | <span data-ttu-id="3cf2a-129">--contexto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="3cf2a-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="3cf2a-130">La clase DbContext usar.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="3cf2a-131">-p</span><span class="sxs-lookup"><span data-stu-id="3cf2a-131">-p</span></span> | <span data-ttu-id="3cf2a-132">--proyecto \<proyecto ></span><span class="sxs-lookup"><span data-stu-id="3cf2a-132">--project \<PROJECT></span></span>             | <span data-ttu-id="3cf2a-133">El proyecto para usar.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-133">The project to use.</span></span>         |
| <span data-ttu-id="3cf2a-134">-s</span><span class="sxs-lookup"><span data-stu-id="3cf2a-134">-s</span></span> | <span data-ttu-id="3cf2a-135">--startup-project \<proyecto ></span><span class="sxs-lookup"><span data-stu-id="3cf2a-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="3cf2a-136">Para usar el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="3cf2a-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="3cf2a-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="3cf2a-138">La plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-138">The target framework.</span></span>       |
|    | <span data-ttu-id="3cf2a-139">--configuración \<configuración ></span><span class="sxs-lookup"><span data-stu-id="3cf2a-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="3cf2a-140">La configuración que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="3cf2a-141">--en tiempo de ejecución \<identificador ></span><span class="sxs-lookup"><span data-stu-id="3cf2a-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="3cf2a-142">El runtime que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-142">The runtime to use.</span></span>         |
| <span data-ttu-id="3cf2a-143">-h</span><span class="sxs-lookup"><span data-stu-id="3cf2a-143">-h</span></span> | <span data-ttu-id="3cf2a-144">--Ayuda</span><span class="sxs-lookup"><span data-stu-id="3cf2a-144">--help</span></span>                           | <span data-ttu-id="3cf2a-145">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-145">Show help information.</span></span>      |
| <span data-ttu-id="3cf2a-146">-v</span><span class="sxs-lookup"><span data-stu-id="3cf2a-146">-v</span></span> | <span data-ttu-id="3cf2a-147">--verbose</span><span class="sxs-lookup"><span data-stu-id="3cf2a-147">--verbose</span></span>                        | <span data-ttu-id="3cf2a-148">Mostrar resultados detallados.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="3cf2a-149">--Sin color</span><span class="sxs-lookup"><span data-stu-id="3cf2a-149">--no-color</span></span>                       | <span data-ttu-id="3cf2a-150">No colorear salida.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="3cf2a-151">--salida de prefijo</span><span class="sxs-lookup"><span data-stu-id="3cf2a-151">--prefix-output</span></span>                  | <span data-ttu-id="3cf2a-152">Prefijo con el nivel de salida.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="3cf2a-153">Para especificar el entorno de ASP.NET Core, establezca el **ASPNETCORE_ENVIRONMENT** variable de entorno antes de ejecutar.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="3cf2a-154">Comandos</span><span class="sxs-lookup"><span data-stu-id="3cf2a-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="3cf2a-155">colocación de base de datos de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="3cf2a-155">dotnet ef database drop</span></span>

<span data-ttu-id="3cf2a-156">Quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-156">Drops the database.</span></span>

<span data-ttu-id="3cf2a-157">Opciones:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="3cf2a-158">-f</span><span class="sxs-lookup"><span data-stu-id="3cf2a-158">-f</span></span> | <span data-ttu-id="3cf2a-159">--forzar</span><span class="sxs-lookup"><span data-stu-id="3cf2a-159">--force</span></span>   | <span data-ttu-id="3cf2a-160">No se confirme.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="3cf2a-161">--dry-run</span><span class="sxs-lookup"><span data-stu-id="3cf2a-161">--dry-run</span></span> | <span data-ttu-id="3cf2a-162">Mostrar qué base de datos se quitarían, pero no colóquelo.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="3cf2a-163">actualización de base de datos de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="3cf2a-163">dotnet ef database update</span></span>

<span data-ttu-id="3cf2a-164">Actualiza la base de datos para una migración especificada.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="3cf2a-165">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="3cf2a-166">\<MIGRACIÓN &GT;</span><span class="sxs-lookup"><span data-stu-id="3cf2a-166">\<MIGRATION></span></span> | <span data-ttu-id="3cf2a-167">La migración de destino.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-167">The target migration.</span></span> <span data-ttu-id="3cf2a-168">Si es 0, se revertirán todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="3cf2a-169">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="3cf2a-170">información de dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="3cf2a-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="3cf2a-171">Obtiene información sobre un tipo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="3cf2a-172">lista de dbcontext de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="3cf2a-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="3cf2a-173">Enumera los tipos de DbContext disponibles.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="3cf2a-174">scaffolding de dbcontext de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="3cf2a-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="3cf2a-175">Aplica la técnica scaffolding un tipos de entidad y DbContext para una base de datos.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="3cf2a-176">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="3cf2a-177">\<CONEXIÓN &GT;</span><span class="sxs-lookup"><span data-stu-id="3cf2a-177">\<CONNECTION></span></span> | <span data-ttu-id="3cf2a-178">La cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="3cf2a-179">\<PROVEEDOR &GT;</span><span class="sxs-lookup"><span data-stu-id="3cf2a-179">\<PROVIDER></span></span>   | <span data-ttu-id="3cf2a-180">El proveedor debe usar.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-180">The provider to use.</span></span> <span data-ttu-id="3cf2a-181">(por ejemplo, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="3cf2a-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="3cf2a-182">Opciones:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3cf2a-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="3cf2a-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="3cf2a-184">--anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="3cf2a-184">--data-annotations</span></span>                      | <span data-ttu-id="3cf2a-185">Usar atributos para configurar el modelo (donde sea posible).</span><span class="sxs-lookup"><span data-stu-id="3cf2a-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="3cf2a-186">Si se omite, se usa solo la API fluida.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="3cf2a-187">-c</span><span class="sxs-lookup"><span data-stu-id="3cf2a-187">-c</span></span>              | <span data-ttu-id="3cf2a-188">--contexto \<nombre ></span><span class="sxs-lookup"><span data-stu-id="3cf2a-188">--context \<NAME></span></span>                       | <span data-ttu-id="3cf2a-189">El nombre de la clase DbContext.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="3cf2a-190">--contexto-dir \<ruta de acceso ></span><span class="sxs-lookup"><span data-stu-id="3cf2a-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="3cf2a-191">Colocar el archivo de DbContext en el directorio.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="3cf2a-192">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="3cf2a-193">-f</span><span class="sxs-lookup"><span data-stu-id="3cf2a-193">-f</span></span>              | <span data-ttu-id="3cf2a-194">--forzar</span><span class="sxs-lookup"><span data-stu-id="3cf2a-194">--force</span></span>                                 | <span data-ttu-id="3cf2a-195">Sobrescribir archivos existentes.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="3cf2a-196">-o</span><span class="sxs-lookup"><span data-stu-id="3cf2a-196">-o</span></span>              | <span data-ttu-id="3cf2a-197">--salida-dir \<ruta de acceso ></span><span class="sxs-lookup"><span data-stu-id="3cf2a-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="3cf2a-198">Para poner los archivos en el directorio.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-198">The directory to put files in.</span></span> <span data-ttu-id="3cf2a-199">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="3cf2a-200"><nobr>--esquema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="3cf2a-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="3cf2a-201">Los esquemas de tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="3cf2a-202">-t</span><span class="sxs-lookup"><span data-stu-id="3cf2a-202">-t</span></span>              | <span data-ttu-id="3cf2a-203">--tabla \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="3cf2a-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="3cf2a-204">Para generar tipos de entidad para las tablas.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="3cf2a-205">--utilizar nombres de base de datos</span><span class="sxs-lookup"><span data-stu-id="3cf2a-205">--use-database-names</span></span>                    | <span data-ttu-id="3cf2a-206">Usar nombres de tabla y columna directamente desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="3cf2a-207">Agregar migraciones de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="3cf2a-207">dotnet ef migrations add</span></span>

<span data-ttu-id="3cf2a-208">Agrega una nueva migración.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-208">Adds a new migration.</span></span>

<span data-ttu-id="3cf2a-209">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="3cf2a-210">\<NOMBRE &GT;</span><span class="sxs-lookup"><span data-stu-id="3cf2a-210">\<NAME></span></span> | <span data-ttu-id="3cf2a-211">El nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-211">The name of the migration.</span></span> |

<span data-ttu-id="3cf2a-212">Opciones:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="3cf2a-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="3cf2a-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="3cf2a-214"><nobr>--salida-dir \<ruta de acceso ></nobr></span><span class="sxs-lookup"><span data-stu-id="3cf2a-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="3cf2a-215">El directorio (y subespacio de nombres) para usar.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="3cf2a-216">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="3cf2a-217">El valor predeterminado es "Migraciones".</span><span class="sxs-lookup"><span data-stu-id="3cf2a-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="3cf2a-218">lista de las migraciones de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="3cf2a-218">dotnet ef migrations list</span></span>

<span data-ttu-id="3cf2a-219">Migraciones de listas disponibles.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="3cf2a-220">quitar las migraciones de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="3cf2a-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="3cf2a-221">Quita la última migración.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-221">Removes the last migration.</span></span>

<span data-ttu-id="3cf2a-222">Opciones:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="3cf2a-223">-f</span><span class="sxs-lookup"><span data-stu-id="3cf2a-223">-f</span></span> | <span data-ttu-id="3cf2a-224">--forzar</span><span class="sxs-lookup"><span data-stu-id="3cf2a-224">--force</span></span> | <span data-ttu-id="3cf2a-225">Revertir la migración si se ha aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="3cf2a-226">secuencia de comandos de dotnet ef migrations</span><span class="sxs-lookup"><span data-stu-id="3cf2a-226">dotnet ef migrations script</span></span>

<span data-ttu-id="3cf2a-227">Genera un script SQL de migraciones.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="3cf2a-228">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="3cf2a-229">\<DESDE &GT;</span><span class="sxs-lookup"><span data-stu-id="3cf2a-229">\<FROM></span></span> | <span data-ttu-id="3cf2a-230">La migración inicial.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-230">The starting migration.</span></span> <span data-ttu-id="3cf2a-231">El valor predeterminado es 0 (la base de datos inicial).</span><span class="sxs-lookup"><span data-stu-id="3cf2a-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="3cf2a-232">\<PARA &GT;</span><span class="sxs-lookup"><span data-stu-id="3cf2a-232">\<TO></span></span>   | <span data-ttu-id="3cf2a-233">La migración final.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-233">The ending migration.</span></span> <span data-ttu-id="3cf2a-234">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="3cf2a-235">Opciones:</span><span class="sxs-lookup"><span data-stu-id="3cf2a-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="3cf2a-236">-o</span><span class="sxs-lookup"><span data-stu-id="3cf2a-236">-o</span></span> | <span data-ttu-id="3cf2a-237">--salida \<archivo ></span><span class="sxs-lookup"><span data-stu-id="3cf2a-237">--output \<FILE></span></span> | <span data-ttu-id="3cf2a-238">Para escribir el resultado en el archivo.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="3cf2a-239">-i</span><span class="sxs-lookup"><span data-stu-id="3cf2a-239">-i</span></span> | <span data-ttu-id="3cf2a-240">--idempotente</span><span class="sxs-lookup"><span data-stu-id="3cf2a-240">--idempotent</span></span>     | <span data-ttu-id="3cf2a-241">Generar un script que se puede usar en una base de datos en cualquier migración.</span><span class="sxs-lookup"><span data-stu-id="3cf2a-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
