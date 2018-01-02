---
title: Proveedor de base de datos de Microsoft SQL Server - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Proveedor de base de datos de Microsoft SQL Server para EF Core

Este proveedor de base de datos permite usar Entity Framework Core con Microsoft SQL Server (incluido SQL Azure). El proveedor se mantiene como parte del [proyecto EntityFramework de GitHub](https://github.com/aspnet/EntityFramework).

## <a name="install"></a>Install

Instale el [paquete NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>Primeros pasos

Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.
* [Introducción a .NET Framework (consola, WinForms, WPF, etc.)](../../get-started/full-dotnet/index.md)

* [Introducción a ASP.NET Core](../../get-started/aspnetcore/index.md)

* [Aplicación de ejemplo UnicornStore](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a>Motores de base de datos compatibles

* Microsoft SQL Server (2008 y superiores)

## <a name="supported-platforms"></a>Plataformas compatibles

* .NET Framework (4.5.1 y superiores)

* Núcleo de .NET

* Mono (4.2.0 y superiores)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
