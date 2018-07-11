---
title: Actualización de EF Core 1.0 RC2 a RTM - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 9561eac253517188251fece9a03f434482246051
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949062"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Actualización de EF Core 1.0 RC2 a RTM

Este artículo proporcionan instrucciones para mover una aplicación compilada con los paquetes de RC2 a 1.0.0 RTM.

## <a name="package-versions"></a>Versiones del paquete

Los nombres de los paquetes de nivel superior que normalmente instalaría en una aplicación no cambió de RC2 a RTM.

**Es preciso actualizar los paquetes instalados a las versiones RTM:**

* Los paquetes en tiempo de ejecución (por ejemplo, `Microsoft.EntityFrameworkCore.SqlServer`) ha cambiado de `1.0.0-rc2-final` a `1.0.0`.

* El `Microsoft.EntityFrameworkCore.Tools` paquete cambió de `1.0.0-preview1-final` a `1.0.0-preview2-final`. Tenga en cuenta que las herramientas son aún una versión preliminar.

## <a name="existing-migrations-may-need-maxlength-added"></a>Las migraciones existentes que necesite maxLength agregado

En RC2, la definición de columna en una migración parecía `table.Column<string>(nullable: true)` y la longitud de la columna se buscó en algunos metadatos se almacenan en el código subyacente de la migración. En RTM, la longitud ahora se incluye en el código con scaffolding `table.Column<string>(maxLength: 450, nullable: true)`.

Las migraciones existentes que se ha aplicado scaffolding antes de usar RTM no tendrán el `maxLength` argumento especificado. Esto significa que se utilizará la longitud máxima admitida por la base de datos (`nvarchar(max)` en SQL Server). Esto puede ser adecuado para algunas columnas, pero las columnas que forman parte de una clave, clave externa, o índice deben actualizarse para incluir una longitud máxima. Por convención, 450 es la longitud máxima se usa para las claves, claves externas y las columnas indizadas. Si ha configurado explícitamente una longitud en el modelo, use esa longitud en su lugar.

**ASP.NET Identity**

Este cambio afecta a los proyectos que usan ASP.NET Identity y se crearon a partir de un pre-RTM de la plantilla de proyecto. La plantilla de proyecto incluye una migración que se usa para crear la base de datos. Esta migración debe modificarse para especificar una longitud máxima de `256` para las columnas siguientes.

*  **AspNetRoles**

    * nombre

    * NormalizedName

*  **AspNetUsers**

   * Correo electrónico

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Si no se realiza este cambio se producirá en la siguiente excepción cuando la migración inicial se aplica a una base de datos.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: Quitar "importaciones" en project.json

Si estuviesen destinadas a .NET Core con RC2, es necesario agregar `imports` a project.json como solución temporal para algunas de las dependencias de EF Core no ofrece compatibilidad con .NET Standard. Ahora puede quitar.

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
> A partir de la versión 1.0 RTM, el [SDK de .NET Core](https://www.microsoft.com/net/download/core) ya no es compatible con `project.json` o desarrollar aplicaciones .NET Core con Visual Studio 2015. Se recomienda [migrar de project.json a csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Si utiliza Visual Studio, se recomienda que actualice a [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP: Agregar redirecciones de enlace

Intentando ejecutar comandos de EF en los proyectos de plataforma Universal de Windows (UWP) en el siguiente error:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Deberá agregar manualmente las redirecciones de enlace para el proyecto de UWP. Cree un archivo denominado `App.config` en el proyecto de la carpeta raíz y agregar redirecciones a las versiones de ensamblado correcto.

``` xml
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
