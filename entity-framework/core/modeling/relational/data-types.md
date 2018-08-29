---
title: Tipos de datos - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993526"
---
# <a name="data-types"></a><span data-ttu-id="64cf6-102">Tipos de datos</span><span class="sxs-lookup"><span data-stu-id="64cf6-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="64cf6-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="64cf6-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="64cf6-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="64cf6-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="64cf6-105">Tipo de datos hace referencia al tipo específico de base de datos de la columna a la que se asigna una propiedad.</span><span class="sxs-lookup"><span data-stu-id="64cf6-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="64cf6-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="64cf6-106">Conventions</span></span>

<span data-ttu-id="64cf6-107">Por convención, el proveedor de base de datos selecciona un tipo de datos en función del tipo CLR de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="64cf6-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="64cf6-108">También tiene en cuenta otros metadatos, como los configurados [longitud máxima](../max-length.md), si la propiedad forma parte de una clave principal, etcetera.</span><span class="sxs-lookup"><span data-stu-id="64cf6-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="64cf6-109">Por ejemplo, SQL Server usa `datetime2(7)` para `DateTime` propiedades, y `nvarchar(max)` para `string` propiedades (o `nvarchar(450)` para `string` propiedades que se usan como clave).</span><span class="sxs-lookup"><span data-stu-id="64cf6-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="64cf6-110">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="64cf6-110">Data Annotations</span></span>

<span data-ttu-id="64cf6-111">Puede usar anotaciones de datos para especificar un tipo de datos exactos para una columna.</span><span class="sxs-lookup"><span data-stu-id="64cf6-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="64cf6-112">Por ejemplo, el siguiente código configura `Url` como una cadena no unicode con una longitud máxima de `200` y `Rating` como decimal con una precisión de `5` y escalar de `2`.</span><span class="sxs-lookup"><span data-stu-id="64cf6-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="64cf6-113">API fluida</span><span class="sxs-lookup"><span data-stu-id="64cf6-113">Fluent API</span></span>

<span data-ttu-id="64cf6-114">También puede usar la API Fluent para especificar los mismos tipos de datos para las columnas.</span><span class="sxs-lookup"><span data-stu-id="64cf6-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
