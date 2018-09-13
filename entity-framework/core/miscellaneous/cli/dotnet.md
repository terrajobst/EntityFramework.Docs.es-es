---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 3534336f1caeed96079b35c739d694a536919020
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489614"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="34029-102">Herramientas de línea de comandos de EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="34029-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="34029-103">Las herramientas de línea de comandos de Entity Framework Core .NET son una extensión multiplataforma **dotnet** comando, que forma parte de la [SDK de .NET Core][2].</span><span class="sxs-lookup"><span data-stu-id="34029-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="34029-104">Si usa Visual Studio, se recomienda [las herramientas de PMC] [ 1] en su lugar ya que proporcionan una experiencia más integrada.</span><span class="sxs-lookup"><span data-stu-id="34029-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="34029-105">Instalación de las herramientas</span><span class="sxs-lookup"><span data-stu-id="34029-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="34029-106">El SDK de .NET Core 2.1.300 versión más reciente incluye **dotnet ef** comandos que son compatibles con EF Core 2.0 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="34029-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="34029-107">Por lo tanto, si usa las versiones recientes de .NET Core SDK y el tiempo de ejecución de EF Core, se requiere ninguna instalación y puede omitir el resto de esta sección.</span><span class="sxs-lookup"><span data-stu-id="34029-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="34029-108">Por otro lado, el **dotnet ef** herramienta contenida en la versión del SDK de .NET Core 2.1.300 y versiones más reciente no es compatible con la versión de EF Core 1.0 y 1.1.</span><span class="sxs-lookup"><span data-stu-id="34029-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="34029-109">Antes de que puede trabajar con un proyecto que usa estas versiones anteriores de EF Core en un equipo que tiene el SDK de .NET Core 2.1.300 o posterior, instalado, también debe instalar la versión 2.1.200 o una versión anterior del SDK y configurar la aplicación para que use esa versión anterior mediante la modificación de sus  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) archivo.</span><span class="sxs-lookup"><span data-stu-id="34029-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="34029-110">Este archivo normalmente se incluye en el directorio de la solución (uno por encima del proyecto).</span><span class="sxs-lookup"><span data-stu-id="34029-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="34029-111">A continuación, puede continuar con las instrucciones de instalación siguientes.</span><span class="sxs-lookup"><span data-stu-id="34029-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="34029-112">Las versiones anteriores del SDK de .NET Core, puede instalar las herramientas de línea de comandos de .NET de EF Core siguiendo estos pasos:</span><span class="sxs-lookup"><span data-stu-id="34029-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="34029-113">Edite el archivo de proyecto y agregue Microsoft.EntityFrameworkCore.Tools.DotNet como un elemento DotNetCliToolReference (ver abajo)</span><span class="sxs-lookup"><span data-stu-id="34029-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="34029-114">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="34029-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="34029-115">El proyecto resultante debe tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="34029-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="34029-116">Una referencia de paquete con `PrivateAssets="All"` significa que no se expone a los proyectos que hacen referencia a este proyecto, que es especialmente útil para los paquetes que normalmente se usan solo durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="34029-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="34029-117">Si hizo todo correctamente, debería poder ejecutar correctamente el siguiente comando en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="34029-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="34029-118">Uso de las herramientas</span><span class="sxs-lookup"><span data-stu-id="34029-118">Using the tools</span></span>
---------------
<span data-ttu-id="34029-119">Cada vez que se invoca un comando, intervienen dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="34029-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="34029-120">El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).</span><span class="sxs-lookup"><span data-stu-id="34029-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="34029-121">El proyecto de destino tiene como valor predeterminado para el proyecto en el directorio actual, pero se puede cambiar mediante la <nobr> **`--project`** </nobr> opción.</span><span class="sxs-lookup"><span data-stu-id="34029-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**`--project`**</nobr> option.</span></span>

<span data-ttu-id="34029-122">El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.</span><span class="sxs-lookup"><span data-stu-id="34029-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="34029-123">También tiene como valor predeterminado para el proyecto en el directorio actual, pero se puede cambiar mediante la **`--startup-project`** opción.</span><span class="sxs-lookup"><span data-stu-id="34029-123">It also defaults to the project in the current directory, but can be changed using the **`--startup-project`** option.</span></span>

