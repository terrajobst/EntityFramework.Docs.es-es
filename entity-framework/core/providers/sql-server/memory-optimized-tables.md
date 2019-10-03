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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Compatibilidad con tablas optimizadas para memoria en EF Core proveedor de bases de datos de SQL Server

[Las tablas optimizadas para memoria](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) son una característica de SQL Server en la que toda la tabla reside en memoria. Una segunda copia de los datos de la tabla se mantiene en el disco, pero solo con fines de durabilidad. Los datos de las tablas optimizadas para memoria solo se leen desde el disco durante la recuperación de la base de datos. Por ejemplo, después de reiniciar el servidor.

## <a name="configuring-a-memory-optimized-table"></a>Configurar una tabla optimizada para memoria

Puede especificar que la tabla a la que está asignada una entidad está optimizada para memoria. Cuando use EF Core para crear y mantener una base de datos basada en el modelo (ya sea con migraciones o `Database.EnsureCreated()`), se creará una tabla optimizada para memoria para estas entidades.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
