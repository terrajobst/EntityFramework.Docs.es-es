---
title: Proveedor de base de datos Npgsql para PostgreSQL - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2018
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="e8c2d-102">Proveedor de base de datos Npgsql para PostgreSQL de EF Core</span><span class="sxs-lookup"><span data-stu-id="e8c2d-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="e8c2d-103">Este proveedor de base de datos permite usar Entity Framework Core con PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="e8c2d-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="e8c2d-104">El proveedor se mantiene como parte del [proyecto Npgsql](http://www.npgsql.org).</span><span class="sxs-lookup"><span data-stu-id="e8c2d-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="e8c2d-105">Este proveedor no se mantiene como parte del proyecto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e8c2d-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="e8c2d-106">Al considerar un proveedor de terceros, evalúe la calidad, las licencias, el soporte técnico, etc. a fin de asegurarse de que satisface los requisitos.</span><span class="sxs-lookup"><span data-stu-id="e8c2d-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="e8c2d-107">Instalar</span><span class="sxs-lookup"><span data-stu-id="e8c2d-107">Install</span></span>

<span data-ttu-id="e8c2d-108">Instale el [paquete NuGet Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="e8c2d-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="e8c2d-109">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="e8c2d-109">Get Started</span></span>

<span data-ttu-id="e8c2d-110">Vea la [documentación de Npgsql](http://www.npgsql.org/efcore/index.html) para empezar a trabajar.</span><span class="sxs-lookup"><span data-stu-id="e8c2d-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="e8c2d-111">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="e8c2d-111">Supported Database Engines</span></span>

* <span data-ttu-id="e8c2d-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="e8c2d-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="e8c2d-113">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="e8c2d-113">Supported Platforms</span></span>

* <span data-ttu-id="e8c2d-114">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="e8c2d-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="e8c2d-115">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="e8c2d-115">.NET Core</span></span>

* <span data-ttu-id="e8c2d-116">Mono (4.2.0 y superiores)</span><span class="sxs-lookup"><span data-stu-id="e8c2d-116">Mono (4.2.0 onwards)</span></span>
