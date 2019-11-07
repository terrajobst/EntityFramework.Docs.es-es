---
title: Claves (principal)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655947"
---
# <a name="keys-primary"></a>Claves (principales)

Una clave actúa como identificador único principal para cada instancia de entidad. Al utilizar una base de datos relacional, se asigna al concepto de una *clave principal*. También puede configurar un identificador único que no sea la clave principal (consulte [claves alternativas](alternate-keys.md) para obtener más información).

Se puede usar uno de los métodos siguientes para configurar o crear una clave principal.

## <a name="conventions"></a>Convenciones

Por Convención, una propiedad denominada `Id` o `<type name>Id` se configurará como la clave de una entidad.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para configurar una propiedad única para que sea la clave de una entidad.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una propiedad única para que sea la clave de una entidad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

También puede usar la API fluida para configurar varias propiedades de forma que sean la clave de una entidad (conocida como clave compuesta). Las claves compuestas solo se pueden configurar mediante las convenciones de API fluidas nunca instalarán una clave compuesta y no se pueden usar anotaciones de datos para configurar una.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
