---
title: Consola del Administrador de paquetes (Visual Studio) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: 3d57a1665da2c94c55981d17e041b0ef74282496
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490860"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="300ca-102">Herramientas de la consola de administrador de paquetes de EF Core</span><span class="sxs-lookup"><span data-stu-id="300ca-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="300ca-103">Las herramientas de la consola de administrador de paquetes (PMC) de EF Core se ejecutan dentro de Visual Studio mediante de NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="300ca-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="300ca-104">Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="300ca-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="300ca-105">¿No se usa Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="300ca-105">Not using Visual Studio?</span></span> <span data-ttu-id="300ca-106">El [herramientas de línea de comandos de EF Core] [ 1] son multiplataforma y se ejecutan dentro de un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="300ca-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="300ca-107">Instalación de las herramientas</span><span class="sxs-lookup"><span data-stu-id="300ca-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="300ca-108">Instalar las herramientas de la consola de administrador de paquetes de EF Core al instalar el paquete Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="300ca-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="300ca-109">Puede instalarlo ejecutando el siguiente comando en [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="300ca-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="300ca-110">Si todo funcionó correctamente, debe ser capaz de ejecutar este comando:</span><span class="sxs-lookup"><span data-stu-id="300ca-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="300ca-111">Si su proyecto de inicio tiene como destino .NET Standard, [entre-destino un marco de trabajo admitida] [ 3] antes de usar las herramientas.</span><span class="sxs-lookup"><span data-stu-id="300ca-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="300ca-112">Si usas **Windows Universal** o **Xamarin**, mueva el código EF a una biblioteca de clases .NET Standard y [entre-destino un marco de trabajo admitida] [ 3] antes de usar las herramientas.</span><span class="sxs-lookup"><span data-stu-id="300ca-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="300ca-113">Especifique la biblioteca de clases como su proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="300ca-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="300ca-114">Uso de las herramientas</span><span class="sxs-lookup"><span data-stu-id="300ca-114">Using the tools</span></span>
---------------
<span data-ttu-id="300ca-115">Cada vez que se invoca un comando, intervienen dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="300ca-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="300ca-116">El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).</span><span class="sxs-lookup"><span data-stu-id="300ca-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="300ca-117">El valor predeterminado es el proyecto de destino para la **proyecto predeterminado** seleccionado en la consola de administrador de paquetes, pero también se puede especificar mediante-parámetro de proyecto.</span><span class="sxs-lookup"><span data-stu-id="300ca-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="300ca-118">El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.</span><span class="sxs-lookup"><span data-stu-id="300ca-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="300ca-119">El valor predeterminado es uno **establecer como proyecto de inicio** en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="300ca-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="300ca-120">También se puede especificar mediante el parámetro - proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="300ca-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="300ca-121">Parámetros comunes:</span><span class="sxs-lookup"><span data-stu-id="300ca-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="300ca-122">-Contexto \<cadena ></span><span class="sxs-lookup"><span data-stu-id="300ca-122">-Context \<String></span></span>        | <span data-ttu-id="300ca-123">La clase DbContext usar.</span><span class="sxs-lookup"><span data-stu-id="300ca-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="300ca-124">-Proyecto \<cadena ></span><span class="sxs-lookup"><span data-stu-id="300ca-124">-Project \<String></span></span>        | <span data-ttu-id="300ca-125">El proyecto para usar.</span><span class="sxs-lookup"><span data-stu-id="300ca-125">The project to use.</span></span>         |
| <span data-ttu-id="300ca-126">Proyecto de inicio - \<cadena ></span><span class="sxs-lookup"><span data-stu-id="300ca-126">-StartupProject \<String></span></span> | <span data-ttu-id="300ca-127">Para usar el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="300ca-127">The startup project to use.</span></span> |
| <span data-ttu-id="300ca-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="300ca-128">-Verbose</span></span>                  | <span data-ttu-id="300ca-129">Mostrar resultados detallados.</span><span class="sxs-lookup"><span data-stu-id="300ca-129">Show verbose output.</span></span>        |

