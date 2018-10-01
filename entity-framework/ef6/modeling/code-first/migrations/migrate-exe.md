---
title: Uso de migrate.exe - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: cf6c3a0a256730b24addf1012d6ff53b17035cd4
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459544"
---
# <a name="using-migrateexe"></a>Uso de migrate.exe
Migraciones de Code First puede usarse para actualizar una base de datos desde dentro de visual studio, pero también se pueden ejecutar a través de la migrate.exe herramienta línea de comandos. Esta página proporcionan una visión general rápida sobre cómo usar migrate.exe para ejecutar migraciones en comparación con una base de datos.

> [!NOTE]
> En este artículo se supone que sabe cómo usar migraciones de Code First en escenarios básicos. Si no lo hace, necesitará leer [migraciones de Code First](~/ef6/modeling/code-first/migrations/index.md) antes de continuar.

## <a name="copy-migrateexe"></a>Copiar migrate.exe

Al instalar Entity Framework mediante NuGet migrate.exe estará dentro de la carpeta de herramientas del paquete descargado. En &lt;carpeta del proyecto&gt;\\paquetes\\EntityFramework.&lt; versión&gt;\\herramientas

Una vez que tenga migrate.exe debe copiarlo en la ubicación del ensamblado que contiene las migraciones.

Si la aplicación está destinada a .NET 4 y 4.5 no, a continuación, deberá copiar el **Redirect.config** en la ubicación de así y cámbiele el nombre **migrate.exe.config**. Esto es para que migrate.exe obtiene las redirecciones de enlace correcto para poder localizar el ensamblado de Entity Framework.

| .NET 4.5                                      | .NET 4.0                                      |
|:----------------------------------------------|:----------------------------------------------|
| ![Archivos de .NET Framework 4.5](~/ef6/media/net45files.png) | ![Archivos de .NET 4.0](~/ef6/media/net40files.png) |

> [!NOTE]
> Migrate.exe no es compatible con x64 ensamblados.

Una vez haya movido migrate.exe a la carpeta correcta, a continuación, podrá usarlo para ejecutar migraciones en comparación con la base de datos. Todo está diseñada para hacer es ejecutar migraciones. Que no se puede generar las migraciones o crear una secuencia de comandos SQL.

## <a name="see-options"></a>Vea las opciones

``` console
Migrate.exe /?
```

Lo anterior mostrará la página de ayuda asociada con esta utilidad, tenga en cuenta que deberá tener EntityFramework.dll en la misma ubicación que se está ejecutando migrate.exe en orden para que funcione.

## <a name="migrate-to-the-latest-migration"></a>Migrar a la migración más reciente

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile=”..\\web.config”
```

Cuando ejecuta migrate.exe el parámetro obligatorio solo es el ensamblado, que es el ensamblado que contiene las migraciones que está intentando ejecutar, pero usará la convención de todos los según la configuración si no especifica el archivo de configuración.

## <a name="migrate-to-a-specific-migration"></a>Migrar a una migración específica

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /targetMigration=”AddTitle”
```

Si desea ejecutar las migraciones hasta una migración específica, a continuación, puede especificar el nombre de la migración. Esto ejecutará todas las migraciones anteriores según sea necesario hasta llegar a la migración especificada.

## <a name="specify-working-directory"></a>Especificar directorio de trabajo

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /startupDirectory=”c:\\MyApp”
```

Si el ensamblado tiene dependencias o lee los archivos en relación con el directorio de trabajo, a continuación, deberá establecer startupDirectory.

## <a name="specify-migration-configuration-to-use"></a>Especifique la configuración de migración que se usará

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile=”..\\web.config”
```

Si tiene varias clases de configuración de migración, las clases que heredan de DbMigrationConfiguration, a continuación, deberá especificar que es que se usará para esta ejecución. Esto se especifica al proporcionar el segundo parámetro opcional sin un modificador como anteriormente.

## <a name="provide-connection-string"></a>Proporcionar la cadena de conexión

``` console
Migrate.exe BlogDemo.dll /connectionString=”Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI” /connectionProviderName=”System.Data.SqlClient”
```

