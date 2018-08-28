---
title: Tabla de historial de migración personalizadas - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.openlocfilehash: 7ee76cadd6fac4ec403918e88460e43067ae5815
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995701"
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="63e16-102">Tabla de historial de migración personalizadas</span><span class="sxs-lookup"><span data-stu-id="63e16-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="63e16-103">De forma predeterminada, EF Core realiza un seguimiento de las migraciones que se han aplicado a la base de datos mediante la grabación de en una tabla denominada `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="63e16-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="63e16-104">Por diversas razones, puede personalizar esta tabla para satisfacer mejor sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="63e16-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="63e16-105">Si personaliza la tabla de historial de migraciones *después* aplicar migraciones, usted es responsable de actualizar la tabla existente en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="63e16-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="63e16-106">Nombre de esquema y tabla</span><span class="sxs-lookup"><span data-stu-id="63e16-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="63e16-107">Puede cambiar el esquema y el nombre de tabla mediante el `MigrationsHistoryTable()` método `OnConfiguring()` (o `ConfigureServices()` en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="63e16-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="63e16-108">Este es un ejemplo con el proveedor de SQL Server EF Core.</span><span class="sxs-lookup"><span data-stu-id="63e16-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="63e16-109">Otros cambios</span><span class="sxs-lookup"><span data-stu-id="63e16-109">Other changes</span></span>
-------------
<span data-ttu-id="63e16-110">Para configurar aspectos adicionales de la tabla, reemplazar y reemplazar específico del proveedor `IHistoryRepository` service.</span><span class="sxs-lookup"><span data-stu-id="63e16-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="63e16-111">Este es un ejemplo de cambiar el nombre de columna MigrationId a *Id* en SQL Server.</span><span class="sxs-lookup"><span data-stu-id="63e16-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="63e16-112">`SqlServerHistoryRepository` está dentro de un espacio de nombres interna y puede cambiar en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="63e16-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