<span data-ttu-id="300ca-130">Para mostrar información de ayuda sobre un comando, use PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="300ca-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="300ca-131">Los parámetros de contexto, el proyecto y el proyecto de inicio admiten la expansión de ficha.</span><span class="sxs-lookup"><span data-stu-id="300ca-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="300ca-132">Establecer **env:ASPNETCORE_ENVIRONMENT** antes de ejecutar para especificar el entorno de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="300ca-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="300ca-133">Comandos</span><span class="sxs-lookup"><span data-stu-id="300ca-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="300ca-134">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="300ca-134">Add-Migration</span></span>

<span data-ttu-id="300ca-135">Agrega una nueva migración.</span><span class="sxs-lookup"><span data-stu-id="300ca-135">Adds a new migration.</span></span>

<span data-ttu-id="300ca-136">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="300ca-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="300ca-137">***-Nombre*** \<cadena ></span><span class="sxs-lookup"><span data-stu-id="300ca-137">***-Name*** \<String></span></span>             | <span data-ttu-id="300ca-138">El nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="300ca-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="300ca-139"><nobr>-OutputDir \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="300ca-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="300ca-140">El directorio (y subespacio de nombres) para usar.</span><span class="sxs-lookup"><span data-stu-id="300ca-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="300ca-141">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="300ca-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="300ca-142">El valor predeterminado es "Migraciones".</span><span class="sxs-lookup"><span data-stu-id="300ca-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="300ca-143">Los parámetros de **negrita** son necesarios y los de *cursiva* son posicionales.</span><span class="sxs-lookup"><span data-stu-id="300ca-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="300ca-144">Quitar la base de datos</span><span class="sxs-lookup"><span data-stu-id="300ca-144">Drop-Database</span></span>

<span data-ttu-id="300ca-145">Quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="300ca-145">Drops the database.</span></span>

<span data-ttu-id="300ca-146">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="300ca-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="300ca-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="300ca-147">-WhatIf</span></span> | <span data-ttu-id="300ca-148">Mostrar qué base de datos se quitarían, pero no colóquelo.</span><span class="sxs-lookup"><span data-stu-id="300ca-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="300ca-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="300ca-149">Get-DbContext</span></span>

<span data-ttu-id="300ca-150">Obtiene información sobre un tipo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="300ca-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="300ca-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="300ca-151">Remove-Migration</span></span>

<span data-ttu-id="300ca-152">Quita la última migración.</span><span class="sxs-lookup"><span data-stu-id="300ca-152">Removes the last migration.</span></span>

<span data-ttu-id="300ca-153">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="300ca-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="300ca-154">-Force</span><span class="sxs-lookup"><span data-stu-id="300ca-154">-Force</span></span> | <span data-ttu-id="300ca-155">Revertir la migración si se ha aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="300ca-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="300ca-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="300ca-156">Scaffold-DbContext</span></span>

