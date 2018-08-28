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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Compatibilidad con tablas optimizadas para memoria en el proveedor de base de datos de SQL Server EF Core

> [!NOTE]  
>
> Esta característica se introdujo en EF Core 1.1.

[Tablas optimizadas para memoria](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) son una característica de SQL Server donde reside toda la tabla en memoria. Una segunda copia de la tabla de datos se mantiene en el disco, pero solo con fines de durabilidad. Datos en tablas optimizadas para memoria solo se leen del disco durante la recuperación de base de datos. Por ejemplo, un servidor después de reiniciar.

## <a name="configuring-a-memory-optimized-table"></a>Configuración de una tabla optimizada para memoria

Puede especificar que la tabla a la que está asignada una entidad está optimizada para memoria. Cuando use EF Core para crear y mantener una base de datos basada en el modelo (ya sea con migraciones o `Database.EnsureCreated()`), se creará una tabla optimizada para memoria para estas entidades.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
