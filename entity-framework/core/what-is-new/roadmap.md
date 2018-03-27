---
title: Mapa de ruta de Entity Framework Core
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
ms.technology: entity-framework-core
uid: core/what-is-new/roadmap
ms.openlocfilehash: 5aef679df2ecdfe7f59458c8994d0d17b4a889ff
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2018
---
# <a name="entity-framework-core-roadmap"></a>Mapa de ruta de Entity Framework Core

> [!IMPORTANT]
> Tenga en cuenta que los conjuntos de características y las programaciones de las versiones futuras siempre están sujetos a cambios y, aunque intentamos mantener esta página actualizada, es posible que no refleje nuestros planes más recientes en todo momento.

La primera versión preliminar de EF Core 2.1 se publicó en febrero de 2018. Ahora puede encontrar más información sobre esta versión en [Novedades de EF Core 2.1](xref:core/what-is-new/ef-core-2.1).

Tenemos previsto publicar más versiones preliminares de EF Core 2.1 mensualmente y una versión final en el segundo trimestre de 2018.

No hemos completado el [planeamiento de versiones](#release-planning-process) para la próxima versión después de la 2.1.

## <a name="schedule"></a>Programación

La programación de EF Core está sincronizada con la [Programación de .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) y la [Programación de ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Trabajo pendiente

Usamos el [hito de trabajo pendiente](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) en el detector de problemas para mantener una lista detallada de problemas y características. Los clientes pueden comentar y enviar su voto a favor.

Normalmente dejamos problemas abiertos si esperamos trabajar en ellos en algún momento o si alguien de la comunidad puede encargarse de ellos, pero eso no implica que haya intenciones de resolverlos en un período de tiempo específico hasta que se asignen a un hito específico como parte del [proceso de planeación de versiones](#release-planning-process).

Si no se tiene previsto implementar alguna característica, es probable que se cierre el problema. Es posible que, una vez cerrado un problema, volvamos a él en el futuro si recibimos nueva información al respecto.

Dicho esto, no tenemos información suficiente sobre el futuro para poder indicar que la característica X se resolverá en un momento o en una versión determinados. Como en todos los proyectos de software, las prioridades, las programaciones de versiones y los recursos disponibles pueden cambiar en cualquier momento.

## <a name="release-planning-process"></a>Proceso de planeamiento de versiones

A menudo nos preguntan cómo se eligen características específicas para incluirlas en una versión concreta. La lista de trabajos pendientes no se corresponde con los planes de versión. La presencia de una característica en EF6 tampoco implica automáticamente que esta se vaya a implementar en EF Core.

Detallar todo el proceso que se sigue para planear una versión es muy difícil, en parte debido a que parte de ello radica en debatir las características específicas, las oportunidades y las prioridades, y en parte porque el proceso evoluciona con cada versión. Sin embargo, es relativamente fácil resumir las preguntas comunes que intentamos responder a la hora de decidir en qué elementos trabajaremos a continuación:

1. **¿Cuántos desarrolladores creemos que usarán la característica y en qué medida hará que las aplicaciones o la experiencia sean mejores?** Recopilamos información de varias fuentes, y los comentarios y los votos son una de esas fuentes.

2. **¿Qué soluciones alternativas pueden adoptar los usuarios si todavía no se ha implementado una característica?** Por ejemplo, hay muchos desarrolladores que pueden asignar una tabla de unión para poder trabajar a pesar de la falta de compatibilidad múltiple de forma nativa. Obviamente, no todos los desarrolladores pueden hacer esto, pero muchos sí, de modo que es un factor importante.

3. **¿La implementación de esta característica hará evolucionar la arquitectura de EF Core tanto como para poder implementar otras características?** Las características que actúan como bloques de construcción para otras suelen tener preferencia. Por ejemplo, la división de tablas que se hizo para los tipos en propiedad nos ayudó a incluir compatibilidad con TPT.

4. **¿La característica es un punto de extensibilidad?** Los puntos de extensibilidad suelten tener preferencia porque facilitan que los desarrolladores puedan crear sus propios comportamientos y obtener las funcionalidades que faltan de este modo. Tenemos intención de hacer algo relacionado con esto como punto de partida para el trabajo con cargas diferidas.

5. **¿Cuál es la sinergia de la característica cuando se usa en combinación con otros productos?** Normalmente tienen preferencia las características que permiten que EF Core se use con otros productos o que mejoran significativamente la experiencia de uso de otros productos, como .NET Core, la última versión de Visual Studio, Microsoft Azure, etc.

6. **¿Cuáles son las capacidades de las personas disponibles para trabajar en una característica y cómo se aprovechan mejor estos recursos?** Todos los miembros del equipo de EF, e incluso los colaboradores de la comunidad, tienen diferentes niveles de experiencia en varias áreas, y tenemos que elaborar el plan de acuerdo con ello. Incluso aunque quisiéramos tener a todos trabajando en una característica específica, como las traducciones de GroupBy o las relaciones múltiples, no sería práctico.

Como mencionamos anteriormente, el proceso evoluciona con cada versión y en el futuro nos gustaría agregar más oportunidades para que los miembros de la comunidad de desarrolladores colaboren en los planes de versión, por ejemplo, facilitando la revisión de borradores propuestos de características o del propio plan de versión.
