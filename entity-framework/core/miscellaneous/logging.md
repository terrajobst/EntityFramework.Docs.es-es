---
title: Registro - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 807560e563eddfb72d4286353b1403a0d2e2a441
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2017
---
# <a name="logging"></a>Registro

> [!TIP]  
> Puede ver este artículo [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) en GitHub.

## <a name="aspnet-core-applications"></a>Aplicaciones de ASP.NET Core

Núcleo EF se integra automáticamente con los mecanismos de registro de ASP.NET Core cada vez que `AddDbContext` o `AddDbContextPool` se utiliza. Por lo tanto, cuando se usa ASP.NET Core, el registro debe configurarse como se describe en el [documentación principal de ASP.NET](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Otras aplicaciones

Núcleo EF registro actualmente requiere un ILoggerFactory que a su vez se configura con uno o más ILoggerProvider. Proveedores comunes se incluyen en los siguientes paquetes:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): un registrador de consola sencilla.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): servicios de aplicaciones de Azure es compatible con 'Registros de diagnóstico' y 'Flujo de registro' características.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): registros de un monitor de depuración mediante System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): registros en el registro de eventos de Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): admite EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): registros de un agente de escucha de seguimiento mediante System.Diagnostics.TraceSource.TraceEvent().

Después de instalar los paquetes adecuados, la aplicación debe crear una instancia singleton/global de un LoggerFactory. Por ejemplo, mediante el registrador de consola:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Esta instancia de singleton o de forma global, a continuación, debe registrarse con EF principales en el `DbContextOptionsBuilder`. Por ejemplo:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Es muy importante que las aplicaciones no crear una nueva instancia de ILoggerFactory para cada instancia de contexto. Si lo hace, se producirá una pérdida de memoria y un bajo rendimiento.

## <a name="filtering-what-is-logged"></a>Lo que se registra el filtrado

Es la manera más fácil de filtrar lo que se registra configurarlo al registrar el ILoggerProvider. Por ejemplo:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

En este ejemplo, el registro se filtra para devolver sólo los mensajes:
 * en la categoría 'Microsoft.EntityFrameworkCore.Database.Command'
 * en el nivel de «Datos»

En el núcleo de EF, categorías de registrador se definen en la `DbLoggerCategory` clase para que sea fácil de encontrar categorías, pero estos resolver en cadenas simples.

Obtener más detalles sobre la infraestructura subyacente de registro pueden encontrarse en el [documentación de registro de ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).