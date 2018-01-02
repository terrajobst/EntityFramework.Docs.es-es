---
title: Proveedor de base de datos SQLite - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a>Proveedor de base de datos SQLite para EF Core

Este proveedor de base de datos permite usar Entity Framework Core con SQLite. El proveedor se mantiene como parte del [proyecto EntityFramework de GitHub](https://github.com/aspnet/EntityFramework).

## <a name="install"></a>Install

Instale el [paquete NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a>Primeros pasos

Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.
* [Local SQLite on UWP (SQLite local en UWP)](../../get-started/uwp/getting-started.md)

* [.NET Core Application to New SQLite Database (Aplicación .NET Core para la nueva base de datos de SQLite)](../../get-started/netcore/new-db-sqlite.md)

* [Unicorn Clicker Sample Application (Aplicación de ejemplo Unicorn Clicker)](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [Unicorn Packer Sample Application (Aplicación de ejemplo Unicorn Packer)](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a>Motores de base de datos compatibles

* SQLite (3.7 y superiores)

## <a name="supported-platforms"></a>Plataformas compatibles

* .NET Framework (4.5.1 y superiores)

* Núcleo de .NET

* Mono (4.2.0 y superiores)

* Plataforma universal de Windows

## <a name="limitations"></a>Limitaciones

Vea [Limitaciones de SQLite](limitations.md) para conocer algunas limitaciones importantes del proveedor de SQLite.
