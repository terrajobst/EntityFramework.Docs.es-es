---
title: Filtros de consulta global - Core EF
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a><span data-ttu-id="83571-102">Filtros de consulta global</span><span class="sxs-lookup"><span data-stu-id="83571-102">Global Query Filters</span></span>

<span data-ttu-id="83571-103">Los filtros de consulta global son predicados de consulta LINQ (normalmente se pasa una expresión booleana a LINQ *donde* operador de consulta) aplicado a tipos de entidad en el modelo de metadatos (normalmente en *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="83571-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="83571-104">Estos filtros se aplican automáticamente a las consultas LINQ que afectan a los tipos de entidad, incluidas las referencias de propiedad de navegación directa o indirectamente, como hace referencia mediante el uso de inclusión de tipos de entidad.</span><span class="sxs-lookup"><span data-stu-id="83571-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="83571-105">Algunas aplicaciones comunes de esta característica son:</span><span class="sxs-lookup"><span data-stu-id="83571-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="83571-106">**Eliminar temporalmente** -un tipo de entidad define una *IsDeleted* propiedad.</span><span class="sxs-lookup"><span data-stu-id="83571-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="83571-107">**La arquitectura multiempresa** -un tipo de entidad define una *TenantId* propiedad.</span><span class="sxs-lookup"><span data-stu-id="83571-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="83571-108">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="83571-108">Example</span></span>

<span data-ttu-id="83571-109">En el ejemplo siguiente se muestra cómo usar filtros de consulta Global para implementar comportamientos de la consulta de eliminación de software y la arquitectura multiempresa en un modelo de creación de blogs simple.</span><span class="sxs-lookup"><span data-stu-id="83571-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="83571-110">Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="83571-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="83571-111">En primer lugar, defina las entidades:</span><span class="sxs-lookup"><span data-stu-id="83571-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="83571-112">Tenga en cuenta la declaración de un __tenantId_ campo el _Blog_ entidad.</span><span class="sxs-lookup"><span data-stu-id="83571-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="83571-113">Esto se utilizará para asociar cada instancia de Blog específicos del inquilino.</span><span class="sxs-lookup"><span data-stu-id="83571-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="83571-114">También es un _IsDeleted_ propiedad en el _Post_ tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="83571-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="83571-115">Se utiliza esta opción para realizar un seguimiento de si una _Post_ instancia ha estado "eliminado".</span><span class="sxs-lookup"><span data-stu-id="83571-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="83571-116">Es decir La instancia se marca como eliminado reemplazar quitar físicamente los datos subyacentes.</span><span class="sxs-lookup"><span data-stu-id="83571-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="83571-117">A continuación, configure los filtros de consulta de _OnModelCreating_ mediante el ```HasQueryFilter``` API.</span><span class="sxs-lookup"><span data-stu-id="83571-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="83571-118">Las expresiones de predicado que se pasan a la _HasQueryFilter_ llamadas ahora se aplicarán automáticamente a las consultas LINQ para esos tipos.</span><span class="sxs-lookup"><span data-stu-id="83571-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="83571-119">Tenga en cuenta el uso de un campo de nivel de instancia de DbContext: ```_tenantId``` usa para establecer el inquilino actual.</span><span class="sxs-lookup"><span data-stu-id="83571-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="83571-120">Filtros de nivel de modelo usará el valor de la instancia de contexto correcto.</span><span class="sxs-lookup"><span data-stu-id="83571-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="83571-121">Es decir La instancia que se está ejecutando la consulta.</span><span class="sxs-lookup"><span data-stu-id="83571-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="83571-122">Deshabilitar filtros</span><span class="sxs-lookup"><span data-stu-id="83571-122">Disabling Filters</span></span>

<span data-ttu-id="83571-123">Los filtros pueden deshabilitarse para consultas LINQ individuales mediante el uso de la ```IgnoreQueryFilters()``` operador.</span><span class="sxs-lookup"><span data-stu-id="83571-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="83571-124">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="83571-124">Limitations</span></span>

<span data-ttu-id="83571-125">Los filtros de consulta global tienen las siguientes limitaciones:</span><span class="sxs-lookup"><span data-stu-id="83571-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="83571-126">Los filtros no pueden contener referencias a las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="83571-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="83571-127">Solo se pueden definir filtros para la raíz del tipo de entidad de una jerarquía de herencia.</span><span class="sxs-lookup"><span data-stu-id="83571-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
