---
title: Índices (base de datos relacional)-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824579"
---
# <a name="indexes-relational-database"></a>Índices (base de datos relacional)

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Un índice en una base de datos relacional se asigna al mismo concepto que un índice en el núcleo de Entity Framework.

## <a name="conventions"></a>Convenciones

Por Convención, los índices se denominan `IX_<type name>_<property name>`. En el caso de los índices compuestos `<property name>` se convierte en una lista de nombres de propiedad separados por guiones bajos.

## <a name="data-annotations"></a>Anotaciones de datos

Los índices no se pueden configurar con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar el nombre de un índice.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

También puede especificar un filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

Al usar el proveedor de SQL Server EF agrega un filtro de `'IS NOT NULL'` para todas las columnas que aceptan valores NULL que forman parte de un índice único. Para invalidar esta Convención, puede proporcionar un valor `null`.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Incluir columnas en índices de SQL Server

Puede configurar [índices con columnas incluidas](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) para mejorar significativamente el rendimiento de las consultas cuando todas las columnas de la consulta se incluyen en el índice como columnas de clave o que no son de clave.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
