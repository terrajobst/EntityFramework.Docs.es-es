---
title: Proveedor de base de datos de Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995674"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="5deac-102">Proveedor de base de datos de Microsoft SQL Server para EF Core</span><span class="sxs-lookup"><span data-stu-id="5deac-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="5deac-103">Este proveedor de base de datos permite usar Entity Framework Core con Microsoft SQL Server (incluido SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="5deac-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="5deac-104">Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="5deac-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="5deac-105">Instalar</span><span class="sxs-lookup"><span data-stu-id="5deac-105">Install</span></span>

<span data-ttu-id="5deac-106">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="5deac-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="5deac-107">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="5deac-107">Get Started</span></span>

<span data-ttu-id="5deac-108">Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.</span><span class="sxs-lookup"><span data-stu-id="5deac-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="5deac-109">Introducción a .NET Framework (consola, WinForms, WPF, etc.)</span><span class="sxs-lookup"><span data-stu-id="5deac-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="5deac-110">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5deac-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="5deac-111">Aplicación de ejemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="5deac-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="5deac-112">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="5deac-112">Supported Database Engines</span></span>

* <span data-ttu-id="5deac-113">Microsoft SQL Server (2008 y superiores)</span><span class="sxs-lookup"><span data-stu-id="5deac-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="5deac-114">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="5deac-114">Supported Platforms</span></span>

* <span data-ttu-id="5deac-115">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="5deac-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="5deac-116">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="5deac-116">.NET Core</span></span>

* <span data-ttu-id="5deac-117">Mono (4.2.0 y superiores)</span><span class="sxs-lookup"><span data-stu-id="5deac-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
