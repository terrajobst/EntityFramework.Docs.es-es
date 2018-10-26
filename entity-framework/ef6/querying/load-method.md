---
title: El método Load - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: 4a795285004612ac03ab26532ac09ca333cb4c8f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123822"
---
# <a name="the-load-method"></a><span data-ttu-id="a87d2-102">El método de carga</span><span class="sxs-lookup"><span data-stu-id="a87d2-102">The Load Method</span></span>
<span data-ttu-id="a87d2-103">Hay varios escenarios donde desea cargar las entidades de la base de datos en el contexto sin inmediatamente hacer nada con esas entidades.</span><span class="sxs-lookup"><span data-stu-id="a87d2-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="a87d2-104">Un buen ejemplo de esto está cargando entidades para el enlace de datos, como se describe en [datos locales](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="a87d2-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="a87d2-105">Una manera común de hacerlo es escribir una consulta LINQ y, a continuación, llame a ToList en él, sólo para descartar inmediatamente la lista creada.</span><span class="sxs-lookup"><span data-stu-id="a87d2-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="a87d2-106">El método de extensión de carga funciona igual que ToList, salvo que evita la creación de la lista por completo.</span><span class="sxs-lookup"><span data-stu-id="a87d2-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="a87d2-107">Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.</span><span class="sxs-lookup"><span data-stu-id="a87d2-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="a87d2-108">Estos son dos ejemplos del uso de la carga.</span><span class="sxs-lookup"><span data-stu-id="a87d2-108">Here are two examples of using Load.</span></span> <span data-ttu-id="a87d2-109">La primera se toma de una aplicación de enlace de datos de Windows Forms que se usa la carga para consultar entidades antes del enlace a la colección local, como se describe en [datos locales](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="a87d2-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="a87d2-110">El segundo ejemplo se muestra mediante Load para cargar una colección filtrada de las entidades relacionadas, como se describe en [cargar entidades relacionadas](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="a87d2-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework"))
        .Load();
}
```  
