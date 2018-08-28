---
title: Trabajar con servidores proxy - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 7b82dd370e67d1622fc00ff5e5275721d0fc4fe1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997208"
---
# <a name="working-with-proxies"></a>Trabajar con servidores proxy
Al crear instancias de tipos de entidad POCO, Entity Framework con frecuencia crea instancias de un tipo derivado generada dinámicamente que actúa como un proxy para la entidad. Este proxy reemplaza algunas propiedades de la entidad que se va a insertar enlaces para realizar automáticamente acciones cuando se tiene acceso a la propiedad virtuales. Por ejemplo, este mecanismo se utiliza para admitir la carga diferida de relaciones. Las técnicas que se muestran en este tema se aplican igualmente a los modelos creados con Code First y EF Designer.  

## <a name="disabling-proxy-creation"></a>Deshabilitar la creación de proxy  

A veces resulta útil para evitar que Entity Framework desde la creación de instancias de proxy. Por ejemplo, serializar instancias de proxy no es considerablemente más fácil que serializar instancias de proxy. Creación de proxy puede desactivarse si desactiva la marca ProxyCreationEnabled. Un buen podría hacer esto es en el constructor de su contexto. Por ejemplo:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Tenga en cuenta que EF no creará los servidores proxy para los tipos que no hay nada para que realice el proxy. Esto significa que también puede evitar a los servidores proxy al tener tipos que están sellados o no tienen ninguna propiedad virtual.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Crear explícitamente una instancia de un servidor proxy  

Si crea una instancia de una entidad mediante el operador new, no se creará una instancia del proxy. Esto puede no ser un problema, pero si necesita crear una instancia del proxy (por ejemplo, para que funcionan diferida carga o el proxy de seguimiento de cambios), a continuación, puede hacerlo mediante el método Create de DbSet. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

La versión genérica de Create puede utilizarse si desea crear una instancia de un tipo de entidad derivada. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Tenga en cuenta que el método Create no agregar o asociar la entidad creada en el contexto.  

Tenga en cuenta que el método Create simplemente creará una instancia del propio tipo de entidad si la creación de un tipo de proxy para la entidad no tendría ningún valor porque podría no hacer nada. Por ejemplo, si el tipo de entidad está sellado o no tiene ninguna propiedad virtual crear simplemente creará una instancia del tipo de entidad.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Obtener el tipo de entidad real de un tipo de proxy  

Tipos de proxy tienen nombres con un aspecto similar al siguiente:  

System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

Puede encontrar el tipo de entidad para este tipo de proxy con el método GetObjectType de ObjectContext. Por ejemplo:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Tenga en cuenta que si el tipo pasado a GetObjectType es todavía se devuelve una instancia de un tipo de entidad que no es un tipo de proxy, a continuación, el tipo de entidad. Esto significa que siempre puede usar este método para obtener el tipo de entidad real sin otras comprobaciones para ver si el tipo es un tipo de proxy o no.  
