---
title: Proveedor de base de datos de Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 1e75bc4bf334b1a60d13a2ec9ef314e3afcf0273
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812101"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="6a3a0-102">Proveedor de base de datos de Microsoft SQL Server para EF Core</span><span class="sxs-lookup"><span data-stu-id="6a3a0-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="6a3a0-103">Este proveedor de base de datos permite usar Entity Framework Core con Microsoft SQL Server (incluido SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="6a3a0-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="6a3a0-104">Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="6a3a0-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="6a3a0-105">Instalar</span><span class="sxs-lookup"><span data-stu-id="6a3a0-105">Install</span></span>

<span data-ttu-id="6a3a0-106">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="6a3a0-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="6a3a0-107">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="6a3a0-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="6a3a0-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a3a0-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="6a3a0-109">A partir de la versión 3.0.0, el proveedor hace referencia a Microsoft.Data.SqlClient (las versiones anteriores dependían de System.Data.SqlClient).</span><span class="sxs-lookup"><span data-stu-id="6a3a0-109">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="6a3a0-110">Si su proyecto depende directamente de SqlClient, asegúrese de que haga referencia al paquete correcto.</span><span class="sxs-lookup"><span data-stu-id="6a3a0-110">If your project takes a direct dependency on SqlClient, make sure it references the correct package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="6a3a0-111">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="6a3a0-111">Supported Database Engines</span></span>

* <span data-ttu-id="6a3a0-112">Microsoft SQL Server (de 2012 en adelante)</span><span class="sxs-lookup"><span data-stu-id="6a3a0-112">Microsoft SQL Server (2012 onwards)</span></span>
