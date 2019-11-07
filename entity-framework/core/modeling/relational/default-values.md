---
title: 'Valores predeterminados: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655901"
---
# <a name="default-values"></a>Valores predeterminados

> [!NOTE]  
> La configuración de esta sección se aplica a bases de datos relacionales en general. Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).

El valor predeterminado de una columna es el valor que se insertará si se inserta una nueva fila pero no se especifica ningún valor para la columna.

## <a name="conventions"></a>Convenciones

Por Convención, no se configura un valor predeterminado.

## <a name="data-annotations"></a>Anotaciones de datos

No se puede establecer un valor predeterminado mediante anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para especificar el valor predeterminado de una propiedad.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

También puede especificar un fragmento de SQL que se utiliza para calcular el valor predeterminado.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
