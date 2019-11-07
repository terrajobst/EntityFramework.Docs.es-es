---
title: Claves (principal)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655947"
---
# <a name="keys-primary"></a><span data-ttu-id="44250-102">Claves (principales)</span><span class="sxs-lookup"><span data-stu-id="44250-102">Keys (primary)</span></span>

<span data-ttu-id="44250-103">Una clave actúa como identificador único principal para cada instancia de entidad.</span><span class="sxs-lookup"><span data-stu-id="44250-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="44250-104">Al utilizar una base de datos relacional, se asigna al concepto de una *clave principal*.</span><span class="sxs-lookup"><span data-stu-id="44250-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="44250-105">También puede configurar un identificador único que no sea la clave principal (consulte [claves alternativas](alternate-keys.md) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="44250-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="44250-106">Se puede usar uno de los métodos siguientes para configurar o crear una clave principal.</span><span class="sxs-lookup"><span data-stu-id="44250-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="44250-107">Convenciones</span><span class="sxs-lookup"><span data-stu-id="44250-107">Conventions</span></span>

<span data-ttu-id="44250-108">Por Convención, una propiedad denominada `Id` o `<type name>Id` se configurará como la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="44250-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a><span data-ttu-id="44250-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="44250-109">Data Annotations</span></span>

<span data-ttu-id="44250-110">Puede usar anotaciones de datos para configurar una propiedad única para que sea la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="44250-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="44250-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="44250-111">Fluent API</span></span>

<span data-ttu-id="44250-112">Puede usar la API fluida para configurar una propiedad única para que sea la clave de una entidad.</span><span class="sxs-lookup"><span data-stu-id="44250-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="44250-113">También puede usar la API fluida para configurar varias propiedades de forma que sean la clave de una entidad (conocida como clave compuesta).</span><span class="sxs-lookup"><span data-stu-id="44250-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="44250-114">Las claves compuestas solo se pueden configurar mediante las convenciones de API fluidas nunca instalarán una clave compuesta y no se pueden usar anotaciones de datos para configurar una.</span><span class="sxs-lookup"><span data-stu-id="44250-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
