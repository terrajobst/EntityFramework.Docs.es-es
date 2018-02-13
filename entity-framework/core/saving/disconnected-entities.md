---
title: Entidades desconectadas - Core EF
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0b145217d40027c4b8e4746e9c5651652a28c9eb
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
---
# <a name="disconnected-entities"></a>Entidades desconectadas

Una instancia de DbContext realizará automáticamente el seguimiento entidades devueltas desde la base de datos. Los cambios realizados en estas entidades, a continuación, se detectará cuando se llama a SaveChanges y se actualizará la base de datos según sea necesario. Vea [guardar básica](basic.md) y [datos relacionados](related-data.md) para obtener más información.

Sin embargo, en ocasiones, las entidades se consultan utilizando una instancia de contexto y, a continuación, se guarda con una instancia diferente. Esto suele ocurrir en escenarios "desconectados" como una aplicación web, donde las entidades se consultar, envía al cliente, modificar, envía al servidor en una solicitud y, a continuación, se guarda. En este caso, el contexto de la segundo instancia debe saber si las entidades son nuevo (debe insertarse) o existente (deben actualizarse).

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) de este artículo en GitHub.

> [!TIP]
> EF Core solo puede realizar un seguimiento de una instancia de cualquier entidad con un determinado valor de clave principal. La mejor manera de evitar este un problema consiste en usar un contexto de corta duración para cada unidad de trabajo de forma que el contexto se inicia vacío, tiene entidades asociadas a él, guarda las entidades y, a continuación, en el contexto se elimina y se descarta.

## <a name="identifying-new-entities"></a>Identificar nuevas entidades

### <a name="client-identifies-new-entities"></a>Cliente identifica nuevas entidades

El caso más simple para tratar con es cuando el cliente informa al servidor si la entidad es nueva o existente. Por ejemplo, a menudo la solicitud para insertar una nueva entidad es diferente de la solicitud para actualizar una entidad existente.

El resto de esta sección trata los casos donde necesario determinar de alguna otra manera, si se debe insertar o actualizar.

### <a name="with-auto-generated-keys"></a>Con claves generadas automáticamente

El valor de una clave generada automáticamente a menudo puede utilizarse para determinar si una entidad debe insertar o actualizar. Si la clave no se ha establecido (es decir, todavía tiene el valor predeterminado CLR de null, cero, etc.), a continuación, la entidad debe ser nuevo y es necesario insertar. Por otro lado, si se ha establecido el valor de clave, a continuación, debe haber ya se ha guardado previamente y ahora necesita actualizar. En otras palabras, si la clave tiene un valor, entidad, a continuación, se consultó, envía al cliente y se vuelven a estar ahora actualizarse.

Resulta sencillo comprobar si una clave sin establecer cuando se conoce el tipo de entidad:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

Sin embargo, EF también tiene un medio integrado para hacer esto para cualquier tipo de entidad y tipo de clave:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Las claves se establecen en cuanto se realiza un seguimiento de las entidades según el contexto, incluso si la entidad está en el estado agregado. Esto resulta útil cuando atraviesa un gráfico de entidades y decidir qué hacer con cada, como cuando se usa la API de TrackGraph. Solo debe usarse el valor de clave de la manera en que se muestra aquí _antes de_ se realiza cualquier llamada para realizar el seguimiento de la entidad.

### <a name="with-other-keys"></a>Con otras teclas

Algún otro mecanismo es necesario para identificar entidades nuevas cuando los valores de clave no se generan automáticamente. Hay dos enfoques generales para esto:
 * Consulta para la entidad
 * Pase un indicador desde el cliente

Para consultar la entidad, use simplemente el método de búsqueda:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Está fuera del ámbito de este documento para mostrar el código completo para pasar una marca de un cliente. En una aplicación web, normalmente significa que se realizan solicitudes diferentes acciones diferentes, o pasar algún estado de la solicitud, a continuación, extraer en el controlador.

## <a name="saving-single-entities"></a>Guardar entidades únicas

Si se conoce como si no se necesita una inserción o actualización, a continuación, agregar o actualizar puede utilizarse correctamente:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Sin embargo, si la entidad utiliza valores de clave generada automáticamente, puede utilizar el método de actualización en ambos casos:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Normalmente, el método Update marca la entidad para la actualización, inserción no. Sin embargo, si la entidad tiene una clave generada automáticamente y no se ha establecido ningún valor de clave, a continuación, la entidad en su lugar, se marca automáticamente para insertar.

