---
title: .NET core CLI - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754501"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="a37b1-102">Herramientas de línea de comandos de EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="a37b1-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="a37b1-103">Las herramientas de línea de comandos de .NET de Entity Framework Core son una extensión a la plataforma entre **dotnet** comando, que forma parte de la [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="a37b1-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="a37b1-104">Si está utilizando Visual Studio, se recomienda [las herramientas PMC] [ 1] en su lugar, ya que proporcionan una experiencia más integrada.</span><span class="sxs-lookup"><span data-stu-id="a37b1-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="a37b1-105">Instalación de las herramientas</span><span class="sxs-lookup"><span data-stu-id="a37b1-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="a37b1-106">.NET Core SDK versión 2.1.300 e incluye más reciente **dotnet ef** comandos que son compatibles con EF Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="a37b1-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="a37b1-107">Por lo tanto, si está utilizando las versiones recientes de .NET Core SDK y el tiempo de ejecución de EF Core, se requiere ninguna instalación y puede omitir el resto de esta sección.</span><span class="sxs-lookup"><span data-stu-id="a37b1-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="a37b1-108">Por otro lado, el **dotnet ef** herramienta independiente en la versión de .NET Core SDK 2.1.300 y versiones más reciente no es compatible con la versión de EF Core 1.0 y 1.1.</span><span class="sxs-lookup"><span data-stu-id="a37b1-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="a37b1-109">Antes de que puede trabajar con un proyecto que usa estas versiones anteriores de EF principal en un equipo que tenga .NET Core SDK 2.1.300 o posterior instalado, también debe instalar la versión 2.1.200 o anterior del SDK y configurar la aplicación para utilizar esa versión anterior mediante la modificación de su  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) archivo.</span><span class="sxs-lookup"><span data-stu-id="a37b1-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="a37b1-110">Este archivo normalmente se incluye en el directorio de la solución (uno por encima del proyecto).</span><span class="sxs-lookup"><span data-stu-id="a37b1-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="a37b1-111">A continuación, puede continuar con la siguiente instrucción de instalación.</span><span class="sxs-lookup"><span data-stu-id="a37b1-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="a37b1-112">Para versiones anteriores de .NET Core SDK, puede instalar las herramientas de línea de comandos de .NET de EF Core siguiendo estos pasos:</span><span class="sxs-lookup"><span data-stu-id="a37b1-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="a37b1-113">Edite el archivo de proyecto y agregar Microsoft.EntityFrameworkCore.Tools.DotNet como un elemento de DotNetCliToolReference (ver abajo)</span><span class="sxs-lookup"><span data-stu-id="a37b1-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="a37b1-114">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="a37b1-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="a37b1-115">El proyecto resultante debe tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a37b1-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="a37b1-116">Una referencia de paquete con `PrivateAssets="All"` significa que no está expuesto a los proyectos que hacen referencia a este proyecto, que es especialmente útil para los paquetes que normalmente se utilizan solo durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="a37b1-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="a37b1-117">Si lo quitó correctamente todos los pasos correctamente, debe poder ejecutar correctamente el comando siguiente en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="a37b1-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="a37b1-118">Uso de las herramientas</span><span class="sxs-lookup"><span data-stu-id="a37b1-118">Using the tools</span></span>
---------------
<span data-ttu-id="a37b1-119">Cada vez que se invoca un comando, existen dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="a37b1-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="a37b1-120">El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).</span><span class="sxs-lookup"><span data-stu-id="a37b1-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="a37b1-121">El proyecto de destino tiene como valor predeterminado para el proyecto en el directorio actual, pero se puede cambiar mediante la <nobr> **--proyecto** </nobr> opción.</span><span class="sxs-lookup"><span data-stu-id="a37b1-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="a37b1-122">El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a37b1-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="a37b1-123">También tiene como valor predeterminado para el proyecto en el directorio actual pero se puede cambiar mediante la **: proyecto de inicio** opción.</span><span class="sxs-lookup"><span data-stu-id="a37b1-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="a37b1-124">Por ejemplo, actualizar la base de datos de la aplicación web que tiene instalado en un proyecto diferente de núcleo de EF sería similar al siguiente: `dotnet ef database update --project {project-path}` (desde el directorio de aplicación web)</span><span class="sxs-lookup"><span data-stu-id="a37b1-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="a37b1-125">Opciones comunes:</span><span class="sxs-lookup"><span data-stu-id="a37b1-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="a37b1-126">--json</span><span class="sxs-lookup"><span data-stu-id="a37b1-126">--json</span></span>                           | <span data-ttu-id="a37b1-127">Mostrar la salida JSON.</span><span class="sxs-lookup"><span data-stu-id="a37b1-127">Show JSON output.</span></span>           |
| <span data-ttu-id="a37b1-128">-c</span><span class="sxs-lookup"><span data-stu-id="a37b1-128">-c</span></span> | <span data-ttu-id="a37b1-129">--contexto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="a37b1-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="a37b1-130">DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="a37b1-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="a37b1-131">-p</span><span class="sxs-lookup"><span data-stu-id="a37b1-131">-p</span></span> | <span data-ttu-id="a37b1-132">--proyecto \<proyecto ></span><span class="sxs-lookup"><span data-stu-id="a37b1-132">--project \<PROJECT></span></span>             | <span data-ttu-id="a37b1-133">El proyecto debe usar.</span><span class="sxs-lookup"><span data-stu-id="a37b1-133">The project to use.</span></span>         |
| <span data-ttu-id="a37b1-134">-s</span><span class="sxs-lookup"><span data-stu-id="a37b1-134">-s</span></span> | <span data-ttu-id="a37b1-135">--proyecto de inicio \<proyecto ></span><span class="sxs-lookup"><span data-stu-id="a37b1-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="a37b1-136">Proyecto de inicio que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="a37b1-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="a37b1-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="a37b1-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="a37b1-138">El marco de destino.</span><span class="sxs-lookup"><span data-stu-id="a37b1-138">The target framework.</span></span>       |
|    | <span data-ttu-id="a37b1-139">--configuración \<configuración ></span><span class="sxs-lookup"><span data-stu-id="a37b1-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="a37b1-140">La configuración que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="a37b1-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="a37b1-141">--en tiempo de ejecución \<identificador ></span><span class="sxs-lookup"><span data-stu-id="a37b1-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="a37b1-142">El runtime que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="a37b1-142">The runtime to use.</span></span>         |
| <span data-ttu-id="a37b1-143">-h</span><span class="sxs-lookup"><span data-stu-id="a37b1-143">-h</span></span> | <span data-ttu-id="a37b1-144">--Ayuda</span><span class="sxs-lookup"><span data-stu-id="a37b1-144">--help</span></span>                           | <span data-ttu-id="a37b1-145">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="a37b1-145">Show help information.</span></span>      |
| <span data-ttu-id="a37b1-146">-v</span><span class="sxs-lookup"><span data-stu-id="a37b1-146">-v</span></span> | <span data-ttu-id="a37b1-147">--detallado</span><span class="sxs-lookup"><span data-stu-id="a37b1-147">--verbose</span></span>                        | <span data-ttu-id="a37b1-148">Mostrar resultados detallados.</span><span class="sxs-lookup"><span data-stu-id="a37b1-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="a37b1-149">--Sin color</span><span class="sxs-lookup"><span data-stu-id="a37b1-149">--no-color</span></span>                       | <span data-ttu-id="a37b1-150">No colorear la salida.</span><span class="sxs-lookup"><span data-stu-id="a37b1-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="a37b1-151">--salida de prefijo</span><span class="sxs-lookup"><span data-stu-id="a37b1-151">--prefix-output</span></span>                  | <span data-ttu-id="a37b1-152">Prefijo con el nivel de salida.</span><span class="sxs-lookup"><span data-stu-id="a37b1-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="a37b1-153">Para especificar el entorno de ASP.NET Core, establezca el **ASPNETCORE_ENVIRONMENT** variable de entorno antes de ejecutar.</span><span class="sxs-lookup"><span data-stu-id="a37b1-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="a37b1-154">Comandos</span><span class="sxs-lookup"><span data-stu-id="a37b1-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="a37b1-155">eliminación de base de datos de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="a37b1-155">dotnet ef database drop</span></span>

