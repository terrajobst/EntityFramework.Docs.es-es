---
title: Reverse Engineering - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: ef729c0c26d5a1f57099f339eb51cda7e83289df
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688685"
---
# <a name="reverse-engineering"></a>Ingeniería inversa

Ingeniería inversa es el proceso de scaffolding de las clases de tipo de entidad y una clase DbContext según un esquema de base de datos. Puede realizarse mediante la `Scaffold-DbContext` comando de las herramientas de la consola de administrador de paquetes (PMC) de EF Core o la `dotnet ef dbcontext scaffold` comando de las herramientas de interfaz de línea de comandos (CLI) de .NET.

## <a name="installing"></a>Instalación

Antes de aplicar ingeniería inversa, deberá instalar el [herramientas PMC](xref:core/miscellaneous/cli/powershell) (solo Visual Studio) o el [herramientas CLI](xref:core/miscellaneous/cli/dotnet). Consulte los vínculos para obtener más información.

También deberá instalar una adecuada [proveedor de base de datos](xref:core/providers/index) para el esquema de base de datos que desea aplicar ingeniería inversa.

## <a name="connection-string"></a>Cadena de conexión

El primer argumento para el comando es una cadena de conexión a la base de datos. Las herramientas usará esta cadena de conexión para leer el esquema de base de datos.

