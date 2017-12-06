---
title: Valores generados - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
ms.technology: entity-framework-core
uid: core/modeling/generated-properties
ms.openlocfilehash: 2d79bf1339ebe522c39fe8971d908c30e1f4dca0
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="generated-values"></a>Valores generados

## <a name="value-generation-patterns"></a>Valor de generación de modelos

Hay tres patrones de generación de valor que se pueden usar para las propiedades.

### <a name="no-value-generation"></a>No se genera valor

No se genera valor significa que siempre proporcione un valor válido van a guardar en la base de datos. Este valor válido debe asignarse a las nuevas entidades antes de que se agregan al contexto.

### <a name="value-generated-on-add"></a>Valor generado en Agregar

Valor generado en complemento significa que se genera un valor para las nuevas entidades.

Dependiendo del proveedor de base de datos que se va a utilizar, los valores pueden ser generado cliente EF o en la base de datos. Si el valor se genera la base de datos, EF puede asignar un valor temporal cuando se agrega la entidad al contexto. Este valor temporal, a continuación, se reemplazará por el valor de la base de datos generada durante la `SaveChanges()`.

Si agrega una entidad en el contexto que tiene un valor asignado a la propiedad, EF intentará insertar ese valor en lugar de generar uno nuevo. Una propiedad se considera que tiene un valor asignado si no se asigna el valor predeterminado CLR (`null` para `string`, `0` para `int`, `Guid.Empty` para `Guid`, etcetera.). Para obtener más información, consulte [valores explícitos para propiedades generadas](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Cómo se genera el valor para entidades agregadas dependerá del proveedor de base de datos que se está usando. Proveedores de base de datos pueden automáticamente el programa de instalación generar valores para algunos tipos de propiedad, pero otros pueden requerir que el programa de instalación manualmente cómo se genera el valor.
>
> Por ejemplo, cuando se usa SQL Server, los valores se generará automáticamente para `GUID` propiedades (utilizando el algoritmo GUID secuencial de SQL Server). Sin embargo, si especifica que un `DateTime` generar propiedad en Agregar, a continuación, debe configurar una manera para que los valores que se genere. Una manera de hacer esto, consiste en configurar un valor predeterminado de `GETDATE()`, consulte [valores predeterminados](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valor generado en Agregar o actualizar

Valor generado en Agregar o actualizar significa que se genera un nuevo valor cada vez que se guarda el registro (insert o update).

Al igual que `value generated on add`, si especifica un valor para la propiedad en una instancia recién agregada de una entidad, que se insertará el valor en lugar de un valor que se está generando. También es posible establecer un valor explícito al actualizar. Para obtener más información, consulte [valores explícitos para propiedades generadas](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Cómo se genera el valor para las entidades agregadas y actualizadas dependerá del proveedor de base de datos que se está usando. Proveedores de base de datos pueden automáticamente el programa de instalación generar valores para algunos tipos de propiedades, mientras que otros usuarios, deberá configurar manualmente cómo se genera el valor.
>
> Por ejemplo, cuando se usa SQL Server, `byte[]` propiedades que se establecen como generadas en Agregar o actualizar y marcados como tokens de simultaneidad, se configurará con la `rowversion` de tipo de datos - para que se generarán valores en la base de datos. Sin embargo, si especifica que un `DateTime` generar propiedad en Agregar o actualizar, a continuación, debe configurar una manera para que los valores que se genere. Una manera de hacer esto, consiste en configurar un valor predeterminado de `GETDATE()` (consulte [valores predeterminados](relational/default-values.md)) para generar valores para las nuevas filas. A continuación, podría utilizar un desencadenador de base de datos para generar valores durante las actualizaciones (por ejemplo, el siguiente desencadenador de ejemplo).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Convenciones

Por convención, las claves principales que son de un entero o un tipo de datos GUID será el programa de instalación tiene valores generados en Agregar. Todas las demás propiedades se configurará con la generación de ningún valor.

## <a name="data-annotations"></a>Anotaciones de datos

### <a name="no-value-generation-data-annotations"></a>No se genera valor (anotaciones de datos)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valor generado en Agregar (anotaciones de datos)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Esto permite EF sabe que los valores se generan para entidades agregadas, no garantiza que EF configurará el mecanismo real para generar valores. Vea [agregar valor generado en](#value-generated-on-add) sección para obtener más detalles.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valor generado en Agregar o actualizar (anotaciones de datos)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Esto permite EF sabe que se generan los valores para las entidades agregadas o actualizadas, no garantiza que EF configurará el mecanismo real para generar valores. Vea [valor generado en Agregar o actualizar](#value-generated-on-add-or-update) sección para obtener más detalles.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para cambiar el modelo de generación de valor para una propiedad determinada.

### <a name="no-value-generation-fluent-api"></a>No se genera valor (API fluida)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valor generado en Agregar (API fluida)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()`solo permite EF saber que los valores se generan para entidades agregadas, no garantiza que EF configurará el mecanismo real para generar valores.  Vea [agregar valor generado en](#value-generated-on-add) sección para obtener más detalles.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Valor generado en Agregar o actualización (API fluida)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Esto permite EF sabe que se generan los valores para las entidades agregadas o actualizadas, no garantiza que EF configurará el mecanismo real para generar valores. Vea [valor generado en Agregar o actualizar](#value-generated-on-add-or-update) sección para obtener más detalles.
