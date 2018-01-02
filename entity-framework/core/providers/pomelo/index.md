---
title: Proveedor de base de datos MySQL de Pomelo - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a><span data-ttu-id="46968-102">Proveedor de base de datos MySQL de Pomelo para EF Core</span><span class="sxs-lookup"><span data-stu-id="46968-102">Pomelo EF Core Database Provider for MySQL</span></span>

<span data-ttu-id="46968-103">Este proveedor de base de datos permite usar Entity Framework Core con MySQL.</span><span class="sxs-lookup"><span data-stu-id="46968-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="46968-104">El proveedor se mantiene como parte del [proyecto Pomelo Foundation](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="46968-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="46968-105">Este proveedor no se mantiene como parte del proyecto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="46968-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="46968-106">Al considerar un proveedor de terceros, evalúe la calidad, las licencias, el soporte técnico, etc. a fin de asegurarse de que satisface los requisitos.</span><span class="sxs-lookup"><span data-stu-id="46968-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="46968-107">Install</span><span class="sxs-lookup"><span data-stu-id="46968-107">Install</span></span>

<span data-ttu-id="46968-108">Instale el [paquete NuGet Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="46968-108">Install the [Pomelo.EntityFrameworkCore.MySql NuGet package](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span></span>

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a><span data-ttu-id="46968-109">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="46968-109">Get Started</span></span>

<span data-ttu-id="46968-110">Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.</span><span class="sxs-lookup"><span data-stu-id="46968-110">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="46968-111">Documentación de introducción</span><span class="sxs-lookup"><span data-stu-id="46968-111">Getting started documentation</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [<span data-ttu-id="46968-112">Aplicación de ejemplo Yuuko Blog</span><span class="sxs-lookup"><span data-stu-id="46968-112">Yuuko Blog sample application</span></span>](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a><span data-ttu-id="46968-113">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="46968-113">Supported Database Engines</span></span>

* <span data-ttu-id="46968-114">MySQL</span><span class="sxs-lookup"><span data-stu-id="46968-114">MySQL</span></span>
* <span data-ttu-id="46968-115">MariaDB</span><span class="sxs-lookup"><span data-stu-id="46968-115">MariaDB</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="46968-116">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="46968-116">Supported Platforms</span></span>

* <span data-ttu-id="46968-117">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="46968-117">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="46968-118">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="46968-118">.NET Core</span></span>

* <span data-ttu-id="46968-119">Mono (4.2.0 y superiores)</span><span class="sxs-lookup"><span data-stu-id="46968-119">Mono (4.2.0 onwards)</span></span>
