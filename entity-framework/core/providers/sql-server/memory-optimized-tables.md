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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Compatible con tablas optimizadas en memoria en el proveedor de base de datos de SQL Server EF Core

> [!NOTE]  
>
> Esta característica se introdujo en EF Core 1.1.

[Tablas optimizadas en memoria](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) son una característica de SQL Server donde reside toda la tabla en la memoria. Una segunda copia de los datos de la tabla se conserva en el disco pero solo por la durabilidad. Los datos de las tablas optimizadas para memoria solo se leen del disco durante la recuperación de la base de datos. Por ejemplo, después de reiniciar el servidor.

## <a name="configuring-a-memory-optimized-table"></a>Configuración de una tabla con optimización para memoria

Puede especificar que la tabla de a que una entidad se asigna está optimizada en memoria. Cuando mediante el uso de EF Core para crear y mantener una base de datos en función del modelo (ya sea con migraciones o `Database.EnsureCreated()`), se creará una tabla optimizada en memoria para estas entidades.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
