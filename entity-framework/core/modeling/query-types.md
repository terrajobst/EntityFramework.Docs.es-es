---
title: Tipos de consulta - Core EF
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 4e02f106e086d243b23a60c02838f32555be210e
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/19/2018
---
# <a name="query-types"></a><span data-ttu-id="c14f9-102">Tipos de consulta</span><span class="sxs-lookup"><span data-stu-id="c14f9-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="c14f9-103">Esta función es nueva en EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="c14f9-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="c14f9-104">Además de los tipos de entidad, puede contener un modelo básico de EF _consultar tipos_, que puede utilizarse para llevar a cabo consultas de base de datos con datos que no está asignados a tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="c14f9-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="c14f9-105">Los tipos de consulta tienen muchas similitudes con tipos de entidad:</span><span class="sxs-lookup"><span data-stu-id="c14f9-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="c14f9-106">Se puede también agregar al modelo o bien en `OnModelCreating`, o a través de una propiedad "set" en un derivada _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="c14f9-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="c14f9-107">Admiten muchas de las mismas capacidades de asignación, como la herencia de propiedades de navegación (vea las limitaciones más adelante), de asignación y, en almacenes relacional, la capacidad de configurar los objetos de base de datos de destino y las columnas a través de métodos de la API fluidas o las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="c14f9-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="c14f9-108">Sin embargo son diferentes de entidad tipos en el que:</span><span class="sxs-lookup"><span data-stu-id="c14f9-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="c14f9-109">No requieren una clave de definirse.</span><span class="sxs-lookup"><span data-stu-id="c14f9-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="c14f9-110">Nunca un seguimiento de cambios para la _DbContext_ y, por tanto, se nunca insertan, actualizan o eliminan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c14f9-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="c14f9-111">Nunca se descubren por convención.</span><span class="sxs-lookup"><span data-stu-id="c14f9-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="c14f9-112">Sólo son compatibles con un subconjunto de funciones de asignación de exploración - específicamente, nunca pueden actuar como el fin principal de una relación.</span><span class="sxs-lookup"><span data-stu-id="c14f9-112">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="c14f9-113">Se tratan en la _ModelBuilder_ mediante la `Query` método en lugar de la `Entity` método.</span><span class="sxs-lookup"><span data-stu-id="c14f9-113">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="c14f9-114">Se asignan en el _DbContext_ a través de propiedades de tipo `DbQuery<T>` en lugar de `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="c14f9-114">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="c14f9-115">Se asignan a objetos de base de datos mediante la `ToView` método, en lugar de `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="c14f9-115">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="c14f9-116">Se pueden asignar a un _definir consulta_ : una definición de consulta es una consulta secundaria declarada en el modelo que actúa un origen de datos para un tipo de consulta.</span><span class="sxs-lookup"><span data-stu-id="c14f9-116">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="c14f9-117">Algunos de los escenarios principales de uso de tipos de consulta son:</span><span class="sxs-lookup"><span data-stu-id="c14f9-117">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="c14f9-118">Que actúa como el tipo devuelto para ad hoc `FromSql()` consultas.</span><span class="sxs-lookup"><span data-stu-id="c14f9-118">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="c14f9-119">Asignación de vistas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="c14f9-119">Mapping to database views.</span></span>
- <span data-ttu-id="c14f9-120">Asignación de tablas que no tienen definida una clave principal.</span><span class="sxs-lookup"><span data-stu-id="c14f9-120">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="c14f9-121">Asignación de las consultas definidas en el modelo.</span><span class="sxs-lookup"><span data-stu-id="c14f9-121">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="c14f9-122">Asignar un tipo de consulta a un objeto de base de datos se logra mediante la `ToView` API fluida.</span><span class="sxs-lookup"><span data-stu-id="c14f9-122">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="c14f9-123">Desde la perspectiva del núcleo de EF, el objeto de base de datos especificado en este método es un _vista_, lo que significa que se trata como un origen de consultas de solo lectura y no puede ser el destino de la actualización, insertar o eliminar operaciones.</span><span class="sxs-lookup"><span data-stu-id="c14f9-123">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="c14f9-124">Sin embargo, esto no significa que el objeto de base de datos es realmente necesario para que sea una vista de base de datos, o bien puede ser una tabla de base de datos que se tratará como de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="c14f9-124">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="c14f9-125">Por el contrario, para los tipos de entidad, EF Core se supone que un objeto de base de datos especificado en el `ToTable` método puede tratarse como un _tabla_, lo que significa que puede usarse como un origen de la consulta pero también el objetivo de actualizar, eliminar e insertar operaciones.</span><span class="sxs-lookup"><span data-stu-id="c14f9-125">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="c14f9-126">De hecho, puede especificar el nombre de una vista de base de datos en `ToTable` y todo debería funcionar bien siempre y cuando la vista está configurada para poder actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c14f9-126">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="c14f9-127">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="c14f9-127">Example</span></span>

<span data-ttu-id="c14f9-128">En el ejemplo siguiente se muestra cómo utilizar el tipo de consulta para una vista de base de datos de consulta.</span><span class="sxs-lookup"><span data-stu-id="c14f9-128">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="c14f9-129">Puede ver un [ejemplo](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="c14f9-129">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="c14f9-130">En primer lugar, definimos un modelo simple de Blog y Post:</span><span class="sxs-lookup"><span data-stu-id="c14f9-130">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="c14f9-131">A continuación, definimos una vista de base de datos simple que nos permitirá consultar el número de mensajes asociados a cada blog:</span><span class="sxs-lookup"><span data-stu-id="c14f9-131">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="c14f9-132">A continuación, se define una clase que contenga el resultado de la vista de base de datos:</span><span class="sxs-lookup"><span data-stu-id="c14f9-132">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="c14f9-133">A continuación, configuramos el tipo de consulta en _OnModelCreating_ con el `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="c14f9-133">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="c14f9-134">Configuración estándar fluent API se utiliza para configurar la asignación para el tipo de consulta:</span><span class="sxs-lookup"><span data-stu-id="c14f9-134">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="c14f9-135">Por último, podemos consultar la vista de base de datos de la forma estándar:</span><span class="sxs-lookup"><span data-stu-id="c14f9-135">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="c14f9-136">Tenga en cuenta también que hemos definido una propiedad de nivel de consulta (DbQuery) para que actúe como raíz para las consultas en este tipo de contexto.</span><span class="sxs-lookup"><span data-stu-id="c14f9-136">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
