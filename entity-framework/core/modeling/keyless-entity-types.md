---
title: 'Tipos de entidad sin llave: EF Core'
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: e78b9f91fd2505de300ced7b5e73291b5d1ad3b4
ms.sourcegitcommit: 7bc43f21e7bdd64926314ea949aae689f1911956
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266772"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="92f9b-102">Tipos de entidad sin llave</span><span class="sxs-lookup"><span data-stu-id="92f9b-102">Keyless Entity Types</span></span>
> [!NOTE]
> <span data-ttu-id="92f9b-103">Esta característica se agregó en EF Core 2,1 bajo el nombre de los tipos de consulta.</span><span class="sxs-lookup"><span data-stu-id="92f9b-103">This feature was added in EF Core 2.1 under the name of query types.</span></span> <span data-ttu-id="92f9b-104">En EF Core 3,0 se cambió el nombre del concepto a tipos de entidad sin entrada.</span><span class="sxs-lookup"><span data-stu-id="92f9b-104">In EF Core 3.0 the concept was renamed to keyless entity types.</span></span>

<span data-ttu-id="92f9b-105">Además de los tipos de entidad normales, un modelo de EF Core puede contener _tipos de entidad_sin clave, que se pueden usar para realizar consultas de base de datos con datos que no contengan valores de clave.</span><span class="sxs-lookup"><span data-stu-id="92f9b-105">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="92f9b-106">Características de tipos de entidad sin llave</span><span class="sxs-lookup"><span data-stu-id="92f9b-106">Keyless entity types characteristics</span></span>

