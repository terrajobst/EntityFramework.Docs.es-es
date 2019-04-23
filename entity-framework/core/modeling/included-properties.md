---
title: Inclusión y exclusión de propiedades - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929830"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="75b3c-102">Inclusión y exclusión de propiedades</span><span class="sxs-lookup"><span data-stu-id="75b3c-102">Including & Excluding Properties</span></span>

<span data-ttu-id="75b3c-103">Incluye una propiedad en el modelo, significa que EF tiene metadatos sobre esa propiedad e intentará leer y escribir valores desde y hacia la base de datos.</span><span class="sxs-lookup"><span data-stu-id="75b3c-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="75b3c-104">Convenciones</span><span class="sxs-lookup"><span data-stu-id="75b3c-104">Conventions</span></span>

<span data-ttu-id="75b3c-105">Por convención, las propiedades públicas con un captador y un establecedor se incluirán en el modelo.</span><span class="sxs-lookup"><span data-stu-id="75b3c-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="75b3c-106">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="75b3c-106">Data Annotations</span></span>

<span data-ttu-id="75b3c-107">Puede usar anotaciones de datos para excluir una propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="75b3c-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="75b3c-108">API fluida</span><span class="sxs-lookup"><span data-stu-id="75b3c-108">Fluent API</span></span>

<span data-ttu-id="75b3c-109">Puede usar la API Fluent para excluir una propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="75b3c-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
