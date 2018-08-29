---
title: Índices - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993222"
---
# <a name="indexes"></a>Índices

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Un índice en una base de datos relacional se asigna en el mismo concepto que un índice en el núcleo de Entity Framework.

## <a name="conventions"></a>Convenciones

Por convención, se denominan índices `IX_<type name>_<property name>`. Para los índices compuestos `<property name>` se convierte en una lista separada por un carácter de subrayado de los nombres de propiedad.

## <a name="data-annotations"></a>Anotaciones de datos

Los índices no se pueden configurar mediante las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para configurar el nombre de un índice.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

También puede especificar un filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Cuando se agrega mediante el proveedor de SQL Server EF filtrar un 'IS NOT NULL' para todas las columnas que aceptan valores null que forman parte de un índice único. Para invalidar esta convención puede proporcionar un `null` valor.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
