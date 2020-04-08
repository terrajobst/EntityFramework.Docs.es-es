---
title: Proveedor de base de datos de Microsoft SQL Server - EF Core
description: Documentación del proveedor de bases de datos que permite usar Entity Framework Core con Microsoft SQL Server.
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413150"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Proveedor de base de datos de Microsoft SQL Server para EF Core

Este proveedor de base de datos permite usar Entity Framework Core con Microsoft SQL Server (incluido Azure SQL Database). Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalar

Instale el [paquete NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

### <a name="net-core-cli"></a>[CLI de .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> A partir de la versión 3.0.0, el proveedor hace referencia a Microsoft.Data.SqlClient (las versiones anteriores dependían de System.Data.SqlClient). Si su proyecto depende directamente de SqlClient, asegúrese de que haga referencia al paquete Microsoft.Data.SqlClient.

## <a name="supported-database-engines"></a>Motores de base de datos compatibles

* Microsoft SQL Server (de 2012 en adelante)
