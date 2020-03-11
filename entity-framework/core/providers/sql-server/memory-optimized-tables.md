---
title: 'Proveedor de base de datos de Microsoft SQL Server: tablas con optimización para memoria-EF Core'
description: Cómo usar tablas optimizadas para memoria con el proveedor de base de datos de SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414768"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="35060-103">Compatibilidad con tablas optimizadas para memoria en EF Core proveedor de bases de datos de SQL Server</span><span class="sxs-lookup"><span data-stu-id="35060-103">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="35060-104">[Las tablas optimizadas para memoria](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) son una característica de SQL Server en la que toda la tabla reside en memoria.</span><span class="sxs-lookup"><span data-stu-id="35060-104">[Memory-Optimized Tables](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="35060-105">Una segunda copia de los datos de la tabla se conserva en el disco pero solo por la durabilidad.</span><span class="sxs-lookup"><span data-stu-id="35060-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="35060-106">Los datos de las tablas optimizadas para memoria solo se leen del disco durante la recuperación de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="35060-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="35060-107">Por ejemplo, después de reiniciar el servidor.</span><span class="sxs-lookup"><span data-stu-id="35060-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="35060-108">Configurar una tabla optimizada para memoria</span><span class="sxs-lookup"><span data-stu-id="35060-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="35060-109">Puede especificar que la tabla a la que está asignada una entidad está optimizada para memoria.</span><span class="sxs-lookup"><span data-stu-id="35060-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="35060-110">Al usar EF Core para crear y mantener una base de datos basada en el modelo (ya sea con [migraciones](xref:core/managing-schemas/migrations/index) o [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), se creará una tabla optimizada para memoria para estas entidades.</span><span class="sxs-lookup"><span data-stu-id="35060-110">When using EF Core to create and maintain a database based on your model (either with [migrations](xref:core/managing-schemas/migrations/index) or [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), a memory-optimized table will be created for these entities.</span></span>

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
