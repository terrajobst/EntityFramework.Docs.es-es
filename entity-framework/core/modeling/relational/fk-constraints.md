---
title: 'Restricciones Foreign Key: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655997"
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
