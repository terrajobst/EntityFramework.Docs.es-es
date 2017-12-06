---
title: Tokens de simultaneidad - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a>Tokens de simultaneidad

Si una propiedad está configurada como un token de simultaneidad EF comprobará que ningún otro usuario modificó ese valor en la base de datos al guardar los cambios a ese registro. EF usa un modelo de simultaneidad optimista, lo que significa que se suponen que no ha cambiado el valor e intente guardar los datos, pero producir si encuentra que el valor ha cambiado.

Por ejemplo que podríamos desear configurar `LastName` en `Person` un token de simultaneidad. Esto significa que si un usuario intenta guardar los cambios a un `Person`, pero otro usuario ha cambiado el `LastName` , a continuación, se producirá una excepción. Esto puede ser deseable para que la aplicación puede solicitar al usuario para asegurarse de que este registro representa siguen la misma persona real antes de guardar sus cambios.

> [!NOTE]
> Esta página documenta cómo configurar los tokens de simultaneidad. Vea [control de simultaneidad](../saving/concurrency.md) para obtener ejemplos de cómo usar simultaneidad optimista en la aplicación.

## <a name="how-concurrency-tokens-work-in-ef"></a>Cómo funcionan los tokens de simultaneidad en EF

Almacenes de datos pueden exigir tokens de simultaneidad mediante la comprobación de que todos los registros que se va a actualizar o eliminar aún tiene el mismo valor para el token de simultaneidad que se asignó cuando el contexto de carga originalmente los datos de la base de datos.

Por ejemplo, bases de datos relacionales para ello, incluido el token de simultaneidad en el `WHERE` cláusula de cualquier `UPDATE` o `DELETE` comandos y comprobando el número de filas afectadas. Si el token de simultaneidad sigue coincidiendo con una fila se actualizará. Si ha cambiado el valor de la base de datos, no se actualiza ninguna fila.

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a>Convenciones

Por convención, propiedades nunca se configuran como tokens de simultaneidad.

## <a name="data-annotations"></a>Anotaciones de datos

Puede usar las anotaciones de datos para configurar una propiedad como un token de simultaneidad.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una propiedad como un token de simultaneidad.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Versión de fila/marca de tiempo

Una marca de tiempo es una propiedad que se genera un nuevo valor de la base de datos cada vez que se inserta o actualiza una fila. La propiedad también se trata como un token de simultaneidad. Esto garantiza que obtendrá una excepción si nadie ha modificado una fila que está intentando actualizar ya que la consulta para los datos.

Cómo esto se consigue es responsabilidad del proveedor de base de datos que se va a usar. Para SQL Server, marca de tiempo se utiliza normalmente en un *byte []* propiedad, que será el programa de instalación como un *ROWVERSION* columna en la base de datos.

### <a name="conventions"></a>Convenciones

Por convención, propiedades nunca se configuran como marcas de tiempo.

### <a name="data-annotations"></a>Anotaciones de datos

Puede usar anotaciones de datos para configurar una propiedad como una marca de tiempo.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API fluida

Puede usar la API fluida para configurar una propiedad como una marca de tiempo.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
