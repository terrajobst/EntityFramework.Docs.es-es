---
title: Migraciones con varios proveedores - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2018
---
<a name="migrations-with-multiple-providers"></a>Migraciones con varios proveedores
==================================
El [herramientas básicas de EF] [ 1] solo aplique la técnica scaffolding las migraciones para el proveedor activo. A veces, sin embargo, puede que desee utilizar más de un proveedor (por ejemplo Microsoft SQL Server y SQLite) con su DbContext. Hay dos formas de controlar esto con las migraciones. Puede mantener dos conjuntos de migraciones: uno para cada proveedor--o combinación de ellas en un solo conjunto que puede trabajar en ambos.

<a name="two-migration-sets"></a>Dos conjuntos de migración
------------------
En el primer enfoque y generar dos migraciones para cada cambio de modelo.

Una manera de hacer esto es colocar cada conjunto de migración [en un ensamblado independiente] [ 2] y cambiar manualmente el proveedor activo (y el ensamblado de migraciones) entre dos migraciones de agregar.

Otro enfoque que facilita el trabajo con las herramientas consiste en crear un nuevo tipo que deriva de su DbContext e invalida el proveedor activo. Este tipo se utiliza en el diseño de tiempo cuando se agregan o aplicar las migraciones.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Puesto que cada conjunto de migración utiliza sus propios tipos de DbContext, este enfoque no requiere el uso de un ensamblado de migraciones independientes.

Al agregar nueva migración, especificar los tipos de contexto.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> No es necesario especificar el directorio de salida para las migraciones posteriores desde que se crearon como nodos relacionados hasta el último.

<a name="one-migration-set"></a>Conjunto de una migración
-----------------
Si no le gusta tener dos conjuntos de migraciones, puede combinarlas manualmente en un único conjunto que se pueden aplicar a ambos proveedores.

Las anotaciones pueden coexistir desde un proveedor omite cualquier anotación que no comprende. Por ejemplo, una columna de clave principal que funciona con Microsoft SQL Server y SQLite podría ser similar al siguiente.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Si solo se pueden aplicar operaciones en un proveedor (o sean más diferente entre proveedores), utilice el `ActiveProvider` propiedad para indicar qué proveedor está activo.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
