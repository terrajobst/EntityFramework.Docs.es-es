---
title: .NET core CLI - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 8a52cb8259bb381729a33a8161aec4b73f69f45d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="5a365-102">Herramientas de línea de comandos de EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="5a365-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="5a365-103">Las herramientas de línea de comandos de .NET de Entity Framework Core son una extensión a la plataforma entre **dotnet** comando, que forma parte de la [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="5a365-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="5a365-104">Si está utilizando Visual Studio, se recomienda [las herramientas PMC] [ 1] en su lugar, ya que proporcionan una experiencia más integrada.</span><span class="sxs-lookup"><span data-stu-id="5a365-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="5a365-105">Instalación de las herramientas</span><span class="sxs-lookup"><span data-stu-id="5a365-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="5a365-106">Instalar las herramientas de línea de comandos de .NET de EF Core siguiendo estos pasos:</span><span class="sxs-lookup"><span data-stu-id="5a365-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="5a365-107">Edite el archivo de proyecto y agregar Microsoft.EntityFrameworkCore.Tools.DotNet como un elemento de DotNetCliToolReference (ver abajo)</span><span class="sxs-lookup"><span data-stu-id="5a365-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="5a365-108">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5a365-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="5a365-109">El proyecto resultante debe tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="5a365-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="5a365-110">Una referencia de paquete con `PrivateAssets="All"` significa que no está expuesto a los proyectos que hacen referencia a este proyecto, que es especialmente útil para los paquetes que normalmente se utilizan solo durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="5a365-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="5a365-111">Si lo quitó correctamente todos los pasos correctamente, debe poder ejecutar correctamente el comando siguiente en un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="5a365-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="5a365-112">Uso de las herramientas</span><span class="sxs-lookup"><span data-stu-id="5a365-112">Using the tools</span></span>
---------------
<span data-ttu-id="5a365-113">Cada vez que se invoca un comando, existen dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="5a365-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="5a365-114">El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).</span><span class="sxs-lookup"><span data-stu-id="5a365-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="5a365-115">El proyecto de destino tiene como valor predeterminado para el proyecto en el directorio actual, pero se puede cambiar mediante la <nobr> **--proyecto** </nobr> opción.</span><span class="sxs-lookup"><span data-stu-id="5a365-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="5a365-116">El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.</span><span class="sxs-lookup"><span data-stu-id="5a365-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="5a365-117">También tiene como valor predeterminado para el proyecto en el directorio actual pero se puede cambiar mediante la **: proyecto de inicio** opción.</span><span class="sxs-lookup"><span data-stu-id="5a365-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="5a365-118">Opciones comunes:</span><span class="sxs-lookup"><span data-stu-id="5a365-118">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="5a365-119">--json</span><span class="sxs-lookup"><span data-stu-id="5a365-119">--json</span></span>                           | <span data-ttu-id="5a365-120">Mostrar la salida JSON.</span><span class="sxs-lookup"><span data-stu-id="5a365-120">Show JSON output.</span></span>           |
| <span data-ttu-id="5a365-121">-c</span><span class="sxs-lookup"><span data-stu-id="5a365-121">-c</span></span> | <span data-ttu-id="5a365-122">--contexto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="5a365-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="5a365-123">DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="5a365-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="5a365-124">-p</span><span class="sxs-lookup"><span data-stu-id="5a365-124">-p</span></span> | <span data-ttu-id="5a365-125">--proyecto \<proyecto ></span><span class="sxs-lookup"><span data-stu-id="5a365-125">--project \<PROJECT></span></span>             | <span data-ttu-id="5a365-126">El proyecto debe usar.</span><span class="sxs-lookup"><span data-stu-id="5a365-126">The project to use.</span></span>         |
| <span data-ttu-id="5a365-127">-s</span><span class="sxs-lookup"><span data-stu-id="5a365-127">-s</span></span> | <span data-ttu-id="5a365-128">--proyecto de inicio \<proyecto ></span><span class="sxs-lookup"><span data-stu-id="5a365-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="5a365-129">Proyecto de inicio que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="5a365-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="5a365-130">--framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="5a365-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="5a365-131">El marco de destino.</span><span class="sxs-lookup"><span data-stu-id="5a365-131">The target framework.</span></span>       |
|    | <span data-ttu-id="5a365-132">--configuración \<configuración ></span><span class="sxs-lookup"><span data-stu-id="5a365-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="5a365-133">La configuración que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="5a365-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="5a365-134">--en tiempo de ejecución \<identificador ></span><span class="sxs-lookup"><span data-stu-id="5a365-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="5a365-135">El runtime que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="5a365-135">The runtime to use.</span></span>         |
| <span data-ttu-id="5a365-136">-h</span><span class="sxs-lookup"><span data-stu-id="5a365-136">-h</span></span> | <span data-ttu-id="5a365-137">--Ayuda</span><span class="sxs-lookup"><span data-stu-id="5a365-137">--help</span></span>                           | <span data-ttu-id="5a365-138">Mostrar información de ayuda.</span><span class="sxs-lookup"><span data-stu-id="5a365-138">Show help information.</span></span>      |
| <span data-ttu-id="5a365-139">-v</span><span class="sxs-lookup"><span data-stu-id="5a365-139">-v</span></span> | <span data-ttu-id="5a365-140">--detallado</span><span class="sxs-lookup"><span data-stu-id="5a365-140">--verbose</span></span>                        | <span data-ttu-id="5a365-141">Mostrar resultados detallados.</span><span class="sxs-lookup"><span data-stu-id="5a365-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="5a365-142">--no-color</span><span class="sxs-lookup"><span data-stu-id="5a365-142">--no-color</span></span>                       | <span data-ttu-id="5a365-143">No colorear la salida.</span><span class="sxs-lookup"><span data-stu-id="5a365-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="5a365-144">--salida de prefijo</span><span class="sxs-lookup"><span data-stu-id="5a365-144">--prefix-output</span></span>                  | <span data-ttu-id="5a365-145">Prefijo con el nivel de salida.</span><span class="sxs-lookup"><span data-stu-id="5a365-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="5a365-146">Para especificar el entorno de ASP.NET Core, establezca el **ASPNETCORE_ENVIRONMENT** variable de entorno antes de ejecutar.</span><span class="sxs-lookup"><span data-stu-id="5a365-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="5a365-147">Comandos</span><span class="sxs-lookup"><span data-stu-id="5a365-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="5a365-148">eliminación de base de datos de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="5a365-148">dotnet ef database drop</span></span>

