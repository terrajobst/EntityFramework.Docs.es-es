---
title: Claves (principal)-EF Core
description: Cómo configurar claves para tipos de entidad al utilizar Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824622"
---
# <a name="keys-primary"></a><span data-ttu-id="4a6df-103">Claves (principales)</span><span class="sxs-lookup"><span data-stu-id="4a6df-103">Keys (primary)</span></span>

<span data-ttu-id="4a6df-104">Una clave actúa como identificador único principal para cada instancia de entidad.</span><span class="sxs-lookup"><span data-stu-id="4a6df-104">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="4a6df-105">Al utilizar una base de datos relacional, se asigna al concepto de una *clave principal*.</span><span class="sxs-lookup"><span data-stu-id="4a6df-105">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="4a6df-106">También puede configurar un identificador único que no sea la clave principal (consulte [claves alternativas](alternate-keys.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="4a6df-106">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="4a6df-107">Se puede usar uno de los métodos siguientes para configurar o crear una clave principal.</span><span class="sxs-lookup"><span data-stu-id="4a6df-107">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="4a6df-108">Convenciones</span><span class="sxs-lookup"><span data-stu-id="4a6df-108">Conventions</span></span>

<span data-ttu-id="4a6df-109">De forma predeterminada, una propiedad denominada `Id` o `<type name>Id` se configurará como la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="4a6df-109">By default, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> <span data-ttu-id="4a6df-110">Los [tipos de entidad de propiedad](xref:core/modeling/owned-entities) usan reglas diferentes para definir claves.</span><span class="sxs-lookup"><span data-stu-id="4a6df-110">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4a6df-111">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="4a6df-111">Data Annotations</span></span>

<span data-ttu-id="4a6df-112">Puede usar anotaciones de datos para configurar una propiedad única para que sea la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="4a6df-112">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="4a6df-113">API fluida</span><span class="sxs-lookup"><span data-stu-id="4a6df-113">Fluent API</span></span>

<span data-ttu-id="4a6df-114">Puede usar la API fluida para configurar una propiedad única para que sea la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="4a6df-114">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="4a6df-115">También puede usar la API fluida para configurar varias propiedades de forma que sean la clave de una entidad (conocida como clave compuesta).</span><span class="sxs-lookup"><span data-stu-id="4a6df-115">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="4a6df-116">Las claves compuestas solo se pueden configurar mediante las convenciones de API fluidas nunca instalarán una clave compuesta y no se pueden usar anotaciones de datos para configurar una.</span><span class="sxs-lookup"><span data-stu-id="4a6df-116">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a><span data-ttu-id="4a6df-117">Tipos de clave y valores</span><span class="sxs-lookup"><span data-stu-id="4a6df-117">Key types and values</span></span>

<span data-ttu-id="4a6df-118">EF Core admite el uso de propiedades de cualquier tipo primitivo como clave principal, incluidos `string`, `Guid`, `byte[]` y otros.</span><span class="sxs-lookup"><span data-stu-id="4a6df-118">EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others.</span></span> <span data-ttu-id="4a6df-119">Pero no todas las bases de datos las admiten.</span><span class="sxs-lookup"><span data-stu-id="4a6df-119">But not all databases support them.</span></span> <span data-ttu-id="4a6df-120">En algunos casos, los valores de clave se pueden convertir automáticamente a un tipo compatible, de lo contrario, la conversión se debe [especificar manualmente](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="4a6df-120">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="4a6df-121">Las propiedades de clave deben tener siempre un valor no predeterminado al agregar una nueva entidad al contexto, pero [la base de datos generará](xref:core/modeling/generated-properties)algunos tipos.</span><span class="sxs-lookup"><span data-stu-id="4a6df-121">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="4a6df-122">En ese caso, EF intentará generar un valor temporal cuando la entidad se agregue con fines de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="4a6df-122">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="4a6df-123">Después de llamar a [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) , el valor temporal se reemplazará por el valor generado por la base de datos.</span><span class="sxs-lookup"><span data-stu-id="4a6df-123">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="4a6df-124">Si una propiedad de clave tiene el valor generado por la base de datos y se especifica un valor no predeterminado al agregar una entidad, EF asumirá que la entidad ya existe en la base de datos e intentará actualizarla en lugar de insertar una nueva.</span><span class="sxs-lookup"><span data-stu-id="4a6df-124">If a key property has value generated by the database and a non-default value is specified when an entity is added then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="4a6df-125">Para evitar esto, desactive la generación de valores o vea [Cómo especificar valores explícitos para las propiedades generadas](../saving/explicit-values-generated-properties.md).</span><span class="sxs-lookup"><span data-stu-id="4a6df-125">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>