<span data-ttu-id="a37b1-156">Quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a37b1-156">Drops the database.</span></span>

<span data-ttu-id="a37b1-157">Opciones:</span><span class="sxs-lookup"><span data-stu-id="a37b1-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="a37b1-158">-f</span><span class="sxs-lookup"><span data-stu-id="a37b1-158">-f</span></span> | <span data-ttu-id="a37b1-159">--forzar</span><span class="sxs-lookup"><span data-stu-id="a37b1-159">--force</span></span>   | <span data-ttu-id="a37b1-160">No realiza esta confirmación.</span><span class="sxs-lookup"><span data-stu-id="a37b1-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="a37b1-161">---simulacro</span><span class="sxs-lookup"><span data-stu-id="a37b1-161">--dry-run</span></span> | <span data-ttu-id="a37b1-162">Mostrar qué base de datos se quitarían, pero no se quita.</span><span class="sxs-lookup"><span data-stu-id="a37b1-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="a37b1-163">actualización de base de datos de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="a37b1-163">dotnet ef database update</span></span>

<span data-ttu-id="a37b1-164">Actualiza la base de datos para una migración especificada.</span><span class="sxs-lookup"><span data-stu-id="a37b1-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="a37b1-165">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="a37b1-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="a37b1-166">\<MIGRACIÓN &GT;</span><span class="sxs-lookup"><span data-stu-id="a37b1-166">\<MIGRATION></span></span> | <span data-ttu-id="a37b1-167">La migración de destino.</span><span class="sxs-lookup"><span data-stu-id="a37b1-167">The target migration.</span></span> <span data-ttu-id="a37b1-168">Si es 0, se revertirán todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="a37b1-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="a37b1-169">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="a37b1-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="a37b1-170">información de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="a37b1-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="a37b1-171">Obtiene información sobre un tipo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="a37b1-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="a37b1-172">lista de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="a37b1-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="a37b1-173">Enumera los tipos de DbContext disponibles.</span><span class="sxs-lookup"><span data-stu-id="a37b1-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="a37b1-174">scaffolding de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="a37b1-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="a37b1-175">Scaffolds un tipos de entidad y DbContext para una base de datos.</span><span class="sxs-lookup"><span data-stu-id="a37b1-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="a37b1-176">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="a37b1-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="a37b1-177">\<CONEXIÓN &GT;</span><span class="sxs-lookup"><span data-stu-id="a37b1-177">\<CONNECTION></span></span> | <span data-ttu-id="a37b1-178">La cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a37b1-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="a37b1-179">\<PROVEEDOR &GT;</span><span class="sxs-lookup"><span data-stu-id="a37b1-179">\<PROVIDER></span></span>   | <span data-ttu-id="a37b1-180">El proveedor debe usar.</span><span class="sxs-lookup"><span data-stu-id="a37b1-180">The provider to use.</span></span> <span data-ttu-id="a37b1-181">(por ejemplo, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="a37b1-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="a37b1-182">Opciones:</span><span class="sxs-lookup"><span data-stu-id="a37b1-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a37b1-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="a37b1-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="a37b1-184">--las anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="a37b1-184">--data-annotations</span></span>                      | <span data-ttu-id="a37b1-185">Usar atributos para configurar el modelo (siempre que sea posible).</span><span class="sxs-lookup"><span data-stu-id="a37b1-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="a37b1-186">Si se omite, se usa solo la API fluida.</span><span class="sxs-lookup"><span data-stu-id="a37b1-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="a37b1-187">-c</span><span class="sxs-lookup"><span data-stu-id="a37b1-187">-c</span></span>              | <span data-ttu-id="a37b1-188">--contexto \<nombre ></span><span class="sxs-lookup"><span data-stu-id="a37b1-188">--context \<NAME></span></span>                       | <span data-ttu-id="a37b1-189">El nombre de DbContext.</span><span class="sxs-lookup"><span data-stu-id="a37b1-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="a37b1-190">--contexto-dir \<ruta de acceso ></span><span class="sxs-lookup"><span data-stu-id="a37b1-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="a37b1-191">El directorio para colocar el archivo de DbContext en.</span><span class="sxs-lookup"><span data-stu-id="a37b1-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="a37b1-192">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="a37b1-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="a37b1-193">-f</span><span class="sxs-lookup"><span data-stu-id="a37b1-193">-f</span></span>              | <span data-ttu-id="a37b1-194">--forzar</span><span class="sxs-lookup"><span data-stu-id="a37b1-194">--force</span></span>                                 | <span data-ttu-id="a37b1-195">Sobrescribir archivos existentes.</span><span class="sxs-lookup"><span data-stu-id="a37b1-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="a37b1-196">-o</span><span class="sxs-lookup"><span data-stu-id="a37b1-196">-o</span></span>              | <span data-ttu-id="a37b1-197">--salida-dir \<ruta de acceso ></span><span class="sxs-lookup"><span data-stu-id="a37b1-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="a37b1-198">El directorio para colocar archivos.</span><span class="sxs-lookup"><span data-stu-id="a37b1-198">The directory to put files in.</span></span> <span data-ttu-id="a37b1-199">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="a37b1-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="a37b1-200"><nobr>--esquema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="a37b1-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="a37b1-201">Los esquemas de tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="a37b1-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="a37b1-202">-t</span><span class="sxs-lookup"><span data-stu-id="a37b1-202">-t</span></span>              | <span data-ttu-id="a37b1-203">--tabla \<nombre_tabla >...</span><span class="sxs-lookup"><span data-stu-id="a37b1-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="a37b1-204">Las tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="a37b1-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="a37b1-205">--utilizar nombres de base de datos</span><span class="sxs-lookup"><span data-stu-id="a37b1-205">--use-database-names</span></span>                    | <span data-ttu-id="a37b1-206">Usar nombres de tabla y columna directamente desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a37b1-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="a37b1-207">agregar las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="a37b1-207">dotnet ef migrations add</span></span>

