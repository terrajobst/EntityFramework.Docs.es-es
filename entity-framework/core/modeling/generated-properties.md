---
title: 'Valores generados: EF Core'
description: Cómo configurar la generación de valores para las propiedades al usar Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/generated-properties
ms.openlocfilehash: 9c616e157ff1bdb9700f436a7ae2788330fe5d45
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502037"
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

Dependiendo del proveedor de base de datos que se use, es posible que se generen valores de cliente en EF o en la base de datos. Si la base de datos genera el valor, EF puede asignar un valor temporal al agregar la entidad al contexto. Este valor temporal se reemplazará por el valor generado por la base de datos durante la `SaveChanges()`.

Si agrega una entidad al contexto que tiene un valor asignado a la propiedad, EF intentará insertar ese valor en lugar de generar uno nuevo. Se considera que una propiedad tiene un valor asignado si no se le asigna el valor predeterminado de CLR (`null` para `string`, `0` para `int`, `Guid.Empty` para `Guid`, etc.). Para obtener más información, vea [valores explícitos para las propiedades generadas](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> La forma en que se genera el valor para las entidades agregadas dependerá del proveedor de base de datos que se use. Los proveedores de bases de datos pueden configurar automáticamente la generación de valores para algunos tipos de propiedad, pero otros pueden requerir que se configure manualmente cómo se genera el valor.
>
> Por ejemplo, al usar SQL Server, se generarán automáticamente valores para las propiedades de `GUID` (mediante el algoritmo GUID secuencial SQL Server). Sin embargo, si especifica que se genere una propiedad `DateTime` en Add, debe configurar una manera de que se generen los valores. Una manera de hacerlo es configurar un valor predeterminado de `GETDATE()`, vea [valores predeterminados](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valor generado al agregar o actualizar

El valor generado al agregar o actualizar significa que se genera un nuevo valor cada vez que se guarda el registro (Insert o Update).

Como `value generated on add`, si especifica un valor para la propiedad en una instancia recién agregada de una entidad, se insertará ese valor en lugar de un valor que se va a generar. También es posible establecer un valor explícito al actualizar. Para obtener más información, vea [valores explícitos para las propiedades generadas](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> La forma en que se genera el valor para las entidades agregadas y actualizadas dependerá del proveedor de base de datos que se use. Los proveedores de bases de datos pueden configurar automáticamente la generación de valores para algunos tipos de propiedades, mientras que otros requerirán que se configure manualmente cómo se genera el valor.
>
> Por ejemplo, al usar SQL Server, `byte[]` propiedades que se establecen como se generan al agregar o actualizar y marcadas como tokens de simultaneidad, se configurarán con el tipo de datos `rowversion`, de modo que los valores se generarán en la base de datos. Sin embargo, si especifica que se genere una propiedad `DateTime` en Add o Update, debe configurar una manera de que se generen los valores. Una forma de hacerlo consiste en configurar un valor predeterminado de `GETDATE()` (vea [valores predeterminados](relational/default-values.md)) para generar valores para las nuevas filas. Después, puede usar un desencadenador de base de datos para generar valores durante las actualizaciones (como el siguiente desencadenador de ejemplo).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="value-generated-on-add"></a>Valor generado al agregar

Por Convención, las claves principales no compuestas de tipo Short, int, Long o GUID están configuradas para que tengan valores generados para las entidades insertadas, si la aplicación no proporciona un valor. Normalmente, el proveedor de base de datos se encarga de la configuración necesaria. por ejemplo, una clave principal numérica en SQL Server se configura automáticamente para que sea una columna de identidad.

Puede configurar cualquier propiedad para que se genere su valor para las entidades insertadas como se indica a continuación:

### <a name="data-annotationstabdata-annotations"></a>[Anotaciones de datos](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[API fluida](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs?name=ValueGeneratedOnAdd&highlight=5)]

***

> [!WARNING]
> Esto solo permite que EF sepa que los valores se generan para las entidades agregadas, no garantiza que EF configurará el mecanismo real para generar valores. Vea la sección [valor generado al agregar](#value-generated-on-add) para obtener más detalles.

### <a name="default-values"></a>Valores predeterminados

En las bases de datos relacionales, una columna se puede configurar con un valor predeterminado. Si se inserta una fila sin un valor para esa columna, se usará el valor predeterminado.

Puede configurar un valor predeterminado en una propiedad:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValue.cs?name=DefaultValue&highlight=5)]

También puede especificar un fragmento de SQL que se usa para calcular el valor predeterminado:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultValueSql.cs?name=DefaultValueSql&highlight=5)]

Si se especifica un valor predeterminado, se configurará implícitamente la propiedad como valor generado al agregar.

## <a name="value-generated-on-add-or-update"></a>Valor generado al agregar o actualizar

### <a name="data-annotationstabdata-annotations"></a>[Anotaciones de datos](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[API fluida](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs?name=ValueGeneratedOnAddOrUpdate&highlight=5)]

***

> [!WARNING]
> Esto solo permite que EF sepa que se generan valores para entidades agregadas o actualizadas, no garantiza que EF configure el mecanismo real para generar valores. Vea la sección [valor generado al agregar o actualizar](#value-generated-on-add-or-update) para obtener más detalles.

### <a name="computed-columns"></a>Columnas calculadas

En algunas bases de datos relacionales, una columna se puede configurar para que se calcule su valor en la base de datos, normalmente con una expresión que haga referencia a otras columnas:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ComputedColumn.cs?name=ComputedColumn&highlight=5)]

> [!NOTE]
> En algunos casos, el valor de la columna se calcula cada vez que se captura (a veces denominado columnas *virtuales* ) y en otros se calcula en cada actualización de la fila y se almacena (a veces denominada columnas *almacenadas* o *persistentes* ). Esto varía en los proveedores de bases de datos.

## <a name="no-value-generation"></a>Sin generación de valores

Normalmente, es necesario deshabilitar la generación de valores en una propiedad si una Convención la configura para la generación de valores. Por ejemplo, si tiene una clave principal de tipo int, se establecerá implícitamente configurada como valor generado al agregar; puede deshabilitarlo a través de lo siguiente:

### <a name="data-annotationstabdata-annotations"></a>[Anotaciones de datos](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[API fluida](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs?name=ValueGeneratedNever&highlight=5)]

***
