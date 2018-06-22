---
title: Proveedor de base de datos de Microsoft SQL Server - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: 2ed7c0dd127db03d5e7340fde1ef83cf01b30135
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678655"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Proveedor de base de datos de Microsoft SQL Server para EF Core

Este proveedor de base de datos permite usar Entity Framework Core con Microsoft SQL Server (incluido SQL Azure). Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalar

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