<span data-ttu-id="a37b1-208">Agrega una nueva migración.</span><span class="sxs-lookup"><span data-stu-id="a37b1-208">Adds a new migration.</span></span>

<span data-ttu-id="a37b1-209">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="a37b1-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="a37b1-210">\<NOMBRE &GT;</span><span class="sxs-lookup"><span data-stu-id="a37b1-210">\<NAME></span></span> | <span data-ttu-id="a37b1-211">El nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="a37b1-211">The name of the migration.</span></span> |

<span data-ttu-id="a37b1-212">Opciones:</span><span class="sxs-lookup"><span data-stu-id="a37b1-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a37b1-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="a37b1-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="a37b1-214"><nobr>--salida dir \<ruta de acceso ></nobr></span><span class="sxs-lookup"><span data-stu-id="a37b1-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="a37b1-215">El directorio (y subespacio de nombres) a utilizar.</span><span class="sxs-lookup"><span data-stu-id="a37b1-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="a37b1-216">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="a37b1-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="a37b1-217">El valor predeterminado es "Migraciones".</span><span class="sxs-lookup"><span data-stu-id="a37b1-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="a37b1-218">lista de las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="a37b1-218">dotnet ef migrations list</span></span>

<span data-ttu-id="a37b1-219">Migraciones de listas disponibles.</span><span class="sxs-lookup"><span data-stu-id="a37b1-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="a37b1-220">quitar las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="a37b1-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="a37b1-221">Quita la última migración.</span><span class="sxs-lookup"><span data-stu-id="a37b1-221">Removes the last migration.</span></span>

