---
title: .NET core CLI - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 396d31c9d0c0f47d299f49e82e557ed29b8420e7
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="21bb2-102">Herramientas de línea de comandos de EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="21bb2-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="21bb2-103">Las herramientas de línea de comandos de .NET de Entity Framework Core son una extensión a la plataforma entre **dotnet** comando, que forma parte de la [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="21bb2-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="21bb2-104">Si está utilizando Visual Studio, se recomienda [las herramientas PMC] [ 1] en su lugar, ya que proporcionan una experiencia más integrada.</span><span class="sxs-lookup"><span data-stu-id="21bb2-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="21bb2-105">Instalación de las herramientas</span><span class="sxs-lookup"><span data-stu-id="21bb2-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="21bb2-106">Instalar las herramientas de línea de comandos de .NET de EF Core siguiendo estos pasos:</span><span class="sxs-lookup"><span data-stu-id="21bb2-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="21bb2-107">Edite el archivo de proyecto y agregar Microsoft.EntityFrameworkCore.Tools.DotNet como un elemento de DotNetCliToolReference (ver abajo)</span><span class="sxs-lookup"><span data-stu-id="21bb2-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="21bb2-108">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="21bb2-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="21bb2-109">El proyecto resultante debe tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="21bb2-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="21bb2-110">Una referencia de paquete con `PrivateAssets="All"` significa que no está expuesto a los proyectos que hacen referencia a este proyecto, que es especialmente útil para los paquetes que normalmente se utilizan solo durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="21bb2-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="21bb2-111">Si lo quitó correctamente todos los pasos correctamente, debe poder ejecutar correctamente el comando siguiente en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="21bb2-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="21bb2-112">Uso de las herramientas</span><span class="sxs-lookup"><span data-stu-id="21bb2-112">Using the tools</span></span>
---------------
<span data-ttu-id="21bb2-113">Cada vez que se invoca un comando, existen dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="21bb2-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="21bb2-114">El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).</span><span class="sxs-lookup"><span data-stu-id="21bb2-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="21bb2-115">El proyecto de destino tiene como valor predeterminado para el proyecto en el directorio actual, pero se puede cambiar mediante la <nobr> **--proyecto** </nobr> opción.</span><span class="sxs-lookup"><span data-stu-id="21bb2-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="21bb2-116">El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.</span><span class="sxs-lookup"><span data-stu-id="21bb2-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="21bb2-117">También tiene como valor predeterminado para el proyecto en el directorio actual pero se puede cambiar mediante la **: proyecto de inicio** opción.</span><span class="sxs-lookup"><span data-stu-id="21bb2-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="21bb2-118">Por ejemplo, actualizar la base de datos de la aplicación web que tiene instalado en un proyecto diferente de núcleo de EF sería similar al siguiente: `dotnet ef database update --project {project-path}` (desde el directorio de aplicación web)</span><span class="sxs-lookup"><span data-stu-id="21bb2-118">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="21bb2-119">Opciones comunes:</span><span class="sxs-lookup"><span data-stu-id="21bb2-119">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="21bb2-120">--json</span><span class="sxs-lookup"><span data-stu-id="21bb2-120">--json</span></span>                           | <span data-ttu-id="21bb2-121">Mostrar la salida JSON.</span><span class="sxs-lookup"><span data-stu-id="21bb2-121">Show JSON output.</span></span>           |
| <span data-ttu-id="21bb2-122">-c</span><span class="sxs-lookup"><span data-stu-id="21bb2-122">-c</span></span> | <span data-ttu-id="21bb2-123">--contexto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="21bb2-123">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="21bb2-124">DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="21bb2-124">The DbContext to use.</span></span>       |
| <span data-ttu-id="21bb2-125">-p</span><span class="sxs-lookup"><span data-stu-id="21bb2-125">-p</span></span> | <span data-ttu-id="21bb2-126">--proyecto \<proyecto ></span><span class="sxs-lookup"><span data-stu-id="21bb2-126">--project \<PROJECT></span></span>             | <span data-ttu-id="21bb2-127">El proyecto debe usar.</span><span class="sxs-lookup"><span data-stu-id="21bb2-127">The project to use.</span></span>         |
| <span data-ttu-id="21bb2-128">-s</span><span class="sxs-lookup"><span data-stu-id="21bb2-128">-s</span></span> | <span data-ttu-id="21bb2-129">--proyecto de inicio \<proyecto ></span><span class="sxs-lookup"><span data-stu-id="21bb2-129">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="21bb2-130">Proyecto de inicio que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="21bb2-130">The startup project to use.</span></span> |
|    | <span data-ttu-id="21bb2-131">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="21bb2-131">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="21bb2-132">El marco de destino.</span><span class="sxs-lookup"><span data-stu-id="21bb2-132">The target framework.</span></span>       |
|    | <span data-ttu-id="21bb2-133">--configuración \<configuración ></span><span class="sxs-lookup"><span data-stu-id="21bb2-133">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="21bb2-134">La configuración que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="21bb2-134">The configuration to use.</span></span>   |
|    | <span data-ttu-id="21bb2-135">--en tiempo de ejecución \<identificador ></span><span class="sxs-lookup"><span data-stu-id="21bb2-135">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="21bb2-136">El runtime que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="21bb2-136">The runtime to use.</span></span>         |
| <span data-ttu-id="21bb2-137">-h</span><span class="sxs-lookup"><span data-stu-id="21bb2-137">-h</span></span> | <span data-ttu-id="21bb2-138">--Ayuda</span><span class="sxs-lookup"><span data-stu-id="21bb2-138">--help</span></span>                           | <span data-ttu-id="21bb2-139">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="21bb2-139">Show help information.</span></span>      |
| <span data-ttu-id="21bb2-140">-v</span><span class="sxs-lookup"><span data-stu-id="21bb2-140">-v</span></span> | <span data-ttu-id="21bb2-141">--detallado</span><span class="sxs-lookup"><span data-stu-id="21bb2-141">--verbose</span></span>                        | <span data-ttu-id="21bb2-142">Mostrar resultados detallados.</span><span class="sxs-lookup"><span data-stu-id="21bb2-142">Show verbose output.</span></span>        |
|    | <span data-ttu-id="21bb2-143">--Sin color</span><span class="sxs-lookup"><span data-stu-id="21bb2-143">--no-color</span></span>                       | <span data-ttu-id="21bb2-144">No colorear la salida.</span><span class="sxs-lookup"><span data-stu-id="21bb2-144">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="21bb2-145">--salida de prefijo</span><span class="sxs-lookup"><span data-stu-id="21bb2-145">--prefix-output</span></span>                  | <span data-ttu-id="21bb2-146">Prefijo con el nivel de salida.</span><span class="sxs-lookup"><span data-stu-id="21bb2-146">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="21bb2-147">Para especificar el entorno de ASP.NET Core, establezca el **ASPNETCORE_ENVIRONMENT** variable de entorno antes de ejecutar.</span><span class="sxs-lookup"><span data-stu-id="21bb2-147">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="21bb2-148">Comandos</span><span class="sxs-lookup"><span data-stu-id="21bb2-148">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="21bb2-149">eliminación de base de datos de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="21bb2-149">dotnet ef database drop</span></span>

