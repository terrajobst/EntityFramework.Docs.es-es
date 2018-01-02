---
title: Proveedores de bases de datos - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 19c275b7e89c62e79c8bded977e39b2cfb2b439a
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="database-providers"></a>Proveedores de bases de datos

Entity Framework Core usa un modelo de proveedor para permitir que se use EF para acceder a muchas bases de datos diferentes. Algunos conceptos son comunes a la mayoría de las bases de datos y se incluyen en los componentes principales de EF Core. Estos conceptos incluyen la expresión de consultas en LINQ, las transacciones y el seguimiento de cambios en objetos una vez cargados desde la base de datos. Algunos conceptos son específicos de un proveedor determinado. Por ejemplo, el proveedor de SQL Server permite configurar tablas optimizadas para memoria (una característica específica de SQL Server). Otros conceptos son específicos de una clase de proveedores. Por ejemplo, los proveedores de EF Core para bases de datos relacionales se basan en la biblioteca común `Microsoft.EntityFrameworkCore.Relational`, que proporciona API para configurar asignaciones de tabla y columna, restricciones de clave externa, etc.

Los proveedores de EF Core se componen de una serie de orígenes. No todos los proveedores se mantienen como parte del proyecto Entity Framework Core. Al considerar un proveedor de terceros, evalúe la calidad, las licencias, el soporte técnico, etc. a fin de asegurarse de que satisface los requisitos.
