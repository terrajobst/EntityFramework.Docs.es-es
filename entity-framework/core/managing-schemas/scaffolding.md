---
title: 'Ingeniería inversa: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 1ba9352d261f1c131b0d70f8cdad2128d9afaefe
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414240"
---
# <a name="reverse-engineering"></a>Ingeniería inversa

La ingeniería inversa es el proceso de scaffolding de las clases de tipo de entidad y una clase DbContext basada en un esquema de base de datos. Puede realizarse mediante el comando `Scaffold-DbContext` de las herramientas de la consola del administrador de paquetes EF Core (PMC) o el comando `dotnet ef dbcontext scaffold` de las herramientas de la interfaz de la línea de comandos (CLI) de .NET.

## <a name="installing"></a>Instalación

Antes de la ingeniería inversa, deberá instalar las herramientas de [PMC](xref:core/miscellaneous/cli/powershell) (solo en Visual Studio) o las [herramientas](xref:core/miscellaneous/cli/dotnet)de la CLI. Vea los vínculos para obtener más información.

También necesitará instalar un [proveedor de base de datos](xref:core/providers/index) adecuado para el esquema de la base de datos al que desea aplicar ingeniería inversa.

## <a name="connection-string"></a>Cadena de conexión

El primer argumento del comando es una cadena de conexión a la base de datos. Las herramientas usarán esta cadena de conexión para leer el esquema de la base de datos.

La forma de citar y escapar de la cadena de conexión depende del shell que use para ejecutar el comando. Consulte la documentación de su shell para obtener información específica. Por ejemplo, PowerShell requiere que se escape el carácter `$`, pero no `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

```dotnetcli
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Configuración y secretos de usuario

Si tiene un proyecto de ASP.NET Core, puede usar la sintaxis de `Name=<connection-string>` para leer la cadena de conexión de la configuración.

Esto funciona bien con la [herramienta de administración de secretos](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) para mantener la contraseña de la base de datos separada del código base.

```dotnetcli
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Nombre de proveedor

El segundo argumento es el nombre del proveedor. El nombre del proveedor suele ser el mismo que el nombre del paquete NuGet del proveedor.

## <a name="specifying-tables"></a>Especificar tablas

De forma predeterminada, se aplica ingeniería inversa a todas las tablas del esquema de la base de datos en tipos de entidad. Puede limitar las tablas a las que se aplica ingeniería inversa mediante la especificación de esquemas y tablas.

El parámetro `-Schemas` en PMC y la opción `--schema` de la CLI se pueden usar para incluir todas las tablas de un esquema.

`-Tables` (PMC) y `--table` (CLI) se pueden usar para incluir tablas específicas.

Para incluir varias tablas en PMC, use una matriz.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Para incluir varias tablas en la CLI, especifique la opción varias veces.

```dotnetcli
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Conservar nombres

Los nombres de tablas y columnas se han corregido para que coincidan mejor con las convenciones de nomenclatura de .NET para tipos y propiedades de forma predeterminada. Si se especifica el modificador `-UseDatabaseNames` en PMC o la opción `--use-database-names` de la CLI, se deshabilitará este comportamiento para conservar los nombres de las bases de datos originales lo máximo posible. Los identificadores de .NET no válidos seguirán siendo fijos y los nombres sintetizados, como las propiedades de navegación, seguirán conforme a las convenciones de nomenclatura de .NET.

## <a name="fluent-api-or-data-annotations"></a>Anotaciones de datos o API fluidas

Los tipos de entidad se configuran mediante la API fluida de forma predeterminada. Especifique `-DataAnnotations` (PMC) o `--data-annotations` (CLI) para usar las anotaciones de datos siempre que sea posible.

Por ejemplo, el uso de la API fluida le aplicará esta técnica:

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Aunque el uso de anotaciones de datos es scaffolding:

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>Nombre de DbContext

El nombre de la clase DbContext con scaffolding será el nombre de la base de datos con sufijo de *contexto* de forma predeterminada. Para especificar otro, use `-Context` en PMC y `--context` en la CLI.

## <a name="directories-and-namespaces"></a>Directorios y espacios de nombres

Las clases de entidad y una clase DbContext se scaffolding en el directorio raíz del proyecto y usan el espacio de nombres predeterminado del proyecto. Puede especificar el directorio en el que se van a aplicar las scaffolding mediante `-OutputDir` (PMC) o `--output-dir` (CLI). El espacio de nombres será el espacio de nombres raíz más los nombres de los subdirectorios del directorio raíz del proyecto.

