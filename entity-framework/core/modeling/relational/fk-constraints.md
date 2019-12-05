---
title: 'Restricciones Foreign Key: EF Core'
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824584"
---
# <a name="foreign-key-constraints"></a>Restricciones de clave externa

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

Se introduce una restricción FOREIGN KEY para cada relación del modelo.

## <a name="conventions"></a>Convenciones

Por Convención, las restricciones Foreign Key se denominan `FK_<dependent type name>_<principal type name>_<foreign key property name>`. En el caso de las claves externas compuestas `<foreign key property name>` se convierte en una lista de nombres de propiedades de clave externa separadas por guiones bajos.

## <a name="data-annotations"></a>Anotaciones de datos

Los nombres de restricción de clave externa no se pueden configurar con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar el nombre de la restricción de clave externa para una relación.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
