---
title: Consideraciones de rendimiento de EF4, EF5 y EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: fb184fe8720b552a2050607bb17648f0413c31d1
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459596"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>Consideraciones de rendimiento para EF 4, 5 y 6
Por David Obando, Eric Dettinger y otros

Fecha de publicación: Abril de 2012

Última actualización: mayo de 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. Introducción

Marcos de asignación relacional de objetos son una manera cómoda de proporcionar una abstracción para el acceso a datos en una aplicación orientada a objetos. Para aplicaciones. NET, Microsoft recomienda QUE O/RM es Entity Framework. Sin embargo, con cualquier abstracción de rendimiento puede volverse un problema.

Estas notas del producto se escribieron para mostrar las consideraciones de rendimiento al desarrollar aplicaciones con Entity Framework, para dar una idea de los algoritmos internos de Entity Framework que puede afectar al rendimiento de los desarrolladores y para proporcionar sugerencias para la investigación y mejorar el rendimiento en aquellas aplicaciones que usan Entity Framework. Hay un número de temas buen rendimiento ya disponibles en la web, y también hemos intentado que apunta a estos recursos, siempre que sea posible.

El rendimiento es un tema complicado. Estas notas del producto está pensado como un recurso para ayudar a realizar rendimiento relacionados con las decisiones para las aplicaciones que usan Entity Framework. Hemos incluido algunas métricas de pruebas para demostrar el rendimiento, pero estas métricas no están diseñadas como absolutos indicadores del rendimiento que ve en la aplicación.

Por motivos prácticos, este documento se supone que Entity Framework 4 se ejecuta en .NET 4.0 y Entity Framework 5 y 6 se ejecutan en .NET 4.5. Muchas de las mejoras de rendimiento para Entity Framework 5 residen dentro de los componentes principales que se incluyen con .NET 4.5.

Entity Framework 6 es un fuera de la versión de banda y no depende de los componentes de Entity Framework que se suministran con. NET. Entity Framework 6 trabajar en .NET 4.0 y 4.5 de .NET y puede ofrecer una mejora del rendimiento grande a aquellos que no ha actualizado de .NET 4.0 pero desea que los bits más recientes de Entity Framework en su aplicación. Cuando este documento hace referencia a Entity Framework 6, se refiere a la versión más reciente disponible en el momento de redactar este artículo: versión 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. Vs en frío. Ejecución de consulta activa

La primera vez que se realiza cualquier consulta contra un modelo determinado, Entity Framework realiza mucho trabajo en segundo plano para cargar y validar el modelo. Con frecuencia nos referimos a esta consulta primera como una consulta "en frío".  Se conocen como "activos" consultas más consultas en un modelo ya cargada y son mucho más rápidas.

Tomemos una visión general de tiempo empleado al ejecutar una consulta mediante Entity Framework y vea dónde están mejorando las cosas en Entity Framework 6.

**Primera ejecución de consulta: consulta frío**

| Escrituras de usuario de código                                                                                     | Acción                    | EF4 Impacto de rendimiento                                                                                                                                                                                                                                                                                                                                                                                                        | EF5 Impacto de rendimiento                                                                                                                                                                                                                                                                                                                                                                                                                                                    | EF6 Impacto de rendimiento                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Creación del contexto          | Medium                                                                                                                                                                                                                                                                                                                                                                                                                        | Medium                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Creación de la expresión de consulta | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                           | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | Ejecución de consultas LINQ      | -Carga metadatos: alta, pero se ha almacenado en caché <br/> -Ver generación: potencialmente muy alta pero almacenados en caché <br/> -Evaluación del parámetro: medio <br/> -Traducción de consultas: medio <br/> -Generación materializador: medio, pero se ha almacenado en caché <br/> -Ejecución de la consulta de base de datos: potencialmente alto <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materialización de objetos: medio <br/> -Búsqueda identity: medio | -Carga metadatos: alta, pero se ha almacenado en caché <br/> -Ver generación: potencialmente muy alta pero almacenados en caché <br/> -Evaluación del parámetro: baja <br/> -Traducción de consultas: medio, pero se ha almacenado en caché <br/> -Generación materializador: medio, pero se ha almacenado en caché <br/> -Ejecución de la consulta de base de datos: potencialmente alto (mejor las consultas en algunas situaciones) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materialización de objetos: medio <br/> -Búsqueda identity: medio | -Carga metadatos: alta, pero se ha almacenado en caché <br/> -Ver generación: medio, pero se ha almacenado en caché <br/> -Evaluación del parámetro: baja <br/> -Traducción de consultas: medio, pero se ha almacenado en caché <br/> -Generación materializador: medio, pero se ha almacenado en caché <br/> -Ejecución de la consulta de base de datos: potencialmente alto (mejor las consultas en algunas situaciones) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materialización de objetos: medio (Faster de EF5) <br/> -Búsqueda identity: medio |
| `}`                                                                                                  | Connection.Close          | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                           | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**Segunda ejecución de consulta: consulta activa**

| Escrituras de usuario de código                                                                                     | Acción                    | EF4 Impacto de rendimiento                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | EF5 Impacto de rendimiento                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | EF6 Impacto de rendimiento                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | Creación del contexto          | Medium                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Medium                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | Creación de la expresión de consulta | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | Ejecución de consultas LINQ      | -Metadata ~~cargando~~ búsqueda: ~~alta, pero se ha almacenado en caché~~ baja <br/> -Ver ~~generación~~ búsqueda: ~~potencialmente muy alta pero almacenados en caché~~ baja <br/> -Evaluación del parámetro: medio <br/> -Query ~~traducción~~ búsqueda: medio <br/> -Materializador ~~generación~~ búsqueda: ~~medio, pero se ha almacenado en caché~~ baja <br/> -Ejecución de la consulta de base de datos: potencialmente alto <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materialización de objetos: medio <br/> -Búsqueda identity: medio | -Metadata ~~cargando~~ búsqueda: ~~alta, pero se ha almacenado en caché~~ baja <br/> -Ver ~~generación~~ búsqueda: ~~potencialmente muy alta pero almacenados en caché~~ baja <br/> -Evaluación del parámetro: baja <br/> -Query ~~traducción~~ búsqueda: ~~medio, pero se ha almacenado en caché~~ baja <br/> -Materializador ~~generación~~ búsqueda: ~~medio, pero se ha almacenado en caché~~ baja <br/> -Ejecución de la consulta de base de datos: potencialmente alto (mejor las consultas en algunas situaciones) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materialización de objetos: medio <br/> -Búsqueda identity: medio | -Metadata ~~cargando~~ búsqueda: ~~alta, pero se ha almacenado en caché~~ baja <br/> -Ver ~~generación~~ búsqueda: ~~medio, pero se ha almacenado en caché~~ baja <br/> -Evaluación del parámetro: baja <br/> -Query ~~traducción~~ búsqueda: ~~medio, pero se ha almacenado en caché~~ baja <br/> -Materializador ~~generación~~ búsqueda: ~~medio, pero se ha almacenado en caché~~ baja <br/> -Ejecución de la consulta de base de datos: potencialmente alto (mejor las consultas en algunas situaciones) <br/> + Connection.Open <br/> + Command.ExecuteReader <br/> + DataReader.Read <br/> Materialización de objetos: medio (Faster de EF5) <br/> -Búsqueda identity: medio |
| `}`                                                                                                  | Connection.Close          | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Bajo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


Hay varias maneras de reducir el costo de rendimiento de las consultas en frío y calientes y echaremos un vistazo en ellos en la sección siguiente. En concreto, veremos que reduce el costo de cargar en las consultas en frío mediante las vistas generadas previamente, lo que deberían ayudar a solucionar las dificultades de rendimiento producidas durante la generación de la vista de modelo. Para consultas semiactivos, hablaremos sobre las opciones de ejecución de consulta diferentes, ninguna consulta de seguimiento y almacenamiento en caché del plan de consulta.

### <a name="21-what-is-view-generation"></a>2.1 ¿Qué es la generación de vistas?

Para entender qué vista generación es, debemos comprender primero qué son las "Vistas de asignación". Las vistas de asignación son representaciones ejecutables de las transformaciones especificadas en la asignación para cada conjunto de entidades y asociaciones. Internamente, estas vistas de asignación adoptar la forma de CQTs (árboles canónicos de consulta). Hay dos tipos de vistas de asignación:

-   Vistas de consulta: representan la transformación necesaria para pasar desde el esquema de base de datos al modelo conceptual.
-   Actualizar vistas: representan la transformación necesaria para pasar del modelo conceptual para el esquema de base de datos.

Tenga en cuenta que el modelo conceptual podría diferir de esquema de base de datos de varias maneras. Por ejemplo, se puede usar una sola tabla para almacenar los datos para dos tipos de entidad diferente. Herencia y las asignaciones no trivial desempeñan un papel en la complejidad de las vistas de asignación.

El proceso de la informática estas vistas según la especificación de la asignación es lo que llamamos generación de la vista. Generación de la vista puede bien tener lugar dinámicamente cuando se carga un modelo, o en tiempo de compilación mediante el uso de "las vistas generadas previamente"; se serializan en forma de instrucciones SQL de la entidad para una C\# o archivo de VB.

Cuando se generan vistas, también se validan. Desde la perspectiva del rendimiento, la mayoría de los costes de generación de vistas es realmente la validación de las vistas que garantiza que las conexiones entre las entidades tienen sentido y tienen la cardinalidad correcta para todas las operaciones compatibles.

Cuando se ejecuta una consulta a través de un conjunto de entidades, la consulta se combina con la vista de consulta correspondiente y se ejecuta el resultado de esta composición mediante el compilador de plan para crear la representación de la consulta que pueda entender el almacén de respaldo. Para SQL Server, el resultado final de esta compilación será una instrucción SELECT de Transact-SQL. La primera vez que se realiza una actualización a través de un conjunto de entidades, la vista de actualización se ejecuta mediante un proceso similar para transformarlos en las instrucciones DML para la base de datos de destino.

### <a name="22-factors-that-affect-view-generation-performance"></a>2.2 factores que afectan al rendimiento de generación de vistas

El rendimiento del paso de generación de vista no solo depende del tamaño del modelo, sino también en el modelo de cómo interconectados es. Si dos entidades están conectadas a través de una cadena de herencia o una asociación, se dice que estar conectado. De forma similar si dos tablas están conectadas a través de una clave externa, están conectados. A medida que aumenta el número de conectados entidades y tablas en los esquemas, la generación de vistas costo aumenta.

El algoritmo que se usa para generar y validar las vistas es exponencial en el peor de los casos, aunque utilizamos algunas optimizaciones para mejorar esta situación. Los factores más importantes que parecen afectar negativamente al rendimiento son:

-   Tamaño del modelo, que hace referencia al número de entidades y la cantidad de las asociaciones entre estas entidades.
-   Complejidad de modelo, específicamente la herencia que implican a un gran número de tipos.
-   Uso de asociaciones independientes, en lugar de las asociaciones de clave externa.

Para los modelos pequeños y sencillos, el costo puede ser lo suficientemente pequeño como para no moleste con las vistas generadas previamente. A medida que aumentan el tamaño del modelo y la complejidad, hay varias opciones reducir el costo de validación y generación de la vista.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>2.3 mediante Pre-Generated vistas para reducir el modelo de tiempo de carga

