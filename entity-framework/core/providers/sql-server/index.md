---
title: Proveedor de base de datos de Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: f0aa290e8c5166c278f8c9782c4304de5e91f26b
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813503"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="4191e-102">Proveedor de base de datos de Microsoft SQL Server para EF Core</span><span class="sxs-lookup"><span data-stu-id="4191e-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="4191e-103">Este proveedor de base de datos permite usar Entity Framework Core con Microsoft SQL Server (incluido SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="4191e-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="4191e-104">Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="4191e-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="4191e-105">Instalar</span><span class="sxs-lookup"><span data-stu-id="4191e-105">Install</span></span>

<span data-ttu-id="4191e-106">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="4191e-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="4191e-107">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="4191e-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="4191e-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4191e-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="4191e-109">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="4191e-109">Supported Database Engines</span></span>

* <span data-ttu-id="4191e-110">Microsoft SQL Server (de 2012 en adelante)</span><span class="sxs-lookup"><span data-stu-id="4191e-110">Microsoft SQL Server (2012 onwards)</span></span>