<span data-ttu-id="92f9b-107">Los tipos de entidad sin llave admiten muchas de las mismas capacidades de asignación que los tipos de entidad normales, como las propiedades de navegación y asignación de herencia.</span><span class="sxs-lookup"><span data-stu-id="92f9b-107">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="92f9b-108">En almacenes relacionales, pueden configurar los objetos de base de datos de destino y las columnas a través de métodos de la API fluidos o las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="92f9b-108">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="92f9b-109">Sin embargo, son diferentes de los tipos de entidad normales en que:</span><span class="sxs-lookup"><span data-stu-id="92f9b-109">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="92f9b-110">No se puede definir una clave.</span><span class="sxs-lookup"><span data-stu-id="92f9b-110">Cannot have a key defined.</span></span>
- <span data-ttu-id="92f9b-111">Nunca se realiza un seguimiento de los cambios en _DbContext_ y, por lo tanto, nunca se insertan, actualizan o eliminan en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="92f9b-111">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="92f9b-112">Nunca se detectan por convención.</span><span class="sxs-lookup"><span data-stu-id="92f9b-112">Are never discovered by convention.</span></span>
- <span data-ttu-id="92f9b-113">Solo admite un subconjunto de capacidades de asignación de navegación, en concreto:</span><span class="sxs-lookup"><span data-stu-id="92f9b-113">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="92f9b-114">Nunca pueden actuar como el extremo principal de una relación.</span><span class="sxs-lookup"><span data-stu-id="92f9b-114">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="92f9b-115">Puede que no tengan navegaciones a entidades propiedad</span><span class="sxs-lookup"><span data-stu-id="92f9b-115">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="92f9b-116">Solo pueden contener propiedades de navegación de referencia que apunten a entidades normales.</span><span class="sxs-lookup"><span data-stu-id="92f9b-116">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="92f9b-117">Las entidades no pueden contener propiedades de navegación a tipos de entidad sin llave.</span><span class="sxs-lookup"><span data-stu-id="92f9b-117">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="92f9b-118">Debe configurarse con `.HasNoKey()` la llamada al método.</span><span class="sxs-lookup"><span data-stu-id="92f9b-118">Need to be configured with `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="92f9b-119">Se puede asignar a una _consulta de definición_.</span><span class="sxs-lookup"><span data-stu-id="92f9b-119">May be mapped to a _defining query_.</span></span> <span data-ttu-id="92f9b-120">Una consulta de definición es una consulta declarada en el modelo que actúa como origen de datos para un tipo de entidad sin entrada.</span><span class="sxs-lookup"><span data-stu-id="92f9b-120">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="92f9b-121">Escenarios de uso</span><span class="sxs-lookup"><span data-stu-id="92f9b-121">Usage scenarios</span></span>

<span data-ttu-id="92f9b-122">Algunos de los escenarios de uso principales de los tipos de entidad sin llave son:</span><span class="sxs-lookup"><span data-stu-id="92f9b-122">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="92f9b-123">Actúa como el tipo de valor devuelto para [las consultas SQL sin procesar](xref:core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="92f9b-123">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="92f9b-124">Asignación a vistas de base de datos que no contienen una clave principal.</span><span class="sxs-lookup"><span data-stu-id="92f9b-124">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="92f9b-125">Asignación de tablas que no tiene definida una clave principal.</span><span class="sxs-lookup"><span data-stu-id="92f9b-125">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="92f9b-126">Asignación de las consultas definidas en el modelo.</span><span class="sxs-lookup"><span data-stu-id="92f9b-126">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="92f9b-127">Asignación de objetos de base de datos</span><span class="sxs-lookup"><span data-stu-id="92f9b-127">Mapping to database objects</span></span>

<span data-ttu-id="92f9b-128">La asignación de un tipo de entidad sin llave a un objeto de `ToTable` base `ToView` de datos se consigue mediante el o la API fluida.</span><span class="sxs-lookup"><span data-stu-id="92f9b-128">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="92f9b-129">Desde la perspectiva de EF Core, el objeto de base de datos especificado en este método es un _vista_, lo que significa que se trata como un origen de consulta de solo lectura y no puede ser el destino de la actualización, insertar o eliminar operaciones.</span><span class="sxs-lookup"><span data-stu-id="92f9b-129">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="92f9b-130">Sin embargo, esto no significa que el objeto de base de datos sea realmente necesario para ser una vista de base de datos.</span><span class="sxs-lookup"><span data-stu-id="92f9b-130">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="92f9b-131">Como alternativa, puede tratarse de una tabla de base de datos que se tratará como de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="92f9b-131">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="92f9b-132">Por el contrario, en el caso de los tipos de entidad normales, EF Core supone que un `ToTable` objeto de base de datos especificado en el método se puede tratar como una _tabla_, lo que significa que se puede usar como origen de la consulta, pero también como destino de las operaciones de actualización, eliminación e inserción.</span><span class="sxs-lookup"><span data-stu-id="92f9b-132">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="92f9b-133">De hecho, puede especificar el nombre de una vista de base de datos en `ToTable` y todo debería funcionar bien siempre que la vista está configurada para ser actualizables en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="92f9b-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="92f9b-134">`ToView`supone que el objeto ya existe en la base de datos y que no lo crearán las migraciones.</span><span class="sxs-lookup"><span data-stu-id="92f9b-134">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="92f9b-135">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="92f9b-135">Example</span></span>

<span data-ttu-id="92f9b-136">En el ejemplo siguiente se muestra cómo utilizar los tipos de entidad sin entrada para consultar una vista de base de datos.</span><span class="sxs-lookup"><span data-stu-id="92f9b-136">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="92f9b-137">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="92f9b-137">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) on GitHub.</span></span>

<span data-ttu-id="92f9b-138">En primer lugar, definimos un modelo sencillo de Blog y Post:</span><span class="sxs-lookup"><span data-stu-id="92f9b-138">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="92f9b-139">A continuación, definimos una vista de base de datos simple que nos permitirá consultar el número de entradas vinculadas a cada blog:</span><span class="sxs-lookup"><span data-stu-id="92f9b-139">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="92f9b-140">A continuación, definimos una clase para contener el resultado de la vista de base de datos:</span><span class="sxs-lookup"><span data-stu-id="92f9b-140">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="92f9b-141">A continuación, configuraremos el tipo de entidad sin llave `HasNoKey` en OnModelCreating con la API.</span><span class="sxs-lookup"><span data-stu-id="92f9b-141">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="92f9b-142">Usamos la API de configuración fluida para configurar la asignación para el tipo de entidad sin llave:</span><span class="sxs-lookup"><span data-stu-id="92f9b-142">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="92f9b-143">A continuación, configuramos el `DbContext` para `DbSet<T>`incluir:</span><span class="sxs-lookup"><span data-stu-id="92f9b-143">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="92f9b-144">Por último, podemos consultar la vista de base de datos de la manera estándar:</span><span class="sxs-lookup"><span data-stu-id="92f9b-144">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="92f9b-145">Nota también hemos definido una propiedad de consulta de nivel de contexto (DbSet) para que actúe como raíz para las consultas en este tipo.</span><span class="sxs-lookup"><span data-stu-id="92f9b-145">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
