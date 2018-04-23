---
title: Consola de administrador de paquetes (Visual Studio) - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="bd73d-102">Herramientas de la consola de administrador de paquetes de EF Core</span><span class="sxs-lookup"><span data-stu-id="bd73d-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="bd73d-103">Las herramientas de la consola de administrador de paquete (PMC) de EF Core se ejecutan dentro de Visual Studio mediante NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="bd73d-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="bd73d-104">Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd73d-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="bd73d-105">¿No usa Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="bd73d-105">Not using Visual Studio?</span></span> <span data-ttu-id="bd73d-106">El [herramientas de línea de comandos principales de EF] [ 1] son multiplataforma y se ejecutan dentro de un símbolo del sistema.</span><span class="sxs-lookup"><span data-stu-id="bd73d-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="bd73d-107">Instalación de las herramientas</span><span class="sxs-lookup"><span data-stu-id="bd73d-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="bd73d-108">Instalar las herramientas de la consola de administrador de paquetes de núcleo de EF al instalar el paquete de Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="bd73d-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="bd73d-109">Puede instalarlo ejecutando el siguiente comando en [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="bd73d-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="bd73d-110">Si todo lo que funcionó correctamente, debe ser capaz de ejecutar este comando:</span><span class="sxs-lookup"><span data-stu-id="bd73d-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="bd73d-111">Si su proyecto de inicio tiene como destino .NET estándar, [entre-destino un marco de trabajo admitida] [ 3] antes de usar las herramientas.</span><span class="sxs-lookup"><span data-stu-id="bd73d-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bd73d-112">Si usas **Universal de Windows** o **Xamarin**, mueva el código EF a una biblioteca de clases .NET estándar y [entre-destino un marco de trabajo admitida] [ 3] antes de usar las herramientas.</span><span class="sxs-lookup"><span data-stu-id="bd73d-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="bd73d-113">Especifique la biblioteca de clases como su proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="bd73d-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="bd73d-114">Uso de las herramientas</span><span class="sxs-lookup"><span data-stu-id="bd73d-114">Using the tools</span></span>
---------------
<span data-ttu-id="bd73d-115">Cada vez que se invoca un comando, existen dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="bd73d-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="bd73d-116">El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan).</span><span class="sxs-lookup"><span data-stu-id="bd73d-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="bd73d-117">El proyecto de destino tiene como valor predeterminado el **proyecto predeterminado** seleccionado en la consola de administrador de paquetes, pero también puede especificarse mediante-parámetro de proyecto.</span><span class="sxs-lookup"><span data-stu-id="bd73d-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="bd73d-118">El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bd73d-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="bd73d-119">El valor predeterminado es una **establecer como proyecto de inicio** en el Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="bd73d-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="bd73d-120">También puede especificar mediante el parámetro - el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="bd73d-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="bd73d-121">Parámetros comunes:</span><span class="sxs-lookup"><span data-stu-id="bd73d-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="bd73d-122">-Context \<cadena ></span><span class="sxs-lookup"><span data-stu-id="bd73d-122">-Context \<String></span></span>        | <span data-ttu-id="bd73d-123">DbContext para usar.</span><span class="sxs-lookup"><span data-stu-id="bd73d-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="bd73d-124">-Proyecto \<cadena ></span><span class="sxs-lookup"><span data-stu-id="bd73d-124">-Project \<String></span></span>        | <span data-ttu-id="bd73d-125">El proyecto debe usar.</span><span class="sxs-lookup"><span data-stu-id="bd73d-125">The project to use.</span></span>         |
| <span data-ttu-id="bd73d-126">El proyecto de inicio - \<cadena ></span><span class="sxs-lookup"><span data-stu-id="bd73d-126">-StartupProject \<String></span></span> | <span data-ttu-id="bd73d-127">Proyecto de inicio que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="bd73d-127">The startup project to use.</span></span> |
| <span data-ttu-id="bd73d-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="bd73d-128">-Verbose</span></span>                  | <span data-ttu-id="bd73d-129">Mostrar resultados detallados.</span><span class="sxs-lookup"><span data-stu-id="bd73d-129">Show verbose output.</span></span>        |

<span data-ttu-id="bd73d-130">Para mostrar información de ayuda sobre un comando, use PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="bd73d-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="bd73d-131">Los parámetros de contexto, el proyecto y el proyecto de inicio admiten la expansión de pestañas.</span><span class="sxs-lookup"><span data-stu-id="bd73d-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="bd73d-132">Establecer **env:ASPNETCORE_ENVIRONMENT** antes de ejecutar para especificar el entorno de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd73d-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="bd73d-133">Comandos</span><span class="sxs-lookup"><span data-stu-id="bd73d-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="bd73d-134">Agregar la migración</span><span class="sxs-lookup"><span data-stu-id="bd73d-134">Add-Migration</span></span>

