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
ms.locfileid: "26053815"
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="15ce3-102">Tabla de historial de migraciones personalizado</span><span class="sxs-lookup"><span data-stu-id="15ce3-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="15ce3-103">De forma predeterminada, el núcleo de EF realiza un seguimiento de las migraciones se han aplicado a la base de datos grabándolos en una tabla denominada `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="15ce3-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="15ce3-104">Por diversas razones, puede que desee personalizar esta tabla para que se ajuste mejor a sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="15ce3-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="15ce3-105">Si personaliza la tabla de historial de migraciones *después* aplica las migraciones, usted es responsable de actualizar la tabla existente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="15ce3-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="15ce3-106">Nombre del esquema y tabla</span><span class="sxs-lookup"><span data-stu-id="15ce3-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="15ce3-107">Puede cambiar el esquema y el nombre de tabla mediante el `MigrationsHistoryTable()` método `OnConfiguring()` (o `ConfigureServices()` en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="15ce3-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="15ce3-108">Este es un ejemplo con el proveedor de EF núcleo de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="15ce3-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="15ce3-109">Otros cambios</span><span class="sxs-lookup"><span data-stu-id="15ce3-109">Other changes</span></span>
-------------
<span data-ttu-id="15ce3-110">Para configurar aspectos adicionales de la tabla, reemplazar y reemplazar específico del proveedor `IHistoryRepository` servicio.</span><span class="sxs-lookup"><span data-stu-id="15ce3-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="15ce3-111">Este es un ejemplo de cambiar el nombre de columna MigrationId para *identificador* en SQL Server.</span><span class="sxs-lookup"><span data-stu-id="15ce3-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="15ce3-112">`SqlServerHistoryRepository`dentro de un espacio de nombres interno y puede cambiar en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="15ce3-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
