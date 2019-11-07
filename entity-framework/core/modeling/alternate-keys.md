---
title: 'Claves alternativas: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655771"
---
# <a name="alternate-keys"></a><span data-ttu-id="a48a5-102">Claves alternativas</span><span class="sxs-lookup"><span data-stu-id="a48a5-102">Alternate Keys</span></span>

<span data-ttu-id="a48a5-103">Una clave alternativa sirve como identificador único alternativo para cada instancia de entidad, además de la clave principal.</span><span class="sxs-lookup"><span data-stu-id="a48a5-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="a48a5-104">Las claves alternativas se pueden usar como destino de una relación.</span><span class="sxs-lookup"><span data-stu-id="a48a5-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="a48a5-105">Al utilizar una base de datos relacional, se asigna al concepto de un índice o una restricción únicos en las columnas de clave alternativas y una o varias restricciones de clave externa que hacen referencia a las columnas.</span><span class="sxs-lookup"><span data-stu-id="a48a5-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="a48a5-106">Si solo desea exigir la unicidad de una columna, querrá un índice único en lugar de una clave alternativa, consulte [índices](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="a48a5-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="a48a5-107">En EF, las claves alternativas proporcionan una mayor funcionalidad que los índices únicos porque se pueden usar como destino de una clave externa.</span><span class="sxs-lookup"><span data-stu-id="a48a5-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="a48a5-108">Normalmente, se introducen claves alternativas cuando sea necesario y no es necesario configurarlas manualmente.</span><span class="sxs-lookup"><span data-stu-id="a48a5-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="a48a5-109">Vea [convenciones](#conventions) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="a48a5-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="a48a5-110">Convenciones</span><span class="sxs-lookup"><span data-stu-id="a48a5-110">Conventions</span></span>

<span data-ttu-id="a48a5-111">Por Convención, se introduce una clave alternativa cuando se identifica una propiedad, que no es la clave principal, como destino de una relación.</span><span class="sxs-lookup"><span data-stu-id="a48a5-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a><span data-ttu-id="a48a5-112">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="a48a5-112">Data Annotations</span></span>

<span data-ttu-id="a48a5-113">Las claves alternativas no se pueden configurar con anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="a48a5-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a48a5-114">API fluida</span><span class="sxs-lookup"><span data-stu-id="a48a5-114">Fluent API</span></span>

<span data-ttu-id="a48a5-115">Puede usar la API fluida para configurar una sola propiedad para que sea una clave alternativa.</span><span class="sxs-lookup"><span data-stu-id="a48a5-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

<span data-ttu-id="a48a5-116">También puede usar la API fluida para configurar varias propiedades de forma que sean una clave alternativa (conocida como clave alternativa compuesta).</span><span class="sxs-lookup"><span data-stu-id="a48a5-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