<span data-ttu-id="bd73d-135">Agrega una nueva migración.</span><span class="sxs-lookup"><span data-stu-id="bd73d-135">Adds a new migration.</span></span>

<span data-ttu-id="bd73d-136">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="bd73d-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bd73d-137">***-Nombre*** \<cadena ></span><span class="sxs-lookup"><span data-stu-id="bd73d-137">***-Name*** \<String></span></span>             | <span data-ttu-id="bd73d-138">El nombre de la migración.</span><span class="sxs-lookup"><span data-stu-id="bd73d-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="bd73d-139"><nobr>-OutputDir \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="bd73d-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="bd73d-140">El directorio (y subespacio de nombres) a utilizar.</span><span class="sxs-lookup"><span data-stu-id="bd73d-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="bd73d-141">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="bd73d-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="bd73d-142">El valor predeterminado es "Migraciones".</span><span class="sxs-lookup"><span data-stu-id="bd73d-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="bd73d-143">Parámetros de **negrita** son necesarios y los de *cursiva* son posicionales.</span><span class="sxs-lookup"><span data-stu-id="bd73d-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="bd73d-144">Quitar la base de datos</span><span class="sxs-lookup"><span data-stu-id="bd73d-144">Drop-Database</span></span>

<span data-ttu-id="bd73d-145">Quita la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bd73d-145">Drops the database.</span></span>

<span data-ttu-id="bd73d-146">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="bd73d-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="bd73d-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="bd73d-147">-WhatIf</span></span> | <span data-ttu-id="bd73d-148">Mostrar qué base de datos se quitarían, pero no se quita.</span><span class="sxs-lookup"><span data-stu-id="bd73d-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="bd73d-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="bd73d-149">Get-DbContext</span></span>

<span data-ttu-id="bd73d-150">Obtiene información sobre un tipo de DbContext.</span><span class="sxs-lookup"><span data-stu-id="bd73d-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="bd73d-151">Quitar la migración</span><span class="sxs-lookup"><span data-stu-id="bd73d-151">Remove-Migration</span></span>

<span data-ttu-id="bd73d-152">Quita la última migración.</span><span class="sxs-lookup"><span data-stu-id="bd73d-152">Removes the last migration.</span></span>

<span data-ttu-id="bd73d-153">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="bd73d-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="bd73d-154">-Force</span><span class="sxs-lookup"><span data-stu-id="bd73d-154">-Force</span></span> | <span data-ttu-id="bd73d-155">Revertir la migración si se ha aplicado a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bd73d-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="bd73d-156">Scaffold DbContext</span><span class="sxs-lookup"><span data-stu-id="bd73d-156">Scaffold-DbContext</span></span>

