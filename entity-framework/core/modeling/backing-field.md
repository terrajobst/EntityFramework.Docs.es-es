---
title: 'Campos de respaldo: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 288440a4494117fe59d27187e24424c4d2fd44ab
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811878"
---
# <a name="backing-fields"></a>Campos de respaldo

> [!NOTE]  
> Esta característica es nueva en EF Core 1,1.

Los campos de respaldo permiten a EF leer o escribir en un campo en lugar de una propiedad. Esto puede ser útil cuando se usa la encapsulación en la clase para restringir el uso de y/o mejorar la semántica en torno al acceso a los datos por código de aplicación, pero el valor debe leerse o escribirse en la base de datos sin usar esas restricciones mejoras.

## <a name="conventions"></a>Convenciones

Por Convención, se detectarán los campos siguientes como campos de respaldo para una propiedad determinada (que se muestra en orden de prioridad). Los campos solo se detectan para las propiedades que se incluyen en el modelo. Para obtener más información sobre las propiedades que se incluyen en el modelo, vea [incluir & excluyendo las propiedades](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Cuando se configura un campo de respaldo, EF escribirá directamente en ese campo al materializar instancias de entidad de la base de datos (en lugar de usar el establecedor de propiedad). Si EF necesita leer o escribir el valor en otros momentos, usará la propiedad si es posible. Por ejemplo, si EF necesita actualizar el valor de una propiedad, usará el establecedor de la propiedad si se ha definido uno. Si la propiedad es de solo lectura, se escribirá en el campo.

## <a name="data-annotations"></a>Anotaciones de datos

Los campos de respaldo no se pueden configurar con anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar un campo de respaldo para una propiedad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Controlar cuándo se usa el campo

Puede configurar Cuándo EF utiliza el campo o la propiedad. Vea la [enumeración PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para ver las opciones admitidas.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Campos sin propiedad

También puede crear una propiedad conceptual en el modelo que no tiene una propiedad de CLR correspondiente en la clase de entidad, sino que usa un campo para almacenar los datos en la entidad. Esto es diferente de [las propiedades de las instantáneas](shadow-properties.md), donde los datos se almacenan en el seguimiento de cambios. Normalmente se utilizaría si la clase de entidad usa métodos para obtener o establecer valores.

Puede dar a EF el nombre del campo en el `Property(...)` API. Si no hay ninguna propiedad con el nombre especificado, EF buscará un campo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

Cuando no hay ninguna propiedad en la clase de entidad, puede usar el método `EF.Property(...)` en una consulta LINQ para hacer referencia a la propiedad que forma conceptualmente parte del modelo.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
