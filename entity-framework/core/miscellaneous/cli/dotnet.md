---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995302"
---
<a name="ef-core-net-command-line-tools"></a>Herramientas de línea de comandos de EF Core .NET
===============================
Las herramientas de línea de comandos de Entity Framework Core .NET son una extensión multiplataforma **dotnet** comando, que forma parte de la [SDK de .NET Core][2].

> [!TIP]
> Si usa Visual Studio, se recomienda [las herramientas de PMC] [ 1] en su lugar ya que proporcionan una experiencia más integrada.

<a name="installing-the-tools"></a>Instalación de las herramientas
--------------------
> [!NOTE]
> El SDK de .NET Core 2.1.300 versión más reciente incluye **dotnet ef** comandos que son compatibles con EF Core 2.0 y versiones posteriores. Por lo tanto, si usa las versiones recientes de .NET Core SDK y el tiempo de ejecución de EF Core, se requiere ninguna instalación y puede omitir el resto de esta sección.
>
> Por otro lado, el **dotnet ef** herramienta contenida en la versión del SDK de .NET Core 2.1.300 y versiones más reciente no es compatible con la versión de EF Core 1.0 y 1.1. Antes de que puede trabajar con un proyecto que usa estas versiones anteriores de EF Core en un equipo que tiene el SDK de .NET Core 2.1.300 o posterior, instalado, también debe instalar la versión 2.1.200 o una versión anterior del SDK y configurar la aplicación para que use esa versión anterior mediante la modificación de sus  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) archivo. Este archivo normalmente se incluye en el directorio de la solución (uno por encima del proyecto). A continuación, puede continuar con las instrucciones de instalación siguientes.

Las versiones anteriores del SDK de .NET Core, puede instalar las herramientas de línea de comandos de .NET de EF Core siguiendo estos pasos:

1. Edite el archivo de proyecto y agregue Microsoft.EntityFrameworkCore.Tools.DotNet como un elemento DotNetCliToolReference (ver abajo)
2. Ejecute los comandos siguientes:

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

El proyecto resultante debe tener un aspecto similar al siguiente:

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
> Una referencia de paquete con `PrivateAssets="All"` significa que no se expone a los proyectos que hacen referencia a este proyecto, que es especialmente útil para los paquetes que normalmente se usan solo durante el desarrollo.

Si hizo todo correctamente, debería poder ejecutar correctamente el siguiente comando en un símbolo del sistema.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Uso de las herramientas
---------------
Cada vez que se invoca un comando, intervienen dos proyectos:

El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan). El proyecto de destino tiene como valor predeterminado para el proyecto en el directorio actual, pero se puede cambiar mediante la <nobr> **--proyecto** </nobr> opción.

El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto. También tiene como valor predeterminado para el proyecto en el directorio actual, pero se puede cambiar mediante la **: proyecto de inicio** opción.

> [!NOTE]
> Por ejemplo, actualizando la base de datos de la aplicación web que tenga instalado en un proyecto diferente de EF Core tendría este aspecto: `dotnet ef database update --project {project-path}` (desde el directorio de aplicación web)

Opciones comunes:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | --json                           | Mostrar la salida JSON.           |
| -c | --contexto \<DBCONTEXT >           | La clase DbContext usar.       |
| -p | --proyecto \<proyecto >             | El proyecto para usar.         |
| -s | --startup-project \<proyecto >     | Para usar el proyecto de inicio. |
|    | --framework \<FRAMEWORK >         | La plataforma de destino.       |
|    | --configuración \<configuración > | La configuración que se va a usar.   |
|    | --en tiempo de ejecución \<identificador >          | El runtime que se va a usar.         |
| -h | --Ayuda                           | Mostrar información de ayuda.      |
| -v | --verbose                        | Mostrar resultados detallados.        |
|    | --Sin color                       | No colorear salida.      |
|    | --salida de prefijo                  | Prefijo con el nivel de salida.   |


