---
title: Tabla de historial de migraciones personalizado - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-history-table"></a>Tabla de historial de migraciones personalizado
===============================
De forma predeterminada, el núcleo de EF realiza un seguimiento de las migraciones se han aplicado a la base de datos grabándolos en una tabla denominada `__EFMigrationsHistory`. Por diversas razones, puede que desee personalizar esta tabla para que se ajuste mejor a sus necesidades.

> [!IMPORTANT]
> Si personaliza la tabla de historial de migraciones *después* aplica las migraciones, usted es responsable de actualizar la tabla existente en la base de datos.

<a name="schema-and-table-name"></a>Nombre del esquema y tabla
----------------------
Puede cambiar el esquema y el nombre de tabla mediante el `MigrationsHistoryTable()` método `OnConfiguring()` (o `ConfigureServices()` en ASP.NET Core). Este es un ejemplo con el proveedor de EF núcleo de SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Otros cambios
-------------
Para configurar aspectos adicionales de la tabla, reemplazar y reemplazar específico del proveedor `IHistoryRepository` servicio. Este es un ejemplo de cambiar el nombre de columna MigrationId para *identificador* en SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository`dentro de un espacio de nombres interno y puede cambiar en futuras versiones.

``` csharp
class MyHistoryRepository : SqlServerHistoryRepository
{
    public MyHistoryRepository(HistoryRepositoryDependencies dependencies)
        : base(dependencies)
    {
    }

    protected override void ConfigureTable(EntityTypeBuilder<HistoryRow> history)
    {
        base.ConfigureTable(history);

        history.Property(h => h.MigrationId).HasColumnName("Id");
    }
}
```
