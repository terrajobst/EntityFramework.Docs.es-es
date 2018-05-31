---
title: 'Funcionamiento de las consultas: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052875"
---
# <a name="how-queries-work"></a>Funcionamiento de las consultas

Entity Framework Core usa Language Integrated Query (LINQ) para consultar los datos de la base de datos. LINQ permite usar C# (o el lenguaje .NET que prefiera) para escribir consultas fuertemente tipadas basadas en el contexto derivado y las clases de entidad.

## <a name="the-life-of-a-query"></a>La duración de una consulta

A continuación se muestra información general de alto nivel del proceso por el que pasa toda consulta.

1. Entity Framework Core procesa la consulta LINQ para crear una representación que está lista para que el proveedor de base de datos la procese.
   1. El resultado se almacena en caché para que no sea necesario hacer este procesamiento cada vez que se ejecuta la consulta.
2. El resultado se pasa al proveedor de base de datos.
   1. El proveedor de base de datos identifica qué partes de la consulta se pueden evaluar en la base de datos.
   2. Estas partes de la consulta se traducen al lenguaje de la consulta específico para la base de datos (por ejemplo, SQL para una base de datos relacional).
   3. Una o varias consultas se envían a la base de datos y se devuelve el conjunto de resultados (los resultados son valores de la base de datos y no instancias de entidad).
3. Para cada elemento del conjunto de resultados
   1. Si se trata de una consulta con seguimiento, EF comprueba si los datos representan una entidad que ya existe en la herramienta de seguimiento de cambios para la instancia de contexto.
      * Si es así, se devuelve la entidad existente.
      * En caso contrario, se crea una entidad nueva, se configura el seguimiento de cambios y se devuelve la entidad nueva.
   2. Si se trata de una consulta sin seguimiento, EF comprueba si los datos representan una entidad que ya existe en el conjunto de resultados de esta consulta.
      * Si es así, se devuelve la entidad existente <sup>(1)</sup>.
      * En caso contrario, se crea y devuelve una entidad nueva.

<sup>(1)</sup> Las consultas sin seguimiento usan referencias parciales para llevar un seguimiento de las entidades que ya se devolvieron. Si un resultado anterior con la misma identidad queda fuera del ámbito y se ejecuta la recolección de elementos no utilizados, puede obtener una instancia de entidad nueva.

## <a name="when-queries-are-executed"></a>Cuándo se ejecutan las consultas

Cuando llama a los operadores LINQ, simplemente crea una representación de la consulta en memoria. La consulta solo se envía a la base de datos cuando se usan los resultados.

Las operaciones más comunes que generan que la consulta se envíe a la base de datos son:
* La iteración de los resultados en un bucle `for`
* El uso de un operador como `ToList`, `ToArray`, `Single`, `Count`
* El enlace de datos de los resultados de una consulta a una interfaz de usuario

> [!WARNING]  
> **Valide siempre la intervención del usuario:** si bien EF brinda protección contra ataques por inyección de código SQL, no realiza ninguna validación general de la intervención. Por tanto, si los valores que se pasan a las API, que se usan en las consultas LINQ, que están asignadas a las propiedades de entidad, etc., provienen de un origen que no es de confianza, se debe realizar la validación correspondiente según los requisitos de la aplicación. Esto incluye cualquier intervención del usuario que se use para construir consultas de manera dinámica. Incluso al usar LINQ, si acepta la intervención del usuario para crear expresiones, necesita garantizar que solo se pueden construir las expresiones previstas.
