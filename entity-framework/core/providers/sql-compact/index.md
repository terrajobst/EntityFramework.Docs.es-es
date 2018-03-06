---
title: Proveedor de base de datos de Microsoft SQL Server Compact Edition - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a><span data-ttu-id="b7ea1-102">Proveedor de base de datos de Microsoft SQL Server Compact Edition para EF Core</span><span class="sxs-lookup"><span data-stu-id="b7ea1-102">Microsoft SQL Server Compact Edition EF Core Database Provider</span></span>

<span data-ttu-id="b7ea1-103">Este proveedor de base de datos permite usar Entity Framework Core con SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="b7ea1-103">This database provider allows Entity Framework Core to be used with SQL Server Compact Edition.</span></span> <span data-ttu-id="b7ea1-104">El proveedor se mantiene como parte del [proyecto ErikEJ/EntityFramework.SqlServerCom de GitHub](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="b7ea1-104">The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span>

> [!NOTE]  
> <span data-ttu-id="b7ea1-105">Este proveedor no se mantiene como parte del proyecto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b7ea1-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="b7ea1-106">Al considerar un proveedor de terceros, evalúe la calidad, las licencias, el soporte técnico, etc. a fin de asegurarse de que satisface los requisitos.</span><span class="sxs-lookup"><span data-stu-id="b7ea1-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="b7ea1-107">Instalar</span><span class="sxs-lookup"><span data-stu-id="b7ea1-107">Install</span></span>

<span data-ttu-id="b7ea1-108">Para trabajar con SQL Server Compact Edition 4.0, instale el [paquete NuGet EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span><span class="sxs-lookup"><span data-stu-id="b7ea1-108">To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

<span data-ttu-id="b7ea1-109">Para trabajar con SQL Server Compact Edition 3.5, instale [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span><span class="sxs-lookup"><span data-stu-id="b7ea1-109">To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a><span data-ttu-id="b7ea1-110">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="b7ea1-110">Get Started</span></span>

<span data-ttu-id="b7ea1-111">Vea la [documentación de introducción en el sitio del proyecto](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications).</span><span class="sxs-lookup"><span data-stu-id="b7ea1-111">See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="b7ea1-112">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="b7ea1-112">Supported Database Engines</span></span>

* <span data-ttu-id="b7ea1-113">SQL Server Compact Edition 3.5</span><span class="sxs-lookup"><span data-stu-id="b7ea1-113">SQL Server Compact Edition 3.5</span></span>

* <span data-ttu-id="b7ea1-114">SQL Server Compact Edition 4.0</span><span class="sxs-lookup"><span data-stu-id="b7ea1-114">SQL Server Compact Edition 4.0</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="b7ea1-115">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="b7ea1-115">Supported Platforms</span></span>

* <span data-ttu-id="b7ea1-116">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="b7ea1-116">.NET Framework (4.5.1 onwards)</span></span>
