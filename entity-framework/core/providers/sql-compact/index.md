---
title: Proveedor de base de datos de Microsoft SQL Server Compact Edition - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a>Proveedor de base de datos de Microsoft SQL Server Compact Edition para EF Core

Este proveedor de base de datos permite usar Entity Framework Core con SQL Server Compact Edition. El proveedor se mantiene como parte del [proyecto ErikEJ/EntityFramework.SqlServerCom de GitHub](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).

> [!NOTE]  
> Este proveedor no se mantiene como parte del proyecto Entity Framework Core. Al considerar un proveedor de terceros, evalúe la calidad, las licencias, el soporte técnico, etc. a fin de asegurarse de que satisface los requisitos.

## <a name="install"></a>Install

Para trabajar con SQL Server Compact Edition 4.0, instale el [paquete NuGet EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

Para trabajar con SQL Server Compact Edition 3.5, instale [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a>Primeros pasos

Vea la [documentación de introducción en el sitio del proyecto](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications).

## <a name="supported-database-engines"></a>Motores de base de datos compatibles

* SQL Server Compact Edition 3.5

* SQL Server Compact Edition 4.0

## <a name="supported-platforms"></a>Plataformas compatibles

* .NET Framework (4.5.1 y superiores)
