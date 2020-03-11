---
title: El método Load-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414498"
---
# <a name="the-load-method"></a><span data-ttu-id="e3682-102">El método de carga</span><span class="sxs-lookup"><span data-stu-id="e3682-102">The Load Method</span></span>
<span data-ttu-id="e3682-103">Hay varios escenarios en los que puede que desee cargar entidades de la base de datos en el contexto sin hacer nada inmediatamente con esas entidades.</span><span class="sxs-lookup"><span data-stu-id="e3682-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="e3682-104">Un buen ejemplo es la carga de entidades para el enlace de datos como se describe en [datos locales](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="e3682-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="e3682-105">Una forma habitual de hacerlo es escribir una consulta LINQ y, a continuación, llamar a ToList en ella, solo para descartar inmediatamente la lista creada.</span><span class="sxs-lookup"><span data-stu-id="e3682-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="e3682-106">El método de extensión de carga funciona igual que ToList, salvo que evita la creación de la lista por completo.</span><span class="sxs-lookup"><span data-stu-id="e3682-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="e3682-107">Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.</span><span class="sxs-lookup"><span data-stu-id="e3682-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="e3682-108">A continuación se muestran dos ejemplos del uso de load.</span><span class="sxs-lookup"><span data-stu-id="e3682-108">Here are two examples of using Load.</span></span> <span data-ttu-id="e3682-109">La primera se toma de un Windows Forms aplicación de enlace de datos donde la carga se utiliza para consultar las entidades antes de enlazarse a la colección local, como se describe en [datos locales](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="e3682-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="e3682-110">En el segundo ejemplo se muestra el uso de LOAD para cargar una colección filtrada de entidades relacionadas, como se describe en [carga de entidades relacionadas](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="e3682-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

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
