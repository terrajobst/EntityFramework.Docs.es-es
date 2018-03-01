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
<a name="ef-core-net-command-line-tools"></a>Herramientas de línea de comandos de EF Core .NET
===============================
Las herramientas de línea de comandos de .NET de Entity Framework Core son una extensión a la plataforma entre **dotnet** comando, que forma parte de la [.NET Core SDK][2].

> [!TIP]
> Si está utilizando Visual Studio, se recomienda [las herramientas PMC] [ 1] en su lugar, ya que proporcionan una experiencia más integrada.

<a name="installing-the-tools"></a>Instalación de las herramientas
--------------------
Instalar las herramientas de línea de comandos de .NET de EF Core siguiendo estos pasos:

1. Edite el archivo de proyecto y agregar Microsoft.EntityFrameworkCore.Tools.DotNet como un elemento de DotNetCliToolReference (ver abajo)
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
> Una referencia de paquete con `PrivateAssets="All"` significa que no está expuesto a los proyectos que hacen referencia a este proyecto, que es especialmente útil para los paquetes que normalmente se utilizan solo durante el desarrollo.

Si lo quitó correctamente todos los pasos correctamente, debe poder ejecutar correctamente el comando siguiente en un símbolo del sistema.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Uso de las herramientas
---------------
Cada vez que se invoca un comando, existen dos proyectos:

El proyecto de destino es donde se agregan los archivos (o en algunos casos se quitan). El proyecto de destino tiene como valor predeterminado para el proyecto en el directorio actual, pero se puede cambiar mediante la <nobr> **--proyecto** </nobr> opción.

El proyecto de inicio es el que emulan las herramientas cuando se ejecuta el código del proyecto. También tiene como valor predeterminado para el proyecto en el directorio actual pero se puede cambiar mediante la **: proyecto de inicio** opción.

Opciones comunes:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | --json                           | Mostrar la salida JSON.           |
| -c | --contexto \<DBCONTEXT >           | DbContext para usar.       |
| -p | --proyecto \<proyecto >             | El proyecto debe usar.         |
| -s | --proyecto de inicio \<proyecto >     | Proyecto de inicio que se va a usar. |
|    | --framework \<FRAMEWORK >         | El marco de destino.       |
|    | --configuración \<configuración > | La configuración que se va a usar.   |
|    | --en tiempo de ejecución \<identificador >          | El runtime que se va a usar.         |
| -h | --Ayuda                           | Mostrar información de ayuda.      |
| -v | --detallado                        | Mostrar resultados detallados.        |
|    | --no-color                       | No colorear la salida.      |
|    | --salida de prefijo                  | Prefijo con el nivel de salida.   |


> [!TIP]
> Para especificar el entorno de ASP.NET Core, establezca el **ASPNETCORE_ENVIRONMENT** variable de entorno antes de ejecutar.

<a name="commands"></a>Comandos
--------

### <a name="dotnet-ef-database-drop"></a>eliminación de base de datos de ef dotnet.

Quita la base de datos.

Opciones:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | --forzar   | No realiza esta confirmación.                                           |
|    | ---simulacro | Mostrar qué base de datos se quitarían, pero no se quita. |

### <a name="dotnet-ef-database-update"></a>actualización de base de datos de ef dotnet.

Actualiza la base de datos para una migración especificada.

Argumentos:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| \<MIGRACIÓN > | La migración de destino. Si es 0, se revertirán todas las migraciones. De forma predeterminada a la última migración. |

### <a name="dotnet-ef-dbcontext-info"></a>información de dbcontext ef dotnet.

Obtiene información sobre un tipo de DbContext.

### <a name="dotnet-ef-dbcontext-list"></a>lista de dbcontext ef dotnet.

Enumera los tipos de DbContext disponibles.

### <a name="dotnet-ef-dbcontext-scaffold"></a>scaffolding de dbcontext ef dotnet.

Scaffolds un tipos de entidad y DbContext para una base de datos.

Argumentos:

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| \<CONEXIÓN > | La cadena de conexión a la base de datos.                              |
| \<PROVEEDOR >   | El proveedor debe usar. (P. ej. Microsoft.EntityFrameworkCore.SqlServer) |

Opciones:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | --data-annotations                      | Usar atributos para configurar el modelo (siempre que sea posible). Si se omite, se usa solo la API fluida. |
| -c              | --contexto \<nombre >                       | El nombre de DbContext.                                                                       |
| -f              | --forzar                                 | Sobrescribir archivos existentes.                                                                        |
| -o              | --salida-dir \<ruta de acceso >                    | El directorio para colocar archivos. Las rutas de acceso son relativas al directorio de proyecto.                      |
|                 | <nobr>--schema \<SCHEMA_NAME>...</nobr> | Los esquemas de tablas para generar tipos de entidad para.                                              |
| -t              | --tabla \<nombre_tabla >...                | Las tablas para generar tipos de entidad para.                                                         |
|                 | --utilizar nombres de base de datos                    | Usar nombres de tabla y columna directamente desde la base de datos.                                           |

### <a name="dotnet-ef-migrations-add"></a>agregar las migraciones de ef dotnet.

Agrega una nueva migración.

Argumentos:

|         |                            |
|:--------|:---------------------------|
| \<NOMBRE > | El nombre de la migración. |

Opciones:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>--salida dir \<ruta de acceso ></nobr> | El directorio (y subespacio de nombres) a utilizar. Las rutas de acceso son relativas al directorio de proyecto. El valor predeterminado es "Migraciones". |

### <a name="dotnet-ef-migrations-list"></a>lista de las migraciones de ef dotnet.

Migraciones de listas disponibles.

### <a name="dotnet-ef-migrations-remove"></a>quitar las migraciones de ef dotnet.

Quita la última migración.

Opciones:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | --forzar | No se comprueban para ver si se ha aplicado la migración a la base de datos. |

### <a name="dotnet-ef-migrations-script"></a>script de migraciones de ef dotnet.

Genera un script SQL de migraciones.

Argumentos:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| \<FROM> | La migración inicial. El valor predeterminado es 0 (la base de datos inicial). |
| \<TO>   | La migración final. De forma predeterminada a la última migración.         |

Opciones:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | --salida \<archivo > | Archivo que se escribirá el resultado.                                   |
| -i | --idempotente     | Generar un script que se puede usar en una base de datos en cualquier migración. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
