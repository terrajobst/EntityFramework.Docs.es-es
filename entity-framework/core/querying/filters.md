---
title: 'Filtros de consulta global: EF Core'
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 4afc9fb0338d34845639d57013ac710445321940
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562447"
---
# <a name="global-query-filters"></a><span data-ttu-id="0ce4d-102">Filtros de consulta global</span><span class="sxs-lookup"><span data-stu-id="0ce4d-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="0ce4d-103">Esta característica se incluyó por primera vez en EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="0ce4d-104">Los filtros de consulta global son predicados de consulta LINQ (una expresión booleana que habitualmente se pasa al operador de consulta LINQ *Where*) aplicados a los tipos de entidad del modelo de metadatos (habitualmente en *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="0ce4d-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="0ce4d-105">Estos filtros se aplican automáticamente a las consultas LINQ que implican a esos tipos de entidad, incluidos aquellos a los que se hace referencia de forma indirecta, por ejemplo mediante el uso de Include o de referencias de propiedad de navegación directas.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="0ce4d-106">Algunas aplicaciones comunes de esta característica son:</span><span class="sxs-lookup"><span data-stu-id="0ce4d-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="0ce4d-107">**Eliminación temporal**: un tipo de entidad define una propiedad *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="0ce4d-108">**Servicios multiinquilino**: un tipo de entidad define una propiedad *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="0ce4d-109">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="0ce4d-109">Example</span></span>

<span data-ttu-id="0ce4d-110">En el ejemplo siguiente se muestra cómo usar los filtros de consulta global para implementar los comportamientos de consulta de multiinquilino y de eliminación temporal en un simple modelo de creación de blogs.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="0ce4d-111">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="0ce4d-112">En primer lugar, defina las entidades:</span><span class="sxs-lookup"><span data-stu-id="0ce4d-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="0ce4d-113">Tenga en cuenta la declaración de un campo __tenantId_ en la entidad _Blog_.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-113">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="0ce4d-114">Se usará para asociar cada instancia de blog con un inquilino específico.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="0ce4d-115">También hay definida una propiedad _IsDeleted_ en el tipo de entidad _Post_.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="0ce4d-116">Se usa para llevar un seguimiento de si una instancia _Post_ se eliminó de manera temporal.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="0ce4d-117">Es decir, la instancia se marca como eliminada sin quitar físicamente los datos subyacentes.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="0ce4d-118">A continuación, configure los filtros de consulta en _OnModelCreating_ con la API ```HasQueryFilter```.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-118">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="0ce4d-119">Las expresiones de predicado pasadas a las llamadas de _HasQueryFilter_ ahora se aplicarán automáticamente a cualquier consulta LINQ para esos tipos.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="0ce4d-120">Tenga en cuenta el uso de un campo en el nivel de instancia de DbContext: ```_tenantId``` se usa para establecer el inquilino actual.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-120">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="0ce4d-121">Los filtros de nivel de modelo usan el valor de la instancia de contexto correcta (es decir, la instancia que está ejecutando la consulta).</span><span class="sxs-lookup"><span data-stu-id="0ce4d-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="0ce4d-122">Deshabilitación de filtros</span><span class="sxs-lookup"><span data-stu-id="0ce4d-122">Disabling Filters</span></span>

<span data-ttu-id="0ce4d-123">Los filtros se pueden deshabilitar para consultas LINQ individuales mediante el operador ```IgnoreQueryFilters()```.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="0ce4d-124">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="0ce4d-124">Limitations</span></span>

<span data-ttu-id="0ce4d-125">Los filtros de consulta global tienen las limitaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0ce4d-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="0ce4d-126">Los filtros no pueden contener referencias a las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="0ce4d-127">Solo se pueden definir filtros para el tipo de entidad raíz de una jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="0ce4d-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
