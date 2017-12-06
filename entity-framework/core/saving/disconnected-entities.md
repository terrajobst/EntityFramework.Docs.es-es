---
title: Entidades desconectadas - Core EF
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: b9d9662ce277e4f7b3d6f997a5117a0592f59fa3
ms.sourcegitcommit: c72d85805db0aa95f980514a18381fdc5e17c786
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2017
---
# <a name="disconnected-entities"></a><span data-ttu-id="55125-102">Entidades desconectadas</span><span class="sxs-lookup"><span data-stu-id="55125-102">Disconnected entities</span></span>

<span data-ttu-id="55125-103">Una instancia de DbContext realizará automáticamente el seguimiento entidades devueltas desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="55125-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="55125-104">Los cambios realizados en estas entidades, a continuación, se detectará cuando se llama a SaveChanges y se actualizará la base de datos según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="55125-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="55125-105">Vea [guardar básica](basic.md) y [datos relacionados](related-data.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="55125-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="55125-106">Sin embargo, en ocasiones, las entidades se consultan utilizando una instancia de contexto y, a continuación, se guarda con una instancia diferente.</span><span class="sxs-lookup"><span data-stu-id="55125-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="55125-107">Esto suele ocurrir en escenarios "desconectados" como una aplicación web, donde las entidades se consultar, envía al cliente, modificar, envía al servidor en una solicitud y, a continuación, se guarda.</span><span class="sxs-lookup"><span data-stu-id="55125-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="55125-108">En este caso, el contexto de la segundo instancia debe saber si las entidades son nuevo (debe insertarse) o existente (deben actualizarse).</span><span class="sxs-lookup"><span data-stu-id="55125-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="55125-109">Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="55125-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="55125-110">Identificar nuevas entidades</span><span class="sxs-lookup"><span data-stu-id="55125-110">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="55125-111">Cliente identifica nuevas entidades</span><span class="sxs-lookup"><span data-stu-id="55125-111">Client identifies new entities</span></span>

<span data-ttu-id="55125-112">El caso más simple para tratar con es cuando el cliente informa al servidor si la entidad es nueva o existente.</span><span class="sxs-lookup"><span data-stu-id="55125-112">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="55125-113">Por ejemplo, a menudo la solicitud para insertar una nueva entidad es diferente de la solicitud para actualizar una entidad existente.</span><span class="sxs-lookup"><span data-stu-id="55125-113">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="55125-114">El resto de esta sección trata los casos donde necesario determinar de alguna otra manera, si se debe insertar o actualizar.</span><span class="sxs-lookup"><span data-stu-id="55125-114">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="55125-115">Con claves generadas automáticamente</span><span class="sxs-lookup"><span data-stu-id="55125-115">With auto-generated keys</span></span>

<span data-ttu-id="55125-116">El valor de una clave generada automáticamente a menudo puede utilizarse para determinar si una entidad debe insertar o actualizar.</span><span class="sxs-lookup"><span data-stu-id="55125-116">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="55125-117">Si la clave no se ha establecido (es decir, todavía tiene el valor predeterminado CLR de null, cero, etc.), a continuación, la entidad debe ser nuevo y es necesario insertar.</span><span class="sxs-lookup"><span data-stu-id="55125-117">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="55125-118">Por otro lado, si se ha establecido el valor de clave, a continuación, debe haber ya se ha guardado previamente y ahora necesita actualizar.</span><span class="sxs-lookup"><span data-stu-id="55125-118">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="55125-119">En otras palabras, si la clave tiene un valor, entidad, a continuación, se consultó, envía al cliente y se vuelven a estar ahora actualizarse.</span><span class="sxs-lookup"><span data-stu-id="55125-119">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="55125-120">Resulta sencillo comprobar si una clave sin establecer cuando se conoce el tipo de entidad:</span><span class="sxs-lookup"><span data-stu-id="55125-120">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="55125-121">Sin embargo, EF también tiene un medio integrado para hacer esto para cualquier tipo de entidad y tipo de clave:</span><span class="sxs-lookup"><span data-stu-id="55125-121">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="55125-122">Las claves se establecen en cuanto se realiza un seguimiento de las entidades según el contexto, incluso si la entidad está en el estado agregado.</span><span class="sxs-lookup"><span data-stu-id="55125-122">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="55125-123">Esto resulta útil cuando atraviesa un gráfico de entidades y decidir qué hacer con cada, como cuando se usa la API de TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="55125-123">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="55125-124">Solo debe usarse el valor de clave de la manera en que se muestra aquí _antes de_ se realiza cualquier llamada para realizar el seguimiento de la entidad.</span><span class="sxs-lookup"><span data-stu-id="55125-124">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="55125-125">Con otras teclas</span><span class="sxs-lookup"><span data-stu-id="55125-125">With other keys</span></span>

<span data-ttu-id="55125-126">Algún otro mecanismo es necesario para las nuevas entidades de identidad cuando los valores de clave no se generan automáticamente.</span><span class="sxs-lookup"><span data-stu-id="55125-126">Some other mechanism is needed to identity new entities when key values are not generated automatically.</span></span> <span data-ttu-id="55125-127">Hay dos enfoques generales para esto:</span><span class="sxs-lookup"><span data-stu-id="55125-127">There are two general approaches to this:</span></span>
 * <span data-ttu-id="55125-128">Consulta para la entidad</span><span class="sxs-lookup"><span data-stu-id="55125-128">Query for the entity</span></span>
 * <span data-ttu-id="55125-129">Pase un indicador desde el cliente</span><span class="sxs-lookup"><span data-stu-id="55125-129">Pass a flag from the client</span></span>

<span data-ttu-id="55125-130">Para consultar la entidad, use simplemente el método de búsqueda:</span><span class="sxs-lookup"><span data-stu-id="55125-130">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="55125-131">Está fuera del ámbito de este documento para mostrar el código completo para pasar una marca de un cliente.</span><span class="sxs-lookup"><span data-stu-id="55125-131">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="55125-132">En una aplicación web, normalmente significa que se realizan solicitudes diferentes acciones diferentes, o pasar algún estado de la solicitud, a continuación, extraer en el controlador.</span><span class="sxs-lookup"><span data-stu-id="55125-132">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="55125-133">Guardar entidades únicas</span><span class="sxs-lookup"><span data-stu-id="55125-133">Saving single entities</span></span>

<span data-ttu-id="55125-134">Si se conoce como si no se necesita una inserción o actualización, a continuación, agregar o actualizar puede utilizarse correctamente:</span><span class="sxs-lookup"><span data-stu-id="55125-134">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="55125-135">Sin embargo, si la entidad utiliza valores de clave generada automáticamente, puede utilizar el método de actualización en ambos casos:</span><span class="sxs-lookup"><span data-stu-id="55125-135">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="55125-136">Normalmente, el método Update marca la entidad para la actualización, inserción no.</span><span class="sxs-lookup"><span data-stu-id="55125-136">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="55125-137">Sin embargo, si la entidad tiene una clave generada automáticamente y no se ha establecido ningún valor de clave, a continuación, la entidad en su lugar, se marca automáticamente para insertar.</span><span class="sxs-lookup"><span data-stu-id="55125-137">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="55125-138">Este comportamiento se introdujo en EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="55125-138">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="55125-139">En las versiones anteriores siempre es necesario elegir explícitamente agregar o actualizar.</span><span class="sxs-lookup"><span data-stu-id="55125-139">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="55125-140">Si la entidad no está utilizando claves generadas automáticamente, la aplicación debe decidir si la entidad se debe insertar o actualizar: por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="55125-140">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="55125-141">Son los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="55125-141">The steps here are:</span></span>
* <span data-ttu-id="55125-142">Si agrega Find devuelve un valor null, a continuación, la base de datos ya no contiene el blog con este Id., por lo que llamamos a marcar para la inserción.</span><span class="sxs-lookup"><span data-stu-id="55125-142">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="55125-143">Si la búsqueda devuelve una entidad, a continuación, se encuentra en la base de datos y el contexto ya está realizando el seguimiento de la entidad existente</span><span class="sxs-lookup"><span data-stu-id="55125-143">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="55125-144">A continuación, utilizamos SetValues para establecer los valores para todas las propiedades de esta entidad a los que procede el cliente.</span><span class="sxs-lookup"><span data-stu-id="55125-144">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="55125-145">La llamada SetValues marcará la entidad que se actualiza según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="55125-145">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="55125-146">SetValues sólo se marcará como modificar las propiedades que tienen valores diferentes a los de la entidad que realiza un seguimiento.</span><span class="sxs-lookup"><span data-stu-id="55125-146">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="55125-147">Esto significa que cuando se envía la actualización, se actualizarán sólo aquellas columnas que han cambiado realmente.</span><span class="sxs-lookup"><span data-stu-id="55125-147">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="55125-148">(Y si no ha cambiado nada, se enviará ninguna actualización en absoluto).</span><span class="sxs-lookup"><span data-stu-id="55125-148">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="55125-149">Trabajar con gráficos</span><span class="sxs-lookup"><span data-stu-id="55125-149">Working with graphs</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="55125-150">Todos los nuevos o todas las entidades existentes</span><span class="sxs-lookup"><span data-stu-id="55125-150">All new/all existing entities</span></span>

<span data-ttu-id="55125-151">Un ejemplo del uso de gráficos es insertar o actualizar un blog junto con su colección de entradas asociadas.</span><span class="sxs-lookup"><span data-stu-id="55125-151">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="55125-152">Si todas las entidades en el gráfico deben insertarse o todos se deben actualizar, el proceso es el mismo tal y como se describió anteriormente para entidades únicas.</span><span class="sxs-lookup"><span data-stu-id="55125-152">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="55125-153">Por ejemplo, un gráfico de blogs y anuncios creado similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="55125-153">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="55125-154">se pueden insertar de forma similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="55125-154">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="55125-155">La llamada a Add marcará el blog y todas las entradas va a insertar.</span><span class="sxs-lookup"><span data-stu-id="55125-155">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="55125-156">Asimismo, si todas las entidades en un gráfico necesitan actualizarse, puede usar actualización:</span><span class="sxs-lookup"><span data-stu-id="55125-156">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="55125-157">El blog y todas sus entradas se marcarán para su actualización.</span><span class="sxs-lookup"><span data-stu-id="55125-157">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="55125-158">Combinación de entidades nuevas y existentes</span><span class="sxs-lookup"><span data-stu-id="55125-158">Mix of new and existing entities</span></span>

<span data-ttu-id="55125-159">Con claves generadas automáticamente, actualización, vuelva a puede utilizarse para las inserciones y actualizaciones, incluso si el gráfico contiene una mezcla de entidades que requieren la inserción y aquellos que requieren actualización:</span><span class="sxs-lookup"><span data-stu-id="55125-159">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="55125-160">Actualización, se marcarán como una entidad en el gráfico, blog o post, de inserción si no tiene un conjunto de valores de clave, mientras que todas las demás entidades están marcados para la actualización.</span><span class="sxs-lookup"><span data-stu-id="55125-160">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="55125-161">Como antes, si no usa claves generadas automáticamente, una consulta y procesamiento se pueden usar:</span><span class="sxs-lookup"><span data-stu-id="55125-161">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="55125-162">Control de eliminaciones</span><span class="sxs-lookup"><span data-stu-id="55125-162">Handling deletes</span></span>

<span data-ttu-id="55125-163">Delete puede ser difícil de controlar desde a menudo la ausencia de una entidad significa que debe eliminarse.</span><span class="sxs-lookup"><span data-stu-id="55125-163">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="55125-164">Una manera de solucionar esto es usar "eliminaciones de software" tal que la entidad se marca como eliminada en lugar de que se eliminan realmente.</span><span class="sxs-lookup"><span data-stu-id="55125-164">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="55125-165">Elimina, a continuación, pasa a ser el mismo que las actualizaciones.</span><span class="sxs-lookup"><span data-stu-id="55125-165">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="55125-166">Eliminaciones de software pueden implementarse de uso [filtros de consulta](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="55125-166">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="55125-167">Para las eliminaciones es true, un patrón común consiste en utilizar una extensión del patrón de consulta para realizar lo que es esencialmente una diferencia de gráfico. Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="55125-167">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="55125-168">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="55125-168">TrackGraph</span></span>

<span data-ttu-id="55125-169">Internamente, agregar, adjuntar y Update utilizan cruce seguro del gráfico con una resolución adoptada para cada entidad en cuanto a si se debería marcarse como Added (para insertar), Modified (para actualizar), sin cambios (no hacen nada), o eliminadas (para eliminar).</span><span class="sxs-lookup"><span data-stu-id="55125-169">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="55125-170">Este mecanismo se expone a través de la API de TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="55125-170">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="55125-171">Por ejemplo, supongamos que, cuando el cliente envía un gráfico de entidades, establece algunas marca en cada entidad que indica cómo debe controlarse.</span><span class="sxs-lookup"><span data-stu-id="55125-171">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="55125-172">TrackGraph, a continuación, puede utilizarse para procesar este indicador:</span><span class="sxs-lookup"><span data-stu-id="55125-172">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="55125-173">Las marcas solo se muestran como parte de la entidad para simplificar el trabajo del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="55125-173">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="55125-174">Normalmente, las marcas sería parte de un DTO o algún otro Estado incluido en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="55125-174">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