<span data-ttu-id="300ca-157">Aplica la técnica scaffolding un tipos de entidad y DbContext para una base de datos.</span><span class="sxs-lookup"><span data-stu-id="300ca-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="300ca-158">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="300ca-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="300ca-159"><nobr>***-Conexión*** \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="300ca-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="300ca-160">La cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="300ca-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="300ca-161">***-Proveedor*** \<cadena ></span><span class="sxs-lookup"><span data-stu-id="300ca-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="300ca-162">El proveedor debe usar.</span><span class="sxs-lookup"><span data-stu-id="300ca-162">The provider to use.</span></span> <span data-ttu-id="300ca-163">(por ejemplo, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="300ca-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="300ca-164">-OutputDir \<cadena ></span><span class="sxs-lookup"><span data-stu-id="300ca-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="300ca-165">Para poner los archivos en el directorio.</span><span class="sxs-lookup"><span data-stu-id="300ca-165">The directory to put files in.</span></span> <span data-ttu-id="300ca-166">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="300ca-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="300ca-167">-ContextDir \<cadena ></span><span class="sxs-lookup"><span data-stu-id="300ca-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="300ca-168">Colocar el archivo de DbContext en el directorio.</span><span class="sxs-lookup"><span data-stu-id="300ca-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="300ca-169">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="300ca-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="300ca-170">-Contexto \<cadena ></span><span class="sxs-lookup"><span data-stu-id="300ca-170">-Context \<String></span></span>                       | <span data-ttu-id="300ca-171">El nombre de la clase DbContext para generar.</span><span class="sxs-lookup"><span data-stu-id="300ca-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="300ca-172">-Esquemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="300ca-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="300ca-173">Los esquemas de tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="300ca-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="300ca-174">-Tablas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="300ca-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="300ca-175">Para generar tipos de entidad para las tablas.</span><span class="sxs-lookup"><span data-stu-id="300ca-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="300ca-176">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="300ca-176">-DataAnnotations</span></span>                         | <span data-ttu-id="300ca-177">Usar atributos para configurar el modelo (donde sea posible).</span><span class="sxs-lookup"><span data-stu-id="300ca-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="300ca-178">Si se omite, se usa solo la API fluida.</span><span class="sxs-lookup"><span data-stu-id="300ca-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="300ca-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="300ca-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="300ca-180">Usar nombres de tabla y columna directamente desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="300ca-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="300ca-181">-Force</span><span class="sxs-lookup"><span data-stu-id="300ca-181">-Force</span></span>                                   | <span data-ttu-id="300ca-182">Sobrescribir archivos existentes.</span><span class="sxs-lookup"><span data-stu-id="300ca-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="300ca-183">Migración de scripts</span><span class="sxs-lookup"><span data-stu-id="300ca-183">Script-Migration</span></span>

<span data-ttu-id="300ca-184">Genera un script SQL de migraciones.</span><span class="sxs-lookup"><span data-stu-id="300ca-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="300ca-185">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="300ca-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="300ca-186">*-Desde* \<cadena ></span><span class="sxs-lookup"><span data-stu-id="300ca-186">*-From* \<String></span></span> | <span data-ttu-id="300ca-187">La migración inicial.</span><span class="sxs-lookup"><span data-stu-id="300ca-187">The starting migration.</span></span> <span data-ttu-id="300ca-188">El valor predeterminado es 0 (la base de datos inicial).</span><span class="sxs-lookup"><span data-stu-id="300ca-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="300ca-189">*-A* \<cadena ></span><span class="sxs-lookup"><span data-stu-id="300ca-189">*-To* \<String></span></span>   | <span data-ttu-id="300ca-190">La migración final.</span><span class="sxs-lookup"><span data-stu-id="300ca-190">The ending migration.</span></span> <span data-ttu-id="300ca-191">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="300ca-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="300ca-192">-Idempotente</span><span class="sxs-lookup"><span data-stu-id="300ca-192">-Idempotent</span></span>       | <span data-ttu-id="300ca-193">Generar un script que se puede usar en una base de datos en cualquier migración.</span><span class="sxs-lookup"><span data-stu-id="300ca-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="300ca-194">-Output \<cadena ></span><span class="sxs-lookup"><span data-stu-id="300ca-194">-Output \<String></span></span> | <span data-ttu-id="300ca-195">Para escribir el resultado en el archivo.</span><span class="sxs-lookup"><span data-stu-id="300ca-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="300ca-196">El To, From, y los parámetros de salida admiten la expansión de ficha.</span><span class="sxs-lookup"><span data-stu-id="300ca-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="300ca-197">Actualizar base de datos</span><span class="sxs-lookup"><span data-stu-id="300ca-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="300ca-198"><nobr>*-Migración* \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="300ca-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="300ca-199">La migración de destino.</span><span class="sxs-lookup"><span data-stu-id="300ca-199">The target migration.</span></span> <span data-ttu-id="300ca-200">Si es "0", se revertirán todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="300ca-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="300ca-201">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="300ca-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="300ca-202">El parámetro de la migración es compatible con la expansión de ficha.</span><span class="sxs-lookup"><span data-stu-id="300ca-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
