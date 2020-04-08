---
title: Proveedor de base de datos InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413010"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="ab6cf-102">Proveedor de base de datos InMemory para EF Core</span><span class="sxs-lookup"><span data-stu-id="ab6cf-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="ab6cf-103">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="ab6cf-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="ab6cf-104">Puede resultar útil en el caso de pruebas, aunque es posible que el proveedor SQLite del modo en memoria sea un reemplazo de pruebas más adecuado para bases de datos relacionales.</span><span class="sxs-lookup"><span data-stu-id="ab6cf-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="ab6cf-105">Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="ab6cf-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="ab6cf-106">Instalar</span><span class="sxs-lookup"><span data-stu-id="ab6cf-106">Install</span></span>

<span data-ttu-id="ab6cf-107">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="ab6cf-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="ab6cf-108">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="ab6cf-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[<span data-ttu-id="ab6cf-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab6cf-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="ab6cf-110">Introducción</span><span class="sxs-lookup"><span data-stu-id="ab6cf-110">Get Started</span></span>

<span data-ttu-id="ab6cf-111">Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.</span><span class="sxs-lookup"><span data-stu-id="ab6cf-111">The following resources will help you get started with this provider.</span></span>

* [<span data-ttu-id="ab6cf-112">Pruebas con InMemory</span><span class="sxs-lookup"><span data-stu-id="ab6cf-112">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)
* [<span data-ttu-id="ab6cf-113">Pruebas de la aplicación de ejemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="ab6cf-113">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="ab6cf-114">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="ab6cf-114">Supported Database Engines</span></span>

<span data-ttu-id="ab6cf-115">Base de datos de memoria en proceso (diseñada para pruebas únicamente)</span><span class="sxs-lookup"><span data-stu-id="ab6cf-115">In-process memory database (designed for testing purposes only)</span></span>
