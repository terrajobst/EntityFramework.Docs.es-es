---
title: Controlar los conflictos de simultaneidad - Core EF
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a>Administrar los conflictos de simultaneidad

> [!NOTE]
> Esta página documenta cómo funciona la simultaneidad en EF Core y cómo tratar los conflictos de simultaneidad en la aplicación. Vea [Tokens de simultaneidad](xref:core/modeling/concurrency) para obtener más información sobre cómo configurar los tokens de simultaneidad en el modelo.

> [!TIP]
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) de este artículo en GitHub.

_Simultaneidad de base de datos_ hace referencia a situaciones en las que varios procesos o a los usuarios tener acceso o cambiar los mismos datos en una base de datos al mismo tiempo. _Control de simultaneidad_ hace referencia a mecanismos específicos utilizados para garantizar la coherencia de datos en presencia de cambios simultáneos.

Núcleo EF implementa _control de simultaneidad optimista_, lo que significa que permitirá a varios procesos o los usuarios realizar cambios de forma independiente sin la sobrecarga de sincronización o de bloqueo. En la situación ideal, estos cambios no interfieran entre sí y, por tanto, se podrán realizar correctamente. En el peor de los caso, dos o más procesos intentará realizar cambios en conflicto y solo uno de ellos debe ejecutarse correctamente.

## <a name="how-concurrency-control-works-in-ef-core"></a>Cómo funciona el control de simultaneidad en EF Core

Propiedades configuradas como tokens de simultaneidad se usan para implementar el control de simultaneidad optimista: cada vez que se realiza una operación update o delete durante `SaveChanges`, el valor del token de simultaneidad en la base de datos se compara con la versión original valor leído por núcleo de EF.

- Si los valores coinciden, puede completar la operación.
- Si los valores no coinciden, se da por supuesto en EF Core que otro usuario ha realizado una operación en conflicto y anula la transacción actual.

La situación cuando otro usuario ha realizado una operación que entra en conflicto con la operación actual se conoce como _conflicto de simultaneidad_.

Proveedores de base de datos son responsables de implementar la comparación de valores de token de simultaneidad.

En bases de datos relacionales EF Core incluye una comprobación para el valor del token de simultaneidad en el `WHERE` cláusula de cualquier `UPDATE` o `DELETE` las instrucciones. Después de ejecutar las instrucciones, Core EF lee el número de filas afectadas.

Si ninguna fila afectada, se detecta un conflicto de simultaneidad y produce EF Core `DbUpdateConcurrencyException`.

Por ejemplo, queremos podemos configurar `LastName` en `Person` un token de simultaneidad. Cualquier operación de actualización en persona incluirá la comprobación de simultaneidad en el `WHERE` cláusula:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Resolver conflictos de simultaneidad

Siguiendo con el ejemplo anterior, si un usuario intenta guardar los cambios a un `Person`, pero otro usuario ya ha cambiado el `LastName` el se producirá una excepción.

En este punto, la aplicación simplemente podría informar al usuario que la actualización no tuvo éxito debido a cambios en conflicto y mover. Pero puede ser deseable para pedir al usuario para asegurarse de que este registro representa siguen la misma persona real y vuelva a intentar la operación.

Este proceso es un ejemplo de _resolver un conflicto de simultaneidad_.

Resolver un conflicto de simultaneidad implica combinar los cambios pendientes actual `DbContext` con los valores de la base de datos. Combinarán los valores que varía en función de la aplicación y puede ser dirigido por proporcionados por el usuario.

**Hay tres conjuntos de valores disponibles para ayudar a resolver un conflicto de simultaneidad:**

* **Valores actuales** son los valores que la aplicación estaba intentando escribir en la base de datos.

* **Valores originales** son los valores que se recuperaron originalmente desde la base de datos antes de que se realizaron las modificaciones.

* **Valores de la base de datos** son los valores almacenados actualmente en la base de datos.

Es el enfoque general para controlar los conflictos de simultaneidad:

1. Detectar `DbUpdateConcurrencyException` durante `SaveChanges`.
2. Use `DbUpdateConcurrencyException.Entries` para preparar un nuevo conjunto de cambios para las entidades afectadas.
3. Actualice los valores originales de lo token de simultaneidad para reflejar los valores actuales de la base de datos.
4. Vuelva a intentar el proceso hasta que no se produzca ningún conflicto.

En el ejemplo siguiente, `Person.FirstName` y `Person.LastName` están configurados como tokens de simultaneidad. Hay un `// TODO:` comentario en la ubicación donde se incluye lógica específica de aplicación para elegir el valor que se guarde.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
