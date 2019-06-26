---
title: Base de datos SQLite proveedor - limitaciones - EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: eaa7d5b1496172e4f3821433a1cd098ee7e8b737
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394800"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Limitaciones del proveedor de base de datos de SQLite EF Core

El proveedor de SQLite tiene una serie de limitaciones de las migraciones. La mayoría de estas limitaciones es el resultado de las limitaciones en el motor de base de datos de SQLite subyacente y no es específica para EF.

## <a name="modeling-limitations"></a>Limitaciones del modelado

La biblioteca común de relacional (compartida por los proveedores de base de datos relacional de Entity Framework) define las API para la modelización de conceptos que son comunes a la mayoría de los motores de base de datos relacional. Un par de estos conceptos no se admiten por el proveedor de SQLite.

* Esquemas
* Secuencias
* Columnas calculadas

## <a name="query-limitations"></a>Limitaciones de la consulta

SQLite no admite de forma nativa los siguientes tipos de datos. EF Core puede leer y escribir valores de estos tipos y consultas de igualdad (`where e.Property == value`) también es compatible con. Otras operaciones, sin embargo, al igual que la comparación y ordenación requerirá la evaluación en el cliente.

* DateTimeOffset
* Decimal
* TimeSpan
* UInt64

En lugar de `DateTimeOffset`, recomendamos usar valores de fecha y hora. Al controlar varias zonas horarias, se recomienda convertir los valores a la hora UTC antes de guardar y, a continuación, volver a convertir a la zona horaria adecuada.

El `Decimal` tipo proporciona un alto nivel de precisión. Si no necesita ese nivel de precisión, sin embargo, se recomienda usar dobles en su lugar. Puede usar un [convertidor](../../modeling/value-conversions.md) para seguir usando el separador decimal en las clases.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

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
| RenameColumn         | ✔          | 2.2.2            |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (no operativo)  | 2.0              |
| DropSchema           | ✔ (no operativo)  | 2.0              |
| Insertar               | ✔          | 2.0              |
| Actualizar               | ✔          | 2.0              |
| Eliminar               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Solución alternativa de las limitaciones de las migraciones

Puede solucionar algunas de estas limitaciones escribiendo manualmente el código en las migraciones para realizar una tabla de volver a generar. Una recompilación de la tabla implica cambiar el nombre de la tabla existente, crear otra tabla, copiar los datos en la tabla nueva y eliminar la anterior. Deberá usar el `Sql(string)` método para realizar algunos de estos pasos.

Consulte [realizar otros tipos de tabla de cambios de esquema](http://sqlite.org/lang_altertable.html#otheralter) en la documentación de SQLite para obtener más detalles.

En el futuro, EF puede admitir algunas de estas operaciones mediante el enfoque de regeneración de la tabla en segundo plano. También puede [realizar un seguimiento de esta característica en nuestro proyecto de GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
