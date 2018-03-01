---
title: Consola de administrador de paquetes (Visual Studio) - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: aacf8c8564a3966db6202c9ff1c1c02a19a10814
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="19bb6-102">Herramientas de la consola de administrador de paquetes de EF Core</span><span class="sxs-lookup"><span data-stu-id="19bb6-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="19bb6-103">Las herramientas de la consola de administrador de paquete (PMC) de EF Core se ejecutan dentro de Visual Studio mediante NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="19bb6-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="19bb6-104">Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="19bb6-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="19bb6-105">¿No usa Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="19bb6-105">Not using Visual Studio?</span></span> <span data-ttu-id="19bb6-106">El [herramientas de línea de comandos principales de EF] [ 1] son multiplataforma y se ejecutan dentro de un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="19bb6-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="19bb6-107">Instalación de las herramientas</span><span class="sxs-lookup"><span data-stu-id="19bb6-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="19bb6-108">Instalar las herramientas de la consola de administrador de paquetes de núcleo de EF al instalar el paquete de Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="19bb6-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="19bb6-109">Puede instalarlo ejecutando el siguiente comando en [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="19bb6-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="19bb6-110">Si todo lo que funcionó correctamente, debe ser capaz de ejecutar este comando:</span><span class="sxs-lookup"><span data-stu-id="19bb6-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="19bb6-111">Si su proyecto de inicio tiene como destino .NET estándar, [entre-destino un marco de trabajo admitida] [ 3] antes de usar las herramientas.</span><span class="sxs-lookup"><span data-stu-id="19bb6-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="19bb6-112">Si usas **Universal de Windows** o **Xamarin**, mueva el código EF a una biblioteca de clases .NET estándar y [entre-destino un marco de trabajo admitida] [ 3] antes de usar las herramientas.</span><span class="sxs-lookup"><span data-stu-id="19bb6-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="19bb6-113">Especifique la biblioteca de clases como su proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="19bb6-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="19bb6-114">Uso de las herramientas</span><span class="sxs-lookup"><span data-stu-id="19bb6-114">Using the tools</span></span>
---------------
<span data-ttu-id="19bb6-115">Cada vez que se invoca un comando, existen dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="19bb6-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="19bb6-116">El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).</span><span class="sxs-lookup"><span data-stu-id="19bb6-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="19bb6-117">El proyecto de destino tiene como valor predeterminado el **proyecto predeterminado** seleccionado en la consola de administrador de paquetes, pero también puede especificarse mediante-parámetro de proyecto.</span><span class="sxs-lookup"><span data-stu-id="19bb6-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="19bb6-118">El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.</span><span class="sxs-lookup"><span data-stu-id="19bb6-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="19bb6-119">El valor predeterminado es una **establecer como proyecto de inicio** en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="19bb6-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="19bb6-120">También puede especificar mediante el parámetro - el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="19bb6-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="19bb6-121">Parámetros comunes:</span><span class="sxs-lookup"><span data-stu-id="19bb6-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="19bb6-122">-Context \<cadena ></span><span class="sxs-lookup"><span data-stu-id="19bb6-122">-Context \<String></span></span>        | <span data-ttu-id="19bb6-123">DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="19bb6-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="19bb6-124">-Proyecto \<cadena ></span><span class="sxs-lookup"><span data-stu-id="19bb6-124">-Project \<String></span></span>        | <span data-ttu-id="19bb6-125">El proyecto debe usar.</span><span class="sxs-lookup"><span data-stu-id="19bb6-125">The project to use.</span></span>         |
| <span data-ttu-id="19bb6-126">El proyecto de inicio - \<cadena ></span><span class="sxs-lookup"><span data-stu-id="19bb6-126">-StartupProject \<String></span></span> | <span data-ttu-id="19bb6-127">Proyecto de inicio que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="19bb6-127">The startup project to use.</span></span> |
| <span data-ttu-id="19bb6-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="19bb6-128">-Verbose</span></span>                  | <span data-ttu-id="19bb6-129">Mostrar resultados detallados.</span><span class="sxs-lookup"><span data-stu-id="19bb6-129">Show verbose output.</span></span>        |

<span data-ttu-id="19bb6-130">Para mostrar información de ayuda sobre un comando, use PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="19bb6-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="19bb6-131">Los parámetros de contexto, el proyecto y el proyecto de inicio admiten la expansión de pestañas.</span><span class="sxs-lookup"><span data-stu-id="19bb6-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="19bb6-132">Establecer **env:ASPNETCORE_ENVIRONMENT** antes de ejecutar para especificar el entorno de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="19bb6-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="19bb6-133">Comandos</span><span class="sxs-lookup"><span data-stu-id="19bb6-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="19bb6-134">Agregar la migración</span><span class="sxs-lookup"><span data-stu-id="19bb6-134">Add-Migration</span></span>

<span data-ttu-id="19bb6-135">Agrega una nueva migración.</span><span class="sxs-lookup"><span data-stu-id="19bb6-135">Adds a new migration.</span></span>

<span data-ttu-id="19bb6-136">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="19bb6-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19bb6-137">***-Nombre*** \<cadena ></span><span class="sxs-lookup"><span data-stu-id="19bb6-137">***-Name*** \<String></span></span>             | <span data-ttu-id="19bb6-138">El nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="19bb6-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="19bb6-139"><nobr>-OutputDir \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="19bb6-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="19bb6-140">El directorio (y subespacio de nombres) a utilizar.</span><span class="sxs-lookup"><span data-stu-id="19bb6-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="19bb6-141">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="19bb6-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="19bb6-142">El valor predeterminado es "Migraciones".</span><span class="sxs-lookup"><span data-stu-id="19bb6-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="19bb6-143">Parámetros de **negrita** son necesarios y los de *cursiva* son posicionales.</span><span class="sxs-lookup"><span data-stu-id="19bb6-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="19bb6-144">Quitar la base de datos</span><span class="sxs-lookup"><span data-stu-id="19bb6-144">Drop-Database</span></span>

<span data-ttu-id="19bb6-145">Quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="19bb6-145">Drops the database.</span></span>

<span data-ttu-id="19bb6-146">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="19bb6-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="19bb6-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="19bb6-147">-WhatIf</span></span> | <span data-ttu-id="19bb6-148">Mostrar qué base de datos se quitarían, pero no se quita.</span><span class="sxs-lookup"><span data-stu-id="19bb6-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="19bb6-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="19bb6-149">Get-DbContext</span></span>

<span data-ttu-id="19bb6-150">Obtiene información sobre un tipo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="19bb6-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="19bb6-151">Quitar la migración</span><span class="sxs-lookup"><span data-stu-id="19bb6-151">Remove-Migration</span></span>

<span data-ttu-id="19bb6-152">Quita la última migración.</span><span class="sxs-lookup"><span data-stu-id="19bb6-152">Removes the last migration.</span></span>

<span data-ttu-id="19bb6-153">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="19bb6-153">Parameters:</span></span>

|        |                                                                       |
|:-------|:----------------------------------------------------------------------|
| <span data-ttu-id="19bb6-154">-Force</span><span class="sxs-lookup"><span data-stu-id="19bb6-154">-Force</span></span> | <span data-ttu-id="19bb6-155">No se comprueban para ver si se ha aplicado la migración a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="19bb6-155">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="19bb6-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="19bb6-156">Scaffold-DbContext</span></span>

<span data-ttu-id="19bb6-157">Scaffolds un tipos de entidad y DbContext para una base de datos.</span><span class="sxs-lookup"><span data-stu-id="19bb6-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="19bb6-158">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="19bb6-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="19bb6-159"><nobr>***-Conexión*** \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="19bb6-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="19bb6-160">La cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="19bb6-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="19bb6-161">***-Proveedor*** \<cadena ></span><span class="sxs-lookup"><span data-stu-id="19bb6-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="19bb6-162">El proveedor debe usar.</span><span class="sxs-lookup"><span data-stu-id="19bb6-162">The provider to use.</span></span> <span data-ttu-id="19bb6-163">(P. ej.</span><span class="sxs-lookup"><span data-stu-id="19bb6-163">(E.g.</span></span> <span data-ttu-id="19bb6-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="19bb6-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="19bb6-165">-OutputDir \<cadena ></span><span class="sxs-lookup"><span data-stu-id="19bb6-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="19bb6-166">El directorio para colocar archivos.</span><span class="sxs-lookup"><span data-stu-id="19bb6-166">The directory to put files in.</span></span> <span data-ttu-id="19bb6-167">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="19bb6-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="19bb6-168">-Context \<cadena ></span><span class="sxs-lookup"><span data-stu-id="19bb6-168">-Context \<String></span></span>                       | <span data-ttu-id="19bb6-169">El nombre de DbContext para generar.</span><span class="sxs-lookup"><span data-stu-id="19bb6-169">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="19bb6-170">-Esquemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="19bb6-170">-Schemas \<String[]></span></span>                     | <span data-ttu-id="19bb6-171">Los esquemas de tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="19bb6-171">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="19bb6-172">-Tablas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="19bb6-172">-Tables \<String[]></span></span>                      | <span data-ttu-id="19bb6-173">Las tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="19bb6-173">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="19bb6-174">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="19bb6-174">-DataAnnotations</span></span>                         | <span data-ttu-id="19bb6-175">Usar atributos para configurar el modelo (siempre que sea posible).</span><span class="sxs-lookup"><span data-stu-id="19bb6-175">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="19bb6-176">Si se omite, se usa solo la API fluida.</span><span class="sxs-lookup"><span data-stu-id="19bb6-176">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="19bb6-177">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="19bb6-177">-UseDatabaseNames</span></span>                        | <span data-ttu-id="19bb6-178">Usar nombres de tabla y columna directamente desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="19bb6-178">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="19bb6-179">-Force</span><span class="sxs-lookup"><span data-stu-id="19bb6-179">-Force</span></span>                                   | <span data-ttu-id="19bb6-180">Sobrescribir archivos existentes.</span><span class="sxs-lookup"><span data-stu-id="19bb6-180">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="19bb6-181">Migración de scripts</span><span class="sxs-lookup"><span data-stu-id="19bb6-181">Script-Migration</span></span>

<span data-ttu-id="19bb6-182">Genera un script SQL de migraciones.</span><span class="sxs-lookup"><span data-stu-id="19bb6-182">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="19bb6-183">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="19bb6-183">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="19bb6-184">*-Desde* \<cadena ></span><span class="sxs-lookup"><span data-stu-id="19bb6-184">*-From* \<String></span></span> | <span data-ttu-id="19bb6-185">La migración inicial.</span><span class="sxs-lookup"><span data-stu-id="19bb6-185">The starting migration.</span></span> <span data-ttu-id="19bb6-186">El valor predeterminado es 0 (la base de datos inicial).</span><span class="sxs-lookup"><span data-stu-id="19bb6-186">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="19bb6-187">*-* \<Cadena ></span><span class="sxs-lookup"><span data-stu-id="19bb6-187">*-To* \<String></span></span>   | <span data-ttu-id="19bb6-188">La migración final.</span><span class="sxs-lookup"><span data-stu-id="19bb6-188">The ending migration.</span></span> <span data-ttu-id="19bb6-189">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="19bb6-189">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="19bb6-190">-Idempotente</span><span class="sxs-lookup"><span data-stu-id="19bb6-190">-Idempotent</span></span>       | <span data-ttu-id="19bb6-191">Generar un script que se puede usar en una base de datos en cualquier migración.</span><span class="sxs-lookup"><span data-stu-id="19bb6-191">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="19bb6-192">-Output \<cadena ></span><span class="sxs-lookup"><span data-stu-id="19bb6-192">-Output \<String></span></span> | <span data-ttu-id="19bb6-193">Archivo que se escribirá el resultado.</span><span class="sxs-lookup"><span data-stu-id="19bb6-193">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="19bb6-194">To, From, y los parámetros de salida admiten la expansión de pestañas.</span><span class="sxs-lookup"><span data-stu-id="19bb6-194">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="19bb6-195">Actualizar base de datos</span><span class="sxs-lookup"><span data-stu-id="19bb6-195">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="19bb6-196"><nobr>*-Migración* \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="19bb6-196"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="19bb6-197">La migración de destino.</span><span class="sxs-lookup"><span data-stu-id="19bb6-197">The target migration.</span></span> <span data-ttu-id="19bb6-198">Si es '0', se revertirán todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="19bb6-198">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="19bb6-199">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="19bb6-199">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="19bb6-200">El parámetro de migración es compatible con la expansión de pestañas.</span><span class="sxs-lookup"><span data-stu-id="19bb6-200">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