<span data-ttu-id="21bb2-150">Quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="21bb2-150">Drops the database.</span></span>

<span data-ttu-id="21bb2-151">Opciones:</span><span class="sxs-lookup"><span data-stu-id="21bb2-151">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="21bb2-152">-f</span><span class="sxs-lookup"><span data-stu-id="21bb2-152">-f</span></span> | <span data-ttu-id="21bb2-153">--forzar</span><span class="sxs-lookup"><span data-stu-id="21bb2-153">--force</span></span>   | <span data-ttu-id="21bb2-154">No realiza esta confirmación.</span><span class="sxs-lookup"><span data-stu-id="21bb2-154">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="21bb2-155">---simulacro</span><span class="sxs-lookup"><span data-stu-id="21bb2-155">--dry-run</span></span> | <span data-ttu-id="21bb2-156">Mostrar qué base de datos se quitarían, pero no se quita.</span><span class="sxs-lookup"><span data-stu-id="21bb2-156">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="21bb2-157">actualización de base de datos de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="21bb2-157">dotnet ef database update</span></span>

<span data-ttu-id="21bb2-158">Actualiza la base de datos para una migración especificada.</span><span class="sxs-lookup"><span data-stu-id="21bb2-158">Updates the database to a specified migration.</span></span>

<span data-ttu-id="21bb2-159">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="21bb2-159">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="21bb2-160">\<MIGRACIÓN &GT;</span><span class="sxs-lookup"><span data-stu-id="21bb2-160">\<MIGRATION></span></span> | <span data-ttu-id="21bb2-161">La migración de destino.</span><span class="sxs-lookup"><span data-stu-id="21bb2-161">The target migration.</span></span> <span data-ttu-id="21bb2-162">Si es 0, se revertirán todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="21bb2-162">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="21bb2-163">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="21bb2-163">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="21bb2-164">información de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="21bb2-164">dotnet ef dbcontext info</span></span>

<span data-ttu-id="21bb2-165">Obtiene información sobre un tipo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="21bb2-165">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="21bb2-166">lista de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="21bb2-166">dotnet ef dbcontext list</span></span>

