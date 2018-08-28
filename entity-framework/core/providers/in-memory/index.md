---
title: Proveedor de base de datos InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: ca802f55d973b9f79073c2507c1e0c7a853a1fce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993325"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="668a2-102">Proveedor de base de datos InMemory para EF Core</span><span class="sxs-lookup"><span data-stu-id="668a2-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="668a2-103">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="668a2-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="668a2-104">Puede resultar útil en el caso de pruebas, aunque es posible que el proveedor SQLite del modo en memoria sea un reemplazo de pruebas más adecuado para bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="668a2-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="668a2-105">Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="668a2-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="668a2-106">Instalar</span><span class="sxs-lookup"><span data-stu-id="668a2-106">Install</span></span>

<span data-ttu-id="668a2-107">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="668a2-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="668a2-108">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="668a2-108">Get Started</span></span>

<span data-ttu-id="668a2-109">Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.</span><span class="sxs-lookup"><span data-stu-id="668a2-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="668a2-110">Pruebas con InMemory</span><span class="sxs-lookup"><span data-stu-id="668a2-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="668a2-111">Pruebas de la aplicación de ejemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="668a2-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="668a2-112">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="668a2-112">Supported Database Engines</span></span>

* <span data-ttu-id="668a2-113">Base de datos en memoria integrada (diseñada para pruebas únicamente)</span><span class="sxs-lookup"><span data-stu-id="668a2-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="668a2-114">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="668a2-114">Supported Platforms</span></span>

* <span data-ttu-id="668a2-115">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="668a2-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="668a2-116">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="668a2-116">.NET Core</span></span>

* <span data-ttu-id="668a2-117">Mono (4.2.0 y superiores)</span><span class="sxs-lookup"><span data-stu-id="668a2-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="668a2-118">Plataforma universal de Windows</span><span class="sxs-lookup"><span data-stu-id="668a2-118">Universal Windows Platform</span></span>
