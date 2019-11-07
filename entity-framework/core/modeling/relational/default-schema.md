---
title: EF Core de esquema predeterminados
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655965"
---
# <a name="default-schema"></a>Esquema predeterminado

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

El esquema predeterminado es el esquema de la base de datos en el que se crearán los objetos si no se configura explícitamente un esquema para ese objeto.

## <a name="conventions"></a>Convenciones

Por Convención, el proveedor de base de datos elegirá el esquema predeterminado más adecuado. Por ejemplo, Microsoft SQL Server usará el esquema de `dbo` y SQLite no usará un esquema (dado que los esquemas no se admiten en SQLite).

## <a name="data-annotations"></a>Anotaciones de datos

No se puede establecer el esquema predeterminado con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para especificar un esquema predeterminado.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
