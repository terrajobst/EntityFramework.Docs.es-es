---
title: .NET core CLI - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d053d53bd50d2e7d16223c5b4e4009c9bb2298bb
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="fff0c-102">Herramientas de línea de comandos de EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="fff0c-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="fff0c-103">Las herramientas de línea de comandos de .NET de Entity Framework Core son una extensión a la plataforma entre **dotnet** comando, que forma parte de la [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="fff0c-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="fff0c-104">Si está utilizando Visual Studio, se recomienda [las herramientas PMC] [ 1] en su lugar, ya que proporcionan una experiencia más integrada.</span><span class="sxs-lookup"><span data-stu-id="fff0c-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="fff0c-105">Instalación de las herramientas</span><span class="sxs-lookup"><span data-stu-id="fff0c-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="fff0c-106">.NET Core SDK versión 2.1.300 e incluye más reciente **dotnet ef** comandos que son compatibles con EF Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="fff0c-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="fff0c-107">Por lo tanto, si está utilizando las versiones recientes de .NET Core SDK y el tiempo de ejecución de EF Core, se requiere ninguna instalación y puede omitir el resto de esta sección.</span><span class="sxs-lookup"><span data-stu-id="fff0c-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="fff0c-108">Por otro lado, el **dotnet ef** herramienta independiente en la versión de .NET Core SDK 2.1.300 y versiones más reciente no es compatible con la versión de EF Core 1.0 y 1.1.</span><span class="sxs-lookup"><span data-stu-id="fff0c-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="fff0c-109">Antes de que puede trabajar con un proyecto que usa estas versiones anteriores de EF principal en un equipo que tenga .NET Core SDK 2.1.300 o posterior instalado, también debe instalar la versión 2.1.200 o anterior del SDK y configurar la aplicación para utilizar esa versión anterior mediante la modificación de su  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) archivo.</span><span class="sxs-lookup"><span data-stu-id="fff0c-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="fff0c-110">Este archivo normalmente se incluye en el directorio de la solución (uno por encima del proyecto).</span><span class="sxs-lookup"><span data-stu-id="fff0c-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="fff0c-111">A continuación, puede continuar con la siguiente instrucción de instalación.</span><span class="sxs-lookup"><span data-stu-id="fff0c-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="fff0c-112">Para versiones anteriores de .NET Core SDK, puede instalar las herramientas de línea de comandos de .NET de EF Core siguiendo estos pasos:</span><span class="sxs-lookup"><span data-stu-id="fff0c-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="fff0c-113">Edite el archivo de proyecto y agregar Microsoft.EntityFrameworkCore.Tools.DotNet como un elemento de DotNetCliToolReference (ver abajo)</span><span class="sxs-lookup"><span data-stu-id="fff0c-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="fff0c-114">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="fff0c-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="fff0c-115">El proyecto resultante debe tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="fff0c-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="fff0c-116">Una referencia de paquete con `PrivateAssets="All"` significa que no está expuesto a los proyectos que hacen referencia a este proyecto, que es especialmente útil para los paquetes que normalmente se utilizan solo durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="fff0c-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="fff0c-117">Si lo quitó correctamente todos los pasos correctamente, debe poder ejecutar correctamente el comando siguiente en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="fff0c-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="fff0c-118">Uso de las herramientas</span><span class="sxs-lookup"><span data-stu-id="fff0c-118">Using the tools</span></span>
---------------
<span data-ttu-id="fff0c-119">Cada vez que se invoca un comando, existen dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="fff0c-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="fff0c-120">El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).</span><span class="sxs-lookup"><span data-stu-id="fff0c-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="fff0c-121">El proyecto de destino tiene como valor predeterminado para el proyecto en el directorio actual, pero se puede cambiar mediante la <nobr> **--proyecto** </nobr> opción.</span><span class="sxs-lookup"><span data-stu-id="fff0c-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="fff0c-122">El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.</span><span class="sxs-lookup"><span data-stu-id="fff0c-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="fff0c-123">También tiene como valor predeterminado para el proyecto en el directorio actual pero se puede cambiar mediante la **: proyecto de inicio** opción.</span><span class="sxs-lookup"><span data-stu-id="fff0c-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="fff0c-124">Por ejemplo, actualizar la base de datos de la aplicación web que tiene instalado en un proyecto diferente de núcleo de EF sería similar al siguiente: `dotnet ef database update --project {project-path}` (desde el directorio de aplicación web)</span><span class="sxs-lookup"><span data-stu-id="fff0c-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="fff0c-125">Opciones comunes:</span><span class="sxs-lookup"><span data-stu-id="fff0c-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="fff0c-126">--json</span><span class="sxs-lookup"><span data-stu-id="fff0c-126">--json</span></span>                           | <span data-ttu-id="fff0c-127">Mostrar la salida JSON.</span><span class="sxs-lookup"><span data-stu-id="fff0c-127">Show JSON output.</span></span>           |
| <span data-ttu-id="fff0c-128">-c</span><span class="sxs-lookup"><span data-stu-id="fff0c-128">-c</span></span> | <span data-ttu-id="fff0c-129">--contexto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="fff0c-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="fff0c-130">DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="fff0c-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="fff0c-131">-p</span><span class="sxs-lookup"><span data-stu-id="fff0c-131">-p</span></span> | <span data-ttu-id="fff0c-132">--proyecto \<proyecto ></span><span class="sxs-lookup"><span data-stu-id="fff0c-132">--project \<PROJECT></span></span>             | <span data-ttu-id="fff0c-133">El proyecto debe usar.</span><span class="sxs-lookup"><span data-stu-id="fff0c-133">The project to use.</span></span>         |
| <span data-ttu-id="fff0c-134">-s</span><span class="sxs-lookup"><span data-stu-id="fff0c-134">-s</span></span> | <span data-ttu-id="fff0c-135">--proyecto de inicio \<proyecto ></span><span class="sxs-lookup"><span data-stu-id="fff0c-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="fff0c-136">Proyecto de inicio que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="fff0c-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="fff0c-137">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="fff0c-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="fff0c-138">El marco de destino.</span><span class="sxs-lookup"><span data-stu-id="fff0c-138">The target framework.</span></span>       |
|    | <span data-ttu-id="fff0c-139">--configuración \<configuración ></span><span class="sxs-lookup"><span data-stu-id="fff0c-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="fff0c-140">La configuración que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="fff0c-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="fff0c-141">--en tiempo de ejecución \<identificador ></span><span class="sxs-lookup"><span data-stu-id="fff0c-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="fff0c-142">El runtime que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="fff0c-142">The runtime to use.</span></span>         |
| <span data-ttu-id="fff0c-143">-h</span><span class="sxs-lookup"><span data-stu-id="fff0c-143">-h</span></span> | <span data-ttu-id="fff0c-144">--Ayuda</span><span class="sxs-lookup"><span data-stu-id="fff0c-144">--help</span></span>                           | <span data-ttu-id="fff0c-145">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="fff0c-145">Show help information.</span></span>      |
| <span data-ttu-id="fff0c-146">-v</span><span class="sxs-lookup"><span data-stu-id="fff0c-146">-v</span></span> | <span data-ttu-id="fff0c-147">--detallado</span><span class="sxs-lookup"><span data-stu-id="fff0c-147">--verbose</span></span>                        | <span data-ttu-id="fff0c-148">Mostrar resultados detallados.</span><span class="sxs-lookup"><span data-stu-id="fff0c-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="fff0c-149">--Sin color</span><span class="sxs-lookup"><span data-stu-id="fff0c-149">--no-color</span></span>                       | <span data-ttu-id="fff0c-150">No colorear la salida.</span><span class="sxs-lookup"><span data-stu-id="fff0c-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="fff0c-151">--salida de prefijo</span><span class="sxs-lookup"><span data-stu-id="fff0c-151">--prefix-output</span></span>                  | <span data-ttu-id="fff0c-152">Prefijo con el nivel de salida.</span><span class="sxs-lookup"><span data-stu-id="fff0c-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="fff0c-153">Para especificar el entorno de ASP.NET Core, establezca el **ASPNETCORE_ENVIRONMENT** variable de entorno antes de ejecutar.</span><span class="sxs-lookup"><span data-stu-id="fff0c-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="fff0c-154">Comandos</span><span class="sxs-lookup"><span data-stu-id="fff0c-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="fff0c-155">eliminación de base de datos de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="fff0c-155">dotnet ef database drop</span></span>

