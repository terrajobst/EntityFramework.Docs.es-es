---
title: Índices - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
ms.locfileid: "29679004"
---
# <a name="indexes"></a>Índices

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Un índice en una base de datos relacional se asigna para el mismo concepto que un índice en el núcleo de Entity Framework.

## <a name="conventions"></a>Convenciones

Por convención, se denominan índices `IX_<type name>_<property name>`. En los índices compuestos `<property name>` se convierte en una lista de subrayado separado de los nombres de propiedad.

## <a name="data-annotations"></a>Anotaciones de datos

Los índices no pueden configurarse mediante las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar el nombre de un índice.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

También puede especificar un filtro.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Cuando utiliza el proveedor SQL Server EF agrega un 'es NOT NULL' filtrar para todas las columnas que aceptan valores null que forman parte de un índice único. Para reemplazar esta convención puede proporcionar un `null` valor.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
