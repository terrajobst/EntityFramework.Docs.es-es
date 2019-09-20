---
title: 'Tipos de entidad sin llave: EF Core'
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: b968ac9602b9aa1f1c1e3181b6b76a64394d70f0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150792"
---
# <a name="keyless-entity-types"></a>Tipos de entidad sin llave
> [!NOTE]
> Esta característica es nueva en EF Core 2.1. Antes de 3,0, se conocían como tipos de consulta

Además de los tipos de entidad normales, un modelo de EF Core puede contener _tipos de entidad_sin clave, que se pueden usar para realizar consultas de base de datos con datos que no contengan valores de clave.

## <a name="keyless-entity-types-characteristics"></a>Características de tipos de entidad sin llave

Los tipos de entidad sin llave admiten muchas de las mismas capacidades de asignación que los tipos de entidad normales, como las propiedades de navegación y asignación de herencia. En almacenes relacionales, pueden configurar los objetos de base de datos de destino y las columnas a través de métodos de la API fluidos o las anotaciones de datos.

Sin embargo, son diferentes de los tipos de entidad normales en que:

- No se puede definir una clave.
- Nunca se realiza un seguimiento de los cambios en _DbContext_ y, por lo tanto, nunca se insertan, actualizan o eliminan en la base de datos.
- Nunca se detectan por convención.
- Solo admite un subconjunto de capacidades de asignación de navegación, en concreto:
  - Nunca pueden actuar como el extremo principal de una relación.
  - Puede que no tengan navegaciones a entidades propiedad
  - Solo pueden contener propiedades de navegación de referencia que apunten a entidades normales.
  - Las entidades no pueden contener propiedades de navegación a tipos de entidad sin llave.
- Debe configurarse con `.HasNoKey()` la llamada al método.
- Se puede asignar a una _consulta de definición_. Una consulta de definición es una consulta declarada en el modelo que actúa como origen de datos para un tipo de entidad sin entrada.

## <a name="usage-scenarios"></a>Escenarios de uso

Algunos de los escenarios de uso principales de los tipos de entidad sin llave son:

- Actúa como el tipo de valor devuelto para [las consultas SQL sin procesar](xref:core/querying/raw-sql).
- Asignación a vistas de base de datos que no contienen una clave principal.
- Asignación de tablas que no tiene definida una clave principal.
- Asignación de las consultas definidas en el modelo.

## <a name="mapping-to-database-objects"></a>Asignación de objetos de base de datos

La asignación de un tipo de entidad sin llave a un objeto de `ToTable` base `ToView` de datos se consigue mediante el o la API fluida. Desde la perspectiva de EF Core, el objeto de base de datos especificado en este método es un _vista_, lo que significa que se trata como un origen de consulta de solo lectura y no puede ser el destino de la actualización, insertar o eliminar operaciones. Sin embargo, esto no significa que el objeto de base de datos sea realmente necesario para ser una vista de base de datos. Como alternativa, puede tratarse de una tabla de base de datos que se tratará como de solo lectura. Por el contrario, en el caso de los tipos de entidad normales, EF Core supone que un `ToTable` objeto de base de datos especificado en el método se puede tratar como una _tabla_, lo que significa que se puede usar como origen de la consulta, pero también como destino de las operaciones de actualización, eliminación e inserción. De hecho, puede especificar el nombre de una vista de base de datos en `ToTable` y todo debería funcionar bien siempre que la vista está configurada para ser actualizables en la base de datos.

> [!NOTE]
> `ToView`supone que el objeto ya existe en la base de datos y que no lo crearán las migraciones.

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra cómo utilizar los tipos de entidad sin entrada para consultar una vista de base de datos.

> [!TIP]
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) de este artículo en GitHub.

En primer lugar, definimos un modelo sencillo de Blog y Post:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

A continuación, definimos una vista de base de datos simple que nos permitirá consultar el número de entradas vinculadas a cada blog:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

A continuación, definimos una clase para contener el resultado de la vista de base de datos:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

A continuación, configuraremos el tipo de entidad sin llave `HasNoKey` en OnModelCreating con la API.
Usamos la API de configuración fluida para configurar la asignación para el tipo de entidad sin llave:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

A continuación, configuramos el `DbContext` para `DbSet<T>`incluir:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Por último, podemos consultar la vista de base de datos de la manera estándar:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Nota también hemos definido una propiedad de consulta de nivel de contexto (DbSet) para que actúe como raíz para las consultas en este tipo.
