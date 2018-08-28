---
title: 'Novedades: EF6'
author: divega
ms.date: 2016-10-23
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 9eb6a916d36ed41fcaea74564395695c048ab0f5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996340"
---
# <a name="whats-new-in-ef6"></a>Novedades de EF6

Se recomienda encarecidamente usar la versión más reciente de Entity Framework para asegurarse de obtener las últimas características y la mayor estabilidad.
Pero es posible que deba usar una versión anterior o que quiera experimentar con las nuevas mejoras de la última versión preliminar.
Para instalar versiones concretas de EF, vea [Get Entity Framework](~/ef6/fundamentals/install.md) (Obtener Entity Framework).

En esta página se documentan las características incluidas en cada nueva versión.

## <a name="recent-releases"></a>Versiones recientes

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Actualización de EF Tools para Visual Studio 2017 15.7

En mayo de 2018 se publicó una versión actualizada de EF6 Tools como parte de Visual Studio 2017 15.7.
Incluye mejoras para algunas de las áreas problemáticas más habituales:

- Correcciones de varios errores de accesibilidad de la interfaz de usuario
- Solución alternativa para la regresión del rendimiento de SQL Server al generar modelos a partir de bases de datos existentes [#4](https://github.com/aspnet/entityframework6/issues/4)
- Compatibilidad para la actualización de modelos más grandes en SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Otra mejora de esta versión nueva de EF Tools es que ahora instala el runtime de EF 6.2 al crear un modelo en un proyecto nuevo. Con versiones anteriores de Visual Studio, es posible usar el runtime de EF 6.2 (así como cualquier versión anterior de EF) mediante la instalación de la versión correspondiente del paquete NuGet.

### <a name="ef-62-runtime"></a>Runtime de EF 6.2

El runtime de EF 6.2 para NuGet se publicó en octubre de 2017.
Gracias en gran medida a los esfuerzos de la comunidad de colaboradores de código abierto, EF 6.2 incluye bastantes [correcciones de errores](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) y [mejoras de producto](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Esta es una breve lista de los cambios más importantes que afectan al runtime de EF 6.2:

- Reducción del tiempo de inicio al cargar los primeros modelos de código finalizados desde una caché persistente [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- API fluida para definir índices [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- DbFunctions.Like() para habilitar la escritura de consultas LINQ que se traducen en LIKE en SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Migrate.exe ahora admite la opción -script [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6 ahora puede trabajar con valores de clave generados por una secuencia en SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- Lista de actualización de errores transitorios de la estrategia de ejecución de SQL Azure [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Error: al volver a intentar consultas o comandos SQL, se produce un error "SqlParameter ya está incluido en otro elemento SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Error: la evaluación de DbQuery.ToString() a menudo agota el tiempo de espera en el depurador [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>Próximas versiones

Para obtener información sobre las próximas versiones de EF6, vea el [plan de desarrollo](roadmap.md).

## <a name="past-releases"></a>Versiones anteriores

La página [Versiones anteriores](past-releases.md) contiene un archivo de todas las versiones anteriores de EF y de las características principales incluidas en cada versión.