> [!NOTE]
> <span data-ttu-id="34029-124">Por ejemplo, actualizando la base de datos de la aplicación web que tenga instalado en un proyecto diferente de EF Core tendría este aspecto: `dotnet ef database update --project {project-path}` (desde el directorio de aplicación web)</span><span class="sxs-lookup"><span data-stu-id="34029-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="34029-125">Opciones comunes:</span><span class="sxs-lookup"><span data-stu-id="34029-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | <span data-ttu-id="34029-126">Mostrar la salida JSON.</span><span class="sxs-lookup"><span data-stu-id="34029-126">Show JSON output.</span></span>           |
| <span data-ttu-id="34029-127">-c</span><span class="sxs-lookup"><span data-stu-id="34029-127">-c</span></span> | `--context <DBCONTEXT>`           | <span data-ttu-id="34029-128">La clase DbContext usar.</span><span class="sxs-lookup"><span data-stu-id="34029-128">The DbContext to use.</span></span>       |
| <span data-ttu-id="34029-129">-p</span><span class="sxs-lookup"><span data-stu-id="34029-129">-p</span></span> | `--project <PROJECT>`             | <span data-ttu-id="34029-130">El proyecto para usar.</span><span class="sxs-lookup"><span data-stu-id="34029-130">The project to use.</span></span>         |
| <span data-ttu-id="34029-131">-s</span><span class="sxs-lookup"><span data-stu-id="34029-131">-s</span></span> | `--startup-project <PROJECT>`     | <span data-ttu-id="34029-132">Para usar el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="34029-132">The startup project to use.</span></span> |
|    | `--framework <FRAMEWORK>`         | <span data-ttu-id="34029-133">La plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="34029-133">The target framework.</span></span>       |
|    | `--configuration <CONFIGURATION>` | <span data-ttu-id="34029-134">La configuración que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="34029-134">The configuration to use.</span></span>   |
|    | `--runtime <IDENTIFIER>`          | <span data-ttu-id="34029-135">El runtime que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="34029-135">The runtime to use.</span></span>         |
| <span data-ttu-id="34029-136">-h</span><span class="sxs-lookup"><span data-stu-id="34029-136">-h</span></span> | `--help`                           | <span data-ttu-id="34029-137">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="34029-137">Show help information.</span></span>      |
| <span data-ttu-id="34029-138">-v</span><span class="sxs-lookup"><span data-stu-id="34029-138">-v</span></span> | `--verbose`                        | <span data-ttu-id="34029-139">Mostrar resultados detallados.</span><span class="sxs-lookup"><span data-stu-id="34029-139">Show verbose output.</span></span>        |
|    | `--no-color`                       | <span data-ttu-id="34029-140">No colorear salida.</span><span class="sxs-lookup"><span data-stu-id="34029-140">Don't colorize output.</span></span>      |
|    | `--prefix-output`                  | <span data-ttu-id="34029-141">Prefijo con el nivel de salida.</span><span class="sxs-lookup"><span data-stu-id="34029-141">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="34029-142">Para especificar el entorno de ASP.NET Core, establezca el **ASPNETCORE_ENVIRONMENT** variable de entorno antes de ejecutar.</span><span class="sxs-lookup"><span data-stu-id="34029-142">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="34029-143">Comandos</span><span class="sxs-lookup"><span data-stu-id="34029-143">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="34029-144">colocación de base de datos de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="34029-144">dotnet ef database drop</span></span>

<span data-ttu-id="34029-145">Quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="34029-145">Drops the database.</span></span>

<span data-ttu-id="34029-146">Opciones:</span><span class="sxs-lookup"><span data-stu-id="34029-146">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="34029-147">-f</span><span class="sxs-lookup"><span data-stu-id="34029-147">-f</span></span> | `--force`   | <span data-ttu-id="34029-148">No se confirme.</span><span class="sxs-lookup"><span data-stu-id="34029-148">Don't confirm.</span></span>                                           |
|    | `--dry-run` | <span data-ttu-id="34029-149">Mostrar qué base de datos se quitarían, pero no colóquelo.</span><span class="sxs-lookup"><span data-stu-id="34029-149">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="34029-150">actualización de base de datos de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="34029-150">dotnet ef database update</span></span>

