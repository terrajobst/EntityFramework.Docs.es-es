---
title: Tipos de consulta - Core EF
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: dfd08cd1c30debddc79740bbf05c39c22e973855
ms.sourcegitcommit: 01b5cf3b7c983bcced91e7cc4c78391ced2d2caa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/17/2018
---
# <a name="query-types"></a><span data-ttu-id="79db2-102">Tipos de consulta</span><span class="sxs-lookup"><span data-stu-id="79db2-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="79db2-103">Esta función es nueva en EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="79db2-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="79db2-104">Tipos de consultas son los tipos de resultado de consulta de sólo lectura que se pueden agregar al modelo EF central.</span><span class="sxs-lookup"><span data-stu-id="79db2-104">Query Types are read-only query result types that can be added to the EF Core model.</span></span> <span data-ttu-id="79db2-105">Tipos de consulta habilitar las consultas ad-hoc (como tipos anónimos), pero son más flexibles, ya que pueden tener configuración de asignación especificada.</span><span class="sxs-lookup"><span data-stu-id="79db2-105">Query Types enable ad-hoc querying (like anonymous types), but are more flexible because they can have mapping configuration specified.</span></span>

<span data-ttu-id="79db2-106">Son conceptualmente similares a los tipos de entidad en la:</span><span class="sxs-lookup"><span data-stu-id="79db2-106">They are conceptually similar to Entity Types in that:</span></span>

- <span data-ttu-id="79db2-107">Son tipos POCO C# que se agregan al modelo, ya sea en ```OnModelCreating``` mediante la ```ModelBuilder.Query``` método, o a través de una propiedad DbContext "set" (para tipos de consulta de este tipo de propiedad se escribe como ```DbQuery<T>``` en lugar de ```DbSet<T>```).</span><span class="sxs-lookup"><span data-stu-id="79db2-107">They are POCO C# types that are added to the model, either in ```OnModelCreating``` using the ```ModelBuilder.Query``` method, or via a DbContext "set" property (for query types such a property is typed as ```DbQuery<T>``` rather than ```DbSet<T>```).</span></span>
- <span data-ttu-id="79db2-108">Soportan la mayor parte de las mismas capacidades de asignación como tipos de entidad regulares.</span><span class="sxs-lookup"><span data-stu-id="79db2-108">They support much of the same mapping capabilities as regular entity types.</span></span> <span data-ttu-id="79db2-109">Por ejemplo, la asignación de herencia, navegaciones (vea limitiations más abajo) y, en almacenes relacionales, la capacidad para configurar los objetos de esquema de base de datos de destino a través de ```ToTable```, ```HasColumn``` api fluida métodos (o las anotaciones de datos).</span><span class="sxs-lookup"><span data-stu-id="79db2-109">For example, inheritance mapping, navigations (see limitiations below) and, on relational stores, the ability to configure the target database schema objects via ```ToTable```, ```HasColumn``` fluent-api methods (or data annotations).</span></span>

<span data-ttu-id="79db2-110">Tipos de consulta son diferentes de entidad tipos en los que:</span><span class="sxs-lookup"><span data-stu-id="79db2-110">Query Types are different from entity types in that they:</span></span>

- <span data-ttu-id="79db2-111">No requieren una clave de definirse.</span><span class="sxs-lookup"><span data-stu-id="79db2-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="79db2-112">Nunca realiza el seguimiento al seguimiento de cambios.</span><span class="sxs-lookup"><span data-stu-id="79db2-112">Are never tracked by the Change Tracker.</span></span>
- <span data-ttu-id="79db2-113">Nunca se descubren por convención.</span><span class="sxs-lookup"><span data-stu-id="79db2-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="79db2-114">Sólo son compatibles con un subconjunto de funciones de asignación de exploración - específicamente, nunca pueden actuar como el fin principal de una relación.</span><span class="sxs-lookup"><span data-stu-id="79db2-114">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="79db2-115">Se pueden asignar a un _definición de consulta_ -una definición de consulta es una consulta secundaria que actúa un origen de datos para un tipo de consulta.</span><span class="sxs-lookup"><span data-stu-id="79db2-115">May be mapped to a _defining query_ - A Defining Query is a secondary query that acts a data source for a Query Type.</span></span>

<span data-ttu-id="79db2-116">Algunos de los escenarios principales de uso de tipos de consulta son:</span><span class="sxs-lookup"><span data-stu-id="79db2-116">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="79db2-117">Asignación de vistas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="79db2-117">Mapping to database views.</span></span>
- <span data-ttu-id="79db2-118">Asignación de tablas que no tienen definida una clave principal.</span><span class="sxs-lookup"><span data-stu-id="79db2-118">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="79db2-119">Que actúa como el tipo devuelto para ad hoc ```FromSql()``` consultas.</span><span class="sxs-lookup"><span data-stu-id="79db2-119">Serving as the return type for ad hoc ```FromSql()``` queries.</span></span>
- <span data-ttu-id="79db2-120">Asignación de las consultas definidas en el modelo.</span><span class="sxs-lookup"><span data-stu-id="79db2-120">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="79db2-121">Asignación de un tipo de consulta a una vista de base de datos se logra utilizando la ```ToTable``` API fluida.</span><span class="sxs-lookup"><span data-stu-id="79db2-121">Mapping a query type to a database view is achieved using the ```ToTable``` fluent API.</span></span>

## <a name="example"></a><span data-ttu-id="79db2-122">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="79db2-122">Example</span></span>

<span data-ttu-id="79db2-123">En el ejemplo siguiente se muestra cómo utilizar el tipo de consulta para una vista de base de datos de consulta.</span><span class="sxs-lookup"><span data-stu-id="79db2-123">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="79db2-124">Puede ver un [ejemplo](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="79db2-124">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="79db2-125">En primer lugar, definimos un modelo simple de Blog y Post:</span><span class="sxs-lookup"><span data-stu-id="79db2-125">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="79db2-126">A continuación, definimos una vista de base de datos simple que nos permitirá consultar el número de mensajes asociados a cada blog:</span><span class="sxs-lookup"><span data-stu-id="79db2-126">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="79db2-127">A continuación, se define una clase que contenga el resultado de la vista de base de datos:</span><span class="sxs-lookup"><span data-stu-id="79db2-127">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="79db2-128">A continuación, configuramos el tipo de consulta en _OnModelCreating_ con el ```modelBuilder.Query<T>``` API.</span><span class="sxs-lookup"><span data-stu-id="79db2-128">Next, we configure the query type in _OnModelCreating_ using the ```modelBuilder.Query<T>``` API.</span></span>
<span data-ttu-id="79db2-129">Configuración estándar fluent API se utiliza para configurar la asignación para el tipo de consulta:</span><span class="sxs-lookup"><span data-stu-id="79db2-129">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="79db2-130">Por último, podemos consultar la vista de base de datos de la forma estándar:</span><span class="sxs-lookup"><span data-stu-id="79db2-130">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="79db2-131">Tenga en cuenta también que hemos definido una propiedad de nivel de consulta (DbQuery) para que actúe como raíz para las consultas en este tipo de contexto.</span><span class="sxs-lookup"><span data-stu-id="79db2-131">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
