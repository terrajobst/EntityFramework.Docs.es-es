---
title: 'Versiones anteriores de Entity Framework: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
ms.openlocfilehash: 4c711bb48938e5c0432881c61766b0bff66498f2
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490149"
---
# <a name="past-releases-of-entity-framework"></a>Versiones anteriores de Entity Framework

La primera versión de Entity Framework se publicó en 2008, como parte de .NET Framework 3.5 SP1 y Visual Studio 2008 SP1.

A partir de la versión EF4.1, se ha distribuido como el [paquete NuGet de EntityFramework](https://www.nuget.org/packages/EntityFramework/) -actualmente uno de los paquetes más populares en NuGet.org.

Entre las versiones 4.1 y 5.0, el paquete EntityFramework NuGet había ampliado las bibliotecas EF que se incluyen como parte de .NET Framework.   

A partir de la versión 6, EF se convirtió en un proyecto de código abierto y también había sacado completamente formulario banda .NET Framework.
Esto significa que al agregar el paquete de NuGet de Entity Framework versión 6 a una aplicación, se obtienen una copia completa de la biblioteca EF que no depende de los bits EF que se incluyen como parte de .NET Framework.
Esto un poco ayudó a acelerar el ritmo de desarrollo y la entrega de nuevas características.

En junio de 2016, hemos lanzado EF Core 1.0. EF Core se basa en una nueva base de código y se ha diseñado como una versión más ligera y extensible de EF.
Actualmente EF Core es el enfoque principal de desarrollo para el equipo de Entity Framework de Microsoft.
Esto significa que no hay características principales nuevas planeadas para EF6. Sin embargo EF6 aún se mantiene como un proyecto de código abierto y un producto de Microsoft compatible.

Esta es la lista de las versiones anteriores, en orden cronológico inverso, con información sobre las nuevas características que se introdujeron en cada versión.

## <a name="ef-613"></a>EF 6.1.3
6.1.3 EF se lanzó en tiempo de ejecución a NuGet en octubre de 2015.
Esta versión contiene correcciones sola a los defectos de alta prioridad y regresiones notificadas en el 6.1.2 versión.
Las correcciones se incluyen:

- Consulta: Regresión en EF 6.1.2: OUTER APPLY introdujo y consultas más complejas para las relaciones de 1:1 y la cláusula "let"
- Problema TPT con ocultar la propiedad de clase base en la clase heredada
- DbMigration.Sql se produce un error cuando se encuentra la palabra "go" en el texto
- Crear marca de compatibilidad para unir todos y Intersect compatibilidad para vistas planas
- Consulta con varios Includes no funciona en 6.1.2 (trabajando en 6.1.1)
- Excepción "tiene un error en la sintaxis SQL" después de actualizar desde EF 6.1.1 a 6.1.2

## <a name="ef-612"></a>EF 6.1.2
6.1.2 EF se lanzó en tiempo de ejecución a NuGet en diciembre de 2014.
Esta versión es principalmente sobre correcciones de errores. También se acepta un par de cambios importantes de los miembros de la Comunidad:
- **Parámetros de la memoria caché de consulta se pueden configurar desde el archivo app/web.configuration**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **Métodos de SqlFile y SqlResource en DbMigration** le permiten ejecutar una instancia de SQL script almacenado como un archivo o recurso incrustado.

## <a name="ef-611"></a>EF 6.1.1
EF 6.1.1 en tiempo de ejecución se lanzó en NuGet en junio de 2014.
Esta versión contiene correcciones para problemas que ha encontrado un número de personas. Entre otros:
- Diseñador: Error si abre edmx EF5 con precisión decimal en el Diseñador de EF6
- Lógica de detección de instancia predeterminada de LocalDB no funciona con SQL Server 2014

## <a name="ef-610"></a>EF 6.1.0
EF 6.1.0 se lanzó en tiempo de ejecución a NuGet en marzo de 2014.
Esta actualización secundaria incluye un número significativo de las nuevas características:

- **Las herramientas de consolidación** proporciona una manera coherente para crear un nuevo modelo EF. Esta característica [amplía el Asistente de Entity Data Model de ADO.NET para admitir la creación de modelos de Code First](~/ef6/modeling/code-first/workflows/existing-database.md), incluidas técnicas de ingeniería inversa desde una base de datos existente. Estas características estaban disponibles en la calidad de la versión Beta en EF Power Tools.
- **[Control de errores de confirmación de transacción](~/ef6/fundamentals/connection-resiliency/commit-failures.md)**  proporciona el CommitFailureHandler que hace uso de la capacidad recién introducida para interceptar las operaciones de transacción. El CommitFailureHandler permite la recuperación automática de errores de conexión al confirmar una transacción.
- **[IndexAttribute](~/ef6/modeling/code-first/data-annotations.md)**  permite que los índices que se especifique mediante la colocación de un `[Index]` atributo en una propiedad (o propiedades) en el modelo de Code First. Código en primer lugar, a continuación, creará un índice correspondiente en la base de datos.
- **La API pública de la asignación** proporciona acceso a la información de EF en cómo se asignan los tipos y propiedades a columnas y tablas en la base de datos. En versiones anteriores, esta API era interna.
- **[Capacidad de configurar los interceptores mediante el archivo App/Web.config](~/ef6/fundamentals/configuring/config-file.md)**  permite interceptores agregarse sin volver a compilar la aplicación.
- **System.Data.Entity.Infrastructure.Interception.DatabaseLogger**es un nuevo interceptor que facilita la tarea iniciar todas las operaciones de base de datos en un archivo. En combinación con la característica anterior, esto le permite fácilmente [activar registro de operaciones de base de datos para una aplicación implementada](~/ef6/fundamentals/configuring/config-file.md), sin necesidad de volver a compilar.
- **Detección de cambios de modelo de migraciones** se ha mejorado para que las migraciones con scaffold son más precisas; también se mejoró el rendimiento del proceso de detección de cambios.
- **Mejoras de rendimiento** incluidas las operaciones de reducción de la base de datos durante la inicialización, las optimizaciones para la comparación de igualdad null en las consultas LINQ, más rápido ver generación (creación de modelos) en otros escenarios y más eficaz materialización de las entidades registradas con varias asociaciones.

## <a name="ef-602"></a>EF 6.0.2
6.0.2 EF se lanzó en tiempo de ejecución a NuGet en diciembre de 2013.
Esta versión de revisión se limita a la solución de problemas que se introdujeron en el lanzamiento de EF6 (regresiones de rendimiento y de comportamiento desde EF5).

## <a name="ef-601"></a>EF 6.0.1
EF 6.0.1 en tiempo de ejecución se lanzó en NuGet en octubre de 2013 simultáneamente con EF 6.0.0, porque se ha incrustado en una versión de Visual Studio que había bloqueado unos meses antes.
Esta versión de revisión se limita a la solución de problemas que se introdujeron en el lanzamiento de EF6 (regresiones de rendimiento y de comportamiento desde EF5).
Los cambios más importantes eran corregir algunos problemas de rendimiento durante la preparación para los modelos EF.
Esto era importante, ya que el rendimiento de preparación fue de un área de enfoque en EF6 y estos problemas eran negando algunas de las otras mejoras de rendimiento realizadas en EF6.

## <a name="ef-60"></a>EF 6.0
EF 6.0.0 en tiempo de ejecución se lanzó en NuGet en octubre de 2013.
Se trata de la primera versión en la que se incluye un tiempo de ejecución EF completo en el [paquete NuGet de EntityFramework](https://www.nuget.org/packages/EntityFramework/) que no depende de los bits EF que forman parte de .NET Framework.
Mover las partes restantes del tiempo de ejecución para el paquete NuGet necesita un número de cambio de código existente.
Consulte la sección sobre [actualizar a Entity Framework 6](upgrading-to-ef6.md) para obtener más detalles sobre los pasos manuales necesarios para la actualización.

Esta versión incluye numerosas características nuevas.
Las siguientes características funcionan para los modelos creados con Code First o EF Designer:

- **[Consulta de Async y guardar](~/ef6/fundamentals/async.md)**  agrega compatibilidad con los patrones asincrónicos basados en tareas que se introdujeron en .NET 4.5.
- **[Resistencia de conexión](~/ef6/fundamentals/connection-resiliency/retry-logic.md)**  permite la recuperación automática de errores transitorios de conexión.
- **[Configuración basada en código](~/ef6/fundamentals/configuring/code-based.md)**  le ofrece la opción de realizar la configuración, que tradicionalmente se han realizado en un archivo de configuración en el código.
- **[Resolución de dependencia](~/ef6/fundamentals/configuring/dependency-resolution.md)**  introduce compatibilidad con el localizador de servicio de patrón y nos hemos factorizados algunas partes de funcionalidad que se pueden reemplazar con implementaciones personalizadas.
- **[Registro de intercepción/SQL](~/ef6/fundamentals/logging-and-interception.md)**  proporciona bloques de creación de bajo nivel para la intercepción de operaciones de EF con el registro de SQL simple basado en la parte superior.
- **Mejoras de capacidad de prueba** que sea más fácil crear dobles de pruebas para DbContext y DbSet cuando [mediante un marco de simulación](~/ef6/fundamentals/testing/mocking.md) o [escribir sus propias de dobles de pruebas](~/ef6/fundamentals/testing/writing-test-doubles.md).
- **[DbContext ahora se puede crear con una DbConnection que ya está abierta](~/ef6/fundamentals/connection-management.md)**  lo que permite escenarios donde es útil si se pudo abrir la conexión al crear el contexto (como compartir una conexión entre componentes donde no puede garantizar el estado de la conexión).
- **[Ha mejorado la compatibilidad con transacciones](~/ef6/saving/transactions.md)**  proporciona compatibilidad para una transacción externa, el marco de trabajo, así como mejorar la creación de una transacción dentro del marco de.
- **Las enumeraciones, espacial y un mejor rendimiento en .NET 4.0** : moviendo los componentes principales que solían haber en .NET Framework en el paquete NuGet de EF, podemos ofrecer compatibilidad de enumeraciones, tipos de datos espaciales y las mejoras de rendimiento de EF5 en .NET 4.0.
- **Mejorado el rendimiento de Enumerable.Contains en consultas LINQ**.
- **Preparación mejorada (generación de vistas) de tiempo**, especialmente para los modelos grandes.
- **Pluralización conectable &amp; Singularización servicio**.
- **Las implementaciones personalizadas de igual a o GetHashCode** en entidad ahora se admiten clases.
- **DbSet.AddRange/RemoveRange** proporciona una forma optimizada para agregar o quitar varias entidades de un conjunto.
- **DbChangeTracker.HasChanges** proporciona una manera fácil y eficaz para ver si hay cambios pendientes se guarden en la base de datos.
- **SqlCeFunctions** proporciona un SQL Compact a SqlFunctions equivalente.

Las siguientes características se aplican sólo a Code First:

- **[Convenciones de Code First personalizadas](~/ef6/modeling/code-first/conventions/custom.md)**  permiten escribir sus propias convenciones para ayudar a evitar repetir la configuración. Se proporciona una API sencilla para las convenciones ligeras, así como algunos bloques de creación más complejos para que pueda crear convenciones más complicadas.
- **[Codificar la primera asignación a los procedimientos almacenados Insert, Update o Delete](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)**  ahora se admite.
- **[Scripts de migración idempotentes](~/ef6/modeling/code-first/migrations/index.md)**  que pueda generar un script SQL que se puede actualizar una base de datos en cualquier versión hasta la versión más reciente.
- **[Tabla de historial de migraciones configurable](~/ef6/modeling/code-first/migrations/history-customization.md)**  le permite personalizar la definición de la tabla de historial de migraciones. Esto es especialmente útil para los proveedores de base de datos que requieren los tipos de datos adecuado etc. especificado para que la tabla de historial de migraciones funcione correctamente.
- **Varios contextos por base de datos** quita la limitación anterior de un modelo de Code First por base de datos cuando se usa migraciones o cuando Code First crea automáticamente la base de datos.
- **[DbModelBuilder.HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)**  es una API de Code First nuevo que permite el esquema de base de datos predeterminado para un modelo de Code First para configurarse en un solo lugar. Anteriormente, el esquema predeterminado de Code First era rígida &quot;dbo&quot; y era la única manera de configurar el esquema a la que pertenecía una tabla mediante la API ToTable.
- **Método DbModelBuilder.Configurations.AddFromAssembly** le permite agregar fácilmente todas las clases de configuración definidas en un ensamblado cuando se usa clases de configuración con la API Fluent de Code First.
- **[Operaciones de migración personalizado](http://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)**  permitía agregar operaciones adicionales que se usará en las migraciones basadas en código.
- **Se cambia el nivel de aislamiento de transacción predeterminado para READ_COMMITTED_SNAPSHOT** para bases de datos creados mediante Code First, lo que permite mayor escalabilidad y menos interbloqueos.
- **Entidad y tipos complejos pueden ahora clases nestedinside**. |

## <a name="ef-50"></a>EF 5.0
EF 5.0.0 en tiempo de ejecución se lanzó en NuGet en agosto de 2012.
Esta versión presenta algunas nuevas características incluidas enum, funciones con valores de tabla, tipos de datos espaciales y diversas mejoras del rendimiento.

Entity Framework Designer en Visual Studio 2012 también presenta compatibilidad con diagramas de varias por modelo, color de las formas en la importación de batch y de superficie de diseño de los procedimientos almacenados.

Esta es una lista de contenido que reunimos específicamente para la versión de EF 5.

-   [Entrada de la versión 5 de EF](http://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   Nuevas características de EF5
    -   [Enumeración se admiten en primer lugar en el código](~/ef6/modeling/code-first/data-types/enums.md)
    -   [Compatibilidad de enumeraciones en el Diseñador de EF](~/ef6/modeling/designer/data-types/enums.md)
    -   [Tipos de datos espaciales en el código en primer lugar](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [Tipos de datos espaciales en el Diseñador de EF](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Proveedor de compatibilidad para tipos espaciales](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Funciones con valores de tabla](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Varios diagramas por modelo](~/ef6/modeling/designer/multiple-diagrams.md)
-   Configurar el modelo
    -   [Creación de un modelo](~/ef6/modeling/index.md)
    -   [Las conexiones y los modelos](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Consideraciones sobre el rendimiento](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Trabajar con Microsoft SQL Azure](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Archivo de configuración](~/ef6/fundamentals/configuring/config-file.md)
    -   [Glosario](~/ef6/resources/glossary.md)
    -   Code First
        -   [Code First en una nueva base de datos (tutorial y vídeo)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Code First en una base de datos (tutorial y vídeo)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Convenciones](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Anotaciones de datos](~/ef6/modeling/code-first/data-annotations.md)
        -   [API de Fluent: configuración y asignación & tipos de propiedades](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [API de Fluent: configurar las relaciones](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [API Fluent con VB.NET](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Migraciones de Code First](~/ef6/modeling/code-first/migrations/index.md)
        -   [Migraciones automáticas de Code First](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrate.exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [Definir DbSets](~/ef6/modeling/code-first/dbsets.md)
    -   EF Designer
        -   [En primer lugar el modelo (tutorial y vídeo)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Base de datos en primer lugar (tutorial y vídeo)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Tipos complejos](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [Las asociaciones y relaciones](~/ef6/modeling/designer/relationships.md)
        -   [Patrón de herencia de TPT](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [Patrón de herencia de TPH](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Consulta con los procedimientos almacenados](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Procedimientos almacenados con varios conjuntos de resultados](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Insertar, actualizar y eliminar con procedimientos almacenados](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Asignar una entidad a varias tablas (separación de entidad)](~/ef6/modeling/designer/entity-splitting.md)
        -   [Varias entidades se asignan a una tabla (la división de tablas)](~/ef6/modeling/designer/table-splitting.md)
        -   [Definir consultas](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Plantillas de generación de código](~/ef6/modeling/designer/codegen/index.md)
        -   [Revertir a un ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   Con el modelo
    -   [Trabajar con DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [Buscar y consultar entidades](~/ef6/querying/index.md)
    -   [Trabajar con relaciones](~/ef6/fundamentals/relationships.md)
    -   [Cargar entidades relacionadas](~/ef6/querying/related-data.md)
    -   [Trabajar con datos locales](~/ef6/querying/local-data.md)
    -   [Aplicaciones de N niveles](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Consultas SQL sin formato](~/ef6/querying/raw-sql.md)
    -   [Patrones de simultaneidad optimista](~/ef6/saving/concurrency.md)
    -   [Trabajar con servidores proxy](~/ef6/fundamentals/proxies.md)
    -   [Detectar los cambios automático](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Consultas de no seguimiento](~/ef6/querying/no-tracking.md)
    -   [Método de carga](~/ef6/querying/load-method.md)
    -   [Agregar o adjuntar y Estados de entidad](~/ef6/saving/change-tracking/entity-state.md)
    -   [Trabajar con valores de propiedad](~/ef6/saving/change-tracking/property-values.md)
    -   [Enlace de datos con WPF (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [Enlace de datos con formularios Windows Forms (formularios Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
EF 4.3.1 en tiempo de ejecución se lanzó en NuGet en febrero de 2012 poco después de EF 4.3.0.
Esta versión de revisión había incluido algunas correcciones de errores a la versión de EF 4.3 y presentó una mejor compatibilidad con LocalDB para clientes que usan EF 4.3 con Visual Studio 2012.

Esta es una lista de contenido que unimos específicamente para la versión de EF 4.3.1, la mayoría del contenido proporcionado de EF 4.1 sigue siendo aplicable al EF 4.3 también.

-   [Entrada de Blog de la versión de EF 4.3.1](http://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4.3
EF 4.3.0 en tiempo de ejecución se lanzó en NuGet en febrero de 2012.
Esta versión incluye la nueva característica de migraciones de Code First que permite una base de datos creado por Code First se puede cambiar incrementalmente a medida que evoluciona su modelo de Code First.

Esta es una lista de contenido que reunimos específicamente para la versión de EF 4.3, la mayoría del contenido proporcionado de EF 4.1 sigue siendo aplicable al EF 4.3, así:
-   [Entrada de la versión 4.3 de EF](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [Tutorial EF 4.3 de las migraciones basadas en código](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [Tutorial EF 4.3 de las migraciones automáticas](http://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4.2
EF 4.2.0 en tiempo de ejecución se lanzó en NuGet en noviembre de 2011.
Esta versión incluye correcciones de errores para EF 4.1.1 de versión.
Porque esta versión solo se incluye correcciones de errores podría haber sido EF 4.1.2 patch versión pero hemos optado por moverse a 4.2 y que nos permitirá ir desde la fecha en función de los números de versión de revisión se usa en el 4.1 libera y adoptarán el [semántica Versionsing](https://semver.org) estándar para el control de versiones semántico.

Esta es una lista de contenido que reunimos específicamente para la versión 4.2 de EF, el contenido proporcionado de EF 4.1 sigue siendo aplicable al 4.2 EF también.

-   [Entrada de la versión 4.2 de EF](http://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Tutorial de Code First](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Modelo de & base de datos del primer tutorial](http://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>4.1.1 EF
EF 4.1.10715 en tiempo de ejecución se lanzó en NuGet en julio de 2011.
Además de correcciones de errores, esta versión de revisión introdujo algunos componentes para que sea más fácil para tiempo de diseño de herramientas para trabajar con un modelo de Code First.
Estos componentes se usan por migraciones de Code First (incluidas en EF 4.3) y EF Power Tools.

Observará que la versión extraña número 4.1.10715 del paquete.
Se usa para usar las versiones de revisión en función de fecha antes de que hemos decidido adoptar [Versionamiento semántico](https://semver.org).
Piense en esta versión como EF 4.1 revisión 1 (o EF 4.1.1).

Esta es una lista de contenido que reunimos para el 4.1.1 de versión:

-   [Versión de EF 4.1.1 Post](http://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4.1
EF 4.1.10331 en tiempo de ejecución fue el primero en ser publicado en NuGet, en abril de 2011.
Esta versión incluye la API de DbContext simplificada y el flujo de trabajo Code First.

Observará que el número de versión extraño, 4.1.10331, que debería haber estado 4.1. Además, hay un 4.1.10311 versión que debería haber sido 4.1.0-rc (el "rc" significa 'release candidate').
Se usa para usar las versiones de revisión en función de fecha antes de que hemos decidido adoptar [Versionamiento semántico](https://semver.org).

Esta es una lista de contenido que reunimos para la versión 4.1. Gran parte de ella aún se aplica a las versiones posteriores de Entity Framework:

-   [Entrada de la versión 4.1 de EF](http://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Tutorial de Code First](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Modelo de & base de datos del primer tutorial](http://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure federaciones y Entity Framework](http://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4.0
Esta versión se incluyó en .NET Framework 4 y Visual Studio 2010, en abril de 2010.
Nuevas características importantes de esta versión incluyen POCO soporte técnico, asignación de clave externa, la carga diferida, las mejoras de capacidad de prueba, la generación de código personalizable y el flujo de trabajo de Model First.

Aunque era la segunda versión de Entity Framework, asignó EF 4 para alinearse con la versión de .NET Framework que lo acompaña.
Después de esta versión, hemos iniciado la disposición de Entity Framework en NuGet y había adoptado versionamiento semántico, ya que nos estábamos ya no está asociados a la versión de .NET Framework.

Tenga en cuenta que algunas versiones posteriores de .NET Framework incluidos con actualizaciones importantes para los bits EF incluye.
De hecho, muchas de las nuevas características de EF 5.0 se implementaron como mejoras en estos bits.
Sin embargo, con el fin de racionalizar la historia de control de versiones para EF, seguimos hacer referencia a los bits EF que forman parte de .NET Framework como el tiempo de ejecución de EF 4.0, mientras que constan de todas las versiones más recientes de la [paquete NuGet de EntityFramework](https://www.nuget.org/packages/EntityFramework/).         

## <a name="ef-35"></a>EF 3.5
La versión inicial de Entity Framework se incluyó en .NET 3.5 Service Pack 1 y Visual Studio 2008 SP1, publicada en agosto de 2008.
Esta versión proporciona compatibilidad básica con O/RM utilizando la base de datos primer flujo de trabajo.
