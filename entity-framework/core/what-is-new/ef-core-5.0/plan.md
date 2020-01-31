---
title: Plan para Entity Framework Core 5.0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 0472841fdcd105ec8ea38db062c6768510b8735d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125359"
---
# <a name="plan-for-entity-framework-core-50"></a>Plan para Entity Framework Core 5.0

Como se describe en el [proceso de planeamiento](../release-planning.md), se ha recopilado la información de las partes interesadas en un plan provisional para la versión EF Core 5.0.

> [!IMPORTANT] 
> Este plan sigue siendo un trabajo en curso. Nada de esto es un compromiso. Este plan es un punto de partida que evolucionará a medida que se obtenga más información. Es posible que algunos aspectos no planeados en la actualidad se incorporen a la versión 5.0. Es posible que algunos aspectos planeados en la actualidad se eliminen de la versión 5.0.

### <a name="version-number-and-release-date"></a>Número de versión y fecha de lanzamiento.

En la actualidad, el lanzamiento de EF Core 5.0 está programado al [mismo tiempo que .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/). Se ha elegido la versión "5.0" para la alineación con .NET 5.0.

### <a name="supported-platforms"></a>Plataformas compatibles

Está previsto que EF Core 5.0 se ejecute en cualquier plataforma .NET 5.0 en función de la [ convergencia de estas plataformas a .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/). Lo que esto significa en términos de .NET Standard y el TFM real que se usa todavía está por determinar.

EF Core 5.0 no se ejecutará en .NET Framework.

### <a name="breaking-changes"></a>Cambios importantes

EF Core 5.0 contendrá algunos cambios importantes, pero serán mucho menos graves que en el caso de EF Core 3.0. El objetivo es permitir que la gran mayoría de las aplicaciones se actualicen sin interrupciones.

Se espera que haya algunos cambios importantes para los proveedores de bases de datos, especialmente relacionados con la compatibilidad con TPT. Pero se espera que el trabajo para actualizar un proveedor para 5.0 sea menor que el necesario para 3.0.

## <a name="themes"></a>Temas

Hemos extraído algunas áreas o temas importantes que formarán la base de las grandes inversiones en EF Core 5.0.

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a>Propiedades de navegación de varios a varios (también denominado "omitir navegaciones")

Jefes de desarrollo: @smitpatel y @AndriySvyryd

Seguimiento realizado por [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)

Talla de camiseta: L

Estado: En curso

Varios a varios es la característica más solicitada (407 votos aproximadamente) en el trabajo pendiente de GitHub. La compatibilidad con las relaciones varios a varios se puede dividir en tres áreas principales:

* Omitir propiedades de navegación. Permiten usar el modelo para las consultas, etc., sin hacer referencia a la entidad de la tabla de combinación subyacente.
* Tipos de entidad de contenedor de propiedades. Permiten usar un tipo CLR estándar (por ejemplo, `Dictionary`) para las instancias de entidad, de modo que no se necesite un tipo CLR explícito para cada tipo de entidad.
* Facilitar la configuración de relaciones varios a varios.

Creemos que el principal obstáculo para los que quieren que se admitan las relaciones varios a varios es la imposibilidad de usar relaciones "naturales", sin hacer referencia a la tabla de combinación, en la lógica de negocios, como las consultas. Es posible que el tipo de entidad de tabla de combinación siga existiendo, pero no debería interferir con la lógica de negocios. Por este motivo hemos optado por abordar la omisión de propiedades de navegación en la versión 5.0.

En este momento, los demás elementos de las relaciones varios a varios se han convertido en un objetivo de extensión para EF Core 5.0. Esto significa que actualmente no se incluyen en el plan para la versión 5.0, pero si todo va bien esperamos incorporarlos.

## <a name="table-per-type-tpt-inheritance-mapping"></a>Asignación de herencia de tabla por tipo (TPT)

Jefe de desarrollo: @AndriySvyryd

Seguimiento realizado por [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)

Talla de camiseta: XL

Estado: Sin iniciar

TPT se va a incluir porque se trata de una característica muy solicitada (aproximadamente 254 votos; en tercera posición) y porque requiere algunos cambios de bajo nivel que consideramos adecuados para la naturaleza fundamental del plan general de .NET 5. Esperamos que esto genere cambios importantes para los proveedores de bases de datos, aunque deberían ser mucho menos graves que los necesarios para la versión 3.0.

## <a name="filtered-include"></a>Inclusión filtrada

Jefe de desarrollo: @maumar

Seguimiento realizado por [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)

Talla de camiseta: M

Estado: Sin iniciar

