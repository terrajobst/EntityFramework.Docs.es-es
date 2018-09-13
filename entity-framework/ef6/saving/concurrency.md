---
title: 'Controlar los conflictos de simultaneidad: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489159"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="d1279-102">Administrar los conflictos de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="d1279-102">Handling Concurrency Conflicts</span></span>
<span data-ttu-id="d1279-103">Optimista de forma optimista implica la simultaneidad al intentar guardar la entidad en la base de datos con la esperanza de que los datos no ha cambiado desde la entidad se cargó.</span><span class="sxs-lookup"><span data-stu-id="d1279-103">Optimistic concurrency involves optimistically attempting to save your entity to the database in the hope that the data there has not changed since the entity was loaded.</span></span> <span data-ttu-id="d1279-104">Si resulta los datos han cambiado, a continuación, se produce una excepción y se debe resolver el conflicto antes de intentar volver a guardar.</span><span class="sxs-lookup"><span data-stu-id="d1279-104">If it turns out that the data has changed then an exception is thrown and you must resolve the conflict before attempting to save again.</span></span> <span data-ttu-id="d1279-105">En este tema se explica cómo controlar dichas excepciones en Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d1279-105">This topic covers how to handle such exceptions in Entity Framework.</span></span> <span data-ttu-id="d1279-106">Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.</span><span class="sxs-lookup"><span data-stu-id="d1279-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="d1279-107">Esta entrada no es el lugar adecuado para obtener una explicación completa de la simultaneidad optimista.</span><span class="sxs-lookup"><span data-stu-id="d1279-107">This post is not the appropriate place for a full discussion of optimistic concurrency.</span></span> <span data-ttu-id="d1279-108">Las siguientes secciones presupone algunos conocimientos de resolución de simultaneidad y mostrar patrones para tareas comunes.</span><span class="sxs-lookup"><span data-stu-id="d1279-108">The sections below assume some knowledge of concurrency resolution and show patterns for common tasks.</span></span>  

<span data-ttu-id="d1279-109">Muchos de estos patrones hacen uso de los temas tratados en [trabajar con valores de propiedad](~/ef6/saving/change-tracking/property-values.md).</span><span class="sxs-lookup"><span data-stu-id="d1279-109">Many of these patterns make use of the topics discussed in [Working with Property Values](~/ef6/saving/change-tracking/property-values.md).</span></span>  

<span data-ttu-id="d1279-110">Resolver problemas de simultaneidad cuando se usa asociaciones independientes (donde la clave externa no está asignada a una propiedad de la entidad) es mucho más difícil que al usar asociaciones de clave externa.</span><span class="sxs-lookup"><span data-stu-id="d1279-110">Resolving concurrency issues when you are using independent associations (where the foreign key is not mapped to a property in your entity) is much more difficult than when you are using foreign key associations.</span></span> <span data-ttu-id="d1279-111">Por lo tanto, si va a realizar una resolución de simultaneidad en la aplicación se aconseja que siempre asigne las claves externas en las entidades.</span><span class="sxs-lookup"><span data-stu-id="d1279-111">Therefore if you are going to do concurrency resolution in your application it is advised that you always map foreign keys into your entities.</span></span> <span data-ttu-id="d1279-112">Todos los ejemplos siguientes se suponen que usa asociaciones de clave externa.</span><span class="sxs-lookup"><span data-stu-id="d1279-112">All the examples below assume that you are using foreign key associations.</span></span>  

<span data-ttu-id="d1279-113">SaveChanges produce un DbUpdateConcurrencyException cuando se detecta una excepción de simultaneidad optimista al intentar guardar una entidad que utiliza las asociaciones de clave externa.</span><span class="sxs-lookup"><span data-stu-id="d1279-113">A DbUpdateConcurrencyException is thrown by SaveChanges when an optimistic concurrency exception is detected while attempting to save an entity that uses foreign key associations.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a><span data-ttu-id="d1279-114">Resolver las excepciones de simultaneidad optimista con volver a cargar (wins de la base de datos)</span><span class="sxs-lookup"><span data-stu-id="d1279-114">Resolving optimistic concurrency exceptions with Reload (database wins)</span></span>  

<span data-ttu-id="d1279-115">El método Reload puede usarse para sobrescribir los valores actuales de la entidad con los valores de ahora en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d1279-115">The Reload method can be used to overwrite the current values of the entity with the values now in the database.</span></span> <span data-ttu-id="d1279-116">La entidad se luego normalmente le envía al usuario en algún tipo y debe intentar realizar de nuevo sus cambios y volver a guardar.</span><span class="sxs-lookup"><span data-stu-id="d1279-116">The entity is then typically given back to the user in some form and they must try to make their changes again and re-save.</span></span> <span data-ttu-id="d1279-117">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d1279-117">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

<span data-ttu-id="d1279-118">Es una buena forma de simular una excepción de simultaneidad establecer un punto de interrupción en la llamada a SaveChanges y, a continuación, modificar una entidad que se va a guardar en la base de datos con otra herramienta como SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="d1279-118">A good way to simulate a concurrency exception is to set a breakpoint on the SaveChanges call and then modify an entity that is being saved in the database using another tool such as SQL Management Studio.</span></span> <span data-ttu-id="d1279-119">También podría insertar una línea antes de SaveChanges para actualizar la base de datos directamente mediante SqlCommand.</span><span class="sxs-lookup"><span data-stu-id="d1279-119">You could also insert a line before SaveChanges to update the database directly using SqlCommand.</span></span> <span data-ttu-id="d1279-120">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d1279-120">For example:</span></span>  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

