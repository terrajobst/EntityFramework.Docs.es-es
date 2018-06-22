---
title: Proveedor de base de datos InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678991"
---
# <a name="ef-core-in-memory-database-provider"></a>Proveedor de base de datos InMemory para EF Core

Este proveedor de base de datos permite usar Entity Framework Core con una base de datos en memoria. Puede resultar útil en el caso de pruebas, aunque es posible que el proveedor SQLite del modo en memoria sea un reemplazo de pruebas más adecuado para bases de datos relacionales. Este proveedor se mantiene como parte del [proyecto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Instalar

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
