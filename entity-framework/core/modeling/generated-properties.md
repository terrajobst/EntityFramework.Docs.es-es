---
title: Valores generados - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 9ecfa924a0614f327f0bd202cb7dda95bea810af
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250704"
---
# <a name="generated-values"></a>Valores generados

## <a name="value-generation-patterns"></a>Patrones de generación de valor

Hay tres patrones de generación de valor que se pueden usar para las propiedades:
* Sin generación de valor
* Valor generado en Agregar
* Valor generado en Agregar o actualizar

### <a name="no-value-generation"></a>Sin generación de valor

Generación de valor no significa que siempre proporcionará un valor válido para guardarse en la base de datos. Este valor válido debe asignarse a las nuevas entidades antes de que se agregan al contexto.

### <a name="value-generated-on-add"></a>Valor generado en Agregar

Valor generado en Agregar significa que se genera un valor para las nuevas entidades.

Dependiendo del proveedor de base de datos que se va a usar, los valores pueden ser generado del lado cliente mediante EF o en la base de datos. Si el valor es generado por la base de datos, EF puede asignar un valor temporal cuando se agrega la entidad al contexto. Este valor temporal, a continuación, se reemplazará por el valor de la base de datos generado durante `SaveChanges()`.

Si agrega una entidad en el contexto que tiene un valor asignado a la propiedad, EF intentará insertar ese valor en lugar de generar uno nuevo. Una propiedad se considera que tiene un valor asignado si no se asigna el valor predeterminado CLR (`null` para `string`, `0` para `int`, `Guid.Empty` para `Guid`, etcetera.). Para obtener más información, consulte [valores explícitos para propiedades generadas](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Cómo se genera el valor para las entidades agregadas dependerá del proveedor de base de datos que se va a usar. Proveedores de base de datos pueden configurar automáticamente la generación de valor para algunos tipos de propiedad, pero otros pueden requerir configurar manualmente cómo se genera el valor.
>
> Por ejemplo, cuando se usa SQL Server, los valores se generará automáticamente para `GUID` propiedades (utilizando el algoritmo GUID secuencial de SQL Server). Sin embargo, si especifica que un `DateTime` generar propiedad en la agregación, a continuación, debe configurar una forma de los valores que se va a generar. Una manera de hacer esto, consiste en configurar un valor predeterminado de `GETDATE()`, consulte [valores predeterminados](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valor generado en Agregar o actualizar

Valor generado en adición o actualización significa que se genera un nuevo valor cada vez que se guarda el registro (insert o update).

Al igual que `value generated on add`, si especifica un valor para la propiedad en una instancia recién agregada de una entidad, que se insertará el valor en lugar de un valor que se está generando. También es posible establecer un valor explícito al actualizar. Para obtener más información, consulte [valores explícitos para propiedades generadas](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Cómo se genera el valor de entidades agregadas y actualizadas dependerá del proveedor de base de datos que se va a usar. Proveedores de base de datos pueden configurar automáticamente la generación de valor para algunos tipos de propiedades, mientras que otros usuarios, deberá configurar manualmente cómo se genera el valor.
> 
> Por ejemplo, cuando se usa SQL Server, `byte[]` propiedades que se establecen como generadas en Agregar o actualizar y marcados como tokens de simultaneidad, se configurará con la `rowversion` tipo de datos - para que se generarán los valores en la base de datos. Sin embargo, si especifica que un `DateTime` generar propiedad en Agregar o actualizar, a continuación, debe configurar una forma de los valores que se va a generar. Una manera de hacer esto, consiste en configurar un valor predeterminado de `GETDATE()` (consulte [valores predeterminados](relational/default-values.md)) para generar valores para nuevas filas. A continuación, podría usar un desencadenador de base de datos para generar valores durante las actualizaciones (por ejemplo, el siguiente ejemplo de desencadenador).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Convenciones

Por convención, las claves principales de no compuestas de tipo short, int, long o Guid será el programa de instalación tiene valores generados en Agregar. Todas las demás propiedades será el programa de instalación con la generación de ningún valor.

## <a name="data-annotations"></a>Anotaciones de datos

### <a name="no-value-generation-data-annotations"></a>Sin generación de valor (anotaciones de datos)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valor generado en Agregar (anotaciones de datos)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Esto simplemente permite que EF sepa que se generan los valores para las entidades agregadas, no garantiza que EF le permitirán configurar el mecanismo real para generar valores. Consulte [agregar valor generado en](#value-generated-on-add) sección para obtener más detalles.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valor generado en Agregar o actualizar (anotaciones de datos)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Esto simplemente permite que EF sepa que se generan los valores para las entidades se ha agregado o actualizadas, no garantiza que EF le permitirán configurar el mecanismo real para generar valores. Consulte [valor generado en Agregar o actualizar](#value-generated-on-add-or-update) sección para obtener más detalles.

## <a name="fluent-api"></a>API fluida

Puede usar la API Fluent para cambiar el patrón de generación de valor para una propiedad determinada.

### <a name="no-value-generation-fluent-api"></a>Sin generación de valor (API de Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valor generado en Agregar (API de Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` solo permite que EF sepa que se generan los valores para las entidades agregadas, no garantiza que EF le permitirán configurar el mecanismo real para generar valores.  Consulte [agregar valor generado en](#value-generated-on-add) sección para obtener más detalles.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Valor generado en Agregar o actualizar (API de Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Esto simplemente permite que EF sepa que se generan los valores para las entidades se ha agregado o actualizadas, no garantiza que EF le permitirán configurar el mecanismo real para generar valores. Consulte [valor generado en Agregar o actualizar](#value-generated-on-add-or-update) sección para obtener más detalles.
