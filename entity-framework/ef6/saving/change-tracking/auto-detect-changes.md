---
title: Automático detectar cambios - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
caps.latest.revision: 3
ms.openlocfilehash: 62f2f026426346fc1230a2f5743c8cb7d232ec7f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121824"
---
# <a name="automatic-detect-changes"></a>Detectar los cambios automático
Cuando se usa la mayoría de las entidades POCO la determinación de cómo ha cambiado una entidad (y, por tanto, las actualizaciones que deben enviarse a la base de datos) se controla mediante el algoritmo de detectar cambios. Detectar el funcionamiento de los cambios al detectar las diferencias entre los valores de propiedad actuales de la entidad y los valores de propiedad originales que se almacenan en una instantánea cuando se consulta o se adjunta la entidad. Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.  

De forma predeterminada, Entity Framework realiza detectar cambios automáticamente cuando se llama a los métodos siguientes:  

- DbSet.Find  
- DbSet.Local  
- DbSet.Add  
- DbSet.AddRange
- DbSet.Remove  
- DbSet.RemoveRange
- DbSet.Attach  
- DbContext.SaveChanges  
- DbContext.GetValidationErrors  
- DbContext.Entry  
- DbChangeTracker.Entries  

## <a name="disabling-automatic-detection-of-changes"></a>Deshabilitar la detección automática de cambios  

Si está realizando el seguimiento de un lote de entidades en el contexto y se llame a uno de estos métodos muchas veces en un bucle, puede obtener mejoras de rendimiento significativas si desactiva la detección de cambios para la duración del bucle. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

No olvide volver a habilitar la detección de cambios después del bucle, hemos usado try/finally para asegurarse de que siempre volver a habilitarla incluso si el código del bucle produce una excepción.  

Una alternativa a deshabilitar y volver a habilitar es dejar la detección automática de los cambios que se ha desactivado en todo momento y en cualquier contexto de llamada. ChangeTracker.DetectChanges explícitamente o use diligentemente proxys de seguimiento de cambios. Ambas opciones son avanzadas y fácilmente puede introducir errores imperceptibles en la aplicación por lo que se utilizan con cuidado.  

Si necesita agregar o quitar varios objetos desde un contexto, considere el uso DbSet.AddRange y DbSet.RemoveRange. Este método detecta automáticamente los cambios solo una vez después de que se hayan completado las operaciones de agregar o quitar. 
