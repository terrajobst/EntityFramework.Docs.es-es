---
title: Tipos de consultas - EF Core
author: anpete
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: c023d442b0fa2728bd20694a55ebb3a7b5c0efd1
ms.sourcegitcommit: 87e72899d17602f7526d6ccd22f3c8ee844145df
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69628406"
---
# <a name="query-types"></a>Tipos de consulta
> [!NOTE]
> Esta característica es nueva en EF Core 2.1

Además de tipos de entidad, puede contener un modelo de EF Core _tipos de consulta_, que se puede usar para llevar a cabo las consultas de base de datos en datos que no se asignan a tipos de entidad.

## <a name="compare-query-types-to-entity-types"></a>Comparar tipos de consulta a tipos de entidad

Tipos de consulta son como tipos de entidad que:

- O bien se pueden agregar al modelo en `OnModelCreating` o a través de una propiedad "set" en una derivada _DbContext_.
- Admite muchas de las mismas funcionalidades de asignación, como las propiedades de navegación y la asignación de herencia. En almacenes relacionales, pueden configurar los objetos de base de datos de destino y las columnas a través de métodos de la API fluidos o las anotaciones de datos.

Sin embargo, son diferentes de entidad tipos en el que:

- No requieren una clave que se defina.
- No se realiza un seguimiento de cambios nunca la _DbContext_ y, por tanto, se nunca insertan, actualizan o eliminan en la base de datos.
- Nunca se detectan por convención.
- Solo admite un subconjunto de funciones de asignación de navegación - específicamente:
  - Nunca pueden actuar como el extremo principal de una relación.
  - Solo pueden contener propiedades de navegación de referencia que apunta a las entidades.
  - Las entidades no pueden contener propiedades de navegación a los tipos de consulta.
- Se tratan en el _ModelBuilder_ utilizando el `Query` método en lugar de `Entity` método.
- Se asignan en el _DbContext_ a través de las propiedades de tipo `DbQuery<T>` en lugar de `DbSet<T>`
- Se asignan a objetos de base de datos mediante el `ToView` método, en lugar de `ToTable`.
- Se pueden asignar a un _definir consulta_ : una definición de consulta es una consulta secundaria declarada en el modelo que actúa el origen de datos para un tipo de consulta.

## <a name="usage-scenarios"></a>Escenarios de uso

Algunos de los escenarios de uso principales para tipos de consulta son:

- Que actúa como el tipo de valor devuelto para ad hoc `FromSql()` consultas.
- Asignación de vistas de base de datos.
- Asignación de tablas que no tiene definida una clave principal.
- Asignación de las consultas definidas en el modelo.

## <a name="mapping-to-database-objects"></a>Asignación de objetos de base de datos

Asignación de un tipo de consulta a un objeto de base de datos se logra mediante el `ToView` API fluida. Desde la perspectiva de EF Core, el objeto de base de datos especificado en este método es un _vista_, lo que significa que se trata como un origen de consulta de solo lectura y no puede ser el destino de la actualización, insertar o eliminar operaciones. Sin embargo, esto no significa que el objeto de base de datos es realmente necesario que sea una vista de base de datos, o bien puede ser una tabla de base de datos que se tratará como de solo lectura. A la inversa, para los tipos de entidad, EF Core asume que un objeto de base de datos se especifica en el `ToTable` método se puede tratar como un _tabla_, lo que significa que puede usarse como un origen de la consulta pero también el objetivo de actualizar, eliminar e insertar operaciones. De hecho, puede especificar el nombre de una vista de base de datos en `ToTable` y todo debería funcionar bien siempre que la vista está configurada para ser actualizables en la base de datos.

## <a name="example"></a>Ejemplo

El ejemplo siguiente muestra cómo usar el tipo de consulta para consultar una vista de base de datos.

> [!TIP]
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) de este artículo en GitHub.

En primer lugar, definimos un modelo sencillo de Blog y Post:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Entities)]

A continuación, definimos una vista de base de datos simple que nos permitirá consultar el número de entradas vinculadas a cada blog:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#View)]

A continuación, definimos una clase para contener el resultado de la vista de base de datos:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#QueryType)]

A continuación, configuramos el tipo de consulta en _OnModelCreating_ utilizando el `modelBuilder.Query<T>` API.
API de configuración fluida estándar se usa para configurar la asignación para el tipo de consulta:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Configuration)]

A continuación, configuramos el `DbContext` para `DbQuery<T>`incluir:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]

Por último, podemos consultar la vista de base de datos de la manera estándar:

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Query)]

> [!TIP]
> Tenga en cuenta también que hemos definido una propiedad de nivel de consulta (DbQuery) para que actúe como raíz para las consultas en este tipo de contexto.
