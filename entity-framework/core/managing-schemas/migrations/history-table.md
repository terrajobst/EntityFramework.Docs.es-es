---
title: Tabla de historial de migración personalizadas - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: 1a253972a8f4e410421ec8a77c079e588d368819
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488821"
---
<a name="custom-migrations-history-table"></a>Tabla de historial de migración personalizadas
===============================
De forma predeterminada, EF Core realiza un seguimiento de las migraciones que se han aplicado a la base de datos mediante la grabación de en una tabla denominada `__EFMigrationsHistory`. Por diversas razones, puede personalizar esta tabla para satisfacer mejor sus necesidades.

> [!IMPORTANT]
> Si personaliza la tabla de historial de migraciones *después* aplicar migraciones, usted es responsable de actualizar la tabla existente en la base de datos.

<a name="schema-and-table-name"></a>Nombre de esquema y tabla
----------------------
Puede cambiar el esquema y el nombre de tabla mediante el `MigrationsHistoryTable()` método `OnConfiguring()` (o `ConfigureServices()` en ASP.NET Core). Este es un ejemplo con el proveedor de SQL Server EF Core.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Otros cambios
-------------
Para configurar aspectos adicionales de la tabla, reemplazar y reemplazar específico del proveedor `IHistoryRepository` service. Este es un ejemplo de cambiar el nombre de columna MigrationId a *Id* en SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` está dentro de un espacio de nombres interna y puede cambiar en futuras versiones.

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
