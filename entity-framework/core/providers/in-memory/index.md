---
title: Proveedor de base de datos InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 4b35e8c4b29a951449d4a26c6e274eb3015069bc
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656023"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="024a4-102">Proveedor de base de datos InMemory para EF Core</span><span class="sxs-lookup"><span data-stu-id="024a4-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="024a4-103">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="024a4-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="024a4-104">Puede resultar útil en el caso de pruebas, aunque es posible que el proveedor SQLite del modo en memoria sea un reemplazo de pruebas más adecuado para bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="024a4-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="024a4-105">Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="024a4-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="024a4-106">Instalar</span><span class="sxs-lookup"><span data-stu-id="024a4-106">Install</span></span>

<span data-ttu-id="024a4-107">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="024a4-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="024a4-108">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="024a4-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="024a4-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="024a4-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="024a4-110">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="024a4-110">Get Started</span></span>

<span data-ttu-id="024a4-111">Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.</span><span class="sxs-lookup"><span data-stu-id="024a4-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="024a4-112">Pruebas con InMemory</span><span class="sxs-lookup"><span data-stu-id="024a4-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="024a4-113">Pruebas de la aplicación de ejemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="024a4-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="024a4-114">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="024a4-114">Supported Database Engines</span></span>

<span data-ttu-id="024a4-115">Base de datos de memoria en proceso (diseñada para pruebas únicamente)</span><span class="sxs-lookup"><span data-stu-id="024a4-115">In-process memory database (designed for testing purposes only)</span></span>
