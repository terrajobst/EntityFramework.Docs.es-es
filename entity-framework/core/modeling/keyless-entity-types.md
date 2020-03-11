---
title: 'Tipos de entidad sin llave: EF Core'
description: Configuración de tipos de entidad sin entrada mediante Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 520c9ed93240c05deee36fa527a3757490fd7082
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414648"
---
# <a name="keyless-entity-types"></a>Tipos de entidad sin llave

> [!NOTE]
> Esta característica se agregó en EF Core 2,1 bajo el nombre de los tipos de consulta. En EF Core 3,0 se cambió el nombre del concepto a tipos de entidad sin entrada.

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
- Debe configurarse con `.HasNoKey()` llamada al método.
- Se puede asignar a una _consulta de definición_. Una consulta de definición es una consulta declarada en el modelo que actúa como origen de datos para un tipo de entidad sin entrada.

## <a name="usage-scenarios"></a>Escenarios de uso

Algunos de los escenarios de uso principales de los tipos de entidad sin llave son:

- Actúa como el tipo de valor devuelto para [las consultas SQL sin procesar](xref:core/querying/raw-sql).
- Asignación a vistas de base de datos que no contienen una clave principal.
- Asignación de tablas que no tiene definida una clave principal.
- Asignación de las consultas definidas en el modelo.

## <a name="mapping-to-database-objects"></a>Asignación de objetos de base de datos

La asignación de un tipo de entidad sin llave a un objeto de base de datos se consigue mediante el `ToTable` o `ToView` API fluida. Desde la perspectiva de EF Core, el objeto de base de datos especificado en este método es una _vista_, lo que significa que se trata como un origen de consulta de solo lectura y no puede ser el destino de las operaciones de actualización, inserción o eliminación. Sin embargo, esto no significa que el objeto de base de datos sea realmente necesario para ser una vista de base de datos. Como alternativa, puede tratarse de una tabla de base de datos que se tratará como de solo lectura. Por el contrario, en el caso de los tipos de entidad normales, EF Core supone que un objeto de base de datos especificado en el método `ToTable` se puede tratar como una _tabla_, lo que significa que se puede usar como origen de la consulta, pero también como destino de las operaciones de actualización, eliminación e inserción. De hecho, puede especificar el nombre de una vista de base de datos en `ToTable` y todo debería funcionar bien siempre que la vista esté configurada para ser actualizable en la base de datos.

> [!NOTE]
> `ToView` supone que el objeto ya existe en la base de datos y que no lo crearán las migraciones.

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra cómo utilizar los tipos de entidad sin entrada para consultar una vista de base de datos.

> [!TIP]
> Puede ver un [ejemplo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) de este artículo en GitHub.

En primer lugar, definimos un modelo sencillo de Blog y Post:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

A continuación, definimos una vista de base de datos simple que nos permitirá consultar el número de entradas vinculadas a cada blog:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

A continuación, definimos una clase para contener el resultado de la vista de base de datos:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

A continuación, configuraremos el tipo de entidad sin llave en _OnModelCreating_ con la API de `HasNoKey`.
Usamos la API de configuración fluida para configurar la asignación para el tipo de entidad sin llave:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

A continuación, configuramos el `DbContext` para incluir el `DbSet<T>`:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

Por último, podemos consultar la vista de base de datos de la manera estándar:

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> Nota también hemos definido una propiedad de consulta de nivel de contexto (DbSet) para que actúe como raíz para las consultas en este tipo.
