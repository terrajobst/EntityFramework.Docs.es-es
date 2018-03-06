---
title: Proveedor de base de datos de IBM Data Server (DB2) - EF Core
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2018
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a><span data-ttu-id="007cc-102">Proveedor de base de datos de IBM Data Server (DB2) para EF Core</span><span class="sxs-lookup"><span data-stu-id="007cc-102">IBM Data Server (DB2) EF Core Database Providers</span></span>

<span data-ttu-id="007cc-103">Este proveedor de base de datos permite usar Entity Framework Core con IBM Data Server.</span><span class="sxs-lookup"><span data-stu-id="007cc-103">This database provider allows Entity Framework Core to be used with IBM Data Server.</span></span> <span data-ttu-id="007cc-104">IBM mantiene a este proveedor.</span><span class="sxs-lookup"><span data-stu-id="007cc-104">The provider is maintained by IBM.</span></span>

> [!NOTE]  
> <span data-ttu-id="007cc-105">Este proveedor no se mantiene como parte del proyecto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="007cc-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="007cc-106">Al considerar un proveedor de terceros, evalúe la calidad, las licencias, el soporte técnico, etc. a fin de asegurarse de que satisface los requisitos.</span><span class="sxs-lookup"><span data-stu-id="007cc-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="007cc-107">Instalar</span><span class="sxs-lookup"><span data-stu-id="007cc-107">Install</span></span>

<span data-ttu-id="007cc-108">Para trabajar con IBM Data Server en Windows, instale el [paquete NuGet IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="007cc-108">To work with IBM Data Server on Windows, install the [IBM.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore
```

<span data-ttu-id="007cc-109">Para trabajar con IBM Data Server en Linux, instale el [paquete NuGet IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span><span class="sxs-lookup"><span data-stu-id="007cc-109">To work with IBM Data Server on Linux, install the [IBM.EntityFrameworkCore-lnx NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a><span data-ttu-id="007cc-110">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="007cc-110">Get Started</span></span>

[<span data-ttu-id="007cc-111">Introducción al proveedor .NET de IBM para .NET Core</span><span class="sxs-lookup"><span data-stu-id="007cc-111">Getting started with IBM .NET Provider for .NET Core</span></span>](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a><span data-ttu-id="007cc-112">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="007cc-112">Supported Database Engines</span></span>

* <span data-ttu-id="007cc-113">zOS</span><span class="sxs-lookup"><span data-stu-id="007cc-113">zOS</span></span>
* <span data-ttu-id="007cc-114">LUW, incluido IBM dashDB</span><span class="sxs-lookup"><span data-stu-id="007cc-114">LUW including IBM dashDB</span></span>
* <span data-ttu-id="007cc-115">IBM I</span><span class="sxs-lookup"><span data-stu-id="007cc-115">IBM I</span></span>
* <span data-ttu-id="007cc-116">Informix</span><span class="sxs-lookup"><span data-stu-id="007cc-116">Informix</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="007cc-117">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="007cc-117">Supported Platforms</span></span>

* <span data-ttu-id="007cc-118">.NET Framework (4.6 y superiores)</span><span class="sxs-lookup"><span data-stu-id="007cc-118">.NET Framework (4.6 onwards)</span></span>
* <span data-ttu-id="007cc-119">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="007cc-119">.NET Core</span></span>
