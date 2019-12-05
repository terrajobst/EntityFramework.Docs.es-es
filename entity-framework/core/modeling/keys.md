---
title: Claves (principal)-EF Core
description: Cómo configurar claves para tipos de entidad al utilizar Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824622"
---
# <a name="keys-primary"></a>Claves (principales)

Una clave actúa como identificador único principal para cada instancia de entidad. Al utilizar una base de datos relacional, se asigna al concepto de una *clave principal*. También puede configurar un identificador único que no sea la clave principal (consulte [claves alternativas](alternate-keys.md) para obtener más información).

Se puede usar uno de los métodos siguientes para configurar o crear una clave principal.

## <a name="conventions"></a>Convenciones

De forma predeterminada, una propiedad denominada `Id` o `<type name>Id` se configurará como la clave de una entidad.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> Los [tipos de entidad de propiedad](xref:core/modeling/owned-entities) usan reglas diferentes para definir claves.

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para configurar una propiedad única para que sea la clave de una entidad.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una propiedad única para que sea la clave de una entidad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

También puede usar la API fluida para configurar varias propiedades de forma que sean la clave de una entidad (conocida como clave compuesta). Las claves compuestas solo se pueden configurar mediante las convenciones de API fluidas nunca instalarán una clave compuesta y no se pueden usar anotaciones de datos para configurar una.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a>Tipos de clave y valores

EF Core admite el uso de propiedades de cualquier tipo primitivo como clave principal, incluidos `string`, `Guid`, `byte[]` y otros. Pero no todas las bases de datos las admiten. En algunos casos, los valores de clave se pueden convertir automáticamente a un tipo compatible, de lo contrario, la conversión se debe [especificar manualmente](xref:core/modeling/value-conversions).

Las propiedades de clave deben tener siempre un valor no predeterminado al agregar una nueva entidad al contexto, pero [la base de datos generará](xref:core/modeling/generated-properties)algunos tipos. En ese caso, EF intentará generar un valor temporal cuando la entidad se agregue con fines de seguimiento. Después de llamar a [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) , el valor temporal se reemplazará por el valor generado por la base de datos.

> [!Important]
> Si una propiedad de clave tiene el valor generado por la base de datos y se especifica un valor no predeterminado al agregar una entidad, EF asumirá que la entidad ya existe en la base de datos e intentará actualizarla en lugar de insertar una nueva. Para evitar esto, desactive la generación de valores o vea [Cómo especificar valores explícitos para las propiedades generadas](../saving/explicit-values-generated-properties.md).