---
title: Implementaciones de .NET compatibles - EF Core
author: rowanmiller
ms.date: 08/30/2017
uid: core/platforms/index
ms.openlocfilehash: 8fc25f4a35794162c92fd292990c24e977d1bf1b
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022267"
---
# <a name="net-implementations-supported-by-ef-core"></a>Implementaciones de .NET compatibles con EF Core

El objetivo es que EF Core esté disponible en cualquier ubicación en la que se pueda escribir código .NET, así que se sigue trabajando para lograrlo. Aunque la compatibilidad de EF Core en .NET Core y .NET Framework está cubierta por pruebas automatizadas y se sabe que muchas aplicaciones van a usarlo correctamente, Mono, Xamarin y UWP presentan algunos problemas.

## <a name="overview"></a>Información general

En la siguiente tabla se ofrecen instrucciones para cada implementación de .NET:

| Implementación de .NET                                                                                                  | Estado                                                             | Requisitos de EF Core 1.x                                                                                | Requisitos de EF Core 2.x <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [Consola](../get-started/netcore/index.md), etc.) | Totalmente compatible y recomendado                                    | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/)                                                | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **.NET Framework** (WinForms, WPF, ASP.NET, [Consola](../get-started/full-dotnet/index.md), etc.)                    | Totalmente compatible y recomendado. EF6 también disponible <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono y Xamarin**                                                                                                   | En curso <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Plataforma universal de Windows**](../get-started/uwp/index.md)                                                        | EF Core 2.0.1 recomendado <sup>(4)</sup>                           | [Paquete .NET Core UWP 5.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [Paquete .NET Core UWP 6.x](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1)</sup> EF Core 2.0 tiene como destino [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard), por lo que requiere implementaciones de .NET que lo admitan.

<sup>(2)</sup> Vea [Comparar EF Core y EF6](../../efcore-and-ef6/index.md) para elegir la tecnología adecuada.

<sup>(3)</sup> Existen problemas y limitaciones conocidas con Xamarin que pueden impedir que algunas aplicaciones desarrolladas con EF Core 2.0 funcionen correctamente. Compruebe la lista de [problemas activos](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) para ver soluciones alternativas.

<sup>(4) </sup> Vea la sección [Plataforma universal de Windows](#universal-windows-platform) de este artículo.

## <a name="universal-windows-platform"></a>Plataforma universal de Windows

Las versiones anteriores de EF Core y .NET UWP tuvieron numerosos problemas de compatibilidad, especialmente con aplicaciones compiladas con la cadena de herramientas de .NET Native. La nueva versión de .NET UWP agrega compatibilidad con .NET Standard 2.0 y contiene .NET Native 2.0, que soluciona la mayoría de los problemas de compatibilidad que se notificaban anteriormente. EF Core 2.0.1 se ha probado más exhaustivamente con UWP pero la prueba no está automatizada.

Al usar EF Core en UWP:

* Para optimizar el rendimiento de las consultas, evite tipos anónimos en las consultas LINQ. Para implementar una aplicación de UWP en la tienda de aplicaciones, la aplicación debe estar compilada con .NET Native. Las consultas con tipos anónimos tienen un menor rendimiento en .NET Native.

* Para optimizar el rendimiento de `SaveChanges()`, use [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) e implemente [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx) y [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx) en los tipos de entidad.

## <a name="report-issues"></a>Problemas de informes

En el caso de cualquier combinación que no funcione según lo esperado, se recomienda crear nuevos problemas en el [seguimiento de problemas de EF Core](https://github.com/aspnet/entityframeworkcore/issues/new). En el caso de problemas relacionados con Xamarin, use el seguimiento de problemas de [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) o de [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
