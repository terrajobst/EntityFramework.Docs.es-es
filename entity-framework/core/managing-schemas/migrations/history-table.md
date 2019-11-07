---
title: 'Tabla de historial de migraciones personalizadas: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655716"
---
# <a name="custom-migrations-history-table"></a>Tabla de historial de migraciones personalizadas

De forma predeterminada, EF Core realiza un seguimiento de las migraciones que se han aplicado a la base de datos mediante su grabación en una tabla denominada `__EFMigrationsHistory`. Por varias razones, puede que desee personalizar esta tabla para satisfacer mejor sus necesidades.

> [!IMPORTANT]
> Si personaliza la tabla de historial de migraciones *después* de aplicar las migraciones, es responsable de actualizar la tabla existente en la base de datos.

## <a name="schema-and-table-name"></a>Esquema y nombre de tabla

Puede cambiar el esquema y el nombre de la tabla mediante el método `MigrationsHistoryTable()` en `OnConfiguring()` (o `ConfigureServices()` en ASP.NET Core). Este es un ejemplo del uso del proveedor de EF Core de SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a>Otros cambios

Para configurar aspectos adicionales de la tabla, invalide y reemplace el servicio de `IHistoryRepository` específico del proveedor. Este es un ejemplo de cómo cambiar el nombre de la columna MigrationId a *ID* en SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> `SqlServerHistoryRepository` está dentro de un espacio de nombres interno y puede cambiar en futuras versiones.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
