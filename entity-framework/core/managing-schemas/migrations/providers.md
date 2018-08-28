---
title: Migraciones con varios proveedores - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.openlocfilehash: 7ae695037992323337a780cda29d8c8ed8a13458
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997978"
---
<a name="migrations-with-multiple-providers"></a>Migraciones con varios proveedores
==================================
El [herramientas de EF Core] [ 1] solo aplicar la técnica scaffolding migraciones para el proveedor activo. A veces, sin embargo, es posible que desee usar más de un proveedor (por ejemplo Microsoft SQL Server y SQLite) con DbContext. Hay dos formas de controlar esto con las migraciones. Puede mantener dos conjuntos de migraciones: uno para cada proveedor--o merge en un único conjunto de que pueda trabajar en ambos.

<a name="two-migration-sets"></a>Dos conjuntos de migración
------------------
En el primer enfoque, generar dos migraciones para cada cambio del modelo.

Una forma de hacerlo es colocar cada conjunto de migración [en un ensamblado independiente] [ 2] y cambie manualmente el proveedor activo (y el ensamblado de migraciones) entre las dos migraciones de agregar.

Otro enfoque que facilita el trabajo con las herramientas es crear un nuevo tipo que deriva de DbContext y reemplaza el proveedor activo. Este tipo se usa en el diseño de tiempo cuando se agregan o aplicar las migraciones.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Puesto que cada conjunto de migración utiliza sus propios tipos de DbContext, este enfoque no requiere el uso de un ensamblado independiente de las migraciones.

Al agregar la nueva migración, especificar los tipos de contexto.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> No es necesario especificar el directorio de salida para las migraciones posteriores desde que se crearon como elementos relacionados a la última de ellas.

<a name="one-migration-set"></a>Migración de un conjunto
-----------------
Si no le gusta tener dos conjuntos de migraciones, puede combinarlos manualmente en un único conjunto que se puede aplicar a ambos proveedores.

Las anotaciones pueden coexistir puesto que un proveedor omite cualquier anotación que no comprenda. Por ejemplo, una columna de clave principal que funciona con Microsoft SQL Server y SQLite podría ser similar al siguiente.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Si solo se pueden aplicar operaciones en un proveedor (o son una forma diferente entre los proveedores), use el `ActiveProvider` propiedad para indicar a qué proveedor está activo.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
