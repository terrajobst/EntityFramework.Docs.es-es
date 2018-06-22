---
title: Tipos de datos - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053505"
---
# <a name="data-types"></a><span data-ttu-id="62178-102">Tipos de datos</span><span class="sxs-lookup"><span data-stu-id="62178-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="62178-103">La configuración de esta sección es aplicable a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="62178-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="62178-104">Los métodos de extensión que se muestra a continuación pasará a estar disponibles cuando se instala un proveedor de base de datos relacional (porque el recurso compartido *Microsoft.EntityFrameworkCore.Relational* paquete).</span><span class="sxs-lookup"><span data-stu-id="62178-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="62178-105">Tipo de datos hace referencia para el tipo específico de la base de datos de la columna a la que está asignada una propiedad.</span><span class="sxs-lookup"><span data-stu-id="62178-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="62178-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="62178-106">Conventions</span></span>

<span data-ttu-id="62178-107">Por convención, el proveedor de base de datos selecciona un tipo de datos basado en el tipo CLR de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="62178-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="62178-108">También tiene en cuenta otros metadatos, como los configurados [longitud máxima](../max-length.md), si la propiedad forma parte de una clave principal, etcetera.</span><span class="sxs-lookup"><span data-stu-id="62178-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="62178-109">Por ejemplo, SQL Server usa `datetime2(7)` para `DateTime` propiedades, y `nvarchar(max)` para `string` propiedades (o `nvarchar(450)` para `string` propiedades que se utilizan como una clave).</span><span class="sxs-lookup"><span data-stu-id="62178-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="62178-110">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="62178-110">Data Annotations</span></span>

<span data-ttu-id="62178-111">Puede usar anotaciones de datos para especificar un tipo de datos exacto para una columna.</span><span class="sxs-lookup"><span data-stu-id="62178-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="62178-112">Por ejemplo, el siguiente código configura `Url` como una cadena no unicode con una longitud máxima de `200` y `Rating` como decimal con una precisión de `5` y escalar de `2`.</span><span class="sxs-lookup"><span data-stu-id="62178-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="62178-113">API fluida</span><span class="sxs-lookup"><span data-stu-id="62178-113">Fluent API</span></span>

<span data-ttu-id="62178-114">También puede utilizar la API fluida para especificar los mismos tipos de datos para las columnas.</span><span class="sxs-lookup"><span data-stu-id="62178-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
