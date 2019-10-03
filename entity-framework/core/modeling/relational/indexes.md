---
title: Índices (base de datos relacional)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7bb74d0bfa6090b597eb988a46f00494e25f233e
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813630"
---
# <a name="indexes-relational-database"></a><span data-ttu-id="54305-102">Índices (base de datos relacional)</span><span class="sxs-lookup"><span data-stu-id="54305-102">Indexes (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="54305-103">La configuración de esta sección se aplica a bases de datos relacionales en general.</span><span class="sxs-lookup"><span data-stu-id="54305-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="54305-104">Los métodos de extensión que se muestran a continuación estarán disponibles cuando instale un proveedor de base de datos relacional (debido al paquete compartido *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="54305-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="54305-105">Un índice en una base de datos relacional se asigna al mismo concepto que un índice en el núcleo de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="54305-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="54305-106">Convenciones</span><span class="sxs-lookup"><span data-stu-id="54305-106">Conventions</span></span>

<span data-ttu-id="54305-107">Por Convención, los índices se denominan `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="54305-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="54305-108">En el caso de `<property name>` los índices compuestos se convierte en una lista de nombres de propiedad separados por guiones bajos.</span><span class="sxs-lookup"><span data-stu-id="54305-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="54305-109">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="54305-109">Data Annotations</span></span>

<span data-ttu-id="54305-110">Los índices no se pueden configurar con anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="54305-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="54305-111">API fluida</span><span class="sxs-lookup"><span data-stu-id="54305-111">Fluent API</span></span>

<span data-ttu-id="54305-112">Puede usar la API fluida para configurar el nombre de un índice.</span><span class="sxs-lookup"><span data-stu-id="54305-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="54305-113">También puede especificar un filtro.</span><span class="sxs-lookup"><span data-stu-id="54305-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="54305-114">Al usar el proveedor de SQL Server EF agrega un filtro ' IS NOT NULL ' para todas las columnas que aceptan valores NULL que forman parte de un índice único.</span><span class="sxs-lookup"><span data-stu-id="54305-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="54305-115">Para invalidar esta Convención, puede proporcionar `null` un valor.</span><span class="sxs-lookup"><span data-stu-id="54305-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="54305-116">Incluir columnas en índices de SQL Server</span><span class="sxs-lookup"><span data-stu-id="54305-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="54305-117">Puede configurar [índices con columnas incluidas](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) para mejorar significativamente el rendimiento de las consultas cuando todas las columnas de la consulta se incluyen en el índice como columnas de clave o que no son de clave.</span><span class="sxs-lookup"><span data-stu-id="54305-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
