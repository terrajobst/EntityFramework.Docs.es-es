---
title: Proveedor de base de datos de Microsoft SQL Server - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: 2ed7c0dd127db03d5e7340fde1ef83cf01b30135
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678655"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="63348-102">Proveedor de base de datos de Microsoft SQL Server para EF Core</span><span class="sxs-lookup"><span data-stu-id="63348-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="63348-103">Este proveedor de base de datos permite usar Entity Framework Core con Microsoft SQL Server (incluido SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="63348-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="63348-104">Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="63348-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="63348-105">Instalar</span><span class="sxs-lookup"><span data-stu-id="63348-105">Install</span></span>

<span data-ttu-id="63348-106">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="63348-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="63348-107">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="63348-107">Get Started</span></span>

<span data-ttu-id="63348-108">Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.</span><span class="sxs-lookup"><span data-stu-id="63348-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="63348-109">Introducción a .NET Framework (consola, WinForms, WPF, etc.)</span><span class="sxs-lookup"><span data-stu-id="63348-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="63348-110">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="63348-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="63348-111">Aplicación de ejemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="63348-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="63348-112">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="63348-112">Supported Database Engines</span></span>

* <span data-ttu-id="63348-113">Microsoft SQL Server (2008 y superiores)</span><span class="sxs-lookup"><span data-stu-id="63348-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="63348-114">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="63348-114">Supported Platforms</span></span>

* <span data-ttu-id="63348-115">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="63348-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="63348-116">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="63348-116">.NET Core</span></span>

* <span data-ttu-id="63348-117">Mono (4.2.0 y superiores)</span><span class="sxs-lookup"><span data-stu-id="63348-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
