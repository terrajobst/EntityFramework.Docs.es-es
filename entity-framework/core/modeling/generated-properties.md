---
title: 'Valores generados: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 6b38fd2e540ec29674f1116e7c204052d06ca1bc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197431"
---
# <a name="generated-values"></a>Valores generados

## <a name="value-generation-patterns"></a>Patrones de generación de valores

Hay tres patrones de generación de valores que se pueden usar para las propiedades:
* Sin generación de valores
* Valor generado al agregar
* Valor generado al agregar o actualizar

### <a name="no-value-generation"></a>Sin generación de valores

Ninguna generación de valores significa que siempre se proporcionará un valor válido que se va a guardar en la base de datos. Este valor válido se debe asignar a las nuevas entidades antes de que se agreguen al contexto.

### <a name="value-generated-on-add"></a>Valor generado al agregar

El valor generado al agregar significa que se genera un valor para las nuevas entidades.

Dependiendo del proveedor de base de datos que se use, es posible que se generen valores de cliente en EF o en la base de datos. Si la base de datos genera el valor, EF puede asignar un valor temporal al agregar la entidad al contexto. Este valor temporal se reemplazará por el valor generado por la base `SaveChanges()`de datos durante.

Si agrega una entidad al contexto que tiene un valor asignado a la propiedad, EF intentará insertar ese valor en lugar de generar uno nuevo. Se considera que una propiedad tiene un valor asignado si no se le asigna el valor predeterminado`null` `0` de CLR ( `string`para, `int`para `Guid.Empty` `Guid`, etc.). Para obtener más información, vea [valores explícitos para las propiedades generadas](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> La forma en que se genera el valor para las entidades agregadas dependerá del proveedor de base de datos que se use. Los proveedores de bases de datos pueden configurar automáticamente la generación de valores para algunos tipos de propiedad, pero otros pueden requerir que se configure manualmente cómo se genera el valor.
>
> Por ejemplo, al usar SQL Server, los valores se generarán automáticamente `GUID` para las propiedades (mediante el algoritmo GUID secuencial SQL Server). Sin embargo, si especifica que se `DateTime` genere una propiedad en Add, debe configurar una manera de que se generen los valores. Una manera de hacerlo es configurar un valor predeterminado de `GETDATE()`, vea [valores predeterminados](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valor generado al agregar o actualizar

El valor generado al agregar o actualizar significa que se genera un nuevo valor cada vez que se guarda el registro (Insert o Update).

Al `value generated on add`igual que, si especifica un valor para la propiedad en una instancia recién agregada de una entidad, se insertará ese valor en lugar de un valor que se va a generar. También es posible establecer un valor explícito al actualizar. Para obtener más información, vea [valores explícitos para las propiedades generadas](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> La forma en que se genera el valor para las entidades agregadas y actualizadas dependerá del proveedor de base de datos que se use. Los proveedores de bases de datos pueden configurar automáticamente la generación de valores para algunos tipos de propiedades, mientras que otros requerirán que se configure manualmente cómo se genera el valor.
> 
> Por ejemplo, al usar SQL Server, `byte[]` las propiedades que se establecen como se generan al agregar o actualizar y marcadas como tokens de simultaneidad, se configurarán con el `rowversion` tipo de datos, de modo que los valores se generarán en la base de datos. Sin embargo, si especifica que se `DateTime` genere una propiedad al agregar o actualizar, debe configurar una manera para que se generen los valores. Una forma de hacerlo consiste en configurar un valor predeterminado de `GETDATE()` (vea [valores predeterminados](relational/default-values.md)) para generar valores para las nuevas filas. Después, puede usar un desencadenador de base de datos para generar valores durante las actualizaciones (como el siguiente desencadenador de ejemplo).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Convenciones

Por Convención, las claves principales no compuestas de tipo Short, int, Long o GUID se configurarán para que se generen valores en Add. El resto de las propiedades se configurarán sin generación de valores.

## <a name="data-annotations"></a>Anotaciones de datos

### <a name="no-value-generation-data-annotations"></a>Sin generación de valores (anotaciones de datos)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valor generado en Add (anotaciones de datos)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Esto solo permite que EF sepa que los valores se generan para las entidades agregadas, no garantiza que EF configurará el mecanismo real para generar valores. Vea la sección [valor generado al agregar](#value-generated-on-add) para obtener más detalles.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valor generado al agregar o actualizar (anotaciones de datos)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Esto solo permite que EF sepa que se generan valores para entidades agregadas o actualizadas, no garantiza que EF configure el mecanismo real para generar valores. Vea la sección [valor generado al agregar o actualizar](#value-generated-on-add-or-update) para obtener más detalles.

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para cambiar el patrón de generación de valores para una propiedad determinada.

### <a name="no-value-generation-fluent-api"></a>Sin generación de valores (API fluida)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valor generado en Add (API fluida)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()`Simplemente permita que EF sepa que se generan valores para las entidades agregadas, no garantiza que EF configurará el mecanismo real para generar valores.  Vea la sección [valor generado al agregar](#value-generated-on-add) para obtener más detalles.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Valor generado al agregar o actualizar (API fluida)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Esto solo permite que EF sepa que se generan valores para entidades agregadas o actualizadas, no garantiza que EF configure el mecanismo real para generar valores. Vea la sección [valor generado al agregar o actualizar](#value-generated-on-add-or-update) para obtener más detalles.