La inclusión filtrada es una característica muy solicitada (aproximadamente 317 votos; en segunda posición) que no requiere demasiado trabajo y que creemos que desbloqueará o facilitará escenarios que actualmente requieren filtros de nivel de modelo o consultas más complejas.

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a>Racionalización de ToTable, ToQuery, ToView, FromSql, etc.

Jefes de desarrollo: @maumar y @smitpatel

Seguimiento realizado por [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)

Talla de camiseta: L

Estado: Sin iniciar

Se han realizado avances en versiones anteriores hacia la compatibilidad con SQL sin procesar, tipos sin clave y áreas relacionadas. Pero hay brechas e incoherencias en el funcionamiento en conjunto de todos los elementos. El objetivo para la versión 5.0 es corregirlos y crear una buena experiencia para definir, migrar y usar otros tipos de entidades y sus consultas y artefactos de base de datos asociados. Esto también puede implicar actualizaciones de la API de consulta compilada.

Tenga en cuenta que este elemento puede dar lugar a algunos cambios importantes en el nivel de la aplicación, ya que algunas de las funcionalidades actuales son demasiado permisivas, lo que puede conducir rápidamente a que los usuarios cometan errores. Lo más probable es que parte de esta funcionalidad se bloquee y se proporcionen instrucciones sobre lo que se debe hacer en su lugar.

## <a name="general-query-enhancements"></a>Mejoras generales de consultas

Jefes de desarrollo: @smitpatel y @maumar

Seguimiento por [problemas etiquetados con `area-query` en el hito 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)

Talla de camiseta: XL

Estado: En curso

El código de traducción de consultas se ha reescrito de forma exhaustiva para EF Core 3.0. Por este motivo, el código de consulta tiene un estado mucho más robusto. Para la versión 5.0, no se planean cambios importantes en las consultas más allá de los necesarios para admitir TPT y la omisión de propiedades de navegación. Pero se necesita un trabajo importante para corregir las deudas técnicas generadas tras la revisión de la versión 3.0. También tenemos previsto corregir muchos errores e implementar pequeñas mejoras para mejorar aún más la experiencia general de las consultas.

## <a name="migrations-and-deployment-experience"></a>Migraciones y experiencia de implementación

Jefes de desarrollo: @bricelam

