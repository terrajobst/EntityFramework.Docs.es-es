---
title: Tipos de consulta - Core EF
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: f16e3a130f3a4f92b2bf6014f2df0ca4eec56a25
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163179"
---
# <a name="query-types"></a>Tipos de consulta
> [!NOTE]
> Esta función es nueva en EF Core 2.1

Además de los tipos de entidad, puede contener un modelo básico de EF _consultar tipos_, que puede utilizarse para llevar a cabo consultas de base de datos con datos que no está asignados a tipos de entidad.

Los tipos de consulta tienen muchas similitudes con tipos de entidad:

- Se puede también agregar al modelo o bien en `OnModelCreating`, o a través de una propiedad "set" en un derivada _DbContext_.
- Admiten muchas de las mismas capacidades de asignación, como la herencia de propiedades de navegación (vea las limitaciones más adelante), de asignación y, en almacenes relacional, la capacidad de configurar los objetos de base de datos de destino y las columnas a través de métodos de la API fluidas o las anotaciones de datos.

Sin embargo son diferentes de entidad tipos en el que:

- No requieren una clave de definirse.
- Nunca un seguimiento de cambios para la _DbContext_ y, por tanto, se nunca insertan, actualizan o eliminan en la base de datos.
- Nunca se descubren por convención.
- Solo se admite un subconjunto de funciones de asignación de navegación - específicamente:
  - Nunca pueden actuar como el extremo principal de una relación.
  - Solo pueden contener propiedades de navegación de referencia que apunta a las entidades.
  - Entidades no pueden contener propiedades de navegación a los tipos de consulta.
- Se tratan en la _ModelBuilder_ mediante la `Query` método en lugar de la `Entity` método.
- Se asignan en el _DbContext_ a través de propiedades de tipo `DbQuery<T>` en lugar de `DbSet<T>`
- Se asignan a objetos de base de datos mediante la `ToView` método, en lugar de `ToTable`.
- Se pueden asignar a un _definir consulta_ : una definición de consulta es una consulta secundaria declarada en el modelo que actúa un origen de datos para un tipo de consulta.

Algunos de los escenarios principales de uso de tipos de consulta son:

- Que actúa como el tipo devuelto para ad hoc `FromSql()` consultas.
- Asignación de vistas de base de datos.
- Asignación de tablas que no tienen definida una clave principal.
- Asignación de las consultas definidas en el modelo.

> [!TIP]
> Asignar un tipo de consulta a un objeto de base de datos se logra mediante la `ToView` API fluida. Desde la perspectiva del núcleo de EF, el objeto de base de datos especificado en este método es un _vista_, lo que significa que se trata como un origen de consultas de solo lectura y no puede ser el destino de la actualización, insertar o eliminar operaciones. Sin embargo, esto no significa que el objeto de base de datos es realmente necesario para que sea una vista de base de datos, o bien puede ser una tabla de base de datos que se tratará como de solo lectura. Por el contrario, para los tipos de entidad, EF Core se supone que un objeto de base de datos especificado en el `ToTable` método puede tratarse como un _tabla_, lo que significa que puede usarse como un origen de la consulta pero también el objetivo de actualizar, eliminar e insertar operaciones. De hecho, puede especificar el nombre de una vista de base de datos en `ToTable` y todo debería funcionar bien siempre y cuando la vista está configurada para poder actualizar la base de datos.

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra cómo utilizar el tipo de consulta para una vista de base de datos de consulta.

> [!TIP]
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) de este artículo en GitHub.

En primer lugar, definimos un modelo simple de Blog y Post:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

A continuación, definimos una vista de base de datos simple que nos permitirá consultar el número de mensajes asociados a cada blog:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

A continuación, se define una clase que contenga el resultado de la vista de base de datos:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

A continuación, configuramos el tipo de consulta en _OnModelCreating_ con el `modelBuilder.Query<T>` API.
Configuración estándar fluent API se utiliza para configurar la asignación para el tipo de consulta:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Por último, podemos consultar la vista de base de datos de la forma estándar:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Tenga en cuenta también que hemos definido una propiedad de nivel de consulta (DbQuery) para que actúe como raíz para las consultas en este tipo de contexto.
