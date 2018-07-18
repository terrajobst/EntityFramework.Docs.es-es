---
title: 'Controlar los conflictos de simultaneidad: EF6'
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
caps.latest.revision: 3
ms.openlocfilehash: b8608dbb4cadd60ff4ff4984583f8a9d843b3949
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121832"
---
# <a name="handling-concurrency-conflicts"></a>Administrar los conflictos de simultaneidad
Optimista de forma optimista implica la simultaneidad al intentar guardar la entidad en la base de datos con la esperanza de que los datos no ha cambiado desde la entidad se cargó. Si resulta los datos han cambiado, a continuación, se produce una excepción y se debe resolver el conflicto antes de intentar volver a guardar. En este tema se explica cómo controlar dichas excepciones en Entity Framework. Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.  

Esta entrada no es el lugar adecuado para obtener una explicación completa de la simultaneidad optimista. Las siguientes secciones presupone algunos conocimientos de resolución de simultaneidad y mostrar patrones para tareas comunes.  

Muchos de estos patrones hacen uso de los temas tratados en [trabajar con valores de propiedad](~/ef6/saving/change-tracking/property-values.md).  

Resolver problemas de simultaneidad cuando se usa asociaciones independientes (donde la clave externa no está asignada a una propiedad de la entidad) es mucho más difícil que al usar asociaciones de clave externa. Por lo tanto, si va a realizar una resolución de simultaneidad en la aplicación se aconseja que siempre asigne las claves externas en las entidades. Todos los ejemplos siguientes se suponen que usa asociaciones de clave externa.  

SaveChanges produce un DbUpdateConcurrencyException cuando se detecta una excepción de simultaneidad optimista al intentar guardar una entidad que utiliza las asociaciones de clave externa.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Resolver las excepciones de simultaneidad optimista con volver a cargar (wins de la base de datos)  

El método Reload puede usarse para sobrescribir los valores actuales de la entidad con los valores de ahora en la base de datos. La entidad se luego normalmente le envía al usuario en algún tipo y debe intentar realizar de nuevo sus cambios y volver a guardar. Por ejemplo:  

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

Es una buena forma de simular una excepción de simultaneidad establecer un punto de interrupción en la llamada a SaveChanges y, a continuación, modificar una entidad que se va a guardar en la base de datos con otra herramienta como SQL Management Studio. También podría insertar una línea antes de SaveChanges para actualizar la base de datos directamente mediante SqlCommand. Por ejemplo:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

El método de entradas en DbUpdateConcurrencyException devuelve las instancias de DbEntityEntry para las entidades que no se pudieron actualizar. (Esta propiedad actualmente siempre devuelve un valor único para problemas de simultaneidad. Pueden devolver varios valores para las excepciones de actualización general.) Podría ser una alternativa para algunas situaciones obtener las entradas de todas las entidades que es posible que deba volver a cargarse desde la base de datos y volver a cargar llamadas para cada uno de ellos.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>Resolver las excepciones de simultaneidad optimista como prevalece el cliente  

El ejemplo anterior que usa la recarga a veces se denomina wins de la base de datos o porque los valores de la entidad se sobrescriben con los valores de la base de datos prevalece el almacén. A veces puede desear hacer lo contrario y sobrescribir los valores de la base de datos con los valores actuales de la entidad. Esto se denomina prevalece el cliente y puede hacerse mediante la obtención de los valores actuales de la base de datos y establece como los valores originales de la entidad. (Consulte [trabajar con valores de propiedad](~/ef6/saving/change-tracking/property-values.md) para obtener información sobre los valores actuales y originales.) Por ejemplo:  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>Resolución personalizada de las excepciones de simultaneidad optimista  

A veces desea combinar los valores actuales de la base de datos con los valores actuales de la entidad. Normalmente esto requiere alguna interacción lógica o de usuario personalizado. Por ejemplo, podría presentar un formulario al usuario que contiene los valores actuales, los valores de la base de datos y establece un valor predeterminado de valores resueltos. El usuario, a continuación, modificar los valores resueltos según sea necesario y sería estos valores resueltos en el que se guardan en la base de datos. Esto puede hacerse mediante los objetos DbPropertyValues devuelto desde CurrentValues y GetDatabaseValues en la entrada de la entidad. Por ejemplo:  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>Resolución personalizada de excepciones de simultaneidad optimista con objetos  

El código anterior usa DbPropertyValues instancias para el tráfico actual, base de datos y valores resueltos. A veces puede ser más fácil de usar las instancias de su tipo de entidad para esta. Esto puede hacerse mediante los métodos ToObject y SetValues de DbPropertyValues. Por ejemplo:  

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