Seguimiento realizado por [#19587](https://github.com/dotnet/efcore/issues/19587)

Talla de camiseta: L

Estado: En curso

En la actualidad, muchos desarrolladores migran sus bases de datos en el momento de inicio de la aplicación. Esto es fácil, pero no se recomienda porque:

* Es posible que varios subprocesos, procesos o servidores intenten migrar la base de datos simultáneamente
* Es posible que las aplicaciones intenten acceder a un estado incoherente mientras se produce esta operación
* Normalmente, no se deben conceder los permisos de base de datos para modificar el esquema para la ejecución de la aplicación
* Es difícil revertir a un estado limpio si se produce algún error

Queremos ofrecer una mejor experiencia que permita migrar la base de datos de forma sencilla en el momento de la implementación. Esto debería:

* Funcionar en Linux, Mac y Windows
* Ser una experiencia positiva en la línea de comandos
* Admitir escenarios con contenedores
* Funcionar con flujos y herramientas de implementación del mundo real que se usan habitualmente
* Integrarse al menos en Visual Studio

Lo más probable es que el resultado sea un gran número de pequeñas mejoras en EF Core (por ejemplo, mejores migraciones en SQLite), junto con instrucciones y colaboraciones a largo plazo con otros equipos para mejorar las experiencias de un extremo a otro que van más allá de EF exclusivamente.

## <a name="ef-core-platforms-experience"></a>Experiencia de las plataformas de EF Core 

Jefes de desarrollo: @roji y @bricelam

Seguimiento realizado por [#19588](https://github.com/dotnet/efcore/issues/19588)

Talla de camiseta: L

Estado: Sin iniciar

Disponemos de instrucciones de calidad para usar EF Core en aplicaciones web tradicionales similares a MVC. Las instrucciones para otras plataformas y modelos de aplicación no existen o no están actualizadas. Para EF Core 5.0 el objetivo es investigar, mejorar y documentar la experiencia de uso de EF Core con:

* Blazor
* Xamarin, incluido el uso del artículo de AOT o vinculador
* WinForms, WPF, WinUI y posiblemente otras interfaces de usuario frameworks

Lo más probable es que el resultado sea un gran número de pequeñas mejoras en EF Core, junto con instrucciones y colaboraciones a largo plazo con otros equipos para mejorar las experiencias de un extremo a otro que van más allá de EF exclusivamente.

Las áreas concretas que tenemos previsto examinar son las siguientes:

* Implementación, incluida la experiencia en el uso de herramientas de EF como para migraciones
* Modelos de aplicación, como Xamarin y Blazor, y probablemente otros
* Experiencias de SQLite, incluida la experiencia espacial y las recompilaciones de tabla
* Experiencias de AOT y vinculación
* Integración de diagnósticos, incluidos los contadores de rendimiento

## <a name="performance"></a>Rendimiento

Jefe de desarrollo: @roji

Seguimiento por [problemas etiquetados con `area-perf` en el hito 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)

Talla de camiseta: L

Estado: En curso

Para EF Core, el plan es mejorar nuestro conjunto de pruebas comparativas de rendimiento y realizar mejoras de rendimiento dirigidas al tiempo de ejecución. Además, tenemos previsto completar la nueva API de procesamiento por lotes de ADO.NET, que se ha creado como prototipo durante el ciclo de versiones de 3.0. En el nivel de ADO.NET también se planean mejoras de rendimiento adicionales para el proveedor Npgsql.

Como parte de este trabajo, también está previsto agregar contadores de rendimiento de ADO.NET y EF Core, y otros diagnósticos según corresponda.

## <a name="architecturalcontributor-documentation"></a>Documentación de arquitectura y colaboradores

Jefe de documentación: @ajcvickers

Seguimiento realizado por [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)

Talla de camiseta: L

Estado: Sin iniciar

La idea es facilitar la comprensión de lo que sucede dentro de EF Core. Esto puede ser útil para cualquiera que utilice EF Core, pero la motivación principal es facilitarlo para los usuarios externos:

* Contribuir al código de EF Core
* Crear proveedores de bases de datos
* Compilar otras extensiones

## <a name="microsoftdatasqlite-documentation"></a>Documentación de Microsoft.Data.Sqlite

Jefe de documentación: @bricelam

Seguimiento realizado por [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)

Talla de camiseta: M

Estado: Completado. La nueva documentación está [activa en el sitio de documentación de Microsoft](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).

El equipo de EF también posee el proveedor de ADO.NET Microsoft.Data.Sqlite. Tenemos previsto documentar completamente este proveedor como parte de la versión 5.0.

## <a name="general-documentation"></a>Documentación general

Jefe de documentación: @ajcvickers

Seguimiento mediante [problemas en el repositorio de documentación del hito 5.0](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)

Talla de camiseta: L

Estado: En curso

Ya se ha iniciado el proceso de actualización de la documentación de las versiones 3.0 y 3.1. También se está trabajando en:
  * Una revisión de la documentación de introducción para que sea más fácil de seguir
  * La reorganización de la documentación para facilitar la búsqueda y la adición de referencias cruzadas
  * La incorporación de más detalles y aclaraciones a la documentación existente
  * La actualización de los ejemplos y la incorporación de otros nuevos

## <a name="fixing-bugs"></a>Corrección de errores

Seguimiento por [problemas etiquetados con `type-bug` en el hito 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)

Desarrolladores: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers

Talla de camiseta: L

Estado: En curso

En el momento de escribir este documento, se han evaluado 135 errores para corregirlos en la versión 5.0 (ya se han corregido 62), pero hay una superposición significativa con la sección anterior _Mejoras generales de consultas_.

La velocidad de entrada (problemas que acaban como trabajo en un hito) fue de aproximadamente 23 problemas al mes en el transcurso de la versión 3.0. No todos se tendrán que corregir en la versión 5.0. Como estimación aproximada, tenemos previsto corregir unos 150 problemas adicionales para la versión 5.0.

## <a name="small-enhancements"></a>Mejoras menores

Seguimiento por [problemas etiquetados con `type-enhancement` en el hito 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)

Desarrolladores: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers

Talla de camiseta: L

Estado: En curso

Además de las características más importantes descritas antes, también hay muchas mejoras más pequeñas programadas para la versión 5.0 a fin de corregir los "elementos excluidos". Tenga en cuenta que muchas de estas mejoras también se incluyen en los temas más generales descritos antes.

## <a name="below-the-line"></a>Nivel inferior

Seguimiento por [problemas etiquetados con `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)

Se trata de correcciones de errores y mejoras **no** programadas actualmente para la versión 5.0, pero que se considerarán objetivos de extensión en función del progreso realizado en el trabajo anterior.

Además, durante la planeación siempre se tienen en cuenta los [problemas más votados](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc). Excluir cualquiera de estos problemas de una versión siempre es complicado, pero necesitamos un plan realista para los recursos que tenemos.

## <a name="feedback"></a>Comentarios

Sus comentarios sobre la planeación son importantes. La mejor manera de indicar la importancia de un problema es votar (pulgar) por ese problema en GitHub. Estos datos se introducirán después en el [proceso de planeación](../release-planning.md) de la próxima versión.
