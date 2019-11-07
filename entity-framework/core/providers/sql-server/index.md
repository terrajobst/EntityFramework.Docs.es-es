---
title: Proveedor de base de datos de Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: dd352b81da05fa8ea8970495f20947bd109edf65
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655893"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Proveedor de base de datos de Microsoft SQL Server para EF Core

Este proveedor de base de datos permite usar Entity Framework Core con Microsoft SQL Server (incluido SQL Azure). Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalar

Instale el [paquete NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

## <a name="net-core-clitabdotnet-core-cli"></a>[CLI de .NET Core](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> A partir de la versión 3.0.0, el proveedor hace referencia a Microsoft.Data.SqlClient (las versiones anteriores dependían de System.Data.SqlClient). Si su proyecto depende directamente de SqlClient, asegúrese de que haga referencia al paquete correcto.

## <a name="supported-database-engines"></a>Motores de base de datos compatibles

* Microsoft SQL Server (de 2012 en adelante)
