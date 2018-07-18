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
# <a name="automatic-detect-changes"></a><span data-ttu-id="01ce0-102">Detectar los cambios automático</span><span class="sxs-lookup"><span data-stu-id="01ce0-102">Automatic detect changes</span></span>
<span data-ttu-id="01ce0-103">Cuando se usa la mayoría de las entidades POCO la determinación de cómo ha cambiado una entidad (y, por tanto, las actualizaciones que deben enviarse a la base de datos) se controla mediante el algoritmo de detectar cambios.</span><span class="sxs-lookup"><span data-stu-id="01ce0-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="01ce0-104">Detectar el funcionamiento de los cambios al detectar las diferencias entre los valores de propiedad actuales de la entidad y los valores de propiedad originales que se almacenan en una instantánea cuando se consulta o se adjunta la entidad.</span><span class="sxs-lookup"><span data-stu-id="01ce0-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="01ce0-105">Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.</span><span class="sxs-lookup"><span data-stu-id="01ce0-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="01ce0-106">De forma predeterminada, Entity Framework realiza detectar cambios automáticamente cuando se llama a los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="01ce0-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="01ce0-107">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="01ce0-107">DbSet.Find</span></span>  
- <span data-ttu-id="01ce0-108">DbSet.Local</span><span class="sxs-lookup"><span data-stu-id="01ce0-108">DbSet.Local</span></span>  
- <span data-ttu-id="01ce0-109">DbSet.Add</span><span class="sxs-lookup"><span data-stu-id="01ce0-109">DbSet.Add</span></span>  
- <span data-ttu-id="01ce0-110">DbSet.AddRange</span><span class="sxs-lookup"><span data-stu-id="01ce0-110">DbSet.AddRange</span></span>
- <span data-ttu-id="01ce0-111">DbSet.Remove</span><span class="sxs-lookup"><span data-stu-id="01ce0-111">DbSet.Remove</span></span>  
- <span data-ttu-id="01ce0-112">DbSet.RemoveRange</span><span class="sxs-lookup"><span data-stu-id="01ce0-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="01ce0-113">DbSet.Attach</span><span class="sxs-lookup"><span data-stu-id="01ce0-113">DbSet.Attach</span></span>  
- <span data-ttu-id="01ce0-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="01ce0-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="01ce0-115">DbContext.GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="01ce0-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="01ce0-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="01ce0-116">DbContext.Entry</span></span>  
- <span data-ttu-id="01ce0-117">DbChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="01ce0-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="01ce0-118">Deshabilitar la detección automática de cambios</span><span class="sxs-lookup"><span data-stu-id="01ce0-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="01ce0-119">Si está realizando el seguimiento de un lote de entidades en el contexto y se llame a uno de estos métodos muchas veces en un bucle, puede obtener mejoras de rendimiento significativas si desactiva la detección de cambios para la duración del bucle.</span><span class="sxs-lookup"><span data-stu-id="01ce0-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="01ce0-120">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="01ce0-120">For example:</span></span>  

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

<span data-ttu-id="01ce0-121">No olvide volver a habilitar la detección de cambios después del bucle, hemos usado try/finally para asegurarse de que siempre volver a habilitarla incluso si el código del bucle produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="01ce0-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="01ce0-122">Una alternativa a deshabilitar y volver a habilitar es dejar la detección automática de los cambios que se ha desactivado en todo momento y en cualquier contexto de llamada. ChangeTracker.DetectChanges explícitamente o use diligentemente proxys de seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="01ce0-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="01ce0-123">Ambas opciones son avanzadas y fácilmente puede introducir errores imperceptibles en la aplicación por lo que se utilizan con cuidado.</span><span class="sxs-lookup"><span data-stu-id="01ce0-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="01ce0-124">Si necesita agregar o quitar varios objetos desde un contexto, considere el uso DbSet.AddRange y DbSet.RemoveRange.</span><span class="sxs-lookup"><span data-stu-id="01ce0-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="01ce0-125">Este método detecta automáticamente los cambios solo una vez después de que se hayan completado las operaciones de agregar o quitar.</span><span class="sxs-lookup"><span data-stu-id="01ce0-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
