---
title: Referencia de las herramientas EF Core (consola de administrador de paquetes) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 9a57b58f8569ee1241e40c3809b03487d1d88e02
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834765"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Referencia: consola de administrador de paquetes en Visual Studio de las herramientas de Entity Framework Core

Las herramientas de la consola de administrador de paquetes (PMC) para Entity Framework Core realizan tareas de desarrollo de tiempo de diseño. Por ejemplo, que crean [migraciones](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), aplicar las migraciones y generar código para un modelo basado en una base de datos existente. Los comandos se ejecutan dentro de Visual Studio mediante el [Package Manager Console](/nuget/tools/package-manager-console). Estas herramientas funcionan con proyectos de .NET Framework y .NET Core.

Si no usa Visual Studio, se recomienda la [herramientas de línea de comandos de EF Core](dotnet.md) en su lugar. Las herramientas de CLI son multiplataforma y se ejecutan dentro de un símbolo del sistema.

## <a name="installing-the-tools"></a>Instalación de las herramientas

Los procedimientos para instalar y actualizar las herramientas difieren entre ASP.NET Core 2.1 + y versiones anteriores u otros tipos de proyecto.

### <a name="aspnet-core-version-21-and-later"></a>ASP.NET Core 2.1 y versiones posteriores

Las herramientas se incluyen automáticamente en un proyecto de ASP.NET Core 2.1 + porque el `Microsoft.EntityFrameworkCore.Tools` paquete se incluye en el [Microsoft.AspNetCore.App metapaquete](/aspnet/core/fundamentals/metapackage-app).

Por lo tanto, no tiene que hacer nada para instalar las herramientas, pero es necesario:
* Restaurar los paquetes antes de usar las herramientas en un proyecto nuevo.
* Instalar un paquete para actualizar las herramientas a una versión más reciente.

Para asegurarse de que está obteniendo la versión más reciente de las herramientas, se recomienda que también realice el paso siguiente:

* Editar su *.csproj* archivo y agregue una línea que especifica la versión más reciente de la [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) paquete. Por ejemplo, el *.csproj* archivo puede incluir un `ItemGroup` similar a este:

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

Actualizar las herramientas cuando reciba un mensaje similar al ejemplo siguiente:

> La versión de herramientas de EF Core '2.1.1-rtm-30846' es mayor que el tiempo de ejecución '2.1.3-rtm-32065'. Actualizar las herramientas para las últimas características y correcciones de errores.

Para actualizar las herramientas:
* Instale el SDK de .NET Core más reciente.
* Actualización a la versión más reciente de Visual Studio.
* Editar el *.csproj* de archivos para que incluya una referencia de paquete para el paquete de herramientas más reciente, como se mostró anteriormente.

### <a name="other-versions-and-project-types"></a>Otras versiones y tipos de proyecto

Instalar las herramientas de la consola de administrador de paquetes con el comando siguiente **Package Manager Console**:

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Actualizar las herramientas ejecutando el siguiente comando en **Package Manager Console**.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>Comprobar la instalación

Compruebe que las herramientas están instaladas, ejecute este comando:

``` powershell
Get-Help about_EntityFrameworkCore
```

El resultado será similar al siguiente (no le diga qué versión de las herramientas que se usa):

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a>Uso de las herramientas

Antes de usar las herramientas:
* Comprender la diferencia entre el proyecto de inicio y de destino.
* Obtenga información sobre cómo usar las herramientas con las bibliotecas de clases .NET Standard.
* Para los proyectos de ASP.NET Core, establezca el entorno.

### <a name="target-and-startup-project"></a>Proyecto de inicio y de destino

Los comandos que hacen referencia a un *proyecto* y un *proyecto de inicio*.

* El *proyecto* también es conocido como el *proyecto de destino* porque es donde los comandos Agregar o quitar archivos. De forma predeterminada, el **proyecto predeterminado** seleccionado en **Package Manager Console** es el proyecto de destino. Puede especificar otro proyecto como proyecto de destino mediante el <nobr> `--project` </nobr> opción.

* El *proyecto de inicio* es lo que las herramientas de compilación y ejecutan. Tienen las herramientas ejecutar código de la aplicación en tiempo de diseño para obtener información sobre el proyecto, como la cadena de conexión de base de datos y la configuración del modelo. De forma predeterminada, el **proyecto de inicio** en **el Explorador de soluciones** es el proyecto de inicio. Puede especificar otro proyecto como proyecto de inicio mediante el <nobr> `--startup-project` </nobr> opción.