<span data-ttu-id="5a365-149">Quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5a365-149">Drops the database.</span></span>

<span data-ttu-id="5a365-150">Opciones:</span><span class="sxs-lookup"><span data-stu-id="5a365-150">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="5a365-151">-f</span><span class="sxs-lookup"><span data-stu-id="5a365-151">-f</span></span> | <span data-ttu-id="5a365-152">--forzar</span><span class="sxs-lookup"><span data-stu-id="5a365-152">--force</span></span>   | <span data-ttu-id="5a365-153">No realiza esta confirmación.</span><span class="sxs-lookup"><span data-stu-id="5a365-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="5a365-154">---simulacro</span><span class="sxs-lookup"><span data-stu-id="5a365-154">--dry-run</span></span> | <span data-ttu-id="5a365-155">Mostrar qué base de datos se quitarían, pero no se quita.</span><span class="sxs-lookup"><span data-stu-id="5a365-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="5a365-156">actualización de base de datos de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="5a365-156">dotnet ef database update</span></span>

<span data-ttu-id="5a365-157">Actualiza la base de datos para una migración especificada.</span><span class="sxs-lookup"><span data-stu-id="5a365-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="5a365-158">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="5a365-158">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="5a365-159">\<MIGRACIÓN ></span><span class="sxs-lookup"><span data-stu-id="5a365-159">\<MIGRATION></span></span> | <span data-ttu-id="5a365-160">La migración de destino.</span><span class="sxs-lookup"><span data-stu-id="5a365-160">The target migration.</span></span> <span data-ttu-id="5a365-161">Si es 0, se revertirán todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="5a365-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="5a365-162">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="5a365-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="5a365-163">información de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="5a365-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="5a365-164">Obtiene información sobre un tipo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="5a365-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="5a365-165">lista de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="5a365-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="5a365-166">Enumera los tipos de DbContext disponibles.</span><span class="sxs-lookup"><span data-stu-id="5a365-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="5a365-167">scaffolding de dbcontext ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="5a365-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="5a365-168">Scaffolds un tipos de entidad y DbContext para una base de datos.</span><span class="sxs-lookup"><span data-stu-id="5a365-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="5a365-169">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="5a365-169">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="5a365-170">\<CONEXIÓN ></span><span class="sxs-lookup"><span data-stu-id="5a365-170">\<CONNECTION></span></span> | <span data-ttu-id="5a365-171">La cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5a365-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="5a365-172">\<PROVEEDOR ></span><span class="sxs-lookup"><span data-stu-id="5a365-172">\<PROVIDER></span></span>   | <span data-ttu-id="5a365-173">El proveedor debe usar.</span><span class="sxs-lookup"><span data-stu-id="5a365-173">The provider to use.</span></span> <span data-ttu-id="5a365-174">(P. ej.</span><span class="sxs-lookup"><span data-stu-id="5a365-174">(E.g.</span></span> <span data-ttu-id="5a365-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="5a365-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="5a365-176">Opciones:</span><span class="sxs-lookup"><span data-stu-id="5a365-176">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5a365-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="5a365-177"><nobr>-d</nobr></span></span> | <span data-ttu-id="5a365-178">--data-annotations</span><span class="sxs-lookup"><span data-stu-id="5a365-178">--data-annotations</span></span>                      | <span data-ttu-id="5a365-179">Usar atributos para configurar el modelo (siempre que sea posible).</span><span class="sxs-lookup"><span data-stu-id="5a365-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="5a365-180">Si se omite, se usa solo la API fluida.</span><span class="sxs-lookup"><span data-stu-id="5a365-180">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="5a365-181">-c</span><span class="sxs-lookup"><span data-stu-id="5a365-181">-c</span></span>              | <span data-ttu-id="5a365-182">--contexto \<nombre ></span><span class="sxs-lookup"><span data-stu-id="5a365-182">--context \<NAME></span></span>                       | <span data-ttu-id="5a365-183">El nombre de DbContext.</span><span class="sxs-lookup"><span data-stu-id="5a365-183">The name of the DbContext.</span></span>                                                                       |
| <span data-ttu-id="5a365-184">-f</span><span class="sxs-lookup"><span data-stu-id="5a365-184">-f</span></span>              | <span data-ttu-id="5a365-185">--forzar</span><span class="sxs-lookup"><span data-stu-id="5a365-185">--force</span></span>                                 | <span data-ttu-id="5a365-186">Sobrescribir archivos existentes.</span><span class="sxs-lookup"><span data-stu-id="5a365-186">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="5a365-187">-o</span><span class="sxs-lookup"><span data-stu-id="5a365-187">-o</span></span>              | <span data-ttu-id="5a365-188">--salida-dir \<ruta de acceso ></span><span class="sxs-lookup"><span data-stu-id="5a365-188">--output-dir \<PATH></span></span>                    | <span data-ttu-id="5a365-189">El directorio para colocar archivos.</span><span class="sxs-lookup"><span data-stu-id="5a365-189">The directory to put files in.</span></span> <span data-ttu-id="5a365-190">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="5a365-190">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="5a365-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="5a365-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="5a365-192">Los esquemas de tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="5a365-192">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="5a365-193">-t</span><span class="sxs-lookup"><span data-stu-id="5a365-193">-t</span></span>              | <span data-ttu-id="5a365-194">--tabla \<nombre_tabla >...</span><span class="sxs-lookup"><span data-stu-id="5a365-194">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="5a365-195">Las tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="5a365-195">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="5a365-196">--utilizar nombres de base de datos</span><span class="sxs-lookup"><span data-stu-id="5a365-196">--use-database-names</span></span>                    | <span data-ttu-id="5a365-197">Usar nombres de tabla y columna directamente desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5a365-197">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="5a365-198">agregar las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="5a365-198">dotnet ef migrations add</span></span>