<span data-ttu-id="21bb2-167">Enumera los tipos de DbContext disponibles.</span><span class="sxs-lookup"><span data-stu-id="21bb2-167">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="21bb2-168">scaffolding de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="21bb2-168">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="21bb2-169">Scaffolds un tipos de entidad y DbContext para una base de datos.</span><span class="sxs-lookup"><span data-stu-id="21bb2-169">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="21bb2-170">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="21bb2-170">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="21bb2-171">\<CONEXIÓN &GT;</span><span class="sxs-lookup"><span data-stu-id="21bb2-171">\<CONNECTION></span></span> | <span data-ttu-id="21bb2-172">La cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="21bb2-172">The connection string to the database.</span></span>                              |
| <span data-ttu-id="21bb2-173">\<PROVEEDOR &GT;</span><span class="sxs-lookup"><span data-stu-id="21bb2-173">\<PROVIDER></span></span>   | <span data-ttu-id="21bb2-174">El proveedor debe usar.</span><span class="sxs-lookup"><span data-stu-id="21bb2-174">The provider to use.</span></span> <span data-ttu-id="21bb2-175">(P. ej.,</span><span class="sxs-lookup"><span data-stu-id="21bb2-175">(E.g.</span></span> <span data-ttu-id="21bb2-176">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="21bb2-176">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="21bb2-177">Opciones:</span><span class="sxs-lookup"><span data-stu-id="21bb2-177">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="21bb2-178"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="21bb2-178"><nobr>-d</nobr></span></span> | <span data-ttu-id="21bb2-179">--las anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="21bb2-179">--data-annotations</span></span>                      | <span data-ttu-id="21bb2-180">Usar atributos para configurar el modelo (siempre que sea posible).</span><span class="sxs-lookup"><span data-stu-id="21bb2-180">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="21bb2-181">Si se omite, se usa solo la API fluida.</span><span class="sxs-lookup"><span data-stu-id="21bb2-181">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="21bb2-182">-c</span><span class="sxs-lookup"><span data-stu-id="21bb2-182">-c</span></span>              | <span data-ttu-id="21bb2-183">--contexto \<nombre ></span><span class="sxs-lookup"><span data-stu-id="21bb2-183">--context \<NAME></span></span>                       | <span data-ttu-id="21bb2-184">El nombre de DbContext.</span><span class="sxs-lookup"><span data-stu-id="21bb2-184">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="21bb2-185">--contexto-dir \<ruta de acceso ></span><span class="sxs-lookup"><span data-stu-id="21bb2-185">--context-dir \<PATH></span></span>                   | <span data-ttu-id="21bb2-186">El directorio para colocar el archivo de DbContext en.</span><span class="sxs-lookup"><span data-stu-id="21bb2-186">The directory to put DbContext file in.</span></span> <span data-ttu-id="21bb2-187">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="21bb2-187">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="21bb2-188">-f</span><span class="sxs-lookup"><span data-stu-id="21bb2-188">-f</span></span>              | <span data-ttu-id="21bb2-189">--forzar</span><span class="sxs-lookup"><span data-stu-id="21bb2-189">--force</span></span>                                 | <span data-ttu-id="21bb2-190">Sobrescribir archivos existentes.</span><span class="sxs-lookup"><span data-stu-id="21bb2-190">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="21bb2-191">-o</span><span class="sxs-lookup"><span data-stu-id="21bb2-191">-o</span></span>              | <span data-ttu-id="21bb2-192">--salida-dir \<ruta de acceso ></span><span class="sxs-lookup"><span data-stu-id="21bb2-192">--output-dir \<PATH></span></span>                    | <span data-ttu-id="21bb2-193">El directorio para colocar archivos.</span><span class="sxs-lookup"><span data-stu-id="21bb2-193">The directory to put files in.</span></span> <span data-ttu-id="21bb2-194">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="21bb2-194">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="21bb2-195"><nobr>--esquema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="21bb2-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="21bb2-196">Los esquemas de tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="21bb2-196">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="21bb2-197">-t</span><span class="sxs-lookup"><span data-stu-id="21bb2-197">-t</span></span>              | <span data-ttu-id="21bb2-198">--tabla \<nombre_tabla >...</span><span class="sxs-lookup"><span data-stu-id="21bb2-198">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="21bb2-199">Las tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="21bb2-199">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="21bb2-200">--utilizar nombres de base de datos</span><span class="sxs-lookup"><span data-stu-id="21bb2-200">--use-database-names</span></span>                    | <span data-ttu-id="21bb2-201">Usar nombres de tabla y columna directamente desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="21bb2-201">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="21bb2-202">agregar las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="21bb2-202">dotnet ef migrations add</span></span>

<span data-ttu-id="21bb2-203">Agrega una nueva migración.</span><span class="sxs-lookup"><span data-stu-id="21bb2-203">Adds a new migration.</span></span>

<span data-ttu-id="21bb2-204">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="21bb2-204">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="21bb2-205">\<NOMBRE &GT;</span><span class="sxs-lookup"><span data-stu-id="21bb2-205">\<NAME></span></span> | <span data-ttu-id="21bb2-206">El nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="21bb2-206">The name of the migration.</span></span> |

<span data-ttu-id="21bb2-207">Opciones:</span><span class="sxs-lookup"><span data-stu-id="21bb2-207">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="21bb2-208"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="21bb2-208"><nobr>-o</nobr></span></span> | <span data-ttu-id="21bb2-209"><nobr>--salida dir \<ruta de acceso ></nobr></span><span class="sxs-lookup"><span data-stu-id="21bb2-209"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="21bb2-210">El directorio (y subespacio de nombres) a utilizar.</span><span class="sxs-lookup"><span data-stu-id="21bb2-210">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="21bb2-211">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="21bb2-211">Paths are relative to the project directory.</span></span> <span data-ttu-id="21bb2-212">El valor predeterminado es "Migraciones".</span><span class="sxs-lookup"><span data-stu-id="21bb2-212">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="21bb2-213">lista de las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="21bb2-213">dotnet ef migrations list</span></span>

<span data-ttu-id="21bb2-214">Migraciones de listas disponibles.</span><span class="sxs-lookup"><span data-stu-id="21bb2-214">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="21bb2-215">quitar las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="21bb2-215">dotnet ef migrations remove</span></span>

<span data-ttu-id="21bb2-216">Quita la última migración.</span><span class="sxs-lookup"><span data-stu-id="21bb2-216">Removes the last migration.</span></span>

<span data-ttu-id="21bb2-217">Opciones:</span><span class="sxs-lookup"><span data-stu-id="21bb2-217">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="21bb2-218">-f</span><span class="sxs-lookup"><span data-stu-id="21bb2-218">-f</span></span> | <span data-ttu-id="21bb2-219">--forzar</span><span class="sxs-lookup"><span data-stu-id="21bb2-219">--force</span></span> | <span data-ttu-id="21bb2-220">Revertir la migración si se ha aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="21bb2-220">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="21bb2-221">script de migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="21bb2-221">dotnet ef migrations script</span></span>

<span data-ttu-id="21bb2-222">Genera un script SQL de migraciones.</span><span class="sxs-lookup"><span data-stu-id="21bb2-222">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="21bb2-223">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="21bb2-223">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="21bb2-224">\<DE &GT;</span><span class="sxs-lookup"><span data-stu-id="21bb2-224">\<FROM></span></span> | <span data-ttu-id="21bb2-225">La migración inicial.</span><span class="sxs-lookup"><span data-stu-id="21bb2-225">The starting migration.</span></span> <span data-ttu-id="21bb2-226">El valor predeterminado es 0 (la base de datos inicial).</span><span class="sxs-lookup"><span data-stu-id="21bb2-226">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="21bb2-227">\<PARA &GT;</span><span class="sxs-lookup"><span data-stu-id="21bb2-227">\<TO></span></span>   | <span data-ttu-id="21bb2-228">La migración final.</span><span class="sxs-lookup"><span data-stu-id="21bb2-228">The ending migration.</span></span> <span data-ttu-id="21bb2-229">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="21bb2-229">Defaults to the last migration.</span></span>         |

<span data-ttu-id="21bb2-230">Opciones:</span><span class="sxs-lookup"><span data-stu-id="21bb2-230">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="21bb2-231">-o</span><span class="sxs-lookup"><span data-stu-id="21bb2-231">-o</span></span> | <span data-ttu-id="21bb2-232">--salida \<archivo ></span><span class="sxs-lookup"><span data-stu-id="21bb2-232">--output \<FILE></span></span> | <span data-ttu-id="21bb2-233">Archivo que se escribirá el resultado.</span><span class="sxs-lookup"><span data-stu-id="21bb2-233">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="21bb2-234">-i</span><span class="sxs-lookup"><span data-stu-id="21bb2-234">-i</span></span> | <span data-ttu-id="21bb2-235">--idempotente</span><span class="sxs-lookup"><span data-stu-id="21bb2-235">--idempotent</span></span>     | <span data-ttu-id="21bb2-236">Generar un script que se puede usar en una base de datos en cualquier migración.</span><span class="sxs-lookup"><span data-stu-id="21bb2-236">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