<span data-ttu-id="bd73d-157">Scaffolds un tipos de entidad y DbContext para una base de datos.</span><span class="sxs-lookup"><span data-stu-id="bd73d-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="bd73d-158">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="bd73d-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bd73d-159"><nobr>***-Conexión*** \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="bd73d-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="bd73d-160">La cadena de conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bd73d-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="bd73d-161">***-Proveedor*** \<cadena ></span><span class="sxs-lookup"><span data-stu-id="bd73d-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="bd73d-162">El proveedor debe usar.</span><span class="sxs-lookup"><span data-stu-id="bd73d-162">The provider to use.</span></span> <span data-ttu-id="bd73d-163">(P. ej.,</span><span class="sxs-lookup"><span data-stu-id="bd73d-163">(E.g.</span></span> <span data-ttu-id="bd73d-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="bd73d-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="bd73d-165">-OutputDir \<cadena ></span><span class="sxs-lookup"><span data-stu-id="bd73d-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="bd73d-166">El directorio para colocar archivos.</span><span class="sxs-lookup"><span data-stu-id="bd73d-166">The directory to put files in.</span></span> <span data-ttu-id="bd73d-167">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="bd73d-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="bd73d-168">-ContextDir \<cadena ></span><span class="sxs-lookup"><span data-stu-id="bd73d-168">-ContextDir \<String></span></span>                    | <span data-ttu-id="bd73d-169">El directorio para colocar el archivo de DbContext en.</span><span class="sxs-lookup"><span data-stu-id="bd73d-169">The directory to put DbContext file in.</span></span> <span data-ttu-id="bd73d-170">Las rutas de acceso son relativas al directorio de proyecto.</span><span class="sxs-lookup"><span data-stu-id="bd73d-170">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="bd73d-171">-Context \<cadena ></span><span class="sxs-lookup"><span data-stu-id="bd73d-171">-Context \<String></span></span>                       | <span data-ttu-id="bd73d-172">El nombre de DbContext para generar.</span><span class="sxs-lookup"><span data-stu-id="bd73d-172">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="bd73d-173">-Esquemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="bd73d-173">-Schemas \<String[]></span></span>                     | <span data-ttu-id="bd73d-174">Los esquemas de tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="bd73d-174">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="bd73d-175">-Tablas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="bd73d-175">-Tables \<String[]></span></span>                      | <span data-ttu-id="bd73d-176">Las tablas para generar tipos de entidad para.</span><span class="sxs-lookup"><span data-stu-id="bd73d-176">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="bd73d-177">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="bd73d-177">-DataAnnotations</span></span>                         | <span data-ttu-id="bd73d-178">Usar atributos para configurar el modelo (siempre que sea posible).</span><span class="sxs-lookup"><span data-stu-id="bd73d-178">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="bd73d-179">Si se omite, se usa solo la API fluida.</span><span class="sxs-lookup"><span data-stu-id="bd73d-179">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="bd73d-180">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="bd73d-180">-UseDatabaseNames</span></span>                        | <span data-ttu-id="bd73d-181">Usar nombres de tabla y columna directamente desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bd73d-181">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="bd73d-182">-Force</span><span class="sxs-lookup"><span data-stu-id="bd73d-182">-Force</span></span>                                   | <span data-ttu-id="bd73d-183">Sobrescribir archivos existentes.</span><span class="sxs-lookup"><span data-stu-id="bd73d-183">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="bd73d-184">Migración de scripts</span><span class="sxs-lookup"><span data-stu-id="bd73d-184">Script-Migration</span></span>

<span data-ttu-id="bd73d-185">Genera un script SQL de migraciones.</span><span class="sxs-lookup"><span data-stu-id="bd73d-185">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="bd73d-186">Parámetros:</span><span class="sxs-lookup"><span data-stu-id="bd73d-186">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="bd73d-187">*-Desde* \<cadena ></span><span class="sxs-lookup"><span data-stu-id="bd73d-187">*-From* \<String></span></span> | <span data-ttu-id="bd73d-188">La migración inicial.</span><span class="sxs-lookup"><span data-stu-id="bd73d-188">The starting migration.</span></span> <span data-ttu-id="bd73d-189">El valor predeterminado es 0 (la base de datos inicial).</span><span class="sxs-lookup"><span data-stu-id="bd73d-189">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="bd73d-190">*-* \<Cadena ></span><span class="sxs-lookup"><span data-stu-id="bd73d-190">*-To* \<String></span></span>   | <span data-ttu-id="bd73d-191">La migración final.</span><span class="sxs-lookup"><span data-stu-id="bd73d-191">The ending migration.</span></span> <span data-ttu-id="bd73d-192">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="bd73d-192">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="bd73d-193">-Idempotente</span><span class="sxs-lookup"><span data-stu-id="bd73d-193">-Idempotent</span></span>       | <span data-ttu-id="bd73d-194">Generar un script que se puede usar en una base de datos en cualquier migración.</span><span class="sxs-lookup"><span data-stu-id="bd73d-194">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="bd73d-195">-Output \<cadena ></span><span class="sxs-lookup"><span data-stu-id="bd73d-195">-Output \<String></span></span> | <span data-ttu-id="bd73d-196">Archivo que se escribirá el resultado.</span><span class="sxs-lookup"><span data-stu-id="bd73d-196">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="bd73d-197">To, From, y los parámetros de salida admiten la expansión de pestañas.</span><span class="sxs-lookup"><span data-stu-id="bd73d-197">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="bd73d-198">Actualizar base de datos</span><span class="sxs-lookup"><span data-stu-id="bd73d-198">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="bd73d-199"><nobr>*-Migración* \<cadena ></nobr></span><span class="sxs-lookup"><span data-stu-id="bd73d-199"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="bd73d-200">La migración de destino.</span><span class="sxs-lookup"><span data-stu-id="bd73d-200">The target migration.</span></span> <span data-ttu-id="bd73d-201">Si es '0', se revertirán todas las migraciones.</span><span class="sxs-lookup"><span data-stu-id="bd73d-201">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="bd73d-202">De forma predeterminada a la última migración.</span><span class="sxs-lookup"><span data-stu-id="bd73d-202">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="bd73d-203">El parámetro de migración es compatible con la expansión de pestañas.</span><span class="sxs-lookup"><span data-stu-id="bd73d-203">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
