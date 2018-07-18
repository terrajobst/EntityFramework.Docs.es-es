---
title: Casos prácticos para Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
caps.latest.revision: 3
ms.openlocfilehash: 27c911799f957dd81a1866a3fd49e7f6cfa0059b
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122616"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Casos prácticos de Microsoft para Entity Framework
Los casos prácticos de esta página se resaltan unos pocos proyectos de producción del mundo real que se han empleado el Entity Framework.
> [!NOTE]
> Las versiones detalladas de estos casos prácticos ya no están disponibles en el sitio Web de Microsoft. Por lo tanto, los vínculos se han quitado.

## <a name="epicor"></a>Epicor
Epicor es una empresa de software global de gran tamaño (con más de 400 desarrolladores) que desarrolla soluciones de planificación de recursos empresariales (ERP) para empresas de más de 150 países.
Su producto estrella, Epicor 9, se basa en una arquitectura orientada (SOA) con .NET Framework.
Se enfrenta a numerosas solicitudes de cliente para proporcionar compatibilidad con Language Integrated Query (LINQ) y también desean reducir la carga en sus servidores SQL de back-end, el equipo decidió actualizar a Visual Studio 2010 y .NET Framework 4.0.
Con Entity Framework 4.0, fueron capaces de alcanzar estos objetivos y simplificar enormemente el desarrollo y mantenimiento.
En concreto, soporte técnico de T4 enriquecida de Entity Framework le permite tomar el control completo de su código generado y genere automáticamente en las características de ahorro de rendimiento, como las consultas compiladas previamente y almacenamiento en caché.

> "Hemos llevado a cabo algunas pruebas de rendimiento recientemente con el código existente, y fuimos capaces de reducir las solicitudes a SQL Server al 90 por ciento.
Eso es debido a ADO.NET Entity Framework 4". – Erik Johnson, vicepresidente de investigación de productos  

## <a name="veracity-solutions"></a>Soluciones de veracidad
Adquirir un sistema de software de planeación de eventos que se iba a ser difícil de mantener y ampliar a largo plazo, soluciones de veracidad usa Visual Studio 2010 para volver a escribirlo como una aplicación de Internet enriquecida eficaz y fácil de usar que se basa en Silverlight 4.
Con servicios .NET RIA, fueron capaces de crear rápidamente una capa de servicio sobre Entity Framework que evitar la duplicación de código y permitidos para la validación comunes y lógica de autenticación a través de niveles.  

> "Nos estábamos vendidas en Entity Framework cuando apareció por primera vez, y Entity Framework 4 ha demostrado para ser aún mejor.
Se ha mejorado las herramientas y es más fácil manipular los archivos .edmx que definen el modelo conceptual, el modelo de almacenamiento y la asignación entre los modelos... Con Entity Framework, puedo hacer que esa capa de acceso a datos, trabajar en un día y genérelo a medida que avanza.
Entity Framework es la capa de acceso a datos de hecho; No sé por qué alguien no usarla." : Para desarrolladores Joe McBride, director

## <a name="nec-display-solutions-of-america"></a>Soluciones de visualización de NEC de América
NEC desea entrar en el mercado de publicidad digital basado en su lugar con una solución para beneficiarse de los anunciantes y los propietarios de la red y aumentar sus ingresos.
Para ello, inicia un par de aplicaciones web que automatizan los procesos manuales necesarios en una campaña de publicidad tradicional.
Los sitios se hayan creado mediante ASP.NET, Silverlight 3, AJAX y WCF, junto con Entity Framework en la capa de acceso a datos para comunicarse con SQL Server 2008.

> "Con SQL Server, pensamos que podíamos obtener el rendimiento que necesitábamos para ofrecer los anunciantes y las redes con información en tiempo real y la confiabilidad para ayudar a garantizar que la información de nuestras aplicaciones de misión crítica siempre estarán disponible"-Mike Corcoran, Director de TI

## <a name="darwin-dimensions"></a>Dimensiones de Darwin
Con una amplia gama de tecnologías de Microsoft, el equipo de Darwin propusimos para crear Evolver - un portal de avatar en línea que los consumidores podrían usar para crear los avatares impresionantes, realistas para su uso en páginas de redes sociales, animaciones y juegos.
Las ventajas de productividad de Entity Framework y dirige a los componentes como Windows Workflow Foundation (WF) y Windows Server AppFabric (una caché de aplicaciones altamente escalable con memoria), el equipo pudo entregar un producto increíble de un 35% menos tiempo de desarrollo.
A pesar de tener los miembros del equipo se dividen en varios países, el equipo siguiendo un proceso de desarrollo ágil con versiones semanales.

 > "Nos no intente crear tecnología para fines de la tecnología. Como un tipo de inicio, es fundamental que hemos aprovechado la tecnología que ahorra tiempo y dinero.
 .NET fue la elección para el desarrollo rápido y rentable." – Zachary Olsen, arquitecto  

## <a name="silverware"></a>Vajilla de plata
Con más de 15 años de experiencia en desarrollo de soluciones de punto de venta (POS) para grupos de restaurante pequeñas y medianas, el equipo de desarrollo de vajilla de plata establecidos para mejorar sus productos con más características de nivel empresarial con el fin de atraer más grande cadenas del restaurante.
Con la versión más reciente de las herramientas de desarrollo de Microsoft, fueron capaces de crear la nueva solución cuatro veces más rápida que antes.
Las nuevas características clave, como LINQ y Entity Framework facilitaba mover de Crystal Reports a SQL Server 2008 y SQL Server Reporting Services (SSRS) para su almacenamiento de datos y necesidades de informes.

> "Administración de datos eficaz es clave para el éxito de vajilla de plata – y por eso decidimos adoptar SQL Reporting". -Nicholas Romanidis, Director de TI o ingeniería de Software
