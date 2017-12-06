---
title: "Cómo funcionan - Core EF las consultas"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a>Cómo funcionan las consultas

Entity Framework Core usa Language integrar Query (LINQ) para consultar los datos de la base de datos. LINQ permite usar C# (o el lenguaje .NET seleccionado) para escribir consultas fuertemente tipadas en función de sus contexto y la entidad que las clases derivadas.

## <a name="the-life-of-a-query"></a>La duración de una consulta

La siguiente es una introducción de alto nivel del proceso de que cada consulta se lleva a cabo.

1. Entity Framework Core para compilar una representación que está lista para ser procesados por el proveedor de base de datos procesa la consulta LINQ
   1. El resultado se almacena en caché para que no es necesario que este procesamiento se realiza cada vez que se ejecuta la consulta
2. El resultado se pasa al proveedor de base de datos
   1. El proveedor de base de datos identifica qué partes de la consulta se pueden evaluar en la base de datos
   2. Estas partes de la consulta se traducen en lenguaje de consulta específica de la base de datos (p. ej. SQL para una base de datos relacional)
   3. Una o más consultas se envían a la base de datos y el conjunto de resultados devuelto (resultados son valores de la base de datos, no las instancias de entidad)
3. Para cada elemento del conjunto de resultados
   1. Si se trata de una consulta de seguimiento, EF comprueba si los datos representan una entidad ya está en la herramienta de seguimiento de cambios para la instancia de contexto
      * Si es así, se devuelve la entidad existente
      * De lo contrario, se crea una nueva entidad, seguimiento de cambios está configurado y se devuelve la nueva entidad
   2. Si se trata de una consulta de seguimiento no, EF comprueba si los datos representan una entidad ya está en el conjunto de resultados de esta consulta
      * Si es así, se devuelve la entidad existente <sup>(1)</sup>
      * En caso contrario, se crea y devuelve una nueva entidad

<sup>(1) </sup> Ninguna consulta de seguimiento utiliza referencias débiles para realizar un seguimiento de las entidades que ya se han devuelto. Si un resultado anterior con la misma identidad queda fuera del ámbito y se ejecuta la recolección de elementos no utilizados, obtendrá una nueva instancia de entidad.

## <a name="when-queries-are-executed"></a>Cuando se ejecutan consultas

Cuando se llama a los operadores de LINQ, simplemente se está creando una representación en memoria de la consulta. La consulta solo se envía a la base de datos cuando se utilizan los resultados.

Las operaciones más comunes que son el resultado de la consulta que se envían a la base de datos son:
* Recorrer en iteración los resultados en un `for` bucles
* Uso de un operador como `ToList`, `ToArray`, `Single`,`Count`
* Enlace de datos de los resultados de una consulta a una interfaz de usuario

> [!WARNING]  
> **Valide siempre los datos proporcionados por el usuario:** EF mientras protegen contra ataques de inyección de SQL, no lleva a cabo ninguna validación de entrada general. Por lo tanto, si los valores que se pasan a la API, que se usan en las consultas LINQ, asignadas a las propiedades de la entidad, etc., proceden de un origen de confianza, a continuación, validación adecuada, según sus requisitos de aplicación, debe realizarse. Esto incluye cualquier proporcionados por el usuario utilizado para construir dinámicamente las consultas. Incluso cuando se usa LINQ, si se acepta la entrada del usuario generar expresiones que debe asegurarse de que solo las expresiones previstas puede crearse.
