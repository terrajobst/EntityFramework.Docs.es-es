---
title: Base de datos SQLite proveedor - limitaciones - EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 69c40fcd8b7ddb925728b1bad9992ad2a81e7540
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994669"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Limitaciones del proveedor de base de datos de SQLite EF Core

El proveedor de SQLite tiene una serie de limitaciones de las migraciones. La mayoría de estas limitaciones es el resultado de las limitaciones en el motor de base de datos de SQLite subyacente y no es específica para EF.

## <a name="modeling-limitations"></a>Limitaciones del modelado

La biblioteca común de relacional (compartida por los proveedores de base de datos relacional de Entity Framework) define las API para la modelización de conceptos que son comunes a la mayoría de los motores de base de datos relacional. Un par de estos conceptos no se admiten por el proveedor de SQLite.

* Esquemas
* Secuencias

## <a name="migrations-limitations"></a>Limitaciones de las migraciones

El motor de base de datos de SQLite no admite un número de operaciones de esquema que son compatibles con la mayoría de otras bases de datos relacionales. Si intenta aplicar una de las operaciones no admitidas en una base de datos de SQLite un `NotSupportedException` se iniciará.

| Operación            | ¿Admite? | Requiere la versión |
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

Puede solucionar algunas de estas limitaciones escribiendo manualmente el código en las migraciones para realizar una tabla de volver a generar. Una recompilación de la tabla implica cambiar el nombre de la tabla existente, creando una nueva tabla, copiar los datos a la nueva tabla y eliminación de la tabla anterior. Deberá usar el `Sql(string)` método para realizar algunos de estos pasos.

Consulte [realizar otros tipos de tabla de cambios de esquema](http://sqlite.org/lang_altertable.html#otheralter) en la documentación de SQLite para obtener más detalles.

En el futuro, EF puede admitir algunas de estas operaciones mediante el enfoque de regeneración de la tabla en segundo plano. También puede [realizar un seguimiento de esta característica en nuestro proyecto de GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
