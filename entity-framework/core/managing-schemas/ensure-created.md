---
title: 'Crear y quitar API: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
uid: core/managing-schemas/ensure-created
ms.openlocfilehash: 32ac6cd043df73cd041780ec4c8805675adc5ab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414324"
---
# <a name="create-and-drop-apis"></a>Crear y quitar API

Los métodos EnsureCreated y EnsureDeleted proporcionan una alternativa ligera a las [migraciones](migrations/index.md) para administrar el esquema de la base de datos. Estos métodos son útiles en escenarios en los que los datos son transitorios y se pueden quitar cuando cambia el esquema. Por ejemplo, durante el prototipo, en las pruebas o en las memorias caché locales.

Algunos proveedores (especialmente los no relacionales) no admiten las migraciones. Para estos proveedores, EnsureCreated suele ser la manera más fácil de inicializar el esquema de la base de datos.

> [!WARNING]
> EnsureCreated y las migraciones no funcionan bien juntos. Si utiliza migraciones, no use EnsureCreated para inicializar el esquema.

La transición de EnsureCreated a migraciones no es una experiencia sin problemas. La manera más sencilla de hacerlo es quitar la base de datos y volver a crearla con las migraciones. Si prevé usar migraciones en el futuro, es mejor empezar con las migraciones en lugar de usar EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

El método EnsureDeleted quitará la base de datos si existe. Si no tiene los permisos adecuados, se produce una excepción.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated creará la base de datos si no existe e inicializará el esquema de la base de datos. Si existe alguna tabla (incluidas las tablas de otra clase DbContext), el esquema no se inicializará.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> También hay disponibles versiones asincrónicas de estos métodos.

## <a name="sql-script"></a>Secuencia de comandos de SQL

Para obtener el SQL que usa EnsureCreated, puede utilizar el método GenerateCreateScript.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Varias clases DbContext

EnsureCreated solo funciona cuando no hay ninguna tabla presente en la base de datos. Si es necesario, puede escribir su propia comprobación para ver si es necesario inicializar el esquema y usar el servicio IRelationalDatabaseCreator subyacente para inicializar el esquema.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
