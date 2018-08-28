---
title: El método Load - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: f7e8410b8fb8b5c3e86c51cd61868604a7566d0c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996656"
---
# <a name="the-load-method"></a>El método de carga
Hay varios escenarios donde desea cargar las entidades de la base de datos en el contexto sin inmediatamente hacer nada con esas entidades. Un buen ejemplo de esto está cargando entidades para el enlace de datos, como se describe en [datos locales](~/ef6/querying/local-data.md). Una manera común de hacerlo es escribir una consulta LINQ y, a continuación, llame a ToList en él, sólo para descartar inmediatamente la lista creada. El método de extensión de carga funciona igual que ToList, salvo que evita la creación de la lista por completo.  

Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.  

Estos son dos ejemplos del uso de la carga. La primera se toma de una aplicación de enlace de datos de Windows Forms que se usa la carga para consultar entidades antes del enlace a la colección local, como se describe en [datos locales](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

El segundo ejemplo se muestra mediante Load para cargar una colección filtrada de las entidades relacionadas, como se describe en [cargar entidades relacionadas](~/ef6/querying/related-data.md):  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  
