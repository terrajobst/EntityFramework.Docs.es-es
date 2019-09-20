---
title: 'Propiedades obligatorias y opcionales: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149148"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="e1898-102">Propiedades obligatorias y opcionales</span><span class="sxs-lookup"><span data-stu-id="e1898-102">Required and Optional Properties</span></span>

<span data-ttu-id="e1898-103">Una propiedad se considera opcional si es válida para que la contenga `null`.</span><span class="sxs-lookup"><span data-stu-id="e1898-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="e1898-104">Si `null` no es un valor válido que se va a asignar a una propiedad, se considera que es una propiedad obligatoria.</span><span class="sxs-lookup"><span data-stu-id="e1898-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="e1898-105">Convenciones</span><span class="sxs-lookup"><span data-stu-id="e1898-105">Conventions</span></span>

<span data-ttu-id="e1898-106">Por Convención, una propiedad cuyo tipo .net pueda contener NULL se configurará como opcional`string`( `int?`, `byte[]`,, etc.).</span><span class="sxs-lookup"><span data-stu-id="e1898-106">By convention, a property whose .NET type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="e1898-107">Las propiedades cuyo tipo CLR no puede contener valores NULL se configurarán `decimal`como `bool`Required (`int`,,, etc.).</span><span class="sxs-lookup"><span data-stu-id="e1898-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="e1898-108">Una propiedad cuyo tipo .NET no puede contener valores NULL no se puede configurar como opcional.</span><span class="sxs-lookup"><span data-stu-id="e1898-108">A property whose .NET type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="e1898-109">La propiedad siempre será considerada requerida por Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e1898-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e1898-110">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="e1898-110">Data Annotations</span></span>

<span data-ttu-id="e1898-111">Puede usar anotaciones de datos para indicar que una propiedad es obligatoria.</span><span class="sxs-lookup"><span data-stu-id="e1898-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="e1898-112">API fluida</span><span class="sxs-lookup"><span data-stu-id="e1898-112">Fluent API</span></span>

<span data-ttu-id="e1898-113">Puede usar la API fluida para indicar que se requiere una propiedad.</span><span class="sxs-lookup"><span data-stu-id="e1898-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

