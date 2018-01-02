---
title: Proveedor de base de datos SQLite - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="ab7da-102">Proveedor de base de datos SQLite para EF Core</span><span class="sxs-lookup"><span data-stu-id="ab7da-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="ab7da-103">Este proveedor de base de datos permite usar Entity Framework Core con SQLite.</span><span class="sxs-lookup"><span data-stu-id="ab7da-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="ab7da-104">El proveedor se mantiene como parte del [proyecto EntityFramework de GitHub](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="ab7da-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="ab7da-105">Install</span><span class="sxs-lookup"><span data-stu-id="ab7da-105">Install</span></span>

<span data-ttu-id="ab7da-106">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="ab7da-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="ab7da-107">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="ab7da-107">Get Started</span></span>

<span data-ttu-id="ab7da-108">Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.</span><span class="sxs-lookup"><span data-stu-id="ab7da-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="ab7da-109">Local SQLite on UWP (SQLite local en UWP)</span><span class="sxs-lookup"><span data-stu-id="ab7da-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="ab7da-110">.NET Core Application to New SQLite Database (Aplicación .NET Core para la nueva base de datos de SQLite)</span><span class="sxs-lookup"><span data-stu-id="ab7da-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="ab7da-111">Unicorn Clicker Sample Application (Aplicación de ejemplo Unicorn Clicker)</span><span class="sxs-lookup"><span data-stu-id="ab7da-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="ab7da-112">Unicorn Packer Sample Application (Aplicación de ejemplo Unicorn Packer)</span><span class="sxs-lookup"><span data-stu-id="ab7da-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="ab7da-113">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="ab7da-113">Supported Database Engines</span></span>

* <span data-ttu-id="ab7da-114">SQLite (3.7 y superiores)</span><span class="sxs-lookup"><span data-stu-id="ab7da-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="ab7da-115">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="ab7da-115">Supported Platforms</span></span>

* <span data-ttu-id="ab7da-116">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="ab7da-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="ab7da-117">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="ab7da-117">.NET Core</span></span>

* <span data-ttu-id="ab7da-118">Mono (4.2.0 y superiores)</span><span class="sxs-lookup"><span data-stu-id="ab7da-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="ab7da-119">Plataforma universal de Windows</span><span class="sxs-lookup"><span data-stu-id="ab7da-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="ab7da-120">Limitaciones</span><span class="sxs-lookup"><span data-stu-id="ab7da-120">Limitations</span></span>

<span data-ttu-id="ab7da-121">Vea [Limitaciones de SQLite](limitations.md) para conocer algunas limitaciones importantes del proveedor de SQLite.</span><span class="sxs-lookup"><span data-stu-id="ab7da-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
