---
title: Tipos de consultas - EF Core
author: anpete
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: 6f0f860c6a4e619e13d55e6207234a8b5261ee09
ms.sourcegitcommit: d1230e34673b8323a227ab37958dfa77f3684728
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68330795"
---
# <a name="query-types"></a><span data-ttu-id="59797-102">Tipos de consulta</span><span class="sxs-lookup"><span data-stu-id="59797-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="59797-103">Esta característica es nueva en EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="59797-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="59797-104">Además de tipos de entidad, puede contener un modelo de EF Core _tipos de consulta_, que se puede usar para llevar a cabo las consultas de base de datos en datos que no se asignan a tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="59797-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

## <a name="compare-query-types-to-entity-types"></a><span data-ttu-id="59797-105">Comparar tipos de consulta a tipos de entidad</span><span class="sxs-lookup"><span data-stu-id="59797-105">Compare query types to entity types</span></span>

<span data-ttu-id="59797-106">Tipos de consulta son como tipos de entidad que:</span><span class="sxs-lookup"><span data-stu-id="59797-106">Query types are like entity types in that they:</span></span>

- <span data-ttu-id="59797-107">O bien se pueden agregar al modelo en `OnModelCreating` o a través de una propiedad "set" en una derivada _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="59797-107">Can be added to the model either in `OnModelCreating` or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="59797-108">Admite muchas de las mismas funcionalidades de asignación, como las propiedades de navegación y la asignación de herencia.</span><span class="sxs-lookup"><span data-stu-id="59797-108">Support many of the same mapping capabilities, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="59797-109">En almacenes relacionales, pueden configurar los objetos de base de datos de destino y las columnas a través de métodos de la API fluidos o las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="59797-109">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="59797-110">Sin embargo, son diferentes de entidad tipos en el que:</span><span class="sxs-lookup"><span data-stu-id="59797-110">However, they are different from entity types in that they:</span></span>

- <span data-ttu-id="59797-111">No requieren una clave que se defina.</span><span class="sxs-lookup"><span data-stu-id="59797-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="59797-112">No se realiza un seguimiento de cambios nunca la _DbContext_ y, por tanto, se nunca insertan, actualizan o eliminan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="59797-112">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="59797-113">Nunca se detectan por convención.</span><span class="sxs-lookup"><span data-stu-id="59797-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="59797-114">Solo admite un subconjunto de funciones de asignación de navegación - específicamente:</span><span class="sxs-lookup"><span data-stu-id="59797-114">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="59797-115">Nunca pueden actuar como el extremo principal de una relación.</span><span class="sxs-lookup"><span data-stu-id="59797-115">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="59797-116">Solo pueden contener propiedades de navegación de referencia que apunta a las entidades.</span><span class="sxs-lookup"><span data-stu-id="59797-116">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="59797-117">Las entidades no pueden contener propiedades de navegación a los tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="59797-117">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="59797-118">Se tratan en el _ModelBuilder_ utilizando el `Query` método en lugar de `Entity` método.</span><span class="sxs-lookup"><span data-stu-id="59797-118">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="59797-119">Se asignan en el _DbContext_ a través de las propiedades de tipo `DbQuery<T>` en lugar de `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="59797-119">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="59797-120">Se asignan a objetos de base de datos mediante el `ToView` método, en lugar de `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="59797-120">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="59797-121">Se pueden asignar a un _definir consulta_ : una definición de consulta es una consulta secundaria declarada en el modelo que actúa el origen de datos para un tipo de consulta.</span><span class="sxs-lookup"><span data-stu-id="59797-121">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="59797-122">Escenarios de uso</span><span class="sxs-lookup"><span data-stu-id="59797-122">Usage scenarios</span></span>

<span data-ttu-id="59797-123">Algunos de los escenarios de uso principales para tipos de consulta son:</span><span class="sxs-lookup"><span data-stu-id="59797-123">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="59797-124">Que actúa como el tipo de valor devuelto para ad hoc `FromSql()` consultas.</span><span class="sxs-lookup"><span data-stu-id="59797-124">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="59797-125">Asignación de vistas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="59797-125">Mapping to database views.</span></span>
- <span data-ttu-id="59797-126">Asignación de tablas que no tiene definida una clave principal.</span><span class="sxs-lookup"><span data-stu-id="59797-126">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="59797-127">Asignación de las consultas definidas en el modelo.</span><span class="sxs-lookup"><span data-stu-id="59797-127">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="59797-128">Asignación de objetos de base de datos</span><span class="sxs-lookup"><span data-stu-id="59797-128">Mapping to database objects</span></span>

<span data-ttu-id="59797-129">Asignación de un tipo de consulta a un objeto de base de datos se logra mediante el `ToView` API fluida.</span><span class="sxs-lookup"><span data-stu-id="59797-129">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="59797-130">Desde la perspectiva de EF Core, el objeto de base de datos especificado en este método es un _vista_, lo que significa que se trata como un origen de consulta de solo lectura y no puede ser el destino de la actualización, insertar o eliminar operaciones.</span><span class="sxs-lookup"><span data-stu-id="59797-130">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="59797-131">Sin embargo, esto no significa que el objeto de base de datos es realmente necesario que sea una vista de base de datos, o bien puede ser una tabla de base de datos que se tratará como de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="59797-131">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="59797-132">A la inversa, para los tipos de entidad, EF Core asume que un objeto de base de datos se especifica en el `ToTable` método se puede tratar como un _tabla_, lo que significa que puede usarse como un origen de la consulta pero también el objetivo de actualizar, eliminar e insertar operaciones.</span><span class="sxs-lookup"><span data-stu-id="59797-132">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="59797-133">De hecho, puede especificar el nombre de una vista de base de datos en `ToTable` y todo debería funcionar bien siempre que la vista está configurada para ser actualizables en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="59797-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="59797-134">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="59797-134">Example</span></span>

<span data-ttu-id="59797-135">El ejemplo siguiente muestra cómo usar el tipo de consulta para consultar una vista de base de datos.</span><span class="sxs-lookup"><span data-stu-id="59797-135">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="59797-136">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="59797-136">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="59797-137">En primer lugar, definimos un modelo sencillo de Blog y Post:</span><span class="sxs-lookup"><span data-stu-id="59797-137">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="59797-138">A continuación, definimos una vista de base de datos simple que nos permitirá consultar el número de entradas vinculadas a cada blog:</span><span class="sxs-lookup"><span data-stu-id="59797-138">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#View)]

<span data-ttu-id="59797-139">A continuación, definimos una clase para contener el resultado de la vista de base de datos:</span><span class="sxs-lookup"><span data-stu-id="59797-139">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="59797-140">A continuación, configuramos el tipo de consulta en _OnModelCreating_ utilizando el `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="59797-140">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="59797-141">API de configuración fluida estándar se usa para configurar la asignación para el tipo de consulta:</span><span class="sxs-lookup"><span data-stu-id="59797-141">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="59797-142">A continuación, configuramos el `DbContext` para `DbQuery<T>`incluir:[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]</span><span class="sxs-lookup"><span data-stu-id="59797-142">Next, we configure the `DbContext` to include the `DbQuery<T>`: [!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]</span></span>

<span data-ttu-id="59797-143">Por último, podemos consultar la vista de base de datos de la manera estándar:</span><span class="sxs-lookup"><span data-stu-id="59797-143">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="59797-144">Tenga en cuenta también que hemos definido una propiedad de nivel de consulta (DbQuery) para que actúe como raíz para las consultas en este tipo de contexto.</span><span class="sxs-lookup"><span data-stu-id="59797-144">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