<span data-ttu-id="5a365-199">Agrega una nueva migración.</span><span class="sxs-lookup"><span data-stu-id="5a365-199">Adds a new migration.</span></span>

<span data-ttu-id="5a365-200">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="5a365-200">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="5a365-201">\<NOMBRE ></span><span class="sxs-lookup"><span data-stu-id="5a365-201">\<NAME></span></span> | <span data-ttu-id="5a365-202">El nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="5a365-202">The name of the migration.</span></span> |

<span data-ttu-id="5a365-203">Opciones:</span><span class="sxs-lookup"><span data-stu-id="5a365-203">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5a365-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="5a365-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="5a365-205"><nobr>--salida dir \<ruta de acceso ></nobr></span><span class="sxs-lookup"><span data-stu-id="5a365-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="5a365-206">El directorio (y subespacio de nombres) a utilizar.</span><span class="sxs-lookup"><span data-stu-id="5a365-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="5a365-207">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="5a365-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="5a365-208">El valor predeterminado es "Migraciones".</span><span class="sxs-lookup"><span data-stu-id="5a365-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="5a365-209">lista de las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="5a365-209">dotnet ef migrations list</span></span>

<span data-ttu-id="5a365-210">Migraciones de listas disponibles.</span><span class="sxs-lookup"><span data-stu-id="5a365-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="5a365-211">quitar las migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="5a365-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="5a365-212">Quita la última migración.</span><span class="sxs-lookup"><span data-stu-id="5a365-212">Removes the last migration.</span></span>