<span data-ttu-id="fff0c-156">Quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fff0c-156">Drops the database.</span></span>

<span data-ttu-id="fff0c-157">Opciones:</span><span class="sxs-lookup"><span data-stu-id="fff0c-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="fff0c-158">-f</span><span class="sxs-lookup"><span data-stu-id="fff0c-158">-f</span></span> | <span data-ttu-id="fff0c-159">--forzar</span><span class="sxs-lookup"><span data-stu-id="fff0c-159">--force</span></span>   | <span data-ttu-id="fff0c-160">No realiza esta confirmación.</span><span class="sxs-lookup"><span data-stu-id="fff0c-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="fff0c-161">---simulacro</span><span class="sxs-lookup"><span data-stu-id="fff0c-161">--dry-run</span></span> | <span data-ttu-id="fff0c-162">Mostrar qué base de datos se quitarían, pero no se quita.</span><span class="sxs-lookup"><span data-stu-id="fff0c-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="fff0c-163">actualización de base de datos de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="fff0c-163">dotnet ef database update</span></span>

<span data-ttu-id="fff0c-164">Actualiza la base de datos para una migración especificada.</span><span class="sxs-lookup"><span data-stu-id="fff0c-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="fff0c-165">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="fff0c-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="fff0c-166">\<MIGRACIÓN &GT;</span><span class="sxs-lookup"><span data-stu-id="fff0c-166">\<MIGRATION></span></span> | <span data-ttu-id="fff0c-167">La migración de destino.</span><span class="sxs-lookup"><span data-stu-id="fff0c-167">The target migration.</span></span> <span data-ttu-id="fff0c-168">Si es 0, se revertirán todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="fff0c-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="fff0c-169">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="fff0c-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="fff0c-170">información de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="fff0c-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="fff0c-171">Obtiene información sobre un tipo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="fff0c-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="fff0c-172">lista de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="fff0c-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="fff0c-173">Enumera los tipos de DbContext disponibles.</span><span class="sxs-lookup"><span data-stu-id="fff0c-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="fff0c-174">scaffolding de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="fff0c-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="fff0c-175">Scaffolds un tipos de entidad y DbContext para una base de datos.</span><span class="sxs-lookup"><span data-stu-id="fff0c-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="fff0c-176">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="fff0c-176">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="fff0c-177">\<CONEXIÓN &GT;</span><span class="sxs-lookup"><span data-stu-id="fff0c-177">\<CONNECTION></span></span> | <span data-ttu-id="fff0c-178">La cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fff0c-178">The connection string to the database.</span></span>                              |
| <span data-ttu-id="fff0c-179">\<PROVEEDOR &GT;</span><span class="sxs-lookup"><span data-stu-id="fff0c-179">\<PROVIDER></span></span>   | <span data-ttu-id="fff0c-180">El proveedor debe usar.</span><span class="sxs-lookup"><span data-stu-id="fff0c-180">The provider to use.</span></span> <span data-ttu-id="fff0c-181">(P. ej.,</span><span class="sxs-lookup"><span data-stu-id="fff0c-181">(E.g.</span></span> <span data-ttu-id="fff0c-182">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="fff0c-182">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="fff0c-183">Opciones:</span><span class="sxs-lookup"><span data-stu-id="fff0c-183">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fff0c-184"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="fff0c-184"><nobr>-d</nobr></span></span> | <span data-ttu-id="fff0c-185">--las anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="fff0c-185">--data-annotations</span></span>                      | <span data-ttu-id="fff0c-186">Usar atributos para configurar el modelo (siempre que sea posible).</span><span class="sxs-lookup"><span data-stu-id="fff0c-186">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="fff0c-187">Si se omite, se usa solo la API fluida.</span><span class="sxs-lookup"><span data-stu-id="fff0c-187">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="fff0c-188">-c</span><span class="sxs-lookup"><span data-stu-id="fff0c-188">-c</span></span>              | <span data-ttu-id="fff0c-189">--contexto \<nombre ></span><span class="sxs-lookup"><span data-stu-id="fff0c-189">--context \<NAME></span></span>                       | <span data-ttu-id="fff0c-190">El nombre de DbContext.</span><span class="sxs-lookup"><span data-stu-id="fff0c-190">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="fff0c-191">--contexto-dir \<ruta de acceso ></span><span class="sxs-lookup"><span data-stu-id="fff0c-191">--context-dir \<PATH></span></span>                   | <span data-ttu-id="fff0c-192">El directorio para colocar el archivo de DbContext en.</span><span class="sxs-lookup"><span data-stu-id="fff0c-192">The directory to put DbContext file in.</span></span> <span data-ttu-id="fff0c-193">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="fff0c-193">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="fff0c-194">-f</span><span class="sxs-lookup"><span data-stu-id="fff0c-194">-f</span></span>              | <span data-ttu-id="fff0c-195">--forzar</span><span class="sxs-lookup"><span data-stu-id="fff0c-195">--force</span></span>                                 | <span data-ttu-id="fff0c-196">Sobrescribir archivos existentes.</span><span class="sxs-lookup"><span data-stu-id="fff0c-196">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="fff0c-197">-o</span><span class="sxs-lookup"><span data-stu-id="fff0c-197">-o</span></span>              | <span data-ttu-id="fff0c-198">--salida-dir \<ruta de acceso ></span><span class="sxs-lookup"><span data-stu-id="fff0c-198">--output-dir \<PATH></span></span>                    | <span data-ttu-id="fff0c-199">El directorio para colocar archivos.</span><span class="sxs-lookup"><span data-stu-id="fff0c-199">The directory to put files in.</span></span> <span data-ttu-id="fff0c-200">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="fff0c-200">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="fff0c-201"><nobr>--esquema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="fff0c-201"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="fff0c-202">Los esquemas de tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="fff0c-202">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="fff0c-203">-t</span><span class="sxs-lookup"><span data-stu-id="fff0c-203">-t</span></span>              | <span data-ttu-id="fff0c-204">--tabla \<nombre_tabla >...</span><span class="sxs-lookup"><span data-stu-id="fff0c-204">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="fff0c-205">Las tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="fff0c-205">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="fff0c-206">--utilizar nombres de base de datos</span><span class="sxs-lookup"><span data-stu-id="fff0c-206">--use-database-names</span></span>                    | <span data-ttu-id="fff0c-207">Usar nombres de tabla y columna directamente desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fff0c-207">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="fff0c-208">agregar las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="fff0c-208">dotnet ef migrations add</span></span>

<span data-ttu-id="fff0c-209">Agrega una nueva migración.</span><span class="sxs-lookup"><span data-stu-id="fff0c-209">Adds a new migration.</span></span>

<span data-ttu-id="fff0c-210">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="fff0c-210">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="fff0c-211">\<NOMBRE &GT;</span><span class="sxs-lookup"><span data-stu-id="fff0c-211">\<NAME></span></span> | <span data-ttu-id="fff0c-212">El nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="fff0c-212">The name of the migration.</span></span> |

<span data-ttu-id="fff0c-213">Opciones:</span><span class="sxs-lookup"><span data-stu-id="fff0c-213">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fff0c-214"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="fff0c-214"><nobr>-o</nobr></span></span> | <span data-ttu-id="fff0c-215"><nobr>--salida dir \<ruta de acceso ></nobr></span><span class="sxs-lookup"><span data-stu-id="fff0c-215"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="fff0c-216">El directorio (y subespacio de nombres) a utilizar.</span><span class="sxs-lookup"><span data-stu-id="fff0c-216">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="fff0c-217">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="fff0c-217">Paths are relative to the project directory.</span></span> <span data-ttu-id="fff0c-218">El valor predeterminado es "Migraciones".</span><span class="sxs-lookup"><span data-stu-id="fff0c-218">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="fff0c-219">lista de las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="fff0c-219">dotnet ef migrations list</span></span>

<span data-ttu-id="fff0c-220">Migraciones de listas disponibles.</span><span class="sxs-lookup"><span data-stu-id="fff0c-220">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="fff0c-221">quitar las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="fff0c-221">dotnet ef migrations remove</span></span>

<span data-ttu-id="fff0c-222">Quita la última migración.</span><span class="sxs-lookup"><span data-stu-id="fff0c-222">Removes the last migration.</span></span>

<span data-ttu-id="fff0c-223">Opciones:</span><span class="sxs-lookup"><span data-stu-id="fff0c-223">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="fff0c-224">-f</span><span class="sxs-lookup"><span data-stu-id="fff0c-224">-f</span></span> | <span data-ttu-id="fff0c-225">--forzar</span><span class="sxs-lookup"><span data-stu-id="fff0c-225">--force</span></span> | <span data-ttu-id="fff0c-226">Revertir la migración si se ha aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fff0c-226">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="fff0c-227">script de migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="fff0c-227">dotnet ef migrations script</span></span>

<span data-ttu-id="fff0c-228">Genera un script SQL de migraciones.</span><span class="sxs-lookup"><span data-stu-id="fff0c-228">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="fff0c-229">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="fff0c-229">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="fff0c-230">\<DE &GT;</span><span class="sxs-lookup"><span data-stu-id="fff0c-230">\<FROM></span></span> | <span data-ttu-id="fff0c-231">La migración inicial.</span><span class="sxs-lookup"><span data-stu-id="fff0c-231">The starting migration.</span></span> <span data-ttu-id="fff0c-232">El valor predeterminado es 0 (la base de datos inicial).</span><span class="sxs-lookup"><span data-stu-id="fff0c-232">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="fff0c-233">\<PARA &GT;</span><span class="sxs-lookup"><span data-stu-id="fff0c-233">\<TO></span></span>   | <span data-ttu-id="fff0c-234">La migración final.</span><span class="sxs-lookup"><span data-stu-id="fff0c-234">The ending migration.</span></span> <span data-ttu-id="fff0c-235">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="fff0c-235">Defaults to the last migration.</span></span>         |

<span data-ttu-id="fff0c-236">Opciones:</span><span class="sxs-lookup"><span data-stu-id="fff0c-236">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="fff0c-237">-o</span><span class="sxs-lookup"><span data-stu-id="fff0c-237">-o</span></span> | <span data-ttu-id="fff0c-238">--salida \<archivo ></span><span class="sxs-lookup"><span data-stu-id="fff0c-238">--output \<FILE></span></span> | <span data-ttu-id="fff0c-239">Archivo que se escribirá el resultado.</span><span class="sxs-lookup"><span data-stu-id="fff0c-239">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="fff0c-240">-i</span><span class="sxs-lookup"><span data-stu-id="fff0c-240">-i</span></span> | <span data-ttu-id="fff0c-241">--idempotente</span><span class="sxs-lookup"><span data-stu-id="fff0c-241">--idempotent</span></span>     | <span data-ttu-id="fff0c-242">Generar un script que se puede usar en una base de datos en cualquier migración.</span><span class="sxs-lookup"><span data-stu-id="fff0c-242">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
