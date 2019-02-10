---
title: Registro - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 0a996403afdbe076b1690c98eeb305b40c4d1f4a
ms.sourcegitcommit: 109a16478de498b65717a6e09be243647e217fb3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2019
ms.locfileid: "55985579"
---
# <a name="logging"></a>Registro

> [!TIP]  
> Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) de este artículo en GitHub.

## <a name="aspnet-core-applications"></a>Aplicaciones ASP.NET Core

EF Core se integra automáticamente con los mecanismos de registro de ASP.NET Core siempre que `AddDbContext` o `AddDbContextPool` se utiliza. Por lo tanto, cuando se usa ASP.NET Core, el registro debe configurarse como se describe en el [documentación de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Otras aplicaciones

EF Core actualmente registro requiere un elemento ILoggerFactory que se configura con uno o más ILoggerProvider. Proveedores comunes se incluyen en los siguientes paquetes:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Un registrador de consola sencilla.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Es compatible con Azure App Services 'Registros de diagnóstico' y 'Iniciar secuencia' características.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Registros para un monitor de depuración mediante System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Registros para el registro de eventos de Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Admite EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Registros para un agente de escucha de seguimiento mediante System.Diagnostics.TraceSource.TraceEvent().

> [!NOTE]
> El siguiente código de ejemplo se usa un `ConsoleLoggerProvider` constructor que ha sido obsoleto en la versión 2.2. Reemplazos adecuados para la API de registro obsoleta estará disponibles en la versión 3.0. Mientras tanto, es seguro pasar por alto y suprimir las advertencias.

Después de instalar los paquetes adecuados, la aplicación debe crear una instancia de un LoggerFactory singleton o global. Por ejemplo, mediante el registrador de consola:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Esta instancia de singleton o global, a continuación, se debe registrar con EF Core en el `DbContextOptionsBuilder`. Por ejemplo:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Es muy importante que las aplicaciones no crean una nueva instancia de ILoggerFactory para cada instancia del contexto. Si lo hace, dará como resultado una pérdida de memoria y un rendimiento deficiente.

## <a name="filtering-what-is-logged"></a>Lo que se registra el filtrado

> [!NOTE]
> El siguiente código de ejemplo se usa un `ConsoleLoggerProvider` constructor que ha sido obsoleto en la versión 2.2. Reemplazos adecuados para la API de registro obsoleta estará disponibles en la versión 3.0. Mientras tanto, es seguro pasar por alto y suprimir las advertencias.

Filtrar lo que se registra la manera más fácil es configurarlo al registrar el ILoggerProvider. Por ejemplo:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

En este ejemplo, el registro se puede filtrar para devolver sólo los mensajes:
 * en la categoría 'Microsoft.EntityFrameworkCore.Database.Command'
 * en el nivel "Información"

Para EF Core, se definen las categorías de registrador en el `DbLoggerCategory` clase para que sea más fácil encontrar las categorías, pero estos resolver como cadenas simples.

Encontrará más detalles sobre la infraestructura subyacente de registro en el [documentación de registro de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
