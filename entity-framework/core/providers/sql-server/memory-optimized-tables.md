---
title: Microsoft SQL Server Core EF de proveedor - tablas optimizadas en memoria - de base de datos
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 83a0decffc5946d036903b8b8add59f0ea31b21f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052645"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="4a63f-102">Compatible con tablas optimizadas en memoria en el proveedor de base de datos de SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="4a63f-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="4a63f-103">Esta característica se introdujo en EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="4a63f-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="4a63f-104">[Tablas optimizadas en memoria](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) son una característica de SQL Server donde reside toda la tabla en la memoria.</span><span class="sxs-lookup"><span data-stu-id="4a63f-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="4a63f-105">Una segunda copia de los datos de la tabla se conserva en el disco pero solo por la durabilidad.</span><span class="sxs-lookup"><span data-stu-id="4a63f-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="4a63f-106">Los datos de las tablas optimizadas para memoria solo se leen del disco durante la recuperación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a63f-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="4a63f-107">Por ejemplo, después de reiniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="4a63f-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="4a63f-108">Configuración de una tabla con optimización para memoria</span><span class="sxs-lookup"><span data-stu-id="4a63f-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="4a63f-109">Puede especificar que la tabla de a que una entidad se asigna está optimizada en memoria.</span><span class="sxs-lookup"><span data-stu-id="4a63f-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="4a63f-110">Cuando mediante el uso de EF Core para crear y mantener una base de datos en función del modelo (ya sea con migraciones o `Database.EnsureCreated()`), se creará una tabla optimizada en memoria para estas entidades.</span><span class="sxs-lookup"><span data-stu-id="4a63f-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