Cómo comillas y escape de la cadena de conexión depende de qué shell que esté utilizando para ejecutar el comando. Consulte la documentación de su shell para obtener información específica. Por ejemplo, PowerShell, deberá escape la `$` de caracteres, pero no `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Configuración y los secretos de usuario

Si tiene un proyecto de ASP.NET Core, puede usar el `Name=<connection-string>` sintaxis para leer la cadena de conexión de configuración.

Esto funciona bien con la [herramienta Secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) separar la contraseña de la base de datos de la base de código.

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Nombre del proveedor

El segundo argumento es el nombre del proveedor. El nombre del proveedor suele ser el mismo que el nombre del paquete NuGet del proveedor.

## <a name="specifying-tables"></a>Especificación de tablas

Todas las tablas en el esquema de base de datos son inversas diseñadas en tipos de entidad de forma predeterminada. Puede limitar qué tablas son inversos ingeniería mediante la especificación de esquemas y tablas.

El `-Schemas` parámetro en la PMC y `--schema` opción en la CLI puede utilizarse para incluir todas las tablas dentro de un esquema.

`-Tables` (PMC) y `--table` (CLI) puede usarse para incluir tablas específicas.

Para incluir varias tablas en la PMC, use una matriz.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Para incluir varias tablas en la CLI, especifique la opción varias veces.

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Conservación de nombres

Los nombres de tabla y columna se corrigen que mejor coincida con las convenciones de nomenclatura .NET para los tipos y propiedades de forma predeterminada. Especificar el `-UseDatabaseNames` cambie en PMC o el `--use-database-names` opción en la CLI, deshabilitará este comportamiento conserva los nombres de base de datos original tanto como sea posible. Todavía se corregirán los identificadores no válidos de .NET y convenciones de nomenclatura .NET aún cumpla sintetizados nombres como propiedades de navegación.

## <a name="fluent-api-or-data-annotations"></a>La API Fluent o las anotaciones de datos

Tipos de entidad se configuran mediante la API Fluent de forma predeterminada. Especificar `-DataAnnotations` (PMC) o `--data-annotations` (CLI) para usar en su lugar, las anotaciones de datos cuando sea posible.

Por ejemplo, mediante la API Fluent aplicará la técnica scaffolding este.

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Al usar anotaciones de datos aplicará la técnica scaffolding esto.

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>Nombre de DbContext

El nombre de clase con scaffolding de DbContext será el nombre de la base de datos con el sufijo *contexto* de forma predeterminada. Para especificar otro diferente, use `-Context` en PMC y `--context` en la CLI.

## <a name="directories-and-namespaces"></a>Espacios de nombres y directorios

Las clases de entidad y una clase DbContext se ha aplicado scaffolding en el directorio raíz del proyecto y usan el espacio de nombres del proyecto predeterminado. Puede especificar el directorio donde se ha aplicado scaffolding a las clases con `-OutputDir` (PMC) o `--output-dir` (CLI). El espacio de nombres será el espacio de nombres raíz y los nombres de sus subdirectorios en el directorio raíz del proyecto.

También puede usar `-ContextDir` (PMC) y `--context-dir` (CLI) para aplicar scaffolding a la clase DbContext en un directorio diferente de las clases de tipo de entidad.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Cómo funciona

Ingeniería inversa comienza por leer el esquema de base de datos. Lee información sobre tablas, columnas, restricciones e índices.

A continuación, usa la información de esquema para crear un modelo de EF Core. Las tablas se utilizan para crear tipos de entidad; las columnas se utilizan para crear las propiedades; y las claves externas se utilizan para crear las relaciones.

Por último, el modelo se usa para generar código. Se ha aplicado scaffolding a la correspondiente entidad clases, la API Fluent y datos de las anotaciones de tipo con el fin de volver a crear el mismo modelo de la aplicación.

## <a name="what-doesnt-work"></a>¿Qué no funciona

No todo sobre un modelo puede representarse mediante un esquema de base de datos. Por ejemplo, información sobre **jerarquías de herencia**, **tipos de propiedad**, y **dividir la tabla** no están presentes en el esquema de base de datos. Por este motivo, estas construcciones no se nunca se ingeniería inversa.

Además, **algunos tipos de columna** es posible que no se admite el proveedor de EF Core. Estas columnas no se incluirán en el modelo.

EF Core requiere tener una clave de cada tipo de entidad. Las tablas, sin embargo, no se deben especificar una clave principal. **Las tablas sin una clave principal** son actualmente no se realizó ingeniería inversa.

Puede definir **tokens de simultaneidad** en un modelo de EF Core para evitar que dos usuarios actualizando la misma entidad al mismo tiempo. Algunas bases de datos tienen un tipo especial para representar este tipo de columna (por ejemplo, rowversion en SQL Server), en cuyo caso de que la podemos invertir diseñar esta información; Sin embargo, otros tokens de simultaneidad no se invertirá ingeniería.

## <a name="customizing-the-model"></a>Personalizar el modelo

El código generado por EF Core es el código. No dude en cambiar. Solo se regenerará si aplicar ingeniería inversa del mismo modelo nuevo. Representa el código con scaffolding *una* modelo que puede utilizarse para tener acceso a la base de datos, pero es ciertamente no la *sólo* modelo que se puede usar.

Personalizar las clases de tipos de entidad y la clase DbContext para ajustarlo a sus necesidades. Por ejemplo, puede cambiar el nombre de tipos y propiedades, introducir a las jerarquías de herencia o dividir una tabla en a varias entidades. También puede quitar índices no únicos, sin usar secuencias y las propiedades de navegación, las propiedades escalares opcionales y los nombres de restricción del modelo.

También se pueden agregar constructores adicionales, métodos, propiedades, etcetera. uso de otra clase parcial en un archivo independiente. Este enfoque funciona incluso cuando se desea aplicar el modelo de ingeniería inversa a nuevo.

## <a name="updating-the-model"></a>Actualización del modelo

Después de realizar cambios en la base de datos, debe actualizar el modelo de EF Core para reflejar dichos cambios. Si los cambios de la base de datos son simples, puede ser más fácil realizar manualmente los cambios en el modelo de EF Core. Por ejemplo, cambiar el nombre de una tabla o columna, quitar una columna o la actualización de un tipo de columna es triviales cambios realizados en el código.

Los cambios más importantes, sin embargo, no son tan fácil hacer manualmente. Un flujo de trabajo común es invertir diseñar el modelo de la base de datos nuevo con `-Force` (PMC) o `--force` (CLI) para sobrescribir el modelo existente con uno actualizado.

Otra característica habitualmente solicitada es la capacidad para actualizar el modelo de la base de datos conservando la personalización, como cambios de nombre, las jerarquías de tipos, etcetera. Usar problema [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) para realizar un seguimiento del progreso de esta característica.

> [!WARNING]
> Si la ingeniería inversa el modelo de la base de datos nuevo, se perderán los cambios realizados en los archivos.
