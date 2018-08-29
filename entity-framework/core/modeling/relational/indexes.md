---
title: Índices - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993222"
---
# <a name="indexes"></a><span data-ttu-id="4d829-102">Índices</span><span class="sxs-lookup"><span data-stu-id="4d829-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="4d829-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="4d829-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="4d829-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="4d829-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="4d829-105">Un índice en una base de datos relacional se asigna en el mismo concepto que un índice en el núcleo de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4d829-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="4d829-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="4d829-106">Conventions</span></span>

<span data-ttu-id="4d829-107">Por convención, se denominan índices `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="4d829-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="4d829-108">Para los índices compuestos `<property name>` se convierte en una lista separada por un carácter de subrayado de los nombres de propiedad.</span><span class="sxs-lookup"><span data-stu-id="4d829-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4d829-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="4d829-109">Data Annotations</span></span>

<span data-ttu-id="4d829-110">Los índices no se pueden configurar mediante las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="4d829-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="4d829-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="4d829-111">Fluent API</span></span>

<span data-ttu-id="4d829-112">Puede usar la API Fluent para configurar el nombre de un índice.</span><span class="sxs-lookup"><span data-stu-id="4d829-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="4d829-113">También puede especificar un filtro.</span><span class="sxs-lookup"><span data-stu-id="4d829-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="4d829-114">Cuando se agrega mediante el proveedor de SQL Server EF filtrar un 'IS NOT NULL' para todas las columnas que aceptan valores null que forman parte de un índice único.</span><span class="sxs-lookup"><span data-stu-id="4d829-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="4d829-115">Para invalidar esta convención puede proporcionar un `null` valor.</span><span class="sxs-lookup"><span data-stu-id="4d829-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