Para obtener información detallada sobre cómo usar las vistas generadas previamente en Entity Framework 6 visite [Pre-Generated vistas de asignación](~/ef6/fundamentals/performance/pre-generated-views.md)

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a>2.3.1 las vistas generadas previamente mediante la edición Community de Entity Framework Power Tools

Puede usar el [Entity Framework 6 Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) para generar vistas de modelos EDMX y Code First haciendo clic en el archivo de clase de modelo y seleccione "Generar vistas" mediante el menú de Entity Framework. La edición Community de Entity Framework Power Tools funcionan únicamente en contextos derivada de DbContext.

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>2.3.2 cómo usar las vistas generadas previamente con un modelo creado forma EDMGen

EDMGen es una utilidad que se distribuye con .NET y funciona con Entity Framework 4 y 5, pero no con Entity Framework 6. EDMGen le permite generar un archivo de modelo, la capa de objeto y las vistas de la línea de comandos. Una de las salidas será un archivo de vistas en el lenguaje elegido, VB o C\#. Se trata de un archivo de código que contiene fragmentos de código de Entity SQL para cada conjunto de entidades. Para habilitar las vistas generadas previamente, basta con incluir el archivo en el proyecto.

Si realiza modificaciones a los archivos de esquema para el modelo manualmente, deberá volver a generar el archivo de vistas. Puede hacerlo mediante la ejecución EDMGen con el **/mode:ViewGeneration** marca.

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 cómo usar vistas Pre-Generated con un archivo EDMX

También puede usar EDMGen para generar vistas para un archivo EDMX: el tema de MSDN mencionado anteriormente describe cómo agregar un evento anterior a la compilación para hacer esto - pero esto es complicado y existen algunos casos donde no es posible. Es por lo general más fácil de usar una plantilla T4 para generar las vistas cuando el modelo está en un archivo edmx.

El blog del equipo ADO.NET tiene una entrada de blog que describe cómo utilizar una plantilla T4 para la generación de vistas ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>). Esta publicación incluye una plantilla que se puede descargar y agregada al proyecto. La plantilla se escribió para la primera versión de Entity Framework, por lo que no se garantiza que funcionen con las últimas versiones de Entity Framework. Sin embargo, puede descargar un conjunto de plantillas de generación de la vista más actualizado para Entity Framework 4 y 5from la Galería de Visual Studio:

-   VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Si usa Entity Framework 6 puede obtener la vista por plantillas T4 de generación de la Galería de Visual Studio en \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>.

### <a name="24-reducing-the-cost-of-view-generation"></a>2.4, lo que reduce el costo de la generación de vistas

Uso de las vistas generadas previamente mueve el costo de la generación de la vista de modelo de carga (tiempo de ejecución) al tiempo de diseño. Aunque esto mejora el rendimiento de inicio en tiempo de ejecución, sigue experimentando la dificultad de generación de la vista mientras está desarrollando. Existen varios trucos adicionales que pueden ayudar a reducir el costo de generación de la vista, tanto en tiempo de compilación y tiempo de ejecución.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1 utilizando las asociaciones de clave externa para reducir los costos de generación de vista

Hemos visto un número de casos que cambiar drásticamente las asociaciones en el modelo de asociaciones independientes a las asociaciones de clave externa mejora el tiempo empleado en la generación de la vista.

Para demostrar esta mejora, se generan dos versiones del modelo Navision mediante EDMGen. *Nota: seeappendix Cfor una descripción del modelo Navision.* El modelo Navision es interesante para este ejercicio debido a su gran cantidad de entidades y relaciones entre ellos.

Se ha generado una versión de este modelo muy grande con asociaciones de las claves externas y la otra se generó con asociaciones independientes. Hemos, a continuación, ha superado el tiempo ¿cuánto tiempo se tardó en generar las vistas para cada modelo. Prueba Framework5 de entidad usa el método GenerateViews() de la clase EntityViewGenerator para generar las vistas, mientras que la prueba de Entity Framework 6 usa el método GenerateViews() de la clase StorageMappingItemCollection. Esto debido a la reestructuración de código que se produjeron en la base de código de Entity Framework 6.

Con Entity Framework 5, generación de la vista para el modelo con claves externas tardó 65 minutos en una máquina de laboratorio. Se desconoce el tiempo que habría necesitado para generar las vistas para el modelo que usa asociaciones independientes. Hemos dejado la prueba en ejecución durante más de un mes antes de que se ha reiniciado el equipo en nuestro laboratorio para instalar las actualizaciones mensuales.

Mediante Entity Framework 6, generación de la vista para el modelo con claves externas tardó 28 segundos en el mismo equipo de laboratorio. Generación de la vista para el modelo que usa asociaciones independientes tardó 58 segundos. Las mejoras de hecho a Entity Framework 6 en su código de generación de vista significan que muchos proyectos no tendrán las vistas generadas previamente para obtener tiempos de inicio más rápidos.

Es importante para el comentario que generar previamente las vistas en Entity Framework 4 y 5 puede hacerse con EDMGen o Entity Framework Power Tools. Para la vista de Entity Framework 6 generación puede realizarse a través de Entity Framework Power Tools o mediante programación, como se describe en [Pre-Generated asignación vistas](~/ef6/fundamentals/performance/pre-generated-views.md).

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>2.4.1.1 cómo usar las claves externas en lugar de asociaciones independientes

Al usar EDMGen o Entity Designer en Visual Studio, obtenga las claves externas de forma predeterminada y solo toma una única marca de casilla o de línea de comandos para cambiar entre las claves externas e IAs.

Si tiene un modelo de Code First grande, uso de asociaciones independientes tendrán el mismo efecto en la generación de la vista. Puede evitar este impacto mediante la inclusión de propiedades de clave externa en las clases para los objetos dependientes, aunque algunos desarrolladores tendrá en cuenta esta opción para ser contaminar su modelo de objetos. Puede encontrar más información sobre este tema en \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>.

| Cuando se usa      | Haga esto                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Diseñador de entidades | Después de agregar una asociación entre dos entidades, asegúrese de que tiene una restricción referencial. Las restricciones referenciales indicar a Entity Framework para usar las claves externas en lugar de asociaciones independientes. Para obtener más información, visite \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>. |
| EDMGen          | Cuando use EDMGen para generar los archivos de la base de datos, las claves externas se respeta y se agregará al modelo como tal. Para obtener más información sobre las distintas opciones que se expone de forma EDMGen visite [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx).                           |
| Code First      | Consulte la sección "Convención relación" de la [convenciones de Code First](~/ef6/modeling/code-first/conventions/built-in.md) tema para obtener información sobre cómo incluir propiedades de clave externa en los objetos dependientes cuando utilice Code First.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>2.4.2 moviendo el modelo a un ensamblado independiente

Cuando el modelo se incluye directamente en el proyecto de la aplicación y generar vistas a través de un evento anterior a la compilación o una plantilla T4, validación y generación de la vista llevará a cabo cada vez que se vuelve a generar el proyecto, incluso si no se ha cambiado el modelo. Si pasa el modelo a un ensamblado independiente y hacer referencia a él desde el proyecto de la aplicación, puede realizar otros cambios a la aplicación sin tener que recompilar el proyecto que contiene el modelo.

*Nota:* al mover el modelo para separar los ensamblados no olvide copiar las cadenas de conexión para el modelo en el archivo de configuración del proyecto de cliente.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 deshabilitar la validación de un modelo basado en edmx

Con modelos EDMX se validan en tiempo de compilación, incluso si el modelo se ha modificado. Si ya se ha validado el modelo, puede suprimir la validación en tiempo de compilación estableciendo la propiedad "Validar al compilar" en false en la ventana Propiedades. Al cambiar la asignación o el modelo, temporalmente puede volver a habilitar la validación para comprobar los cambios.

Tenga en cuenta que se han realizado mejoras de rendimiento para el Diseñador de Entity Framework para Entity Framework 6 y el costo de la "validar en la compilación" es mucho menor que en versiones anteriores del diseñador.

## <a name="3-caching-in-the-entity-framework"></a>3 de almacenamiento en caché en Entity Framework

Entity Framework tiene las siguientes formas de almacenamiento en caché integrado:

1.  Objeto de almacenamiento en caché: ObjectStateManager integrado en una instancia de ObjectContext realiza un seguimiento de los objetos que se han recuperado mediante esa instancia en memoria. Esto es también conocida como caché de primer nivel.
2.  Caching de Plan de consulta: volver a usar el comando generado cuando se ejecuta una consulta más de una vez.
3.  Metadatos de almacenamiento en caché: los metadatos para un modelo de uso compartido a través de diferentes conexiones con el mismo modelo.

Además de las memorias caché que ofrece EF desde el principio, un tipo especial de proveedor de datos ADO.NET, conocido como un proveedor de ajuste también se puede usar para extender el marco de entidad con una memoria caché para los resultados recuperados de la base de datos, también conocido como almacenamiento en caché de segundo nivel.

### <a name="31-object-caching"></a>3.1 objeto de almacenamiento en caché

De forma predeterminada cuando se devuelve una entidad en los resultados de una consulta, justo antes de que EF materializa, ObjectContext comprobará si una entidad con la misma clave ya se han cargado en el ObjectStateManager. Si ya hay una entidad con las mismas claves EF incluirá en los resultados de la consulta. Aunque EF sigue emitirá la consulta en la base de datos, puede omitir este comportamiento gran parte del costo de materializar la entidad varias veces.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>3.1.1 obtener entidades de la caché de objetos mediante DbContext Find

A diferencia de una consulta regular, el método Find en DbSet (API incluidas por primera vez en EF 4.1) llevará a cabo una búsqueda en la memoria antes de emitir incluso la consulta en la base de datos. Es importante tener en cuenta que dos instancias distintas de ObjectContext tendrá dos instancias de ObjectStateManager diferentes, lo que significa que tienen las memorias caché de objeto independiente.

Búsqueda usa el valor de clave principal para intentar buscar una entidad en el contexto realiza el seguimiento. Si la entidad no está en el contexto, a continuación, una consulta se ejecuta y evalúa con respecto a la base de datos y se devuelve null si no se encuentra la entidad en el contexto o en la base de datos. Tenga en cuenta que encuentre también devuelve las entidades que se han agregado al contexto, pero aún no se han guardado en la base de datos.

Hay una consideración de rendimiento que se realizará cuando el uso de la búsqueda. Las llamadas a este método de forma predeterminada desencadenarán una validación de la caché de objetos con el fin de detectar cambios siguen pendientes de confirmación a la base de datos. Este proceso puede resultar muy caro si hay un gran número de objetos en la caché de objetos o en un gráfico de objetos grandes que se va a agregar a la caché de objetos, pero también se puede deshabilitar. En algunos casos, pueden percibir a través de un orden de magnitud de diferencia en la llamada a la búsqueda de método cuando se deshabilita automáticamente detectar cambios. Aún se percibe un segundo orden de magnitud cuando el objeto está realmente en la caché frente a cuando el objeto tiene que deben recuperarse de la base de datos. Este es un gráfico de ejemplo con las mediciones tomadas con algunos de nuestros microbenchmarks, expresado en milisegundos, con una carga de 5000 entidades:

![Escala logarítmica de .NET 4.5](~/ef6/media/net45logscale.png ".NET 4.5 - escala logarítmica")

Ejemplo de búsqueda con los cambios de la detección automática deshabilitada:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

¿Qué debe tener en cuenta al usar el método Find es:

