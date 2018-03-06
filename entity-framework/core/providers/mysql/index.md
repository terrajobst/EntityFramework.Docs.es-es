---
title: Proveedor de base de datos MySQL - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="7681f-102">Proveedor de base de datos MySQL para EF Core</span><span class="sxs-lookup"><span data-stu-id="7681f-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="7681f-103">Este proveedor de base de datos permite usar Entity Framework Core con MySQL.</span><span class="sxs-lookup"><span data-stu-id="7681f-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="7681f-104">El proveedor se mantiene como parte del [proyecto MySQL](http://dev.mysql.com).</span><span class="sxs-lookup"><span data-stu-id="7681f-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="7681f-105">Este proveedor es una versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="7681f-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="7681f-106">Este proveedor no se mantiene como parte del proyecto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7681f-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="7681f-107">Al considerar un proveedor de terceros, evalúe la calidad, las licencias, el soporte técnico, etc. a fin de asegurarse de que satisface los requisitos.</span><span class="sxs-lookup"><span data-stu-id="7681f-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="7681f-108">Instalar</span><span class="sxs-lookup"><span data-stu-id="7681f-108">Install</span></span>

<span data-ttu-id="7681f-109">Instale el [paquete NuGet MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="7681f-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="7681f-110">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="7681f-110">Get Started</span></span>

<span data-ttu-id="7681f-111">Vea [Starting with MySQL EF Core provider and Connector/Net 7.0.4 (Empezar a trabajar con el proveedor MySQL para EF Core y Connector/Net 7.0.4)](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span><span class="sxs-lookup"><span data-stu-id="7681f-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="7681f-112">Motores de base de datos compatibles</span><span class="sxs-lookup"><span data-stu-id="7681f-112">Supported Database Engines</span></span>

* <span data-ttu-id="7681f-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="7681f-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="7681f-114">Plataformas compatibles</span><span class="sxs-lookup"><span data-stu-id="7681f-114">Supported Platforms</span></span>

* <span data-ttu-id="7681f-115">.NET Framework (4.5.1 y superiores)</span><span class="sxs-lookup"><span data-stu-id="7681f-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="7681f-116">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="7681f-116">.NET Core</span></span>

<span data-ttu-id="7681f-117">No olvide consultar la documentación de MySQL para ver información de compatibilidad de versiones [aquí](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) y [aquí](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html).</span><span class="sxs-lookup"><span data-stu-id="7681f-117">Be sure to review the MySQL documentation for version compatibility information [here](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) and [here](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span></span>
