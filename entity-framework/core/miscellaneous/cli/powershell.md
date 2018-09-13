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
<a name="ef-core-package-manager-console-tools"></a>Herramientas de la consola de administrador de paquetes de EF Core
=====================================
Las herramientas de la consola de administrador de paquetes (PMC) de EF Core se ejecutan dentro de Visual Studio mediante de NuGet [Package Manager Console][2].
Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.

> [!TIP]
> ¿No se usa Visual Studio? El [herramientas de línea de comandos de EF Core] [ 1] son multiplataforma y se ejecutan dentro de un símbolo del sistema.

<a name="installing-the-tools"></a>Instalación de las herramientas
--------------------
Instalar las herramientas de la consola de administrador de paquetes de EF Core al instalar el paquete Microsoft.EntityFrameworkCore.Tools NuGet.
Puede instalarlo ejecutando el siguiente comando en [Package Manager Console][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Si todo funcionó correctamente, debe ser capaz de ejecutar este comando:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Si su proyecto de inicio tiene como destino .NET Standard, [entre-destino un marco de trabajo admitida] [ 3] antes de usar las herramientas.

> [!IMPORTANT]
> Si usas **Windows Universal** o **Xamarin**, mueva el código EF a una biblioteca de clases .NET Standard y [entre-destino un marco de trabajo admitida] [ 3] antes de usar las herramientas. Especifique la biblioteca de clases como su proyecto de inicio.

<a name="using-the-tools"></a>Uso de las herramientas
---------------
Cada vez que se invoca un comando, intervienen dos proyectos:

El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan). El valor predeterminado es el proyecto de destino para la **proyecto predeterminado** seleccionado en la consola de administrador de paquetes, pero también se puede especificar mediante-parámetro de proyecto.

El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto. El valor predeterminado es uno **establecer como proyecto de inicio** en el Explorador de soluciones. También se puede especificar mediante el parámetro - proyecto de inicio.

Parámetros comunes:

|                           |                             |
|:--------------------------|:----------------------------|
| -Contexto \<cadena >        | La clase DbContext usar.       |
| -Proyecto \<cadena >        | El proyecto para usar.         |
| Proyecto de inicio - \<cadena > | Para usar el proyecto de inicio. |
| -Verbose                  | Mostrar resultados detallados.        |

Para mostrar información de ayuda sobre un comando, use PowerShell `Get-Help` comando.

> [!TIP]
> Los parámetros de contexto, el proyecto y el proyecto de inicio admiten la expansión de ficha.

> [!TIP]
> Establecer **env:ASPNETCORE_ENVIRONMENT** antes de ejecutar para especificar el entorno de ASP.NET Core.

<a name="commands"></a>Comandos
--------

### <a name="add-migration"></a>Add-Migration

Agrega una nueva migración.

Parámetros:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-Nombre*** \<cadena >             | El nombre de la migración.                                                                                       |
| <nobr>-OutputDir \<cadena ></nobr> | El directorio (y subespacio de nombres) para usar. Las rutas de acceso son relativas al directorio de proyecto. El valor predeterminado es "Migraciones". |

> [!NOTE]
> Los parámetros de **negrita** son necesarios y los de *cursiva* son posicionales.

### <a name="drop-database"></a>Quitar la base de datos

Quita la base de datos.

Parámetros:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Mostrar qué base de datos se quitarían, pero no colóquelo. |

### <a name="get-dbcontext"></a>Get-DbContext

Obtiene información sobre un tipo de DbContext.

### <a name="remove-migration"></a>Remove-Migration

Quita la última migración.

Parámetros:

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Revertir la migración si se ha aplicado a la base de datos. |

### <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Aplica la técnica scaffolding un tipos de entidad y DbContext para una base de datos.

Parámetros:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Conexión*** \<cadena ></nobr> | La cadena de conexión a la base de datos.                                                           |
| ***-Proveedor*** \<cadena >                | El proveedor debe usar. (por ejemplo, Microsoft.EntityFrameworkCore.SqlServer)                      |
| -OutputDir \<cadena >                     | Para poner los archivos en el directorio. Las rutas de acceso son relativas al directorio de proyecto.                      |
| -ContextDir \<cadena >                    | Colocar el archivo de DbContext en el directorio. Las rutas de acceso son relativas al directorio de proyecto.             |
| -Contexto \<cadena >                       | El nombre de la clase DbContext para generar.                                                           |
| -Esquemas \<String [] >                     | Los esquemas de tablas para generar tipos de entidad para.                                              |
| -Tablas \<String [] >                      | Para generar tipos de entidad para las tablas.                                                         |
| -DataAnnotations                         | Usar atributos para configurar el modelo (donde sea posible). Si se omite, se usa solo la API fluida. |
| -UseDatabaseNames                        | Usar nombres de tabla y columna directamente desde la base de datos.                                           |
| -Force                                   | Sobrescribir archivos existentes.                                                                        |

### <a name="script-migration"></a>Migración de scripts

Genera un script SQL de migraciones.

Parámetros:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-Desde* \<cadena > | La migración inicial. El valor predeterminado es 0 (la base de datos inicial).      |
| *-A* \<cadena >   | La migración final. De forma predeterminada a la última migración.              |
| -Idempotente       | Generar un script que se puede usar en una base de datos en cualquier migración. |
| -Output \<cadena > | Para escribir el resultado en el archivo.                                   |

> [!TIP]
> El To, From, y los parámetros de salida admiten la expansión de ficha.

### <a name="update-database"></a>Actualizar base de datos

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*-Migración* \<cadena ></nobr> | La migración de destino. Si es "0", se revertirán todas las migraciones. De forma predeterminada a la última migración. |

> [!TIP]
> El parámetro de la migración es compatible con la expansión de ficha.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
