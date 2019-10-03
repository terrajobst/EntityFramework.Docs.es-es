---
title: 'Proveedor de base de datos de Microsoft SQL Server: tablas con optimización para memoria-EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 7383e74b3f83172f9b8e0eaf9bd09d4e187e87f8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813495"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="5c36c-102">Compatibilidad con tablas optimizadas para memoria en EF Core proveedor de bases de datos de SQL Server</span><span class="sxs-lookup"><span data-stu-id="5c36c-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="5c36c-103">[Las tablas optimizadas para memoria](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) son una característica de SQL Server en la que toda la tabla reside en memoria.</span><span class="sxs-lookup"><span data-stu-id="5c36c-103">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="5c36c-104">Una segunda copia de los datos de la tabla se mantiene en el disco, pero solo con fines de durabilidad.</span><span class="sxs-lookup"><span data-stu-id="5c36c-104">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="5c36c-105">Los datos de las tablas optimizadas para memoria solo se leen desde el disco durante la recuperación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5c36c-105">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="5c36c-106">Por ejemplo, después de reiniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="5c36c-106">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="5c36c-107">Configurar una tabla optimizada para memoria</span><span class="sxs-lookup"><span data-stu-id="5c36c-107">Configuring a memory-optimized table</span></span>

<span data-ttu-id="5c36c-108">Puede especificar que la tabla a la que está asignada una entidad está optimizada para memoria.</span><span class="sxs-lookup"><span data-stu-id="5c36c-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="5c36c-109">Cuando use EF Core para crear y mantener una base de datos basada en el modelo (ya sea con migraciones o `Database.EnsureCreated()`), se creará una tabla optimizada para memoria para estas entidades.</span><span class="sxs-lookup"><span data-stu-id="5c36c-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
