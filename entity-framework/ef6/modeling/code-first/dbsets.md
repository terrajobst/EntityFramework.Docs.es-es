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
# <a name="defining-dbsets"></a><span data-ttu-id="9c2e8-102">Definir DbSets</span><span class="sxs-lookup"><span data-stu-id="9c2e8-102">Defining DbSets</span></span>
<span data-ttu-id="9c2e8-103">Cuando se desarrolla con el flujo de trabajo Code First definen un DbContext derivado que representa la sesión con la base de datos y expone una clase DbSet para cada tipo en el modelo.</span><span class="sxs-lookup"><span data-stu-id="9c2e8-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="9c2e8-104">En este tema se trata las distintas formas que puede definir las propiedades DbSet.</span><span class="sxs-lookup"><span data-stu-id="9c2e8-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="9c2e8-105">DbContext con propiedades DbSet</span><span class="sxs-lookup"><span data-stu-id="9c2e8-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="9c2e8-106">El caso común que se muestra en ejemplos de Code First es que una clase DbContext con propiedades públicas de DbSet automática para los tipos de entidad del modelo.</span><span class="sxs-lookup"><span data-stu-id="9c2e8-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="9c2e8-107">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9c2e8-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="9c2e8-108">Cuando se utiliza en modo Code First, esto configurará Blogs y publicaciones como tipos de entidad, así como la configuración de otros tipos accesibles desde estas.</span><span class="sxs-lookup"><span data-stu-id="9c2e8-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="9c2e8-109">Además DbContext llamará automáticamente el establecedor para cada una de estas propiedades para configurar una instancia de la clase DbSet adecuada.</span><span class="sxs-lookup"><span data-stu-id="9c2e8-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="9c2e8-110">DbContext con propiedades IDbSet</span><span class="sxs-lookup"><span data-stu-id="9c2e8-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="9c2e8-111">Existen situaciones, como al crear objetos ficticios o fakes, donde resulta más útil declarar las propiedades de conjunto utilizando una interfaz.</span><span class="sxs-lookup"><span data-stu-id="9c2e8-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="9c2e8-112">En estos casos el IDbSet interfaz puede usarse en lugar de DbSet.</span><span class="sxs-lookup"><span data-stu-id="9c2e8-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="9c2e8-113">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9c2e8-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="9c2e8-114">Este contexto funciona exactamente igual que el contexto que utiliza la clase DbSet para sus propiedades de conjunto.</span><span class="sxs-lookup"><span data-stu-id="9c2e8-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="9c2e8-115">DbContext con las propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="9c2e8-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="9c2e8-116">Si no desea exponer establecedores públicos para las propiedades DbSet o IDbSet en su lugar, puede crear propiedades de solo lectura y crear las instancias del conjunto por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="9c2e8-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="9c2e8-117">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9c2e8-117">For example:</span></span>  

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

<span data-ttu-id="9c2e8-118">Tenga en cuenta que DbContext almacena en caché la instancia de DbSet devuelto desde el método Set para que cada una de estas propiedades devolverá la misma instancia cada vez que se llama.</span><span class="sxs-lookup"><span data-stu-id="9c2e8-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="9c2e8-119">Detección de tipos de entidad para Code First funciona de la misma manera como lo hace para las propiedades públicas captadores y establecedores.</span><span class="sxs-lookup"><span data-stu-id="9c2e8-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