1.  Si el objeto no está en la memoria caché se niega los beneficios de buscar, pero la sintaxis es aún más sencilla que una consulta mediante la clave.
2.  Si los cambios de detección automática está habilitada puede aumentar el costo del método Find en un orden de magnitud, o incluso más según la complejidad de su modelo y la cantidad de entidades en la memoria caché de objetos.

Además, tenga en cuenta que busque sólo devuelve la entidad que está buscando y lo hace automáticamente carga sus entidades asociadas si aún no están en la caché de objetos. Si necesita recuperar las entidades asociadas, puede usar una consulta mediante la clave con la carga diligente. Para obtener más información, consulte **8.1 frente a la carga diferida. La carga diligente**.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>3.1.2 problemas de rendimiento de cuando la caché de objetos tiene muchas entidades

La caché de objetos ayuda a aumentar la capacidad de respuesta general de Entity Framework. Sin embargo, cuando la caché de objetos tiene una gran cantidad de entidades cargadas puede afectar a ciertas operaciones, como agregar, quitar, buscar, entrada, SaveChanges y mucho más. En concreto, el operaciones que desencadenan una llamada a DetectChanges se verán afectadas negativamente por las memorias caché de objetos muy grandes. DetectChanges sincroniza el gráfico de objetos con el Administrador de estado de objeto y su rendimiento determinada directamente por el tamaño del gráfico de objetos. Para obtener más información acerca de DetectChanges, consulte [seguimiento de los cambios en entidades POCO](https://msdn.microsoft.com/library/dd456848.aspx).

Cuando se usa Entity Framework 6, los desarrolladores pueden realizar llamadas a AddRange y RemoveRange directamente en una clase DbSet, en lugar de efectuar una iteración en una colección y llamar a agregar una vez por cada instancia. La ventaja de usar los métodos range es que el costo de DetectChanges solo se paga una vez para todo el conjunto de entidades en lugar de una vez por cada entidad agregada.

### <a name="32-query-plan-caching"></a>3.2 almacenamiento en caché de Plan de consulta de

La primera vez que se ejecuta una consulta, pasa por el compilador plan interno para traducir la consulta conceptual en el comando de almacén (por ejemplo, el T-SQL que se ejecuta cuando se ejecuta en SQL Server).  Si está habilitado el almacenamiento en caché del plan de consulta, la próxima vez que la consulta es ejecuta el almacén de comando se recupera directamente desde la caché del plan de consulta para su ejecución, omitiendo el compilador de plan.

La caché del plan de consulta se comparte entre las instancias de ObjectContext dentro del mismo AppDomain. No es necesario retener una instancia de ObjectContext para beneficiarse del almacenamiento en caché del plan de consulta.

#### <a name="321-some-notes-about-query-plan-caching"></a>3.2.1 algunas notas acerca de la caché de Plan de consultas

-   Se comparte la caché del plan de consulta para todos los tipos de consulta: Entity SQL, LINQ to Entities y los objetos de CompiledQuery.
-   De forma predeterminada, el almacenamiento en caché del plan de consulta está habilitado para las consultas de Entity SQL, si se ejecuta a través de un EntityCommand o un elemento ObjectQuery. También está habilitada de forma predeterminada para LINQ a consultas de las entidades de Entity Framework en .NET 4.5 y en Entity Framework 6
    -   Almacenamiento en caché del plan de consulta se puede deshabilitar estableciendo la propiedad EnablePlanCaching (en EntityCommand o ObjectQuery) en false. Por ejemplo:
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   Para las consultas parametrizadas, cambiar el valor del parámetro todavía alcanzará la consulta en caché. Pero cambiar las facetas de un parámetro (por ejemplo, tamaño, precisión o escala) llegará a una entrada distinta en la memoria caché.
-   Al utilizar Entity SQL, la cadena de consulta forma parte de la clave. Cambiar la consulta en absoluto dará como resultado en entradas de caché diferente, incluso si las consultas son funcionalmente equivalentes. Esto incluye los cambios de mayúsculas y minúsculas o espacio en blanco.
-   Al usar LINQ, se procesa la consulta para generar una parte de la clave. Cambiar la expresión LINQ, por tanto, se generará una clave diferente.
-   Pueden aplicar otras limitaciones técnicas; Para obtener más información, consulte Autocompiled consultas.

#### <a name="322------cache-eviction-algorithm"></a>3.2.2 algoritmo de expulsión de caché

Comprender cómo el algoritmo interno funciona le ayudarán a averiguar cuando para habilitar o deshabilitar caché de plan de consulta. El algoritmo de limpieza es como sigue:

1.  Una vez que la memoria caché contiene un número de conjunto de entradas (800), se inicia un temporizador que periódicamente (una vez por minuto) barre la memoria caché.
2.  Durante los rastreos de memoria caché, se quitan las entradas de la memoria caché en un LFRU (menos frecuentemente – usados recientemente) base. Este algoritmo tiene número de llamadas y la antigüedad en cuenta al decidir las entradas que se expulsan.
3.  Al final de cada barrido de caché, la memoria caché nuevo contiene 800 entradas.

Todas las entradas de caché se consideran iguales al determinar las entradas que se va a expulsar. Esto significa que el comando de almacén para un CompiledQuery tiene la misma probabilidad de expulsión que el comando de almacén para una consulta de Entity SQL.

Tenga en cuenta que el temporizador de expulsión de caché es entrara en acción cuando hay 800 entidades en la memoria caché, pero la memoria caché solo es más de 60 segundos después de este temporizador se inicia. Esto significa que hasta 60 segundos la memoria caché puede crecer a ser bastante grande.

#### <a name="323-------test-metrics-demonstrating-query-plan-caching-performance"></a>3.2.3 probar las métricas que muestran el rendimiento de almacenamiento en caché el plan de consulta

Para mostrar el efecto del plan de consulta en el rendimiento de la aplicación de almacenamiento en caché, se realiza una prueba donde se ejecuta un número de consultas de Entity SQL en el modelo de Navision. Consulte el apéndice para obtener una descripción del modelo Navision y los tipos de consultas que se ejecutaron. En esta prueba, primero se recorrer en iteración la lista de consultas y ejecute cada uno de ellos una vez para agregarlos a la memoria caché (si está habilitado el almacenamiento en caché). Este paso es untimed. A continuación, nos suspender el subproceso principal de más de 60 segundos permitir que la memoria caché de barrido de hiperparámetros para tener lugar; Por último, establecemos una iteración por la hora de la lista una 2ª para ejecutar las consultas en caché. Además, le caché de planes de SQL Server se vacíe antes de que se ejecuta cada conjunto de consultas para que reflejen de las horas que obtenemos con precisión el beneficio proporcionado por la caché del plan de consulta.

##### <a name="3231-------test-results"></a>3.2.3.1 los resultados de pruebas

| Prueba                                                                   | EF5 sin caché | EF5 en caché | EF6 sin caché | EF6 en caché |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| Enumerar todas las 18723 consultas                                          | 124          | 125.4      | 124.3        | 125.3      |
| Evitar barrido (solo el primero 800 consultas, independientemente de complejidad)  | 41.7         | 5.5        | 40,5         | 5.4        |
| Solo las consultas AggregatingSubtotals (178 total: lo que evita el barrido) | 39.5         | 4.5        | 38.1         | 4.6        |

*Todas las horas en segundos.*

Moral - cuando la ejecución de lotes de consultas distintivas (por ejemplo, creado dinámicamente las consultas), almacenamiento en caché no ayuda y el vaciado resultante de la memoria caché puede mantener las consultas que se beneficiarían de la mayoría de almacenamiento en caché del plan de usarla realmente.

Las consultas AggregatingSubtotals son más complejas de las consultas que hemos probado con. Según lo previsto, es más complejo de la consulta, más ventajas verá del almacenamiento en caché del plan de consulta.

Dado que un CompiledQuery es realmente una consulta LINQ con su plan en caché, la comparación de un CompiledQuery frente a la consulta de Entity SQL equivalente debe tener resultados similares. De hecho, si una aplicación tiene una gran cantidad de consultas de Entity SQL dinámicas, llenar la memoria caché con consultas también provocará CompiledQueries "descompilar" cuando se vacían de la memoria caché. En este escenario, puede mejorar el rendimiento al deshabilitar el almacenamiento en caché en las consultas dinámicas para dar prioridad a la CompiledQueries. Mejor aún, por supuesto, sería volver a escribir la aplicación para usar consultas parametrizadas en lugar de consultas dinámicas.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3.3 usar CompiledQuery para mejorar el rendimiento con las consultas LINQ

Nuestras pruebas indican que uso CompiledQuery puede aportar una ventaja de % 7 a través de autocompiled consultas LINQ; Esto significa que se pierda 7% menos tiempo ejecutando código de la pila de Entity Framework. no significa que la aplicación estará 7% más rápido. Por lo general, el costo de escribir y mantener los objetos de CompiledQuery de EF 5.0 no puede ser que vale la pena el problema en comparación con las ventajas. El kilometraje puede variar, así que actúan sobre esta opción si el proyecto requiere la inserción adicional. Tenga en cuenta que CompiledQueries solo son compatibles con modelos de ObjectContext derivado y no es compatible con los modelos derivada de DbContext.

Para obtener más información sobre la creación e invocar un CompiledQuery, consulte [consultas compiladas (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx).

Existen dos consideraciones que debe seguir al usar un CompiledQuery, es decir, el requisito para utilizar instancias estáticas y los problemas que tienen con capacidad de composición. A continuación se sigue una explicación detallada de estos dos factores.

#### <a name="331-------use-static-compiledquery-instances"></a>3.3.1 usar instancias estáticas de CompiledQuery

Dado que compilar una consulta LINQ es un proceso lento, no queremos hacerlo cada vez que se necesita capturar datos de la base de datos. Las instancias de CompiledQuery le permiten compilar una vez y ejecutar varias veces, pero tener cuidado y adquirir para volver a usar la misma instancia de CompiledQuery cada vez en lugar de compilarlo y otra vez. El uso de los miembros estáticos para almacenar las instancias de CompiledQuery es necesario; en caso contrario, no verá ninguna ventaja.

Por ejemplo, supongamos que la página tiene el siguiente cuerpo de método para controlar la visualización de los productos para la categoría seleccionada:

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

En este caso, creará una nueva instancia de CompiledQuery sobre la marcha cada vez que se llama al método. En lugar de ver las ventajas de rendimiento al recuperar el comando de almacén de la caché del plan de consulta, el CompiledQuery pasarán por el compilador plan cada vez que se crea una nueva instancia. De hecho, se pueden contaminar la caché del plan de consulta con una nueva entrada de CompiledQuery cada vez que se llama al método.

En su lugar, desea crear una instancia estática de la consulta compilada, por lo que se está invocando la misma consulta compilada cada vez que se llama al método. Una manera de esto es mediante la adición de la instancia de CompiledQuery como miembro del contexto del objeto.  Posteriormente, puede hacer cosas un poco claro mediante el acceso a la CompiledQuery a través de un método auxiliar:

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

Este método auxiliar se invocaría como sigue:

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-------composing-over-a-compiledquery"></a>3.3.2 componer sobre un CompiledQuery

La capacidad de crear a través de cualquier consulta LINQ es extremadamente útil; Para ello, basta con invocar un método después de IQueryable como *Skip()* o *Count()*. Sin embargo, si se realiza básicamente, devuelve un nuevo objeto IQueryable. Aunque no hay nada que impida técnicamente componer sobre un CompiledQuery, esto hará que la generación de un nuevo objeto IQueryable requiere pasando por el compilador plan nuevo.

Algunos componentes hará uso de IQueryable compuesto objetos para habilitar la funcionalidad avanzada. Por ejemplo, ASP. GridView de la red puede estar enlazado a datos a un objeto IQueryable a través de la propiedad SelectMethod. El control GridView, a continuación, se creará sobre este objeto IQueryable para permitir la ordenación y paginación a través del modelo de datos. Como puede ver, utilizando un CompiledQuery de GridView podría no alcanzar la consulta compilada pero generaría una nueva consulta autocompiled.

El equipo de asesoramiento al cliente se describe en su blog "Potencial rendimiento problemas con compilado LINQ consulta vuelve a compilar": <http://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/potential-performance-issues-with-compiled-linq-query-re-compiles.aspx>.

Un único lugar donde puede encontrarse con esto es cuando se agregan filtros progresivos a una consulta. Por ejemplo, suponga que tiene una página de los clientes con varias listas desplegables para filtros opcionales (por ejemplo, país y OrdersCount). Estos filtros puede redactar sobre los resultados de IQueryable de un CompiledQuery, pero hacerlo dará lugar a en la nueva consulta que pasar por el compilador plan cada vez que la ejecute.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Para evitar esta recompilación, puede volver a escribir la CompiledQuery para los filtros posibles de tener en cuenta:

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

Que se invoquen en la interfaz de usuario, como:

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 Un inconveniente es que el comando generado siempre tendrá los filtros con las comprobaciones de null, pero deben ser bastante sencillas para el servidor de base de datos optimizar:

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>3.4 almacenamiento en caché de metadatos

Entity Framework también admite el almacenamiento en caché de metadatos. Esto es esencialmente almacenamiento en caché de información de tipo y la información de asignación de tipo a base de datos a través de diferentes conexiones con el mismo modelo. La caché de metadatos es única para cada dominio de aplicación.

#### <a name="341-metadata-caching-algorithm"></a>3.4.1 algoritmo de almacenamiento en caché de metadatos

1.  Información de metadatos para un modelo se almacena en un objeto ItemCollection para cada EntityConnection.
    -   Como una nota al margen, hay diferentes objetos ItemCollection para las distintas partes del modelo. Por ejemplo, StoreItemCollections contiene la información sobre el modelo de base de datos; ObjectItemCollection contiene información sobre el modelo de datos; EdmItemCollection contiene información sobre el modelo conceptual.

2.  Si dos conexiones usan la misma cadena de conexión, comparten la misma instancia ItemCollection.
3.  Es posible que las cadenas de conexión funcionalmente equivalentes pero textualmente diferentes en las memorias caché de metadatos diferentes. Simplemente cambiando el orden de los tokens debe producir metadatos compartidos se dividir las cadenas de conexión. Pero dos cadenas de conexión que parecen funcionalmente la misma no se pueden evaluar como idénticos después de la tokenización.
4.  ItemCollection se comprueba periódicamente para su uso. Si se determina que no se ha accedido recientemente un área de trabajo, se marcará para la limpieza en el siguiente rastreo de memoria caché.
5.  Simplemente crear una EntityConnection hará que una memoria caché de metadatos para crearse (aunque no se inicializarán las colecciones de elementos en ella hasta que se abre la conexión). Esta área de trabajo permanecerá en memoria hasta que el algoritmo de almacenamiento en caché determina no es "en uso".

Customer Advisory Team ha escrito una entrada de blog que describe que contiene una referencia a un objeto ItemCollection con el fin de evitar la "degradación" al usar los modelos grandes: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 la relación entre el almacenamiento en caché de metadatos y la caché de Plan de consultas

La instancia de caché del plan de consulta reside en ItemCollection del MetadataWorkspace de tipos de almacén. Esto significa que los comandos de almacenamiento en caché se utilizará para las consultas en cualquier contexto que se crea una instancia con un MetadataWorkspace determinado. También significa que si tiene dos cadenas de conexiones que son ligeramente diferentes y no coinciden después de dividir en tokens, habrá consulta diferentes instancias de caché de plan.

### <a name="35-results-caching"></a>3.5 almacenamiento en caché de resultados de

Con los resultados de almacenamiento en caché (también conocido como "segundo nivel de almacenamiento en caché"), mantener los resultados de consultas en una caché local. Al emitir una consulta, aparece en primer lugar si los resultados están disponibles localmente antes de la consulta en el almacén. Aunque el almacenamiento en caché de resultados no es compatible directamente con Entity Framework, es posible agregar una caché de segundo nivel con un proveedor de ajuste. Un ejemplo de proveedor de ajuste con una memoria caché de segundo nivel es del Alachisoft [Entity Framework segundo nivel de caché en función de NCache](http://www.alachisoft.com/ncache/entity-framework.html).

Esta implementación de almacenamiento en caché de segundo nivel es una funcionalidad insertada que tiene lugar después de que se ha evaluado la expresión LINQ (y funcletized) y el plan de ejecución de consulta es calculado o se recupera de la caché de primer nivel. La memoria caché de segundo nivel, a continuación, almacenará solo los resultados de la base de datos sin procesar, por lo que la canalización de materialización ejecuta posteriormente.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>3.5.1 referencias adicionales de para con el proveedor de ajuste de almacenamiento en caché de resultados

-   Julie Lerman ha escrito un artículo MSDN "Segundo nivel de almacenamiento en caché en Entity Framework y Windows Azure" que incluye cómo actualizar el proveedor de ajuste de ejemplo para usar el almacenamiento en caché de Windows Server AppFabric: [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   Si trabaja con Entity Framework 5, el blog del equipo tiene una entrada de blog que describe cómo empezar a funcionar con el proveedor de almacenamiento en caché para Entity Framework 5: \< http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>. También incluye una plantilla T4 para ayudar a automatizar la adición de la memoria caché a nivel 2 º al proyecto.

## <a name="4-autocompiled-queries"></a>4 consultas Autocompiled

Cuando se emite una consulta en una base de datos mediante Entity Framework, debe pasar a través de una serie de pasos antes de realmente materializar los resultados. un paso de este tipo es la compilación de la consulta. Consultas de Entity SQL se sabe que tiene un buen rendimiento tal como se almacenan automáticamente en caché, por lo que el tiempo de segundo o tercer ejecutar la misma consulta puede omitir el compilador de plan y utilizar el plan almacenado en caché en su lugar.

Entity Framework 5 introdujo el almacenamiento en caché automático para LINQ a consultas de las entidades también. En ediciones anteriores de Entity Framework creando un CompiledQuery para acelerar el rendimiento de la era una práctica común, como esto haría que las de LINQ para consultar entidades almacenables en caché. Puesto que el almacenamiento en caché se realiza automáticamente sin necesidad de utilizar un CompiledQuery, llamamos a esta característica "autocompiled consultas". Para obtener más información acerca de la caché del plan de consulta y sus mecanismos, consulte almacenamiento en caché del Plan de consulta.

Entity Framework detecta cuando una consulta requiere volver a compilar, y lo hace cuando se invoca la consulta aunque había sido compilado antes. Condiciones comunes que hacer que se vuelvan a compilar la consulta son:

-   Cambiar MergeOption asociado a la consulta. No se usará la consulta en caché, en su lugar, el compilador de plan se ejecutará de nuevo y se almacenan en caché el plan recién creado.
-   Cambiar el valor de ContextOptions.UseCSharpNullComparisonBehavior. Obtener el mismo efecto que el cambio de MergeOption.

Otras condiciones pueden impedir que la consulta utilizando la memoria caché. Algunos ejemplos comunes son:

-   Uso de IEnumerable&lt;T&gt;. Contiene&lt;&gt;(valor de T).
-   Uso de funciones que generan consultas con constantes.
-   Mediante las propiedades de un objeto no asignados.
-   La vinculación a otra consulta que requiere volver a compilar la consulta.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4.1 con IEnumerable&lt;T&gt;. Contiene&lt;T&gt;(valor de T)

Entity Framework no almacena en caché las consultas que invocan IEnumerable&lt;T&gt;. Contiene&lt;T&gt;(valor de T) frente a una colección en memoria, ya que los valores de la colección se consideran volátiles. La consulta de ejemplo siguiente no se almacenará en caché, por lo que siempre se procesarán por el compilador de plan:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

Tenga en cuenta que se ejecuta el tamaño de la interfaz IEnumerable contra qué Contains determina la rapidez o lentitud cómo se compila la consulta. Rendimiento puede verse afectado significativamente al utilizar las colecciones grandes, como se muestra en el ejemplo anterior.

Entity Framework 6 contiene optimizado la forma IEnumerable&lt;T&gt;. Contiene&lt;T&gt;(valor de T) funciona cuando se ejecutan las consultas. El código SQL que se genera es mucho más rápido para generar y más legible, y en la mayoría de los casos también se ejecuta más rápido en el servidor.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4.2 utilizando las funciones que generan consultas con constantes

Los operadores Skip(), utilice, Contains() y DefautIfEmpty() LINQ no generan consultas SQL con parámetros, pero en su lugar para colocar los valores que se les pasados como constantes. Por este motivo, las consultas en caso contrario, podrían ser idéntica acaban contaminar la consulta plan la memoria caché tanto en la pila EF en el servidor de base de datos y no obtener reutilized a menos que el mismas constantes se utilizan en una ejecución de consulta subsiguientes. Por ejemplo:

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

En este ejemplo, cada vez que se ejecuta esta consulta con un valor diferente para el Id. de la consulta se compilarán en un nuevo plan.

En el pago particular atención al uso de Skip y Take al realizar la paginación. En EF6, estos métodos tienen una sobrecarga de lambda que se convierte de manera efectiva el plan de consulta almacenado en caché reutilizables porque EF puede capturar variables pasadas a estos métodos y trasladarlas a SQLparameters. Esto también ayuda a mantener la memoria caché más limpio, ya que de lo contrario, cada consulta con una constante distinta de Skip y Take obtendría su propia entrada de caché del plan de consulta.

Tenga en cuenta el código siguiente, que no es óptimo, pero solo está pensado para ejemplificar esta clase de consultas:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

Una versión más rápida de este mismo código implicaría una llamada a Skip con una expresión lambda:

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i \< count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

El segundo fragmento de código puede ejecutarse un 11% más rápido porque se utiliza el mismo plan de consulta cada vez que se ejecuta la consulta, lo que ahorra tiempo de CPU y evita contaminar la caché de consultas. Además, dado que el parámetro Skip es en una clausura el código podría también este aspecto ahora:

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4.3 mediante las propiedades de un objeto no asignado

Cuando una consulta usa las propiedades de un tipo de objeto no asignado como un parámetro, a continuación, la consulta no se almacene en caché. Por ejemplo:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

En este ejemplo, suponga que la clase NonMappedType no forma parte del modelo de entidad. Esta consulta puede modificarse fácilmente para no usar un tipo no asignado y en su lugar, use una variable local como parámetro a la consulta:

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

En este caso, la consulta podrá se almacene en caché y se beneficiará de la caché del plan de consulta.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4.4 vinculación a las consultas que requieren volver a compilar

Siguiendo el mismo ejemplo anterior, si tiene una segunda consulta que se basa en una consulta que necesite volver a compilar, la segunda consulta toda también se volverá a compilar. Este es un ejemplo para ilustrar este escenario:

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

El ejemplo es genérico, pero ilustra cómo vincular a firstQuery está causando secondQuery no se pueden obtener en la memoria caché. Si no se hubiera una consulta que requiere volver a compilar firstQuery, a continuación, secondQuery habría ha almacenado en caché.

## <a name="5-notracking-queries"></a>5 consultas NoTracking

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5.1 deshabilitar el seguimiento de cambios para reducir la sobrecarga de administración de estado

Si encuentra en un escenario de solo lectura y desea evitar la sobrecarga de carga de los objetos en el ObjectStateManager, se pueden emitir consultas de "Seguimiento de n".  En el nivel de consulta, se pueden deshabilitar el seguimiento de cambios.

Sin embargo, tenga en cuenta que al deshabilitar, seguimiento de cambios eficaz desactiva la caché de objetos. Al consultar una entidad, no podemos omitimos materialización extrayendo los resultados de consulta materializada previamente de ObjectStateManager. Si consulta repetidamente las mismas entidades en el mismo contexto, es posible que vea realmente un rendimiento beneficiarse de la habilitación de seguimiento de cambios.

Al consultar el uso de ObjectContext, las instancias de ObjectQuery y ObjectSet recordará un MergeOption una vez que se ha configurado y las consultas que están compuestas por ellos heredará MergeOption efectivo de la consulta primaria. Al usar DbContext, se puede deshabilitar el seguimiento llamando el modificador AsNoTracking() en DbSet.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 deshabilitar el seguimiento de cambios para una consulta al usar DbContext

Puede cambiar el modo de una consulta a NoTracking al encadenar una llamada al método AsNoTracking() en la consulta. A diferencia de ObjectQuery, las clases DbSet y DbQuery en la API de DbContext no tienen una propiedad mutable para MergeOption.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 deshabilitar en el nivel de consulta mediante ObjectContext con establecimiento de seguimiento de cambios

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 deshabilitar el seguimiento de cambios para una entidad completa establecer uso de ObjectContext

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52-test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5.2 comprobar las métricas que muestran las ventajas de rendimiento de consultas NoTracking

En esta prueba nos centramos a costa de rellenar el ObjectStateManager mediante la comparación de seguimiento a las consultas NoTracking para el modelo de Navision. Consulte el apéndice para obtener una descripción del modelo Navision y los tipos de consultas que se ejecutaron. En esta prueba, hemos recorrer en iteración la lista de consultas y ejecutar una vez cada uno de ellos. Se produjo dos variaciones de la prueba, una vez con NoTracking consultas y una vez con la opción de combinación predeterminada de "AppendOnly". Se ha ejecutado cada variación 3 veces y toma el valor medio de las ejecuciones. Entre las pruebas se borrar la caché de consultas en SQL Server y reduce la tempdb que se ejecute los comandos siguientes:

1.  DBCC DROPCLEANBUFFERS
2.  DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (tempdb, 0)

Resultados de pruebas, mediana se ejecuta más de 3:

|                        | NINGÚN SEGUIMIENTO: EL ESPACIO DE TRABAJO | SIN SEGUIMIENTO: HORA | ANEXAR SOLO: ESPACIO DE TRABAJO | ANEXAR SOLO: HORA |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 ms         | 596545536                 | 1273042 ms         |
| **Entity Framework 6** | 647127040                 | 190228 ms          | 832798720                 | 195521 ms          |

Entity Framework 5 tendrá una superficie de memoria menor al final de la ejecución de Entity Framework 6. La memoria adicional utilizada por Entity Framework 6 es el resultado de las estructuras de memoria adicional y el código que permiten a las nuevas características y un mejor rendimiento.

También hay una diferencia clara en la superficie de memoria cuando se usa el ObjectStateManager. Entity Framework 5 aumenta su impacto al 30% al realizar el seguimiento de todas las entidades que se materializan a partir de la base de datos. Entity Framework 6 aumenta su impacto en un 28% al hacerlo.

En términos de tiempo, Entity Framework 6 supera a Entity Framework 5 en esta prueba por un margen de gran tamaño. Entity Framework 6 ha completado la prueba en aproximadamente 16% del tiempo consumido por Entity Framework 5. Además, Entity Framework 5 tarda más de 9% en completarse cuando se usa el ObjectStateManager. En comparación, Entity Framework 6 está usando 3% más tiempo cuando se usa el ObjectStateManager.

## <a name="6-query-execution-options"></a>6 opciones de ejecución de consulta

Entity Framework ofrece varias formas de consulta. Se le Eche un vistazo a las siguientes opciones, compare las ventajas y desventajas de cada uno y examinar sus características de rendimiento:

-   LINQ to Entities.
-   Sin seguimiento LINQ to Entities.
-   Entity SQL a través de un elemento ObjectQuery.
-   Entity SQL a través de un EntityCommand.
-   ExecuteStoreQuery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-------linq-to-entities-queries"></a>6.1 consultas LINQ to Entities

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**Profesionales**

-   Adecuado para las operaciones CUD.
-   Objetos materializados completamente.
-   Más fáciles de escribir con la sintaxis se integran en el lenguaje de programación.
-   Buen rendimiento.

**Desventajas**

-   Ciertas restricciones técnicas, tales como:
    -   Como resultado patrones de uso de DefaultIfEmpty para consultas OUTER JOIN en consultas más complejas que instrucciones sencillas OUTER JOIN de Entity SQL.
    -   Todavía no se puede utilizar LIKE con coincidencia de patrones generales.

### <a name="62-------no-tracking-linq-to-entities-queries"></a>6.2 ningún seguimiento LINQ a consultas de entidades

Cuando el contexto deriva ObjectContext:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

Cuando el contexto deriva de DbContext:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**Profesionales**

-   Mejorar el rendimiento a través de las consultas LINQ normales.
-   Objetos materializados completamente.
-   Más fáciles de escribir con la sintaxis se integran en el lenguaje de programación.

**Desventajas**

-   No es adecuado para las operaciones CUD.
-   Ciertas restricciones técnicas, tales como:
    -   Como resultado patrones de uso de DefaultIfEmpty para consultas OUTER JOIN en consultas más complejas que instrucciones sencillas OUTER JOIN de Entity SQL.
    -   Todavía no se puede utilizar LIKE con coincidencia de patrones generales.

Tenga en cuenta que las consultas que las propiedades escalares del proyecto no se realiza el seguimiento incluso si no se especifica el NoTracking. Por ejemplo:

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

Esta consulta en particular no especifica explícitamente que se va a NoTracking, pero dado que no lo es materializar un tipo que se sabe que el Administrador de estado de objeto, a continuación, el resultado materializado no realiza el seguimiento.

### <a name="63-------entity-sql-over-an-objectquery"></a>6.3 entity SQL a través de un elemento ObjectQuery

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**Profesionales**

-   Adecuado para las operaciones CUD.
-   Objetos materializados completamente.
-   Admite el almacenamiento en caché de plan de consulta.

**Desventajas**

-   Incluye cadenas de consulta textual que son más propensas a errores del usuario que las construcciones de consulta integradas en el lenguaje.

### <a name="64-------entity-sql-over-an-entity-command"></a>6.4 entity SQL a través de un comando de entidad

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

**Profesionales**

-   Admite el almacenamiento en caché de plan en .NET Framework 4.0 (almacenamiento en caché de plan es compatible con todos los demás tipos de consulta en .NET 4.5) de consulta.

**Desventajas**

-   Incluye cadenas de consulta textual que son más propensas a errores del usuario que las construcciones de consulta integradas en el lenguaje.
-   No es adecuado para las operaciones CUD.
-   Los resultados no se materializan automáticamente y se deben leer del lector de datos.

### <a name="65-------sqlquery-and-executestorequery"></a>6.5 SqlQuery y ExecuteStoreQuery

Consulta SQL en la base de datos:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

SqlQuery en DbSet:

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

ExecyteStoreQuery:

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

**Profesionales**

-   Por lo general rendimiento más rápido porque el compilador de plan se omite.
-   Objetos materializados completamente.
-   Adecuado para las operaciones CUD cuando se usa en la clase DbSet.

**Desventajas**

-   La consulta es textual y propenso a errores.
-   Consulta está asociada a un back-end específico mediante la semántica del almacén en lugar de semántica conceptual.
-   Cuando está presente la herencia, consultas artesanales deben tener en cuenta las condiciones de asignación para el tipo solicitado.

### <a name="66-------compiledquery"></a>6.6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**Profesionales**

-   Proporciona hasta una mejora del rendimiento de 7% con respecto a las consultas LINQ normales.
-   Objetos materializados completamente.
-   Adecuado para las operaciones CUD.

**Desventajas**

-   Mayor complejidad y sobrecarga de programación.
-   La mejora del rendimiento se pierde al redactar encima de una consulta compilada.
-   Algunas consultas LINQ no se puede escribir como un CompiledQuery - por ejemplo, las proyecciones de tipos anónimos.

### <a name="67-------performance-comparison-of-different-query-options"></a>6.7 comparación de rendimiento de opciones de consulta diferentes

Microbenchmarks simple donde no se programó la creación del contexto se ponga a prueba. Hemos medido consultas 5.000 veces superior para un conjunto de entidades sin almacenamiento en caché en un entorno controlado. Estos números son que se debe realizar con una advertencia: no reflejan los números reales generados por una aplicación, pero en su lugar, son una medida muy precisa de cuánto hay cuando se comparan las opciones de consultas diferentes una diferencia de rendimiento Apples-a-manzanas, sin incluir el costo de crear un nuevo contexto.

| EF  | Prueba                                 | Tiempo (ms) | Memoria   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ESQL ObjectContext                   | 2414      | 38801408 |
| EF5 | Consulta de Linq de ObjectContext             | 2692      | 38277120 |
| EF5 | Ningún seguimiento de consultas de DbContext Linq     | 2818      | 41840640 |
| EF5 | Consulta de Linq de DbContext                 | 2930      | 41771008 |
| EF5 | Ningún seguimiento de consultas de ObjectContext Linq | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ESQL ObjectContext                   | 2059      | 46039040 |
| EF6 | Consulta de Linq de ObjectContext             | 3074      | 45248512 |
| EF6 | Ningún seguimiento de consultas de DbContext Linq     | 3125      | 47575040 |
| EF6 | Consulta de Linq de DbContext                 | 3420      | 47652864 |
| EF6 | Ningún seguimiento de consultas de ObjectContext Linq | 3593      | 45260800 |

![Pruebas comparativas microcargas de EF5, 5000 iteraciones semiactivos](~/ef6/media/ef5micro5000warm.png)

![EF6 micro las pruebas comparativas, 5000 iteraciones semiactivos](~/ef6/media/ef6micro5000warm.png)

Microbenchmarks son muy sensibles a pequeños cambios en el código. En este caso, la diferencia entre los costos de Entity Framework 5 y Entity Framework 6 son debido a la adición de [intercepción](~/ef6/fundamentals/logging-and-interception.md) y [mejoras transaccionales](~/ef6/saving/transactions.md). Estos números microbenchmarks, sin embargo, son una visión amplificada en un fragmento muy pequeño de lo que hace Entity Framework. Escenarios del mundo real de las consultas calientes no deberían ver una regresión del rendimiento al actualizar desde Entity Framework 5 a Entity Framework 6.

Para comparar el rendimiento real de las opciones de consulta diferentes, creamos 5 variaciones de prueba independiente donde se utilice una opción de consulta diferentes para seleccionar todos los productos cuyo nombre de categoría es "Bebidas". Cada iteración incluye el costo de crear el contexto y el costo de materializar las entidades devueltas todo. 10 iteraciones se ejecutan untimed antes de realizar la suma de iteraciones ha superado el tiempo de 1000. Los resultados mostrados son la mediana ejecución tomada de 5 ejecuciones de cada prueba. Para obtener más información, consulte el apéndice B, que incluye el código de la prueba.

| EF  | Prueba                                        | Tiempo (ms) | Memoria   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | Comando de la entidad de ObjectContext                | 621       | 39350272 |
| EF5 | Consulta Sql de DbContext en la base de datos             | 825       | 37519360 |
| EF5 | Query Store ObjectContext                   | 878       | 39460864 |
| EF5 | Ningún seguimiento de consultas de ObjectContext Linq        | 969       | 38293504 |
| EF5 | Mediante la consulta de objeto ObjectContext Entity Sql | 1089      | 38981632 |
| EF5 | Consulta compilada de ObjectContext                | 1099      | 38682624 |
| EF5 | Consulta de Linq de ObjectContext                    | 1152      | 38178816 |
| EF5 | Ningún seguimiento de consultas de DbContext Linq            | 1208      | 41803776 |
| EF5 | Consulta Sql de DbContext en DbSet                | 1414      | 37982208 |
| EF5 | Consulta de Linq de DbContext                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | Comando de la entidad de ObjectContext                | 480       | 47247360 |
| EF6 | Query Store ObjectContext                   | 493       | 46739456 |
| EF6 | Consulta Sql de DbContext en la base de datos             | 614       | 41607168 |
| EF6 | Ningún seguimiento de consultas de ObjectContext Linq        | 684       | 46333952 |
| EF6 | Mediante la consulta de objeto ObjectContext Entity Sql | 767       | 48865280 |
| EF6 | Consulta compilada de ObjectContext                | 788       | 48467968 |
| EF6 | Ningún seguimiento de consultas de DbContext Linq            | 878       | 47554560 |
| EF6 | Consulta de Linq de ObjectContext                    | 953       | 47632384 |
| EF6 | Consulta Sql de DbContext en DbSet                | 1023      | 41992192 |
| EF6 | Consulta de Linq de DbContext                        | 1290      | 47529984 |


![Iteraciones de consulta activa 1000 EF5](~/ef6/media/ef5warmquery1000.png)

![Iteraciones de consulta activa 1000 EF6](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> Por integridad, hemos incluido una variación donde se ejecuta una consulta de Entity SQL en un EntityCommand. Sin embargo, dado que no se materializan los resultados de este tipo de consultas, la comparación no es necesariamente manzanas a manzanas. La prueba incluye una buena aproximación al materializar intentar realizar la comparación más justa.

En este caso to-end, Entity Framework 6 supera a Entity Framework 5 debido a las mejoras de rendimiento realizadas en varias partes de la pila, incluida una inicialización mucho más ligera de DbContext y más rápido MetadataCollection&lt;T&gt; búsquedas.

## <a name="7-design-time-performance-considerations"></a>7 Consideraciones de rendimiento de tiempo de diseño

### <a name="71-------inheritance-strategies"></a>7.1 estrategias de herencia

Otra consideración de rendimiento al usar Entity Framework es la estrategia de herencia que se utiliza. Entity Framework admite 3 tipos básicos de herencia y sus combinaciones:

-   Tabla por jerarquía (TPH), donde cada herencia establecido se asigna a una tabla con una columna discriminadora para indicar qué tipo concreto de la jerarquía se va a representar en la fila.
-   Tabla por tipo (TPT), donde cada tipo tiene su propia tabla de la base de datos; las tablas secundarias solo definen las columnas que no contiene la tabla primaria.
-   Tabla por clase (TPC), donde cada tipo tiene su propia tabla completa en la base de datos; las tablas secundarias definen sus campos, los definidos en los tipos de elemento primario incluidos.

Si el modelo usa la herencia de TPT, las consultas que se generan serán más complejas que los que se generan con las otras estrategias de herencia, lo que pueden resultar en tiempos de ejecución en el almacén.  Generalmente tarda más tiempo para generar consultas a través de un modelo TPT y para materializar los objetos resultantes.

Vea las "Consideraciones de rendimiento al usar la herencia de TPT (tabla por tipo) en Entity Framework" entrada de blog MSDN: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>.

#### <a name="711-------avoiding-tpt-in-model-first-or-code-first-applications"></a>7.1.1 evitar TPT en aplicaciones de Model First o Code First

Cuando se crea un modelo a través de una base de datos existente que tiene un esquema TPT, no tiene muchas opciones. Pero, al crear una aplicación mediante Model First o Code First, debe evitar la herencia de TPT para problemas de rendimiento.

Al usar Model First en el Asistente de Entity Designer, obtendrá TPT para cualquier herencia en el modelo. Si desea cambiar a una estrategia de herencia de TPH con Model First, puede usar el "Entity Designer Database Generation Power Pack" disponible desde la Galería de Visual Studio ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>).

Al usar Code First para configurar la asignación de un modelo con herencia, EF la usará TPH de manera predeterminada, por lo tanto, todas las entidades de la jerarquía de herencia se asignará a la misma tabla. Consulte la sección "Asignar con la API Fluent" del artículo "La código primero en entidad Framework4.1" en MSDN Magazine ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) para obtener más detalles.

### <a name="72-------upgrading-from-ef4-to-improve-model-generation-time"></a>7.2 actualización de EF4 a mejorar la generación del modelo de tiempo

Una mejora específica de SQL Server para el algoritmo que genera la capa de almacenamiento (SSDL) del modelo está disponible en Entity Framework 5 y 6 y como una actualización de Entity Framework 4, cuando se instala Visual Studio 2010 SP1. Los resultados de pruebas siguientes muestran la mejora al generar un modelo muy grande, en este caso, el modelo de Navision. Consulte el apéndice C para obtener más detalles sobre él.

El modelo contiene conjuntos de entidades 1005 y 4227 conjuntos de asociaciones.

| Configuración                              | Desglose de tiempo consumido                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010, Entity Framework 4     | Generación de SSDL: 2 horas 27 min. <br/> Generación de asignación: 1 segundo <br/> Generación de CSDL: 1 segundo <br/> Generación de ObjectLayer: 1 segundo <br/> Generación de vistas: 2 h 14 minutos |
| Visual Studio 2010 SP1, Entity Framework 4 | Generación de SSDL: 1 segundo <br/> Generación de asignación: 1 segundo <br/> Generación de CSDL: 1 segundo <br/> Generación de ObjectLayer: 1 segundo <br/> Generación de vistas: 1 hora 53 min.   |
| Visual Studio 2013, Entity Framework 5     | Generación de SSDL: 1 segundo <br/> Generación de asignación: 1 segundo <br/> Generación de CSDL: 1 segundo <br/> Generación de ObjectLayer: 1 segundo <br/> Generación de vistas: 65 minutos    |
| Visual Studio 2013, Entity Framework 6     | Generación de SSDL: 1 segundo <br/> Generación de asignación: 1 segundo <br/> Generación de CSDL: 1 segundo <br/> Generación de ObjectLayer: 1 segundo <br/> Generación de vistas: 28 segundos.   |


Es importante destacar que cuando se genera el SSDL, la carga se casi por completo dedica a SQL Server, mientras se espera a que el equipo de desarrollo de cliente inactivo que proceden del servidor de resultados. Los DBA deberían apreciarán especialmente esta mejora. También merece la pena tener en cuenta que básicamente el coste íntegro de generación de modelos tiene lugar en la generación de vistas de ahora.

### <a name="73-------splitting-large-models-with-database-first-and-model-first"></a>7.3 dividir grandes modelos con la base de datos en primer lugar y Model First

A medida que aumenta el tamaño del modelo, la superficie del diseñador se convierte en confuso y difícil de usar. Normalmente consideramos un modelo con más de 300 entidades sea demasiado grande para utilizar el Diseñador de forma efectiva. Entrada de blog siguiente describe varias opciones para dividir los modelos grandes: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>.

La publicación se ha escrito para la primera versión de Entity Framework, pero los pasos se siguen aplican.

### <a name="74-------performance-considerations-with-the-entity-data-source-control"></a>7.4 consideraciones de rendimiento de con el Control de origen de datos de entidad

Hemos visto casos de pruebas de esfuerzo y rendimiento multiproceso donde se deteriora considerablemente el rendimiento de una aplicación web mediante el EntityDataSource Control. La causa subyacente es que EntityDataSource llama repetidamente a MetadataWorkspace.LoadFromAssembly en los ensamblados que se hace referencia la aplicación Web para detectar los tipos que se usará como entidades.

La solución es establecer el ContextTypeName de EntityDataSource en el nombre de tipo de la clase derivada de ObjectContext. Esta opción desactiva el mecanismo que examina todos los ensamblados que se hace referencia para tipos de entidad.

Establecer el campo ContextTypeName también evita que un problema funcional donde EntityDataSource en .NET Framework 4.0 produce una ReflectionTypeLoadException cuando un tipo no puede cargar desde un ensamblado mediante reflexión. Este problema se corrigió en .NET 4.5.

### <a name="75-------poco-entities-and-change-tracking-proxies"></a>7.5 entidades POCO y proxies de seguimiento de cambios

Entity Framework permite usar clases de datos personalizadas junto con el modelo de datos sin realizar ninguna modificación en las clases de datos. Esto significa que podrá utilizar objetos CLR "antiguos" (POCO), tales como objetos de dominio existentes, con el modelo de datos. Estas clases de datos POCO (también conocido como objetos que ignoran la persistencia), que se asignan a entidades que se definen en un modelo de datos, admiten la mayoría de la misma consulta, insertarán, actualizarán y eliminación comportamientos como tipos de entidad generados por las herramientas de Entity Data Model.

Entity Framework también puede crear clases de proxy que deriva de los tipos POCO, que se usan cuando desea habilitar características como la carga diferida y automática seguimiento de cambios en entidades POCO. Las clases POCO deben cumplir ciertos requisitos para permitir que Entity Framework utilizar servidores proxy, como se describe aquí: [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx).

Los proxys de seguimiento de oportunidad notificará el Administrador de estado del objeto cada vez que cualquiera de las propiedades de las entidades tiene su valor puede cambiar, por lo que Entity Framework conoce el estado real de las entidades de todo el tiempo. Esto se hace agregando eventos de notificación en el cuerpo de los métodos establecedores de las propiedades, y que el Administrador de estado de objeto procesamiento dichos eventos. Tenga en cuenta que la creación de un proxy entidad tendrán normalmente es más costoso que crear una entidad POCO no proxy debido al conjunto de eventos creadas por Entity Framework se ha agregado.

Cuando una entidad POCO no tiene un proxy de seguimiento de cambios, se detectan cambios, comparando el contenido de las entidades con una copia de un estado guardado anterior. Esta comparación profunda se convertirá en un proceso largo cuando haya muchas entidades en el contexto, o cuando las entidades tienen una gran cantidad de propiedades, aunque ninguno de ellos cambiado desde la última comparación tuvo lugar.

En resumen: deberá pagar un impacto al crear el proxy de seguimiento de cambios en el rendimiento, pero el seguimiento de cambios le ayudarán a acelerar el proceso de detección de cambios cuando las entidades tienen muchas propiedades o cuando se tienen muchas entidades en el modelo. Para las entidades con un pequeño número de propiedades donde la cantidad de entidades no aumente demasiado, no puede ser tener objetos proxy de seguimiento de cambios de muchas ventajas.

## <a name="8-loading-related-entities"></a>Las entidades relacionadas con la carga de 8

### <a name="81-lazy-loading-vs-eager-loading"></a>8.1 frente a la carga diferida Carga diligente

Entity Framework ofrece varias formas de cargar las entidades que están relacionados con la entidad de destino. Por ejemplo, al consultar los productos, hay diferentes maneras de que se cargarán los pedidos relacionados en el Administrador de estado de objeto. Desde la perspectiva del rendimiento, la cuestión más importante a tener en cuenta al cargar las entidades relacionadas será si se debe usar la carga diferida o la carga diligente.

Cuando se usa la carga diligente, las entidades relacionadas se cargan junto con su conjunto de entidades de destino. Utilice una instrucción de inclusión en la consulta para indicar que relacionados con las entidades que desea incorporar.

Cuando se usa la carga diferida, la consulta inicial solo vuelve a colocar en el conjunto de entidades de destino. Pero siempre que tenga acceso a una propiedad de navegación, se emite otra consulta en el almacén para cargar la entidad relacionada.

Una vez que se ha cargado una entidad, cualquier otra consulta para la entidad cargará directamente desde el Administrador de estado de objeto, si está utilizando la carga diferida o la carga diligente.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8.2 cómo elegir entre la carga diferida y la carga diligente

Lo importante es comprender la diferencia entre la carga diferida y la carga diligente para que pueda tomar la opción correcta para su aplicación. Esto ayudará a evaluar el equilibrio entre varias solicitudes en la base de datos frente a una solicitud única que puede contener una carga grande. Puede ser adecuado utilizar la carga diligente en algunas partes de la aplicación y la carga diferida en otras partes.

Como ejemplo de lo que sucede en segundo plano, suponga que desea consultar para los clientes que viven en el Reino Unido y el número de pedido.

**Mediante la carga diligente**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**Uso de la carga diferida**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

Cuando se usa la carga diligente, deberá emitir una única consulta que devuelve todos los clientes y todos los pedidos. El comando de almacén tiene el siguiente aspecto:

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

Cuando se usa la carga diferida, deberá emitir la siguiente consulta inicialmente:

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

Y cada vez que utilice la propiedad de navegación de pedidos de un cliente se emite otra consulta similar al siguiente en el almacén:

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

Para obtener más información, consulte el [cargar objetos relacionados](https://msdn.microsoft.com/library/bb896272.aspx).

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 la carga diferida frente a la hoja de referencia de la carga diligente

No hay nada como una única a elegir la carga diligente frente a la carga diferida. Pruebe primero comprender las diferencias entre ambas estrategias de manera que pueda hacer una decisión informada bien; Además, tenga en cuenta si el código se ajusta a cualquiera de los siguientes escenarios:

| Escenario                                                                    | Nuestra sugerencia                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ¿Necesita tener acceso a muchas de las propiedades de navegación de las entidades recuperadas? | **No** -probablemente hará ambas opciones. Sin embargo, si la carga que aporta la consulta no es demasiado grande, que puede experimentar las ventajas de rendimiento mediante el uso de la carga diligente como requerirá menos recorridos de ida y para materializar los objetos de red. <br/> <br/> **Sí** -si necesita tener acceso a muchas de las propiedades de navegación de las entidades, haría que utilizando varias incluir instrucciones en la consulta con la carga diligente. Las entidades más incluye, mayor es la carga de la consulta va a devolver. Una vez que se incluyen tres o más entidades en la consulta, considere la posibilidad de Lazy cargando. |
| ¿Sabe exactamente qué datos se necesitará en tiempo de ejecución?                   | **No** -carga diferida será mejor para usted. En caso contrario, puede acabar consultar los datos que no necesitará. <br/> <br/> **Sí** : diligente, la carga es probablemente la mejor opción; le ayudarán a conjuntos completos de carga con mayor rapidez. Si la consulta requiere la obtención de una gran cantidad de datos, y esto se convierte en demasiado lento, vuelva a intentar Lazy cargar en su lugar.                                                                                                                                                                                                                                                       |
| ¿Se está ejecutando el código lejos de la base de datos? (mayor latencia de red)  | **No** : cuando la latencia de red no es un problema, utilizando la carga diferida puede simplificar el código. Recuerde que puede cambiar la topología de la aplicación, por lo que no toman la proximidad de la base de datos por sentado. <br/> <br/> **Sí** : cuando la red es un problema, solo puede decidir lo que se adapta mejor para su escenario. Normalmente la carga diligente será mejor porque requiere menos ida y vuelta.                                                                                                                                                                                                      |


#### <a name="822-------performance-concerns-with-multiple-includes"></a>8.2.2 problemas de rendimiento de with incluye varios

Cuando recibimos preguntas de rendimiento que implican problemas de tiempo de respuesta de servidor, el origen del problema es con frecuencia consultas con varias instrucciones de inclusión. Aunque es eficaz incluidas entidades relacionadas en una consulta, es importante entender lo que sucede en segundo plano.

Tarda un tiempo relativamente largo para una consulta con varias instrucciones Include en él para ir a través de nuestro compilador plan interno para generar el comando de almacén. La mayoría de este tiempo se dedica a tratar de optimizar la consulta resultante. El comando generado contendrá una Outer Join o Union para incluir cada, dependiendo de su asignación. Consultas similares a esto llevará de gráficos conectados de gran tamaño desde la base de datos en una única carga, que se acerbate los problemas de ancho de banda, especialmente cuando hay una gran cantidad de redundancia en la carga (por ejemplo, cuando se utilizan varios niveles de inclusión para atravesar asociaciones en la dirección de uno a varios).

Puede buscar los casos donde las consultas devuelven excesivamente grandes cargas mediante el acceso a la TSQL subyacente para la consulta mediante ToTraceString y ejecutar el comando de almacén de SQL Server Management Studio para ver el tamaño de carga. En tales casos que puede intentar reducir el número de instrucciones Include en la consulta simplemente poner en los datos que necesita. O bien, es posible que pueda dividir la consulta en una secuencia más pequeña de subconsultas, por ejemplo:

**Antes de interrumpir la consulta:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

**Después de interrumpir la consulta:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

Esto funcionará sólo en las consultas de seguimiento, como vamos a hacer que el uso de la capacidad que tiene el contexto para realizar la corrección de asociación y la resolución de identidad automáticamente.

Al igual que con la carga diferida, el equilibrio será más consultas para las cargas más pequeñas. También puede usar las proyecciones de las propiedades individuales para seleccionar explícitamente sólo los datos que necesita de cada entidad, pero, no se carga entidades en este caso, y no se admitirán actualizaciones.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3 solución alternativa para obtener la carga diferida de propiedades

Entity Framework actualmente no admite la carga diferida de propiedades escalares o complejas. Sin embargo, en casos donde haya una tabla que incluye un objeto grande, como un BLOB, puede usar la división de tablas para separar las propiedades de gran tamaño en una entidad independiente. Por ejemplo, suponga que tiene una tabla Product que incluya una columna de la foto varbinary. Si con frecuencia no es necesario tener acceso a esta propiedad en las consultas, puede usar para mostrar solo las partes de la entidad que normalmente necesita la división de tabla. La entidad que representa la fotografía del producto sólo se cargarán cuando necesite explícitamente.

Un buen recurso que se muestra cómo habilitar la división de tablas es "Tabla dividir en Entity Framework" entrada de blog de Gil Fink: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>.

## <a name="9-other-considerations"></a>9 otras consideraciones

### <a name="91------server-garbage-collection"></a>9.1 recolección de elementos no utilizados de servidor de

Algunos usuarios podrían experimentar contención de recursos que limita el paralelismo que se espera cuando el recolector de elementos no utilizados no está configurado correctamente. Cada vez que EF se usa en un escenario con varios subprocesos, o en cualquier aplicación que se parece a un sistema de servidor, asegúrese de habilitar la recolección de elementos no utilizados de servidor. Esto se realiza a través de una configuración simple en el archivo de configuración de aplicación:

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

Esto debería reducir la contención de subprocesos y aumentar su rendimiento en hasta un 30% en los escenarios de CPU saturado. En términos generales, siempre debe probar cómo comporta la aplicación mediante la colección de elementos no utilizados clásico (que se ajusta mejor para escenarios de lado cliente y la interfaz de usuario), así como la colección de elementos no utilizados de servidor.

### <a name="92------autodetectchanges"></a>9.2 AutoDetectChanges

Como se mencionó anteriormente, Entity Framework podría mostrar problemas de rendimiento cuando la caché de objetos tiene muchas entidades. Ciertas operaciones, como agregar, quitar, buscar, entrada y SaveChanges, desencadenan llamadas a DetectChanges que puede consumir una gran cantidad de CPU en función de tamaño se ha vuelto la caché de objetos. El motivo es que la caché de objetos e intente de administrador de estado de objeto para permanecer como sincronizan como sea posible en cada operación realizada en un contexto para que se garantiza que los datos producidos sea correcta en una amplia gama de escenarios.

Suele ser una buena idea dejar la detección de cambios automática de Entity Framework habilitada para toda la vida de la aplicación. Si su escenario es que se ve afectado negativamente por el uso elevado de CPU y los perfiles indican que el culpable es la llamada a DetectChanges, considere la posibilidad de desactivar temporalmente AutoDetectChanges en la parte del código confidencial:

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

Antes de desactivar AutoDetectChanges, es conveniente entender que esto puede provocar que Entity Framework a perder su habilidad para realizar un seguimiento de cierta información sobre los cambios que están teniendo lugar en las entidades. Si se controlan correctamente, esto podría causar incoherencia de datos en la aplicación. Para obtener más información sobre cómo desactivar AutoDetectChanges, lea \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>.

### <a name="93------context-per-request"></a>9.3 contexto por solicitud

Contextos de Entity Framework están diseñadas para usarse como instancias de corta duración para proporcionar un rendimiento óptimo con más experimentan. Contextos se esperan que sea más corta duración y se descartan y por lo tanto se han implementado para que sea muy ligero y reutilize metadatos siempre que sea posible. En escenarios de web es importante tener esto en mente y no tiene un contexto para la mayor que la duración de una única solicitud. De forma similar, en escenarios que no son web, contexto debe descartarse según su comprensión de los distintos niveles de almacenamiento en caché en Entity Framework. Por lo general, se debería evitar tener una instancia de contexto durante la vida de la aplicación, así como contextos de subprocesos y contextos estáticos.

### <a name="94------database-null-semantics"></a>9.4 semántica de null de base de datos

Entity Framework de forma predeterminada generará código SQL que tiene C\# null semántica de comparación. Tenga en cuenta la siguiente consulta de ejemplo:

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

En este ejemplo, nos estamos comparando una serie de variables que aceptan valores NULL en las propiedades que aceptan valores NULL en la entidad, como SupplierID y UnitPrice. El código SQL generado para esta consulta le preguntará si el valor del parámetro es el mismo que el valor de columna, o si el parámetro y los valores de columna son nulos. Esto ocultará la manera en que el servidor de base de datos controla los valores NULL y le proporcionará una C coherente\# null experiencia a través de proveedores de base de datos diferente. Por otro lado, el código generado es un poco complicado y no se puede realizar bien cuando la cantidad de comparaciones en el lugar en la instrucción de la consulta que crece a un gran número.

Una manera de tratar esta situación es mediante la semántica de null de base de datos. Tenga en cuenta que esto podría potencialmente comportarse de manera diferente a la C\# null semántica desde ahora Entity Framework generará código SQL más sencillo que expone la manera en que el motor de base de datos controla los valores null. Semántica de null de base de datos puede ser activado por contexto con una línea de configuración único frente a la configuración de contexto:

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

Pequeña a mediana con tamaño de las consultas no mostrarán una mejora del rendimiento perceptible cuando se usa la semántica de null de base de datos, pero la diferencia será más evidente en las consultas con un gran número de posibles comparaciones de null.

En la consulta del ejemplo anterior, la diferencia de rendimiento era menor que 2% en un microbenchmark que se ejecuta en un entorno controlado.

### <a name="95------async"></a>9.5 Async

Entity Framework 6 introdujo compatibilidad con las operaciones asincrónicas cuando se ejecuta en .NET 4.5 o versiones posteriores. En su mayor parte, las aplicaciones que tienen E/S relacionados con la contención se beneficiarán el máximo partido de la consulta asincrónica de uso y las operaciones de guardado. Si la aplicación no sufren la contención de E/S, el uso de async se, en el mejor de los casos, se ejecutan sincrónicamente y devuelven el resultado en la misma cantidad de tiempo que una llamada sincrónica, o en el peor de los casos, simplemente aplazar la ejecución de una tarea asincrónica y agregue a tim adicional e a la finalización de su escenario.

Para obtener información sobre el trabajo de programación asincrónico que le ayudarán a decidir si async mejorará el rendimiento de la aplicación visita [http://msdn.microsoft.com/library/hh191443.aspx](https://msdn.microsoft.com/library/hh191443.aspx). Para obtener más información sobre el uso de operaciones asincrónicas en Entity Framework, vea [consulta asincrónica y guardar](~/ef6/fundamentals/async.md
).

### <a name="96------ngen"></a>9.6 NGEN

Entity Framework 6 no se incluye en la instalación predeterminada de .NET framework. Por lo tanto, los ensamblados de Entity Framework no están que Ngen de forma predeterminada, lo que significa que todo el código de Entity Framework está sujeto a los mismos costes JIT'ing como cualquier otro ensamblado MSIL. Esto podría degradar la experiencia de F5 al desarrollar y también el inicio en frío de la aplicación en los entornos de producción. Con el fin de reducir los costos de CPU y memoria de JIT'ing es aconsejable NGEN Entity Framework imágenes según corresponda. Para obtener más información sobre cómo mejorar el rendimiento de inicio de Entity Framework 6 con NGEN, consulte [mejorar el rendimiento de inicio con NGen](~/ef6/fundamentals/performance/ngen.md).

### <a name="97------code-first-versus-edmx"></a>9.7 code First y EDMX

Motivos de Entity Framework acerca del problema de falta de coincidencia de impedancia entre la programación orientada a objetos y bases de datos relacionales al tener una representación en memoria del modelo conceptual (objetos), el esquema de almacenamiento (base de datos) y una asignación entre la dos. Estos metadatos se denominan un Entity Data Model o EDM para abreviar. De este tipo de EDM, Entity Framework se derivan de las vistas a los datos de ida y vuelta de los objetos en memoria a la base de datos y realizar una copia.

Cuando Entity Framework se usa con un archivo EDMX formalmente que especifica el modelo conceptual, el esquema de almacenamiento y la asignación y, después, la fase de carga del modelo solo tiene que validar que el EDM es correcto (por ejemplo, asegúrese de que no faltan asignaciones), a continuación, generar las vistas, validar las vistas y estos metadatos para su uso. Solo puede, a continuación, una consulta se ejecuta o se puede guardar nuevos datos al almacén de datos.

En esencia, el enfoque Code First es un generador de Entity Data Model sofisticado. Entity Framework debe generar un EDM desde el código proporcionado; lo hace analizar las clases implicadas en el modelo, aplicar las convenciones y configuración del modelo mediante la API Fluent. Una vez compilado el EDM, Entity Framework básicamente se comporta igual manera que haría tenía un archivo EDMX estado presente en el proyecto. Por lo tanto, la creación del modelo de Code First agrega complejidad adicional que se traduce en un tiempo de inicio más lento para Entity Framework cuando se comparan para tener un EDMX. El costo depende completamente del tamaño y complejidad del modelo que se está compilando.

Al elegir la opción Usar EDMX frente a Code First, es importante saber que la flexibilidad introducida por Code First aumenta el costo de generar el modelo por primera vez. Si la aplicación puede resistir el costo de esta carga por primera vez, a continuación, normalmente Code First será la mejor manera de ir.

## <a name="10-investigating-performance"></a>Rendimiento de investigación de 10

### <a name="101-using-the-visual-studio-profiler"></a>10.1 utilizando al Profiler de Visual Studio

Si tiene problemas de rendimiento con Entity Framework, puede usar un generador de perfiles como el integrado en Visual Studio para ver dónde la aplicación dedica el tiempo. Esta es la herramienta que se usa para generar los gráficos circulares en la entrada de blog "Explorar el rendimiento de ADO.NET Entity Framework - parte 1" ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) que muestre en Entity Framework emplea tiempo durante las consultas en frío y calientes.

La entrada de blog "Generación de perfiles de Entity Framework mediante el Profiler de 2010 Visual Studio" escrita por los datos y modelado de Customer Advisory Team muestra un ejemplo de cómo usa el generador de perfiles para investigar un problema de rendimiento del mundo real.  \<http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>. Este artículo se escribió para una aplicación de windows. Si necesita un perfil de una aplicación web de las herramientas de Windows Performance Recorder (WPR) y Windows Performance Analyzer (WPA) funcionen mejor que el trabajo desde Visual Studio. WPR y WPA son parte del Kit de herramientas de rendimiento de Windows que se incluye con el Windows Assessment and Deployment Kit ( [http://www.microsoft.com/en-US/download/details.aspx?id=39982 ](https://www.microsoft.com/en-US/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>10.2 aplicación/base de datos de generación de perfiles

Herramientas como el generador de perfiles integrado en Visual Studio indican dónde dedica tiempo la aplicación.  Hay otro tipo de generador de perfiles que realiza el análisis dinámico de la aplicación en ejecución, en producción o preproducción según las necesidades y busca los riesgos comunes y antipatrones de acceso de la base de datos.

Dos generadores de perfiles comercialmente disponibles son el Profiler de Entity Framework ( \<http://efprof.com>) y ORMProfiler ( \<http://ormprofiler.com>).

Si la aplicación es una aplicación MVC mediante Code First, puede usar MiniProfiler de StackExchange. Scott Hanselman describe esta herramienta en su blog en: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>.

Para obtener más información sobre la generación de perfiles de actividad de la base de datos de la aplicación, consulte artículo de MSDN Magazine de Julie titulada [actividad de base de datos de generación de perfiles en Entity Framework](https://msdn.microsoft.com/magazine/gg490349.aspx).

### <a name="103-database-logger"></a>10.3 registrador de la base de datos

Si usa Entity Framework 6 también considere el uso de la funcionalidad de registro integrados. La propiedad de base de datos del contexto puede indicará que su actividad a través de una configuración simple de una línea de registro:

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

En este ejemplo se registrará la actividad de la base de datos en la consola, pero se puede configurar la propiedad del registro para llamar a cualquier acción&lt;cadena&gt; delegar.

Si desea habilitar el registro de base de datos sin tener que volver a compilar y está utilizando Entity Framework 6.1 o posterior, puede hacerlo mediante la adición de un interceptor en el archivo web.config o app.config de la aplicación.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

Para obtener más información sobre cómo agregar un registro sin volver a compilar vaya a \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>.

## <a name="11-appendix"></a>Apéndice 11

### <a name="111-a-test-environment"></a>11.1 entorno de prueba de r de.

Este entorno usa una configuración de máquina de 2 con la base de datos en un equipo independiente de la aplicación cliente. Las máquinas están en el mismo bastidor, por lo que la latencia de red es relativamente bajo, pero más realista de una sola máquina del entorno.

#### <a name="1111-------app-server"></a>11.1.1 servidor de aplicaciones

##### <a name="11111------software-environment"></a>11.1.1.1 entorno de software

-   Entorno de Software de Entity Framework 4
    -   Nombre del sistema operativo: Windows Server 2008 R2 SP1 Enterprise.
    -   Visual Studio 2010: Ultimate.
    -   Visual Studio 2010 SP1 (solo para algunos comparaciones).
-   Entorno de Software de Entity Framework 5 y 6
    -   Nombre del sistema operativo: Windows 8.1 Enterprise
    -   Visual Studio 2013: Ultimate.

##### <a name="11112------hardware-environment"></a>11.1.1.2 entorno de hardware

-   Procesador dual: W3530 de Intel (r) Xeon (r) CPU L5520 a 2,27 GHz, 2261 Mhz8 GHz, 4 núcleos, 84 procesadores lógicos.
-   RamRAM de 2412 GB.
-   136 GB SCSI250GB SATA 7200 rpm / 3GB/s unidad dividir en 4 particiones.

#### <a name="1112-------db-server"></a>11.1.2 servidor de base de datos

##### <a name="11121------software-environment"></a>11.1.2.1 entorno de software

-   Nombre del sistema operativo: Windows Server 2008 R28.1 Enterprise SP1.
-   SQL Server 2008 R22012.

##### <a name="11122------hardware-environment"></a>11.1.2.2 entorno de hardware

-   Único procesador: Intel (r) Xeon (r) CPU L5520 a 2,27 GHz, 2261 MhzES-1620 0 @ 3,60 GHz, 4 núcleos, 8 procesadores lógicos.
-   RamRAM de 824 GB.
-   465 GB ATA500GB SATA 7200 rpm 6GB/s unidad dividir en 4 particiones.

### <a name="112------b-query-performance-comparison-tests"></a>11.2 pruebas comparativas de rendimiento de consulta B.

El modelo de Northwind se usó para ejecutar estas pruebas. Se generó desde la base de datos mediante el Diseñador de Entity Framework. A continuación, el código siguiente se usó para comparar el rendimiento de las opciones de ejecución de consulta:

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a>C. 11.3 Navision modelo

La base de datos Navision es una gran base de datos usa para la demostración de Microsoft Dynamics: barra de navegación. El modelo conceptual generado contiene conjuntos de entidades 1005 y 4227 conjuntos de asociaciones. El modelo utilizado en la prueba no es "plano": se agregó ningún herencia a él.

#### <a name="1131-queries-used-for-navision-tests"></a>11.3.1 consultas que se usan para las pruebas de Navision

La lista de consultas que se usa con el modelo Navision contiene 3 categorías de las consultas de Entity SQL:

##### <a name="11311-lookup"></a>11.3.1.1 búsqueda de

Una consulta de búsqueda simple sin agregaciones

-   Recuento: 16232
-   Ejemplo:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312-singleaggregating"></a>11.3.1.2 SingleAggregating

Una consulta de BI normal con varias agregaciones, pero no los subtotales (consulta individual)

-   Recuento: 2313
-   Ejemplo:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

Donde MDF\_SessionLogin\_tiempo\_Max() se define en el modelo como:

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313-aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

Una consulta de BI con las agregaciones y subtotales (a través de la unión de todos)

-   Recuento: 178
-   Ejemplo:

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
