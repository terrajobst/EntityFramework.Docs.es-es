---
title: Incluir & excluyendo propiedades-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197414"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="fa156-102">Incluir & excluyendo propiedades</span><span class="sxs-lookup"><span data-stu-id="fa156-102">Including & Excluding Properties</span></span>

<span data-ttu-id="fa156-103">La inclusión de una propiedad en el modelo significa que EF tiene metadatos sobre esa propiedad e intentará leer y escribir valores desde y hacia la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fa156-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="fa156-104">Convenciones</span><span class="sxs-lookup"><span data-stu-id="fa156-104">Conventions</span></span>

<span data-ttu-id="fa156-105">Por Convención, las propiedades públicas con un captador y un establecedor se incluirán en el modelo.</span><span class="sxs-lookup"><span data-stu-id="fa156-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="fa156-106">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="fa156-106">Data Annotations</span></span>

<span data-ttu-id="fa156-107">Puede usar anotaciones de datos para excluir una propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="fa156-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="fa156-108">API fluida</span><span class="sxs-lookup"><span data-stu-id="fa156-108">Fluent API</span></span>

<span data-ttu-id="fa156-109">Puede usar la API fluida para excluir una propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="fa156-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