<span data-ttu-id="5a365-213">Opciones:</span><span class="sxs-lookup"><span data-stu-id="5a365-213">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="5a365-214">-f</span><span class="sxs-lookup"><span data-stu-id="5a365-214">-f</span></span> | <span data-ttu-id="5a365-215">--forzar</span><span class="sxs-lookup"><span data-stu-id="5a365-215">--force</span></span> | <span data-ttu-id="5a365-216">No se comprueban para ver si se ha aplicado la migración a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5a365-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="5a365-217">script de migraciones de ef dotnet.</span><span class="sxs-lookup"><span data-stu-id="5a365-217">dotnet ef migrations script</span></span>

<span data-ttu-id="5a365-218">Genera un script SQL de migraciones.</span><span class="sxs-lookup"><span data-stu-id="5a365-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="5a365-219">Argumentos:</span><span class="sxs-lookup"><span data-stu-id="5a365-219">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="5a365-220">\<FROM></span><span class="sxs-lookup"><span data-stu-id="5a365-220">\<FROM></span></span> | <span data-ttu-id="5a365-221">La migración inicial.</span><span class="sxs-lookup"><span data-stu-id="5a365-221">The starting migration.</span></span> <span data-ttu-id="5a365-222">El valor predeterminado es 0 (la base de datos inicial).</span><span class="sxs-lookup"><span data-stu-id="5a365-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="5a365-223">\<TO></span><span class="sxs-lookup"><span data-stu-id="5a365-223">\<TO></span></span>   | <span data-ttu-id="5a365-224">La migración final.</span><span class="sxs-lookup"><span data-stu-id="5a365-224">The ending migration.</span></span> <span data-ttu-id="5a365-225">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="5a365-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="5a365-226">Opciones:</span><span class="sxs-lookup"><span data-stu-id="5a365-226">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="5a365-227">-o</span><span class="sxs-lookup"><span data-stu-id="5a365-227">-o</span></span> | <span data-ttu-id="5a365-228">--salida \<archivo ></span><span class="sxs-lookup"><span data-stu-id="5a365-228">--output \<FILE></span></span> | <span data-ttu-id="5a365-229">Archivo que se escribirá el resultado.</span><span class="sxs-lookup"><span data-stu-id="5a365-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="5a365-230">-i</span><span class="sxs-lookup"><span data-stu-id="5a365-230">-i</span></span> | <span data-ttu-id="5a365-231">--idempotente</span><span class="sxs-lookup"><span data-stu-id="5a365-231">--idempotent</span></span>     | <span data-ttu-id="5a365-232">Generar un script que se puede usar en una base de datos en cualquier migración.</span><span class="sxs-lookup"><span data-stu-id="5a365-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