<span data-ttu-id="a37b1-222">Opciones:</span><span class="sxs-lookup"><span data-stu-id="a37b1-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="a37b1-223">-f</span><span class="sxs-lookup"><span data-stu-id="a37b1-223">-f</span></span> | <span data-ttu-id="a37b1-224">--forzar</span><span class="sxs-lookup"><span data-stu-id="a37b1-224">--force</span></span> | <span data-ttu-id="a37b1-225">Revertir la migración si se ha aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a37b1-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="a37b1-226">script de migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="a37b1-226">dotnet ef migrations script</span></span>

<span data-ttu-id="a37b1-227">Genera un script SQL de migraciones.</span><span class="sxs-lookup"><span data-stu-id="a37b1-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="a37b1-228">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="a37b1-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="a37b1-229">\<DE &GT;</span><span class="sxs-lookup"><span data-stu-id="a37b1-229">\<FROM></span></span> | <span data-ttu-id="a37b1-230">La migración inicial.</span><span class="sxs-lookup"><span data-stu-id="a37b1-230">The starting migration.</span></span> <span data-ttu-id="a37b1-231">El valor predeterminado es 0 (la base de datos inicial).</span><span class="sxs-lookup"><span data-stu-id="a37b1-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="a37b1-232">\<PARA &GT;</span><span class="sxs-lookup"><span data-stu-id="a37b1-232">\<TO></span></span>   | <span data-ttu-id="a37b1-233">La migración final.</span><span class="sxs-lookup"><span data-stu-id="a37b1-233">The ending migration.</span></span> <span data-ttu-id="a37b1-234">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="a37b1-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="a37b1-235">Opciones:</span><span class="sxs-lookup"><span data-stu-id="a37b1-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="a37b1-236">-o</span><span class="sxs-lookup"><span data-stu-id="a37b1-236">-o</span></span> | <span data-ttu-id="a37b1-237">--salida \<archivo ></span><span class="sxs-lookup"><span data-stu-id="a37b1-237">--output \<FILE></span></span> | <span data-ttu-id="a37b1-238">Archivo que se escribirá el resultado.</span><span class="sxs-lookup"><span data-stu-id="a37b1-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="a37b1-239">-i</span><span class="sxs-lookup"><span data-stu-id="a37b1-239">-i</span></span> | <span data-ttu-id="a37b1-240">--idempotente</span><span class="sxs-lookup"><span data-stu-id="a37b1-240">--idempotent</span></span>     | <span data-ttu-id="a37b1-241">Generar un script que se puede usar en una base de datos en cualquier migración.</span><span class="sxs-lookup"><span data-stu-id="a37b1-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
