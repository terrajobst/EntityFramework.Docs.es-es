---
title: Registro - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: efc78fbada3c59bf9cf2c4cb694835bb5ad60e76
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997010"
---
# <a name="logging"></a><span data-ttu-id="bea02-102">Registro</span><span class="sxs-lookup"><span data-stu-id="bea02-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="bea02-103">Puede ver un [ejemplo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) de este artículo en GitHub.</span><span class="sxs-lookup"><span data-stu-id="bea02-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="bea02-104">Aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bea02-104">ASP.NET Core applications</span></span>

<span data-ttu-id="bea02-105">EF Core se integra automáticamente con los mecanismos de registro de ASP.NET Core siempre que `AddDbContext` o `AddDbContextPool` se utiliza.</span><span class="sxs-lookup"><span data-stu-id="bea02-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="bea02-106">Por lo tanto, cuando se usa ASP.NET Core, el registro debe configurarse como se describe en el [documentación de ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="bea02-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="bea02-107">Otras aplicaciones</span><span class="sxs-lookup"><span data-stu-id="bea02-107">Other applications</span></span>

<span data-ttu-id="bea02-108">EF Core actualmente registro requiere un elemento ILoggerFactory que se configura con uno o más ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="bea02-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="bea02-109">Proveedores comunes se incluyen en los siguientes paquetes:</span><span class="sxs-lookup"><span data-stu-id="bea02-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="bea02-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): un registrador de consola sencilla.</span><span class="sxs-lookup"><span data-stu-id="bea02-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="bea02-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): es compatible con Azure App Services 'Registros de diagnóstico' y 'Iniciar secuencia' características.</span><span class="sxs-lookup"><span data-stu-id="bea02-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="bea02-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): los registros de un monitor de depuración mediante System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="bea02-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="bea02-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): registros de registro de eventos de Windows.</span><span class="sxs-lookup"><span data-stu-id="bea02-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="bea02-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): admite EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="bea02-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="bea02-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): los registros de un agente de escucha de seguimiento mediante System.Diagnostics.TraceSource.TraceEvent().</span><span class="sxs-lookup"><span data-stu-id="bea02-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="bea02-116">Después de instalar los paquetes adecuados, la aplicación debe crear una instancia de un LoggerFactory singleton o global.</span><span class="sxs-lookup"><span data-stu-id="bea02-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="bea02-117">Por ejemplo, mediante el registrador de consola:</span><span class="sxs-lookup"><span data-stu-id="bea02-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="bea02-118">Esta instancia de singleton o global, a continuación, se debe registrar con EF Core en el `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bea02-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="bea02-119">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bea02-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="bea02-120">Es muy importante que las aplicaciones no crean una nueva instancia de ILoggerFactory para cada instancia del contexto.</span><span class="sxs-lookup"><span data-stu-id="bea02-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="bea02-121">Si lo hace, dará como resultado una pérdida de memoria y un rendimiento deficiente.</span><span class="sxs-lookup"><span data-stu-id="bea02-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="bea02-122">Lo que se registra el filtrado</span><span class="sxs-lookup"><span data-stu-id="bea02-122">Filtering what is logged</span></span>

<span data-ttu-id="bea02-123">Filtrar lo que se registra la manera más fácil es configurarlo al registrar el ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="bea02-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="bea02-124">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bea02-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="bea02-125">En este ejemplo, el registro se puede filtrar para devolver sólo los mensajes:</span><span class="sxs-lookup"><span data-stu-id="bea02-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="bea02-126">en la categoría 'Microsoft.EntityFrameworkCore.Database.Command'</span><span class="sxs-lookup"><span data-stu-id="bea02-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="bea02-127">en el nivel "Información"</span><span class="sxs-lookup"><span data-stu-id="bea02-127">at the 'Information' level</span></span>

<span data-ttu-id="bea02-128">Para EF Core, se definen las categorías de registrador en el `DbLoggerCategory` clase para que sea más fácil encontrar las categorías, pero estos resolver como cadenas simples.</span><span class="sxs-lookup"><span data-stu-id="bea02-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="bea02-129">Encontrará más detalles sobre la infraestructura subyacente de registro en el [documentación de registro de ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="bea02-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