> [!TIP]
> Para especificar el entorno de ASP.NET Core, establezca el **ASPNETCORE_ENVIRONMENT** variable de entorno antes de ejecutar.

<a name="commands"></a>Comandos
--------

### <a name="dotnet-ef-database-drop"></a>colocación de base de datos de ef de dotnet

Quita la base de datos.

Opciones:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | --forzar   | No se confirme.                                           |
|    | --dry-run | Mostrar qué base de datos se quitarían, pero no colóquelo. |

### <a name="dotnet-ef-database-update"></a>actualización de base de datos de ef de dotnet

Actualiza la base de datos para una migración especificada.

Argumentos:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| \<MIGRACIÓN &GT; | La migración de destino. Si es 0, se revertirán todas las migraciones. De forma predeterminada a la última migración. |

### <a name="dotnet-ef-dbcontext-info"></a>información de dotnet ef dbcontext

Obtiene información sobre un tipo de DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>lista de dbcontext de ef de dotnet

Enumera los tipos de DbContext disponibles.

### <a name="dotnet-ef-dbcontext-scaffold"></a>scaffolding de dbcontext de ef de dotnet

Aplica la técnica scaffolding un tipos de entidad y DbContext para una base de datos.

Argumentos:

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| \<CONEXIÓN &GT; | La cadena de conexión a la base de datos.                                      |
| \<PROVEEDOR &GT;   | El proveedor debe usar. (por ejemplo, Microsoft.EntityFrameworkCore.SqlServer) |

Opciones:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | --anotaciones de datos                      | Usar atributos para configurar el modelo (donde sea posible). Si se omite, se usa solo la API fluida. |
| -c              | --contexto \<nombre >                       | El nombre de la clase DbContext.                                                                       |
|                 | --contexto-dir \<ruta de acceso >                   | Colocar el archivo de DbContext en el directorio. Las rutas de acceso son relativas al directorio de proyecto.             |
| -f              | --forzar                                 | Sobrescribir archivos existentes.                                                                        |
| -o              | --salida-dir \<ruta de acceso >                    | Para poner los archivos en el directorio. Las rutas de acceso son relativas al directorio de proyecto.                      |
|                 | <nobr>--esquema \<SCHEMA_NAME >...</nobr> | Los esquemas de tablas para generar tipos de entidad para.                                              |
| -t              | --tabla \<TABLE_NAME >...                | Para generar tipos de entidad para las tablas.                                                         |
|                 | --utilizar nombres de base de datos                    | Usar nombres de tabla y columna directamente desde la base de datos.                                           |

### <a name="dotnet-ef-migrations-add"></a>Agregar migraciones de ef de dotnet

Agrega una nueva migración.

Argumentos:

|         |                            |
|:--------|:---------------------------|
| \<NOMBRE &GT; | El nombre de la migración. |

Opciones:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>--salida-dir \<ruta de acceso ></nobr> | El directorio (y subespacio de nombres) para usar. Las rutas de acceso son relativas al directorio de proyecto. El valor predeterminado es "Migraciones". |

### <a name="dotnet-ef-migrations-list"></a>lista de las migraciones de ef de dotnet

Migraciones de listas disponibles.

### <a name="dotnet-ef-migrations-remove"></a>quitar las migraciones de ef de dotnet

Quita la última migración.

Opciones:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | --forzar | Revertir la migración si se ha aplicado a la base de datos. |

### <a name="dotnet-ef-migrations-script"></a>secuencia de comandos de dotnet ef migrations

Genera un script SQL de migraciones.

Argumentos:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| \<DESDE &GT; | La migración inicial. El valor predeterminado es 0 (la base de datos inicial). |
| \<PARA &GT;   | La migración final. De forma predeterminada a la última migración.         |

Opciones:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | --salida \<archivo > | Para escribir el resultado en el archivo.                                   |
| -i | --idempotente     | Generar un script que se puede usar en una base de datos en cualquier migración. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