Si desea especificar una cadena de conexión en la línea de comandos también debe proporcionar el nombre del proveedor. No se especifica el nombre del proveedor se producirá una excepción.

## <a name="common-problems"></a>Problemas habituales

| Mensaje de error                                                                                                                                                                                                                                                                                                                      | Soluciones                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Excepción no controlada: System.IO.FileLoadException: no se pudo cargar el archivo o ensamblado ' Entity Framework, versión = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089' o uno de sus dependencias. Definición del manifiesto de ensamblado encontrada no coincide con la referencia de ensamblado. (Excepción de HRESULT: 0x80131040)         | Normalmente, esto significa que se está ejecutando una aplicación de .NET 4 sin el archivo Redirect.config. Deberá copiar el Redirect.config en la misma ubicación que migrate.exe y cámbielo por migrate.exe.config.                                                                                       |
| Excepción no controlada: System.IO.FileLoadException: no se pudo cargar el archivo o ensamblado ' Entity Framework, versión = 4.4.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089' o uno de sus dependencias. Definición del manifiesto de ensamblado encontrada no coincide con la referencia de ensamblado. (Excepción de HRESULT: 0x80131040)          | Esta excepción significa que se está ejecutando una aplicación con el Redirect.config se copia en la ubicación de migrate.exe de .NET 4.5. Si su aplicación es .NET 4.5 no es necesario tener el archivo de configuración con las redirecciones dentro. Elimine el archivo migrate.exe.config.                                    |
| ERROR: No se puede actualizar la base de datos para que coincida con el modelo actual porque hay cambios pendientes y migración automática está deshabilitada. Escribir los cambios pendientes del modelo a una migración basada en código o habilitar la migración automática. Establecer DbMigrationsConfiguration.AutomaticMigrationsEnabled en True para habilitar la migración automática. | Este error se produce si la migración cuando no ha creado una migración para hacer frente a los cambios realizados en el modelo y la base de datos no coincide con el modelo de ejecución. Agregar una propiedad a una clase de modelo, a continuación, ejecuta migrate.exe sin necesidad de crear una migración para actualizar la base de datos es un ejemplo de esto. |
| ERROR: No se resuelve el tipo de miembro ' System.Data.Entity.Migrations.Design.ToolingFacade+UpdateRunner,EntityFramework, Version = 5.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089'.                                                                                                                                       | Este error puede deberse al especificar un directorio de inicio incorrecta. Debe tratarse de la ubicación de migrate.exe                                                                                                                                                                                      |
| Excepción no controlada: System.NullReferenceException: referencia a objeto no establecida para una instancia de un objeto. <br/>   en System.Data.Entity.Migrations.Console.Program.Main (String [] args)                                                                                                                                             | Esto puede deberse al no especificar un parámetro necesario para un escenario que esté utilizando. Por ejemplo si se especifica una cadena de conexión sin especificar el nombre del proveedor.                                                                                                                        |
| ERROR: se encontró más de un tipo de configuración de las migraciones en el ensamblado 'ClassLibrary1'. Especifique el nombre de usar uno.                                                                                                                                                                                                  | Como indica el error, hay más de una clase de configuración en el ensamblado especificado. Debe usar el modificador /configurationType para especificar que se va a usar.                                                                                                                                           |
| ERROR: No se pudo cargar archivo o ensamblado '&lt;assemblyName&gt;' o uno de sus dependencias. El ensamblado dado el nombre o código base no era válido. (Excepción de HRESULT: 0x80131047)                                                                                                                                                    | Esto puede deberse a especificar un nombre de ensamblado incorrecto o no tener                                                                                                                                                                                                                          |
| ERROR: No se pudo cargar archivo o ensamblado '&lt;assemblyName&gt;' o uno de sus dependencias. Se ha intentado cargar un programa con un formato incorrecto.                                                                                                                                                                          | Esto ocurre si intenta ejecutar migrate.exe contra un x64 aplicación. EF 5.0 y, a continuación solo funcionará en x86.                                                                                                                                                                                |
