---
title: Consola de administrador de paquetes (Visual Studio) - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2018
---
<a name="ef-core-package-manager-console-tools"></a>Herramientas de la consola de administrador de paquetes de EF Core
=====================================
Las herramientas de la consola de administrador de paquete (PMC) de EF Core se ejecutan dentro de Visual Studio mediante NuGet [Package Manager Console][2].
Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.

> [!TIP]
> ¿No usa Visual Studio? El [herramientas de línea de comandos principales de EF] [ 1] son multiplataforma y se ejecutan dentro de un símbolo del sistema.

<a name="installing-the-tools"></a>Instalación de las herramientas
--------------------
Instalar las herramientas de la consola de administrador de paquetes de núcleo de EF al instalar el paquete de Microsoft.EntityFrameworkCore.Tools NuGet.
Puede instalarlo ejecutando el siguiente comando en [Package Manager Console][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Si todo lo que funcionó correctamente, debe ser capaz de ejecutar este comando:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Si su proyecto de inicio tiene como destino .NET estándar, [entre-destino un marco de trabajo admitida] [ 3] antes de usar las herramientas.

> [!IMPORTANT]
> Si usas **Universal de Windows** o **Xamarin**, mueva el código EF a una biblioteca de clases .NET estándar y [entre-destino un marco de trabajo admitida] [ 3] antes de usar las herramientas. Especifique la biblioteca de clases como su proyecto de inicio.

<a name="using-the-tools"></a>Uso de las herramientas
---------------
Cada vez que se invoca un comando, existen dos proyectos:

El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan). El proyecto de destino tiene como valor predeterminado el **proyecto predeterminado** seleccionado en la consola de administrador de paquetes, pero también puede especificarse mediante-parámetro de proyecto.

El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto. El valor predeterminado es una **establecer como proyecto de inicio** en el Explorador de soluciones. También puede especificar mediante el parámetro - el proyecto de inicio.

Parámetros comunes:

|                           |                             |
|:--------------------------|:----------------------------|
| -Context \<cadena >        | DbContext para usar.       |
| -Proyecto \<cadena >        | El proyecto debe usar.         |
| El proyecto de inicio - \<cadena > | Proyecto de inicio que se va a usar. |
| -Verbose                  | Mostrar resultados detallados.        |

Para mostrar información de ayuda sobre un comando, use PowerShell `Get-Help` comando.

> [!TIP]
> Los parámetros de contexto, el proyecto y el proyecto de inicio admiten la expansión de pestañas.

> [!TIP]
> Establecer **env:ASPNETCORE_ENVIRONMENT** antes de ejecutar para especificar el entorno de ASP.NET Core.

<a name="commands"></a>Comandos
--------

### <a name="add-migration"></a>Agregar la migración

Agrega una nueva migración.

Parámetros:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-Nombre*** \<cadena >             | El nombre de la migración.                                                                                       |
| <nobr>-OutputDir \<cadena ></nobr> | El directorio (y subespacio de nombres) a utilizar. Las rutas de acceso son relativas al directorio de proyecto. El valor predeterminado es "Migraciones". |

> [!NOTE]
> Parámetros de **negrita** son necesarios y los de *cursiva* son posicionales.

### <a name="drop-database"></a>Quitar la base de datos

Quita la base de datos.

Parámetros:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Mostrar qué base de datos se quitarían, pero no se quita. |

### <a name="get-dbcontext"></a>Get-DbContext

Obtiene información sobre un tipo de DbContext.

### <a name="remove-migration"></a>Quitar la migración

Quita la última migración.

Parámetros:

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Revertir la migración si se ha aplicado a la base de datos. |

### <a name="scaffold-dbcontext"></a>Scaffold DbContext

Scaffolds un tipos de entidad y DbContext para una base de datos.

Parámetros:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Conexión*** \<cadena ></nobr> | La cadena de conexión a la base de datos.                                                           |
| ***-Proveedor*** \<cadena >                | El proveedor debe usar. (P. ej., Microsoft.EntityFrameworkCore.SqlServer)                              |
| -OutputDir \<cadena >                     | El directorio para colocar archivos. Las rutas de acceso son relativas al directorio de proyecto.                      |
| -ContextDir \<cadena >                    | El directorio para colocar el archivo de DbContext en. Las rutas de acceso son relativas al directorio de proyecto.             |
| -Context \<cadena >                       | El nombre de DbContext para generar.                                                           |
| -Esquemas \<String [] >                     | Los esquemas de tablas para generar tipos de entidad para.                                              |
| -Tablas \<String [] >                      | Las tablas para generar tipos de entidad para.                                                         |
| -DataAnnotations                         | Usar atributos para configurar el modelo (siempre que sea posible). Si se omite, se usa solo la API fluida. |
| -UseDatabaseNames                        | Usar nombres de tabla y columna directamente desde la base de datos.                                           |
| -Force                                   | Sobrescribir archivos existentes.                                                                        |

### <a name="script-migration"></a>Migración de scripts

Genera un script SQL de migraciones.

Parámetros:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-Desde* \<cadena > | La migración inicial. El valor predeterminado es 0 (la base de datos inicial).      |
| *-* \<Cadena >   | La migración final. De forma predeterminada a la última migración.              |
| -Idempotente       | Generar un script que se puede usar en una base de datos en cualquier migración. |
| -Output \<cadena > | Archivo que se escribirá el resultado.                                   |

> [!TIP]
> To, From, y los parámetros de salida admiten la expansión de pestañas.

### <a name="update-database"></a>Actualizar base de datos

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*-Migración* \<cadena ></nobr> | La migración de destino. Si es '0', se revertirán todas las migraciones. De forma predeterminada a la última migración. |

> [!TIP]
> El parámetro de migración es compatible con la expansión de pestañas.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
