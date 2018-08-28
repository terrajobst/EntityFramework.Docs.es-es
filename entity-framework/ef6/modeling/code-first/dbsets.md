---
title: Definición de DbSets - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: cc45ed1ceb20bc90090adb3e93c10651c69c9a6a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993862"
---
# <a name="defining-dbsets"></a><span data-ttu-id="7da3f-102">Definir DbSets</span><span class="sxs-lookup"><span data-stu-id="7da3f-102">Defining DbSets</span></span>
<span data-ttu-id="7da3f-103">Cuando se desarrolla con el flujo de trabajo Code First definen un DbContext derivado que representa la sesión con la base de datos y expone una clase DbSet para cada tipo en el modelo.</span><span class="sxs-lookup"><span data-stu-id="7da3f-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="7da3f-104">En este tema se trata las distintas formas que puede definir las propiedades DbSet.</span><span class="sxs-lookup"><span data-stu-id="7da3f-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="7da3f-105">DbContext con propiedades DbSet</span><span class="sxs-lookup"><span data-stu-id="7da3f-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="7da3f-106">El caso común que se muestra en ejemplos de Code First es que una clase DbContext con propiedades públicas de DbSet automática para los tipos de entidad del modelo.</span><span class="sxs-lookup"><span data-stu-id="7da3f-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="7da3f-107">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7da3f-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="7da3f-108">Cuando se utiliza en modo Code First, esto configurará Blogs y publicaciones como tipos de entidad, así como la configuración de otros tipos accesibles desde estas.</span><span class="sxs-lookup"><span data-stu-id="7da3f-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="7da3f-109">Además DbContext llamará automáticamente el establecedor para cada una de estas propiedades para configurar una instancia de la clase DbSet adecuada.</span><span class="sxs-lookup"><span data-stu-id="7da3f-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="7da3f-110">DbContext con propiedades IDbSet</span><span class="sxs-lookup"><span data-stu-id="7da3f-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="7da3f-111">Existen situaciones, como al crear objetos ficticios o fakes, donde resulta más útil declarar las propiedades de conjunto utilizando una interfaz.</span><span class="sxs-lookup"><span data-stu-id="7da3f-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="7da3f-112">En estos casos el IDbSet interfaz puede usarse en lugar de DbSet.</span><span class="sxs-lookup"><span data-stu-id="7da3f-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="7da3f-113">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7da3f-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="7da3f-114">Este contexto funciona exactamente igual que el contexto que utiliza la clase DbSet para sus propiedades de conjunto.</span><span class="sxs-lookup"><span data-stu-id="7da3f-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="7da3f-115">DbContext con las propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="7da3f-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="7da3f-116">Si no desea exponer establecedores públicos para las propiedades DbSet o IDbSet en su lugar, puede crear propiedades de solo lectura y crear las instancias del conjunto por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="7da3f-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="7da3f-117">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7da3f-117">For example:</span></span>  

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

<span data-ttu-id="7da3f-118">Tenga en cuenta que DbContext almacena en caché la instancia de DbSet devuelto desde el método Set para que cada una de estas propiedades devolverá la misma instancia cada vez que se llama.</span><span class="sxs-lookup"><span data-stu-id="7da3f-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="7da3f-119">Detección de tipos de entidad para Code First funciona de la misma manera como lo hace para las propiedades públicas captadores y establecedores.</span><span class="sxs-lookup"><span data-stu-id="7da3f-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