<span data-ttu-id="d1279-121">El método de entradas en DbUpdateConcurrencyException devuelve las instancias de DbEntityEntry para las entidades que no se pudieron actualizar.</span><span class="sxs-lookup"><span data-stu-id="d1279-121">The Entries method on DbUpdateConcurrencyException returns the DbEntityEntry instances for the entities that failed to update.</span></span> <span data-ttu-id="d1279-122">(Esta propiedad actualmente siempre devuelve un valor único para problemas de simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="d1279-122">(This property currently always returns a single value for concurrency issues.</span></span> <span data-ttu-id="d1279-123">Pueden devolver varios valores para las excepciones de actualización general.) Podría ser una alternativa para algunas situaciones obtener las entradas de todas las entidades que es posible que deba volver a cargarse desde la base de datos y volver a cargar llamadas para cada uno de ellos.</span><span class="sxs-lookup"><span data-stu-id="d1279-123">It may return multiple values for general update exceptions.) An alternative for some situations might be to get entries for all entities that may need to be reloaded from the database and call reload for each of these.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a><span data-ttu-id="d1279-124">Resolver las excepciones de simultaneidad optimista como prevalece el cliente</span><span class="sxs-lookup"><span data-stu-id="d1279-124">Resolving optimistic concurrency exceptions as client wins</span></span>  

<span data-ttu-id="d1279-125">El ejemplo anterior que usa la recarga a veces se denomina wins de la base de datos o porque los valores de la entidad se sobrescriben con los valores de la base de datos prevalece el almacén.</span><span class="sxs-lookup"><span data-stu-id="d1279-125">The example above that uses Reload is sometimes called database wins or store wins because the values in the entity are overwritten by values from the database.</span></span> <span data-ttu-id="d1279-126">A veces puede desear hacer lo contrario y sobrescribir los valores de la base de datos con los valores actuales de la entidad.</span><span class="sxs-lookup"><span data-stu-id="d1279-126">Sometimes you may wish to do the opposite and overwrite the values in the database with the values currently in the entity.</span></span> <span data-ttu-id="d1279-127">Esto se denomina prevalece el cliente y puede hacerse mediante la obtención de los valores actuales de la base de datos y establece como los valores originales de la entidad.</span><span class="sxs-lookup"><span data-stu-id="d1279-127">This is sometimes called client wins and can be done by getting the current database values and setting them as the original values for the entity.</span></span> <span data-ttu-id="d1279-128">(Consulte [trabajar con valores de propiedad](~/ef6/saving/change-tracking/property-values.md) para obtener información sobre los valores actuales y originales.) Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d1279-128">(See [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) for information on current and original values.) For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a><span data-ttu-id="d1279-129">Resolución personalizada de las excepciones de simultaneidad optimista</span><span class="sxs-lookup"><span data-stu-id="d1279-129">Custom resolution of optimistic concurrency exceptions</span></span>  

<span data-ttu-id="d1279-130">A veces desea combinar los valores actuales de la base de datos con los valores actuales de la entidad.</span><span class="sxs-lookup"><span data-stu-id="d1279-130">Sometimes you may want to combine the values currently in the database with the values currently in the entity.</span></span> <span data-ttu-id="d1279-131">Normalmente esto requiere alguna interacción lógica o de usuario personalizado.</span><span class="sxs-lookup"><span data-stu-id="d1279-131">This usually requires some custom logic or user interaction.</span></span> <span data-ttu-id="d1279-132">Por ejemplo, podría presentar un formulario al usuario que contiene los valores actuales, los valores de la base de datos y establece un valor predeterminado de valores resueltos.</span><span class="sxs-lookup"><span data-stu-id="d1279-132">For example, you might present a form to the user containing the current values, the values in the database, and a default set of resolved values.</span></span> <span data-ttu-id="d1279-133">El usuario, a continuación, modificar los valores resueltos según sea necesario y sería estos valores resueltos en el que se guardan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d1279-133">The user would then edit the resolved values as necessary and it would be these resolved values that get saved to the database.</span></span> <span data-ttu-id="d1279-134">Esto puede hacerse mediante los objetos DbPropertyValues devuelto desde CurrentValues y GetDatabaseValues en la entrada de la entidad.</span><span class="sxs-lookup"><span data-stu-id="d1279-134">This can be done using the DbPropertyValues objects returned from CurrentValues and GetDatabaseValues on the entity’s entry.</span></span> <span data-ttu-id="d1279-135">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d1279-135">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a><span data-ttu-id="d1279-136">Resolución personalizada de excepciones de simultaneidad optimista con objetos</span><span class="sxs-lookup"><span data-stu-id="d1279-136">Custom resolution of optimistic concurrency exceptions using objects</span></span>  

<span data-ttu-id="d1279-137">El código anterior usa DbPropertyValues instancias para el tráfico actual, base de datos y valores resueltos.</span><span class="sxs-lookup"><span data-stu-id="d1279-137">The code above uses DbPropertyValues instances for passing around current, database, and resolved values.</span></span> <span data-ttu-id="d1279-138">A veces puede ser más fácil de usar las instancias de su tipo de entidad para esta.</span><span class="sxs-lookup"><span data-stu-id="d1279-138">Sometimes it may be easier to use instances of your entity type for this.</span></span> <span data-ttu-id="d1279-139">Esto puede hacerse mediante los métodos ToObject y SetValues de DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="d1279-139">This can be done using the ToObject and SetValues methods of DbPropertyValues.</span></span> <span data-ttu-id="d1279-140">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d1279-140">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
