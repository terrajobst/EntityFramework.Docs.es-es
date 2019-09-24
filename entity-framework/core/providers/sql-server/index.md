---
title: Proveedor de base de datos de Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 70e7f3d4bc1f57f1b23d9b3e0bd6264236ddbd27
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149197"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="90032-102">Proveedor de base de datos de Microsoft SQL Server para EF Core</span><span class="sxs-lookup"><span data-stu-id="90032-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="90032-103">Este proveedor de base de datos permite usar Entity Framework Core con Microsoft SQL Server (incluido SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="90032-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="90032-104">Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="90032-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="90032-105">Instalar</span><span class="sxs-lookup"><span data-stu-id="90032-105">Install</span></span>

<span data-ttu-id="90032-106">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="90032-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="supported-database-engines"></a><span data-ttu-id="90032-107">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="90032-107">Supported Database Engines</span></span>

* <span data-ttu-id="90032-108">Microsoft SQL Server (de 2012 en adelante)</span><span class="sxs-lookup"><span data-stu-id="90032-108">Microsoft SQL Server (2012 onwards)</span></span>
