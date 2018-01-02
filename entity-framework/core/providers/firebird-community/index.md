---
title: Proveedor de base de datos FirebirdSQL - EF Core
author: ralmsdeveloper
ms.author: ralmsdeveloper
ms.date: 11/22/2017
ms.assetid: d0168c04-d30d-4219-98f8-a54690cea3c6
ms.technology: entity-framework-core
uid: core/providers/firebird-community/index
ms.openlocfilehash: 682988a91ef04dbd552588a537f53124b931f17d
ms.sourcegitcommit: 1cbd3d3cd92bdaf8223b8821c58200bcfed10ede
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2017
---
# <a name="firebird-ef-core-database-provider"></a><span data-ttu-id="381fa-102">Proveedor de base de datos Firebird para EF Core</span><span class="sxs-lookup"><span data-stu-id="381fa-102">Firebird EF Core Database Provider</span></span>

<span data-ttu-id="381fa-103">Este proveedor de base de datos permite usar Entity Framework Core con FirebirdSQL.</span><span class="sxs-lookup"><span data-stu-id="381fa-103">This database provider allows Entity Framework Core to be used with FirebirdSQL.</span></span> <span data-ttu-id="381fa-104">El proveedor se mantiene como parte del [proyecto ralmsdeveloper/EntityFrameworkCore.FirebirdSQL de GitHub](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="381fa-104">The provider is maintained as part of the [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub Project](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="381fa-105">Este proveedor no se mantiene como parte del proyecto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="381fa-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="381fa-106">Al considerar un proveedor de terceros, evalúe la calidad, las licencias, el soporte técnico, etc. a fin de asegurarse de que satisface los requisitos.</span><span class="sxs-lookup"><span data-stu-id="381fa-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="381fa-107">Install</span><span class="sxs-lookup"><span data-stu-id="381fa-107">Install</span></span>

<span data-ttu-id="381fa-108">Instale el [paquete NuGet EntityFrameworkCore.FirebirdSQL](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="381fa-108">Install the [EntityFrameworkCore.FirebirdSQL NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span></span>

``` powershell
Install-Package EntityFrameworkCore.FirebirdSQL
```

## <a name="get-started"></a><span data-ttu-id="381fa-109">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="381fa-109">Get Started</span></span>

<span data-ttu-id="381fa-110">Vea la [documentación de introducción en el sitio del proyecto](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki).</span><span class="sxs-lookup"><span data-stu-id="381fa-110">See the [getting started documentation on the project site](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="381fa-111">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="381fa-111">Supported Database Engines</span></span>

* <span data-ttu-id="381fa-112">FirebirdSql 2.5</span><span class="sxs-lookup"><span data-stu-id="381fa-112">FirebirdSql 2.5</span></span>
* <span data-ttu-id="381fa-113">FirebirdSql 3.X</span><span class="sxs-lookup"><span data-stu-id="381fa-113">FirebirdSql 3.X</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="381fa-114">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="381fa-114">Supported Platforms</span></span>

* <span data-ttu-id="381fa-115">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="381fa-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="381fa-116">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="381fa-116">.NET Core</span></span>

* <span data-ttu-id="381fa-117">Mono (4.2.0 y superiores)</span><span class="sxs-lookup"><span data-stu-id="381fa-117">Mono (4.2.0 onwards)</span></span>
