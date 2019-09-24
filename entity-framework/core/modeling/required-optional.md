---
title: 'Propiedades obligatorias y opcionales: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: fd9e96e6f79965e63b07c21217edd004fd5c4d54
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197843"
---
# <a name="required-and-optional-properties"></a>Propiedades obligatorias y opcionales

Una propiedad se considera opcional si es válida para que la contenga `null`. Si `null` no es un valor válido que se va a asignar a una propiedad, se considera que es una propiedad obligatoria.

Al asignar a un esquema de base de datos relacional, las propiedades requeridas se crean como columnas que no aceptan valores NULL y las propiedades opcionales se crean como columnas que aceptan valores NULL.

## <a name="conventions"></a>Convenciones

Por Convención, una propiedad cuyo tipo .NET pueda contener NULL se configurará como opcional, mientras que las propiedades cuyo tipo .NET no puede contener valores NULL se configurarán según sea necesario. Por ejemplo, todas las propiedades con tipos de valor`int`.net `decimal`( `bool`,,, etc.) se configuran como necesario y todas las propiedades con tipos de valor `decimal?`de `bool?`.net que aceptan valores NULL (`int?`,,, etc.) son configurado como opcional.

C#8 presentó una nueva característica denominada [tipos de referencia que aceptan valores NULL](/dotnet/csharp/tutorials/nullable-reference-types), que permite anotar tipos de referencia, lo que indica si es válido para que contengan null o not. Esta característica está deshabilitada de forma predeterminada y, si está habilitada, modifica el comportamiento de EF Core de la siguiente manera:

* Si los tipos de referencia que aceptan valores NULL están deshabilitados (el valor predeterminado), todas las propiedades con tipos de referencia de `string`.net se configuran como opcionales por Convención (por ejemplo,).
* Si los tipos de referencia que aceptan valores NULL están habilitados, las propiedades C# se configurarán en función de `string?` la nulabilidad de su tipo .net `string` : se configurarán como opcionales, mientras que se configurarán según sea necesario.

En el ejemplo siguiente se muestra un tipo de entidad con propiedades obligatorias y opcionales, con la característica de referencia que acepta valores NULL deshabilitada (valor predeterminado) y habilitada:

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[Sin tipos de referencia que aceptan valores NULL (valor predeterminado)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[Con tipos de referencia que aceptan valores NULL](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

Se recomienda el uso de tipos de referencia que aceptan valores NULL, ya que C# fluye la nulabilidad expresada en el código para EF Core modelo y en la base de datos, e obvia el uso de las anotaciones de datos o la API fluida para expresar el mismo concepto dos veces.

> [!NOTE]
> Tenga cuidado al habilitar los tipos de referencia que aceptan valores NULL en un proyecto existente: las propiedades de tipo de referencia que se configuraron anteriormente como opcional ahora se configurarán según sea necesario, a menos que se anoten explícitamente para que acepten valores NULL. Al administrar un esquema de base de datos relacional, esto puede provocar que se generen migraciones que modifiquen la nulabilidad de la columna de la base de datos.

Para obtener más información sobre los tipos de referencia que aceptan valores NULL y cómo usarlos con EF Core, [consulte la página de documentación dedicada para esta característica](xref:core/miscellaneous/nullable-reference-types).

## <a name="configuration"></a>Configuración

Una propiedad que sería opcional por Convención se puede configurar para que sea necesaria de la siguiente manera:

# <a name="data-annotationstabdata-annotations"></a>[Anotaciones de datos](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[API fluida](#tab/fluent-api) 

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***