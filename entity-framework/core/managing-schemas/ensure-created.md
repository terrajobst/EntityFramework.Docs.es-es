---
title: Crear y quitar API - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285644"
---
# <a name="create-and-drop-apis"></a>Crear y quitar API

Los métodos EnsureDeleted y de EnsureCreated proporcionan una alternativa ligera a [migraciones](migrations/index.md) para administrar el esquema de base de datos. Esto es útil en escenarios cuando los datos es transitorios y se pueden quitar cuando cambia el esquema. Por ejemplo, durante la creación de prototipos, pruebas o en las memorias caché locales.

Algunos proveedores (especialmente los que no son relacionales) no admiten las migraciones. En estos casos, EnsureCreated suele ser la manera más fácil para inicializar el esquema de base de datos.

> [!WARNING]
> Migraciones y EnsureCreated no funcionan bien juntos. Si usa las migraciones, no use EnsureCreated para inicializar el esquema.

Transición de EnsureCreated a las migraciones no es una experiencia sin problemas. La simpelest para lograr esto consiste en colocar la base de datos y volver a crearla mediante migraciones. Si prevé que usará las migraciones en el futuro, es mejor comenzar con las migraciones en lugar de usar EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

El método EnsureDeleted quitará la base de datos si existe. Si no tiene los permisos adecuados, se produce una excepción.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated creará la base de datos si no existe e inicializar el esquema de base de datos. Si existen cualquier tablas (incluidas las tablas de otra clase DbContext), el esquema no inicializarse.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Versiones asincrónicas de estos métodos también están disponibles.

## <a name="sql-script"></a>Secuencia de comandos SQL

Para obtener el SQL que usa EnsureCreated, puede usar el método GenerateCreateScript.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Varias clases DbContext

EnsureCreated solo funciona cuando no hay tablas están presentes en la base de datos. Si es necesario, puede escribir su propia comprobación para ver si se debe inicializar el esquema y el servicio IRelationalDatabaseCreator subyacente para inicializar el esquema.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
