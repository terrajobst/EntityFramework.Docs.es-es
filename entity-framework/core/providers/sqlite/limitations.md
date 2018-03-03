---
title: Base de datos de SQLite proveedor - limitaciones - EF Core
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 8a60ccfc61a5757df8ebedf257379d4d3dbffbf6
ms.sourcegitcommit: 60b831318c4f5ec99061e8af6a7c9e7c03b3469c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Limitaciones del proveedor de base de datos de SQLite EF Core

El proveedor de código tiene una serie de limitaciones de las migraciones. La mayoría de estas limitaciones es el resultado de las limitaciones en el motor de base de datos de código subyacente y no es específica de EF.

## <a name="modeling-limitations"></a>Limitaciones de modelado

La biblioteca relacional común (compartida por los proveedores de base de datos relacional de Entity Framework) define las API para la modelización conceptos que son comunes a la mayoría de los motores de base de datos relacional. Un par de estos conceptos no se admiten por el proveedor de código.

* Esquemas
* Secuencias

## <a name="migrations-limitations"></a>Limitaciones de las migraciones

El motor de base de datos de SQLite no es compatible con un número de operaciones de esquema que son compatibles con la mayoría de las otras bases de datos relacionales. Si intenta aplicar una de las operaciones no admitidas en una base de datos de SQLite un `NotSupportedException` se iniciará.

| Operación            | ¿Es compatible? | Requiere la versión |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| CreateTable          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DropTable            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✗          |                  |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (no operativo)  | 2.0              |
| DropSchema           | ✔ (no operativo)  | 2.0              |
| Insertar               | ✔          | 2.0              |
| Actualizar               | ✔          | 2.0              |
| Eliminar               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Solución alternativa de las limitaciones de las migraciones

Puede solucionar algunas de estas limitaciones, escriba manualmente el código en las migraciones para realizar una tabla volver a generar. Una recompilación de tabla implica cambiar el nombre de la tabla existente, crea una nueva tabla, copiar los datos a la nueva tabla y eliminación de la tabla anterior. Debe utilizar el `Sql(string)` método para realizar algunos de estos pasos.

Vea [realizar otros tipos de tabla de cambios de esquema](http://sqlite.org/lang_altertable.html#otheralter) en la documentación de SQLite para obtener más detalles.

En el futuro, EF puede admitir algunas de estas operaciones mediante el enfoque de regeneración de tabla en segundo plano. También puede [realizar el seguimiento de esta característica en nuestro proyecto de GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