<span data-ttu-id="34029-151">Actualiza la base de datos para una migración especificada.</span><span class="sxs-lookup"><span data-stu-id="34029-151">Updates the database to a specified migration.</span></span>

<span data-ttu-id="34029-152">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="34029-152">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | <span data-ttu-id="34029-153">La migración de destino.</span><span class="sxs-lookup"><span data-stu-id="34029-153">The target migration.</span></span> <span data-ttu-id="34029-154">Si es 0, se revertirán todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="34029-154">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="34029-155">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="34029-155">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="34029-156">información de dotnet ef dbcontext</span><span class="sxs-lookup"><span data-stu-id="34029-156">dotnet ef dbcontext info</span></span>

<span data-ttu-id="34029-157">Obtiene información sobre un tipo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="34029-157">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="34029-158">lista de dbcontext de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="34029-158">dotnet ef dbcontext list</span></span>

<span data-ttu-id="34029-159">Enumera los tipos de DbContext disponibles.</span><span class="sxs-lookup"><span data-stu-id="34029-159">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="34029-160">scaffolding de dbcontext de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="34029-160">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="34029-161">Aplica la técnica scaffolding un tipos de entidad y DbContext para una base de datos.</span><span class="sxs-lookup"><span data-stu-id="34029-161">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="34029-162">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="34029-162">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | <span data-ttu-id="34029-163">La cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="34029-163">The connection string to the database.</span></span>                                      |
| `<PROVIDER>`   | <span data-ttu-id="34029-164">El proveedor debe usar.</span><span class="sxs-lookup"><span data-stu-id="34029-164">The provider to use.</span></span> <span data-ttu-id="34029-165">(por ejemplo, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="34029-165">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="34029-166">Opciones:</span><span class="sxs-lookup"><span data-stu-id="34029-166">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="34029-167"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="34029-167"><nobr>-d</nobr></span></span> | `--data-annotations`                      | <span data-ttu-id="34029-168">Usar atributos para configurar el modelo (donde sea posible).</span><span class="sxs-lookup"><span data-stu-id="34029-168">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="34029-169">Si se omite, se usa solo la API fluida.</span><span class="sxs-lookup"><span data-stu-id="34029-169">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="34029-170">-c</span><span class="sxs-lookup"><span data-stu-id="34029-170">-c</span></span>              | `--context <NAME>`                       | <span data-ttu-id="34029-171">El nombre de la clase DbContext.</span><span class="sxs-lookup"><span data-stu-id="34029-171">The name of the DbContext.</span></span>                                                                       |
|                 | `--context-dir <PATH>`                   | <span data-ttu-id="34029-172">Colocar el archivo de DbContext en el directorio.</span><span class="sxs-lookup"><span data-stu-id="34029-172">The directory to put DbContext file in.</span></span> <span data-ttu-id="34029-173">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="34029-173">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="34029-174">-f</span><span class="sxs-lookup"><span data-stu-id="34029-174">-f</span></span>              | `--force`                                 | <span data-ttu-id="34029-175">Sobrescribir archivos existentes.</span><span class="sxs-lookup"><span data-stu-id="34029-175">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="34029-176">-o</span><span class="sxs-lookup"><span data-stu-id="34029-176">-o</span></span>              | `--output-dir <PATH>`                    | <span data-ttu-id="34029-177">Para poner los archivos en el directorio.</span><span class="sxs-lookup"><span data-stu-id="34029-177">The directory to put files in.</span></span> <span data-ttu-id="34029-178">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="34029-178">Paths are relative to the project directory.</span></span>                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | <span data-ttu-id="34029-179">Los esquemas de tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="34029-179">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="34029-180">-t</span><span class="sxs-lookup"><span data-stu-id="34029-180">-t</span></span>              | <span data-ttu-id="34029-181">`--table <TABLE_NAME>`...</span><span class="sxs-lookup"><span data-stu-id="34029-181">`--table <TABLE_NAME>`...</span></span>                | <span data-ttu-id="34029-182">Para generar tipos de entidad para las tablas.</span><span class="sxs-lookup"><span data-stu-id="34029-182">The tables to generate entity types for.</span></span>                                                         |
|                 | `--use-database-names`                    | <span data-ttu-id="34029-183">Usar nombres de tabla y columna directamente desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="34029-183">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="34029-184">Agregar migraciones de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="34029-184">dotnet ef migrations add</span></span>

<span data-ttu-id="34029-185">Agrega una nueva migración.</span><span class="sxs-lookup"><span data-stu-id="34029-185">Adds a new migration.</span></span>

<span data-ttu-id="34029-186">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="34029-186">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | <span data-ttu-id="34029-187">El nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="34029-187">The name of the migration.</span></span> |

<span data-ttu-id="34029-188">Opciones:</span><span class="sxs-lookup"><span data-stu-id="34029-188">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="34029-189"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="34029-189"><nobr>-o</nobr></span></span> | <span data-ttu-id="34029-190"><nobr> `--output-dir <PATH>` </nobr></span><span class="sxs-lookup"><span data-stu-id="34029-190"><nobr> `--output-dir <PATH>` </nobr></span></span> | <span data-ttu-id="34029-191">El directorio (y subespacio de nombres) para usar.</span><span class="sxs-lookup"><span data-stu-id="34029-191">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="34029-192">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="34029-192">Paths are relative to the project directory.</span></span> <span data-ttu-id="34029-193">El valor predeterminado es "Migraciones".</span><span class="sxs-lookup"><span data-stu-id="34029-193">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="34029-194">lista de las migraciones de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="34029-194">dotnet ef migrations list</span></span>

<span data-ttu-id="34029-195">Migraciones de listas disponibles.</span><span class="sxs-lookup"><span data-stu-id="34029-195">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="34029-196">quitar las migraciones de ef de dotnet</span><span class="sxs-lookup"><span data-stu-id="34029-196">dotnet ef migrations remove</span></span>

<span data-ttu-id="34029-197">Quita la última migración.</span><span class="sxs-lookup"><span data-stu-id="34029-197">Removes the last migration.</span></span>

<span data-ttu-id="34029-198">Opciones:</span><span class="sxs-lookup"><span data-stu-id="34029-198">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="34029-199">-f</span><span class="sxs-lookup"><span data-stu-id="34029-199">-f</span></span> | `--force` | <span data-ttu-id="34029-200">Revertir la migración si se ha aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="34029-200">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="34029-201">secuencia de comandos de dotnet ef migrations</span><span class="sxs-lookup"><span data-stu-id="34029-201">dotnet ef migrations script</span></span>

<span data-ttu-id="34029-202">Genera un script SQL de migraciones.</span><span class="sxs-lookup"><span data-stu-id="34029-202">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="34029-203">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="34029-203">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | <span data-ttu-id="34029-204">La migración inicial.</span><span class="sxs-lookup"><span data-stu-id="34029-204">The starting migration.</span></span> <span data-ttu-id="34029-205">El valor predeterminado es 0 (la base de datos inicial).</span><span class="sxs-lookup"><span data-stu-id="34029-205">Defaults to 0 (the initial database).</span></span> |
| `<TO>`   | <span data-ttu-id="34029-206">La migración final.</span><span class="sxs-lookup"><span data-stu-id="34029-206">The ending migration.</span></span> <span data-ttu-id="34029-207">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="34029-207">Defaults to the last migration.</span></span>         |

<span data-ttu-id="34029-208">Opciones:</span><span class="sxs-lookup"><span data-stu-id="34029-208">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="34029-209">-o</span><span class="sxs-lookup"><span data-stu-id="34029-209">-o</span></span> | `--output <FILE>` | <span data-ttu-id="34029-210">Para escribir el resultado en el archivo.</span><span class="sxs-lookup"><span data-stu-id="34029-210">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="34029-211">-i</span><span class="sxs-lookup"><span data-stu-id="34029-211">-i</span></span> | `--idempotent`     | <span data-ttu-id="34029-212">Generar un script que se puede usar en una base de datos en cualquier migración.</span><span class="sxs-lookup"><span data-stu-id="34029-212">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
