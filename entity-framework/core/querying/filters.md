---
title: 'Filtros de consulta global: EF Core'
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 73efe62262cf45cc1841d7a86cf59249cf07c5ea
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996670"
---
# <a name="global-query-filters"></a><span data-ttu-id="b2bb9-102">Filtros de consulta global</span><span class="sxs-lookup"><span data-stu-id="b2bb9-102">Global Query Filters</span></span>

<span data-ttu-id="b2bb9-103">Los filtros de consulta global son predicados de consulta LINQ (una expresión booleana que habitualmente se pasa al operador de consulta LINQ *Where*) aplicados a los tipos de entidad del modelo de metadatos (habitualmente en *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="b2bb9-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="b2bb9-104">Estos filtros se aplican automáticamente a las consultas LINQ que implican a esos tipos de entidad, incluidos aquellos a los que se hace referencia de forma indirecta, por ejemplo mediante el uso de Include o de referencias de propiedad de navegación directas.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="b2bb9-105">Algunas aplicaciones comunes de esta característica son:</span><span class="sxs-lookup"><span data-stu-id="b2bb9-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="b2bb9-106">**Eliminación temporal**: un tipo de entidad define una propiedad *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="b2bb9-107">**Servicios multiinquilino**: un tipo de entidad define una propiedad *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="b2bb9-108">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b2bb9-108">Example</span></span>

<span data-ttu-id="b2bb9-109">En el ejemplo siguiente se muestra cómo usar los filtros de consulta global para implementar los comportamientos de consulta de multiinquilino y de eliminación temporal en un simple modelo de creación de blogs.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="b2bb9-110">Puede ver un [ejemplo](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="b2bb9-111">En primer lugar, defina las entidades:</span><span class="sxs-lookup"><span data-stu-id="b2bb9-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="b2bb9-112">Tenga en cuenta la declaración de un campo __tenantId_ en la entidad _Blog_.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="b2bb9-113">Se usará para asociar cada instancia de blog con un inquilino específico.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="b2bb9-114">También hay definida una propiedad _IsDeleted_ en el tipo de entidad _Post_.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="b2bb9-115">Se usa para llevar un seguimiento de si una instancia _Post_ se eliminó de manera temporal.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-115">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="b2bb9-116">Es decir, la instancia se marca como eliminada sin quitar físicamente los datos subyacentes.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-116">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="b2bb9-117">A continuación, configure los filtros de consulta en _OnModelCreating_ con la API ```HasQueryFilter```.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="b2bb9-118">Las expresiones de predicado pasadas a las llamadas de _HasQueryFilter_ ahora se aplicarán automáticamente a cualquier consulta LINQ para esos tipos.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="b2bb9-119">Tenga en cuenta el uso de un campo en el nivel de instancia de DbContext: ```_tenantId``` se usa para establecer el inquilino actual.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="b2bb9-120">Los filtros de nivel de modelo usan el valor de la instancia de contexto correcta (es decir, la instancia que está ejecutando la consulta).</span><span class="sxs-lookup"><span data-stu-id="b2bb9-120">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="b2bb9-121">Deshabilitación de filtros</span><span class="sxs-lookup"><span data-stu-id="b2bb9-121">Disabling Filters</span></span>

<span data-ttu-id="b2bb9-122">Los filtros se pueden deshabilitar para consultas LINQ individuales mediante el operador ```IgnoreQueryFilters()```.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-122">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="b2bb9-123">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="b2bb9-123">Limitations</span></span>

<span data-ttu-id="b2bb9-124">Los filtros de consulta global tienen las limitaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="b2bb9-124">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="b2bb9-125">Los filtros no pueden contener referencias a las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-125">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="b2bb9-126">Solo se pueden definir filtros para el tipo de entidad raíz de una jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="b2bb9-126">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
