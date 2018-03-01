---
title: Tipos de consulta - Core EF
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: d03c4b1d5635530e63b93e051cb69583718deb4e
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
---
# <a name="query-types"></a>Tipos de consulta
> [!NOTE]
> Esta función es nueva en EF Core 2.1

Tipos de consultas son los tipos de resultado de consulta de sólo lectura que se pueden agregar al modelo EF central. Tipos de consulta habilitar las consultas ad-hoc (como tipos anónimos), pero son más flexibles, ya que pueden tener configuración de asignación especificada.

Son conceptualmente similares a los tipos de entidad en la:

- Son tipos POCO C# que se agregan al modelo, en ```OnModelCreating``` con el ```ModelBuilder.Query``` método, o a través de una propiedad DbContext "set" (para los tipos de consulta de este tipo de propiedad es de tipo ```DbQuery<T>``` sino que ```DbSet<T>```).
- Soportan la mayor parte de las mismas capacidades de asignación como tipos de entidad regulares. Por ejemplo, la asignación de herencia, navegaciones (vea limitiations más abajo) y, en almacenes relacionales, la capacidad para configurar los objetos de esquema de base de datos de destino a través de ```ToTable```, ```HasColumn``` api fluida métodos (o las anotaciones de datos).

Tipos de consulta son diferentes de entidad tipos en los que:

- No requieren una clave de definirse.
- Nunca realiza el seguimiento al seguimiento de cambios.
- Nunca se descubren por convención.
- Sólo son compatibles con un subconjunto de funciones de asignación de exploración - específicamente, nunca pueden actuar como el fin principal de una relación.
- Se pueden asignar a un _definición de consulta_ -una definición de consulta es una consulta secundaria que actúa un origen de datos para un tipo de consulta.

Algunos de los escenarios principales de uso de tipos de consulta son:

- Asignación de vistas de base de datos.
- Asignación de tablas que no tienen definida una clave principal.
- Que actúa como el tipo devuelto para ad hoc ```FromSql()``` consultas.
- Asignación de las consultas definidas en el modelo.

> [!TIP]
> Asignación de un tipo de consulta a una vista de base de datos se logra utilizando la ```ToTable``` API fluida.

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra cómo utilizar el tipo de consulta para una vista de base de datos de consulta.

> [!TIP]
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) de este artículo en GitHub.

En primer lugar, definimos un modelo simple de Blog y Post:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

A continuación, definimos una vista de base de datos simple que nos permitirá consultar el número de mensajes asociados a cada blog:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

A continuación, configuramos el tipo de consulta en _OnModelCreating_ con el ```modelBuilder.Query<T>``` API.
Configuración estándar fluent API se utiliza para configurar la asignación para el tipo de consulta:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

Por último, podemos consultar la vista de base de datos de la forma estándar:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Tenga en cuenta también que hemos definido una propiedad de nivel de consulta (DbQuery) para que actúe como raíz para las consultas en este tipo de contexto.
