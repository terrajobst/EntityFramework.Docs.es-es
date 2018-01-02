---
title: Proveedor de base de datos InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="1bd41-102">Proveedor de base de datos InMemory para EF Core</span><span class="sxs-lookup"><span data-stu-id="1bd41-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="1bd41-103">Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="1bd41-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="1bd41-104">Es útil a la hora de probar código que usa Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1bd41-104">This is useful when testing code that uses Entity Framework Core.</span></span> <span data-ttu-id="1bd41-105">El proveedor se mantiene como parte del [proyecto EntityFramework de GitHub](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="1bd41-105">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="1bd41-106">Install</span><span class="sxs-lookup"><span data-stu-id="1bd41-106">Install</span></span>

<span data-ttu-id="1bd41-107">Instale el [paquete NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="1bd41-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="1bd41-108">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="1bd41-108">Get Started</span></span>

<span data-ttu-id="1bd41-109">Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.</span><span class="sxs-lookup"><span data-stu-id="1bd41-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="1bd41-110">Pruebas con InMemory</span><span class="sxs-lookup"><span data-stu-id="1bd41-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="1bd41-111">Pruebas de la aplicación de ejemplo UnicornStore</span><span class="sxs-lookup"><span data-stu-id="1bd41-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="1bd41-112">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="1bd41-112">Supported Database Engines</span></span>

* <span data-ttu-id="1bd41-113">Base de datos en memoria integrada (diseñada para pruebas únicamente)</span><span class="sxs-lookup"><span data-stu-id="1bd41-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="1bd41-114">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="1bd41-114">Supported Platforms</span></span>

* <span data-ttu-id="1bd41-115">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="1bd41-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="1bd41-116">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="1bd41-116">.NET Core</span></span>

* <span data-ttu-id="1bd41-117">Mono (4.2.0 y superiores)</span><span class="sxs-lookup"><span data-stu-id="1bd41-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="1bd41-118">Plataforma universal de Windows</span><span class="sxs-lookup"><span data-stu-id="1bd41-118">Universal Windows Platform</span></span>
