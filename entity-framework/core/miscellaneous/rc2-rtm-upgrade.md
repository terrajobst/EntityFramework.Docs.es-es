---
title: Actualización de EF Core 1,0 RC2 a RTM-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: e7f121d18931e26e7b5d11842da6da4a9b789efe
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181361"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="35424-102">Actualización de EF Core 1,0 RC2 a RTM</span><span class="sxs-lookup"><span data-stu-id="35424-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="35424-103">En este artículo se proporcionan instrucciones para mover una aplicación compilada con los paquetes de RC2 a 1.0.0 RTM.</span><span class="sxs-lookup"><span data-stu-id="35424-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="35424-104">Versiones de paquetes</span><span class="sxs-lookup"><span data-stu-id="35424-104">Package Versions</span></span>

<span data-ttu-id="35424-105">Los nombres de los paquetes de nivel superior que se instalarían normalmente en una aplicación no cambiaban entre RC2 y RTM.</span><span class="sxs-lookup"><span data-stu-id="35424-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="35424-106">**Debe actualizar los paquetes instalados a las versiones de RTM:**</span><span class="sxs-lookup"><span data-stu-id="35424-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="35424-107">Los paquetes en tiempo de ejecución (por ejemplo, `Microsoft.EntityFrameworkCore.SqlServer`) cambiaron de `1.0.0-rc2-final` a `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="35424-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="35424-108">El paquete `Microsoft.EntityFrameworkCore.Tools` cambió de `1.0.0-preview1-final` a `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="35424-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="35424-109">Tenga en cuenta que las herramientas siguen siendo versiones preliminares.</span><span class="sxs-lookup"><span data-stu-id="35424-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="35424-110">Es posible que las migraciones existentes necesiten maxLength agregadas</span><span class="sxs-lookup"><span data-stu-id="35424-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="35424-111">En RC2, se buscó en la definición de columna de una migración como `table.Column<string>(nullable: true)` y la longitud de la columna en algunos metadatos que almacenamos en el código subyacente a la migración.</span><span class="sxs-lookup"><span data-stu-id="35424-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="35424-112">En RTM, la longitud se incluye ahora en el código con scaffolding `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="35424-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="35424-113">Las migraciones existentes con scaffolding antes de usar RTM no tendrán el argumento `maxLength` especificado.</span><span class="sxs-lookup"><span data-stu-id="35424-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="35424-114">Esto significa que se usará la longitud máxima admitida por la base de datos (`nvarchar(max)` en SQL Server).</span><span class="sxs-lookup"><span data-stu-id="35424-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="35424-115">Esto puede ser adecuado para algunas columnas, pero las columnas que forman parte de una clave, clave externa o índice deben actualizarse para incluir una longitud máxima.</span><span class="sxs-lookup"><span data-stu-id="35424-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="35424-116">Por Convención, 450 es la longitud máxima utilizada para las claves, las claves externas y las columnas indizadas.</span><span class="sxs-lookup"><span data-stu-id="35424-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="35424-117">Si ha configurado explícitamente una longitud en el modelo, debe utilizar esa longitud en su lugar.</span><span class="sxs-lookup"><span data-stu-id="35424-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="35424-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="35424-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="35424-119">Este cambio afecta a los proyectos que usan ASP.NET Identity y que se crearon a partir de una plantilla de proyecto anterior a RTM.</span><span class="sxs-lookup"><span data-stu-id="35424-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="35424-120">La plantilla de proyecto incluye una migración que se usa para crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="35424-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="35424-121">Esta migración se debe editar para especificar una longitud máxima de `256` para las columnas siguientes.</span><span class="sxs-lookup"><span data-stu-id="35424-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="35424-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="35424-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="35424-123">Name</span><span class="sxs-lookup"><span data-stu-id="35424-123">Name</span></span>

    * <span data-ttu-id="35424-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="35424-124">NormalizedName</span></span>

*  <span data-ttu-id="35424-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="35424-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="35424-126">Correo electrónico</span><span class="sxs-lookup"><span data-stu-id="35424-126">Email</span></span>

   * <span data-ttu-id="35424-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="35424-127">NormalizedEmail</span></span>

   * <span data-ttu-id="35424-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="35424-128">NormalizedUserName</span></span>

   * <span data-ttu-id="35424-129">UserName</span><span class="sxs-lookup"><span data-stu-id="35424-129">UserName</span></span>

<span data-ttu-id="35424-130">Si no se realiza este cambio, se producirá la siguiente excepción cuando la migración inicial se aplique a una base de datos.</span><span class="sxs-lookup"><span data-stu-id="35424-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

```console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="35424-131">.NET Core: Quitar "Imports" en Project. JSON</span><span class="sxs-lookup"><span data-stu-id="35424-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="35424-132">Si el destino era .NET Core con RC2, era necesario agregar `imports` a Project. JSON como una solución temporal para algunas de EF Core dependencias de que no admiten .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="35424-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="35424-133">Ahora se pueden quitar.</span><span class="sxs-lookup"><span data-stu-id="35424-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> <span data-ttu-id="35424-134">A partir de la versión 1,0 RTM, el [SDK de .net Core](https://www.microsoft.com/net/download/core) ya no admite `project.json` ni el desarrollo de aplicaciones .net Core con Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="35424-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="35424-135">Se recomienda [migrar de project.json a csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="35424-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="35424-136">Si usa Visual Studio, se recomienda que actualice a [visual studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="35424-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="35424-137">UWP: Agregar redirecciones de enlace</span><span class="sxs-lookup"><span data-stu-id="35424-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="35424-138">Al intentar ejecutar los comandos EF en los proyectos de Plataforma universal de Windows (UWP) se produce el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="35424-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

```console
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

<span data-ttu-id="35424-139">Debe agregar manualmente redirecciones de enlace al proyecto de UWP.</span><span class="sxs-lookup"><span data-stu-id="35424-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="35424-140">Cree un archivo denominado `App.config` en la carpeta raíz del proyecto y agregue redireccionamientos a las versiones de ensamblado correctas.</span><span class="sxs-lookup"><span data-stu-id="35424-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

```xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
