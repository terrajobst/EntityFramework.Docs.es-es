---
title: 'Microsoft SQL Server Database proveedor - tablas con optimización para memoria: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 63d2cbf8b69e4f1945ad60914e284fb42c48e8db
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995807"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="468ec-102">Compatibilidad con tablas optimizadas para memoria en el proveedor de base de datos de SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="468ec-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="468ec-103">Esta característica se introdujo en EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="468ec-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="468ec-104">[Tablas optimizadas para memoria](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) son una característica de SQL Server donde reside toda la tabla en memoria.</span><span class="sxs-lookup"><span data-stu-id="468ec-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="468ec-105">Una segunda copia de la tabla de datos se mantiene en el disco, pero solo con fines de durabilidad.</span><span class="sxs-lookup"><span data-stu-id="468ec-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="468ec-106">Datos en tablas optimizadas para memoria solo se leen del disco durante la recuperación de base de datos.</span><span class="sxs-lookup"><span data-stu-id="468ec-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="468ec-107">Por ejemplo, un servidor después de reiniciar.</span><span class="sxs-lookup"><span data-stu-id="468ec-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="468ec-108">Configuración de una tabla optimizada para memoria</span><span class="sxs-lookup"><span data-stu-id="468ec-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="468ec-109">Puede especificar que la tabla a la que está asignada una entidad está optimizada para memoria.</span><span class="sxs-lookup"><span data-stu-id="468ec-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="468ec-110">Cuando use EF Core para crear y mantener una base de datos basada en el modelo (ya sea con migraciones o `Database.EnsureCreated()`), se creará una tabla optimizada para memoria para estas entidades.</span><span class="sxs-lookup"><span data-stu-id="468ec-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
