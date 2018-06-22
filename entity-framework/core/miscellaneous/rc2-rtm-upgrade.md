---
title: Actualización de EF principales 1.0 RC2 a RTM - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 4bb4c5736708413f6581cad250b089b7bc22a559
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2018
ms.locfileid: "30151045"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>Actualización de EF Core 1.0 RC2 a RTM

Este artículo proporcionan instrucciones para mover una aplicación compilada con los paquetes de RC2 en 1.0.0 RTM.

## <a name="package-versions"></a>Versiones del paquete

Los nombres de los paquetes de nivel superior que instala normalmente en una aplicación no cambió de RC2 y RTM.

**Debe actualizar los paquetes instalados a las versiones RTM:**

* Paquetes en tiempo de ejecución (por ejemplo, `Microsoft.EntityFrameworkCore.SqlServer`) cambió de `1.0.0-rc2-final` a `1.0.0`.

* El `Microsoft.EntityFrameworkCore.Tools` paquete cambió de `1.0.0-preview1-final` a `1.0.0-preview2-final`. Tenga en cuenta que las herramientas es aún una versión preliminar.

## <a name="existing-migrations-may-need-maxlength-added"></a>Migraciones existentes que tenga maxLength agregado

En RC2, la definición de columna en una migración de aspecto `table.Column<string>(nullable: true)` y se busca la longitud de la columna en algunos metadatos se almacena en el código subyacente de la migración. En RTM, la longitud se incluye ahora en el código con scaffolding `table.Column<string>(maxLength: 450, nullable: true)`.

Las migraciones existentes que se scaffolding antes de usar RTM no tendrán el `maxLength` argumento especificado. Esto significa que se usará la longitud máxima admitida por la base de datos (`nvarchar(max)` en SQL Server). Esto puede ser un problema para algunas columnas, pero las columnas que forman parte de una clave, clave externa, o índice deben actualizarse para incluir una longitud máxima. Por convención, 450 es la longitud máxima usado para las claves, claves externas y las columnas indizadas. Si ha configurado explícitamente una longitud en el modelo, use esa longitud en su lugar.

**Identidad de ASP.NET**

Este cambio afecta a proyectos que usan ASP.NET Identity y se crearon a partir un pre-RTM plantilla de proyecto. La plantilla de proyecto incluye una migración que se utiliza para crear la base de datos. Esta migración debe modificarse para especificar una longitud máxima de `256` para las columnas siguientes.

*  **AspNetRoles**

    * nombre

    * NormalizedName

*  **AspNetUsers**

   * Correo electrónico

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Si no se realiza este cambio se producirá la excepción siguiente cuando la migración inicial se aplica a una base de datos.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: Quitar "importaciones" en project.json

Si se establece como destino .NET Core con RC2, había que agregar `imports` a project.json como solución temporal para algunas de las dependencias del núcleo de EF no son compatibles con .NET estándar. Ahora puede quitar.

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
> A partir de la versión 1.0 RTM, la [.NET Core SDK](https://www.microsoft.com/net/download/core) ya no es compatible con `project.json` o desarrollar aplicaciones de .NET Core con Visual Studio 2015. Se recomienda [migrar de project.json a csproj](https://docs.microsoft.com/dotnet/articles/core/migration/). Si se utiliza Visual Studio, se recomienda actualizar a [2017 de Visual Studio](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP: Agregar redirecciones de enlace

Si tratas de ejecutar comandos EF en resultados de proyectos de plataforma Universal de Windows (UWP) en el siguiente error:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

Debe agregar manualmente las redirecciones de enlace para el proyecto de UWP. Cree un archivo denominado `App.config` en el proyecto de la carpeta raíz y agrega redirecciones a las versiones de ensamblado correcto.

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