> [!TIP]  
> Este comportamiento se introdujo en EF Core 2.0. En las versiones anteriores siempre es necesario elegir explícitamente agregar o actualizar.

Si la entidad no está utilizando claves generadas automáticamente, la aplicación debe decidir si la entidad se debe insertar o actualizar: por ejemplo:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Son los siguientes pasos:
* Si agrega Find devuelve un valor null, a continuación, la base de datos ya no contiene el blog con este Id., por lo que llamamos a marcar para la inserción.
* Si la búsqueda devuelve una entidad, a continuación, se encuentra en la base de datos y el contexto ya está realizando el seguimiento de la entidad existente
  * A continuación, utilizamos SetValues para establecer los valores para todas las propiedades de esta entidad a los que procede el cliente.
  * La llamada SetValues marcará la entidad que se actualiza según sea necesario.

> [!TIP]  
> SetValues sólo se marcará como modificar las propiedades que tienen valores diferentes a los de la entidad que realiza un seguimiento. Esto significa que cuando se envía la actualización, se actualizarán sólo aquellas columnas que han cambiado realmente. (Y si no ha cambiado nada, se enviará ninguna actualización en absoluto).

## <a name="working-with-graphs"></a>Trabajar con gráficos

### <a name="identity-resolution"></a>Resolución de identidades

Como se mencionó anteriormente, EF Core solo puede realizar un seguimiento una instancia de cualquier entidad con un determinado valor de clave principal. Al trabajar con gráficos lo ideal es que se debe crear el gráfico de forma que se mantiene esta condición invariable, y se debe usar el contexto para solo una unidad de trabajo. Si el gráfico contiene duplicados, será necesario procesar el gráfico antes de enviarlo a EF consolidar varias instancias en una. Esto puede no ser trivial donde instancias tienen valores en conflicto y relaciones, por lo que deben hacerlo tan pronto como sea posible consolidar duplicados en la canalización de aplicación para evitar la resolución de conflictos.

### <a name="all-newall-existing-entities"></a>Todos los nuevos o todas las entidades existentes

Un ejemplo del uso de gráficos es insertar o actualizar un blog junto con su colección de entradas asociadas. Si todas las entidades en el gráfico deben insertarse o todos se deben actualizar, el proceso es el mismo tal y como se describió anteriormente para entidades únicas. Por ejemplo, un gráfico de blogs y anuncios creado similar al siguiente:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

se pueden insertar de forma similar al siguiente:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

La llamada a Add marcará el blog y todas las entradas va a insertar.

Asimismo, si todas las entidades en un gráfico necesitan actualizarse, puede usar actualización:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

El blog y todas sus entradas se marcarán para su actualización.

### <a name="mix-of-new-and-existing-entities"></a>Combinación de entidades nuevas y existentes

Con claves generadas automáticamente, actualización, vuelva a puede utilizarse para las inserciones y actualizaciones, incluso si el gráfico contiene una mezcla de entidades que requieren la inserción y aquellos que requieren actualización:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Actualización, se marcarán como una entidad en el gráfico, blog o post, de inserción si no tiene un conjunto de valores de clave, mientras que todas las demás entidades están marcados para la actualización.

Como antes, si no usa claves generadas automáticamente, una consulta y procesamiento se pueden usar:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Control de eliminaciones

Delete puede ser difícil de controlar desde a menudo la ausencia de una entidad significa que debe eliminarse. Una manera de solucionar esto es usar "eliminaciones de software" tal que la entidad se marca como eliminada en lugar de que se eliminan realmente. Elimina, a continuación, pasa a ser el mismo que las actualizaciones. Eliminaciones de software pueden implementarse de uso [filtros de consulta](xref:core/querying/filters).

Para las eliminaciones es true, un patrón común consiste en utilizar una extensión del patrón de consulta para realizar lo que es esencialmente una diferencia de gráfico. Por ejemplo:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Internamente, agregar, adjuntar y Update utilizan cruce seguro del gráfico con una resolución adoptada para cada entidad en cuanto a si se debería marcarse como Added (para insertar), Modified (para actualizar), sin cambios (no hacen nada), o eliminadas (para eliminar). Este mecanismo se expone a través de la API de TrackGraph. Por ejemplo, supongamos que, cuando el cliente envía un gráfico de entidades, establece algunas marca en cada entidad que indica cómo debe controlarse. TrackGraph, a continuación, puede utilizarse para procesar este indicador:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

Las marcas solo se muestran como parte de la entidad para simplificar el trabajo del ejemplo. Normalmente, las marcas sería parte de un DTO o algún otro Estado incluido en la solicitud.
