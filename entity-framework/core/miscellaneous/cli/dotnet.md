---
title: Referencia de las herramientas EF Core (CLI) de .NET - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/20/2018
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: a280aad0344a89c41c30be27a249df3c28c44c70
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447175"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Referencia de las herramientas de Entity Framework Core - CLI de .NET

Las herramientas de interfaz de línea de comandos (CLI) para Entity Framework Core realizan tareas de desarrollo de tiempo de diseño. Por ejemplo, que crean [migraciones](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), aplicar las migraciones y generar código para un modelo basado en una base de datos existente. Los comandos son una extensión multiplataforma [dotnet](/dotnet/core/tools) comando, que forma parte de la [SDK de .NET Core](https://www.microsoft.com/net/core). Estas herramientas funcionan con proyectos de .NET Core.

Si usa Visual Studio, se recomienda la [herramientas de la consola de administrador de paquetes](powershell.md) en su lugar:
* Trabajan automáticamente con el proyecto actual seleccionado en el **Package Manager Console** sin necesidad de cambiar manualmente los directorios.
* Abran automáticamente los archivos generados por un comando una vez completado el comando.

## <a name="installing-the-tools"></a>Instalación de las herramientas

El procedimiento de instalación depende de la versión y tipo de proyecto:

* ASP.NET Core 2.1 y versiones posteriores
* EF Core 2.x
* EF Core 1.x

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 +

* Instalar actual [SDK de .NET Core](https://www.microsoft.com/net/download/core). El SDK debe estar instalado incluso si dispone de la versión más reciente de Visual Studio 2017.

  Esto es todo lo que es necesario para ASP.NET Core 2.1 + porque el `Microsoft.EntityFrameworkCore.Design` paquete se incluye en el [Microsoft.AspNetCore.App metapaquete](/aspnet/core/fundamentals/metapackage-app).

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2.x (no en ASP.NET Core)

El `dotnet ef` comandos están incluidos en el SDK de .NET Core, pero para habilitar los comandos tiene que instalar el `Microsoft.EntityFrameworkCore.Design` paquete.

* Instalar actual [SDK de .NET Core](https://www.microsoft.com/net/download/core). El SDK debe estar instalado incluso si dispone de la versión más reciente de Visual Studio 2017.

* Instalar el estable más reciente `Microsoft.EntityFrameworkCore.Design` paquete. 

  ``` Console   
  dotnet add package Microsoft.EntityFrameworkCore.Design   
  ```

### <a name="ef-core-1x"></a>EF Core 1.x

* Instale el SDK de .NET Core versión 2.1.200. Las versiones posteriores no son compatibles con herramientas de la CLI de EF Core 1.0 y 1.1.

* Configurar la aplicación para uso el 2.1.200 SDK versión modificando su [global.json](/dotnet/core/tools/global-json) archivo. Este archivo normalmente se incluye en el directorio de la solución (uno por encima del proyecto). 

* Edite el archivo de proyecto y agregue `Microsoft.EntityFrameworkCore.Tools.DotNet` como un `DotNetCliToolReference` elemento. Especifique la última versión 1.x, por ejemplo: 1.1.6. Vea el ejemplo de archivo de proyecto al final de esta sección.

* Instale la última versión 1.x de la `Microsoft.EntityFrameworkCore.Design` empaquetar, por ejemplo:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Con ambas referencias de paquete que agregan, el archivo de proyecto tiene un aspecto similar al siguiente:

  ``` xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
      <OutputType>Exe</OutputType>
      <TargetFramework>netcoreapp1.1</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
      <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                        Version="1.1.6"
                         PrivateAssets="All" />
    </ItemGroup>
    <ItemGroup>
       <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                              Version="1.1.6" />
    </ItemGroup>
  </Project>
  ```

  Una referencia de paquete con `PrivateAssets="All"` no se expone a los proyectos que hacen referencia a este proyecto. Esta restricción es especialmente útil para los paquetes que normalmente se usan solo durante el desarrollo.

### <a name="verify-installation"></a>Comprobar la instalación

Ejecute los comandos siguientes para comprobar que se han instalado correctamente herramientas de CLI de EF Core:

  ``` Console
  dotnet restore
  dotnet ef
  ```

La salida del comando identifica la versión de las herramientas de uso:

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

Entity Framework Core .NET Command-line Tools 2.1.3-rtm-32065

<Usage documentation follows, not shown.>
```

## <a name="using-the-tools"></a>Uso de las herramientas

Antes de usar las herramientas, es posible que deba crear un proyecto de inicio o establecer el entorno.

### <a name="target-project-and-startup-project"></a>Proyecto de destino y proyecto de inicio

Los comandos que hacen referencia a un *proyecto* y un *proyecto de inicio*.

* El *proyecto* también es conocido como el *proyecto de destino* porque es donde los comandos Agregar o quitar archivos. De forma predeterminada, el proyecto en el directorio actual es el proyecto de destino. Puede especificar otro proyecto como proyecto de destino mediante el <nobr> `--project` </nobr> opción.

* El *proyecto de inicio* es lo que las herramientas de compilación y ejecutan. Tienen las herramientas ejecutar código de la aplicación en tiempo de diseño para obtener información sobre el proyecto, como la cadena de conexión de base de datos y la configuración del modelo. De forma predeterminada, el proyecto en el directorio actual es el proyecto de inicio. Puede especificar otro proyecto como proyecto de inicio mediante el <nobr> `--startup-project` </nobr> opción.

El proyecto de inicio y el proyecto de destino suelen ser el mismo proyecto. Un escenario típico, donde son proyectos independientes es cuando:

* Las clases de entidad y el contexto de EF Core están en una biblioteca de clases de .NET Core.
* Una aplicación de consola .NET Core o una aplicación web hace referencia a la biblioteca de clases.

También es posible [colocar código de migraciones en una biblioteca de clases independiente desde el contexto de EF Core](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Otros marcos de destino

Las herramientas CLI funcionan con proyectos de .NET Core y proyectos de .NET Framework. Las aplicaciones que tienen el modelo de EF Core en una biblioteca de clases .NET Standard que no tenga un proyecto de .NET Framework o .NET Core. Por ejemplo, esto es cierto de aplicaciones de Xamarin y plataforma Universal de Windows. En tales casos, puede crear un proyecto de aplicación de consola .NET Core cuyo único propósito es actuar como proyecto de inicio para las herramientas. El proyecto puede ser un proyecto ficticio sin código real &mdash; solo es necesario proporcionar un destino para las herramientas.

¿Por qué es un proyecto ficticio necesario? Como se mencionó anteriormente, tienen las herramientas ejecutar código de la aplicación en tiempo de diseño. Para ello, deben usar el runtime de .NET Core. Cuando el modelo de EF Core está en un proyecto destinado a .NET Core o .NET Framework, las herramientas de EF Core pedir prestado el tiempo de ejecución del proyecto. No puede hacerlo si el modelo de EF Core está en una biblioteca de clases .NET Standard. .NET Standard no es una implementación real. NET; es una especificación de un conjunto de API que las implementaciones de .NET deben admitir. Por lo tanto, .NET Standard no es suficiente para las herramientas de EF Core ejecutar código de la aplicación. El proyecto ficticio creado para usar como proyecto de inicio proporciona una plataforma de destino concreta en la que las herramientas pueden cargar la biblioteca de clases .NET Standard. 

### <a name="aspnet-core-environment"></a>Entorno de ASP.NET Core

Para especificar el entorno para proyectos de ASP.NET Core, establezca el **ASPNETCORE_ENVIRONMENT** variable de entorno antes de ejecutar comandos.

## <a name="common-options"></a>Opciones comunes

|                   | Opción                             | Descripción                                                                                                                                                                                                                                                   |
|-------------------|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                           | Mostrar la salida JSON.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`            | La `DbContext` clase se debe utilizar. Nombre de clase completa con espacios de nombres o única.  Si se omite esta opción, EF Core encontrará la clase de contexto. Si hay varias clases de contexto, esta opción es necesaria.                                            |
| `-p`              | `--project <PROJECT>`              | Ruta de acceso relativa a la carpeta de proyecto del proyecto de destino.  Valor predeterminado es la carpeta actual.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`      | Ruta de acceso relativa a la carpeta de proyecto del proyecto de inicio. Valor predeterminado es la carpeta actual.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`          | El [Moniker de la plataforma de destino](/dotnet/standard/frameworks#supported-target-framework-versions) para el [.NET framework de destino](/dotnet/standard/frameworks).  Se utiliza cuando el archivo de proyecto especifica varios marcos de destino, y desea seleccionar uno de ellos. |
|                   | `--configuration <CONFIGURATION>`  | La configuración de compilación, por ejemplo: `Debug` o `Release`.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`           | El identificador de restaurar los paquetes para el runtime de destino. Para obtener una lista de identificadores de tiempo de ejecución (RID), consulte el [catálogo de RID](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                           | Mostrar información de ayuda.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                        | Mostrar resultados detallados.                                                                                                                                                                                                                                          |
|                   | `--no-color`                       | No colorear salida.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                  | Prefijo con el nivel de salida.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>colocación de base de datos de ef de dotnet

Quita la base de datos.

Opciones:

|                   | Opción                   | Descripción                                                |
|-------------------|--------------------------|------------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | No se confirme.                                             |
|                   | <nobr>`--dry-run`</nobr> | Mostrar qué base de datos se quitarían, pero no colóquelo.   |

## <a name="dotnet-ef-database-update"></a>actualización de base de datos de ef de dotnet

Actualiza la base de datos a la última migración o a una migración especificada.

Argumentos:

| Argumento       | Descripción                                                                                                                                                                                                                                                     |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>`  | La migración de destino. Las migraciones pueden identificarse por su nombre o identificador. El número 0 es un caso especial que significa *antes de la primera migración* y hace que todas las migraciones se puede revertir. Si no se especifica ninguna migración, lo valores predeterminados del comando a la última migración. |

Los ejemplos siguientes actualización la base de datos para una migración especificada. El primero usa el nombre de la migración y la segunda usa el identificador de la migración:

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>información de dotnet ef dbcontext

Obtiene información sobre un `DbContext` tipo.

## <a name="dotnet-ef-dbcontext-list"></a>lista de dbcontext de ef de dotnet

Listas disponibles `DbContext` tipos.

## <a name="dotnet-ef-dbcontext-scaffold"></a>scaffolding de dbcontext de ef de dotnet

Genera código para un `DbContext` y tipos de entidad para una base de datos.

Argumentos:

| Argumento        | Descripción                                                                                                                                                                                                             |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>`  | La cadena de conexión a la base de datos. Para los proyectos de ASP.NET Core 2.x, el valor puede ser *nombre =\<nombre de cadena de conexión >*. En ese caso, el nombre procede de los orígenes de configuración que se configuran para el proyecto. |
| `<PROVIDER>`    | El proveedor debe usar. Normalmente, esto es el nombre del paquete de NuGet, por ejemplo: `Microsoft.EntityFrameworkCore.SqlServer`.                                                                                           |

Opciones:

|                   | Opción                                    | Descripción                                                                                                                                                                    |
|-------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr>   | `--data-annotations`                      | Usar atributos para configurar el modelo (donde sea posible). Si se omite esta opción, se usa solo la API fluida.                                                                |
| `-c`              | `--context <NAME>`                        | El nombre de la `DbContext` clase genere.                                                                                                                                 |
|                   | `--context-dir <PATH>`                    | El directorio para colocar el `DbContext` archivo de clase en. Las rutas de acceso son relativas al directorio de proyecto. Los espacios de nombres se derivan de los nombres de carpeta.                                 |
| `-f`              | `--force`                                 | Sobrescribir archivos existentes.                                                                                                                                                      |
| `-o`              | `--output-dir <PATH>`                     | Para poner los archivos de clase de entidad en el directorio. Las rutas de acceso son relativas al directorio de proyecto.                                                                                       |
|                   | <nobr>`--schema <SCHEMA_NAME>...`</nobr>  | Los esquemas de tablas para generar tipos de entidad para. Para especificar varios esquemas, repita `--schema` para cada uno de ellos. Si se omite esta opción, se incluyen todos los esquemas.          |
| `-t`              | `--table <TABLE_NAME>`...                 | Para generar tipos de entidad para las tablas. Para especificar varias tablas, repita `-t` o `--table` para cada uno de ellos. Si se omite esta opción, se incluyen todas las tablas.                |
|                   | `--use-database-names`                    | Usar nombres de tabla y columna exactamente como aparecen en la base de datos. Si se omite esta opción, se cambian los nombres de base de datos que mejor se ajuste a convenciones de estilo de nombre de C#. |

El ejemplo siguiente se aplica la técnica scaffolding todos los esquemas y tablas y coloca los nuevos archivos en el *modelos* carpeta.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

El ejemplo siguiente se aplica la técnica scaffolding únicamente las tablas seleccionadas y crea el contexto en una carpeta independiente con un nombre especificado:

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>Agregar migraciones de ef de dotnet

Agrega una nueva migración.

Argumentos:

| Argumento  | Descripción                  |
|-----------|------------------------------|
| `<NAME>`  | El nombre de la migración.   |

Opciones:

|                   | Opción                              | Descripción                                                                                                        |
|-------------------|-------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr>  | El directorio (y subespacio de nombres) para usar. Las rutas de acceso son relativas al directorio de proyecto. El valor predeterminado es "Migraciones".   |

## <a name="dotnet-ef-migrations-list"></a>lista de las migraciones de ef de dotnet

Migraciones de listas disponibles.

## <a name="dotnet-ef-migrations-remove"></a>quitar las migraciones de ef de dotnet

Quita la última migración (revierte los cambios de código que se realizaron para la migración). 

Opciones:

|                   | Opción    | Descripción                                                                        |
|-------------------|-----------|------------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Revertir la migración (revertir los cambios que se aplicaron a la base de datos).    |

## <a name="dotnet-ef-migrations-script"></a>secuencia de comandos de dotnet ef migrations

Genera un script SQL de migraciones.

Argumentos:

| Argumento  | Descripción                                                                                                                                                   |
|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>`  | La migración inicial. Las migraciones pueden identificarse por su nombre o identificador. El número 0 es un caso especial que significa *antes de la primera migración*. El valor predeterminado es 0. |
| `<TO>`    | La migración final. De forma predeterminada a la última migración.                                                                                                         |

Opciones:

|                   | Opción             | Descripción                                                          |
|-------------------|--------------------|----------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>`  | El archivo para escribir la secuencia de comandos.                                     |
| `-i`              | `--idempotent`     | Generar un script que se puede usar en una base de datos en cualquier migración.   |

El ejemplo siguiente crea una secuencia de comandos para la migración InitialCreate:

```console
dotnet ef migrations script 0 InitialCreate
```

El ejemplo siguiente crea un script para todas las migraciones después de la migración InitialCreate.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```