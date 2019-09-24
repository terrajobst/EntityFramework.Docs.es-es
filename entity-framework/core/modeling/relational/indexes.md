---
title: 'Índices: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7dcf27dedbde45302a462a4c41a811b9868e40bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197014"
---
# <a name="indexes"></a>Índices

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Un índice en una base de datos relacional se asigna al mismo concepto que un índice en el núcleo de Entity Framework.

## <a name="conventions"></a>Convenciones

Por Convención, los índices se denominan `IX_<type name>_<property name>`. En el caso de `<property name>` los índices compuestos se convierte en una lista de nombres de propiedad separados por guiones bajos.

## <a name="data-annotations"></a>Anotaciones de datos

Los índices no se pueden configurar con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar el nombre de un índice.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

También puede especificar un filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

Al usar el proveedor de SQL Server EF agrega un filtro ' IS NOT NULL ' para todas las columnas que aceptan valores NULL que forman parte de un índice único. Para invalidar esta Convención, puede proporcionar `null` un valor.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Incluir columnas en índices de SQL Server

Puede configurar [índices con columnas incluidas](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) para mejorar significativamente el rendimiento de las consultas cuando todas las columnas de la consulta se incluyen en el índice como columnas de clave o que no son de clave.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