También puede usar `-ContextDir` (PMC) y `--context-dir` (CLI) para aplicar scaffolding a la clase DbContext en un directorio independiente de las clases de tipo de entidad.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

```dotnetcli
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>Funcionamiento

La ingeniería inversa comienza leyendo el esquema de la base de datos. Lee información acerca de las tablas, columnas, restricciones e índices.

A continuación, usa la información de esquema para crear un modelo de EF Core. Las tablas se usan para crear tipos de entidad. las columnas se usan para crear propiedades; y las claves externas se utilizan para crear relaciones.

Por último, el modelo se usa para generar código. Las clases de tipo de entidad, la API fluida y las anotaciones de datos correspondientes son scaffolding para volver a crear el mismo modelo desde la aplicación.

## <a name="limitations"></a>Limitaciones

* No todo lo relacionado con un modelo se puede representar mediante un esquema de la base de datos. Por ejemplo, la información sobre las [**jerarquías de herencia**](../modeling/inheritance.md), los [**tipos de propiedad**](../modeling/owned-entities.md)y la [**División de tablas**](../modeling/table-splitting.md) no están presentes en el esquema de la base de datos. Por este motivo, estas construcciones nunca se aplicarán a ingeniería inversa.
* Además, es posible que **algunos tipos de columna** no sean compatibles con el proveedor de EF Core. Estas columnas no se incluirán en el modelo.
* Puede definir [**tokens de simultaneidad**](../modeling/concurrency.md)en un modelo de EF Core para evitar que dos usuarios actualicen la misma entidad al mismo tiempo. Algunas bases de datos tienen un tipo especial para representar este tipo de columna (por ejemplo, rowversion en SQL Server), en cuyo caso se puede aplicar ingeniería inversa a esta información; sin embargo, no se aplicarán ingeniería inversa a otros tokens de simultaneidad.
* [La C# característica 8 tipos de referencia que aceptan valores NULL](/dotnet/csharp/tutorials/nullable-reference-types) no se admite actualmente en ingeniería inversa: EF Core C# siempre genera código que supone que la característica está deshabilitada. Por ejemplo, las columnas de texto que aceptan valores NULL se scaffolding como una propiedad con el tipo `string`, no `string?`, con la API fluida o las anotaciones de datos que se usan para configurar si una propiedad es obligatoria o no. Puede editar el código con scaffolding y reemplazarlo con anotaciones de C# nulabilidad. El seguimiento de la compatibilidad con scaffolding para tipos de referencia que aceptan valores NULL se realiza mediante el problema [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).

## <a name="customizing-the-model"></a>Personalización del modelo

El código generado por EF Core es el código. No dude en cambiarlo. Solo se regenerará si vuelve a aplicar ingeniería inversa al mismo modelo. El código con scaffolding representa *un* modelo que se puede utilizar para tener acceso a la base de datos, pero ciertamente no es el *único* modelo que se puede usar.

Personalice las clases de tipo de entidad y la clase DbContext para que se adapte a sus necesidades. Por ejemplo, puede elegir cambiar el nombre de tipos y propiedades, introducir jerarquías de herencia o dividir una tabla en varias entidades. También puede quitar índices no únicos, secuencias sin usar y propiedades de navegación, propiedades escalares opcionales y nombres de restricción del modelo.

También puede Agregar constructores, métodos, propiedades, etc. adicionales. usar otra clase parcial en un archivo independiente. Este enfoque funciona incluso cuando se desea volver a aplicar ingeniería inversa al modelo.

## <a name="updating-the-model"></a>Actualizar el modelo

Después de realizar cambios en la base de datos, puede que tenga que actualizar el modelo de EF Core para reflejar los cambios. Si los cambios en la base de datos son sencillos, puede que sea más fácil realizar los cambios manualmente en el modelo de EF Core. Por ejemplo, cambiar el nombre de una tabla o columna, quitar una columna o actualizar el tipo de una columna son cambios triviales que se deben realizar en el código.

Sin embargo, los cambios más significativos no son tan sencillos como los que se realizan de forma manual. Un flujo de trabajo común consiste en volver a aplicar ingeniería inversa del modelo de la base de datos mediante `-Force` (PMC) o `--force` (CLI) para sobrescribir el modelo existente con uno actualizado.

Otra característica solicitada comúnmente es la posibilidad de actualizar el modelo de la base de datos a la vez que se conserva la personalización, como cambiar el nombre, las jerarquías de tipos, etc. Use el [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) de problemas para realizar el seguimiento del progreso de esta característica.

> [!WARNING]
> Si vuelve a aplicar ingeniería inversa al modelo desde la base de datos, se perderán los cambios realizados en los archivos.
