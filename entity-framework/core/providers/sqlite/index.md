---
title: Proveedor de base de datos SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f8b0044e11ba665b610ac583bf3a79ed7174630
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813481"
---
# <a name="sqlite-ef-core-database-provider"></a>Proveedor de base de datos SQLite para EF Core

Este proveedor de base de datos permite usar Entity Framework Core con SQLite. Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalar

Instale el [paquete NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

# <a name="net-core-clitabdotnet-core-cli"></a>[CLI de .NET Core](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Motores de base de datos compatibles

* SQLite (3.7 y superiores)

## <a name="limitations"></a>Limitaciones

Vea [Limitaciones de SQLite](limitations.md) para conocer algunas limitaciones importantes del proveedor de SQLite.
