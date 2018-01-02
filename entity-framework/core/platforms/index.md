---
title: Implementaciones de .NET compatibles - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 6b6ea9ca833810169d0d850f3bea8776b761c007
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2017
---
# <a name="net-implementations-supported-by-ef-core"></a>Implementaciones de .NET compatibles con EF Core

El objetivo es que EF Core esté disponible en cualquier ubicación en la que se pueda escribir código .NET, así que se sigue trabajando para lograrlo. La tabla siguiente proporciona orientación sobre cada implementación de .NET en la que se quiere habilitar EF Core.

EF Core 2.0 tiene como destino [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard), por lo que requiere implementaciones de .NET que lo admitan.

| Implementación de .NET | Estado | 1.x requiere | 2.x requiere
|-|-|-|-
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [Consola](../get-started/netcore/index.md), etc.) | **Totalmente compatible y recomendado:** cubierto por las pruebas automatizadas; se sabe que muchas aplicaciones lo usan correctamente. | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/) | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)
| **.NET Framework** (WinForms, WPF, ASP.NET, [Consola](../get-started/full-dotnet/index.md), etc.) | **Totalmente compatible y recomendado:** cubierto por las pruebas automatizadas; se sabe que muchas aplicaciones lo usan correctamente. EF 6 también está disponible en esta plataforma (vea [Comparar EF Core y EF6](../../efcore-and-ef6/index.md) para elegir la tecnología adecuada). | .NET Framework 4.5.1 | .NET Framework 4.6.1
| **Mono y Xamarin** | **En progreso: se pueden producir problemas:** el equipo de EF Core y los clientes han realizado algunas pruebas. Los usuarios pioneros han notificado aspectos correctos, pero [se han detectado problemas](https://github.com/aspnet/entityframework/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) y se detectarán otros a medida que continúen las pruebas. En concreto, existen limitaciones en Xamarin.iOS que pueden evitar que algunas aplicaciones desarrolladas con EF Core 2.0 funcionen correctamente. | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7 | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5
| [**Plataforma universal de Windows**](../get-started/uwp/index.md) | **En progreso: se pueden producir problemas:** el equipo de EF Core y los clientes han realizado algunas pruebas. Se han notificado [problemas](https://github.com/aspnet/entityframework/issues?utf8=%E2%9C%93&q=is%3Aopen%20is%3Aissue%20label%3Aarea-uwp%20) al compilar con la cadena de herramientas de .NET Native, que normalmente se usa durante una compilación de versión y es un requisito para implementar en Microsoft Store (si no usa .NET Native o simplemente quiere experimentar, muchos de los problemas no le afectan). | [Paquete más reciente de .NET UWP 5](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/5.4.1) | [Paquete más reciente de .NET UWP 6](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) <sup>(1)</sup>

<sup>(1)</sup> Esta versión de .NET UWP agrega compatibilidad con .NET Standard 2.0 y contiene .NET Native 2.0, que soluciona la mayor parte de los problemas de compatibilidad comunicados anteriormente, aunque las pruebas han revelado [algunos problemas restantes](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0.1+label%3Aarea-uwp) con EF Core 2.0 que se planea solucionar en una próxima versión de revisión.

En el caso de cualquier combinación que no funcione según lo esperado, se recomienda crear nuevos problemas en el [seguimiento de problemas de EF Core](https://github.com/aspnet/entityframeworkcore/issues/new) y, en el caso de problemas relacionados con Xamarin, el [seguimiento de problemas de Xamarin](https://bugzilla.xamarin.com/newbug).
