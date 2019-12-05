---
title: Opciones de Azure SQL Database de proveedor de base de datos Microsoft SQL Server-EF Core
description: Cómo especificar el nivel de servicio y de rendimiento para Azure SQL Database con el proveedor de base de datos de SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/azure-sql-database
ms.openlocfilehash: c4f7b91110a0e700ed06130661e611cf45bee05f
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824790"
---
# <a name="specifying-azure-sql-database-options"></a><span data-ttu-id="5d603-103">Especificar opciones de Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5d603-103">Specifying Azure SQL Database Options</span></span>

>[!NOTE]
> <span data-ttu-id="5d603-104">Esta API es nueva en EF Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="5d603-104">This API is new in EF Core 3.1.</span></span>

<span data-ttu-id="5d603-105">Azure SQL Database proporciona [una variedad de opciones de precios](https://azure.microsoft.com/pricing/details/sql-database/single/) que normalmente se configuran a través de Azure portal.</span><span class="sxs-lookup"><span data-stu-id="5d603-105">Azure SQL Database provides [a variety of pricing options](https://azure.microsoft.com/pricing/details/sql-database/single/) that are usually configured through the Azure Portal.</span></span> <span data-ttu-id="5d603-106">Sin embargo, si administra el esquema mediante [migraciones de EF Core](xref:core/managing-schemas/migrations/index) , puede especificar las opciones deseadas en el propio modelo.</span><span class="sxs-lookup"><span data-stu-id="5d603-106">However if you are managing the schema using [EF Core migrations](xref:core/managing-schemas/migrations/index) you can specify the desired options in the model itself.</span></span>

<span data-ttu-id="5d603-107">Puede especificar el nivel de servicio de la base de datos (edición) mediante [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span><span class="sxs-lookup"><span data-stu-id="5d603-107">You can specify the service tier of the database (EDITION) using [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span></span>

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

<span data-ttu-id="5d603-108">Puede especificar el tamaño máximo de la base de datos mediante [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span><span class="sxs-lookup"><span data-stu-id="5d603-108">You can specify the maximum size of the database using [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span></span>

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

<span data-ttu-id="5d603-109">Puede especificar el nivel de rendimiento de la base de datos (SERVICE_OBJECTIVE) mediante [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span><span class="sxs-lookup"><span data-stu-id="5d603-109">You can specify the performance level of the database (SERVICE_OBJECTIVE) using [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

<span data-ttu-id="5d603-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) para configurar el grupo elástico, ya que el valor no es un literal de cadena:</span><span class="sxs-lookup"><span data-stu-id="5d603-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) to configure the elastic pool, since the value is not a string literal:</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> <span data-ttu-id="5d603-111">Puede encontrar todos los valores admitidos en la [documentación de ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span><span class="sxs-lookup"><span data-stu-id="5d603-111">You can find all the supported values in the [ALTER DATABASE documentation](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span></span>