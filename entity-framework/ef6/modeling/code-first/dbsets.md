---
title: Definición de DbSets - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489003"
---
# <a name="defining-dbsets"></a>Definir DbSets
Cuando se desarrolla con el flujo de trabajo Code First definen un DbContext derivado que representa la sesión con la base de datos y expone una clase DbSet para cada tipo en el modelo. En este tema se trata las distintas formas que puede definir las propiedades DbSet.  

## <a name="dbcontext-with-dbset-properties"></a>DbContext con propiedades DbSet  

El caso común que se muestra en ejemplos de Code First es que una clase DbContext con propiedades públicas de DbSet automática para los tipos de entidad del modelo. Por ejemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Cuando se utiliza en modo Code First, esto configurará Blogs y publicaciones como tipos de entidad, así como la configuración de otros tipos accesibles desde estas. Además DbContext llamará automáticamente el establecedor para cada una de estas propiedades para configurar una instancia de la clase DbSet adecuada.  

## <a name="dbcontext-with-idbset-properties"></a>DbContext con propiedades IDbSet  

Existen situaciones, como al crear objetos ficticios o fakes, donde resulta más útil declarar las propiedades de conjunto utilizando una interfaz. En estos casos el IDbSet interfaz puede usarse en lugar de DbSet. Por ejemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Este contexto funciona exactamente igual que el contexto que utiliza la clase DbSet para sus propiedades de conjunto.  

## <a name="dbcontext-with-read-only-set-properties"></a>DbContext con las propiedades de solo lectura  

Si no desea exponer establecedores públicos para las propiedades DbSet o IDbSet en su lugar, puede crear propiedades de solo lectura y crear las instancias del conjunto por sí mismo. Por ejemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

Tenga en cuenta que DbContext almacena en caché la instancia de DbSet devuelto desde el método Set para que cada una de estas propiedades devolverá la misma instancia cada vez que se llama.  

Detección de tipos de entidad para Code First funciona de la misma manera como lo hace para las propiedades públicas captadores y establecedores.  
