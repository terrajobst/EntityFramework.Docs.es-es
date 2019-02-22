---
title: Mapa de ruta de Entity Framework Core
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: 9064b323c11282418f2bedf70f874d45c18bb78a
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325345"
---
# <a name="entity-framework-core-roadmap"></a>Mapa de ruta de Entity Framework Core

> [!IMPORTANT]
> Tenga en cuenta que los conjuntos de características y las programaciones de las versiones futuras siempre están sujetos a cambios y, aunque intentamos mantener esta página actualizada, es posible que no refleje nuestros planes más recientes en todo momento.

### <a name="ef-core-30"></a>EF Core 3.0

Con EF Core 2.2 ya listo, nuestro objetivo principal es ahora EF Core 3.0, que se alineará con las versiones .NET Core 3.0 y ASP.NET 3.0.

Aún no hemos completado ninguna característica nueva, así que los  [paquetes de EF Core 3.0 Preview 1 publicados en la Galería NuGet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.0-preview.18572.1) en diciembre de 2018 solo contienen [corrección de errores, mejoras menores y cambios que se realizaron en preparación para el trabajo de la versión 3.0](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed+label%3Aclosed-fixed).

De hecho, todavía necesitamos perfeccionar nuestra [planeación de la versión](#release-planning-process) 3.0, para asegurarnos de que tenemos el conjunto correcto de características que se pueden completar en el tiempo asignado.
Compartiremos más información a medida que tengamos más claridad, pero estos son algunos temas y características de alto nivel en los que nos proponemos trabajar:

- **Mejoras del LINQ ([N.º 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795))**: LINQ permite escribir consultas a la base de datos sin abandonar el idioma de elección, con lo que se aprovecha la información de tipo enriquecido para obtener la comprobación de IntelliSense y de tipos en tiempo de compilación.
  Pero LINQ también le permite escribir un número ilimitado de consultas complicadas, y eso siempre ha sido un gran reto para los proveedores de LINQ.
  En las primeras versiones de EF Core, resolvimos esto en parte al calcular qué partes de una consulta se podían traducir a SQL, y luego al permitir que el resto de la consulta se ejecutara en memoria en el cliente.
  Esta ejecución de cliente puede ser deseable en algunas situaciones, pero en muchos otros casos puede dar como resultado consultas ineficientes que pueden no ser identificadas hasta que una aplicación se implemente en producción.
  En EF Core 3.0, tenemos previsto hacer cambios profundos en cómo funciona nuestra implementación de LINQ, y cómo la probamos.
  Los objetivos son hacerlo más eficaz (por ejemplo, evitar interrumpir las consultas en las versiones de revisión), poder traducir más expresiones correctamente a SQL, generar consultas eficientes en más casos y evitar que las consultas ineficientes pasen desapercibidas.

- **Compatibilidad con Cosmos DB ([n.º 8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443))**: Estamos trabajando en un proveedor de Cosmos DB para EF Core, para que los desarrolladores familiarizados con el modelo de programación de EF puedan utilizar fácilmente Azure Cosmos DB como base de datos de aplicación.
  El objetivo es hacer que algunas de las ventajas de Cosmos DB, como la distribución global, la disponibilidad "siempre activa", la escalabilidad elástica y la baja latencia, sean aún más accesibles para los desarrolladores de .NET.
  El proveedor habilitará la mayoría de las características de EF Core, como el seguimiento automático de cambios, LINQ y conversiones de valores, en comparación con SQL API de Cosmos DB. Comenzamos este esfuerzo antes de EF Core 2.2 y [hemos puesto a disposición algunas versiones preliminares del proveedor](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
  El nuevo plan es continuar desarrollando el proveedor junto con EF Core 3.0.   

- **Compatibilidad con C# 8.0 ([n.º 12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047))**: Queremos que nuestros clientes aprovechen algunas de las [nuevas características que se incluyen en C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/), como las secuencias de sincronización (incluida la espera de cada una) y los tipos de referencia que aceptan valores NULL mientras usan EF Core.

- **Vistas de base de datos de ingeniería inversa en tipos de consulta ([n.º 1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679))**: En EF Core 2.1, hemos agregado compatibilidad para tipos de consulta, que pueden representar datos que pueden leerse desde la base de datos, pero que no pueden actualizarse.
  Los tipos de consulta son perfectos para asignar vistas de bases de datos, así que en EF Core 3.0 nos gustaría automatizar la creación de tipos de consulta para vistas de bases de datos.

- **Entidades de la bolsa de propiedades ([n.º 13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) y [n.º 9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914))**: Esta característica trata de permitir entidades que almacenan datos en propiedades indexadas en lugar de propiedades regulares, y también de utilizar instancias de la misma clase .NET (potencialmente algo tan simple como un `Dictionary<string, object>`) para representar diferentes tipos de entidades en el mismo modelo de EF Core.
  Esta característica es un punto de partida para admitir las relaciones varios a varios sin una entidad de unión, que es una de las mejoras más solicitadas por EF Core.

- **EF 6.3 en .NET Core ([EF6 n.º 271](https://github.com/aspnet/EntityFramework6/issues/271))**: Entendemos que muchas aplicaciones existentes utilizan versiones anteriores de EF, y que portarlas a EF Core solo para aprovechar las ventajas de .NET Core puede requerir a veces un esfuerzo considerable.
  Por ese motivo, adaptaremos la próxima versión de EF 6 para se ejecute en .NET Core 3.0.
  Hacemos esto para facilitar la portabilidad de las aplicaciones existentes con cambios mínimos.
  Habrá algunas limitaciones (por ejemplo, se necesitarán nuevos proveedores, no se habilitará el soporte espacial con SQL Server), y no hay nuevas características planeadas para EF 6.

Mientras, puede usar [esta consulta en nuestro rastreador de problemas](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc) para consultar los elementos de trabajo asignados temporalmente a 3.0.

## <a name="schedule"></a>Programación

La programación de EF Core está sincronizada con la [Programación de .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) y la [Programación de ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Trabajo pendiente

El [hito de trabajo pendiente](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) de nuestro rastreador de problemas contiene cuestiones en las que esperamos trabajar algún día o bien consideramos que alguien de la comunidad puede abordar.
Los clientes pueden enviar comentarios y votar en relación con estos problemas.
Se alienta a los colaboradores que busquen trabajar en cualquiera de estos temas a que comiencen primero una discusión sobre cómo abordarlos.

Nunca hay una garantía de que trabajaremos en una característica determinada en una versión específica de EF Core.
Al igual que en todos los proyectos de software, pueden cambiar las prioridades, las programaciones de lanzamiento y los recursos disponibles en cualquier momento.
Pero si pretendemos resolver un problema en un período de tiempo específico, lo asignaremos a un hito de lanzamiento en lugar de al hito de trabajo pendiente.
Como parte de nuestro [proceso de planeamiento de versiones](#release-planning-process), movemos periódicamente los problemas entre los hitos de lanzamiento y de trabajo pendiente.

Probablemente cerraremos un problema si no tenemos previsto solucionarlo.
Pero podemos reconsiderar un problema que cerramos previamente si obtenemos nueva información al respecto.

## <a name="release-planning-process"></a>Proceso de planeamiento de versiones

A menudo nos preguntan cómo se eligen características específicas para incluirlas en una versión concreta.
La lista de trabajos pendientes no se corresponde con los planes de versión.
La presencia de una característica en EF6 tampoco implica automáticamente que esta se vaya a implementar en EF Core.

Es difícil detallar todo el proceso que se sigue para planear una versión.
Gran parte consiste en describir las características específicas, las oportunidades y las prioridades, y el propio proceso también evoluciona con cada versión.
Sin embargo, podemos resumir las preguntas comunes que intentamos responder a la hora de decidir en qué elementos trabajaremos a continuación:

1. **¿Cuántos desarrolladores creemos que usarán la característica y en qué medida hará que las aplicaciones o la experiencia sean mejores?** Para responder a ello, recopilamos información de varias fuentes, y los comentarios y los votos son una de esas fuentes.

2. **¿Qué soluciones alternativas pueden adoptar los usuarios si todavía no se ha implementado una característica?** Por ejemplo, hay muchos desarrolladores que pueden asignar una tabla de unión para poder trabajar a pesar de la falta de compatibilidad múltiple de forma nativa. Obviamente, no todos los desarrolladores quieren hacerlo, pero muchos pueden, y eso se considera un factor decisivo.

3. **¿La implementación de esta característica hará evolucionar la arquitectura de EF Core tanto como para poder implementar otras características?** Normalmente tienen preferencia las características que actúan como bloques de creación de otras características. Por ejemplo, las entidades contenedoras de propiedades pueden ayudarnos a avanzar hacia la compatibilidad de varios a varios, y los constructores de entidades habilitaron nuestra compatibilidad de carga diferida. 

4. **¿La característica es un punto de extensibilidad?** Los puntos de extensibilidad suelten tener preferencia sobre las características normales porque permiten que los desarrolladores puedan crear sus propios comportamientos y compensar las funcionalidades que faltan. 

5. **¿Cuál es la sinergia de la característica cuando se usa en combinación con otros productos?** Tienen preferencia las características que permiten o mejoran significativamente la experiencia de uso de EF Core con otros productos, como .NET Core, la última versión de Visual Studio, Microsoft Azure, etc.

6. **¿Cuáles son las habilidades de las personas disponibles para trabajar en una característica y cómo se aprovechan mejor estos recursos?** Todos los miembros del equipo de EF, e incluso los colaboradores de la comunidad, tienen diferentes niveles de experiencia en varias áreas, y tenemos que elaborar el plan de acuerdo con ello. Incluso aunque quisiéramos tener a todos trabajando en una característica específica, como las traducciones de GroupBy o las relaciones múltiples, no sería práctico.

Como se mencionó antes, el proceso evoluciona con cada versión.
En el futuro, intentaremos incluir más oportunidades para que los miembros de la comunidad proporcionen información sobre nuestros planes de lanzamiento.
Por ejemplo, nos gustaría que fuera más fácil revisar los diseños de borrador de las características y el propio plan de lanzamiento.
