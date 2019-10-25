---
title: 'Tabla de historial de migraciones personalizadas: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0db393ff3101564f8d8081d0a57b264c2c459df7
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812073"
---
# <a name="custom-migrations-history-table"></a>Tabla de historial de migraciones personalizadas

De forma predeterminada, EF Core realiza un seguimiento de las migraciones que se han aplicado a la base de datos mediante su grabación en una tabla denominada `__EFMigrationsHistory`. Por varias razones, puede que desee personalizar esta tabla para satisfacer mejor sus necesidades.

> [!IMPORTANT]
> Si personaliza la tabla de historial de migraciones *después* de aplicar las migraciones, es responsable de actualizar la tabla existente en la base de datos.

## <a name="schema-and-table-name"></a>Esquema y nombre de tabla

Puede cambiar el esquema y el nombre de la tabla mediante el método `MigrationsHistoryTable()` en `OnConfiguring()` (o `ConfigureServices()` en ASP.NET Core). Este es un ejemplo del uso del proveedor de EF Core de SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a>Otros cambios

Para configurar aspectos adicionales de la tabla, invalide y reemplace el servicio de `IHistoryRepository` específico del proveedor. Este es un ejemplo de cómo cambiar el nombre de la columna MigrationId a *ID* en SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` está dentro de un espacio de nombres interno y puede cambiar en futuras versiones.

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