El proyecto de inicio y el proyecto de destino suelen ser el mismo proyecto. Un escenario típico, donde son proyectos independientes es cuando:

* Las clases de entidad y el contexto de EF Core están en una biblioteca de clases de .NET Core.
* Una aplicación de consola .NET Core o una aplicación web hace referencia a la biblioteca de clases.

También es posible [colocar código de migraciones en una biblioteca de clases independiente desde el contexto de EF Core](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Otros marcos de destino

Las herramientas de la consola de administrador de paquetes de trabajar con proyectos de .NET Core o .NET Framework. Las aplicaciones que tienen el modelo de EF Core en una biblioteca de clases .NET Standard que no tenga un proyecto de .NET Framework o .NET Core. Por ejemplo, esto es cierto de aplicaciones de Xamarin y plataforma Universal de Windows. En tales casos, puede crear un proyecto de aplicación de consola .NET Core o .NET Framework cuyo único propósito es actuar como proyecto de inicio para las herramientas. El proyecto puede ser un proyecto ficticio sin código real &mdash; solo es necesario proporcionar un destino para las herramientas.

¿Por qué es un proyecto ficticio necesario? Como se mencionó anteriormente, tienen las herramientas ejecutar código de la aplicación en tiempo de diseño. Para ello, deben usar el runtime de .NET Core o .NET Framework. Cuando el modelo de EF Core está en un proyecto destinado a .NET Core o .NET Framework, las herramientas de EF Core pedir prestado el tiempo de ejecución del proyecto. No puede hacerlo si el modelo de EF Core está en una biblioteca de clases .NET Standard. .NET Standard no es una implementación real. NET; es una especificación de un conjunto de API que las implementaciones de .NET deben admitir. Por lo tanto, .NET Standard no es suficiente para las herramientas de EF Core ejecutar código de la aplicación. El proyecto ficticio creado para usar como proyecto de inicio proporciona una plataforma de destino concreta en la que las herramientas pueden cargar la biblioteca de clases .NET Standard.

### <a name="aspnet-core-environment"></a>Entorno de ASP.NET Core

Para especificar el entorno para proyectos de ASP.NET Core, establezca **env:ASPNETCORE_ENVIRONMENT** antes de ejecutar comandos.

## <a name="common-parameters"></a>Parámetros comunes

La siguiente tabla muestra los parámetros que son comunes a todos los comandos de EF Core:

| Parámetro                 | Descripción                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Contexto \<cadena >        | La `DbContext` clase se debe utilizar. Nombre de clase completa con espacios de nombres o única.  Si se omite este parámetro, EF Core busca la clase de contexto. Si hay varias clases de contexto, este parámetro es necesario. |
| -Proyecto \<cadena >        | El proyecto de destino. Si se omite este parámetro, el **proyecto predeterminado** para **Package Manager Console** se utiliza como el proyecto de destino.                                                                             |
| Proyecto de inicio - \<cadena > | El proyecto de inicio. Si se omite este parámetro, el **proyecto de inicio** en **propiedades de la solución** se utiliza como el proyecto de destino.                                                                                 |
| -Verbose                  | Mostrar resultados detallados.                                                                                                                                                                                                 |

Para mostrar información de ayuda sobre un comando, use PowerShell `Get-Help` comando.

> [!TIP]
> Los parámetros de contexto, el proyecto y el proyecto de inicio admiten la expansión de ficha.

## <a name="add-migration"></a>Add-Migration

Agrega una nueva migración.

Parámetros:

| Parámetro                         | Descripción                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Nombre \<cadena ><nobr>       | El nombre de la migración. Esto es un parámetro posicional y es necesario.                                              |
| <nobr>-OutputDir \<cadena ></nobr> | El directorio (y subespacio de nombres) para usar. Son las rutas de acceso relativa al directorio del proyecto de destino. El valor predeterminado es "Migraciones". |

## <a name="drop-database"></a>Quitar la base de datos

Quita la base de datos.

Parámetros:

| Parámetro | Descripción                                              |
|:----------|:---------------------------------------------------------|
| -WhatIf   | Mostrar qué base de datos se quitarían, pero no colóquelo. |

## <a name="get-dbcontext"></a>Get-DbContext

Listas disponibles `DbContext` tipos.

## <a name="remove-migration"></a>Remove-Migration

Quita la última migración (revierte los cambios de código que se realizaron para la migración).

Parámetros:

| Parámetro | Descripción                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| -Force    | Revertir la migración (revertir los cambios que se aplicaron a la base de datos). |

## <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Genera código para un `DbContext` y tipos de entidad para una base de datos. En orden para `Scaffold-DbContext` para generar un tipo de entidad, la tabla de base de datos debe tener una clave principal.

Parámetros:

| Parámetro                          | Descripción                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Connection \<cadena ></nobr> | La cadena de conexión a la base de datos. Para los proyectos de ASP.NET Core 2.x, el valor puede ser *nombre =\<nombre de cadena de conexión >*. En ese caso, el nombre procede de los orígenes de configuración que se configuran para el proyecto. Esto es un parámetro posicional y es necesario. |
| <nobr>-Proveedor \<cadena ></nobr>   | El proveedor debe usar. Normalmente, esto es el nombre del paquete de NuGet, por ejemplo: `Microsoft.EntityFrameworkCore.SqlServer`. Esto es un parámetro posicional y es necesario.                                                                                           |
| -OutputDir \<cadena >               | Para poner los archivos en el directorio. Las rutas de acceso son relativas al directorio de proyecto.                                                                                                                                                                                             |
| -ContextDir \<cadena >              | El directorio para colocar el `DbContext` archivo. Las rutas de acceso son relativas al directorio de proyecto.                                                                                                                                                                              |
| -Contexto \<cadena >                 | El nombre de la `DbContext` clase genere.                                                                                                                                                                                                                          |
| -Esquemas \<String [] >               | Los esquemas de tablas para generar tipos de entidad para. Si se omite este parámetro, se incluyen todos los esquemas.                                                                                                                                                             |
| -Tablas \<String [] >                | Para generar tipos de entidad para las tablas. Si se omite este parámetro, se incluyen todas las tablas.                                                                                                                                                                         |
| -DataAnnotations                   | Usar atributos para configurar el modelo (donde sea posible). Si se omite este parámetro, se usa solo la API fluida.                                                                                                                                                      |
| -UseDatabaseNames                  | Usar nombres de tabla y columna exactamente como aparecen en la base de datos. Si se omite este parámetro, se cambian los nombres de base de datos que mejor se ajuste a convenciones de estilo de nombre de C#.                                                                                       |
| -Force                             | Sobrescribir archivos existentes.                                                                                                                                                                                                                                               |

Ejemplo:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Ejemplo que se aplica la técnica scaffolding únicamente las tablas seleccionadas y crea el contexto en una carpeta independiente con un nombre especificado:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>Migración de scripts

Genera un script SQL que se aplica a todos los cambios de una migración seleccionada a otra migración seleccionado.

Parámetros:

| Parámetro                | Descripción                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *-Desde* \<cadena >        | La migración inicial. Las migraciones pueden identificarse por su nombre o identificador. El número 0 es un caso especial que significa *antes de la primera migración*. El valor predeterminado es 0.                                                              |
| *-A* \<cadena >          | La migración final. De forma predeterminada a la última migración.                                                                                                                                                                      |
| <nobr>-Idempotente</nobr> | Generar un script que se puede usar en una base de datos en cualquier migración.                                                                                                                                                         |
| -Output \<cadena >        | Para escribir el resultado en el archivo. Si se omite este parámetro, se crea el archivo con un nombre generado en la misma carpeta cuando se crean archivos de tiempo de ejecución de la aplicación, por ejemplo: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*. |

> [!TIP]
> El To, From, y los parámetros de salida admiten la expansión de ficha.

El ejemplo siguiente crea una secuencia de comandos para la migración de InitialCreate, usando el nombre de la migración.

```powershell
Script-Migration -To InitialCreate
```

En el ejemplo siguiente se crea una secuencia de comandos para todas las migraciones tras la migración InitialCreate, utilizando el identificador de la migración.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>Actualizar base de datos

Actualiza la base de datos a la última migración o a una migración especificada.

| Parámetro                           | Descripción                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>*-Migración* \<cadena ></nobr> | La migración de destino. Las migraciones pueden identificarse por su nombre o identificador. El número 0 es un caso especial que significa *antes de la primera migración* y hace que todas las migraciones se puede revertir. Si no se especifica ninguna migración, lo valores predeterminados del comando a la última migración. |

> [!TIP]
> El parámetro de la migración es compatible con la expansión de ficha.

El ejemplo siguiente devuelve todas las migraciones.

```powershell
Update-Database -Migration 0
```

Los ejemplos siguientes actualización la base de datos para una migración especificada. El primero usa el nombre de la migración y la segunda usa el identificador de la migración:

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```
