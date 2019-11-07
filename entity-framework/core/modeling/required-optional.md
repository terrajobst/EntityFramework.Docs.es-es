---
title: 'Propiedades obligatorias y opcionales: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 62b2b3f5a761c0aacece986ecd0b2dd2f958d048
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655665"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="09fcb-102">Propiedades obligatorias y opcionales</span><span class="sxs-lookup"><span data-stu-id="09fcb-102">Required and Optional Properties</span></span>

<span data-ttu-id="09fcb-103">Una propiedad se considera opcional si es válida para que contenga `null`.</span><span class="sxs-lookup"><span data-stu-id="09fcb-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="09fcb-104">Si `null` no es un valor válido que se va a asignar a una propiedad, se considera que es una propiedad obligatoria.</span><span class="sxs-lookup"><span data-stu-id="09fcb-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

<span data-ttu-id="09fcb-105">Al asignar a un esquema de base de datos relacional, las propiedades requeridas se crean como columnas que no aceptan valores NULL y las propiedades opcionales se crean como columnas que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="09fcb-105">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

## <a name="conventions"></a><span data-ttu-id="09fcb-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="09fcb-106">Conventions</span></span>

<span data-ttu-id="09fcb-107">Por Convención, una propiedad cuyo tipo .NET pueda contener NULL se configurará como opcional, mientras que las propiedades cuyo tipo .NET no puede contener valores NULL se configurarán según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="09fcb-107">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="09fcb-108">Por ejemplo, todas las propiedades con tipos de valor .NET (`int`, `decimal`, `bool`, etc.) se configuran según sea necesario y todas las propiedades con tipos de valor .NET que aceptan valores NULL (`int?`, `decimal?`, `bool?`, etc.) se configuran como opcionales.</span><span class="sxs-lookup"><span data-stu-id="09fcb-108">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="09fcb-109">C#8 presentó una nueva característica denominada [tipos de referencia que aceptan valores NULL](/dotnet/csharp/tutorials/nullable-reference-types), que permite anotar tipos de referencia, lo que indica si es válido para que contengan null o not.</span><span class="sxs-lookup"><span data-stu-id="09fcb-109">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="09fcb-110">Esta característica está deshabilitada de forma predeterminada y, si está habilitada, modifica el comportamiento de EF Core de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="09fcb-110">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="09fcb-111">Si los tipos de referencia que aceptan valores NULL están deshabilitados (el valor predeterminado), todas las propiedades con tipos de referencia de .NET se configuran como opcionales por Convención (por ejemplo, `string`).</span><span class="sxs-lookup"><span data-stu-id="09fcb-111">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="09fcb-112">Si los tipos de referencia que aceptan valores NULL están habilitados, las propiedades C# se configurarán en función de la nulabilidad de su tipo .net: `string?` se configurarán como opcionales, mientras que `string` se configurarán según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="09fcb-112">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="09fcb-113">En el ejemplo siguiente se muestra un tipo de entidad con propiedades obligatorias y opcionales, con la característica de referencia que acepta valores NULL deshabilitada (valor predeterminado) y habilitada:</span><span class="sxs-lookup"><span data-stu-id="09fcb-113">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="09fcb-114">Sin tipos de referencia que aceptan valores NULL (valor predeterminado)</span><span class="sxs-lookup"><span data-stu-id="09fcb-114">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="09fcb-115">Con tipos de referencia que aceptan valores NULL</span><span class="sxs-lookup"><span data-stu-id="09fcb-115">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="09fcb-116">Se recomienda el uso de tipos de referencia que aceptan valores NULL, ya que C# fluye la nulabilidad expresada en el código para EF Core modelo y en la base de datos, e obvia el uso de las anotaciones de datos o la API fluida para expresar el mismo concepto dos veces.</span><span class="sxs-lookup"><span data-stu-id="09fcb-116">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="09fcb-117">Tenga cuidado al habilitar los tipos de referencia que aceptan valores NULL en un proyecto existente: las propiedades de tipo de referencia que se configuraron anteriormente como opcional ahora se configurarán según sea necesario, a menos que se anoten explícitamente para que acepten valores NULL.</span><span class="sxs-lookup"><span data-stu-id="09fcb-117">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="09fcb-118">Al administrar un esquema de base de datos relacional, esto puede provocar que se generen migraciones que modifiquen la nulabilidad de la columna de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="09fcb-118">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="09fcb-119">Para obtener más información sobre los tipos de referencia que aceptan valores NULL y cómo usarlos con EF Core, [consulte la página de documentación dedicada para esta característica](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="09fcb-119">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

## <a name="configuration"></a><span data-ttu-id="09fcb-120">Configuración</span><span class="sxs-lookup"><span data-stu-id="09fcb-120">Configuration</span></span>

<span data-ttu-id="09fcb-121">Una propiedad que sería opcional por Convención se puede configurar para que sea necesaria de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="09fcb-121">A property that would be optional by convention can be configured to be required as follows:</span></span>

# <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="09fcb-122">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="09fcb-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="09fcb-123">API fluida</span><span class="sxs-lookup"><span data-stu-id="09fcb-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***
