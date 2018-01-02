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
# <a name="ef-core-in-memory-database-provider"></a>Proveedor de base de datos InMemory para EF Core

Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria. Es útil a la hora de probar código que usa Entity Framework Core. El proveedor se mantiene como parte del [proyecto EntityFramework de GitHub](https://github.com/aspnet/EntityFramework).

## <a name="install"></a>Install

Instale el [paquete NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>Primeros pasos

Los siguientes recursos le ayudarán a empezar a trabajar con este proveedor.
* [Pruebas con InMemory](../../miscellaneous/testing/in-memory.md)

* [Pruebas de la aplicación de ejemplo UnicornStore](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Motores de base de datos compatibles

* Base de datos en memoria integrada (diseñada para pruebas únicamente)

## <a name="supported-platforms"></a>Plataformas compatibles

* .NET Framework (4.5.1 y superiores)

* Núcleo de .NET

* Mono (4.2.0 y superiores)

* Plataforma universal de Windows
