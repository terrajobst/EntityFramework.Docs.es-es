---
title: Proveedor de base de datos de Microsoft SQL Server - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="94471-102">Proveedor de base de datos de Microsoft SQL Server para EF Core</span><span class="sxs-lookup"><span data-stu-id="94471-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="94471-103">Este proveedor de base de datos permite usar Entity Framework Core con Microsoft SQL Server (incluido SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="94471-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="94471-104">El proveedor se mantiene como parte del [proyecto EntityFramework de GitHub](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="94471-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="94471-105">Install</span><span class="sxs-lookup"><span data-stu-id="94471-105">Install</span></span>

<span data-ttu-id="94471-106">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="94471-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="94471-107">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="94471-107">Get Started</span></span>

<span data-ttu-id="94471-108">Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.</span><span class="sxs-lookup"><span data-stu-id="94471-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="94471-109">Introducción a .NET Framework (consola, WinForms, WPF, etc.)</span><span class="sxs-lookup"><span data-stu-id="94471-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="94471-110">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94471-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="94471-111">Aplicación de ejemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="94471-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="94471-112">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="94471-112">Supported Database Engines</span></span>

* <span data-ttu-id="94471-113">Microsoft SQL Server (2008 y superiores)</span><span class="sxs-lookup"><span data-stu-id="94471-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="94471-114">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="94471-114">Supported Platforms</span></span>

* <span data-ttu-id="94471-115">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="94471-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="94471-116">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="94471-116">.NET Core</span></span>

* <span data-ttu-id="94471-117">Mono (4.2.0 y superiores)</span><span class="sxs-lookup"><span data-stu-id="94471-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
