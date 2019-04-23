---
title: Claves (principal) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 51d163b867085f42f415dbd7afa9e311ab1781a0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929841"
---
# <a name="keys-primary"></a>Claves (principal)

Una clave actúa como identificador único para cada instancia de la entidad principal. Cuando se usa una base de datos relacional se asigna al concepto de un *clave principal*. También puede configurar un identificador único que no es la clave principal (consulte [claves alternativas](alternate-keys.md) para obtener más información). 

Uno de los métodos siguientes se puede usar el programa de instalación o crear una clave principal.

## <a name="conventions"></a>Convenciones

Por convención, una propiedad denominada `Id` o `<type name>Id` se configurarán como la clave de una entidad.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
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

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para configurar una propiedad única para que sea la clave de una entidad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

También puede usar la API Fluent para configurar varias propiedades para que sea la clave de una entidad (conocida como una clave compuesta). Las claves compuestas solo se pueden configurar mediante la API de Fluent: convenciones nunca realizará la instalación de una clave compuesta y no se pueden usar anotaciones de datos para configurar uno.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
