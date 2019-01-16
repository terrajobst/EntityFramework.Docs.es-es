---
title: Trabajar con DbContext - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489068"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="47f92-102">Trabajar con DbContext</span><span class="sxs-lookup"><span data-stu-id="47f92-102">Working with DbContext</span></span>

<span data-ttu-id="47f92-103">Para usar Entity Framework para consultar, insertar, actualizar y eliminar datos mediante objetos .NET, primero deberá [crear un modelo](~/ef6/modeling/index.md) que asigna las entidades y relaciones que se definen en el modelo en las tablas de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="47f92-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="47f92-104">Una vez tenga un modelo, la clase principal con la que tu aplicación interactúa es `System.Data.Entity.DbContext` (a menudo denominado como la clase de contexto).</span><span class="sxs-lookup"><span data-stu-id="47f92-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="47f92-105">Puede usar un DbContext asociado a un modelo para:</span><span class="sxs-lookup"><span data-stu-id="47f92-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="47f92-106">Escribir y ejecutar consultas</span><span class="sxs-lookup"><span data-stu-id="47f92-106">Write and execute queries</span></span>   
- <span data-ttu-id="47f92-107">Materializar resultados de una consulta como objetos entidad</span><span class="sxs-lookup"><span data-stu-id="47f92-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="47f92-108">Realizar un seguimiento de los cambios realizados en esos objetos</span><span class="sxs-lookup"><span data-stu-id="47f92-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="47f92-109">Almacenar los cambios del objeto en la base de datos</span><span class="sxs-lookup"><span data-stu-id="47f92-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="47f92-110">Enlazar objetos en memoria a los controles de interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="47f92-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="47f92-111">Esta página proporciona algunas instrucciones sobre cómo administrar la clase de contexto.</span><span class="sxs-lookup"><span data-stu-id="47f92-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="47f92-112">Definir una clase derivada de DbContext</span><span class="sxs-lookup"><span data-stu-id="47f92-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="47f92-113">La manera recomendada para trabajar con el contexto es definir una clase que derive de DbContext y exponga las propiedades DbSet que representan colecciones de las entidades especificadas en el contexto.</span><span class="sxs-lookup"><span data-stu-id="47f92-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="47f92-114">Si está trabajando con EF Designer, se generará el contexto para usted.</span><span class="sxs-lookup"><span data-stu-id="47f92-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="47f92-115">Si está trabajando con Code First, normalmente usted mismo escribirá el contexto.</span><span class="sxs-lookup"><span data-stu-id="47f92-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="47f92-116">Una vez que tenga un contexto, podría consultar, agregar (con los métodos `Add` o `Attach`) o eliminar (con `Remove`) entidades en el contexto a través de estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="47f92-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="47f92-117">Obtener acceso a un `DbSet` propiedad en un objeto de contexto representa una consulta inicial que devuelve todas las entidades del tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="47f92-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="47f92-118">Tenga en cuenta acceder a la propiedad no ejecutará la consulta.</span><span class="sxs-lookup"><span data-stu-id="47f92-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="47f92-119">Se ejecuta una consulta cuando:</span><span class="sxs-lookup"><span data-stu-id="47f92-119">A query is executed when:</span></span>  

