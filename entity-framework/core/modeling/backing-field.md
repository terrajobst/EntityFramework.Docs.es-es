---
title: Realizar una copia de los campos - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2017
---
# <a name="backing-fields"></a>Campos de respaldo

> [!NOTE]  
> Esta característica es nueva en EF Core 1.1.

Campos de respaldo permiten EF leer o escribir en un campo en lugar de una propiedad. Esto puede resultar útil cuando se utiliza para mejorar la semántica de relación con el acceso a los datos o restringir el uso de encapsulación de la clase mediante código de aplicación, pero el valor se debe leer desde o escribir en la base de datos sin utilizar esas restricciones / mejoras.

## <a name="conventions"></a>Convenciones

Por convención, se detectarán los siguientes campos como realizar una copia de los campos de una propiedad determinada (que se muestran en orden de prioridad). Solo se detectan los campos para las propiedades que se incluyen en el modelo. Para obtener más información en el que se incluyen propiedades en el modelo, vea [incluidos & Excluir propiedades](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Cuando se configura un campo de respaldo, EF escribirá directamente en ese campo al materializar instancias de entidad desde la base de datos (en lugar de usar el establecedor de propiedades). Si necesita EF leer o escribir el valor en otras ocasiones, utilizará la propiedad si es posible. Por ejemplo, si EF debe actualizar el valor de una propiedad, usará el establecedor de propiedad si se ha definido uno. Si la propiedad es de solo lectura, se escribirá en el campo.

## <a name="data-annotations"></a>Anotaciones de datos

Realizar una copia de los campos no se puede configurar con las anotaciones de datos.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar un campo de respaldo para una propiedad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Controlar cuando se usa el campo

Puede configurar cuando EF usa el campo o propiedad. Consulte la [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) para las opciones admitidas.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Campos sin una propiedad

También puede crear una propiedad conceptual en el modelo que no tiene una propiedad CLR correspondiente en la clase de entidad, sino que utiliza un campo para almacenar los datos de la entidad. Esto es diferente de [reemplazar propiedades](shadow-properties.md), donde se almacenan los datos en la herramienta de seguimiento de cambios. Esto lo que normalmente se utiliza si la clase de entidad utiliza métodos para obtener o establecer valores.

Puede asignar EF el nombre del campo en el `Property(...)` API. Si no hay ninguna propiedad con el nombre especificado, EF buscará un campo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

También puede asignar un nombre distinto del nombre de campo a la propiedad. Este nombre se utiliza al crear el modelo, sobre todo se usará para el nombre de columna que se asigna a la base de datos.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Cuando no hay ninguna propiedad en la clase de entidad, puede usar el `EF.Property(...)` método en una consulta LINQ para hacer referencia a la propiedad que es conceptualmente parte del modelo.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
