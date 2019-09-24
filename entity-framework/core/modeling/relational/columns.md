---
title: 'Asignación de columnas: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197204"
---
# <a name="column-mapping"></a><span data-ttu-id="0153c-102">Asignación de columnas</span><span class="sxs-lookup"><span data-stu-id="0153c-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="0153c-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="0153c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0153c-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="0153c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0153c-105">La asignación de columnas identifica qué datos de columna se deben consultar y guardar en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0153c-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="0153c-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="0153c-106">Conventions</span></span>

<span data-ttu-id="0153c-107">Por Convención, cada propiedad se configurará para asignarse a una columna con el mismo nombre que la propiedad.</span><span class="sxs-lookup"><span data-stu-id="0153c-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0153c-108">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="0153c-108">Data Annotations</span></span>

<span data-ttu-id="0153c-109">Puede usar anotaciones de datos para configurar la columna a la que está asignada una propiedad.</span><span class="sxs-lookup"><span data-stu-id="0153c-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="0153c-110">API fluida</span><span class="sxs-lookup"><span data-stu-id="0153c-110">Fluent API</span></span>

<span data-ttu-id="0153c-111">Puede usar la API fluida para configurar la columna a la que está asignada una propiedad.</span><span class="sxs-lookup"><span data-stu-id="0153c-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