- <span data-ttu-id="47f92-120">Se enumera mediante una instrucción `foreach` (C#) o `For Each` (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="47f92-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="47f92-121">Se enumera mediante una operación de colección como `ToArray`, `ToDictionary`, o `ToList`.</span><span class="sxs-lookup"><span data-stu-id="47f92-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="47f92-122">Se especifican operadores LINQ, como `First` o `Any` en la parte exterior de la consulta.</span><span class="sxs-lookup"><span data-stu-id="47f92-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="47f92-123">Se llama a uno de los métodos siguientes: el método de extensión `Load`, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, y `DbSet<T>.Find`, si una entidad con la clave especificada no se encuentra ya cargada en el contexto.</span><span class="sxs-lookup"><span data-stu-id="47f92-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="47f92-124">Período de duración</span><span class="sxs-lookup"><span data-stu-id="47f92-124">Lifetime</span></span>  

<span data-ttu-id="47f92-125">La duración del contexto comienza cuando se crea y finaliza cuando la instancia se elimina o es recogida por el recolector de basura.</span><span class="sxs-lookup"><span data-stu-id="47f92-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="47f92-126">Use **using** si desea que todos los recursos que controla el contexto sean eliminados al final del bloque.</span><span class="sxs-lookup"><span data-stu-id="47f92-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="47f92-127">Cuando usas **using**, el compilador crea automáticamente un bloque try/finally y llama a `dispose` en el bloque **finally**.  </span><span class="sxs-lookup"><span data-stu-id="47f92-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="47f92-128">Estas son algunas directrices generales al decidir la duración del contexto:</span><span class="sxs-lookup"><span data-stu-id="47f92-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="47f92-129">Cuando se trabaja con aplicaciones Web, utilice una instancia de contexto por solicitud.</span><span class="sxs-lookup"><span data-stu-id="47f92-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="47f92-130">Cuando se trabaja con Windows Presentation Foundation (WPF) o Windows Forms, use una instancia de contexto por formulario.</span><span class="sxs-lookup"><span data-stu-id="47f92-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="47f92-131">Esto le permite usar la funcionalidad de seguimiento de cambios proporciona ese contexto.</span><span class="sxs-lookup"><span data-stu-id="47f92-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="47f92-132">Si la instancia de contexto se crea un contenedor de inyección de dependencia, normalmente es responsabilidad del contenedor el eliminar el contexto.</span><span class="sxs-lookup"><span data-stu-id="47f92-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="47f92-133">Si se crea el contexto en el código de aplicación, recuerde eliminar el contexto cuando ya no sea necesario.</span><span class="sxs-lookup"><span data-stu-id="47f92-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="47f92-134">Cuando se trabaja con el contexto de ejecución prolongada, tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="47f92-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="47f92-135">La carga de más objetos y sus referencias en la memoria puede aumentar rápidamente el consumo de memoria del contexto.</span><span class="sxs-lookup"><span data-stu-id="47f92-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="47f92-136">lo que puede ocasionar problemas de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="47f92-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="47f92-137">El contexto no es seguro para subprocesos, por lo tanto no debe compartirse entre varios subprocesos que realizan el trabajo en él al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="47f92-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="47f92-138">Si una excepción provoca que el contexto entre en un estado irrecuperable, puede finalizar toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="47f92-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="47f92-139">Las posibilidades de que haya problemas relacionados con la concurrencia se incrementan a medida que aumenta la distancia entre el momento en que se consultan los datos y el momento en que se actualizan.</span><span class="sxs-lookup"><span data-stu-id="47f92-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="47f92-140">Conexiones</span><span class="sxs-lookup"><span data-stu-id="47f92-140">Connections</span></span>  

<span data-ttu-id="47f92-141">De forma predeterminada, el contexto administra las conexiones a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="47f92-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="47f92-142">El contexto se abre y cierra las conexiones según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="47f92-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="47f92-143">Por ejemplo, el contexto abre una conexión para ejecutar una consulta y, a continuación, cierra la conexión cuando se han procesado todos los conjuntos de resultados.</span><span class="sxs-lookup"><span data-stu-id="47f92-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="47f92-144">Hay casos en los que se desea más control sobre el momento en que se abre y cierra la conexión.</span><span class="sxs-lookup"><span data-stu-id="47f92-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="47f92-145">Por ejemplo, cuando se trabaja con SQL Server Compact, a menudo se recomienda mantener una conexión abierta independiente a la base de datos durante la vigencia de la aplicación para mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="47f92-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="47f92-146">Puede administrar este proceso manualmente mediante la propiedad `Connection`.</span><span class="sxs-lookup"><span data-stu-id="47f92-146">You can manage this process manually by using the `Connection` property.</span></span>  
