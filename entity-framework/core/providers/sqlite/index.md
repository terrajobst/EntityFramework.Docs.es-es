---
title: Proveedor de base de datos SQLite - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 2e392f382f0e6f4d092a362c44f2149eb336db17
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678753"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="5c217-102">Proveedor de base de datos SQLite para EF Core</span><span class="sxs-lookup"><span data-stu-id="5c217-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="5c217-103">Este proveedor de base de datos permite usar Entity Framework Core con SQLite.</span><span class="sxs-lookup"><span data-stu-id="5c217-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="5c217-104">Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="5c217-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="5c217-105">Instalar</span><span class="sxs-lookup"><span data-stu-id="5c217-105">Install</span></span>

<span data-ttu-id="5c217-106">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="5c217-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="5c217-107">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="5c217-107">Get Started</span></span>

<span data-ttu-id="5c217-108">Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.</span><span class="sxs-lookup"><span data-stu-id="5c217-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="5c217-109">Local SQLite on UWP (SQLite local en UWP)</span><span class="sxs-lookup"><span data-stu-id="5c217-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="5c217-110">.NET Core Application to New SQLite Database (Aplicación .NET Core para la nueva base de datos de SQLite)</span><span class="sxs-lookup"><span data-stu-id="5c217-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="5c217-111">Unicorn Clicker Sample Application (Aplicación de ejemplo Unicorn Clicker)</span><span class="sxs-lookup"><span data-stu-id="5c217-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="5c217-112">Unicorn Packer Sample Application (Aplicación de ejemplo Unicorn Packer)</span><span class="sxs-lookup"><span data-stu-id="5c217-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="5c217-113">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="5c217-113">Supported Database Engines</span></span>

* <span data-ttu-id="5c217-114">SQLite (3.7 y superiores)</span><span class="sxs-lookup"><span data-stu-id="5c217-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="5c217-115">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="5c217-115">Supported Platforms</span></span>

* <span data-ttu-id="5c217-116">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="5c217-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="5c217-117">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="5c217-117">.NET Core</span></span>

* <span data-ttu-id="5c217-118">Mono (4.2.0 y superiores)</span><span class="sxs-lookup"><span data-stu-id="5c217-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="5c217-119">Plataforma universal de Windows</span><span class="sxs-lookup"><span data-stu-id="5c217-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="5c217-120">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="5c217-120">Limitations</span></span>

<span data-ttu-id="5c217-121">Vea [Limitaciones de SQLite](limitations.md) para conocer algunas limitaciones importantes del proveedor de SQLite.</span><span class="sxs-lookup"><span data-stu-id="5c217-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
