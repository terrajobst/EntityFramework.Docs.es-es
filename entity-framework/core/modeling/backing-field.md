---
title: Seguridad de los campos - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994098"
---
# <a name="backing-fields"></a>Campos de respaldo

> [!NOTE]  
> Esta característica es nueva en EF Core 1.1.

Los campos de respaldo para permitir EF leer o escribir en un campo en lugar de una propiedad. Esto puede ser útil cuando la encapsulación de la clase se utiliza para mejorar la semántica de relación con el acceso a los datos y restringir el uso de código de aplicación, pero el valor se debería leen o escriben en la base de datos sin usar esas restricciones / mejoras.

## <a name="conventions"></a>Convenciones

Por convención, se detectarán los siguientes campos como campos para una propiedad determinada (que se muestran en orden de prioridad) de respaldo. Solo se detectan los campos para las propiedades que se incluyen en el modelo. Para obtener más información en el que se incluyan las propiedades en el modelo, vea [inclusión y exclusión de propiedades](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Cuando se configura un campo de respaldo, EF escribirá directamente en ese campo al materializar las instancias de entidad desde la base de datos (en lugar de utilizar el establecedor de propiedad). Si EF necesita para leer o escribir el valor en otras ocasiones, usará la propiedad si es posible. Por ejemplo, si necesita actualizar el valor de una propiedad de EF, usará el establecedor de propiedad si hay alguno definido. Si la propiedad es de solo lectura, escribirá en el campo.

## <a name="data-annotations"></a>Anotaciones de datos

Campos de respaldo no pueden configurarse con las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para configurar un campo de respaldo para una propiedad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Cuando se usa el campo de control

Puede configurar cuando EF usa el campo o propiedad. Consulte la [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para las opciones admitidas.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Campos sin una propiedad

También puede crear una propiedad conceptual en el modelo que no tiene una propiedad CLR correspondiente en la clase de entidad, sino que usa un campo para almacenar los datos de la entidad. Esto es diferente de [reemplazar propiedades](shadow-properties.md), donde los datos se almacenan en el seguimiento de cambios. Esto normalmente se usaría si la clase de entidad usa los métodos para obtener o establecer valores.

Puede dar el nombre del campo en EF la `Property(...)` API. Si no hay ninguna propiedad con el nombre especificado, EF buscará un campo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

También puede dar un nombre distinto del nombre de campo a la propiedad. Este nombre, a continuación, se usa al crear el modelo, sobre todo se usará para el nombre de columna que se asigna a la base de datos.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Cuando no hay ninguna propiedad en la clase de entidad, puede usar el `EF.Property(...)` método en una consulta LINQ para hacer referencia a la propiedad que conceptualmente es parte del modelo.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
