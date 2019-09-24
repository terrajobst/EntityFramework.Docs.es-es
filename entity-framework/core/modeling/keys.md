---
title: Claves (principal)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 8b32bf6417890a954c933a5973a2c90c609beeca
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197272"
---
# <a name="keys-primary"></a>Claves (principal)

Una clave actúa como identificador único principal para cada instancia de entidad. Al utilizar una base de datos relacional, se asigna al concepto de una *clave principal*. También puede configurar un identificador único que no sea la clave principal (consulte [claves alternativas](alternate-keys.md) para obtener más información). 

Se puede usar uno de los métodos siguientes para configurar o crear una clave principal.

## <a name="conventions"></a>Convenciones

Por Convención, se configurará `<type name>Id` una propiedad denominada `Id` o como la clave de una entidad.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para configurar una propiedad única para que sea la clave de una entidad.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una propiedad única para que sea la clave de una entidad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

También puede usar la API fluida para configurar varias propiedades de forma que sean la clave de una entidad (conocida como clave compuesta). Las claves compuestas solo se pueden configurar mediante las convenciones de API fluidas nunca instalarán una clave compuesta y no se pueden usar anotaciones de datos para configurar una.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
