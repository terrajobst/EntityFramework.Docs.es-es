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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Compatibilidad con tablas optimizadas para memoria en EF Core proveedor de bases de datos de SQL Server

[Las tablas optimizadas para memoria](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) son una característica de SQL Server en la que toda la tabla reside en memoria. Una segunda copia de los datos de la tabla se conserva en el disco pero solo por la durabilidad. Los datos de las tablas optimizadas para memoria solo se leen del disco durante la recuperación de la base de datos. Por ejemplo, después de reiniciar el servidor.

## <a name="configuring-a-memory-optimized-table"></a>Configurar una tabla optimizada para memoria

Puede especificar que la tabla a la que está asignada una entidad está optimizada para memoria. Al usar EF Core para crear y mantener una base de datos basada en el modelo (ya sea con [migraciones](xref:core/managing-schemas/migrations/index) o [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), se creará una tabla optimizada para memoria para estas entidades.

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
